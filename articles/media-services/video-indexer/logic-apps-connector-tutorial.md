---
title: 'Tutorial: Video Indexer-Connectors mit Logic Apps und Power Automate.'
description: In diesem Tutorial wird veranschaulicht, wie Sie neue Umgebungen und Monetarisierungschancen nutzen können, indem Sie Video Indexer-Connectors mit Logic Apps und Power Automate verwenden.
author: anzaman
manager: johndeu
ms.author: alzam
ms.service: media-services
ms.subservice: video-indexer
ms.topic: tutorial
ms.date: 05/01/2020
ms.openlocfilehash: 5f29e616c0643914ca28921eee481105a5feb0c5
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2020
ms.locfileid: "87047091"
---
# <a name="tutorial-use-video-indexer-with-logic-app-and-power-automate"></a>Tutorial: Verwenden von Video Indexer mit Logic Apps und Power Automate

Die [REST-API für Video Indexer v2](https://api-portal.videoindexer.ai/docs/services/Operations/operations/Delete-Video?) von Azure Media Services unterstützt sowohl die Kommunikation von Server zu Server als auch von Client zu Server. Benutzer von Video Indexer können Erkenntnisse, die anhand von Video- und Audiodaten gewonnen werden, leicht in ihre Anwendungslogik integrieren und so neue Umgebungen und Monetarisierungschancen schaffen.

Zur weiteren Vereinfachung der Integration werden Connectors für [Logic Apps](https://azure.microsoft.com/services/logic-apps/) und [Power Automate](https://preview.flow.microsoft.com/connectors/shared_videoindexer-v2/video-indexer-v2/) unterstützt, die mit unserer API kompatibel sind. Mit den Connectors können Sie benutzerdefinierte Workflows einrichten, um auf effektive Weise für eine große Zahl von Video- und Audiodateien Erkenntnisse zu indizieren und zu extrahieren, ohne auch nur eine Codezeile zu schreiben. Wenn Sie die Connectors für Ihre Integration verwenden, haben Sie außerdem einen besseren Überblick über die Integrität Ihres Workflows und können ihn leicht debuggen.  

Damit Sie schnell mit der Verwendung der Video Indexer-Connectors beginnen können, ist hier eine exemplarische Vorgehensweise für eine Lösung mit Logic Apps und Power Automate beschrieben, die Sie einrichten können. 

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Automatisches Hochladen und Indizieren Ihres Videos
> * Einrichten des Workflows für den Dateiupload
> * Einrichten des Workflows für die JSON-Extraktion

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Voraussetzungen

Zunächst benötigen Sie auch ein Video Indexer-Konto sowie Zugriff auf die APIs über einen API-Schlüssel. 

Sie benötigen außerdem ein Azure Storage-Konto. Notieren Sie sich den Zugriffsschlüssel für Ihr Storage-Konto. Erstellen Sie zwei Container: einen zum Speichern von Videos und einen zum Speichern der von Video Indexer generierten Erkenntnisse.  

Als Nächstes müssen Sie – entweder für Logic Apps oder Power Automate – zwei separate Workflows öffnen (je nachdem, was Sie verwenden).  

## <a name="upload-and-index-your-video-automatically"></a>Automatisches Hochladen und Indizieren Ihres Videos 

Dieses Szenario umfasst zwei unterschiedliche Workflows, die zusammenarbeiten. Der erste Workflow wird ausgelöst, wenn ein Blob in einem Azure Storage-Konto hinzugefügt oder geändert wird. Hierbei wird die neue Datei in Video Indexer mit einer Rückruf-URL hochgeladen, um eine Benachrichtigung senden zu können, nachdem der Indizierungsvorgang abgeschlossen ist. Der zweite Workflow wird basierend auf der Rückruf-URL ausgelöst, und die extrahierten Erkenntnisse werden als JSON-Datei in Azure Storage gespeichert. Dieser Ansatz mit zwei Workflows wird verwendet, um das effektive asynchrone Hochladen und Indizieren von größeren Dateien zu unterstützen. 

### <a name="set-up-the-file-upload-flow"></a>Einrichten des Workflows für den Dateiupload 

Der erste Workflow wird jeweils ausgelöst, wenn in Ihrem Azure Storage-Container ein Blob hinzugefügt wird. Nach dem Auslösen wird ein SAS-URI erstellt, den Sie verwenden können, um das Video in Video Indexer hochzuladen und zu indizieren. Erstellen Sie zunächst den folgenden Workflow. 

![Workflow für Dateiupload](./media/logic-apps-connector-tutorial/file-upload-flow.png)

Zum Einrichten des ersten Workflows müssen Sie Ihren Video Indexer-API-Schlüssel und die Azure Storage-Anmeldeinformationen angeben. 

![Azure Blob Storage](./media/logic-apps-connector-tutorial/azure-blob-storage.png)

![Verbindungsname und API-Schlüssel](./media/logic-apps-connector-tutorial/connection-name-api-key.png)

Wenn Sie eine Verbindung mit Ihren Azure Storage- und Video Indexer-Konten herstellen können, können Sie zum Trigger „Wenn ein Blob hinzugefügt oder geändert wird“ navigieren und den Container auswählen, der Ihre Videodateien enthalten soll. 

![Container](./media/logic-apps-connector-tutorial/container.png)

Navigieren Sie als Nächstes zur Aktion „SAS-URI nach Pfad erstellen“, und wählen Sie in den Optionen für dynamischen Inhalt den Pfad zur Dateiliste aus.  

![SAS-URI nach Pfad](./media/logic-apps-connector-tutorial/sas-uri-by-path.jpg)

Geben Sie [Standort und ID für Ihr Konto](./video-indexer-use-apis.md#account-id) an, um das Token für das Video Indexer-Konto abzurufen.

![Abrufen des Tokens für den Kontozugriff](./media/logic-apps-connector-tutorial/account-access-token.png)

Geben Sie für „Upload video and index“ (Video hochladen und indizieren) die erforderlichen Parameter und die Video-URL an. Wählen Sie „Neuen Parameter hinzufügen“ und dann „Rückruf-URL“ aus. 

![Hochladen und Indizieren](./media/logic-apps-connector-tutorial/upload-and-index.png)

Lassen Sie die Rückruf-URL vorerst leer. Sie fügen sie erst nach Abschluss des zweiten Workflows hinzu, bei dem die Rückruf-URL erstellt wird. 

Sie können den Standardwert für die anderen Parameter verwenden oder den Wert gemäß Ihren Anforderungen festlegen. 

Klicken Sie auf „Speichern“. Wir fahren nun mit dem Konfigurieren des zweiten Workflows fort, bei dem die Erkenntnisse extrahiert werden, nachdem das Hochladen und Indizieren abgeschlossen ist. 

## <a name="set-up-the-json-extraction-flow"></a>Einrichten des Workflows für die JSON-Extraktion 

Nach Abschluss des Hochladens und Indizierens aus dem ersten Workflow wird eine HTTP-Anforderung mit der richtigen Rückruf-URL gesendet, um den zweiten Workflow auszulösen. Anschließend werden die von Video Indexer generierten Erkenntnisse abgerufen. In diesem Beispiel wird die Ausgabe Ihres Indizierungsauftrags auf Ihrer Azure Storage-Instanz gespeichert.  Es liegt aber an Ihnen, was Sie mit der Ausgabe machen.  

Erstellen Sie den zweiten Workflow separat vom ersten Workflow. 

![Workflow für JSON-Extraktion](./media/logic-apps-connector-tutorial/json-extraction-flow.png)

Zum Einrichten dieses Workflows müssen Sie erneut Ihren Video Indexer-API-Schlüssel und die Azure Storage-Anmeldeinformationen angeben. Sie müssen die gleichen Parameter wie beim ersten Workflow aktualisieren. 

Für Ihren Trigger wird ein Feld für die HTTP POST-URL angezeigt. Die URL wird erst generiert, nachdem Sie Ihren Workflow gespeichert haben. Sie benötigen die URL dann später aber noch. Wir kommen dann darauf zurück. 

Geben Sie [Standort und ID für Ihr Konto](./video-indexer-use-apis.md#account-id) an, um das Token für das Video Indexer-Konto abzurufen.  

Navigieren Sie zur Aktion „Get Video Index“ (Videoindex abrufen), und geben Sie die erforderlichen Parameter an. Geben Sie als Video-ID den folgenden Ausdruck ein: triggerOutputs()['queries']['id'] 

![Informationen zur Video Indexer-Aktion](./media/logic-apps-connector-tutorial/video-indexer-action-info.jpg)

Mit diesem Ausdruck wird der Connector angewiesen, die Video-ID aus der Ausgabe Ihres Triggers abzurufen. In diesem Fall ist die Ausgabe Ihres Triggers die Ausgabe von „Upload video and index“ (Video hochladen und indizieren) in Ihrem ersten Trigger. 

Navigieren Sie zur Aktion „Create blob“ (Blob erstellen), und wählen Sie den Pfad zum Ordner aus, in dem Sie die Erkenntnisse speichern möchten. Legen Sie den Namen des Blobs fest, das Sie erstellen. Geben Sie für den Blobinhalt den folgenden Ausdruck ein: body(‘Get_Video_Index’) 

![Aktion „Create blob“ (Blob erstellen)](./media/logic-apps-connector-tutorial/create-blob-action.jpg)

Bei diesem Ausdruck wird die Ausgabe der Aktion „Get Video Index“ (Videoindex abrufen) aus diesem Workflow verwendet. 

Klicken Sie auf „Flow speichern“. 

Nachdem der Workflow gespeichert wurde, wird im Trigger eine HTTP POST-URL erstellt. Kopieren Sie die URL aus dem Trigger. 

![Speichern des URL-Triggers](./media/logic-apps-connector-tutorial/save-url-trigger.png)

Navigieren Sie nun zurück zum ersten Workflow, und fügen Sie die URL aus der Aktion „Upload video and index“ (Video hochladen und indizieren) als Parameter für die Rückruf-URL ein. 

Stellen Sie sicher, dass beide Workflows gespeichert wurden und alles bereit ist! 

Probieren Sie Ihre neu erstellte Lösung mit Logic Apps bzw. Power Automate aus, indem Sie Ihrem Azure-Blobcontainer ein Video hinzufügen und nach einigen Minuten Wartezeit überprüfen, ob die Erkenntnisse im Zielordner angezeigt werden. 

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Nachdem Sie dieses Tutorial durchgearbeitet haben, können Sie die Lösung mit Logic Apps bzw. Power Automate bei Bedarf weiter ausführen. Falls Sie die weitere Ausführung nicht wünschen und das Anfallen von Gebühren vermeiden möchten, müssen Sie bei Verwendung von Power Automate Ihre beiden Workflows deaktivieren. Deaktivieren Sie beide Workflows, falls Sie Logic Apps verwenden. 

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial wurde nur ein Beispiel für Video Indexer-Connectors veranschaulicht. Sie können die Video Indexer-Connectors für alle API-Aufrufe verwenden, die von Video Indexer bereitgestellt werden. Beispiel: Hochladen und Abrufen von Erkenntnissen, Übersetzen der Ergebnisse, Nutzen von einbettungsfähigen Widgets und sogar Anpassen Ihrer Modelle. Darüber hinaus können Sie sich dafür entscheiden, dass diese Aktionen basierend auf unterschiedlichen Quellen ausgelöst werden, z. B. Updates von Dateirepositorys oder gesendete E-Mails. Sie können dann auswählen, dass die Ergebnisse gemäß unserer relevanten Infrastruktur oder Anwendung aktualisiert werden sollen oder eine beliebige Anzahl von Aktionselementen generiert werden soll.  

> [!div class="nextstepaction"]
> [Verwenden der Video Indexer-API](video-indexer-use-apis.md)
