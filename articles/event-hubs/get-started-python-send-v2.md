---
title: Senden oder Empfangen von Ereignissen mit Python und Azure Event Hubs | Microsoft-Dokumentation
description: Dieser Artikel enthält eine exemplarische Vorgehensweise für die Erstellung einer Python-Anwendung, die Ereignisse an Azure Event Hubs sendet.
services: event-hubs
author: spelluru
ms.service: event-hubs
ms.workload: core
ms.topic: article
ms.date: 01/08/2020
ms.author: spelluru
ms.openlocfilehash: d7ab79d49aade7dd6e98cf33ce538174d176c784
ms.sourcegitcommit: af6847f555841e838f245ff92c38ae512261426a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/23/2020
ms.locfileid: "76705342"
---
# <a name="send-events-to-or-receive-events-from-event-hubs-using-python"></a>Senden von Ereignissen an oder Empfangen von Ereignissen aus Event Hubs mithilfe von Python

Azure Event Hubs ist eine Big Data-Streamingplattform und ein Ereigniserfassungsdienst, der pro Sekunde Millionen von Ereignissen empfangen und verarbeiten kann. Event Hubs kann Ereignisse, Daten oder Telemetriedaten, die von verteilter Software und verteilten Geräten erzeugt wurden, verarbeiten und speichern. An einen Event Hub gesendete Daten können transformiert und mit einem beliebigen Echtzeitanalyse-Anbieter oder Batchverarbeitungs-/Speicheradapter gespeichert werden. Eine ausführliche Übersicht über Event Hubs finden Sie unter [Was ist Azure Event Hubs?](event-hubs-about.md) und [Event Hubs-Features im Überblick](event-hubs-features.md).

In diesem Tutorial wird beschrieben, wie Sie Python-Anwendungen erstellen, die Ereignisse an einen Event Hub senden oder von diesem empfangen.

> [!IMPORTANT]
> In diesem Schnellstart wird Version 5 des Python SDK für Azure Event Hubs verwendet. Eine Schnellstartanleitung, in der die alte Version 1 des Python SDK verwendet wird, finden Sie in [diesem Artikel](event-hubs-python-get-started-send.md). Wenn Sie Version 1 des SDK verwenden, empfiehlt es sich, den Code zur neuesten Version zu migrieren. Weitere Informationen finden Sie im [Migrationsleitfaden](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/eventhub/azure-eventhub/migration_guide.md).


## <a name="prerequisites"></a>Voraussetzungen

Zum Durchführen dieses Tutorials benötigen Sie Folgendes:

- Ein Azure-Abonnement. Falls Sie kein Abonnement besitzen, können Sie ein [kostenloses Konto erstellen](https://azure.microsoft.com/free/), bevor Sie beginnen.
- Einen aktiven Event Hubs-Namespace und Event Hub, erstellt durch Befolgen der Anweisungen unter [Schnellstart: Erstellen eines Event Hubs mithilfe des Azure-Portals](event-hubs-create.md). Notieren Sie sich die Namen des Namespace und Event Hub, weil sie später in dieser exemplarischen Vorgehensweise verwendet werden.
- Den Namen des freigegebenen Zugriffsschlüssels und den Wert des Primärschlüssels für Ihren Event Hubs-Namespace. Rufen Sie den Namen und den Wert des Zugriffsschlüssels ab, indem Sie die Anweisungen unter [Abrufen der Verbindungszeichenfolge](event-hubs-get-connection-string.md#get-connection-string-from-the-portal) befolgen. Der Standardname des Zugriffsschlüssels lautet **RootManageSharedAccessKey**. Kopieren Sie den Namen des Zugriffsschlüssels und den Wert des Primärschlüssels, um beide Angaben später in dieser exemplarischen Vorgehensweise zu verwenden.
- Python 2.7 und 3.5 oder höher mit installiertem und aktualisiertem `pip`.
- Das Python-Paket für Event Hubs. Um das Paket zu installieren, führen Sie diesen Befehl in einer Eingabeaufforderung aus, in deren Pfad Python enthalten ist:

    ```cmd
    pip install azure-eventhub
    ```

    Installieren Sie dieses Paket, um die Ereignisse mithilfe von Azure Blob Storage als Prüfpunktspeicher zu empfangen.

    ```cmd
    pip install azure-eventhub-checkpointstoreblob-aio
    ```

## <a name="send-events"></a>Senden von Ereignisse
In diesem Abschnitt erstellen Sie ein Python-Skript, um Ereignisse an den Event Hub zu senden, den Sie zuvor erstellt haben.

1. Öffnen Sie Ihren bevorzugten Python-Editor, z. B. [Visual Studio Code](https://code.visualstudio.com/).
2. Erstellen Sie ein Skript mit dem Namen **send.py**. Dieses Skript sendet einen Batch von Ereignissen an den Event Hub, den Sie zuvor erstellt haben.
3. Fügen Sie den folgenden Code in „send.py“ ein. Weitere Einzelheiten finden Sie in den Codekommentaren.

    ```python
    import asyncio
    from azure.eventhub.aio import EventHubProducerClient
    from azure.eventhub import EventData

    async def run():
        # create a producer client to send messages to the event hub
        # specify connection string to your event hubs namespace and
            # the event hub name
        producer = EventHubProducerClient.from_connection_string(conn_str="EVENT HUBS NAMESPACE - CONNECTION STRING", eventhub_name="EVENT HUB NAME")
        async with producer:
            # create a batch
            event_data_batch = await producer.create_batch()

            # add events to the batch
            event_data_batch.add(EventData('First event '))
            event_data_batch.add(EventData('Second event'))
            event_data_batch.add(EventData('Third event'))

            # send the batch of events to the event hub
            await producer.send_batch(event_data_batch)

    loop = asyncio.get_event_loop()
    loop.run_until_complete(run())

    ```

    > [!NOTE]
    > Den gesamten Quellcode mit sehr nützlichen Kommentaren finden Sie [auf GitHub in dieser Datei](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/eventhub/azure-eventhub/samples/async_samples/send_async.py).

## <a name="receive-events"></a>Empfangen von Ereignissen
In dieser Schnellstartanleitung wird eine Azure Blob Storage-Instanz als Prüfpunktspeicher verwendet. Der Prüfpunktspeicher wird verwendet, um Prüfpunkte (letzte Leseposition) beizubehalten.  

### <a name="create-an-azure-storage-and-a-blob-container"></a>Erstellen eines Azure Storage-Kontos und eines Blobcontainers
Führen Sie die folgenden Schritte aus, um ein Azure Storage-Konto und darin einen Blobcontainer zu erstellen.

1. [Erstellen Sie ein Azure Storage-Konto.](../storage/common/storage-account-create.md?tabs=azure-portal)
2. [Erstellen eines Blobcontainers](../storage/blobs/storage-quickstart-blobs-portal.md#create-a-container)
3. [Abrufen der Verbindungszeichenfolge für das Speicherkonto](../storage/common/storage-configure-connection-string.md?#view-and-copy-a-connection-string)

    Notieren Sie sich die Verbindungszeichenfolge und den Containernamen. Sie werden diese Angaben im Empfangscode verwenden.


### <a name="create-python-script-to-receive-events"></a>Erstellen eines Python-Skripts zum Empfangen von Ereignissen

In diesem Abschnitt erstellen Sie ein Python-Skript, um Ereignisse aus ihrem Event Hub zu empfangen:

1. Öffnen Sie Ihren bevorzugten Python-Editor, z. B. [Visual Studio Code](https://code.visualstudio.com/).
2. Erstellen Sie ein Skript mit dem Namen **recv.py**.
3. Fügen Sie den folgenden Code in „recv.py“ ein. Weitere Einzelheiten finden Sie in den Codekommentaren.

    ```python
    import asyncio
    from azure.eventhub.aio import EventHubConsumerClient
    from azure.eventhub.extensions.checkpointstoreblobaio import BlobCheckpointStore


    async def on_event(partition_context, event):
        # print the event data
        print("Received the event: \"{}\" from the partition with ID: \"{}\"".format(event.body_as_str(encoding='UTF-8'), partition_context.partition_id))

        # update the checkpoint so that the program doesn't read the events
        # that it has already read when you run it next time
        await partition_context.update_checkpoint(event)

    async def main():
        # create an Azure blob checkpoint store to store the checkpoints
        checkpoint_store = BlobCheckpointStore.from_connection_string("AZURE STORAGE CONNECTION STRING", "BLOB CONTAINER NAME")

        # create a consumer client for the event hub
        client = EventHubConsumerClient.from_connection_string("EVENT HUBS NAMESPACE CONNECTION STRING", consumer_group="$Default", eventhub_name="EVENT HUB NAME", checkpoint_store=checkpoint_store)
        async with client:
            # call the receive method
            await client.receive(on_event=on_event)

    if __name__ == '__main__':
        loop = asyncio.get_event_loop()
        # run the main method
        loop.run_until_complete(main())    
    ```

    > [!NOTE]
    > Den gesamten Quellcode mit sehr nützlichen Kommentaren finden Sie [auf GitHub in dieser Datei](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/eventhub/azure-eventhub/samples/async_samples/recv_with_checkpoint_store_async.py).


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
In dieser Schnellstartanleitung haben Sie Ereignisse asynchron gesendet und empfangen. Beispiele zum synchronen Senden und Empfangen von Ereignissen finden Sie [an diesem Speicherort](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/eventhub/azure-eventhub/samples/sync_samples).

Alle Beispiele (synchron und asynchron) finden Sie [hier](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/eventhub/azure-eventhub/samples) auf GitHub.
