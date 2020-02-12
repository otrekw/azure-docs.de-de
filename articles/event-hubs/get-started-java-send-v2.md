---
title: Senden oder Empfangen von Ereignissen an bzw. von Azure Event Hubs unter Verwendung von Java (aktuelles Paket)
description: Dieser Artikel enthält eine exemplarische Vorgehensweise zum Erstellen einer Java-Anwendung, die unter Verwendung des aktuellen Pakets „azure-messaging-eventhubs“ Ereignisse an Azure Event Hubs sendet bzw. von dort empfängt.
services: event-hubs
author: spelluru
ms.service: event-hubs
ms.workload: core
ms.topic: quickstart
ms.date: 01/15/2020
ms.author: spelluru
ms.openlocfilehash: d9d22374868f3befd659918c532f339d49ba1642
ms.sourcegitcommit: f0f73c51441aeb04a5c21a6e3205b7f520f8b0e1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/05/2020
ms.locfileid: "77032048"
---
# <a name="use-java-to-send-events-to-or-receive-events-from-azure-event-hubs-azure-messaging-eventhubs"></a>Senden oder Empfangen von Ereignissen an bzw. von Azure Event Hubs unter Verwendung von Java (azure-messaging-eventhubs)
In dieser Schnellstartanleitung erfahren Sie, wie Sie Java-Anwendungen erstellen, die Ereignisse an Azure Event Hubs senden oder daraus empfangen. 

Azure Event Hubs ist eine Big Data-Streamingplattform und ein Ereigniserfassungsdienst, der pro Sekunde Millionen von Ereignissen empfangen und verarbeiten kann. Event Hubs kann Ereignisse, Daten oder Telemetriedaten, die von verteilter Software und verteilten Geräten erzeugt wurden, verarbeiten und speichern. An einen Event Hub gesendete Daten können transformiert und mit einem beliebigen Echtzeitanalyse-Anbieter oder Batchverarbeitungs-/Speicheradapter gespeichert werden. Eine ausführliche Übersicht über Event Hubs finden Sie unter [Was ist Azure Event Hubs?](event-hubs-about.md) und [Event Hubs-Features im Überblick](event-hubs-features.md).

> [!IMPORTANT]
> In dieser Schnellstartanleitung wird das neue Paket **azure-messaging-eventhubs** verwendet. Eine Schnellstartanleitung mit den alten Paketen **azure-eventhubs** und **azure-eventhubs-eph** finden Sie in [diesem Artikel](event-hubs-java-get-started-send.md). 


## <a name="prerequisites"></a>Voraussetzungen

Zum Durchführen dieses Tutorials benötigen Sie Folgendes:

- Ein aktives Azure-Konto. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.
- Eine Java-Entwicklungsumgebung. In diesem Lernprogramm wird [Eclipse](https://www.eclipse.org/) verwendet. Es wird mindestens Java Development Kit (JDK) mit der Version 8 benötigt. 
- **Erstellen Sie einen Event Hubs-Namespace und einen Event Hub**. Verwenden Sie zunächst das [Azure-Portal](https://portal.azure.com), um einen Namespace vom Typ „Event Hubs“ zu erstellen, und beschaffen Sie die Verwaltungsanmeldeinformationen, die Ihre Anwendung für die Kommunikation mit dem Event Hub benötigt. Erstellen Sie anhand der Anleitung in [diesem Artikel](event-hubs-create.md) einen Namespace und einen Event Hub. Gehen Sie anschließend wie im Artikel beschrieben vor, um den Wert des Zugriffsschlüssels für den Event Hub abzurufen: [Abrufen der Verbindungszeichenfolge](event-hubs-get-connection-string.md#get-connection-string-from-the-portal). Sie verwenden den Zugriffsschlüssel im Code, den Sie später in diesem Tutorial schreiben. Der Standardschlüsselname lautet: **RootManageSharedAccessKey**.

## <a name="send-events"></a>Senden von Ereignisse 
In diesem Abschnitt erfahren Sie, wie Sie eine Java-Anwendung zum Senden von Ereignissen an einen Event Hub erstellen. 

### <a name="add-reference-to-azure-event-hubs-library"></a>Hinzufügen von Verweisen auf die Azure Event Hubs-Bibliothek

Die Java-Clientbibliothek für Event Hubs steht für die Verwendung in Maven-Projekten über das [zentrale Maven-Repository](https://search.maven.org/search?q=a:azure-messaging-eventhubs) bereit. Sie können über die folgende Abhängigkeitsdeklaration in Ihrer Maven-Projektdatei auf diese Bibliothek verweisen:

```xml
<dependency>
    <groupId>com.azure</groupId>
    <artifactId>azure-messaging-eventhubs</artifactId>
    <version>5.0.1</version>
</dependency>
```

### <a name="write-code-to-send-messages-to-the-event-hub"></a>Schreiben von Code zum Senden von Nachrichten an den Event Hub

Erstellen Sie für das folgende Beispiel zuerst ein neues Maven-Projekt für eine Konsolen-/Shellanwendung in Ihrer bevorzugten Java-Entwicklungsumgebung. Fügen Sie eine Klasse namens `SimpleSend` sowie den folgenden Code zur Klasse hinzu:

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
    .buildProducer();
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
            .buildProducer();

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
Der Code in diesem Tutorial basiert auf dem [EventProcessorClient-Beispiel auf GitHub](https://github.com/Azure/azure-sdk-for-java/blob/master/sdk/eventhubs/azure-messaging-eventhubs/src/samples/java/com/azure/messaging/eventhubs/EventProcessorClientSample.java). Dort können Sie sich die vollständige funktionierende Anwendung ansehen.

### <a name="create-a-java-project"></a>Erstellen eines Java-Projekts

Die Java-Clientbibliothek für Event Hubs steht für die Verwendung in Maven-Projekten im [zentralen Maven-Repository](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs-eph%22) zur Verfügung. Sie können darauf verweisen, indem Sie in der Maven-Projektdatei die folgende Abhängigkeitsdeklaration verwenden: 

```xml
<dependencies>
    <dependency>
        <groupId>com.azure</groupId>
        <artifactId>azure-messaging-eventhubs</artifactId>
        <version>5.0.1</version>
    </dependency>
</dependencies>
```

1. Verwenden Sie den folgenden Code, um eine neue Klasse mit dem Namen `Receiver`zu erstellen. Ersetzen Sie die Platzhalter durch die Werte, die Sie beim Erstellen des Event Hubs und des Speicherkontos verwendet haben:
   
   ```java
    import com.azure.messaging.eventhubs.*;
    import com.azure.messaging.eventhubs.models.ErrorContext;
    import com.azure.messaging.eventhubs.models.EventContext;
    import java.util.concurrent.TimeUnit;
    import java.util.function.Consumer;

    public class Receiver {

        private static final String connectionString = "EVENT HUBS NAMESPACE CONNECTION STRING";
        private static final String eventHubName = "EVENT HUB NAME";
    
        public static void main(String[] args) throws Exception {

            // function to process events
            Consumer<EventContext> processEvent = eventContext  -> {
                System.out.print("Received event: ");
                // print the body of the event
                System.out.println(eventContext.getEventData().getBodyAsString());
                eventContext.updateCheckpoint();
            };

            // function to process errors
            Consumer<ErrorContext> processError = errorContext -> {
                // print the error message
                System.out.println(errorContext.getThrowable().getMessage());
            };

            EventProcessorBuilder eventProcessorBuilder = new EventProcessorBuilder()
                .consumerGroup(EventHubClientBuilder.DEFAULT_CONSUMER_GROUP_NAME)
                .connectionString(connectionString, eventHubName)
                .processEvent(processEvent)
                .processError(processError)
                .checkpointStore(new InMemoryCheckpointStore());
        
            EventProcessorClient eventProcessorClient = eventProcessorClientBuilder.buildEventProcessorClient();
            System.out.println("Starting event processor");
            eventProcessorClient.start();

            System.out.println("Press enter to stop.");
            System.in.read();

            System.out.println("Stopping event processor");
            eventProcessor.stop();
            System.out.println("Event processor stopped.");
    
            System.out.println("Exiting process");
        }
    }
    ```
    
2. Laden Sie die Datei **InMemoryCheckpointStore.java** von [GitHub](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/eventhubs/azure-messaging-eventhubs/src/samples/java/com/azure/messaging/eventhubs) herunter, und fügen Sie sie Ihrem Projekt hinzu. 
3. Erstellen Sie das Programm, und vergewissern Sie sich, dass keine Fehler vorhanden sind. 

## <a name="run-the-applications"></a>Ausführen der Anwendungen
1. Führen Sie zuerst die Anwendung **receiver** (Empfänger) aus.
1. Führen Sie dann die Anwendung **sender** (Absender) aus. 
1. Vergewissern Sie sich im Fenster der Anwendung **receiver** (Empfänger), dass die von der Absenderanwendung veröffentlichten Ereignisse angezeigt werden.
1. Drücken Sie im Fenster der Anwendung **receiver** (Empfänger) die EINGABETASTE, um die Anwendung zu beenden. 

## <a name="next-steps"></a>Nächste Schritte
Sehen Sie sich die [Java SDK-Beispiele auf GitHub](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/eventhubs/azure-messaging-eventhubs/src/samples/java/com/azure/messaging/eventhubs) an.

