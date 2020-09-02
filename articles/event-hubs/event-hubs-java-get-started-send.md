---
title: Senden oder Empfangen von Ereignissen an bzw. von Azure Event Hubs unter Verwendung von Java (aktuelles Paket)
description: Dieser Artikel enthält eine exemplarische Vorgehensweise zum Erstellen einer Java-Anwendung, die unter Verwendung des aktuellen Pakets „azure-messaging-eventhubs“ Ereignisse an Azure Event Hubs sendet bzw. von dort empfängt.
ms.topic: quickstart
ms.date: 06/23/2020
ms.custom: devx-track-java
ms.openlocfilehash: 5ca03883cf7daa66e94fd78df9e03535fe6f51e6
ms.sourcegitcommit: 62e1884457b64fd798da8ada59dbf623ef27fe97
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/26/2020
ms.locfileid: "88934003"
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
    <version>5.0.1</version>
</dependency>
```

### <a name="write-code-to-send-messages-to-the-event-hub"></a>Schreiben von Code zum Senden von Nachrichten an den Event Hub

Erstellen Sie für das folgende Beispiel zuerst ein neues Maven-Projekt für eine Konsolen-/Shellanwendung in Ihrer bevorzugten Java-Entwicklungsumgebung. Fügen Sie eine Klasse namens `Sender` sowie den folgenden Code zur Klasse hinzu:

```java
import com.azure.messaging.eventhubs.*;
import static java.nio.charset.StandardCharsets.UTF_8;

public class Sender {
       public static void main(String[] args) {
    }
}
```

### <a name="connection-string-and-event-hub"></a>Verbindungszeichenfolge und Event Hub
Im folgenden Code werden die Verbindungszeichenfolge für den Event Hubs-Namespace und der Name des Event Hubs verwendet, um einen Event Hubs-Client zu erstellen: 

```java
String connectionString = "<CONNECTION STRING to EVENT HUBS NAMESPACE>";
String eventHubName = "<EVENT HUB NAME>";
```

### <a name="create-an-event-hubs-producer-client"></a>Erstellen eines Event Hubs-Producerclients 
Der folgende Code dient zum Erstellen eines Producerclient-Objekts, das verwendet wird, um Ereignisse zu generieren und an den Event Hub zu senden: 

```java
EventHubProducerClient producer = new EventHubClientBuilder()
    .connectionString(connectionString, eventHubName)
    .buildProducerClient();
```

### <a name="prepare-a-batch-of-events"></a>Vorbereiten eines Ereignisbatchs
Der folgende Code dient zum Vorbereiten eines Batchs mit Ereignissen: 

```java
EventDataBatch batch = producer.createBatch();
batch.tryAdd(new EventData("First event"));
batch.tryAdd(new EventData("Second event"));
batch.tryAdd(new EventData("Third event"));
batch.tryAdd(new EventData("Fourth event"));
batch.tryAdd(new EventData("Fifth event"));
```

### <a name="send-the-batch-of-events-to-the-event-hub"></a>Senden des Ereignisbatchs an den Event Hub
Durch diesen Code wird der im vorherigen Schritt vorbereitete Ereignisbatch an den Event Hub gesendet. Der folgende Code bewirkt eine Blockierung für den Sendevorgang: 

```java
producer.send(batch);
```

### <a name="close-and-cleanup"></a>Schließen und Bereinigen
Der folgende Code schließt den Producer: 

```java
producer.close();
```
### <a name="complete-code-to-send-events"></a>Vollständiger Code zum Senden von Ereignissen
Im Anschluss finden Sie den vollständigen Code für das Senden von Ereignissen an den Event Hub: 

```java
import com.azure.messaging.eventhubs.*;

public class Sender {
    public static void main(String[] args) {
        final String connectionString = "EVENT HUBS NAMESPACE CONNECTION STRING";
        final String eventHubName = "EVENT HUB NAME";

        // create a producer using the namespace connection string and event hub name
        EventHubProducerClient producer = new EventHubClientBuilder()
            .connectionString(connectionString, eventHubName)
            .buildProducerClient();

        // prepare a batch of events to send to the event hub    
        EventDataBatch batch = producer.createBatch();
        batch.tryAdd(new EventData("First event"));
        batch.tryAdd(new EventData("Second event"));
        batch.tryAdd(new EventData("Third event"));
        batch.tryAdd(new EventData("Fourth event"));
        batch.tryAdd(new EventData("Fifth event"));

        // send the batch of events to the event hub
        producer.send(batch);

        // close the producer
        producer.close();
    }
}
```

Erstellen Sie das Programm, und vergewissern Sie sich, dass keine Fehler vorhanden sind. Dieses Programm wird nach dem Empfängerprogramm ausgeführt. 

## <a name="receive-events"></a>Empfangen von Ereignissen
Der Code in diesem Tutorial basiert auf dem [EventProcessorClient-Beispiel auf GitHub](https://github.com/Azure/azure-sdk-for-java/blob/master/sdk/eventhubs/azure-messaging-eventhubs-checkpointstore-blob/src/samples/java/com/azure/messaging/eventhubs/checkpointstore/blob/EventProcessorBlobCheckpointStoreSample.java). Dort können Sie sich die vollständige funktionierende Anwendung ansehen.

> [!NOTE]
> Wenn Sie Azure Stack Hub als Plattform verwenden, wird möglicherweise eine andere Storage Blob SDK-Version unterstützt als die üblicherweise in Azure verfügbaren SDKs. Wenn Sie z. B. die [Azure Stack Hub-Version 2002](/azure-stack/user/event-hubs-overview) verwenden, ist die Version 2017-11-09 die höchste verfügbare Version für den Storage-Dienst. In diesem Fall müssen Sie neben den folgenden Schritten in diesem Abschnitt auch Code für die API-Version 2017-11-09 des Storage-Diensts hinzufügen. Ein Beispiel für die Verwendung einer bestimmten Storage-API-Version als Ziel finden Sie in [diesem Beispiel auf GitHub](https://github.com/Azure/azure-sdk-for-java/blob/master/sdk/eventhubs/azure-messaging-eventhubs-checkpointstore-blob/src/samples/java/com/azure/messaging/eventhubs/checkpointstore/blob/EventProcessorWithCustomStorageVersion.java). Weitere Informationen zu den Versionen des Azure Storage-Diensts, die für Azure Stack Hub unterstützt werden, finden Sie unter [Azure Stack Hub-Speicher: Unterschiede und Überlegungen](/azure-stack/user/azure-stack-acs-differences).

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
        <version>5.1.1</version>
    </dependency>
    <dependency>
        <groupId>com.azure</groupId>
        <artifactId>azure-messaging-eventhubs-checkpointstore-blob</artifactId>
        <version>1.1.1</version>
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
    import java.util.concurrent.TimeUnit;
    ```
2. Erstellen Sie eine Klasse namens `Receiver`, und fügen Sie die den Zeichenfolgenvariablen der Klasse hinzu. Ersetzen Sie die Platzhalter durch die richtigen Werte. 
    ```java
    private static final String EH_NAMESPACE_CONNECTION_STRING = "<EVENT HUBS NAMESPACE CONNECTION STRING>";
    private static final String eventHubName = "<EVENT HUB NAME>";
    private static final String STORAGE_CONNECTION_STRING = "<AZURE STORAGE CONNECTION STRING>";
    private static final String STORAGE_CONTAINER_NAME = "<AZURE STORAGE CONTAINER NAME>";
    ```
3. Fügen Sie der Klasse die folgende `main`-Methode hinzu. 

    ```java
    public static void main(String[] args) throws Exception {
        // Create a blob container client that you use later to build an event processor client to receive and process events
        BlobContainerAsyncClient blobContainerAsyncClient = new BlobContainerClientBuilder()
            .connectionString(STORAGE_CONNECTION_STRING) 
            .containerName(STORAGE_CONTAINER_NAME) 
            .buildAsyncClient();
    
        // Create a builder object that you will use later to build an event processor client to receive and process events and errors.
        EventProcessorClientBuilder eventProcessorClientBuilder = new EventProcessorClientBuilder()
            .connectionString(EH_NAMESPACE_CONNECTION_STRING, eventHubName) 
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
3. Der vollständige Code sollte wie folgt aussehen: 

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
    import java.util.concurrent.TimeUnit;
    
    public class Receiver {
        
        private static final String EH_NAMESPACE_CONNECTION_STRING = "<EVENT HUBS NAMESPACE CONNECTION STRING>";
        private static final String eventHubName = "<EVENT HUB NAME>";
        private static final String STORAGE_CONNECTION_STRING = "<AZURE STORAGE CONNECTION STRING>";
        private static final String STORAGE_CONTAINER_NAME = "<AZURE STORAGE CONTAINER NAME>";
    
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
        
        public static void main(String[] args) throws Exception {
            BlobContainerAsyncClient blobContainerAsyncClient = new BlobContainerClientBuilder()
                .connectionString(STORAGE_CONNECTION_STRING)
                .containerName(STORAGE_CONTAINER_NAME)
                .buildAsyncClient();
    
            EventProcessorClientBuilder eventProcessorClientBuilder = new EventProcessorClientBuilder()
                .connectionString(EH_NAMESPACE_CONNECTION_STRING, eventHubName)
                .consumerGroup(EventHubClientBuilder.DEFAULT_CONSUMER_GROUP_NAME)
                .processEvent(PARTITION_PROCESSOR)
                .processError(ERROR_HANDLER)
                .checkpointStore(new BlobCheckpointStore(blobContainerAsyncClient));
    
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
        
    }
    ```
3. Erstellen Sie das Programm, und vergewissern Sie sich, dass keine Fehler vorhanden sind. 

## <a name="run-the-applications"></a>Ausführen der Anwendungen
1. Führen Sie zuerst die Anwendung **receiver** (Empfänger) aus.
1. Führen Sie dann die Anwendung **sender** (Absender) aus. 
1. Vergewissern Sie sich im Fenster der Anwendung **receiver** (Empfänger), dass die von der Absenderanwendung veröffentlichten Ereignisse angezeigt werden.
1. Drücken Sie im Fenster der Anwendung **receiver** (Empfänger) die EINGABETASTE, um die Anwendung zu beenden. 

## <a name="next-steps"></a>Nächste Schritte
Im Folgenden sind auf GitHub verfügbare Beispiele aufgeführt:

- [azure-messaging-eventhubs samples](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/eventhubs/azure-messaging-eventhubs/src/samples/java/com/azure/messaging/eventhubs)
- [azure-messaging-eventhubs-checkpointstore-blob samples](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/eventhubs/azure-messaging-eventhubs-checkpointstore-blob/src/samples/java/com/azure/messaging/eventhubs/checkpointstore/blob).  
