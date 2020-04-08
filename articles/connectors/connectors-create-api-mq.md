---
title: Herstellen einer Verbindung mit IBM MQ-Server
description: Senden und Empfangen von Nachrichten mit einem Azure-basierten oder lokalen IBM MQ-Server und Azure Logic Apps
services: logic-apps
ms.suite: integration
author: ChristopherHouser
ms.author: chrishou
ms.reviewer: valthom, logicappspm
ms.topic: article
ms.date: 03/31/2020
tags: connectors
ms.openlocfilehash: 737c5b90b216156ca08346f4a64fd0b421ad6c19
ms.sourcegitcommit: 27bbda320225c2c2a43ac370b604432679a6a7c0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/31/2020
ms.locfileid: "80410249"
---
# <a name="connect-to-an-ibm-mq-server-from-azure-logic-apps"></a>Herstellen einer Verbindung mit dem IBM MQ-Server über Azure Logic Apps

Der IBM MQ-Connector sendet und empfängt Nachrichten, die lokal oder in Azure auf einem IBM MQ-Server gespeichert sind. Dieser Connector umfasst einen Microsoft-Client zum Kommunizieren mit einem IBM MQ-Remoteserver über ein TCP/IP-Netzwerk. Der vorliegende Artikel ist ein Leitfaden für den Einstieg in die Verwendung des MQ-Connectors. Sie können zuerst eine einzelne Nachricht in einer Warteschlange suchen und anschließend andere Aktionen ausprobieren.

Der IBM MQ-Connector stellt die folgenden Aktionen, jedoch keine Trigger bereit:

- Durchsuchen einer einzelnen Nachricht ohne Löschen der Nachricht vom IBM MQ-Server
- Durchsuchen eines Batchs von Nachrichten ohne Löschen der Nachrichten vom IBM MQ-Server
- Empfangen einer einzelnen Nachricht und Löschen der Nachricht vom IBM MQ-Server
- Empfangen eines Batchs von Nachrichten und Löschen der Nachrichten vom IBM MQ-Server
- Senden einer einzelnen Nachricht an den IBM MQ-Server

Offiziell unterstützte IBM WebSphere MQ-Versionen:

  * MQ 7.5
  * MQ 8.0
  * MQ 9.0

## <a name="prerequisites"></a>Voraussetzungen

* Wenn Sie einen lokalen MQ-Server verwenden, [installieren Sie das lokale Datengateway](../logic-apps/logic-apps-gateway-install.md) auf einem Server in Ihrem Netzwerk. Der Server, auf dem das lokale Datengateway installiert ist, muss über .NET Framework 4.6 verfügen, damit der MQ-Connector funktioniert.

  Nach dem Installieren des Gateways müssen Sie in Azure außerdem eine Ressource für das lokale Datengateway erstellen. Weitere Informationen finden Sie unter [Einrichten der Datengatewayverbindung](../logic-apps/logic-apps-gateway-connection.md).

  Wenn Ihr MQ-Server öffentlich oder in Azure verfügbar ist, müssen Sie das Datengateway nicht verwenden.

* Die Logik-App, der Sie die MQ-Aktion hinzufügen möchten. Für diese Logik-App müssen derselbe Standort wie für die Verbindung mit dem lokalen Datengateway und ein bereits vorhandener Trigger verwendet werden, der den Workflow startet.

  Da der MQ-Connector keine Trigger bereitstellt, müssen Sie Ihrer Logik-App zuerst einen hinzufügen. Sie können beispielsweise den Wiederholungstrigger verwenden. Falls Sie noch nicht mit Logik-Apps gearbeitet haben, lesen Sie zuerst den [Schnellstart zum Erstellen Ihrer ersten Logik-App](../logic-apps/quickstart-create-first-logic-app-workflow.md).

<a name="create-connection"></a>

## <a name="create-mq-connection"></a>Erstellen der MQ-Verbindung

Wenn Sie beim Hinzufügen einer MQ-Aktion noch über keine MQ-Verbindung verfügen, werden Sie aufgefordert, die Verbindung zu erstellen, z. B.:

![Angeben von Verbindungsinformationen](media/connectors-create-api-mq/connection-properties.png)

1. Wenn Sie eine Verbindung mit einem lokalen MQ-Server herstellen, wählen Sie **Über lokales Datengateway verbinden** aus.

1. Geben Sie die Verbindungsinformationen für den MQ-Server an.

   * Für **Server** können Sie den Namen des MQ-Server oder die IP-Adresse gefolgt von einem Doppelpunkt und der Portnummer eingeben.

   * Um SSL (Secure Sockets Layer) zu verwenden, wählen Sie **SSL aktivieren?** aus.

     Der MQ-Connector unterstützt derzeit nur die Serverauthentifizierung, jedoch nicht die Clientauthentifizierung. Weitere Informationen finden Sie unter [Probleme bei der Verbindung und Authentifizierung](#connection-problems).

1. Gehen Sie im Bereich **Gateway** wie folgt vor:

   1. Wählen Sie in der Liste **Abonnement** das Azure-Abonnement aus, das Ihrer Azure-Gatewayressource zugeordnet ist.

   1. Wählen Sie in der Liste **Verbindungsgateway** die Azure-Gatewayressource aus, die Sie verwenden möchten.

1. Wählen Sie **Erstellen**, wenn Sie fertig sind.

<a name="connection-problems"></a>

### <a name="connection-and-authentication-problems"></a>Probleme bei der Verbindung und Authentifizierung

Wenn die Logik-App versucht, eine Verbindung mit dem lokalen MQ-Server herzustellen, wird möglicherweise die folgende Fehlermeldung angezeigt:

`"MQ: Could not Connect the Queue Manager '<queue-manager-name>': The Server was expecting an SSL connection."`

* Wenn Sie den MQ-Connector direkt in Azure verwenden, muss auf dem MQ-Server ein Zertifikat verwendet werden, das von einer vertrauenswürdigen [Zertifizierungsstelle](https://www.ssl.com/faqs/what-is-a-certificate-authority/) ausgestellt wurde.

* Wenn Sie das lokale Datengateway verwenden, versuchen Sie, nach Möglichkeit ein Zertifikat zu verwenden, das von einer vertrauenswürdigen [Zertifizierungsstelle](https://www.ssl.com/faqs/what-is-a-certificate-authority/) ausgestellt wurde. Wenn dies jedoch nicht möglich ist, können Sie ein selbstsigniertes Zertifikat verwenden, das nicht von einer vertrauenswürdigen [Zertifizierungsstelle](https://www.ssl.com/faqs/what-is-a-certificate-authority/) ausgestellt wurde und als weniger sicher eingestuft wird.

  Zum Installieren des selbstsignierten Zertifikats des Servers können Sie das Tool **Windows Certification Manager** („certmgr.msc“) verwenden. In diesem Szenario müssen Sie auf dem lokalen Computer, auf dem der lokale Datengatewaydienst ausgeführt wird, das Zertifikat im Zertifikatspeicher des **lokalen Computers** auf der Ebene **Vertrauenswürdige Stammzertifizierungsstellen** installieren.

  1. Öffnen Sie auf dem Computer, auf dem der lokale Datengatewaydienst ausgeführt wird, das Startmenü, suchen Sie **Benutzerzertifikate verwalten**, und wählen die Option aus.

  1. Nachdem das Tool Windows Certification Manager geöffnet wurde, wechseln Sie zum Ordner **Zertifikate – Lokaler Computer** >  **Vertrauenswürdige Stammzertifizierungsstellen**, und installieren Sie das Zertifikat.

     > [!IMPORTANT]
     > Stellen Sie sicher, dass Sie das Zertifikat im Speicher **Zertifikate – Lokaler Computer** > **Vertrauenswürdige Stammzertifizierungsstellen** installieren.

* Für den MQ-Server muss eine Verschlüsselungsspezifikation definiert werden, die für SSL-Verbindungen verwendet werden soll. In SsLStream in .NET ist es jedoch nicht möglich, dass Sie die Reihenfolge für Verschlüsselungsspezifikationen angeben. Um diese Einschränkung zu umgehen, können Sie die Konfiguration des MQ-Servers so ändern, dass die erste Verschlüsselungsspezifikation in der Auflistung verwendet wird, die der Connector in der SSL-Aushandlung sendet.

  Wenn Sie die Verbindung testen, protokolliert der MQ-Server eine Ereignismeldung, die angibt, dass die Verbindung nicht hergestellt werden konnte, da am anderen Ende die falsche Verschlüsselungsspezifikation verwendet wurde. Die Ereignismeldung enthält die Verschlüsselungsspezifikation, die oben in der Liste angezeigt wird. Aktualisieren Sie die Verschlüsselungsspezifikation in der Kanalkonfiguration so, dass sie der Verschlüsselungsspezifikation in der Ereignismeldung entspricht.

## <a name="browse-single-message"></a>Durchsuchen einer einzelnen Nachricht

1. Wählen Sie in Ihrer Logik-App unter dem Trigger oder einer anderen Aktion die Option **Neuer Schritt** aus.

1. Geben Sie `mq` im Suchfeld ein, und wählen Sie die Aktion **Nachricht suchen** aus.

   ![Auswählen der Aktion „Nachricht suchen“](media/connectors-create-api-mq/browse-message.png)

1. Wenn Sie noch keine MQ-Verbindung erstellt haben, werden Sie aufgefordert, [diese Verbindung zu erstellen](#create-connection).

1. Nachdem Sie die Verbindung erstellt haben, richten Sie die Eigenschaften der Aktion **Nachricht suchen** ein:

   | Eigenschaft | BESCHREIBUNG |
   |----------|-------------|
   | **Warteschlange** | Wenn in der Verbindung eine andere Warteschlange angegeben ist, geben Sie diese Warteschlange an. |
   | **MessageId**, **CorrelationId**, **GroupId** und andere Eigenschaften | Durchsuchen einer Nachricht basierend auf den unterschiedlichen MQ-Nachrichteneigenschaften |
   | **IncludeInfo** | Wählen Sie **true** aus, um zusätzliche Nachrichteninformationen in die Ausgabe einzufügen. Wählen Sie andernfalls **false** aus. |
   | **Timeout** | Geben Sie einen Wert ein, um festzulegen, wie lange in einer leeren Warteschlange auf den Empfang einer Nachricht gewartet wird. Wenn hier kein Wert festgelegt ist, wird die erste Nachricht in der Warteschlange abgerufen, und es wird nicht gewartet, bis eine Nachricht eingeht. |
   |||

   Beispiel:

   ![Eigenschaften für die Aktion „Nachricht suchen“](media/connectors-create-api-mq/browse-message-properties.png)

1. Wenn Sie fertig sind, wählen Sie auf der Symbolleiste des Designers die Option **Speichern** aus. Wählen Sie **Ausführen** aus, um die App zu testen.

   Nach der Ausführung werden im Designer die Workflowschritte und deren Status angezeigt, sodass Sie die Ausgabe überprüfen können.

1. Um die Details zu den einzelnen Schritten anzuzeigen, klicken Sie auf die Titelleiste des jeweiligen Schritts. Wenn Sie weitere Informationen zu der Ausgabe eines Schritts aufrufen möchten, wählen Sie **Unformatierte Ausgaben anzeigen** aus.

   ![Ausgabe für die Suche nach einer Nachricht](media/connectors-create-api-mq/browse-message-output.png)

   Im Folgenden sehen Sie ein Beispiel für eine unformatierte Ausgabe:

   ![Unformatierte Ausgabe für die Suche nach einer Nachricht](media/connectors-create-api-mq/browse-message-raw-output.png)

1. Wenn Sie **IncludeInfo** auf **true** festlegen, werden zusätzliche Ausgabeinformationen angezeigt:

   ![Informationen für die Suche nach einer Nachricht einschließen](media/connectors-create-api-mq/browse-message-include-info.png)

## <a name="browse-multiple-messages"></a>Suche nach mehreren Nachrichten

Die Aktion **Nachrichten durchsuchen** umfasst die Option **BatchSize**, mit der Sie angeben können, wie viele Nachrichten aus der Warteschlange zurückgegeben werden sollen. Wenn **BatchSize** keinen Wert enthält, werden alle Nachrichten zurückgegeben. Die zurückgegebene Ausgabe ist ein Array aus Nachrichten.

1. Führen Sie die oben angegebenen Schritte aus, wählen Sie jedoch die Aktion **Nachrichten durchsuchen** aus.

1. Wenn Sie noch keine MQ-Verbindung erstellt haben, werden Sie aufgefordert, [diese Verbindung zu erstellen](#create-connection). Andernfalls wird standardmäßig die erste zuvor konfigurierte Verbindung verwendet. Wählen Sie **Verbindung ändern** aus, um eine neue Verbindung zu erstellen. Alternativ können Sie auch eine andere Verbindung auswählen.

1. Geben Sie die Informationen für die Aktion an.

1. Speichern Sie die Logik-App, und führen Sie sie dann aus.

   Im Folgenden sehen Sie eine Beispielausgabe für die Aktion **Nachrichten durchsuchen**. Die Ausgabe wird nach der Ausführung der Logik-App angezeigt.

   ![Beispielausgabe für „Nachrichten durchsuchen“](media/connectors-create-api-mq/browse-messages-output.png)

## <a name="receive-single-message"></a>Empfangen einer einzelnen Nachricht

Die Aktion **Nachricht empfangen** umfasst dieselben Eingaben und Ausgaben wie die Aktion **Nachricht suchen**. Bei Verwendung von **Nachricht empfangen** wird die Nachricht aus der Warteschlange gelöscht.

## <a name="receive-multiple-messages"></a>Empfangen mehrerer Nachrichten

Die Aktion **Nachrichten empfangen** umfasst dieselben Eingaben und Ausgaben wie die Aktion **Nachrichten suchen**. Bei Verwendung von **Nachrichten empfangen** werden die Nachrichten aus der Warteschlange gelöscht.

> [!NOTE]
> Wenn eine Aktion zum Durchsuchen oder Empfangen von Nachrichten in einer Warteschlange ausgeführt wird, die keine Nachrichten enthält, tritt bei der Aktion ein Fehler mit der folgenden Ausgabe auf:
>
> ![MQ-Fehler „Keine Nachricht“](media/connectors-create-api-mq/mq-no-message-error.png)

## <a name="send-message"></a>Nachricht senden

1. Führen Sie die oben angegebenen Schritte aus, wählen Sie jedoch die Aktion **Nachricht senden** aus.

1. Wenn Sie noch keine MQ-Verbindung erstellt haben, werden Sie aufgefordert, [diese Verbindung zu erstellen](#create-connection). Andernfalls wird standardmäßig die erste zuvor konfigurierte Verbindung verwendet. Wählen Sie **Verbindung ändern** aus, um eine neue Verbindung zu erstellen. Alternativ können Sie auch eine andere Verbindung auswählen.

1. Geben Sie die Informationen für die Aktion an. Wählen Sie für **MessageType** einen gültigen Nachrichtentyp aus: **Datagramm**, **Antwort** oder **Anforderung**.

   ![Eigenschaften für die Aktion „Nachricht senden“](media/connectors-create-api-mq/send-message-properties.png)

1. Speichern Sie die Logik-App, und führen Sie sie dann aus.

   Im Folgenden sehen Sie eine Beispielausgabe für die Aktion **Nachrichten senden**. Die Ausgabe wird nach der Ausführung der Logik-App angezeigt:

   ![Beispielausgabe für „Nachricht senden“](media/connectors-create-api-mq/send-message-output.png)

## <a name="connector-reference"></a>Connector-Referenz

Technische Details zu Aktionen und Einschränkungen aus der Swagger-Beschreibung des Connectors finden Sie auf der [Referenzseite](/connectors/mq/) des Connectors.

## <a name="next-steps"></a>Nächste Schritte

* Informationen zu anderen [Logic Apps-Connectors](../connectors/apis-list.md)
