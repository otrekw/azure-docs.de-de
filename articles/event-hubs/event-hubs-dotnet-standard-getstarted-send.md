---
title: Senden und Empfangen von Ereignissen von Azure Event Hubs mithilfe von .NET (alt)
description: Dieser Artikel enthält eine exemplarische Vorgehensweise zum Erstellen einer .NET Core-App, die Ereignisse über das alte Paket „Microsoft.Azure.EventHubs“ an Azure Event Hubs sendet bzw. von dort empfängt.
services: event-hubs
documentationcenter: na
author: spelluru
editor: ''
ms.assetid: ''
ms.service: event-hubs
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/11/2020
ms.author: spelluru
ms.openlocfilehash: 2a3b14acad4990059a27201b7e1e6b9e93123194
ms.sourcegitcommit: 75089113827229663afed75b8364ab5212d67323
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/22/2020
ms.locfileid: "82025168"
---
# <a name="send-events-to-or-receive-events-from-azure-event-hubs-using-net-core-microsoftazureeventhubs"></a>Senden von Ereignissen an oder Empfangen von Ereignissen aus Event Hubs mithilfe von .NET Core (Microsoft.Azure.EventHubs)
In dieser Schnellstartanleitung erfahren Sie, wie Sie mithilfe der .NET Core-Bibliothek **Microsoft.Azure.EventHubs** Ereignisse an einen Event Hub senden bzw. von diesem empfangen.

> [!WARNING]
> In diesem Schnellstart wird das alte **Microsoft.Azure.EventHubs**-Paket verwendet. Eine Schnellstartanleitung, in der die neueste **Azure.Messaging.EventHubs**-Bibliothek verwendet wird, finden Sie unter [Senden und Empfangen von Ereignissen mithilfe der Bibliothek „Azure.Messaging.EventHubs“](get-started-dotnet-standard-send-v2.md). Informationen, wie sie Ihre Anwendung nicht mit der alten, sondern mit der neuen Bibliothek verwenden, finden Sie im [Handbuch für die Migration von Microsoft.Azure.EventHubs zu Azure.Messaging.EventHubs](https://github.com/Azure/azure-sdk-for-net/blob/master/sdk/eventhub/Azure.Messaging.EventHubs/MigrationGuide.md).

## <a name="prerequisites"></a>Voraussetzungen
Wenn Sie mit Azure Event Hubs noch nicht vertraut sind, lesen Sie vor dem Durcharbeiten dieser Schnellstartanleitung die Informationen unter [Übersicht über Event Hubs](event-hubs-about.md). 

Zum Durchführen dieser Schnellstartanleitung benötigen Sie Folgendes:

- **Microsoft Azure-Abonnement.** Für die Verwendung von Azure-Diensten benötigen Sie ein Abonnement. Das gilt auch für Azure Event Hubs.  Falls Sie noch nicht über ein Azure-Konto verfügen, können Sie sich für eine [kostenlose Testversion](https://azure.microsoft.com/free/) registrieren oder beim [Erstellen eines Kontos](https://azure.microsoft.com) Ihre MSDN-Abonnentenvorteile nutzen.
- [Microsoft Visual Studio 2019](https://www.visualstudio.com).
- [.NET Core Visual Studio 2015- oder 2017-Tools](https://www.microsoft.com/net/core). 
- **Erstellen Sie einen Event Hubs-Namespace und einen Event Hub**. Verwenden Sie zunächst das [Azure-Portal](https://portal.azure.com), um einen Namespace vom Typ „Event Hubs“ zu erstellen, und beschaffen Sie die Verwaltungsanmeldeinformationen, die Ihre Anwendung für die Kommunikation mit dem Event Hub benötigt. Erstellen Sie anhand der Anleitung in [diesem Artikel](event-hubs-create.md) einen Namespace und einen Event Hub. Gehen Sie dann wie im folgenden Artikel beschrieben vor, um die **Verbindungszeichenfolge für den Event Hub-Namespace** abzurufen: [Abrufen der Verbindungszeichenfolge](event-hubs-get-connection-string.md#get-connection-string-from-the-portal). Sie verwenden die Verbindungszeichenfolge im weiteren Verlauf dieser Schnellstartanleitung.

## <a name="send-events"></a>Senden von Ereignisse 
In diesem Abschnitt erfahren Sie, wie Sie eine .NET Core-Anwendung zum Senden von Ereignissen an einen Event Hub erstellen. 

> [!NOTE]
> Sie können diese Schnellstartvorlage als Beispiel aus [GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleSender) herunterladen, die Zeichenfolgen `EventHubConnectionString` und `EventHubName` durch die Werte für Ihren Event Hub ersetzen und das Beispiel dann ausführen. Alternativ können Sie auch die Schritte in dieser Schnellstartanleitung ausführen, um eine eigene Vorlage zu erstellen.


### <a name="create-a-console-application"></a>Erstellen einer Konsolenanwendung

Starten Sie Visual Studio. Klicken Sie im Menü **Datei** auf **Neu** und dann auf **Projekt**. Erstellen Sie eine .NET Core-Konsolenanwendung.

![Neues Projekt](./media/event-hubs-dotnet-standard-getstarted-send/netcoresnd.png)

### <a name="add-the-event-hubs-nuget-package"></a>Hinzufügen des Event Hubs-NuGet-Pakets

Führen Sie die folgenden Schritte aus, um Ihrem Projekt das NuGet-Paket der [`Microsoft.Azure.EventHubs`](https://www.nuget.org/packages/Microsoft.Azure.EventHubs/) .NET Core-Bibliothek hinzuzufügen: 

1. Klicken Sie mit der rechten Maustaste auf das neu erstellte Projekt, und wählen Sie **NuGet-Pakete verwalten** aus.
2. Klicken Sie auf die Registerkarte **Durchsuchen**, und suchen Sie nach „Microsoft.Azure.EventHubs“. Wählen Sie anschließend das Paket **Microsoft.Azure.EventHubs** aus. Klicken Sie auf **Installieren**, um die Installation abzuschließen. Schließen Sie danach dieses Dialogfeld.

### <a name="write-code-to-send-messages-to-the-event-hub"></a>Schreiben von Code zum Senden von Nachrichten an den Event Hub

1. Fügen Sie am Anfang der Datei „Program.cs“ die folgenden `using`-Anweisungen hinzu:

    ```csharp
    using Microsoft.Azure.EventHubs;
    using System.Text;
    using System.Threading.Tasks;
    ```

2. Fügen Sie der `Program`-Klasse Konstanten für die Event Hubs-Verbindungszeichenfolge und den Event Hubs-Entitätspfad (einzelner Event Hub-Name) hinzu. Ersetzen Sie die Platzhalter in Klammern durch die entsprechenden Werte, die beim Erstellen des Event Hubs abgerufen wurden. Stellen Sie sicher, dass es sich bei `{Event Hubs connection string}` um die Verbindungszeichenfolge auf Namespaceebene und nicht um die Event Hub-Zeichenfolge handelt. 

    ```csharp
    private static EventHubClient eventHubClient;
    private const string EventHubConnectionString = "{Event Hubs connection string}";
    private const string EventHubName = "{Event Hub path/name}";
    ```

3. Fügen Sie wie folgt eine neue Methode mit dem Namen `MainAsync` in die `Program`-Klasse ein:

    ```csharp
    private static async Task MainAsync(string[] args)
    {
        // Creates an EventHubsConnectionStringBuilder object from the connection string, and sets the EntityPath.
        // Typically, the connection string should have the entity path in it, but this simple scenario
        // uses the connection string from the namespace.
        var connectionStringBuilder = new EventHubsConnectionStringBuilder(EventHubConnectionString)
        {
            EntityPath = EventHubName
        };

        eventHubClient = EventHubClient.CreateFromConnectionString(connectionStringBuilder.ToString());

        await SendMessagesToEventHub(100);

        await eventHubClient.CloseAsync();

        Console.WriteLine("Press ENTER to exit.");
        Console.ReadLine();
    }
    ```

4. Fügen Sie wie folgt eine neue Methode mit dem Namen `SendMessagesToEventHub` in die `Program`-Klasse ein:

    ```csharp
    // Uses the event hub client to send 100 messages to the event hub.
    private static async Task SendMessagesToEventHub(int numMessagesToSend)
    {
        for (var i = 0; i < numMessagesToSend; i++)
        {
            try
            {
                var message = $"Message {i}";
                Console.WriteLine($"Sending message: {message}");
                await eventHubClient.SendAsync(new EventData(Encoding.UTF8.GetBytes(message)));
            }
            catch (Exception exception)
            {
                Console.WriteLine($"{DateTime.Now} > Exception: {exception.Message}");
            }

            await Task.Delay(10);
        }

        Console.WriteLine($"{numMessagesToSend} messages sent.");
    }
    ```

5. Fügen Sie der `Main`-Methode in der `Program`-Klasse den folgenden Code hinzu:

    ```csharp
    MainAsync(args).GetAwaiter().GetResult();
    ```

   „Program.cs“ sollte nun wie folgt aussehen:

    ```csharp
    namespace SampleSender
    {
        using System;
        using System.Text;
        using System.Threading.Tasks;
        using Microsoft.Azure.EventHubs;

        public class Program
        {
            private static EventHubClient eventHubClient;
            private const string EventHubConnectionString = "{Event Hubs connection string}";
            private const string EventHubName = "{Event Hub path/name}";

            public static void Main(string[] args)
            {
                MainAsync(args).GetAwaiter().GetResult();
            }

            private static async Task MainAsync(string[] args)
            {
                // Creates an EventHubsConnectionStringBuilder object from the connection string, and sets the EntityPath.
                // Typically, the connection string should have the entity path in it, but for the sake of this simple scenario
                // we are using the connection string from the namespace.
                var connectionStringBuilder = new EventHubsConnectionStringBuilder(EventHubConnectionString)
                {
                    EntityPath = EventHubName
                };

                eventHubClient = EventHubClient.CreateFromConnectionString(connectionStringBuilder.ToString());

                await SendMessagesToEventHub(100);

                await eventHubClient.CloseAsync();

                Console.WriteLine("Press ENTER to exit.");
                Console.ReadLine();
            }

            // Uses the event hub client to send 100 messages to the event hub.
            private static async Task SendMessagesToEventHub(int numMessagesToSend)
            {
                for (var i = 0; i < numMessagesToSend; i++)
                {
                    try
                    {
                        var message = $"Message {i}";
                        Console.WriteLine($"Sending message: {message}");
                        await eventHubClient.SendAsync(new EventData(Encoding.UTF8.GetBytes(message)));
                    }
                    catch (Exception exception)
                    {
                        Console.WriteLine($"{DateTime.Now} > Exception: {exception.Message}");
                    }

                    await Task.Delay(10);
                }

                Console.WriteLine($"{numMessagesToSend} messages sent.");
            }
        }
    }
    ```

6. Führen Sie das Programm aus, und stellen Sie sicher, dass keine Fehler auftreten.

## <a name="receive-events"></a>Empfangen von Ereignissen
In diesem Abschnitt erfahren Sie, wie Sie eine .NET Core-Konsolenanwendung schreiben, die unter Verwendung des [Ereignisprozessorhosts](event-hubs-event-processor-host.md) Nachrichten von einem Event Hub empfängt. Der [Ereignisprozessorhost](event-hubs-event-processor-host.md) ist eine .NET-Klasse, die das Empfangen von Ereignissen von Event Hubs durch die Verwaltung von permanenten Prüfpunkten und parallelen Empfangsvorgängen von diesen Event Hubs vereinfacht. Mit dem Ereignisprozessorhost können Sie Ereignisse selbst dann auf mehrere Empfänger aufteilen, wenn sie in verschiedenen Knoten gehostet werden. Dieses Beispiel zeigt, wie der Ereignisprozessorhost für einen einzelnen Empfänger verwendet wird.
> [!NOTE]
> Sie können diese Schnellstartvorlage als Beispiel aus [GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleEphReceiver) herunterladen, die Zeichenfolgen `EventHubConnectionString`, `EventHubName`, `StorageAccountName`, `StorageAccountKey` und `StorageContainerName` durch die Werte für Ihren Event Hub ersetzen, und das Beispiel dann ausführen. Alternativ können Sie auch die Schritte in diesem Tutorial ausführen, um eine eigene Vorlage zu erstellen.

[!INCLUDE [event-hubs-create-storage](../../includes/event-hubs-create-storage.md)]

### <a name="create-a-console-application"></a>Erstellen einer Konsolenanwendung

Starten Sie Visual Studio. Klicken Sie im Menü **Datei** auf **Neu** und dann auf **Projekt**. Erstellen Sie eine .NET Core-Konsolenanwendung.

![Neues Projekt](./media/event-hubs-dotnet-standard-getstarted-receive-eph/netcorercv.png)

### <a name="add-the-event-hubs-nuget-package"></a>Hinzufügen des Event Hubs-NuGet-Pakets

Führen Sie die folgenden Schritte aus, um Ihrem Projekt das NuGet-Paket der .NET Standard-Bibliotheken [**Microsoft.Azure.EventHubs**](https://www.nuget.org/packages/Microsoft.Azure.EventHubs/) und [**Microsoft.Azure.EventHubs.Processor**](https://www.nuget.org/packages/Microsoft.Azure.EventHubs.Processor/) hinzuzufügen: 

1. Klicken Sie mit der rechten Maustaste auf das neu erstellte Projekt, und wählen Sie **NuGet-Pakete verwalten** aus.
2. Klicken Sie auf die Registerkarte **Durchsuchen**, und suchen Sie nach **Microsoft.Azure.EventHubs**. Wählen Sie anschließend das Paket **Microsoft.Azure.EventHubs** aus. Klicken Sie auf **Installieren**, um die Installation abzuschließen. Schließen Sie danach dieses Dialogfeld.
3. Wiederholen Sie die Schritte 1 und 2, und installieren Sie das Paket **Microsoft.Azure.EventHubs.Processor**.

### <a name="implement-the-ieventprocessor-interface"></a>Implementieren der IEventProcessor-Schnittstelle

1. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt, und klicken Sie auf **Hinzufügen** und anschließend auf **Klasse**. Nennen Sie die neue Klasse **SimpleEventProcessor**.

2. Öffnen Sie die Datei „SimpleEventProcessor.cs“, und fügen Sie folgende `using`-Anweisung am Anfang der Datei ein.

    ```csharp
    using Microsoft.Azure.EventHubs;
    using Microsoft.Azure.EventHubs.Processor;
    using System.Threading.Tasks;
    ```

3. Implementieren Sie die `IEventProcessor`-Schnittstelle. Ersetzen Sie den gesamten Inhalt der Klasse `SimpleEventProcessor` durch folgenden Code:

    ```csharp
    public class SimpleEventProcessor : IEventProcessor
    {
        public Task CloseAsync(PartitionContext context, CloseReason reason)
        {
            Console.WriteLine($"Processor Shutting Down. Partition '{context.PartitionId}', Reason: '{reason}'.");
            return Task.CompletedTask;
        }

        public Task OpenAsync(PartitionContext context)
        {
            Console.WriteLine($"SimpleEventProcessor initialized. Partition: '{context.PartitionId}'");
            return Task.CompletedTask;
        }

        public Task ProcessErrorAsync(PartitionContext context, Exception error)
        {
            Console.WriteLine($"Error on Partition: {context.PartitionId}, Error: {error.Message}");
            return Task.CompletedTask;
        }

        public Task ProcessEventsAsync(PartitionContext context, IEnumerable<EventData> messages)
        {
            foreach (var eventData in messages)
            {
                var data = Encoding.UTF8.GetString(eventData.Body.Array, eventData.Body.Offset, eventData.Body.Count);
                Console.WriteLine($"Message received. Partition: '{context.PartitionId}', Data: '{data}'");
            }

            return context.CheckpointAsync();
        }
    }
    ```

### <a name="update-the-main-method-to-use-simpleeventprocessor"></a>Aktualisieren der Main-Methode zur Verwendung von „SimpleEventProcessor“

1. Fügen Sie am Anfang der Datei „Program.cs“ die folgenden `using`-Anweisungen hinzu:

    ```csharp
    using Microsoft.Azure.EventHubs;
    using Microsoft.Azure.EventHubs.Processor;
    using System.Threading.Tasks;
    ```

2. Fügen Sie der `Program`-Klasse Konstanten für die Event Hub-Verbindungszeichenfolge, den Event Hub-Namen, den Speicherkonto-Containernamen, den Speicherkontonamen und den Speicherkontoschlüssel hinzu. Fügen Sie folgenden Code ein, sodass die Platzhalter durch die entsprechenden Werte ersetzt werden:

    ```csharp
    private const string EventHubConnectionString = "{Event Hubs connection string}";
    private const string EventHubName = "{Event Hub path/name}";
    private const string StorageContainerName = "{Storage account container name}";
    private const string StorageAccountName = "{Storage account name}";
    private const string StorageAccountKey = "{Storage account key}";

    private static readonly string StorageConnectionString = string.Format("DefaultEndpointsProtocol=https;AccountName={0};AccountKey={1}", StorageAccountName, StorageAccountKey);
    ```   

3. Fügen Sie wie folgt eine neue Methode mit dem Namen `MainAsync` in die `Program`-Klasse ein:

    ```csharp
    private static async Task MainAsync(string[] args)
    {
        Console.WriteLine("Registering EventProcessor...");

        var eventProcessorHost = new EventProcessorHost(
            EventHubName,
            PartitionReceiver.DefaultConsumerGroupName,
            EventHubConnectionString,
            StorageConnectionString,
            StorageContainerName);

        // Registers the Event Processor Host and starts receiving messages
        await eventProcessorHost.RegisterEventProcessorAsync<SimpleEventProcessor>();

        Console.WriteLine("Receiving. Press ENTER to stop worker.");
        Console.ReadLine();

        // Disposes of the Event Processor Host
        await eventProcessorHost.UnregisterEventProcessorAsync();
    }
    ```

3. Fügen Sie der `Main`-Methode die folgende Codezeile hinzu:

    ```csharp
    MainAsync(args).GetAwaiter().GetResult();
    ```

    Die Datei „Program.cs“ sollte nun wie folgt aussehen:

    ```csharp
    namespace SampleEphReceiver
    {

        public class Program
        {
            private const string EventHubConnectionString = "{Event Hubs connection string}";
            private const string EventHubName = "{Event Hub path/name}";
            private const string StorageContainerName = "{Storage account container name}";
            private const string StorageAccountName = "{Storage account name}";
            private const string StorageAccountKey = "{Storage account key}";

            private static readonly string StorageConnectionString = string.Format("DefaultEndpointsProtocol=https;AccountName={0};AccountKey={1}", StorageAccountName, StorageAccountKey);

            public static void Main(string[] args)
            {
                MainAsync(args).GetAwaiter().GetResult();
            }

            private static async Task MainAsync(string[] args)
            {
                Console.WriteLine("Registering EventProcessor...");

                var eventProcessorHost = new EventProcessorHost(
                    EventHubName,
                    PartitionReceiver.DefaultConsumerGroupName,
                    EventHubConnectionString,
                    StorageConnectionString,
                    StorageContainerName);

                // Registers the Event Processor Host and starts receiving messages
                await eventProcessorHost.RegisterEventProcessorAsync<SimpleEventProcessor>();

                Console.WriteLine("Receiving. Press ENTER to stop worker.");
                Console.ReadLine();

                // Disposes of the Event Processor Host
                await eventProcessorHost.UnregisterEventProcessorAsync();
            }
        }
    }
    ```

4. Führen Sie das Programm aus, und stellen Sie sicher, dass keine Fehler auftreten.


## <a name="next-steps"></a>Nächste Schritte
Lesen Sie die folgenden Artikel:

- [Beispiele für rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC)](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/Rbac). 
    
    In diesen Beispielen wird die alte **Microsoft.Azure.EventHubs**-Bibliothek verwendet, aber Sie können sie problemlos so aktualisieren, dass die neueste **Azure.Messaging.EventHubs**-Bibliothek verwendet wird. Informationen dazu, wie Sie das Beispiel nicht mit der alten, sondern mit der neuen Bibliothek verwenden, finden Sie in der [Anleitung für die Migration von Microsoft.Azure.EventHubs zu Azure.Messaging.EventHubs](https://github.com/Azure/azure-sdk-for-net/blob/master/sdk/eventhub/Azure.Messaging.EventHubs/MigrationGuide.md).
- [EventProcessorHost](event-hubs-event-processor-host.md)
- [Features und Terminologie in Azure Event Hubs](event-hubs-features.md)
- [Event Hubs – häufig gestellte Fragen](event-hubs-faq.md)


