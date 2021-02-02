---
title: 'Verfügbarkeit und Konsistenz: Azure Event Hubs | Microsoft-Dokumentation'
description: Hier erfahren Sie, wie Sie maximale Verfügbarkeit und Konsistenz in Azure Event Hubs mit Partitionen erzielen.
ms.topic: article
ms.date: 01/25/2021
ms.custom: devx-track-csharp
ms.openlocfilehash: 2fdb62e953230a38a26d22e136789fea52c8ee8c
ms.sourcegitcommit: aaa65bd769eb2e234e42cfb07d7d459a2cc273ab
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/27/2021
ms.locfileid: "98882194"
---
# <a name="availability-and-consistency-in-event-hubs"></a>Verfügbarkeit und Konsistenz in Event Hubs
Dieser Artikel enthält Informationen zur Unterstützung der Verfügbarkeit und Konsistenz für Azure Event Hubs. 

## <a name="availability"></a>Verfügbarkeit
Mit Azure Event Hubs wird das Risiko von katastrophalen Ausfällen einzelner Computer oder sogar ganzer Racks auf Cluster verteilt, die sich über mehrere Fehlerdomänen in einem Rechenzentrum erstrecken. Außerdem werden vom Dienst transparente Fehlererkennungs- und Failovermechanismen implementiert. Daher kann er auch bei den oben erwähnten Ausfällen weiterhin innerhalb der garantierten Servicelevel und in der Regel ohne spürbare Unterbrechungen ausgeführt werden. Wenn ein Event Hubs-Namespace mit aktivierter Option für [Verfügbarkeitszonen](../availability-zones/az-overview.md) erstellt wurde, wird das Ausfallrisiko noch weiter auf drei physisch getrennte Einrichtungen verteilt. Der Dienst verfügt dann über genügend Kapazitätsreserven, um den vollständigen, katastrophalen Ausfall einer gesamten Einrichtung sofort bewältigen zu können. Weitere Informationen finden Sie unter [Azure Event Hubs: Georedundante Notfallwiederherstellung](event-hubs-geo-dr.md).

Wenn eine Clientanwendung Ereignisse an einen Event Hub sendet, werden diese automatisch auf die Partitionen Ihres Event Hubs verteilt. Falls eine Partition nicht verfügbar sein sollte, werden die Ereignisse auf die restlichen Partitionen verteilt. Dieses Verhalten ermöglicht das größte Maß an Betriebszeit. Für Anwendungsfälle, für die eine maximale Betriebszeit Voraussetzung ist, wird dieses Modell bevorzugt, anstatt Ereignisse an eine bestimmte Partition zu senden. Weitere Informationen finden Sie unter [Partitionen](event-hubs-scalability.md#partitions).

## <a name="consistency"></a>Konsistenz
In manchen Szenarios kann die Reihenfolge der Ereignisse wichtig sein. Nehmen Sie beispielsweise an, dass Ihr Back-End-System einen Updatebefehl vor einem Löschbefehl ausführen soll. Bei diesem Szenario sendet eine Clientanwendung Ereignisse an eine bestimmte Partition, damit die Reihenfolge beibehalten wird. Wenn eine Consumeranwendung diese auf der Partition vorhandenen Ereignisse nutzt, werden sie in der richtigen Reihenfolge gelesen. 

Bedenken Sie bei dieser Konfiguration, dass Sie eine Fehlerantwort erhalten, wenn die Partition, an die Sie senden, nicht verfügbar ist. Zum Vergleich: Wenn Sie keine bestimmte Partition bevorzugen, sendet der Event Hubs-Dienst das Ereignis an die nächste verfügbare Partition.

Eine mögliche Lösung, um die Reihenfolge sicherzustellen und dabei auch die Betriebszeit zu maximieren, wäre das Aggregieren der Ereignisse als Teil Ihrer Anwendung zur Ereignisverarbeitung. Dies kann am einfachsten erreicht werden, indem das Ereignis mit einer benutzerdefinierten Sequenznummerneigenschaft gestempelt wird.

Bei diesem Szenario sendet der Producerclient Ereignisse an eine verfügbare Partition in Ihrem Event Hub und legt die entsprechende Sequenznummer aus Ihrer Anwendung fest. Diese Lösung erfordert, dass der Status von der Verarbeitungsanwendung beibehalten werden muss, aber sie bietet Ihren Absendern einen Endpunkt, dessen Verfügbarkeit wahrscheinlicher ist.

## <a name="appendix"></a>Anhang

### <a name="net-examples"></a>.NET-Beispiele

#### <a name="send-events-to-a-specific-partition"></a>Senden von Ereignissen an eine bestimmte Partition
Legen Sie entweder den Partitionsschlüssel für ein Ereignis fest, oder verwenden Sie ein `PartitionSender`-Objekt (bei Nutzung der alten Microsoft.Azure.Messaging-Bibliothek), um Ereignisse nur an eine bestimmte Partition zu senden. Auf diese Weise wird sichergestellt, dass diese Ereignisse ggf. in der richtigen Reihenfolge aus der Partition gelesen werden. 

Wenn Sie die neuere **Azure.Messaging.EventHubs**-Bibliothek verwenden, finden Sie weitere Informationen unter [Migrieren von Code von PartitionSender zu EventHubProducerClient zum Veröffentlichen von Ereignissen in einer Partition](https://github.com/Azure/azure-sdk-for-net/blob/master/sdk/eventhub/Azure.Messaging.EventHubs/MigrationGuide.md#migrating-code-from-partitionsender-to-eventhubproducerclient-for-publishing-events-to-a-partition).

#### <a name="azuremessagingeventhubs-500-or-later"></a>[Azure.Messaging.EventHubs (5.0.0 oder höher](#tab/latest))

```csharp
var connectionString = "<< CONNECTION STRING FOR THE EVENT HUBS NAMESPACE >>";
var eventHubName = "<< NAME OF THE EVENT HUB >>";

await using (var producerClient = new EventHubProducerClient(connectionString, eventHubName))
{
    var batchOptions = new CreateBatchOptions() { PartitionId = "my-partition-id" };
    using EventDataBatch eventBatch = await producerClient.CreateBatchAsync(batchOptions);
    eventBatch.TryAdd(new EventData(Encoding.UTF8.GetBytes("First")));
    eventBatch.TryAdd(new EventData(Encoding.UTF8.GetBytes("Second")));
    
    await producerClient.SendAsync(eventBatch);
}
```

#### <a name="microsoftazureeventhubs-410-or-earlier"></a>[Microsoft.Azure.EventHubs (4.1.0 oder früher)](#tab/old)

```csharp
var connectionString = "<< CONNECTION STRING FOR THE EVENT HUBS NAMESPACE >>";
var eventHubName = "<< NAME OF THE EVENT HUB >>";

var connectionStringBuilder = new EventHubsConnectionStringBuilder(connectionString){ EntityPath = eventHubName }; 
var eventHubClient = EventHubClient.CreateFromConnectionString(connectionStringBuilder.ToString());
PartitionSender partitionSender = eventHubClient.CreatePartitionSender("my-partition-id");
try
{
    EventDataBatch eventBatch = partitionSender.CreateBatch();
    eventBatch.TryAdd(new EventData(Encoding.UTF8.GetBytes("First")));
    eventBatch.TryAdd(new EventData(Encoding.UTF8.GetBytes("Second")));

    await partitionSender.SendAsync(eventBatch);
}
finally
{
    await partitionSender.CloseAsync();
    await eventHubClient.CloseAsync();
}
```

---

### <a name="set-a-sequence-number"></a>Festlegen einer Sequenznummer
Im folgenden Beispiel wird Ihr Ereignis mit einer Eigenschaft für eine benutzerdefinierte Sequenznummer gestempelt. 

#### <a name="azuremessagingeventhubs-500-or-later"></a>[Azure.Messaging.EventHubs (5.0.0 oder höher](#tab/latest))

```csharp
// create a producer client that you can use to send events to an event hub
await using (var producerClient = new EventHubProducerClient(connectionString, eventHubName))
{
    // get the latest sequence number from your application
    var sequenceNumber = GetNextSequenceNumber();

    // create a batch of events 
    using EventDataBatch eventBatch = await producerClient.CreateBatchAsync();

    // create a new EventData object by encoding a string as a byte array
    var data = new EventData(Encoding.UTF8.GetBytes("This is my message..."));

    // set a custom sequence number property
    data.Properties.Add("SequenceNumber", sequenceNumber);

    // add events to the batch. An event is a represented by a collection of bytes and metadata. 
    eventBatch.TryAdd(data);

    // use the producer client to send the batch of events to the event hub
    await producerClient.SendAsync(eventBatch);
}
```

#### <a name="microsoftazureeventhubs-410-or-earlier"></a>[Microsoft.Azure.EventHubs (4.1.0 oder früher)](#tab/old)
```csharp
// Create an Event Hubs client
var client = new EventHubClient(connectionString, eventHubName);

//Create a producer to produce events
EventHubProducer producer = client.CreateProducer();

// Get the latest sequence number from your application 
var sequenceNumber = GetNextSequenceNumber();

// Create a new EventData object by encoding a string as a byte array
var data = new EventData(Encoding.UTF8.GetBytes("This is my message..."));

// Set a custom sequence number property
data.Properties.Add("SequenceNumber", sequenceNumber);

// Send single message async
await producer.SendAsync(data);
```
---

Bei diesem Beispiel wird das Ereignis an eine der verfügbaren Partitionen in Ihrem Event Hub gesendet und die entsprechende Sequenznummer aus Ihrer Anwendung festgelegt. Diese Lösung erfordert, dass der Status von der Verarbeitungsanwendung beibehalten werden muss, aber sie bietet Ihren Absendern einen Endpunkt, dessen Verfügbarkeit wahrscheinlicher ist.

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu Event Hubs finden Sie unter den folgenden Links:

* [Event Hubs-Dienst: Übersicht](./event-hubs-about.md)
* [Erstellen eines Event Hubs](event-hubs-create.md)
