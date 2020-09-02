---
title: Lesen erfasster Azure Event Hubs-Daten aus einer Python-App (aktuelles Paket)
description: In diesem Artikel erfahren Sie, wie Sie Python-Code zum Erfassen von Daten, die an einen Event Hub gesendet werden, sowie zum Lesen der erfassten Ereignisdaten aus einem Azure-Speicherkonto schreiben.
ms.topic: quickstart
ms.date: 06/23/2020
ms.openlocfilehash: cb7165565516136a8425c4c77748c2e13715edb7
ms.sourcegitcommit: 62e1884457b64fd798da8ada59dbf623ef27fe97
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/26/2020
ms.locfileid: "88927866"
---
# <a name="capture-event-hubs-data-in-azure-storage-and-read-it-by-using-python-azure-eventhub-version-5"></a>Erfassen von Event Hubs-Daten in Azure Storage und Lesen dieser Daten mithilfe von Python (azure-eventhub, Version 5)

Sie können einen Event Hub so konfigurieren, dass die an einen Event Hub gesendeten Daten in einem Azure-Speicherkonto oder in Azure Data Lake Storage Gen 1 oder Gen 2 erfasst werden. In diesem Artikel erfahren Sie, wie Sie Python-Code zum Senden von Ereignissen an einen Event Hub sowie zum Lesen der erfassten Daten aus **Azure Blob Storage** verwenden. Weitere Informationen zu diesem Feature finden Sie unter [Erfassen von Ereignissen über Azure Event Hubs in Azure Blob Storage oder Azure Data Lake Storage](event-hubs-capture-overview.md).

In dieser Schnellstartanleitung wird das [Azure Python SDK](https://azure.microsoft.com/develop/python/) verwendet, um die Verwendung des Erfassungsfeatures zu veranschaulichen. Mit der App *sender.py* werden simulierte Telemetriedaten der Umgebung im JSON-Format an Event Hubs gesendet. Der Event Hub ist für die Verwendung des Capture-Features konfiguriert, um diese Daten in Batches in den Blobspeicher zu schreiben. Die App *capturerereader.py* liest diese Blobs und erstellt eine Anfügedatei für jedes Gerät. Anschließend schreibt die App die Daten in CSV-Dateien.

In dieser Schnellstartanleitung führen Sie die folgenden Schritte aus: 

> [!div class="checklist"]
> * Erstellen Sie im Azure-Portal ein Azure Blob Storage-Konto und einen Container.
> * Erstellen Sie einen Event Hubs-Namespace mithilfe des Azure-Portals.
> * Erstellen Sie einen Event Hub mit aktivierter Erfassungsfunktion, und verbinden Sie ihn mit Ihrem Speicherkonto.
> * Senden Sie Daten mithilfe eines Python-Skripts an den Event Hub.
> * Lesen und Verarbeiten Sie Dateien aus Event Hubs Capture mithilfe eines weiteren Python-Skripts.

## <a name="prerequisites"></a>Voraussetzungen

- Python 2.7 und 3.5 oder höher mit installiertem und aktualisiertem PIP  
- Ein Azure-Abonnement. Falls Sie kein Abonnement besitzen, können Sie ein [kostenloses Konto erstellen](https://azure.microsoft.com/free/), bevor Sie beginnen.  
- Ein aktiver Event Hubs-Namespace und -Event Hub.
[Erstellen Sie einen Event Hubs-Namespace und einen Event Hub im Namespace](event-hubs-create.md). Notieren Sie sich den Namen des Event Hubs-Namespace, den Namen des Event Hubs und den primären Zugriffsschlüssel für den Namespace. Anweisungen zum Abrufen des Zugriffsschlüssels finden Sie unter [Abrufen einer Event Hubs-Verbindungszeichenfolge](event-hubs-get-connection-string.md#get-connection-string-from-the-portal). Der Standardname des Schlüssels lautet *RootManageSharedAccessKey*. Für diese Schnellstartanleitung benötigen Sie nur den Primärschlüssel. Die Verbindungszeichenfolge ist nicht erforderlich.  
- Ein Azure-Speicherkonto, ein Blobcontainer im Speicherkonto und eine Verbindungszeichenfolge für das Speicherkonto. Besitzen Sie diese Elemente nicht, führen Sie die folgenden Schritte aus:  
    1. [Erstellen eines Azure-Speicherkontos](../storage/common/storage-account-create.md?tabs=azure-portal)  
    1. [Erstellen eines Blobcontainers im Speicherkonto](../storage/blobs/storage-quickstart-blobs-portal.md#create-a-container)  
    1. [Abrufen der Verbindungszeichenfolge für das Speicherkonto](../storage/common/storage-configure-connection-string.md)

    Notieren Sie sich die Verbindungszeichenfolge und den Containernamen zur späteren Verwendung in dieser Schnellstartanleitung.  
- Aktivieren Sie das Erfassungsfeature für den Event Hub. Befolgen Sie hierzu die Anweisungen in [Aktivieren der Erfassung von über Azure Event Hubs gestreamten Ereignissen](event-hubs-capture-enable-through-portal.md). Wählen Sie das Speicherkonto und den Blobcontainer aus, die Sie im vorherigen Schritt erstellt haben. Sie können das Feature auch aktivieren, wenn Sie einen Event Hub erstellen.  

## <a name="create-a-python-script-to-send-events-to-your-event-hub"></a>Erstellen eines Python-Skripts zum Senden von Ereignissen an Ihren Event Hub
In diesem Abschnitt erstellen Sie ein Python-Skript, das 200 Ereignisse (10 Geräte * 20 Ereignisse) an einen Event Hub sendet. Bei den Ereignissen handelt es sich um einfache Umweltbeispieldaten im JSON-Format. 

1. Öffnen Sie Ihren bevorzugten Python-Editor, z.B. [Visual Studio Code][Visual Studio Code].
2. Erstellen Sie ein Skript mit dem Namen *sender.py*. 
3. Fügen Sie den folgenden Code in *sender.py* ein. 
   
    ```python
    import time
    import os
    import uuid
    import datetime
    import random
    import json
    
    from azure.eventhub import EventHubProducerClient, EventData
    
    # This script simulates the production of events for 10 devices.
    devices = []
    for x in range(0, 10):
        devices.append(str(uuid.uuid4()))
    
    # Create a producer client to produce and publish events to the event hub.
    producer = EventHubProducerClient.from_connection_string(conn_str="EVENT HUBS NAMESAPCE CONNECTION STRING", eventhub_name="EVENT HUB NAME")
    
    for y in range(0,20):    # For each device, produce 20 events. 
        event_data_batch = producer.create_batch() # Create a batch. You will add events to the batch later. 
        for dev in devices:
            # Create a dummy reading.
            reading = {'id': dev, 'timestamp': str(datetime.datetime.utcnow()), 'uv': random.random(), 'temperature': random.randint(70, 100), 'humidity': random.randint(70, 100)}
            s = json.dumps(reading) # Convert the reading into a JSON string.
            event_data_batch.add(EventData(s)) # Add event data to the batch.
        producer.send_batch(event_data_batch) # Send the batch of events to the event hub.
    
    # Close the producer.    
    producer.close()
    ```
4. Ersetzen Sie die folgenden Werte in den Skripts:  
    * Ersetzen Sie `EVENT HUBS NAMESPACE CONNECTION STRING` durch die Verbindungszeichenfolge für Ihren Event Hubs-Namespace.  
    * Ersetzen Sie `EVENT HUB NAME` durch den Namen Ihres Event Hubs.  
5. Führen Sie das Skript aus, um Ereignisse an den Event Hub zu senden.  
6. Im Azure-Portal können Sie überprüfen, ob der Event Hub die Nachrichten empfangen hat. Wechseln Sie im Abschnitt **Metriken** zur Ansicht **Nachrichten**. Aktualisieren Sie die Seite, um das Diagramm zu aktualisieren. Es kann einige Sekunden dauern, bis der Empfang der Nachrichten auf der Seite angezeigt wird. 

    [![Überprüfen, ob der Event Hub die Nachrichten empfangen hat](./media/get-started-capture-python-v2/messages-portal.png)](./media/get-started-capture-python-v2/messages-portal.png#lightbox)

## <a name="create-a-python-script-to-read-your-capture-files"></a>Erstellen eines Python-Skripts zum Lesen Ihrer Erfassungsdateien
In diesem Beispiel werden die erfassten Daten in Azure Blob Storage gespeichert. Das Skript in diesem Abschnitt liest die Dateien mit den erfassten Daten aus Ihrem Azure-Speicherkonto und generiert CSV-Dateien, die problemlos geöffnet und angezeigt werden können. Im aktuellen Arbeitsverzeichnis der Anwendung werden 10 Dateien angezeigt. Diese Dateien enthalten die Umgebungsmesswerte für die 10 Geräte. 

1. Erstellen Sie in Ihrem Python-Editor ein Skript namens *capturereader.py*. Dieses Skript liest die erfassten Dateien und erstellt jeweils eine Datei für jedes Gerät, um nur die Daten für das jeweilige Gerät zu schreiben.
2. Fügen Sie den folgenden Code in *capturereader.py* ein. 
   
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
        # Create a blob container client.
        container = ContainerClient.from_connection_string("AZURE STORAGE CONNECTION STRING", container_name="BLOB CONTAINER NAME")
        blob_list = container.list_blobs() # List all the blobs in the container.
        for blob in blob_list:
            # Content_length == 508 is an empty file, so process only content_length > 508 (skip empty files).        
            if blob.size > 508:
                print('Downloaded a non empty blob: ' + blob.name)
                # Create a blob client for the blob.
                blob_client = ContainerClient.get_blob_client(container, blob=blob.name)
                # Construct a file name based on the blob name.
                cleanName = str.replace(blob.name, '/', '_')
                cleanName = os.getcwd() + '\\' + cleanName 
                with open(cleanName, "wb+") as my_file: # Open the file to write. Create it if it doesn't exist. 
                    my_file.write(blob_client.download_blob().readall()) # Write blob contents into the file.
                processBlob2(cleanName) # Convert the file into a CSV file.
                os.remove(cleanName) # Remove the original downloaded file.
                # Delete the blob from the container after it's read.
                container.delete_blob(blob.name)
    
    startProcessing()    
    ```
3. Ersetzen Sie `AZURE STORAGE CONNECTION STRING` durch die Verbindungszeichenfolge für Ihr Azure-Speicherkonto. Der Name des Containers, den Sie in dieser Schnellstartanleitung erstellt haben, lautet *capture*. Wenn Sie einen anderen Namen für den Container verwendet haben, ersetzen Sie *capture* durch den Namen des Containers im Speicherkonto. 

## <a name="run-the-scripts"></a>Ausführen der Skripts
1. Öffnen Sie eine Eingabeaufforderung, deren Pfad Python enthält, und führen Sie dann diese Befehle aus, um die Python-Pakete mit den erforderlichen Komponenten zu installieren:
   
   ```
   pip install azure-storage-blob
   pip install azure-eventhub
   pip install avro-python3
   ```
2. Wechseln Sie zu dem Verzeichnis, in dem Sie *sender.py* und *capturereader.py* gespeichert haben, und führen Sie den folgenden Befehl aus:
   
   ```
   python sender.py
   ```
   
   Dieser Befehl startet einen neuen Python-Prozess zum Ausführen des Absenders.
3. Warten Sie einige Minuten, bis die Erfassung ausgeführt wird, und geben Sie dann den folgenden Befehl in das ursprüngliche Befehlsfenster ein:
   
   ```
   python capturereader.py
   ```

   Dieser Erfassungsprozessor verwendet das lokale Verzeichnis, um alle Blobs aus dem Speicherkonto und Container herunterzuladen. Er verarbeitet alle Blobs, die nicht leer sind, und schreibt die Ergebnisse als CSV-Dateien in das lokale Verzeichnis.

## <a name="next-steps"></a>Nächste Schritte
Sehen Sie sich die [Python-Beispiele auf GitHub](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/eventhub/azure-eventhub/samples) an. 


[Azure portal]: https://portal.azure.com/
[Overview of Event Hubs Capture]: event-hubs-capture-overview.md
[1]: ./media/event-hubs-archive-python/event-hubs-python1.png
[About Azure storage accounts]:../storage/common/storage-create-storage-account.md
[Visual Studio Code]: https://code.visualstudio.com/
[Event Hubs overview]: ./event-hubs-about.md
