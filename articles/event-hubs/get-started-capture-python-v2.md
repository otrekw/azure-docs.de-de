---
title: Lesen erfasster Azure Event Hubs-Daten aus einer Python-App | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie, wie Sie Python-Code zum Erfassen von Daten, die an einen Event Hub gesendet werden, sowie zum Lesen der erfassten Ereignisdaten aus Azure Storage schreiben.
services: event-hubs
documentationcenter: ''
author: spelluru
editor: ''
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/05/2019
ms.author: spelluru
ms.openlocfilehash: 43223f7cb9ed254340c99d235d494d1e93583c7f
ms.sourcegitcommit: 7221918fbe5385ceccf39dff9dd5a3817a0bd807
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/21/2020
ms.locfileid: "76293537"
---
# <a name="capture-event-hubs-data-in-azure-storage-and-read-it-using-python"></a>Erfassen von Event Hubs-Daten in Azure Storage und Lesen dieser Daten mithilfe von Python 
Sie können einen Event Hub so konfigurieren, dass die an einen Event Hub gesendeten Daten in Azure Storage oder Azure Data Lake Storage erfasst werden. In diesem Artikel erfahren Sie, wie Sie Python-Code zum Senden von Ereignissen an einen Event Hub sowie zum Lesen der erfassten Daten aus Azure Blob Storage verwenden. Weitere Informationen zu diesem Feature finden Sie unter [Event Hubs-Erfassungsfunktion: Übersicht](event-hubs-capture-overview.md).

In diesem Beispiel wird das [Azure Python SDK](https://azure.microsoft.com/develop/python/) verwendet, um die Verwendung des Erfassungsfeatures zu veranschaulichen. Mit dem Programm „sender.py“ werden simulierte Telemetriedaten der Umgebung im JSON-Format an Event Hubs gesendet. Der Event Hub ist für die Verwendung des Capture-Features konfiguriert, um diese Daten in Batches in den Blobspeicher zu schreiben. Die App „capturerereader.py“ liest diese Blobs und erstellt jeweils eine Anfügedatei pro Gerät. Anschließend schreibt die App die Daten in CSV-Dateien.

> [!IMPORTANT]
> In diesem Schnellstart wird Version 5 des Python SDK für Azure Event Hubs verwendet. Eine Schnellstartanleitung, in der die alte Version 1 des Python SDK verwendet wird, finden Sie in [diesem Artikel](event-hubs-capture-python.md). Wenn Sie Version 1 des SDK verwenden, empfiehlt es sich, den Code zur neuesten Version zu migrieren. Weitere Informationen finden Sie im [Migrationsleitfaden](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/eventhub/azure-eventhub/migration_guide.md).

In dieser Schnellstartanleitung führen Sie die folgenden Schritte aus: 

> [!div class="checklist"]
> * Erstellen Sie im Azure-Portal ein Azure Blob Storage-Konto und einen Container.
> * Erstellen Sie einen Event Hubs-Namespace mithilfe des Azure-Portals.
> * Erstellen Sie einen Event Hub mit aktivierter Erfassungsfunktion, und verbinden Sie ihn mit Ihrem Speicherkonto.
> * Senden Sie Daten mithilfe eines Python-Skripts an den Event Hub.
> * Lesen und Verarbeiten Sie Dateien aus Event Hubs Capture mithilfe eines weiteren Python-Skripts.

## <a name="prerequisites"></a>Voraussetzungen

- Python 2.7 und 3.5 oder höher mit installiertem und aktualisiertem `pip`.
- Ein Azure-Abonnement. Falls Sie kein Abonnement besitzen, können Sie ein [kostenloses Konto erstellen](https://azure.microsoft.com/free/), bevor Sie beginnen.
- [Erstellen Sie einen Event Hubs-Namespace und einen Event Hub im Namespace](event-hubs-create.md). Notieren Sie sich den Namen des Event Hubs-Namespace, den Namen des Event Hubs und den primären Zugriffsschlüssel für den Namespace. Gehen Sie wie im Artikel beschrieben vor, um den Zugriffsschlüssel abzurufen: [Abrufen der Verbindungszeichenfolge](event-hubs-get-connection-string.md#get-connection-string-from-the-portal). Der Standardschlüsselname lautet: **RootManageSharedAccessKey**. Die Verbindungszeichenfolge ist für das Tutorial nicht erforderlich. Sie benötigen lediglich den Primärschlüssel. 
- Führen Sie die folgenden Schritte aus, um ein **Azure Storage-Konto** und einen **Blobcontainer** zu erstellen:
    1. [Erstellen Sie ein Azure Storage-Konto](../storage/common/storage-quickstart-create-account.md?tabs=azure-portal).
    2. [Erstellen Sie einen Blobcontainer im Speicher](../storage/blobs/storage-quickstart-blobs-portal.md#create-a-container). 
    3. [Abrufen der Verbindungszeichenfolge für das Speicherkonto](../storage/common/storage-configure-connection-string.md#view-and-copy-a-connection-string).

        Notieren Sie sich die **Verbindungszeichenfolge** und den **Containernamen**. Beide Angaben werden später im Code verwendet. 
- Aktivieren Sie das Feature **Capture** für den Event Hub mithilfe der folgenden Anweisungen: [Aktivieren von Event Hubs Capture über das Azure-Portal](event-hubs-capture-enable-through-portal.md) Wählen Sie das Speicherkonto und den Blobcontainer aus, die Sie im vorherigen Schritt erstellt haben. Sie können das Feature auch aktivieren, wenn Sie einen Event Hub erstellen. 

## <a name="create-a-python-script-to-send-events-to-your-event-hub"></a>Erstellen eines Python-Skripts zum Senden von Ereignissen an Ihren Event Hub
In diesem Abschnitt erstellen Sie ein Python-Skript, das 200 Ereignisse (10 Geräte * 20 Ereignisse) an einen Event Hub sendet. Bei den Ereignissen handelt es sich um einfache Umweltbeispieldaten im JSON-Format. 

1. Öffnen Sie Ihren bevorzugten Python-Editor, z.B. [Visual Studio Code][Visual Studio Code].
2. Erstellen Sie ein Skript mit dem Namen **sender.py**. 
3. Fügen Sie den folgenden Code in „sender.py“ ein. Weitere Einzelheiten finden Sie in den Codekommentaren. 
   
    ```python
    import time
    import os
    import uuid
    import datetime
    import random
    import json
    
    from azure.eventhub import EventHubProducerClient, EventData
    
    # this scripts simulates production of events for 10 devices
    devices = []
    for x in range(0, 10):
        devices.append(str(uuid.uuid4()))
    
    # create a producer client to produce/publish events to the event hub
    producer = EventHubProducerClient.from_connection_string(conn_str="EVENT HUBS NAMESAPCE CONNECTION STRING", eventhub_name="EVENT HUB NAME")
    
    for y in range(0,20):    # for each device, produce 20 events 
        event_data_batch = producer.create_batch() # create a batch. you will add events to the batch later. 
        for dev in devices:
            # create a dummy reading
            reading = {'id': dev, 'timestamp': str(datetime.datetime.utcnow()), 'uv': random.random(), 'temperature': random.randint(70, 100), 'humidity': random.randint(70, 100)}
            s = json.dumps(reading) # convert reading into a JSON string
            event_data_batch.add(EventData(s)) # add event data to the batch
        producer.send_batch(event_data_batch) # send the batch of events to the event hub
    
    # close the producer    
    producer.close()
    ```
4. Ersetzen Sie die folgenden Werte in den Skripts:
    1. Ersetzen Sie `EVENT HUBS NAMESPACE CONNECTION STRING` durch die Verbindungszeichenfolge für Ihren Event Hubs-Namespace.
    2. Ersetzen Sie `EVENT HUB NAME` durch den Namen Ihres Event Hubs. 
5. Führen Sie das Skript aus, um Ereignisse an den Event Hub zu senden. 
6. Im Azure-Portal können Sie überprüfen, ob der Event Hub die Nachrichten empfangen hat. Wechseln Sie im Abschnitt **Metriken** zur Ansicht **Nachrichten**. Aktualisieren Sie die Seite, um das Diagramm zu aktualisieren. Es kann einige Sekunden dauern, bis angezeigt wird, dass die Nachrichten empfangen wurden. 

    [![Überprüfen, ob der Event Hub die Nachrichten empfangen hat](./media/get-started-capture-python-v2/messages-portal.png)](./media/get-started-capture-python-v2/messages-portal.png#lightbox)

## <a name="create-a-python-script-to-read-your-capture-files"></a>Erstellen eines Python-Skripts zum Lesen Ihrer Erfassungsdateien
In diesem Beispiel werden die erfassten Daten in Azure Blob Storage gespeichert. Das Skript in diesem Abschnitt liest die Dateien mit den erfassten Daten aus Azure Storage und generiert CSV-Dateien, damit Sie den Inhalt problemlos öffnen und anzeigen können. Im aktuellen Arbeitsverzeichnis der Anwendung werden 10 Dateien angezeigt. Diese Dateien enthalten die Umgebungsmesswerte für die 10 Geräte. 

1. Erstellen Sie in Ihrem Python-Editor ein Skript namens **capturereader.py**. Dieses Skript liest die erfassten Dateien und erstellt jeweils eine Datei pro Gerät, um nur die Daten für das jeweilige Gerät zu schreiben.
2. Fügen Sie den folgenden Code in „capturereader.py“ ein. Weitere Einzelheiten finden Sie in den Codekommentaren. 
   
    ```python
    import os
    import string
    import json
    import uuid
    import avro.schema
    
    from azure.storage.blob import ContainerClient, BlobClient
    from avro.datafile import DataFileReader, DataFileWriter
    from avro.io import DatumReader, DatumWriter
    
    
    def processBlob2(filename):
        reader = DataFileReader(open(filename, 'rb'), DatumReader())
        dict = {}
        for reading in reader:
            parsed_json = json.loads(reading["Body"])
            if not 'id' in parsed_json:
                return
            if not parsed_json['id'] in dict:
                list = []
                dict[parsed_json['id']] = list
            else:
                list = dict[parsed_json['id']]
                list.append(parsed_json)
        reader.close()
        for device in dict.keys():
            filename = os.getcwd() + '\\' + str(device) + '.csv'
            deviceFile = open(filename, "a")
            for r in dict[device]:
                deviceFile.write(", ".join([str(r[x]) for x in r.keys()])+'\n')
    
    def startProcessing():
        print('Processor started using path: ' + os.getcwd())
        # create a blob container client
        container = ContainerClient.from_connection_string("AZURE STORAGE CONNECTION STRING", container_name="BLOB CONTAINER NAME")
        blob_list = container.list_blobs() # list all the blobs in the container
        for blob in blob_list:
            #content_length == 508 is an empty file, so only process content_length > 508 (skip empty files)        
            if blob.size > 508:
                print('Downloaded a non empty blob: ' + blob.name)
                # create a blob client for the blob
                blob_client = ContainerClient.get_blob_client(container, blob=blob.name)
                # construct a file name based on the blob name
                cleanName = str.replace(blob.name, '/', '_')
                cleanName = os.getcwd() + '\\' + cleanName 
                with open(cleanName, "wb+") as my_file: # open the file to write. create if it doesn't exist. 
                    my_file.write(blob_client.download_blob().readall()) # write blob contents into the file
                processBlob2(cleanName) # convert the file into a CSV file
                os.remove(cleanName) # remove the original downloaded file
                # delete the blob from the container after it's read
                container.delete_blob(blob.name)
    
    startProcessing()    
    ```
4. Ersetzen Sie `<AZURE STORAGE CONNECTION STRING>` durch die Verbindungszeichenfolge für Ihr Azure Storage-Konto. Der Name des Containers, den Sie in diesem Tutorial erstellt haben, lautet: **capture**. Wenn Sie einen anderen Namen für den Container verwendet haben, ersetzen Sie `capture` durch den Namen des Containers im Speicherkonto. 

## <a name="run-the-scripts"></a>Ausführen der Skripts
1. Öffnen Sie eine Eingabeaufforderung, deren Pfad Python enthält, und führen Sie dann diese Befehle aus, um die Python-Pakete mit den erforderlichen Komponenten zu installieren:
   
   ```
   pip install azure-storage-blob
   pip install azure-eventhub
   pip install avro-python3
   ```
2. Wechseln Sie zu dem Verzeichnis, in dem Sie „sender.py“ und „capturereader.py“ gespeichert haben, und führen Sie den folgenden Befehl aus:
   
   ```
   python sender.py
   ```
   
   Dieser Befehl startet einen neuen Python-Prozess zum Ausführen des Absenders.
3. Warten Sie einige Minuten, während die Erfassung ausgeführt wird. Geben Sie dann den folgenden Befehl in das ursprüngliche Befehlsfenster ein:
   
   ```
   python capturereader.py
   ```

   Dieser Erfassungsprozessor verwendet das lokale Verzeichnis, um alle Blobs aus dem Speicherkonto/Container herunterzuladen. Er verarbeitet alle Blobs, die nicht leer sind, und schreibt die Ergebnisse als CSV-Dateien in das lokale Verzeichnis.

## <a name="next-steps"></a>Nächste Schritte
Sehen Sie sich [hier](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/eventhub/azure-eventhub/samples) Python-Beispiele auf GitHub an. 


[Azure portal]: https://portal.azure.com/
[Overview of Event Hubs Capture]: event-hubs-capture-overview.md
[1]: ./media/event-hubs-archive-python/event-hubs-python1.png
[About Azure storage accounts]:../storage/common/storage-create-storage-account.md
[Visual Studio Code]: https://code.visualstudio.com/
[Event Hubs overview]: event-hubs-what-is-event-hubs.md
