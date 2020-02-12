---
title: Senden oder Empfangen von Ereignissen an bzw. von Azure Event Hubs unter Verwendung von Node.js (aktuelles Paket)
description: Dieser Artikel enthält eine exemplarische Vorgehensweise zum Erstellen einer Node.js-Anwendung, die unter Verwendung des aktuellen Pakets „azure/event-hubs“ (Version 5) Ereignisse an Azure Event Hubs sendet bzw. von dort empfängt.
services: event-hubs
author: spelluru
ms.service: event-hubs
ms.workload: core
ms.topic: quickstart
ms.date: 01/30/2020
ms.author: spelluru
ms.openlocfilehash: b523e4a7b463564cbfeb407c91b7bb05317f8166
ms.sourcegitcommit: 67e9f4cc16f2cc6d8de99239b56cb87f3e9bff41
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/31/2020
ms.locfileid: "76906367"
---
# <a name="send-events-to-or-receive-events-from-event-hubs-by-using-nodejs--azureevent-hubs-version-5"></a>Senden oder Empfangen von Ereignissen an bzw. von Event Hubs unter Verwendung von Node.js (Version 5 von „azure/event-hubs“)

Azure Event Hubs ist eine Big Data-Streamingplattform und ein Ereigniserfassungsdienst, der Millionen von Ereignissen pro Sekunde empfangen und verarbeiten kann. Event Hubs können Ereignisse, Daten oder Telemetriedaten, die von verteilter Software und verteilten Geräten erzeugt wurden, verarbeiten und speichern. An einen Event Hub gesendete Daten können transformiert und mit einem beliebigen Echtzeitanalyseanbieter oder Batchverarbeitungs-/Speicheradapter gespeichert werden. Weitere Informationen finden Sie unter [Azure Event Hubs: Big Data-Streamingplattform und Ereigniserfassungsdienst](event-hubs-about.md) sowie unter [Features und Terminologie in Azure Event Hubs](event-hubs-features.md).

In dieser Schnellstartanleitung erfahren Sie, wie Sie Node.js-Anwendungen erstellen, die Ereignisse an einen Event Hub senden bzw. von diesem empfangen können.

> [!IMPORTANT]
> In dieser Schnellstartanleitung wird Version 5 des JavaScript SDK für Azure Event Hubs verwendet. Eine Schnellstartanleitung, in der Version 2 des JavaScript SDK verwendet wird, finden Sie in [diesem Artikel](event-hubs-node-get-started-send.md). 

## <a name="prerequisites"></a>Voraussetzungen

Zum Durchführen dieser Schnellstartanleitung benötigen Sie Folgendes:

- Ein Azure-Abonnement. Falls Sie kein Abonnement besitzen, können Sie ein [kostenloses Konto erstellen](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio), bevor Sie beginnen.  
- Node.js ab Version 8.x. Die aktuelle Version mit langfristigem Support (Long-Term Support, LTS) steht [hier](https://nodejs.org) zum Download bereit.  
- Visual Studio Code (empfohlen) oder eine andere integrierte Entwicklungsumgebung (Integrated Development Environment, IDE)  
- Ein aktiver Event Hubs-Namespace und ein aktiver Event Hub. Diese können Sie wie folgt erstellen: 

   1. Erstellen Sie im [Azure-Portal](https://portal.azure.com) einen Namespace vom Typ *Event Hubs*, und rufen Sie anschließend die Verwaltungsanmeldeinformationen ab, die Ihre Anwendung für die Kommunikation mit dem Event Hub benötigt. 
   1. Eine Anleitung zum Erstellen des Namespace und des Event Hubs finden Sie unter [Schnellstart: Erstellen eines Event Hubs mithilfe des Azure-Portals](event-hubs-create.md).
   1. Fahren Sie mit den Schritten in dieser Schnellstartanleitung fort. 
   1. Eine Anleitung zum Abrufen der Verbindungszeichenfolge für Ihren Event Hub-Namespace finden Sie unter [Abrufen der Verbindungszeichenfolge im Portal](event-hubs-get-connection-string.md#get-connection-string-from-the-portal). Notieren Sie sich die Verbindungszeichenfolge zur späteren Verwendung in dieser Schnellstartanleitung.

### <a name="install-the-npm-package"></a>Installieren des npm-Pakets
Öffnen Sie eine Eingabeaufforderung, die *npm* im Pfad enthält, ändern Sie das Verzeichnis in den Ordner, in dem die Beispiele gespeichert werden sollen, und führen Sie dann den folgenden Befehl aus, um das [npm-Paket für Event Hubs](https://www.npmjs.com/package/@azure/event-hubs) zu installieren:

```shell
npm install @azure/event-hubs
```

Für die empfangende Seite müssen zwei weitere Pakete installiert werden. In dieser Schnellstartanleitung wird Azure Blob Storage zum Speichern von Prüfpunkten verwendet, damit das Programm die bereits gelesenen Ereignisse nicht erneut liest. In einem Blob werden für empfangene Nachrichten in regelmäßigen Abständen Metadatenprüfpunkte erstellt. Dadurch kann der Empfang von Nachrichten später problemlos an der Stelle fortgesetzt werden, an der Sie aufgehört haben.

Führen Sie die folgenden Befehle aus:

```shell
npm install @azure/storage-blob
```

```shell
npm install @azure/eventhubs-checkpointstore-blob
```

## <a name="send-events"></a>Senden von Ereignisse

In diesem Abschnitt wird eine Node.js-Anwendung erstellt, die Ereignisse an einen Event Hub sendet.

1. Öffnen Sie Ihren bevorzugten Editor, z. B. [Visual Studio Code](https://code.visualstudio.com).
1. Erstellen Sie eine Datei namens *send.js*, und fügen Sie den folgenden Code ein:

    ```javascript
    const { EventHubProducerClient } = require("@azure/event-hubs");

    const connectionString = "EVENT HUBS NAMESPACE CONNECTION STRING";
    const eventHubName = "EVENT HUB NAME";

    async function main() {

      // Create a producer client to send messages to the event hub.
      const producer = new EventHubProducerClient(connectionString, eventHubName);

      // Prepare a batch of three events.
      const batch = await producer.createBatch();
      batch.tryAdd({ body: "First event" });
      batch.tryAdd({ body: "Second event" });
      batch.tryAdd({ body: "Third event" });    

      // Send the batch to the event hub.
      await producer.sendBatch(batch);

      // Close the producer client.
      await producer.close();

      console.log("A batch of three events have been sent to the event hub");
    }

    main().catch((err) => {
      console.log("Error occurred: ", err);
    });
    ```
1. Ersetzen Sie im Code Folgendes durch echte Werte:
    * `EVENT HUBS NAMESPACE CONNECTION STRING` 
    * `EVENT HUB NAME`
1. Führen Sie `node send.js` aus, um diese Datei auszuführen. Durch diesen Befehl wird ein Batch mit drei Ereignissen an Ihren Event Hub gesendet.
1. Vergewissern Sie sich im Azure-Portal, dass der Event Hub die Nachrichten erhalten hat. Wechseln Sie im Abschnitt **Metriken** zur Ansicht **Nachrichten**. Aktualisieren Sie die Seite, um das Diagramm zu aktualisieren. Es kann einige Sekunden dauern, bis der Empfang der Nachrichten angezeigt wird.

    [![Überprüfen, ob der Event Hub die Nachrichten empfangen hat](./media/getstarted-dotnet-standard-send-v2/verify-messages-portal.png)](./media/getstarted-dotnet-standard-send-v2/verify-messages-portal.png#lightbox)

    > [!NOTE]
    > Der gesamte Quellcode (einschließlich zusätzlicher hilfreicher Kommentare) steht auf der [GitHub-Seite „sendEvents.js“](https://github.com/Azure/azure-sdk-for-js/blob/master/sdk/eventhub/event-hubs/samples/javascript/sendEvents.js) zur Verfügung.

Glückwunsch! Sie haben jetzt Ereignisse an einen Event Hub gesendet.


## <a name="receive-events"></a>Empfangen von Ereignissen
In diesem Abschnitt werden mithilfe eines Azure Blob Storage-Prüfpunktspeichers in einer Node.js-Anwendung Ereignisse von einem Event Hub empfangen. In einem Azure Storage-Blob werden für empfangene Nachrichten in regelmäßigen Abständen Metadatenprüfpunkte erstellt. Dadurch kann der Empfang von Nachrichten später problemlos an der Stelle fortgesetzt werden, an der Sie aufgehört haben.

### <a name="create-an-azure-storage-account-and-a-blob-container"></a>Erstellen eines Azure-Speicherkontos und eines Blobcontainers
Führen Sie die folgenden Schritte aus, um ein Azure-Speicherkonto und einen darin befindlichen Blobcontainer zu erstellen:

1. [Erstellen eines Azure-Speicherkontos](../storage/common/storage-account-create.md?tabs=azure-portal)  
2. [Erstellen eines Blobcontainers im Speicherkonto](../storage/blobs/storage-quickstart-blobs-portal.md#create-a-container)  
3. [Abrufen der Verbindungszeichenfolge für das Speicherkonto](../storage/common/storage-configure-connection-string.md?#view-and-copy-a-connection-string)

Notieren Sie sich die Verbindungszeichenfolge und den Containernamen zur späteren Verwendung im Empfangscode.

### <a name="write-code-to-receive-events"></a>Schreiben von Code zum Empfangen von Ereignissen

1. Öffnen Sie Ihren bevorzugten Editor, z. B. [Visual Studio Code](https://code.visualstudio.com).
1. Erstellen Sie eine Datei namens *receive.js*, und fügen Sie den folgenden Code ein:

    ```javascript
    const { EventHubConsumerClient } = require("@azure/event-hubs");
    const { ContainerClient } = require("@azure/storage-blob");    
    const { BlobCheckpointStore } = require("@azure/eventhubs-checkpointstore-blob");

    const connectionString = "EVENT HUBS NAMESPACE CONNECTION STRING";    
    const eventHubName = "EVENT HUB NAME";
    const consumerGroup = "$Default"; // name of the default consumer group
    const storageConnectionString = "AZURE STORAGE CONNECTION STRING";
    const containerName = "BLOB CONTAINER NAME";

    async function main() {
      // Create a blob container client and a blob checkpoint store using the client.
      const containerClient = new ContainerClient(storageConnectionString, containerName);
      const checkpointStore = new BlobCheckpointStore(containerClient);

      // Create a consumer client for the event hub by specifying the checkpoint store.
      const consumerClient = new EventHubConsumerClient(consumerGroup, connectionString, eventHubName, checkpointStore);

      // Subscribe to the events, and specify handlers for processing the events and errors.
      const subscription = consumerClient.subscribe({
          processEvents: async (events, context) => {
            for (const event of events) {
              console.log(`Received event: '${event.body}' from partition: '${context.partitionId}' and consumer group: '${context.consumerGroup}'`);
            }
            // Update the checkpoint.
            await context.updateCheckpoint(events[events.length - 1]);
          },

          processError: async (err, context) => {
            console.log(`Error : ${err}`);
          }
        }
      );

      // After 30 seconds, stop processing.
      await new Promise((resolve) => {
        setTimeout(async () => {
          await subscription.close();
          await consumerClient.close();
          resolve();
        }, 30000);
      });
    }

    main().catch((err) => {
      console.log("Error occurred: ", err);
    });    
    ```
1. Ersetzen Sie im Code die folgenden Werte durch echte Werte:
    - `EVENT HUBS NAMESPACE CONNECTION STRING`
    - `EVENT HUB NAME`
    - `AZURE STORAGE CONNECTION STRING`
    - `BLOB CONTAINER NAME`
1. Führen Sie `node receive.js` an einer Eingabeaufforderung aus, um diese Datei auszuführen. Im Fenster sollten Meldungen zu empfangenen Ereignissen angezeigt werden.

    > [!NOTE]
    > Der gesamte Quellcode (einschließlich zusätzlicher hilfreicher Kommentare) steht auf der [GitHub-Seite „receiveEventsUsingCheckpointStore.js“](https://github.com/Azure/azure-sdk-for-js/blob/master/sdk/eventhub/eventhubs-checkpointstore-blob/samples/receiveEventsUsingCheckpointStore.js) zur Verfügung.

Glückwunsch! Sie haben erfolgreich Ereignisse von Ihrem Event Hub empfangen. Das Empfängerprogramm empfängt Ereignisse von allen Partitionen der Standardconsumergruppe im Event Hub.

## <a name="next-steps"></a>Nächste Schritte
Sehen Sie sich diese Beispiele auf GitHub an:

- [JavaScript samples](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/eventhub/event-hubs/samples/javascript) (JavaScript-Beispiele)
- [TypeScript-Beispiele](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/eventhub/event-hubs/samples/typescript)
