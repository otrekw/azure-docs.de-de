---
title: 'Verfügbarkeit und Konsistenz: Azure Event Hubs | Microsoft-Dokumentation'
description: Hier erfahren Sie, wie Sie maximale Verfügbarkeit und Konsistenz in Azure Event Hubs mit Partitionen erzielen.
ms.topic: article
ms.date: 03/15/2021
ms.custom: devx-track-csharp
ms.openlocfilehash: e014a33e94fe7f90569dd2ef1e9b620eef274842
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "104952863"
---
# <a name="availability-and-consistency-in-event-hubs"></a>Verfügbarkeit und Konsistenz in Event Hubs
Dieser Artikel enthält Informationen zur Unterstützung der Verfügbarkeit und Konsistenz für Azure Event Hubs. 

## <a name="availability"></a>Verfügbarkeit
Mit Azure Event Hubs wird das Risiko von katastrophalen Ausfällen einzelner Computer oder sogar ganzer Racks auf Cluster verteilt, die sich über mehrere Fehlerdomänen in einem Rechenzentrum erstrecken. Außerdem werden vom Dienst transparente Fehlererkennungs- und Failovermechanismen implementiert. Daher kann er auch bei den oben erwähnten Ausfällen weiterhin innerhalb der garantierten Servicelevel und in der Regel ohne spürbare Unterbrechungen ausgeführt werden. 

Wenn ein Event Hubs-Namespace mit aktivierten [Verfügbarkeitszonen](../availability-zones/az-overview.md) erstellt wurde, wird das Ausfallrisiko noch weiter auf drei physisch getrennte Einrichtungen verteilt. Der Dienst verfügt dann über genügend Kapazitätsreserven, um den vollständigen, katastrophalen Ausfall einer gesamten Einrichtung sofort bewältigen zu können. Weitere Informationen finden Sie unter [Azure Event Hubs: Georedundante Notfallwiederherstellung](event-hubs-geo-dr.md).

Wenn eine Clientanwendung Ereignisse an einen Event Hub sendet, ohne eine Partition anzugeben, werden diese automatisch auf die Partitionen Ihres Event Hubs verteilt. Falls eine Partition nicht verfügbar sein sollte, werden die Ereignisse auf die restlichen Partitionen verteilt. Dieses Verhalten ermöglicht das größte Maß an Betriebszeit. Für Anwendungsfälle, für die eine maximale Betriebszeit Voraussetzung ist, wird dieses Modell bevorzugt, anstatt Ereignisse an eine bestimmte Partition zu senden. 

## <a name="consistency"></a>Konsistenz
In manchen Szenarios kann die Reihenfolge der Ereignisse wichtig sein. Nehmen Sie beispielsweise an, dass Ihr Back-End-System einen Updatebefehl vor einem Löschbefehl ausführen soll. Bei diesem Szenario sendet eine Clientanwendung Ereignisse an eine bestimmte Partition, damit die Reihenfolge beibehalten wird. Wenn eine Consumeranwendung diese auf der Partition vorhandenen Ereignisse nutzt, werden sie in der richtigen Reihenfolge gelesen. 

Bedenken Sie bei dieser Konfiguration, dass Sie eine Fehlerantwort erhalten, wenn die Partition, an die Sie senden, nicht verfügbar ist. Zum Vergleich: Wenn Sie keine bestimmte Partition bevorzugen, sendet der Event Hubs-Dienst das Ereignis an die nächste verfügbare Partition.

Wenn Hochverfügbarkeit besonders wichtig ist, sollten Sie daher keine bestimmte Partition (mit Partitions-ID/Schlüssel) als Ziel verwenden. Durch die Verwendung von Partitions-ID bzw. -schlüssel wird die Verfügbarkeit eines Event Hub auf Partitionsebene herabgestuft. In diesem Szenario treffen Sie explizit die Wahl zwischen Verfügbarkeit (keine Partitions-ID/kein -schlüssel) und Konsistenz (Anheften von Ereignissen an eine bestimmte Partition). Ausführliche Informationen zu Partitionen in Event Hubs finden Sie unter [Partitionen](event-hubs-features.md#partitions).

## <a name="appendix"></a>Anhang

### <a name="send-events-without-specifying-a-partition"></a>Senden von Ereignissen ohne Angabe einer Partition
Es wird empfohlen, Ereignisse an einen Event Hub zu senden, ohne Partitionsinformationen festzulegen, damit der Event Hubs-Dienst die Last über partitionsübergreifend ausgleichen kann. Lesen Sie die folgenden Schnellstarts, um zu erfahren, wie Sie diesen Schritt in unterschiedlichen Programmiersprachen ausführen. 

- [Senden von Ereignissen mithilfe von .NET](event-hubs-dotnet-standard-getstarted-send.md)
- [Senden von Ereignissen mithilfe von Java](event-hubs-java-get-started-send.md)
- [Senden von Ereignissen mithilfe von JavaScript](event-hubs-python-get-started-send.md)
- [Senden von Ereignissen mithilfe von Python](event-hubs-python-get-started-send.md)


### <a name="send-events-to-a-specific-partition"></a>Senden von Ereignissen an eine bestimmte Partition
In diesem Abschnitt erfahren Sie, wie Sie mithilfe verschiedener Programmiersprachen Ereignisse an eine bestimmte Partition senden. 

### <a name="net"></a>[.NET](#tab/dotnet)
Um Ereignisse an eine bestimmte Partition zu senden, erstellen Sie den Batch mithilfe der [EventHubProducerClient.CreateBatchAsync](/dotnet/api/azure.messaging.eventhubs.producer.eventhubproducerclient.createbatchasync#Azure_Messaging_EventHubs_Producer_EventHubProducerClient_CreateBatchAsync_Azure_Messaging_EventHubs_Producer_CreateBatchOptions_System_Threading_CancellationToken_)-Methode, indem Sie entweder `PartitionId` oder `PartitionKey` in [CreateBatchOptions](/dotnet/api/azure.messaging.eventhubs.producer.createbatchoptions) angeben. Der folgende Code sendet einen Batch von Ereignissen an eine bestimmte Partition, indem ein Partitionsschlüssel angegeben wird. Event Hubs stellt sicher, dass alle Ereignisse mit dem gleichen Partitionsschlüsselwert zusammen gespeichert und in der Empfangsreihenfolge übermittelt werden.

```csharp
var batchOptions = new CreateBatchOptions { PartitionKey = "cities" };
using var eventBatch = await producer.CreateBatchAsync(batchOptions);
```

Sie können auch die [EventHubProducerClient.SendAsync](/dotnet/api/azure.messaging.eventhubs.producer.eventhubproducerclient.sendasync#Azure_Messaging_EventHubs_Producer_EventHubProducerClient_SendAsync_System_Collections_Generic_IEnumerable_Azure_Messaging_EventHubs_EventData__Azure_Messaging_EventHubs_Producer_SendEventOptions_System_Threading_CancellationToken_)-Methode verwenden, indem Sie **PartitionId** oder **PartitionKey** in [SendEventOptions](/dotnet/api/azure.messaging.eventhubs.producer.sendeventoptions) angeben.

```csharp
var sendEventOptions  = new SendEventOptions { PartitionKey = "cities" };
// create the events array
producer.SendAsync(events, sendOptions)
```


### <a name="java"></a>[Java](#tab/java)
Um Ereignisse an eine bestimmte Partition zu senden, erstellen Sie den Batch mit der Methode [createBatch](/java/api/com.azure.messaging.eventhubs.eventhubproducerclient.createbatch), indem Sie entweder die **Partitions-ID** oder den **Partitionsschlüssel** in [createBatchOptions](/java/api/com.azure.messaging.eventhubs.models.createbatchoptions) angeben. Der folgende Code sendet einen Batch von Ereignissen an eine bestimmte Partition, indem ein Partitionsschlüssel angegeben wird. 

```java
CreateBatchOptions batchOptions = new CreateBatchOptions();
batchOptions.setPartitionKey("cities");
```

Sie können auch die Methode [EventHubProducerClient.send](/java/api/com.azure.messaging.eventhubs.eventhubproducerclient.send#com_azure_messaging_eventhubs_EventHubProducerClient_send_java_lang_Iterable_com_azure_messaging_eventhubs_EventData__com_azure_messaging_eventhubs_models_SendOptions_) verwenden, indem Sie entweder die **Partitions-ID** oder den **Partitionsschlüssel** in [SendOptions](/java/api/com.azure.messaging.eventhubs.models.sendoptions) angeben.

```java
List<EventData> events = Arrays.asList(new EventData("Melbourne"), new EventData("London"), new EventData("New York"));
SendOptions sendOptions = new SendOptions();
sendOptions.setPartitionKey("cities");
producer.send(events, sendOptions);
```


### <a name="python"></a>[Python](#tab/python) 
Um Ereignisse an eine bestimmte Partition zu senden, geben Sie beim Erstellen eines Batches mithilfe der- [`EventHubProducerClient.create_batch`](/python/api/azure-eventhub/azure.eventhub.eventhubproducerclient#create-batch---kwargs-)Methode `partition_id` oder `partition_key` an. Verwenden Sie dann die [`EventHubProducerClient.send_batch`](/python/api/azure-eventhub/azure.eventhub.aio.eventhubproducerclient#send-batch-event-data-batch--typing-union-azure-eventhub--common-eventdatabatch--typing-list-azure-eventhub-)-Methode, um den Batch an die Partition des Event Hub zu senden. 

```python
event_data_batch = await producer.create_batch(partition_key='cities')
```

Sie können auch die [EventHubProducerClient.send_batch](/python/api/azure-eventhub/azure.eventhub.eventhubproducerclient#send-batch-event-data-batch----kwargs-)-Methode verwenden, indem Sie Werte für die Parameter `partition_id` oder `partition_key` angeben.

```python
producer.send_batch(event_data_batch, partition_key="cities")
```

### <a name="javascript"></a>[JavaScript](#tab/javascript)
Um Ereignisse an eine bestimmte Partition zu senden, [erstellen Sie einen Batch](/javascript/api/@azure/event-hubs/eventhubproducerclient#createBatch_CreateBatchOptions_) mithilfe des [EventHubProducerClient.CreateBatchOptions](/javascript/api/@azure/event-hubs/eventhubproducerclient#createBatch_CreateBatchOptions_)-Objekts, indem Sie `partitionId` oder `partitionKey` angeben. Senden Sie den Batch dann mithilfe der [EventHubProducerClient.SendBatch](/javascript/api/@azure/event-hubs/eventhubproducerclient#sendBatch_EventDataBatch__OperationOptions_)-Methode an den Event Hub. 

Siehe folgendes Beispiel.

```javascript
const batchOptions = { partitionKey = "cities"; };
const batch = await producer.createBatch(batchOptions);
```

Sie können auch die Methode [EventHubProducerClient.sendBatch](/javascript/api/@azure/event-hubs/eventhubproducerclient#sendBatch_EventData____SendBatchOptions_) verwenden, indem Sie entweder die **Partitions-ID** oder den **Partitionsschlüssel** in [SendBatchOptions](/javascript/api/@azure/event-hubs/sendbatchoptions) angeben.

```javascript
const sendBatchOptions = { partitionKey = "cities"; };
// prepare events
producer.sendBatch(events, sendBatchOptions);
```

---



## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu Event Hubs finden Sie unter den folgenden Links:

- [Event Hubs-Dienst: Übersicht](./event-hubs-about.md)
- [Event Hubs-Terminologie](event-hubs-features.md)
