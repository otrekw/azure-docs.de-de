---
title: Senden oder Empfangen von Ereignissen an bzw. von Azure Event Hubs unter Verwendung von Python (aktuelles Paket)
description: Dieser Artikel enthält eine exemplarische Vorgehensweise zum Erstellen einer Python-Anwendung, die unter Verwendung des aktuellen Pakets „azure-eventhub“ (Version 5) Ereignisse an Azure Event Hubs sendet bzw. von dort empfängt.
services: event-hubs
author: spelluru
ms.service: event-hubs
ms.workload: core
ms.topic: quickstart
ms.date: 01/30/2020
ms.author: spelluru
ms.openlocfilehash: d977ae9ea8b78664ac1d3a318f58553da696c089
ms.sourcegitcommit: 67e9f4cc16f2cc6d8de99239b56cb87f3e9bff41
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/31/2020
ms.locfileid: "76906358"
---
# <a name="send-events-to-or-receive-events-from-event-hubs-by-using-python-azure-eventhub-version-5"></a>Senden oder Empfangen von Ereignissen an bzw. von Event Hubs unter Verwendung von Python (Version 5 von „azure-eventhub“)

Azure Event Hubs ist eine Big Data-Streamingplattform und ein Ereigniserfassungsdienst, der Millionen von Ereignissen pro Sekunde empfangen und verarbeiten kann. Event Hubs können Ereignisse, Daten oder Telemetriedaten, die von verteilter Software und verteilten Geräten erzeugt wurden, verarbeiten und speichern. An einen Event Hub gesendete Daten können transformiert und mit einem beliebigen Echtzeitanalyseanbieter oder Batchverarbeitungs-/Speicheradapter gespeichert werden. Weitere Informationen finden Sie unter [Azure Event Hubs: Big Data-Streamingplattform und Ereigniserfassungsdienst](event-hubs-about.md) sowie unter [Features und Terminologie in Azure Event Hubs](event-hubs-features.md).

In dieser Schnellstartanleitung erfahren Sie, wie Sie Python-Anwendungen erstellen, die Ereignisse an einen Event Hub senden bzw. von diesem empfangen können.

> [!IMPORTANT]
> In diesem Schnellstart wird Version 5 des Python SDK für Azure Event Hubs verwendet. Eine Schnellstartanleitung, in der Version 1 des Python SDK verwendet wird, finden Sie in [diesem Artikel](event-hubs-python-get-started-send.md). 

## <a name="prerequisites"></a>Voraussetzungen

Zum Durchführen dieser Schnellstartanleitung benötigen Sie Folgendes:

- Ein Azure-Abonnement. Falls Sie kein Abonnement besitzen, können Sie ein [kostenloses Konto erstellen](https://azure.microsoft.com/free/), bevor Sie beginnen.
- Ein aktiver Event Hubs-Namespace und -Event Hub. Informationen zu ihrer Erstellung finden Sie unter [Schnellstart: Erstellen eines Event Hubs mithilfe des Azure-Portals](event-hubs-create.md). Notieren Sie sich den Namespace- und den Event Hub-Namen zur späteren Verwendung in dieser Schnellstartanleitung.
- Den Namen des freigegebenen Zugriffsschlüssels und den Wert des Primärschlüssels für Ihren Event Hubs-Namespace. Rufen Sie den Namen und den Wert des Zugriffsschlüssels ab, indem Sie die Anweisungen unter [Abrufen einer Event Hubs-Verbindungszeichenfolge](event-hubs-get-connection-string.md#get-connection-string-from-the-portal) befolgen. Der Standardname des Zugriffsschlüssels lautet *RootManageSharedAccessKey*. Notieren Sie den Namen des Schlüssels und den Wert des Primärschlüssels zur späteren Verwendung in dieser Schnellstartanleitung.
- Python 2.7 bzw. 3.5 oder höher mit installiertem und aktualisiertem PIP
- Das Python-Paket für Event Hubs. 

    Um das Paket zu installieren, führen Sie diesen Befehl in einer Eingabeaufforderung aus, in deren Pfad Python enthalten ist:

    ```cmd
    pip install azure-eventhub
    ```

    Installieren Sie das folgende Paket, um die Ereignisse mithilfe von Azure Blob Storage als Prüfpunktspeicher zu empfangen:

    ```cmd
    pip install azure-eventhub-checkpointstoreblob-aio
    ```

## <a name="send-events"></a>Senden von Ereignisse
In diesem Abschnitt erstellen Sie ein Python-Skript, um Ereignisse an den Event Hub zu senden, den Sie zuvor erstellt haben.

1. Öffnen Sie Ihren bevorzugten Python-Editor, z.B. [Visual Studio Code](https://code.visualstudio.com/).
2. Erstellen Sie ein Skript mit dem Namen *send.py*. Dieses Skript sendet einen Batch von Ereignissen an den Event Hub, den Sie zuvor erstellt haben.
3. Fügen Sie den folgenden Code in *send.py* ein:

    ```python
    import asyncio
    from azure.eventhub.aio import EventHubProducerClient
    from azure.eventhub import EventData

    async def run():
        # Create a producer client to send messages to the event hub.
        # Specify a connection string to your event hubs namespace and
            # the event hub name.
        producer = EventHubProducerClient.from_connection_string(conn_str="EVENT HUBS NAMESPACE - CONNECTION STRING", eventhub_name="EVENT HUB NAME")
        async with producer:
            # Create a batch.
            event_data_batch = await producer.create_batch()

            # Add events to the batch.
            event_data_batch.add(EventData('First event '))
            event_data_batch.add(EventData('Second event'))
            event_data_batch.add(EventData('Third event'))

            # Send the batch of events to the event hub.
            await producer.send_batch(event_data_batch)

    loop = asyncio.get_event_loop()
    loop.run_until_complete(run())

    ```

    > [!NOTE]
    > Der gesamte Quellcode (einschließlich hilfreicher Kommentare) steht auf der [GitHub-Seite „send_async.py“](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/eventhub/azure-eventhub/samples/async_samples/send_async.py) zur Verfügung.

## <a name="receive-events"></a>Empfangen von Ereignissen
In dieser Schnellstartanleitung wird Azure Blob Storage als Prüfpunktspeicher verwendet. Der Prüfpunktspeicher wird verwendet, um Prüfpunkte (d. h. die letzte Leseposition) beizubehalten.  

### <a name="create-an-azure-storage-account-and-a-blob-container"></a>Erstellen eines Azure-Speicherkontos und eines Blobcontainers
Führen Sie die folgenden Schritte aus, um ein Azure-Speicherkonto und einen darin befindlichen Blobcontainer zu erstellen:

1. [Erstellen Sie ein Azure Storage-Konto.](../storage/common/storage-account-create.md?tabs=azure-portal)
2. [Erstellen eines Blobcontainers](../storage/blobs/storage-quickstart-blobs-portal.md#create-a-container)
3. [Abrufen der Verbindungszeichenfolge für das Speicherkonto](../storage/common/storage-configure-connection-string.md?#view-and-copy-a-connection-string)

Notieren Sie sich die Verbindungszeichenfolge und den Containernamen zur späteren Verwendung im Empfangscode.


### <a name="create-a-python-script-to-receive-events"></a>Erstellen eines Python-Skripts zum Empfangen von Ereignissen

In diesem Abschnitt erstellen Sie ein Python-Skript, um Ereignisse aus ihrem Event Hub zu empfangen:

1. Öffnen Sie Ihren bevorzugten Python-Editor, z.B. [Visual Studio Code](https://code.visualstudio.com/).
2. Erstellen Sie ein Skript mit dem Namen *recv.py*.
3. Fügen Sie den folgenden Code in *recv.py* ein:

    ```python
    import asyncio
    from azure.eventhub.aio import EventHubConsumerClient
    from azure.eventhub.extensions.checkpointstoreblobaio import BlobCheckpointStore


    async def on_event(partition_context, event):
        # Print the event data.
        print("Received the event: \"{}\" from the partition with ID: \"{}\"".format(event.body_as_str(encoding='UTF-8'), partition_context.partition_id))

        # Update the checkpoint so that the program doesn't read the events
        # that it has already read when you run it next time.
        await partition_context.update_checkpoint(event)

    async def main():
        # Create an Azure blob checkpoint store to store the checkpoints.
        checkpoint_store = BlobCheckpointStore.from_connection_string("AZURE STORAGE CONNECTION STRING", "BLOB CONTAINER NAME")

        # Create a consumer client for the event hub.
        client = EventHubConsumerClient.from_connection_string("EVENT HUBS NAMESPACE CONNECTION STRING", consumer_group="$Default", eventhub_name="EVENT HUB NAME", checkpoint_store=checkpoint_store)
        async with client:
            # Call the receive method.
            await client.receive(on_event=on_event)

    if __name__ == '__main__':
        loop = asyncio.get_event_loop()
        # Run the main method.
        loop.run_until_complete(main())    
    ```

    > [!NOTE]
    > Der gesamte Quellcode (einschließlich zusätzlicher hilfreicher Kommentare) steht auf der [GitHub-Seite „recv_with_checkpoint_store_async.py“](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/eventhub/azure-eventhub/samples/async_samples/recv_with_checkpoint_store_async.py) zur Verfügung.


### <a name="run-the-receiver-app"></a>Ausführen der Empfänger-App

Um das Skript auszuführen, öffnen Sie eine Eingabeaufforderung, deren Pfad Python enthält, und führen Sie dann diesen Befehl aus:

```bash
python recv.py
```

### <a name="run-the-sender-app"></a>Ausführen der Sender-App

Um das Skript auszuführen, öffnen Sie eine Eingabeaufforderung, deren Pfad Python enthält, und führen Sie dann diesen Befehl aus:

```bash
python send.py
```

Die Nachrichten, die an den Event Hub gesendet wurden, sollten im Empfängerfenster angezeigt werden.


## <a name="next-steps"></a>Nächste Schritte
In dieser Schnellstartanleitung haben Sie Ereignisse asynchron gesendet und empfangen. Beispiele zum synchronen Senden und Empfangen von Ereignissen finden Sie auf der [GitHub-Seite „sync_samples“](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/eventhub/azure-eventhub/samples/sync_samples).

Alle (synchronen und asynchronen) Beispiele auf GitHub finden Sie in der [Azure Event Hubs-Clientbibliothek für Python-Beispiele](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/eventhub/azure-eventhub/samples).
