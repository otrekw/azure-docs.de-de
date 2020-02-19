---
title: Senden oder Empfangen von Ereignissen an bzw. von Azure Event Hubs unter Verwendung von JavaScript (Legacypaket)
description: Dieser Artikel enthält eine exemplarische Vorgehensweise zum Erstellen einer JavaScript-Anwendung, die unter Verwendung des alten Pakets „azure/event-hubs“ (Version 2) Ereignisse an Azure Event Hubs sendet bzw. von dort empfängt.
services: event-hubs
author: spelluru
ms.service: event-hubs
ms.workload: core
ms.topic: quickstart
ms.date: 01/15/2020
ms.author: spelluru
ms.openlocfilehash: 0a4b76bd1febca864cab6060fbdbd96dd0061cff
ms.sourcegitcommit: 76bc196464334a99510e33d836669d95d7f57643
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/12/2020
ms.locfileid: "77162615"
---
# <a name="quickstart-send-events-to-or-receive-events-from-azure-event-hubs-using-javascript-azureevent-hubs-version-2"></a>Schnellstart: Senden oder Empfangen von Ereignissen an bzw. von Event Hubs mithilfe von JavaScript (@azure/event-hubs, Version 2)
In dieser Schnellstartanleitung erfahren Sie, wie Sie mithilfe des JavaScript-Pakets „azure/event-hubs“ (Version 2) JavaScript-Anwendungen erstellen, um Ereignisse an einen Event Hub zu senden bzw. von dort zu empfangen. 

> [!WARNING]
> In dieser Schnellstartanleitung wird das alte Paket „azure/event-hubs“ (Version 2) verwendet. Eine Schnellstartanleitung, in der die aktuelle **Version 5** des Pakets verwendet wird, finden Sie unter [Senden oder Empfangen von Ereignissen an bzw. von Event Hubs unter Verwendung von JavaScript (Version 5 von „azure/event-hubs“)](get-started-node-send-v2.md). Informationen zur Umstellung Ihrer Anwendung vom alten auf das neue Paket finden Sie im [Leitfaden zur Migration von Version 1 von „azure/eventhubs“ zu Version 5](https://github.com/Azure/azure-sdk-for-js/blob/master/sdk/eventhub/event-hubs/migrationguide.md). 


## <a name="prerequisites"></a>Voraussetzungen

Wenn Sie mit Azure Event Hubs noch nicht vertraut sind, lesen Sie vor dem Durcharbeiten dieser Schnellstartanleitung die Informationen unter [Übersicht über Event Hubs](event-hubs-about.md). 

Zum Durchführen dieser Schnellstartanleitung benötigen Sie Folgendes:

- **Microsoft Azure-Abonnement.** Für die Verwendung von Azure-Diensten benötigen Sie ein Abonnement. Das gilt auch für Azure Event Hubs.  Falls Sie noch nicht über ein Azure-Konto verfügen, können Sie sich für eine [kostenlose Testversion](https://azure.microsoft.com/free/) registrieren oder beim [Erstellen eines Kontos](https://azure.microsoft.com) Ihre MSDN-Abonnentenvorteile nutzen.
- Node.js, Version 8.x oder höher. Laden Sie die neueste LTS-Version von [https://nodejs.org](https://nodejs.org) herunter.
- Visual Studio Code (empfohlen) oder eine beliebige andere IDE.
- **Erstellen Sie einen Event Hubs-Namespace und einen Event Hub**. Verwenden Sie zunächst das [Azure-Portal](https://portal.azure.com), um einen Namespace vom Typ „Event Hubs“ zu erstellen, und beschaffen Sie die Verwaltungsanmeldeinformationen, die Ihre Anwendung für die Kommunikation mit dem Event Hub benötigt. Folgen Sie dem Ablauf in [diesem Artikel](event-hubs-create.md), um einen Namespace und einen Event Hub zu erstellen, und fahren Sie dann mit den folgenden Schritten in diesem Tutorial fort. Gehen Sie dann wie im folgenden Artikel beschrieben vor, um die Verbindungszeichenfolge für den Event Hub-Namespace abzurufen: [Abrufen der Verbindungszeichenfolge](event-hubs-get-connection-string.md#get-connection-string-from-the-portal). Die Verbindungszeichenfolge wird im weiteren Verlauf dieses Tutorials benötigt.


### <a name="install-npm-package"></a>Installieren des npm-Pakets
Öffnen Sie eine Eingabeaufforderung, die `npm` im Pfad enthält, ändern Sie das Verzeichnis in den Ordner, in dem die Beispiele gespeichert werden sollen, und führen Sie dann den folgenden Befehl aus, um das [npm-Paket für Event Hubs](https://www.npmjs.com/package/@azure/event-hubs/v/2.1.0) zu installieren.

```shell
npm install @azure/event-hubs@2
```

Führen Sie stattdessen den folgenden Befehl aus, um das [npm-Paket für den Ereignisprozessorhost](https://www.npmjs.com/package/@azure/event-processor-host) zu installieren.

```shell
npm install @azure/event-processor-host
```

## <a name="send-events"></a>Senden von Ereignisse

In diesem Abschnitt erfahren Sie, wie Sie eine JavaScript-Anwendung zum Senden von Ereignissen an einen Event Hub erstellen. 

> [!NOTE]
> Sie können diese Schnellstartvorlage als Beispiel aus [GitHub](https://github.com/Azure/azure-event-hubs-node/tree/master/client) herunterladen, die Zeichenfolgen `EventHubConnectionString` und `EventHubName` durch die Werte für Ihren Event Hub ersetzen und das Beispiel dann ausführen. Alternativ können Sie auch die Schritte in diesem Tutorial ausführen, um eine eigene Vorlage zu erstellen.

1. Öffnen Sie Ihren bevorzugten Editor, z. B. [Visual Studio Code](https://code.visualstudio.com). 
2. Erstellen Sie eine Datei namens `send.js`, und fügen Sie den folgenden Code ein. Gehen Sie wie im folgenden Artikel beschrieben vor, um die Verbindungszeichenfolge für den Event Hub-Namespace abzurufen: [Abrufen der Verbindungszeichenfolge](event-hubs-get-connection-string.md#get-connection-string-from-the-portal). 

    ```javascript
    const { EventHubClient } = require("@azure/event-hubs@2");

    // Connection string - primary key of the Event Hubs namespace. 
    // For example: Endpoint=sb://myeventhubns.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
    const connectionString = "Endpoint=sb://<EVENT HUBS NAMESPACE NAME>.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=<SHARED ACCESS KEY>";

    // Name of the event hub. For example: myeventhub
    const eventHubsName = "<EVENT HUB NAME>";

    async function main() {
      const client = EventHubClient.createFromConnectionString(connectionString, eventHubsName);

      for (let i = 0; i < 100; i++) {
        const eventData = {body: `Event ${i}`};
        console.log(`Sending message: ${eventData.body}`);
        await client.send(eventData);
      }

      await client.close();
    }

    main().catch(err => {
      console.log("Error occurred: ", err);
    });
    ```
3. Geben Sie die Verbindungszeichenfolge und den Namen Ihres Event Hubs in den obigen Code ein.
4. Führen Sie anschließend den Befehl `node send.js` in einer Eingabeaufforderung aus, um diese Datei auszuführen. Dadurch werden 100 Ereignisse an Ihren Event Hub gesendet.

Glückwunsch! Sie haben jetzt Ereignisse an einen Event Hub gesendet.


## <a name="receive-events"></a>Empfangen von Ereignissen

In diesem Abschnitt wird veranschaulicht, wie Sie eine JavaScript-Anwendung erstellen, die Ereignisse von einer einzelnen Partition der Standardconsumergruppe in einem Event Hub empfängt. 

1. Öffnen Sie Ihren bevorzugten Editor, z. B. [Visual Studio Code](https://code.visualstudio.com). 
2. Erstellen Sie eine Datei namens `receive.js`, und fügen Sie den folgenden Code ein.
    ```javascript
    const { EventHubClient, delay } = require("@azure/event-hubs@2");

    // Connection string - primary key of the Event Hubs namespace. 
    // For example: Endpoint=sb://myeventhubns.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
    const connectionString = "Endpoint=sb://<EVENT HUBS NAMESPACE NAME>.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=<SHARED ACCESS KEY>";

    // Name of the event hub. For example: myeventhub
    const eventHubsName = "<EVENT HUB NAME>";

    async function main() {
      const client = EventHubClient.createFromConnectionString(connectionString, eventHubsName);
      const allPartitionIds = await client.getPartitionIds();
      const firstPartitionId = allPartitionIds[0];

      const receiveHandler = client.receive(firstPartitionId, eventData => {
        console.log(`Received message: ${eventData.body} from partition ${firstPartitionId}`);
      }, error => {
        console.log('Error when receiving message: ', error)
      });

      // Sleep for a while before stopping the receive operation.
      await delay(15000);
      await receiveHandler.stop();

      await client.close();
    }

    main().catch(err => {
      console.log("Error occurred: ", err);
    });
    ```
3. Geben Sie die Verbindungszeichenfolge und den Namen Ihres Event Hubs in den obigen Code ein.
4. Führen Sie anschließend den Befehl `node receive.js` in einer Eingabeaufforderung aus, um diese Datei auszuführen. Dadurch werden Ereignisse aus einer anderen Partition der Standardconsumergruppe in Ihrem Event Hub empfangen.

Glückwunsch! Hiermit haben Sie erfolgreich Ereignisse von Event Hub empfangen.

## <a name="receive-events-using-event-processor-host"></a>Empfangen von Ereignissen mithilfe des Ereignisprozessorhosts

In diesem Abschnitt erfahren Sie, wie Sie mithilfe eines Azure-[Ereignisprozessorhosts](event-hubs-event-processor-host.md) (EventProcessorHost) in einer JavaScript-Anwendung Ereignisse von einem Event Hub empfangen. Der Ereignisprozessorhost ermöglicht den effizienten Empfang von Ereignissen von einem Event Hub. Zu diesem Zweck werden auf allen Partitionen in der Consumergruppe eines Event Hubs Empfänger erstellt. In einer Azure Storage Blob-Instanz werden die Metadaten empfangener Nachrichten in regelmäßigen Abständen mit Prüfpunkten versehen. Dadurch kann der Empfang von Nachrichten später an der Stelle fortgesetzt werden, an der Sie aufgehört haben.

1. Öffnen Sie Ihren bevorzugten Editor, z. B. [Visual Studio Code](https://code.visualstudio.com). 
2. Erstellen Sie eine Datei namens `receiveAll.js`, und fügen Sie den folgenden Code ein.
    ```javascript
    const { EventProcessorHost, delay } = require("@azure/event-processor-host");

    // Connection string - primary key of the Event Hubs namespace. 
    // For example: Endpoint=sb://myeventhubns.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
    const eventHubConnectionString = "Endpoint=sb://<EVENT HUBS NAMESPACE NAME>.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=<SHARED ACCESS KEY>";

    // Name of the event hub. For example: myeventhub
    const eventHubName = "<EVENT HUB NAME>";

    // Azure Storage connection string
    const storageConnectionString = "DefaultEndpointsProtocol=https;AccountName=<STORAGE ACCOUNT NAME>;AccountKey=<STORAGE ACCOUNT KEY>;EndpointSuffix=core.windows.net";

    async function main() {
      const eph = EventProcessorHost.createFromConnectionString(
        "my-eph",
        storageConnectionString,
        "my-storage-container-name",
        eventHubConnectionString,
        {
          eventHubPath: eventHubName,
          onEphError: (error) => {
            console.log("[%s] Error: %O", error);
          }
        }
      );


      eph.start((context, eventData) => {
        console.log(`Received message: ${eventData.body} from partition ${context.partitionId}`);
      }, error => {
        console.log('Error when receiving message: ', error)
      });

      // Sleep for a while before stopping the receive operation.
      await delay(15000);
      await eph.stop();
    }

    main().catch(err => {
      console.log("Error occurred: ", err);
    });

    ```
3. Geben Sie die Verbindungszeichenfolge und den Namen Ihres Event Hubs sowie die Verbindungszeichenfolge für eine Azure Blob Storage-Instanz in den obigen Code ein.
4. Führen Sie anschließend den Befehl `node receiveAll.js` in einer Eingabeaufforderung aus, um diese Datei auszuführen.

Glückwunsch! Soeben haben Sie Ereignisse von einem Event Hub mithilfe des Ereignisprozessorhosts erhalten. Dadurch werden Ereignisse aus allen anderen Partitionen der Standardconsumergruppe in Ihrem Event Hub empfangen.

## <a name="next-steps"></a>Nächste Schritte
Lesen Sie die folgenden Artikel:

- [EventProcessorHost](event-hubs-event-processor-host.md)
- [Features und Terminologie in Azure Event Hubs](event-hubs-features.md)
- [Event Hubs – häufig gestellte Fragen](event-hubs-faq.md)
- Sehen Sie sich weitere JavaScript-Beispiele für [Event Hubs](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/eventhub/event-hubs/samples) und den [Ereignisprozessorhost](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/eventhub/event-processor-host/samples) auf GitHub an.
