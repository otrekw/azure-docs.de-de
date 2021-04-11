---
title: Senden von Nachrichten an Azure Service Bus-Themen mit „azure-messaging-servicebus“
description: In dieser Schnellstartanleitung wird beschrieben, wie Sie Nachrichten an Azure Service Bus-Themen senden, indem Sie das Paket „azure-messaging-servicebus“ verwenden.
ms.topic: quickstart
ms.tgt_pltfrm: dotnet
ms.date: 03/16/2021
ms.custom: contperf-fy21q3
ms.openlocfilehash: 79eb7783fd3daf546539dd5b9048f4e9f484374f
ms.sourcegitcommit: 02bc06155692213ef031f049f5dcf4c418e9f509
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/03/2021
ms.locfileid: "106279798"
---
# <a name="send-messages-to-an-azure-service-bus-topic-and-receive-messages-from-subscriptions-to-the-topic-net"></a>Senden von Nachrichten an ein Azure Service Bus-Thema und Empfangen von Nachrichten von Abonnements für das Thema (.NET)
In diesem Tutorial erstellen Sie eine C#-Anwendung für die folgenden Aufgaben:

1. Senden von Nachricht an ein Service Bus-Thema. 

    Ein Service Bus-Thema stellt einen Endpunkt für Senderanwendungen zum Senden von Nachrichten bereit. Ein Thema kann über ein oder mehrere Abonnements verfügen. Jedes Abonnement eines Themas erhält eine Kopie der Nachricht, die an das Thema gesendet wird. Weitere Informationen zu Service Bus-Themen finden Sie unter [Was ist Azure Service Bus?](service-bus-messaging-overview.md). 
1. Empfangen von Nachrichten aus einem Abonnement des Themas 

    :::image type="content" source="./media/service-bus-messaging-overview/about-service-bus-topic.png" alt-text="Service Bus-Themen und -Abonnements":::

    > [!Important]
    > In dieser Schnellstartanleitung wird das neue Paket **Azure.Messaging.ServiceBus** verwendet. Wenn Sie das alte Paket „Microsoft.Azure.ServiceBus“ verwenden, lesen Sie die Informationen unter [Erste Schritte mit Service Bus-Themen](service-bus-dotnet-how-to-use-topics-subscriptions-legacy.md).

## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure-Abonnement. Um dieses Tutorial abzuschließen, benötigen Sie ein Azure-Konto. Sie können [Ihre Visual Studio-oder MSDN-Abonnentenvorteile aktivieren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A85619ABF) oder [sich für ein kostenloses Konto anmelden](https://azure.microsoft.com/free/?WT.mc_id=A85619ABF).
- Befolgen Sie die Schritte in diesem [Schnellstart](service-bus-quickstart-topics-subscriptions-portal.md), um ein Service Bus-Thema und Abonnements für das Thema zu erstellen. 

    > [!NOTE]
    > Sie verwenden die Verbindungszeichenfolge für den Namespace, den Themennamen und den Namen eines der Abonnements für das Thema in diesem Tutorial.  
- [Visual Studio 2019](https://www.visualstudio.com/vs). 
 
## <a name="send-messages-to-a-topic"></a>Senden von Nachrichten an ein Thema
In diesem Abschnitt erstellen Sie eine .NET Core-Konsolenanwendung in Visual Studio und fügen Code zum Senden von Nachrichten an das von Ihnen erstellte Service Bus-Thema hinzu. 

### <a name="create-a-console-application"></a>Erstellen einer Konsolenanwendung
Erstellen Sie mithilfe von Visual Studio eine .NET Core-Konsolenanwendung. 

1. Starten Sie Visual Studio.  
1. Wählen Sie auf der Seite **Erste Schritte** die Option **Neues Projekt erstellen** aus. 
1. Führen Sie auf der Seite **Neues Projekt erstellen** die folgenden Schritte aus: 
    1. Wählen Sie die Programmiersprache **C#** aus. 
    1. Wählen Sie den Projekttyp **Konsole** aus. 
    1. Wählen Sie in der Liste der Vorlagen **Konsolen-App (.NET Core)** aus. 
    1. Klicken Sie anschließend auf **Weiter**. 
    
        :::image type="content" source="./media/service-bus-dotnet-how-to-use-topics-subscriptions/create-console-project.png" alt-text="Erstellen eines Konsolen-App-Projekts":::
1. Gehen Sie auf der Seite **Neues Projekt konfigurieren** wie folgt vor: 
    1. Geben Sie unter **Projektname** einen Namen für das Projekt ein. 
    1. Wählen Sie unter **Speicherort** einen Speicherort für die Projekt- und Projektmappendateien aus. 
    1. Geben Sie unter **Projektmappenname** einen Namen für die Projektmappe ein. Eine Visual Studio-Projektmappe kann mehrere Projekte enthalten. In dieser Schnellstartanleitung enthält die Projektmappe nur ein Projekt. 
    1. Wählen Sie **Create** (Erstellen), um das Projekt zu erstellen. 
            
        :::image type="content" source="./media/service-bus-dotnet-how-to-use-topics-subscriptions/create-console-project-2.png" alt-text="Eingeben des Namens und des Speicherorts für das Projekt und die Projektmappe":::    


### <a name="add-the-service-bus-nuget-package"></a>Hinzufügen des NuGet-Pakets "Service Bus"
1. Klicken Sie mit der rechten Maustaste auf das neu erstellte Projekt, und wählen Sie **NuGet-Pakete verwalten** aus.

    :::image type="content" source="./media/service-bus-dotnet-how-to-use-topics-subscriptions/manage-nuget-packages-menu.png" alt-text="Menü „NuGet-Pakete verwalten“":::        
1. Wechseln Sie zur Registerkarte **Durchsuchen**.
1. Suchen Sie nach dem Paket **[Azure.Messaging.ServiceBus](https://www.nuget.org/packages/Azure.Messaging.ServiceBus/)** , und wählen Sie es aus.
1. Klicken Sie auf **Installieren**, um die Installation abzuschließen.

    :::image type="content" source="./media/service-bus-dotnet-how-to-use-topics-subscriptions/select-service-bus-package.png" alt-text="Auswählen des Service Bus-NuGet-Pakets":::
5. Wählen Sie im angezeigten Dialogfeld **Vorschau der Änderungen** die Option **OK** aus, um fortzufahren. 
1. Wenn die Seite **Zustimmung zur Lizenz** angezeigt wird, wählen Sie **Ich stimme zu** aus, um den Vorgang fortzusetzen. 
    

### <a name="add-code-to-send-messages-to-the-topic"></a>Hinzufügen von Code zum Senden von Nachrichten an das Thema 

1. Doppelklicken Sie im **Projektmappen-Explorer** auf **Program.cs**, um die Datei im Editor zu öffnen. 
1. Fügen Sie am Beginn der Namespacedefinition die folgenden `using`-Anweisungen vor der Klassendeklaration hinzu:
   
    ```csharp
    using System;
    using System.Collections.Generic;
    using System.Threading.Tasks;
    using Azure.Messaging.ServiceBus;
    ```
1. Deklarieren Sie in der Klasse `Program` oberhalb der Funktion `Main` die folgenden Variablen:

    ```csharp
        static string connectionString = "<NAMESPACE CONNECTION STRING>";
        static string topicName = "<SERVICE BUS TOPIC NAME>";
        static string subscriptionName = "<SERVICE BUS - TOPIC SUBSCRIPTION NAME>";
    ```

    Ersetzen Sie die folgenden Werte:
    - `<NAMESPACE CONNECTION STRING>` durch die Verbindungszeichenfolge für Ihren Service Bus-Namespace
    - `<TOPIC NAME>` durch den Namen des Themas
    - `<SUBSCRIPTION NAME>` durch den Namen des Abonnements

### <a name="send-a-single-message-to-the-topic"></a>Senden einer einzelnen Nachricht an das Thema
Fügen Sie der Klasse `Program` ober- oder unterhalb der Methode `Main` eine Methode namens `SendMessageToTopicAsync` hinzu. Diese Methode sendet eine einzelne Nachricht an das Thema.

```csharp
    static async Task SendMessageToTopicAsync()
    {
        // create a Service Bus client 
        await using (ServiceBusClient client = new ServiceBusClient(connectionString))
        {
            // create a sender for the topic
            ServiceBusSender sender = client.CreateSender(topicName);
            await sender.SendMessageAsync(new ServiceBusMessage("Hello, World!"));
            Console.WriteLine($"Sent a single message to the topic: {topicName}");
        }
    }
```

Diese Methode führt die folgenden Schritte aus: 
1. Erstellen eines [ServiceBusClient](/dotnet/api/azure.messaging.servicebus.servicebusclient)-Objekts unter Verwendung der Verbindungszeichenfolge für den Namespace 
1. Verwenden des `ServiceBusClient`-Objekts zum Erstellen eines [ServiceBusSender](/dotnet/api/azure.messaging.servicebus.servicebussender)-Objekts für das angegebene Service Bus-Thema. In diesem Schritt wird die Methode [ServiceBusClient.CreateSender](/dotnet/api/azure.messaging.servicebus.servicebusclient.createsender) verwendet.
1. Anschließend sendet die Methode mithilfe der Methode [ServiceBusSender.SendMessageAsync](/dotnet/api/azure.messaging.servicebus.servicebussender.sendmessageasync) eine einzelne Testnachricht an das Service Bus-Thema. 

### <a name="send-a-batch-of-messages-to-the-topic"></a>Senden eines Nachrichtenbatches an das Thema
1. Fügen Sie eine Methode namens `CreateMessages` hinzu, um eine Warteschlange (.NET-Warteschlange, keine Service Bus-Warteschlange) mit Nachrichten für die Klasse `Program` zu erstellen. Normalerweise erhalten Sie diese Nachrichten von den verschiedenen Teilen Ihrer Anwendung. Hier erstellen wir eine Warteschlange mit Beispielnachrichten. Wenn Sie mit .NET-Warteschlangen nicht vertraut sind, lesen Sie die Informationen unter [Queue.Enqueue](/dotnet/api/system.collections.queue.enqueue).

    ```csharp
        static Queue<ServiceBusMessage> CreateMessages()
        {
            // create a queue containing the messages and return it to the caller
            Queue<ServiceBusMessage> messages = new Queue<ServiceBusMessage>();
            messages.Enqueue(new ServiceBusMessage("First message"));
            messages.Enqueue(new ServiceBusMessage("Second message"));
            messages.Enqueue(new ServiceBusMessage("Third message"));
            return messages;
        }
    ```
1. Fügen Sie der `Program`-Klasse eine Methode mit dem Namen `SendMessageBatchAsync` und den folgenden Code hinzu. Bei dieser Methode wird eine Warteschlange mit Nachrichten verwendet, und es werden ein oder mehrere Batches für das Senden an das Service Bus-Thema vorbereitet. 

    ```csharp
        static async Task SendMessageBatchToTopicAsync()
        {
            // create a Service Bus client 
            await using (ServiceBusClient client = new ServiceBusClient(connectionString))
            {

                // create a sender for the topic 
                ServiceBusSender sender = client.CreateSender(topicName);

                // get the messages to be sent to the Service Bus topic
                Queue<ServiceBusMessage> messages = CreateMessages();

                // total number of messages to be sent to the Service Bus topic
                int messageCount = messages.Count;

                // while all messages are not sent to the Service Bus topic
                while (messages.Count > 0)
                {
                    // start a new batch 
                    using ServiceBusMessageBatch messageBatch = await sender.CreateMessageBatchAsync();

                    // add the first message to the batch
                    if (messageBatch.TryAddMessage(messages.Peek()))
                    {
                        // dequeue the message from the .NET queue once the message is added to the batch
                        messages.Dequeue();
                    }
                    else
                    {
                        // if the first message can't fit, then it is too large for the batch
                        throw new Exception($"Message {messageCount - messages.Count} is too large and cannot be sent.");
                    }

                    // add as many messages as possible to the current batch
                    while (messages.Count > 0 && messageBatch.TryAddMessage(messages.Peek()))
                    {
                        // dequeue the message from the .NET queue as it has been added to the batch
                        messages.Dequeue();
                    }

                    // now, send the batch
                    await sender.SendMessagesAsync(messageBatch);

                    // if there are any remaining messages in the .NET queue, the while loop repeats 
                }

                Console.WriteLine($"Sent a batch of {messageCount} messages to the topic: {topicName}");
            }
        }
    ```    

    Hier sind die wesentlichen Schritte aus dem Code aufgeführt:
    1. Erstellen eines [ServiceBusClient](/dotnet/api/azure.messaging.servicebus.servicebusclient)-Objekts unter Verwendung der Verbindungszeichenfolge für den Namespace 
    1. Aufrufen der Methode [CreateSender](/dotnet/api/azure.messaging.servicebus.servicebusclient.createsender) für das `ServiceBusClient`-Objekt, um ein [ServiceBusSender](/dotnet/api/azure.messaging.servicebus.servicebussender)-Objekt für das angegebene Service Bus-Thema zu erstellen 
    1. Aufrufen der Hilfsmethode `GetMessages` zum Abrufen einer Warteschlange mit Nachrichten, die an das Service Bus-Thema gesendet werden sollen 
    1. Erstellen von [ServiceBusMessageBatch](/dotnet/api/azure.messaging.servicebus.servicebusmessagebatch) mithilfe von [ServiceBusSender.CreateMessageBatchAsync](/dotnet/api/azure.messaging.servicebus.servicebussender.createmessagebatchasync)
    1. Hinzufügen von Nachrichten zum Batch mithilfe von [ServiceBusMessageBatch.TryAddMessage](/dotnet/api/azure.messaging.servicebus.servicebusmessagebatch.tryaddmessage). Beim Hinzufügen der Nachrichten zum Batch werden sie aus der .NET-Warteschlange entfernt. 
    1. Senden des Nachrichtenbatches an das Service Bus-Thema mithilfe der Methode [ServiceBusSender.SendMessagesAsync](/dotnet/api/azure.messaging.servicebus.servicebussender.sendmessagesasync)

### <a name="update-the-main-method"></a>Aktualisieren der Main-Methode
Ersetzen Sie die `Main()`-Methode durch die folgende **asynchrone** `Main`-Methode. Hiermit werden die beiden Sendemethoden aufgerufen, um eine einzelne Nachricht und einen Batch mit Nachrichten an das Thema zu senden.  

```csharp
    static async Task Main()
    {
        // send a single message to the topic
        await SendMessageToTopicAsync();

        // send a batch of messages to the topic
        await SendMessageBatchToTopicAsync();
    }
```

### <a name="test-the-app-to-send-messages-to-the-topic"></a>Testen der App zum Senden von Nachrichten an das Thema
1. Führen Sie die Anwendung aus. Die folgende Ausgabe wird angezeigt.

    ```console
    Sent a single message to the topic: mytopic
    Sent a batch of 3 messages to the topic: mytopic
    ```
1. Gehen Sie im Azure-Portal wie folgt vor:
    1. Navigieren Sie zu Ihrem Service Bus-Namespace. 
    1. Wechseln Sie auf der Seite **Übersicht** unten im mittleren Bereich zur Registerkarte **Themen**, und wählen Sie das Service Bus-Thema aus. Im folgenden Beispiel ist dies `mytopic`.
    
        :::image type="content" source="./media/service-bus-dotnet-how-to-use-topics-subscriptions/select-topic.png" alt-text="Auswählen des Themas":::
    1. Auf der Seite **Service Bus-Thema** unten im Abschnitt **Metriken** können Sie im Diagramm **Nachrichten** sehen, dass für das Thema vier eingehende Nachrichten vorhanden sind. Wenn der Wert nicht angezeigt wird, sollten Sie einige Minuten warten und die Seite dann aktualisieren, damit das aktualisierte Diagramm angezeigt wird. 

        :::image type="content" source="./media/service-bus-dotnet-how-to-use-topics-subscriptions/sent-messages-essentials.png" alt-text="An das Thema gesendete Nachrichten" lightbox="./media/service-bus-dotnet-how-to-use-topics-subscriptions/sent-messages-essentials.png":::
    4. Wählen Sie im unteren Bereich das Abonnement aus. Im folgenden Beispiel ist dies **S1**. Auf der Seite **Service Bus-Abonnement** wird für **Anzahl aktiver Nachrichten** der Wert **4** angezeigt. Das Abonnement hat die vier Nachrichten empfangen, die Sie an das Thema gesendet haben, aber sie wurden noch nicht von einem Empfänger ausgewählt. 
    
        :::image type="content" source="./media/service-bus-dotnet-how-to-use-topics-subscriptions/subscription-page.png" alt-text="Vom Abonnement empfangene Nachrichten" lightbox="./media/service-bus-dotnet-how-to-use-topics-subscriptions/subscription-page.png":::
    

## <a name="receive-messages-from-a-subscription"></a>Empfangen von Nachrichten aus einem Abonnement

1. Fügen Sie der `Program`-Klasse die folgenden Methoden hinzu, mit denen Nachrichten und Fehler verarbeitet werden. 

    ```csharp
        static async Task MessageHandler(ProcessMessageEventArgs args)
        {
            string body = args.Message.Body.ToString();
            Console.WriteLine($"Received: {body} from subscription: {subscriptionName}");

            // complete the message. messages is deleted from the queue. 
            await args.CompleteMessageAsync(args.Message);
        }

        static Task ErrorHandler(ProcessErrorEventArgs args)
        {
            Console.WriteLine(args.Exception.ToString());
            return Task.CompletedTask;
        }
    ```
1. Fügen Sie der `Program`-Klasse die folgende `ReceiveMessagesFromSubscriptionAsync`-Methode hinzu.

    ```csharp
        static async Task ReceiveMessagesFromSubscriptionAsync()
        {
            await using (ServiceBusClient client = new ServiceBusClient(connectionString))
            {
                // create a processor that we can use to process the messages
                ServiceBusProcessor processor = client.CreateProcessor(topicName, subscriptionName, new ServiceBusProcessorOptions());

                // add handler to process messages
                processor.ProcessMessageAsync += MessageHandler;

                // add handler to process any errors
                processor.ProcessErrorAsync += ErrorHandler;

                // start processing 
                await processor.StartProcessingAsync();

                Console.WriteLine("Wait for a minute and then press any key to end the processing");
                Console.ReadKey();

                // stop processing 
                Console.WriteLine("\nStopping the receiver...");
                await processor.StopProcessingAsync();
                Console.WriteLine("Stopped receiving messages");
            }
        }
    ```

    Hier sind die wesentlichen Schritte aus dem Code aufgeführt:
    1. Erstellen eines [ServiceBusClient](/dotnet/api/azure.messaging.servicebus.servicebusclient)-Objekts unter Verwendung der Verbindungszeichenfolge für den Namespace 
    1. Aufrufen der Methode [CreateProcessor](/dotnet/api/azure.messaging.servicebus.servicebusclient.createprocessor) für das `ServiceBusClient`-Objekt zum Erstellen eines [ServiceBusProcessor](/dotnet/api/azure.messaging.servicebus.servicebusprocessor)-Objekts für die angegebene Kombination aus Service Bus-Thema und Abonnement 
    1. Angeben von Handlern für die Ereignisse [ProcessMessageAsync](/dotnet/api/azure.messaging.servicebus.servicebusprocessor.processmessageasync) und [ProcessErrorAsync](/dotnet/api/azure.messaging.servicebus.servicebusprocessor.processerrorasync) des `ServiceBusProcessor`-Objekts 
    1. Starten der Verarbeitung von Nachrichten durch Aufrufen von [StartProcessingAsync](/dotnet/api/azure.messaging.servicebus.servicebusprocessor.startprocessingasync) für das `ServiceBusProcessor`-Objekt 
    1. Aufrufen von [StopProcessingAsync](/dotnet/api/azure.messaging.servicebus.servicebusprocessor.stopprocessingasync) für das `ServiceBusProcessor`-Objekt, wenn der Benutzer eine Taste zum Beenden der Verarbeitung drückt 
1. Fügen Sie der `Main`-Methode einen Aufruf der `ReceiveMessagesFromSubscriptionAsync`-Methode hinzu. Kommentieren Sie die `SendMessagesToTopicAsync`-Methode aus, falls Sie nur das Empfangen von Nachrichten testen möchten. Wenn Sie dies nicht tun, sehen Sie, dass vier weitere Nachrichten an das Thema gesendet werden. 

    ```csharp
        static async Task Main()
        {
            // send a message to the topic
            await SendMessageToTopicAsync();

            // send a batch of messages to the topic
            await SendMessageBatchToTopicAsync();

            // receive messages from the subscription
            await ReceiveMessagesFromSubscriptionAsync();
        }
    ```
## <a name="run-the-app"></a>Ausführen der App
Führen Sie die Anwendung aus. Warten Sie eine Minute lang, und drücken Sie dann eine beliebige Taste, um den Empfang von Nachrichten zu beenden. Die folgende Ausgabe sollte angezeigt werden (Leertaste für Schlüssel). 

```console
Sent a single message to the topic: mytopic
Sent a batch of 3 messages to the topic: mytopic
Wait for a minute and then press any key to end the processing
Received: Hello, World! from subscription: mysub
Received: First message from subscription: mysub
Received: Second message from subscription: mysub
Received: Third message from subscription: mysub
Received: Hello, World! from subscription: mysub
Received: First message from subscription: mysub
Received: Second message from subscription: mysub
Received: Third message from subscription: mysub

Stopping the receiver...
Stopped receiving messages
```

Sehen Sie sich erneut die Informationen im Portal an. 

- Auf der Seite **Service Bus-Thema** werden im Diagramm **Nachrichten** acht ein- und acht ausgehende Nachrichten angezeigt. Falls diese Zahlen nicht angezeigt werden, sollten Sie einige Minuten warten und die Seite dann aktualisieren, damit der aktuelle Stand im Diagramm angezeigt wird. 

    :::image type="content" source="./media/service-bus-dotnet-how-to-use-topics-subscriptions/messages-size-final.png" alt-text="Gesendete und empfangene Nachrichten" lightbox="./media/service-bus-dotnet-how-to-use-topics-subscriptions/messages-size-final.png":::
- Auf der Seite **Service Bus-Abonnement** wird für **Anzahl aktiver Nachrichten** der Wert „0“ angezeigt. Dies liegt daran, dass ein Empfänger Nachrichten von diesem Abonnement empfangen und den Vorgang für die Nachrichten abgeschlossen hat. 

    :::image type="content" source="./media/service-bus-dotnet-how-to-use-topics-subscriptions/subscription-page-final.png" alt-text="„Anzahl aktiver Nachrichten“ aufseiten des Abonnements" lightbox="./media/service-bus-dotnet-how-to-use-topics-subscriptions/subscription-page-final.png":::
    


## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen finden Sie in der folgenden Dokumentation bzw. unter den folgenden Beispielen:

- [Azure Service Bus-Clientbibliothek für .NET: Infodatei](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/servicebus/Azure.Messaging.ServiceBus)
- [.NET-Beispiele für Azure Service Bus auf GitHub](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/servicebus/Azure.Messaging.ServiceBus/samples)
- [.NET-API-Referenz](/dotnet/api/azure.messaging.servicebus?preserve-view=true)