---
title: 'Senden und Empfangen von Ereignissen unter Verwendung von Node.js: Azure Event Hubs'
description: Dieser Artikel enthält eine exemplarische Vorgehensweise für die Erstellung einer Node.js-Anwendung, die Ereignisse aus Azure Event Hubs sendet.
services: event-hubs
author: spelluru
ms.service: event-hubs
ms.workload: core
ms.topic: article
ms.date: 01/09/2020
ms.author: spelluru
ms.openlocfilehash: d4810c325acc42d5aa665002654cb01154cdc6bb
ms.sourcegitcommit: 3dc1a23a7570552f0d1cc2ffdfb915ea871e257c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/15/2020
ms.locfileid: "75981612"
---
# <a name="send-events-to-or-receive-events-from-azure-event-hubs-using-nodejs"></a>Senden von Ereignissen an oder Empfangen von Ereignissen aus Event Hubs mithilfe von Node.js

Azure Event Hubs ist eine Big Data-Streamingplattform und ein Ereigniserfassungsdienst, der Millionen von Ereignissen pro Sekunde empfangen und verarbeiten kann. Event Hubs kann Ereignisse, Daten oder Telemetriedaten, die von verteilter Software und verteilten Geräten erzeugt wurden, verarbeiten und speichern. An einen Event Hub gesendete Daten können transformiert und mit einem beliebigen Echtzeitanalyse-Anbieter oder Batchverarbeitungs-/Speicheradapter gespeichert werden. Eine ausführliche Übersicht über Event Hubs finden Sie unter [Was ist Azure Event Hubs?](event-hubs-about.md) und [Event Hubs-Features im Überblick](event-hubs-features.md).

In diesem Tutorial wird beschrieben, wie Sie Node.js-Anwendungen erstellen, die Ereignisse an einen Event Hub senden oder von diesem empfangen.

> [!IMPORTANT]
> In diesem Schnellstart wird Version 5 des Java Script SDK für Azure Event Hubs verwendet. Eine Schnellstartanleitung, in der die alte Version 2 des Java Script SDK verwendet wird, finden Sie in [diesem Artikel](event-hubs-node-get-started-send.md). Wenn Sie Version 2 des SDK verwenden, empfiehlt es sich, den Code zur neuesten Version zu migrieren. Weitere Informationen finden Sie im [Migrationsleitfaden](https://github.com/Azure/azure-sdk-for-js/blob/master/sdk/eventhub/event-hubs/migrationguide.md).

## <a name="prerequisites"></a>Voraussetzungen

Zum Durchführen dieses Tutorials benötigen Sie Folgendes:

- Ein aktives Azure-Konto. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) erstellen, bevor Sie beginnen.
- Node.js, Version 8.x oder höher. Laden Sie die neueste LTS-Version von [https://nodejs.org](https://nodejs.org) herunter.
- Visual Studio Code (empfohlen) oder eine beliebige andere IDE.
- **Erstellen Sie einen Event Hubs-Namespace und einen Event Hub**. Verwenden Sie zunächst das [Azure-Portal](https://portal.azure.com), um einen Namespace vom Typ „Event Hubs“ zu erstellen, und beschaffen Sie die Verwaltungsanmeldeinformationen, die Ihre Anwendung für die Kommunikation mit dem Event Hub benötigt. Folgen Sie dem Ablauf in [diesem Artikel](event-hubs-create.md), um einen Namespace und einen Event Hub zu erstellen, und fahren Sie dann mit den folgenden Schritten in diesem Tutorial fort. Gehen Sie dann wie im folgenden Artikel beschrieben vor, um die Verbindungszeichenfolge für den Event Hub-Namespace abzurufen: [Abrufen der Verbindungszeichenfolge](event-hubs-get-connection-string.md#get-connection-string-from-the-portal). Die Verbindungszeichenfolge wird im weiteren Verlauf dieses Tutorials benötigt.


### <a name="install-npm-packages"></a>Installieren von npm-Paketen
Öffnen Sie eine Eingabeaufforderung, die `npm` im Pfad enthält, ändern Sie das Verzeichnis in den Ordner, in dem die Beispiele gespeichert werden sollen, und führen Sie dann den folgenden Befehl aus, um das [npm-Paket für Event Hubs](https://www.npmjs.com/package/@azure/event-hubs) zu installieren.

```shell
npm install @azure/event-hubs
```

Für die empfangende Seite müssen zwei weitere Pakete installiert werden. In diesem Schnellstart verwenden Sie Azure Blob Storage für die Beibehaltung von Prüfpunkten, damit das Programm die bereits gelesenen Ereignisse nicht liest. In einem Blob werden die Metadaten empfangener Nachrichten in regelmäßigen Abständen mit Prüfpunkten versehen. Dadurch kann der Empfang von Nachrichten später an der Stelle fortgesetzt werden, an der Sie aufgehört haben.

```shell
npm install @azure/storage-blob
```

```shell
npm install @azure/eventhubs-checkpointstore-blob
```

## <a name="send-events"></a>Senden von Ereignisse

In diesem Abschnitt erfahren Sie, wie Sie eine Node.js-Anwendung zum Senden von Ereignissen an einen Event Hub erstellen.

1. Öffnen Sie Ihren bevorzugten Editor, z. B. [Visual Studio Code](https://code.visualstudio.com).
2. Erstellen Sie eine Datei namens `send.js`, und fügen Sie den folgenden Code in die Datei ein.

    ```javascript
    const { EventHubProducerClient } = require("@azure/event-hubs");

    const connectionString = "EVENT HUBS NAMESPACE CONNECTION STRING";
    const eventHubName = "EVENT HUB NAME";

    async function main() {

      // create a producer client to send messages to the event hub
      const producer = new EventHubProducerClient(connectionString, eventHubName);

      // prepare a batch of three events
      const batch = await producer.createBatch();
      batch.tryAdd({ body: "First event" });
      batch.tryAdd({ body: "Second event" });
      batch.tryAdd({ body: "Third event" });    

      // send the batch to the event hub
      await producer.sendBatch(batch);

      // close the producer client
      await producer.close();

      console.log("A batch of three events have been sent to the event hub");
    }

    main().catch((err) => {
      console.log("Error occurred: ", err);
    });
    ```
3. Vergessen Sie nicht, die Werte für **Verbindungszeichenfolge** und **Event Hub-Name** im Code zu ersetzen.
5. Führen Sie den Befehl `node send.js` aus, um diese Datei auszuführen. Dadurch wird ein Batch mit drei Ereignissen an Ihren Event Hub gesendet.
6. Im Azure-Portal können Sie überprüfen, ob der Event Hub die Nachrichten empfangen hat. Wechseln Sie im Abschnitt **Metriken** zur Ansicht **Nachrichten**. Aktualisieren Sie die Seite, um das Diagramm zu aktualisieren. Es kann einige Sekunden dauern, bis angezeigt wird, dass die Nachrichten empfangen wurden.

    [![Überprüfen, ob der Event Hub die Nachrichten empfangen hat](./media/getstarted-dotnet-standard-send-v2/verify-messages-portal.png)](./media/getstarted-dotnet-standard-send-v2/verify-messages-portal.png#lightbox)

    > [!NOTE]
    > Den gesamten Quellcode mit weiteren informativen Kommentaren finden Sie [auf GitHub in dieser Datei](https://github.com/Azure/azure-sdk-for-js/blob/master/sdk/eventhub/event-hubs/samples/javascript/sendEvents.js).

Glückwunsch! Sie haben jetzt Ereignisse an einen Event Hub gesendet.


## <a name="receive-events"></a>Empfangen von Ereignissen
In diesem Abschnitt erfahren Sie, wie Sie mithilfe eines Azure Blob Store-Prüfpunkts in einer Node.js-Anwendung Ereignisse von einem Event Hub empfangen. In einer Azure Storage Blob-Instanz werden die Metadaten empfangener Nachrichten in regelmäßigen Abständen mit Prüfpunkten versehen. Dadurch kann der Empfang von Nachrichten später an der Stelle fortgesetzt werden, an der Sie aufgehört haben.

### <a name="create-an-azure-storage-and-a-blob-container"></a>Erstellen eines Azure Storage- und eines Blobcontainers
Führen Sie die folgenden Schritte aus, um ein Azure Storage-Konto und darin einen Blobcontainer zu erstellen.

1. [Erstellen Sie ein Azure Storage-Konto.](../storage/common/storage-account-create.md?tabs=azure-portal)
2. [Erstellen eines Blobcontainers](../storage/blobs/storage-quickstart-blobs-portal.md#create-a-container)
3. [Abrufen der Verbindungszeichenfolge für das Speicherkonto](../storage/common/storage-configure-connection-string.md?#view-and-copy-a-connection-string)

    Notieren Sie sich die Verbindungszeichenfolge und den Containernamen. Sie werden diese Angaben im Empfangscode verwenden.

### <a name="write-code-to-receive-events"></a>Schreiben von Code zum Empfangen von Ereignissen

1. Öffnen Sie Ihren bevorzugten Editor, z. B. [Visual Studio Code](https://code.visualstudio.com).
2. Erstellen Sie eine Datei namens `receive.js`, und fügen Sie den folgenden Code in die Datei ein. Weitere Einzelheiten finden Sie in den Codekommentaren.
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
      // create a blob container client and a blob checkpoint store using the client
      const containerClient = new ContainerClient(storageConnectionString, containerName);
      const checkpointStore = new BlobCheckpointStore(containerClient);

      // create a consumer client for the event hub by specifying the checkpoint store
      const consumerClient = new EventHubConsumerClient(consumerGroup, connectionString, eventHubName, checkpointStore);

      // subscribe for the events and specify handlers for processing the events and errors.
      const subscription = consumerClient.subscribe({
          processEvents: async (events, context) => {
            for (const event of events) {
              console.log(`Received event: '${event.body}' from partition: '${context.partitionId}' and consumer group: '${context.consumerGroup}'`);
            }
            // update the checkpoint
            await context.updateCheckpoint(events[events.length - 1]);
          },

          processError: async (err, context) => {
            console.log(`Error : ${err}`);
          }
        }
      );

      // after 30 seconds, stop processing
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
3. Vergessen Sie nicht, die **folgenden Werte** im Code anzugeben:
    - Verbindungszeichenfolge für den Event Hubs-Namespace
    - Name des Event Hubs
    - Verbindungszeichenfolge für das Azure Storage-Konto
    - Name des Blobcontainers
5. Führen Sie anschließend den Befehl `node receive.js` in einer Eingabeaufforderung aus, um diese Datei auszuführen. Die Meldungen zu empfangenen Ereignissen sollten im Fenster angezeigt werden.

    > [!NOTE]
    > Den gesamten Quellcode mit weiteren informativen Kommentaren finden Sie [auf GitHub in dieser Datei](https://github.com/Azure/azure-sdk-for-js/blob/master/sdk/eventhub/eventhubs-checkpointstore-blob/samples/receiveEventsUsingCheckpointStore.js).

Glückwunsch! Hiermit haben Sie erfolgreich Ereignisse von Event Hub empfangen. Das Empfängerprogramm empfängt Ereignisse von allen Partitionen der Standardconsumergruppe in Ihrem Event Hub.

## <a name="next-steps"></a>Nächste Schritte
Sehen Sie sich diese Beispiele auf GitHub an:

- [JavaScript samples](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/eventhub/event-hubs/samples/javascript) (JavaScript-Beispiele)
- [TypeScript-Beispiele](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/eventhub/event-hubs/samples/typescript)
