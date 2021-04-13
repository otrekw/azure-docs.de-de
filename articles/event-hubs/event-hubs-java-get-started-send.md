---
title: Senden oder Empfangen von Ereignissen an bzw. von Azure Event Hubs unter Verwendung von Java (aktuelles Paket)
description: Dieser Artikel enthält eine exemplarische Vorgehensweise zum Erstellen einer Java-Anwendung, die unter Verwendung des aktuellen Pakets „azure-messaging-eventhubs“ Ereignisse an Azure Event Hubs sendet bzw. von dort empfängt.
ms.topic: quickstart
ms.date: 04/05/2021
ms.custom: devx-track-java
ms.openlocfilehash: bae0d4147fddf57398d494ca7f13c347f892e45e
ms.sourcegitcommit: 56b0c7923d67f96da21653b4bb37d943c36a81d6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2021
ms.locfileid: "106448439"
---
# <a name="use-java-to-send-events-to-or-receive-events-from-azure-event-hubs-azure-messaging-eventhubs"></a>Senden oder Empfangen von Ereignissen an bzw. von Azure Event Hubs unter Verwendung von Java (azure-messaging-eventhubs)
In dieser Schnellstartanleitung erfahren Sie, wie Sie mithilfe des Java-Pakets **azure-messaging-eventhubs** Ereignisse an einen Event Hub senden bzw. von dort empfangen.

> [!IMPORTANT]
> In dieser Schnellstartanleitung wird das neue Paket **azure-messaging-eventhubs** verwendet. Eine Schnellstartanleitung, in der die alten Pakete von **azure-eventhubs** und **azure-eventhubs-eph** verwendet werden, finden Sie unter [Senden oder Empfangen von Ereignissen an bzw. von Azure Event Hubs unter Verwendung von Java (azure-eventhubs)](event-hubs-java-get-started-send-legacy.md). 


## <a name="prerequisites"></a>Voraussetzungen
Wenn Sie mit Azure Event Hubs noch nicht vertraut sind, lesen Sie vor dem Durcharbeiten dieser Schnellstartanleitung die Informationen unter [Übersicht über Event Hubs](event-hubs-about.md). 

Zum Durchführen dieser Schnellstartanleitung benötigen Sie Folgendes:

- **Microsoft Azure-Abonnement.** Für die Verwendung von Azure-Diensten benötigen Sie ein Abonnement. Das gilt auch für Azure Event Hubs.  Falls Sie noch nicht über ein Azure-Konto verfügen, können Sie sich für eine [kostenlose Testversion](https://azure.microsoft.com/free/) registrieren oder beim [Erstellen eines Kontos](https://azure.microsoft.com) Ihre MSDN-Abonnentenvorteile nutzen.
- Eine Java-Entwicklungsumgebung. In dieser Schnellstartanleitung wird [Eclipse](https://www.eclipse.org/) verwendet. Es wird mindestens Java Development Kit (JDK) mit der Version 8 benötigt. 
- **Erstellen Sie einen Event Hubs-Namespace und einen Event Hub**. Verwenden Sie zunächst das [Azure-Portal](https://portal.azure.com), um einen Namespace vom Typ „Event Hubs“ zu erstellen, und beschaffen Sie die Verwaltungsanmeldeinformationen, die Ihre Anwendung für die Kommunikation mit dem Event Hub benötigt. Erstellen Sie anhand der Anleitung in [diesem Artikel](event-hubs-create.md) einen Namespace und einen Event Hub. Gehen Sie dann wie im folgenden Artikel beschrieben vor, um die **Verbindungszeichenfolge für den Event Hubs-Namespace** abzurufen: [Abrufen der Verbindungszeichenfolge](event-hubs-get-connection-string.md#get-connection-string-from-the-portal). Sie verwenden die Verbindungszeichenfolge im weiteren Verlauf dieser Schnellstartanleitung.

## <a name="send-events"></a>Senden von Ereignisse 
In diesem Abschnitt erfahren Sie, wie Sie eine Java-Anwendung zum Senden von Ereignissen an einen Event Hub erstellen. 

### <a name="add-reference-to-azure-event-hubs-library"></a>Hinzufügen von Verweisen auf die Azure Event Hubs-Bibliothek

Die Java-Clientbibliothek für Event Hubs steht über das [zentrale Maven-Repository](https://search.maven.org/search?q=a:azure-messaging-eventhubs) zur Verfügung. Sie können über die folgende Abhängigkeitsdeklaration in Ihrer Maven-Projektdatei auf diese Bibliothek verweisen:

```xml
<dependency>
    <groupId>com.azure</groupId>
    <artifactId>azure-messaging-eventhubs</artifactId>
    <version>5.6.0</version>
</dependency>
```

> [!NOTE]
> Aktualisieren Sie Ihre Version auf die neueste Version, die im Maven-Repository veröffentlicht wurde. 

### <a name="write-code-to-send-messages-to-the-event-hub"></a>Schreiben von Code zum Senden von Nachrichten an den Event Hub

Erstellen Sie für das folgende Beispiel zuerst ein neues Maven-Projekt für eine Konsolen-/Shellanwendung in Ihrer bevorzugten Java-Entwicklungsumgebung. Fügen Sie eine Klasse namens `Sender` sowie den folgenden Code zur Klasse hinzu:

> [!IMPORTANT]
> Aktualisieren Sie `<Event Hubs namespace connection string>` mit der Verbindungszeichenfolge für Ihren Event Hubs-Namespace. Aktualisieren Sie `<Event hub name>` mit dem Namen Ihres Event Hubs im Namespace. 

```java
import com.azure.messaging.eventhubs.*;
import java.util.Arrays;
import java.util.List;

public class Sender {
    private static final String connectionString = "<Event Hubs namespace connection string>";
    private static final String eventHubName = "<Event hub name>";

    public static void main(String[] args) {
        publishEvents();
    }
}
```
### <a name="add-code-to-publish-events-to-the-event-hub"></a>Fügen Sie Code zum Veröffentlichen von Ereignissen im Event Hub ein
Fügen Sie wie nachfolgend gezeigt eine Methode mit dem Namen `publishEvents` in die `Sender`-Klasse ein. 

```java
    /**
     * Code sample for publishing events.
     * @throws IllegalArgumentException if the event data is bigger than max batch size.
     */
    public static void publishEvents() {
        // create a producer client
        EventHubProducerClient producer = new EventHubClientBuilder()
            .connectionString(connectionString, eventHubName)
            .buildProducerClient();

        // sample events in an array
        List<EventData> allEvents = Arrays.asList(new EventData("Foo"), new EventData("Bar"));
        
        // create a batch
        EventDataBatch eventDataBatch = producer.createBatch();

        for (EventData eventData : allEvents) {
            // try to add the event from the array to the batch
            if (!eventDataBatch.tryAdd(eventData)) {
                // if the batch is full, send it and then create a new batch
                producer.send(eventDataBatch);
                eventDataBatch = producer.createBatch();

                // Try to add that event that couldn't fit before.
                if (!eventDataBatch.tryAdd(eventData)) {
                    throw new IllegalArgumentException("Event is too large for an empty batch. Max size: "
                        + eventDataBatch.getMaxSizeInBytes());
                }
            }
        }
        // send the last batch of remaining events
        if (eventDataBatch.getCount() > 0) {
            producer.send(eventDataBatch);
        }
        producer.close();
    }
```

Erstellen Sie das Programm, und vergewissern Sie sich, dass keine Fehler vorhanden sind. Dieses Programm wird nach dem Empfängerprogramm ausgeführt. 

## <a name="receive-events"></a>Empfangen von Ereignissen
Der Code in diesem Tutorial basiert auf dem [EventProcessorClient-Beispiel auf GitHub](https://github.com/Azure/azure-sdk-for-java/blob/master/sdk/eventhubs/azure-messaging-eventhubs-checkpointstore-blob/src/samples/java/com/azure/messaging/eventhubs/checkpointstore/blob/EventProcessorBlobCheckpointStoreSample.java). Dort können Sie sich die vollständige funktionierende Anwendung ansehen.

> [!WARNING]
> Wenn Sie diesen Code in Azure Stack Hub ausführen, treten Laufzeitfehler auf, es sei denn, Sie verwenden eine bestimmte Storage-API-Version als Ziel. Dies liegt daran, dass das Event Hubs SDK die neueste verfügbare Azure Storage API verwendet, die in Azure verfügbar ist und auf Ihrer Azure Stack Hub-Plattform möglicherweise nicht verfügbar ist. Azure Stack Hub unterstützt möglicherweise eine andere Storage Blob SDK-Version als die üblicherweise in Azure verfügbaren SDKs. Wenn Sie Azure Blob Storage als Prüfpunktspeicher verwenden, überprüfen Sie die [unterstützte Azure Storage-API-Version für Ihren Azure Stack Hub-Build](/azure-stack/user/azure-stack-acs-differences?#api-version), und verwenden Sie diese Version im Code als Ziel. 
>
> Wenn Sie z. B. die Azure Stack Hub-Version 2005 verwenden, ist die Version 2019-02-02 die höchste verfügbare Version für den Storage-Dienst. Standardmäßig verwendet die Clientbibliothek des Event Hubs SDK die höchste verfügbare Version in Azure (2019-07-07 zum Zeitpunkt der Veröffentlichung des SDK). In diesem Fall müssen Sie neben den folgenden Schritten in diesem Abschnitt auch Code für die API-Version 2019-02-02 des Storage-Diensts hinzufügen. Ein Beispiel für die Verwendung einer bestimmten Storage-API-Version als Ziel finden Sie in [diesem Beispiel auf GitHub](https://github.com/Azure/azure-sdk-for-java/blob/master/sdk/eventhubs/azure-messaging-eventhubs-checkpointstore-blob/src/samples/java/com/azure/messaging/eventhubs/checkpointstore/blob/EventProcessorWithCustomStorageVersion.java). 


### <a name="create-an-azure-storage-and-a-blob-container"></a>Erstellen eines Azure Storage-Kontos und eines Blobcontainers
In dieser Schnellstartanleitung wird Azure Storage (insbesondere Blob Storage) als Prüfpunktspeicher verwendet. Das Festlegen von Prüfpunkten ist ein Vorgang, durch den ein Ereignisprozessor die Position des letzten erfolgreich verarbeiteten Ereignisses innerhalb einer Partition markiert oder committet. Das Markieren eines Prüfpunkts erfolgt in der Regel innerhalb der Funktion, die die Ereignisse verarbeitet. Weitere Informationen zu Prüfpunkten finden Sie unter [Ereignisprozessor](event-processor-balance-partition-load.md).

Führen Sie die folgenden Schritte aus, um ein Azure Storage-Konto zu erstellen: 

1. [Erstellen Sie ein Azure Storage-Konto.](../storage/common/storage-account-create.md?tabs=azure-portal)
2. [Erstellen eines Blobcontainers](../storage/blobs/storage-quickstart-blobs-portal.md#create-a-container)
3. [Abrufen der Verbindungszeichenfolge für das Speicherkonto](../storage/common/storage-configure-connection-string.md)

    Notieren Sie sich die **Verbindungszeichenfolge** und den **Containernamen**. Diese Angaben werden im Empfangscode verwendet. 

### <a name="add-event-hubs-libraries-to-your-java-project"></a>Hinzufügen Event Hubs-Bibliotheken zu Ihrem Java-Projekt
Fügen Sie in der Datei „pom.xml“ die folgenden Abhängigkeiten ein. 

- [azure-messaging-eventhubs](https://search.maven.org/search?q=a:azure-messaging-eventhubs)
- [azure-messaging-eventhubs-checkpointstore-blob](https://search.maven.org/search?q=a:azure-messaging-eventhubs-checkpointstore-blob)

```xml
<dependencies>
    <dependency>
        <groupId>com.azure</groupId>
        <artifactId>azure-messaging-eventhubs</artifactId>
        <version>5.6.0</version>
    </dependency>
    <dependency>
        <groupId>com.azure</groupId>
        <artifactId>azure-messaging-eventhubs-checkpointstore-blob</artifactId>
        <version>1.5.1</version>
    </dependency>
</dependencies>
```

1. Fügen Sie die folgenden **import**-Anweisungen am Anfang der Java-Datei ein. 

    ```java
    import com.azure.messaging.eventhubs.EventHubClientBuilder;
    import com.azure.messaging.eventhubs.EventProcessorClient;
    import com.azure.messaging.eventhubs.EventProcessorClientBuilder;
    import com.azure.messaging.eventhubs.checkpointstore.blob.BlobCheckpointStore;
    import com.azure.messaging.eventhubs.models.ErrorContext;
    import com.azure.messaging.eventhubs.models.EventContext;
    import com.azure.storage.blob.BlobContainerAsyncClient;
    import com.azure.storage.blob.BlobContainerClientBuilder;
    import java.util.function.Consumer;
    ```
2. Erstellen Sie eine Klasse namens `Receiver`, und fügen Sie die den Zeichenfolgenvariablen der Klasse hinzu. Ersetzen Sie die Platzhalter durch die richtigen Werte. 

    > [!IMPORTANT]
    > Ersetzen Sie die Platzhalter durch die richtigen Werte.
    > - `<Event Hubs namespace connection string>` mit der Verbindungszeichenfolge für Ihren Event Hubs-Namespace. Aktualisieren 
    > - `<Event hub name>` mit dem Namen Ihres Event Hubs im Namespace. 
    > - `<Storage connection string>` durch die Verbindungszeichenfolge für Ihr Azure-Speicherkonto. 
    > - `<Storage container name>` mit dem Namen Ihres Containers in Azure Blob Storage. 

    ```java
    private static final String connectionString = "<Event Hubs namespace connection string>";
    private static final String eventHubName = "<Event hub name>";
    private static final String storageConnectionString = "<Storage connection string>";
    private static final String storageContainerName = "<Storage container name>";
    ```
1. Fügen Sie der Klasse die folgende `main`-Methode hinzu. 

    ```java
    public static void main(String[] args) throws Exception {
        // Create a blob container client that you use later to build an event processor client to receive and process events
        BlobContainerAsyncClient blobContainerAsyncClient = new BlobContainerClientBuilder()
            .connectionString(storageConnectionString) 
            .containerName(storageContainerName) 
            .buildAsyncClient();
    
        // Create a builder object that you will use later to build an event processor client to receive and process events and errors.
        EventProcessorClientBuilder eventProcessorClientBuilder = new EventProcessorClientBuilder()
            .connectionString(connectionString, eventHubName) 
            .consumerGroup(EventHubClientBuilder.DEFAULT_CONSUMER_GROUP_NAME)
            .processEvent(PARTITION_PROCESSOR) 
            .processError(ERROR_HANDLER) 
            .checkpointStore(new BlobCheckpointStore(blobContainerAsyncClient)); 

        // Use the builder object to create an event processor client 
        EventProcessorClient eventProcessorClient = eventProcessorClientBuilder.buildEventProcessorClient();

        System.out.println("Starting event processor");
        eventProcessorClient.start();

        System.out.println("Press enter to stop.");
        System.in.read();

        System.out.println("Stopping event processor");
        eventProcessorClient.stop();
        System.out.println("Event processor stopped.");

        System.out.println("Exiting process");
    }    
    ```
4. Fügen Sie die beiden Hilfsmethoden (`PARTITION_PROCESSOR` und `ERROR_HANDLER`) hinzu, die Ereignisse und Fehler in der `Receiver`-Klasse verarbeiten. 

    ```java
    public static final Consumer<EventContext> PARTITION_PROCESSOR = eventContext -> {
        System.out.printf("Processing event from partition %s with sequence number %d with body: %s %n", 
                eventContext.getPartitionContext().getPartitionId(), eventContext.getEventData().getSequenceNumber(), eventContext.getEventData().getBodyAsString());

        if (eventContext.getEventData().getSequenceNumber() % 10 == 0) {
            eventContext.updateCheckpoint();
        }
    };
    
    public static final Consumer<ErrorContext> ERROR_HANDLER = errorContext -> {
        System.out.printf("Error occurred in partition processor for partition %s, %s.%n",
            errorContext.getPartitionContext().getPartitionId(),
            errorContext.getThrowable());
    };    
    ```
3. Erstellen Sie das Programm, und vergewissern Sie sich, dass keine Fehler vorhanden sind. 

## <a name="run-the-applications"></a>Ausführen der Anwendungen
1. Führen Sie zuerst die Anwendung **receiver** (Empfänger) aus.
1. Führen Sie dann die Anwendung **sender** (Absender) aus. 
1. Vergewissern Sie sich im Fenster der Anwendung **receiver** (Empfänger), dass die von der Absenderanwendung veröffentlichten Ereignisse angezeigt werden.

    ```cmd
    Starting event processor
    Press enter to stop.
    Processing event from partition 0 with sequence number 331 with body: Foo 
    Processing event from partition 0 with sequence number 332 with body: Bar 
    ```
1. Drücken Sie im Fenster der Anwendung **receiver** (Empfänger) die EINGABETASTE, um die Anwendung zu beenden. 

    ```cmd
    Starting event processor
    Press enter to stop.
    Processing event from partition 0 with sequence number 331 with body: Foo 
    Processing event from partition 0 with sequence number 332 with body: Bar 
    
    Stopping event processor
    Event processor stopped.
    Exiting process
    ```

## <a name="next-steps"></a>Nächste Schritte
Im Folgenden sind auf GitHub verfügbare Beispiele aufgeführt:

- [azure-messaging-eventhubs samples](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/eventhubs/azure-messaging-eventhubs/src/samples/java/com/azure/messaging/eventhubs)
- [azure-messaging-eventhubs-checkpointstore-blob samples](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/eventhubs/azure-messaging-eventhubs-checkpointstore-blob/src/samples/java/com/azure/messaging/eventhubs/checkpointstore/blob).  
