---
title: Hinzufügen von benutzerdefinierten Daten zu Ereignissen in Azure Event Hubs
description: In diesem Artikel erfahren Sie, wie Sie benutzerdefinierte Daten zu Ereignissen in Azure Event Hubs hinzufügen.
ms.topic: how-to
ms.date: 03/19/2021
ms.openlocfilehash: 2e3f94228c6f2fbe5d533635a01062c701aa9664
ms.sourcegitcommit: 6ea4d4d1cfc913aef3927bef9e10b8443450e663
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2021
ms.locfileid: "113297530"
---
# <a name="add-custom-data-to-events-in-azure-event-hubs"></a>Hinzufügen von benutzerdefinierten Daten zu Ereignissen in Azure Event Hubs
Da ein Ereignis hauptsächlich aus einer undurchsichtigen Menge von Bytes besteht, ist es für Consumer dieser Ereignisse unter Umständen schwierig fundierte Entscheidungen hinsichtlich ihrer Verarbeitung zu treffen. Damit Ereignisherausgeber Consumern einen besseren Kontext bieten können, können Ereignisse auch benutzerdefinierte Metadaten in Form von mehreren Schlüssel-Wert-Paaren enthalten. Ein typisches Szenario für die Einbindung von Metadaten ist das Bereitstellen eines Hinweises zu dem in einem Ereignis enthaltenen Datentyp, sodass Consumer das Format erkennen und entsprechend deserialisieren können.

> [!NOTE]
> Diese Metadaten werden vom Event Hubs-Dienst nicht verwendet und sind für diesen nicht von Bedeutung. Sie dienen lediglich der Koordination zwischen Ereignisherausgebern und Consumern. 

In den folgenden Abschnitten erfahren Sie, wie Sie benutzerdefinierte Daten in verschiedenen Programmiersprachen zu Ereignissen hinzufügen. 

## <a name="net"></a>.NET 

```csharp
var eventBody = new BinaryData("Hello, Event Hubs!");
var eventData = new EventData(eventBody);
eventData.Properties.Add("EventType", "com.microsoft.samples.hello-event");
eventData.Properties.Add("priority", 1);
eventData.Properties.Add("score", 9.0);
```

Das vollständige Codebeispiel finden Sie unter [Publishing events with custom metadata (Veröffentlichen von Ereignissen mit benutzerdefinierten Metadaten)](https://github.com/Azure/azure-sdk-for-net/blob/master/sdk/eventhub/Azure.Messaging.EventHubs/samples/Sample04_PublishingEvents.md#publishing-events-with-custom-metadata).

## <a name="java"></a>Java

```java
EventData firstEvent = new EventData("EventData Sample 1".getBytes(UTF_8));
firstEvent.getProperties().put("EventType", "com.microsoft.samples.hello-event");
firstEvent.getProperties().put("priority", 1);
firstEvent.getProperties().put("score", 9.0);
```

Das vollständige Codebeispiel finden Sie unter [Publish events with custom metadata (Veröffentlichen von Ereignissen mit benutzerdefinierten Metadaten)](https://github.com/Azure/azure-sdk-for-java/blob/master/sdk/eventhubs/azure-messaging-eventhubs/src/samples/java/com/azure/messaging/eventhubs/PublishEventsWithCustomMetadata.java).


## <a name="python"></a>Python

```python
event_data = EventData('Message with properties')
event_data.properties = {'event-type': 'com.microsoft.samples.hello-event', 'priority': 1, "score": 9.0}
```

Das vollständige Codebeispiel finden Sie unter [Send Event Data batch with properties (Senden von Ereignisdatenbatches mit Eigenschaften)](https://github.com/Azure/azure-sdk-for-python/blob/azure-eventhub_5.3.1/sdk/eventhub/azure-eventhub/samples/async_samples/send_async.py).

## <a name="javascript"></a>JavaScript

```javascript
let eventData = { body: "First event", properties: { "event-type": "com.microsoft.samples.hello-event", "priority": 1, "score": 9.0  } };
```


## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen finden Sie in den folgenden Schnellstartanleitungen und Beispielen. 

- Schnellstartanleitungen: [.NET](event-hubs-dotnet-standard-getstarted-send.md), [Java](event-hubs-java-get-started-send.md), [Python](event-hubs-python-get-started-send.md), [JavaScript](event-hubs-node-get-started-send.md)
- Beispiele bei GitHub: [.NET](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/eventhub/Azure.Messaging.EventHubs/samples), [Java](https://github.com/Azure/azure-sdk-for-java/blob/master/sdk/eventhubs/azure-messaging-eventhubs/src/samples), [Python](https://github.com/Azure/azure-sdk-for-python/blob/azure-eventhub_5.3.1/sdk/eventhub/azure-eventhub/samples), [JavaScript](https://github.com/Azure/azure-sdk-for-js/tree/main/sdk/eventhub/event-hubs/samples/v5/javascript), [TypeScript](https://github.com/Azure/azure-sdk-for-js/tree/main/sdk/eventhub/event-hubs/samples/v5/typescript)
