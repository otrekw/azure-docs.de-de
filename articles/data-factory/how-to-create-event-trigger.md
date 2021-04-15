---
title: Erstellen von ereignisbasierten Triggern in Azure Data Factory
description: Erfahren Sie, wie in Azure Data Factory ein Trigger erstellt wird, der eine Pipeline als Reaktion auf ein Ereignis ausführt.
ms.service: data-factory
author: chez-charlie
ms.author: chez
ms.reviewer: jburchel
ms.topic: conceptual
ms.date: 03/11/2021
ms.openlocfilehash: 3021d049a38f1d883518fc7c45aa8ca0a906c2f7
ms.sourcegitcommit: 3f684a803cd0ccd6f0fb1b87744644a45ace750d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/02/2021
ms.locfileid: "106221584"
---
# <a name="create-a-trigger-that-runs-a-pipeline-in-response-to-a-storage-event"></a>Erstellen eines Triggers zum Ausführen einer Pipeline als Reaktion auf ein Speicherereignis

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

In diesem Artikel wird der Trigger für Speicherereignisse erläutert, den Sie in Ihren Data Factory-Pipelines erstellen können.

Die ereignisgesteuerte Architektur (EDA) ist ein allgemeines Datenintegrationsmuster, das die Produktion, Erkennung, Verbrauch und Reaktion auf Ereignisse beinhaltet. In Datenintegrationsszenarios ist es häufig notwendig, dass Data Factory-Kunden Pipelines auf der Grundlage von Ereignissen in Speicherkonten auslösen, z. B. wenn Dateien zu einem Azure Blob Storage-Konto hinzugefügt oder aus diesem gelöscht werden. Data Factory integriert nativ mit [Azure Event Grid](https://azure.microsoft.com/services/event-grid/), sodass Sie Pipelines für solche Ereignisse auslösen können.

Das folgende Video enthält eine zehnminütige Einführung und Demonstration dieses Features:

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Event-based-data-integration-with-Azure-Data-Factory/player]

> [!NOTE]
> Die in diesem Artikel beschriebene Integration basiert auf [Azure Event Grid](https://azure.microsoft.com/services/event-grid/). Stellen Sie sicher, dass Ihr Abonnement für den Event Grid-Ressourcenanbieter registriert ist. Weitere Informationen finden Sie unter [Ressourcenanbieter und -typen](../azure-resource-manager/management/resource-providers-and-types.md#azure-portal). Sie müssen dazu in der Lage sein, die Aktion *Microsoft.EventGrid/eventSubscriptions/* * auszuführen. Diese Aktion ist Teil der integrierten Rolle EventGrid-EventSubscription-Mitwirkender.

## <a name="data-factory-ui"></a>Data Factory-Benutzeroberfläche

In diesem Abschnitt wird gezeigt, wie Sie über die Benutzeroberfläche von Azure Data Factory einen Trigger für Speicherereignisse erstellen können.

1. Wechseln Sie zur Registerkarte **Bearbeiten**, die mit einem Stiftsymbol angezeigt wird.

1. Wählen Sie im Menü **Trigger** und dann **Neu/Bearbeiten** aus.

1. Wählen Sie auf der Seite **Trigger hinzufügen** die Option **Trigger auswählen...** und dann **+Neu** aus.

1. Wählen Sie den Triggertyp **Storage Event** (Speicherereignis) aus.

    :::image type="content" source="media/how-to-create-event-trigger/event-based-trigger-image1.png" alt-text="Screenshot der Seite „Autor“ zum Erstellen eines neuen Speicherereignisauslösers über die Data Factory-Benutzeroberfläche.":::

1. Wählen Sie aus der Dropdownliste für die Azure-Abonnements Ihr Storage-Konto aus, oder geben Sie die Ressourcen-ID des Storage-Kontos ein. Wählen Sie den Container aus, in dem die Ereignisse auftreten sollen. Die Containerauswahl ist erforderlich, aber beachten Sie, dass die Auswahl aller Container zu einer hohen Anzahl von Ereignissen führen kann.

   > [!NOTE]
   > Der Speicherereignistrigger unterstützt derzeit nur Azure Data Lake Storage Gen2-Speicherkonten und universelle Speicherkonten (Version 2). Aufgrund einer Einschränkung von Azure Event Grid unterstützt Azure Data Factory nur maximal 500 Speicherereignistrigger pro Speicherkonto. Wenn Sie das Limit erreichen, wenden Sie sich bitte an den Support, um Empfehlungen zu erhalten und das Limit nach Auswertung durch das Event Grid-Team zu erhöhen. 

   > [!NOTE]
   > Um einen neuen Speicherereignisauslöser zu erstellen oder einen vorhandenen zu ändern, muss das Azure-Konto, das für die Anmeldung bei Data Factory und die Veröffentlichung des Speicherereignisauslösers verwendet wird, über die entsprechende rollenbasierte Zugriffssteuerungsberechtigung (Azure RBAC) für das Speicherkonto verfügen. Zusätzliche Berechtigungen sind nicht erforderlich: Der Dienstprinzipal für Azure Data Factory benötigt _keine_ spezielle Berechtigung für das Speicherkonto oder Event Grid. Weitere Informationen zur Zugriffssteuerung finden Sie im Abschnitt [Rollenbasierte Zugriffssteuerung](#role-based-access-control).

1. Sie können die Eigenschaften **Blob path begins with** (Blobpfad beginnt mit) und **Blob path ends with** (Blobpfad endet mit) verwenden, um die Container, Ordner und Blobnamen anzugeben, für die Ereignisse empfangen werden sollen. Der Speicherereignistrigger erfordert, dass mindestens eine dieser Eigenschaften definiert wird. Für die Eigenschaften **Blob-Pfad beginnt mit** und **Blob-Pfad endet mit** können Sie eine Vielzahl von Mustern verwenden, wie in den Beispielen im weiteren Verlauf dieses Artikels dargestellt.

    * **Blob path begins with** (Blobpfad beginnt mit): Der Blobpfad muss mit einem Ordnerpfad beginnen. Beispielsweise können die folgenden Werte verwendet werden: `2018/` und `2018/april/shoes.csv`. Dieses Feld kann nicht ausgewählt werden, wenn kein Container ausgewählt ist.
    * **Blob path ends with** (Blobpfad endet mit): Der Blobpfad muss auf einem Dateinamen oder einer Erweiterung enden. Beispielsweise können die folgenden Werte verwendet werden: `shoes.csv` und `.csv`. Container- und Ordnernamen müssen, sofern angegeben, durch ein `/blobs/`-Segment getrennt werden. Beispielsweise kann ein Container mit dem Namen „Bestellungen“ den Wert `/orders/blobs/2018/april/shoes.csv` haben. Wenn Sie einen Ordner in einem beliebigen Container angeben möchten, lassen Sie das vorangestellte Zeichen „/“ aus. Beispielsweise löst `april/shoes.csv` ein Ereignis für jede Datei mit dem Namen `shoes.csv` aus, die sich in einem beliebigen Container in einem Ordner mit dem Namen „April“ befindet.
    * Beachten Sie, dass für den Musterabgleich in Speicherereignisauslösern lediglich die Blobpfadfestlegungen **Beginnt mit** und **Endet auf** zulässig sind. Andere Arten des Platzhalterabgleichs sind für den Auslösertyp nicht zulässig.

1. Wählen Sie aus, ob der Trigger auf ein **Blob created**-Ereignis (Blob erstellt), auf ein **Blob deleted**-Ereignis (Blob gelöscht) oder auf beides reagieren soll. Jedes Ereignis löst die Data Factory-Pipelines am angegebenen Speicherort aus, die dem Trigger zugeordnet sind.

    :::image type="content" source="media/how-to-create-event-trigger/event-based-trigger-image2.png" alt-text="Screenshot der Seite zum Erstellen eines Speicherereignisauslösers.":::

1. Wählen Sie aus, ob Ihr Trigger Blobs mit null Bytes ignorieren soll oder nicht.

1. Nachdem Sie den Trigger konfiguriert haben, klicken Sie auf **Weiter: Datenvorschau**. Diese Anzeige informiert über die vorhandenen Blobs, die Ihrer Konfiguration für Speicherereignistrigger entsprechen. Vergewissern Sie sich, dass Sie genaue Filter ausgewählt haben. Wenn Sie Filter auswählen, die zu weit gefasst sind, können diese auf eine große Anzahl an erstellten bzw. gelöschten Dateien zutreffen. Dafür können höhere Kosten anfallen. Sobald Sie Ihre Filterbedingungen überprüft haben, klicken Sie auf **Fertig stellen**.

    :::image type="content" source="media/how-to-create-event-trigger/event-based-trigger-image3.png" alt-text="Screenshot der Vorschauseite für einen Speicherereignisauslöser.":::

1. Sie können eine Pipeline an diesen Trigger anfügen, indem Sie zum Bereich „Pipeline“ navigieren und erst auf **Trigger** und dann auf **New/Edit** (Neu/Bearbeiten) klicken. Wenn die Seitennavigationsleiste angezeigt wird, klicken Sie auf die Dropdownliste **Choose trigger...** (Trigger auswählen...), und wählen Sie den von Ihnen erstellten Trigger aus. Klicken Sie auf **Weiter: Datenvorschau**, um zu überprüfen, ob die Konfiguration stimmt, und klicken Sie anschließend auf **Weiter**, um die Datenvorschau zu überprüfen.

1. Wenn Ihre Pipeline über Parameter verfügt, können Sie diese auf der Seitennavigationsleiste „Trigger run parameter“ (Parameter für die Triggerausführung) angeben. Der Speicherereignistrigger erfasst den Ordnerpfad und den Dateinamen des Blobs in den Eigenschaften `@triggerBody().folderPath` und `@triggerBody().fileName`. Um die Werte dieser Eigenschaften in einer Pipeline zu verwenden, müssen Sie die Eigenschaften Pipelineparametern zuordnen. Nach der Zuordnung der Eigenschaften zu Parametern können Sie über den Ausdruck `@pipeline().parameters.parameterName` auf die vom Trigger erfassten Werte in der gesamten Pipeline zugreifen. Eine ausführliche Erläuterung finden Sie unter [Verweis auf Triggermetadaten in Pipelines](how-to-use-trigger-parameterization.md).

   :::image type="content" source="media/how-to-create-event-trigger/event-based-trigger-image4.png" alt-text="Screenshot der Zuordnung von Eigenschaften durch den Speicherereignisauslöser zu Pipelineparametern.":::

   Im vorherigen Beispiel wurde der Trigger so konfiguriert, dass er ausgelöst wird, wenn im Container _sample-data_ (Beispieldaten) ein Blobpfad mit der Erweiterung „.csv“ im Ordner _event-testing_ (Ereignistest) erstellt wird. Die Eigenschaften **folderPath** and **fileName** erfassen den Speicherort des neuen Blobs. Wenn z. B. „MoviesDB.csv“ zum Pad „sample-data/event-testing“ (Beispieldaten/Ereignistest) hinzugefügt wird, hat `@triggerBody().folderPath` den Wert `sample-data/event-testing` und `@triggerBody().fileName` den Wert `moviesDB.csv`. Diese Werte werden im Beispiel den Pipelineparametern `sourceFolder` und `sourceFile` zugeordnet, die in der gesamten Pipeline jeweils als `@pipeline().parameters.sourceFolder` bzw. `@pipeline().parameters.sourceFile` verwendet werden können.

   > [!NOTE]
   > Wenn Sie die Pipeline und den Trigger in [Azure Synapse Analytics](../synapse-analytics/overview-what-is.md) erstellen, müssen Sie `@trigger().outputs.body.fileName` und `@trigger().outputs.body.folderPath` als Parameter verwenden. Diese beiden Eigenschaften erfassen Blobinformationen. Verwenden Sie diese Eigenschaften anstelle von `@triggerBody().fileName` und `@triggerBody().folderPath`.

1. Wenn Sie fertig sind, klicken Sie auf **Fertig stellen**.

## <a name="json-schema"></a>JSON-Schema

Die folgende Tabelle bietet eine Übersicht über die Schemaelemente, die mit Speicherereignistriggern verknüpft sind:

| **JSON-Element** | **Beschreibung** | **Typ** | **Zulässige Werte** | **Erforderlich** |
| ---------------- | --------------- | -------- | ------------------ | ------------ |
| **scope** | Die Ressourcen-ID von Azure Resource Manager im Speicherkonto. | String | Azure Resource Manager-ID | Ja |
| **events** | Der Ereignistyp, die diesen Trigger auslöst. | Array    | Microsoft.Storage.BlobCreated, Microsoft.Storage.BlobDeleted | Ja (beliebige Kombination dieser Werte). |
| **blobPathBeginsWith** | Der Blobpfad muss mit dem angegebenen Muster beginnen, damit der Trigger ausgelöst wird. `/records/blobs/december/` löst den Trigger beispielsweise nur für Blobs im Ordner `december` unter dem Container `records` aus. | String   | | Geben Sie einen Wert für mindestens eine der folgenden Eigenschaften an: `blobPathBeginsWith` oder `blobPathEndsWith`. |
| **blobPathEndsWith** | Der Blobpfad muss mit dem angegebenen Muster enden, damit der Trigger ausgelöst wird. `december/boxes.csv` löst den Trigger beispielsweise nur für Blobs namens `boxes` aus, die sich in einem Ordner namens `december` befinden. | String   | | Geben Sie einen Wert für mindestens eine der folgenden Eigenschaften an: `blobPathBeginsWith` oder `blobPathEndsWith`. |
| **ignoreEmptyBlobs** | Legt fest, ob Null-Byte-Blobs eine Pipelineausführung auslösen oder nicht. Dieser Wert ist standardmäßig auf „true“ festgelegt. | Boolean | true oder false | Nein |

## <a name="examples-of-storage-event-triggers"></a>Beispiele für Speicherereignistrigger

Dieser Abschnitt enthält Beispiele für die Einstellungen für Speicherereignistrigger.

> [!IMPORTANT]
> Sie müssen das Segment `/blobs/` des Pfads wie in den folgenden Beispielen gezeigt immer dann einbeziehen, wenn Sie Container und Ordner, Container und Datei oder Container, Ordner und Datei angeben. Für **blobPathBeginsWith** fügt die Benutzeroberfläche für Data Factory automatisch das Segment `/blobs/` zwischen dem Ordner und dem Containernamen zum JSON-Code des Auslösers ein.

| Eigenschaft | Beispiel | BESCHREIBUNG |
|---|---|---|
| **Blobpfad beginnt mit** | `/containername/` | Empfängt Ereignisse für ein beliebiges Blob im Container. |
| **Blobpfad beginnt mit** | `/containername/blobs/foldername/` | Empfängt Ereignisse für ein beliebiges Blob im Container `containername` und Ordner `foldername`. |
| **Blobpfad beginnt mit** | `/containername/blobs/foldername/subfoldername/` | Sie können auch auf einen Unterordner verweisen. |
| **Blobpfad beginnt mit** | `/containername/blobs/foldername/file.txt` | Empfängt Ereignisse für ein Blob namens `file.txt` im Ordner `foldername` unter dem Container `containername`. |
| **Blobpfad endet mit** | `file.txt` | Empfängt Ereignisse für ein Blob namens `file.txt` unter einem beliebigen Pfad. |
| **Blobpfad endet mit** | `/containername/blobs/file.txt` | Empfängt Ereignisse für ein Blob namens `file.txt` unter dem Container `containername`. |
| **Blobpfad endet mit** | `foldername/file.txt` | Empfängt Ereignisse für ein Blob namens `file.txt` im Ordner `foldername` unter einem beliebigen Container. |

## <a name="role-based-access-control"></a>Rollenbasierte Zugriffssteuerung

In Azure Data Factory wird die rollenbasierte Zugriffssteuerung von Azure (Azure RBAC) verwendet, um sicherzustellen, dass unbefugte Zugriffe zum Belauschen von, zum Abonnieren von Aktualisierungen von sowie zum Auslösen von Pipelines, die mit Blobereignissen verknüpft sind, streng verboten sind.

* Zum erfolgreichen Erstellen eines neuen oder Aktualisieren eines vorhandenen Speicherereignisauslösers muss das bei Data Factory angemeldete Azure-Konto über entsprechenden Zugriff auf das relevante Speicherkonto verfügen. Andernfalls schlägt der Vorgang mit der Meldung _Zugriff verweigert_ fehl.
* Data Factory benötigt keine spezielle Berechtigung für Ihr Event Grid, und Sie müssen dem Data Factory-Dienstprinzipal _keine_ spezielle RBAC-Berechtigung für den Vorgang zuweisen.

Folgende RBAC-Einstellungen eignen sich für einen Speicherereignisauslöser:

* Besitzerrolle für das Speicherkonto
* Die Rolle „Mitwirkender“ für das Speicherkonto
* _Microsoft.EventGrid/EventSubscriptions/Write_-Berechtigung für das Speicherkonto _/subscriptions/####/resourceGroups/####/providers/Microsoft.Storage/storageAccounts/storageAccountName_

Um zu verstehen, wie Azure Data Factory die beiden Zusagen erfüllt, lassen Sie uns einen Schritt zurücktreten und einen Blick hinter die Kulissen werfen. Im Folgenden finden Sie die allgemeinen Workflows für die Integration von Data Factory, Storage und Event Grid.

### <a name="create-a-new-storage-event-trigger"></a>Erstellen eines neuen Speicherereignisauslösers

Dieser allgemeine Workflow beschreibt, wie Azure Data Factory mit Event Grid interagiert, um einen Speicherereignisauslöser zu erstellen.

:::image type="content" source="media/how-to-create-event-trigger/storage-event-trigger-5-create-subscription.png" alt-text="Workflow der Erstellung eines Speicherereignisauslösers.":::

Zwei wichtige Anmerkungen zu den Workflows:

* Azure Data Factory stellt _keinen_ direkten Kontakt mit dem Speicherkonto her. Die Anforderung zum Erstellen eines Abonnements wird stattdessen weitergeleitet und von Event Grid verarbeitet. Daher benötigt Data Factory für diesen Schritt keine Berechtigung für das Speicherkonto.

* Zugriffssteuerung und Berechtigungsüberprüfung erfolgen auf der Seite von Azure Data Factory. Bevor ADF eine Anforderung zum Abonnieren eines Speicherereignisses sendet, wird die Berechtigung für den Benutzer überprüft. Genauer gesagt wird überprüft, ob das angemeldete Azure-Konto, das versucht, den Speicherereignisauslöser zu erstellen, entsprechenden Zugriff auf das relevante Speicherkonto hat. Wenn die Berechtigungsüberprüfung fehlschlägt, schlägt die Erstellung des Auslösers ebenfalls fehl.

### <a name="storage-event-trigger-data-factory-pipeline-run"></a>Auslösung einer Data Factory-Pipelineausführung durch Speicherereignisse

Diese allgemeinen Workflows beschreiben, wie Speicherereignisse eine Pipelineausführung über Event Grid auslösen.

:::image type="content" source="media/how-to-create-event-trigger/storage-event-trigger-6-trigger-pipeline.png" alt-text="Workflow der Auslösung von Pipelineausführungen durch Speicherereignisse.":::

Im Hinblick auf die Auslösung von Pipelines in Data Factory durch Ereignisse drei wichtige Anmerkungen zum Workflow:

* Event Grid verwendet ein Pushmodell, d. h., es leitet die Nachricht so schnell wie möglich weiter, wenn die Nachricht vom Speicher im System abgelegt wird. Das ist ein Unterschied zu Messaging-Systemen, wie z. B. Kafka, die ein Pullsystem verwenden.
* Der Ereignisauslöser in Azure Data Factory dient als aktiver Listener für die eingehende Nachricht und löst die zugehörige Pipeline ordnungsgemäß aus.
* Der Speicherereignisauslöser selbst hat keinen direkten Kontakt zum Speicherkonto.

  * Wenn jedoch in der Pipeline eine Kopier- oder andere Aktivität ausgeführt wird, um die Daten im Speicherkonto zu verarbeiten, nimmt Data Factory direkt Kontakt mit Storage auf und verwendet dabei die im verknüpften Dienst gespeicherten Anmeldeinformationen. Stellen Sie sicher, dass der verknüpfte Dienst ordnungsgemäß eingerichtet ist.
  * Wenn in der Pipeline jedoch kein Verweis auf das Speicherkonto erfolgt, müssen Sie Data Factory keine Berechtigung für den Zugriff auf das Speicherkonto erteilen.

## <a name="next-steps"></a>Nächste Schritte

* Detaillierte Informationen zu Triggern finden Sie unter [Pipelineausführung und -trigger](concepts-pipeline-execution-triggers.md#trigger-execution).
* Informationen zum Verweisen auf Triggermetadaten in der Pipeline finden Sie unter [Verweisen auf Triggermetadaten in Pipelineausführungen](how-to-use-trigger-parameterization.md).
