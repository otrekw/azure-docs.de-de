---
author: craigshoemaker
ms.service: azure-functions
ms.topic: include
ms.date: 03/05/2019
ms.author: cshoe
ms.openlocfilehash: a31dc1c6d1a7f4dce6e7baae5a0e0e8f3d6d3d34
ms.sourcegitcommit: bdf31d87bddd04382effbc36e0c465235d7a2947
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/12/2020
ms.locfileid: "77179145"
---
## <a name="trigger"></a>Trigger

Der Funktionstrigger kann verwendet werden, um auf ein Ereignis zu reagieren, das an einen Event Hub-Datenstrom gesendet wird. Sie benötigen Lesezugriff auf den zugrunde liegenden Event Hub, um den Trigger einzurichten. Beim Auslösen der Funktion wird die an die Funktion übergebene Nachricht als Zeichenfolge eingegeben.

## <a name="trigger---scaling"></a>Trigger – Skalierung

Jede Instanz einer durch ein Ereignis ausgelösten Funktion wird durch eine einzelne [EventProcessorHost](https://docs.microsoft.com/dotnet/api/microsoft.azure.eventhubs.processor)-Instanz gesichert. Der (auf Event Hubs basierende) Trigger stellt sicher, dass nur eine [EventProcessorHost](https://docs.microsoft.com/dotnet/api/microsoft.azure.eventhubs.processor)-Instanz eine Lease für eine bestimmte Partition erhalten kann.

Stellen Sie sich einen Event Hub wie folgt vor:

* 10 Partitionen
* 1\.000 gleichmäßig auf alle Partitionen verteilte Ereignisse mit 100 Nachrichten in jeder Partition

Wenn Ihre Funktion zuerst aktiviert wird, gibt es nur eine Instanz der Funktion. Wir nennen die erste Funktionsinstanz `Function_0`. Die `Function_0`-Funktion umfasst eine einzelne Instanz von [EventProcessorHost](https://docs.microsoft.com/dotnet/api/microsoft.azure.eventhubs.processor) mit einer Lease auf allen zehn Partitionen. Diese Instanz beginnt mit dem Lesen von Ereignissen von den Partitionen 0-9. Von diesem Punkt an wird eines der folgenden Ereignisse eintreten:

* **Es sind keine neuen Funktionsinstanzen erforderlich:** `Function_0` kann alle 1.000 Ereignisse verarbeiten, bevor die Skalierungslogik von Functions einsetzt. In diesem Fall werden alle 1.000 Nachrichten von `Function_0` verarbeitet.

* **Eine weitere Funktionsinstanz wird hinzugefügt**: Wenn die Functions-Skalierungslogik bestimmt, dass `Function_0` mehr Nachrichten enthält, als verarbeitet werden können, wird eine neue Funktions-App-Instanz (`Function_1`) erstellt. Diese neue Funktion umfasst auch eine zugeordnete Instanz von [EventProcessorHost](https://docs.microsoft.com/dotnet/api/microsoft.azure.eventhubs.processor). Wenn die zugrunde liegende Event Hubs-Instanz erkennt, dass eine neue Hostinstanz versucht, Nachrichten zu lesen, wird ein Lastenausgleich der Partitionen in den Hostinstanzen vorgenommen. Zum Beispiel können die Partitionen 0-4 `Function_0` und die Partitionen 5-9 `Function_1` zugewiesen werden.

* **N weitere Funktionsinstanzen werden hinzufügt**: Wenn die Functions-Skalierungslogik bestimmt, dass sowohl `Function_0` als auch `Function_1` mehr Nachrichten aufweisen, als verarbeitet werden können, werden neue `Functions_N`-Funktions-App-Instanzen erstellt.  Apps werden erstellt, bis `N` größer ist als die Anzahl der Event Hub-Partitionen. In unserem Beispiel für Event Hubs erneut einen Lastenausgleich für die Partitionen aus, in diesem Fall für die Instanzen `Function_0`... `Functions_9`.

Wenn skaliert wird, entsprechen `N` Instanzen einer Zahl, die größer als die Anzahl der Event Hub-Partitionen ist. Dieses Muster wird verwendet, um sicherzustellen, dass [EventProcessorHost](https://docs.microsoft.com/dotnet/api/microsoft.azure.eventhubs.processor)-Instanzen verfügbar sind, um Partitionen zu sperren, sobald diese von anderen Instanzen bereitgestellt werden. Es werden Ihnen nur die Ressourcen berechnet, die bei der Ausführung der Funktionsinstanz in Anspruch genommen werden. Das heißt, die Kosten für diese übermäßige Bereitstellung werden nicht berechnet.

Wenn alle Funktionsausführungen abgeschlossen sind (mit oder ohne Fehler), werden Prüfpunkte dem zugehörigen Speicherkonto hinzugefügt. Wenn die Prüfpunkte erfolgreich erstellt wurden, werden alle 1.000 Nachrichten nie wieder abgerufen.

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

Das folgende Beispiel zeigt eine [C#-Funktion](../articles/azure-functions/functions-dotnet-class-library.md), die den Nachrichtentext des Event Hub-Triggers protokolliert.

```csharp
[FunctionName("EventHubTriggerCSharp")]
public static void Run([EventHubTrigger("samples-workitems", Connection = "EventHubConnectionAppSetting")] string myEventHubMessage, ILogger log)
{
    log.LogInformation($"C# function triggered to process a message: {myEventHubMessage}");
}
```

Um Zugriff auf die [Ereignismetadaten](#trigger---event-metadata) im Funktionscode zu erhalten, muss eine Bindung an ein [EventData](/dotnet/api/microsoft.servicebus.messaging.eventdata)-Objekt erfolgen (erfordert eine using-Anweisung für `Microsoft.Azure.EventHubs`). Sie können auch mithilfe von Bindungsausdrücken in der Methodensignatur auf die gleichen Eigenschaften zugreifen.  Im folgenden Beispiel werden beide Methoden zum Abrufen der gleichen Daten veranschaulicht:

```csharp
[FunctionName("EventHubTriggerCSharp")]
public static void Run(
    [EventHubTrigger("samples-workitems", Connection = "EventHubConnectionAppSetting")] EventData myEventHubMessage,
    DateTime enqueuedTimeUtc,
    Int64 sequenceNumber,
    string offset,
    ILogger log)
{
    log.LogInformation($"Event: {Encoding.UTF8.GetString(myEventHubMessage.Body)}");
    // Metadata accessed by binding to EventData
    log.LogInformation($"EnqueuedTimeUtc={myEventHubMessage.SystemProperties.EnqueuedTimeUtc}");
    log.LogInformation($"SequenceNumber={myEventHubMessage.SystemProperties.SequenceNumber}");
    log.LogInformation($"Offset={myEventHubMessage.SystemProperties.Offset}");
    // Metadata accessed by using binding expressions in method parameters
    log.LogInformation($"EnqueuedTimeUtc={enqueuedTimeUtc}");
    log.LogInformation($"SequenceNumber={sequenceNumber}");
    log.LogInformation($"Offset={offset}");
}
```

Um Ereignisse in einem Batch zu empfangen, deklarieren Sie `string` oder `EventData` als Array.  

> [!NOTE]
> Beim Empfangen in einem Batch können Sie `DateTime enqueuedTimeUtc` nicht für Bindungen mit Methodenparametern wie im obigen Beispiel verwenden und müssen diese von jedem `EventData`-Objekt erhalten.  

```cs
[FunctionName("EventHubTriggerCSharp")]
public static void Run([EventHubTrigger("samples-workitems", Connection = "EventHubConnectionAppSetting")] EventData[] eventHubMessages, ILogger log)
{
    foreach (var message in eventHubMessages)
    {
        log.LogInformation($"C# function triggered to process a message: {Encoding.UTF8.GetString(message.Body)}");
        log.LogInformation($"EnqueuedTimeUtc={message.SystemProperties.EnqueuedTimeUtc}");
    }
}
```

# <a name="c-scripttabcsharp-script"></a>[C#-Skript](#tab/csharp-script)

Das folgende Beispiel zeigt eine Event Hub-Triggerbindung in einer Datei *function.json* sowie eine [C#-Skriptfunktion](../articles/azure-functions/functions-reference-csharp.md), die die Bindung verwendet. Die Funktion protokolliert den Nachrichtentext des Event Hub-Triggers.

Die folgenden Beispiele zeigen die Event Hubs-Bindungsdaten in der *function.json*-Datei.

#### <a name="version-2x-and-higher"></a>Version 2.x und höher

```json
{
  "type": "eventHubTrigger",
  "name": "myEventHubMessage",
  "direction": "in",
  "eventHubName": "MyEventHub",
  "connection": "myEventHubReadConnectionAppSetting"
}
```

### <a name="version-1x"></a>Version 1.x

```json
{
  "type": "eventHubTrigger",
  "name": "myEventHubMessage",
  "direction": "in",
  "path": "MyEventHub",
  "connection": "myEventHubReadConnectionAppSetting"
}
```

Der C#-Skriptcode sieht wie folgt aus:

```cs
using System;

public static void Run(string myEventHubMessage, TraceWriter log)
{
    log.Info($"C# function triggered to process a message: {myEventHubMessage}");
}
```

Um Zugriff auf die [Ereignismetadaten](#trigger---event-metadata) im Funktionscode zu erhalten, muss eine Bindung an ein [EventData](/dotnet/api/microsoft.servicebus.messaging.eventdata)-Objekt erfolgen (erfordert eine using-Anweisung für `Microsoft.Azure.EventHubs`). Sie können auch mithilfe von Bindungsausdrücken in der Methodensignatur auf die gleichen Eigenschaften zugreifen.  Im folgenden Beispiel werden beide Methoden zum Abrufen der gleichen Daten veranschaulicht:

```cs
#r "Microsoft.Azure.EventHubs"

using System.Text;
using System;
using Microsoft.ServiceBus.Messaging;
using Microsoft.Azure.EventHubs;

public static void Run(EventData myEventHubMessage,
    DateTime enqueuedTimeUtc,
    Int64 sequenceNumber,
    string offset,
    TraceWriter log)
{
    log.Info($"Event: {Encoding.UTF8.GetString(myEventHubMessage.Body)}");
    log.Info($"EnqueuedTimeUtc={myEventHubMessage.SystemProperties.EnqueuedTimeUtc}");
    log.Info($"SequenceNumber={myEventHubMessage.SystemProperties.SequenceNumber}");
    log.Info($"Offset={myEventHubMessage.SystemProperties.Offset}");

    // Metadata accessed by using binding expressions
    log.Info($"EnqueuedTimeUtc={enqueuedTimeUtc}");
    log.Info($"SequenceNumber={sequenceNumber}");
    log.Info($"Offset={offset}");
}
```

Um Ereignisse in einem Batch zu empfangen, deklarieren Sie `string` oder `EventData` als ein Array:

```cs
public static void Run(string[] eventHubMessages, TraceWriter log)
{
    foreach (var message in eventHubMessages)
    {
        log.Info($"C# function triggered to process a message: {message}");
    }
}
```

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

Das folgende Beispiel zeigt eine Event Hub-Triggerbindung in einer Datei vom Typ *function.json* sowie eine [JavaScript-Funktion](../articles/azure-functions/functions-reference-node.md), die die Bindung verwendet. Die Funktion liest [Ereignismetadaten](#trigger---event-metadata) und protokolliert die Meldung.

Die folgenden Beispiele zeigen die Event Hubs-Bindungsdaten in der *function.json*-Datei.

#### <a name="version-2x-and-higher"></a>Version 2.x und höher

```json
{
  "type": "eventHubTrigger",
  "name": "myEventHubMessage",
  "direction": "in",
  "eventHubName": "MyEventHub",
  "connection": "myEventHubReadConnectionAppSetting"
}
```

### <a name="version-1x"></a>Version 1.x

```json
{
  "type": "eventHubTrigger",
  "name": "myEventHubMessage",
  "direction": "in",
  "path": "MyEventHub",
  "connection": "myEventHubReadConnectionAppSetting"
}
```

Der JavaScript-Code sieht wie folgt aus:

```javascript
module.exports = function (context, myEventHubMessage) {
    context.log('Function triggered to process a message: ', myEventHubMessage);
    context.log('EnqueuedTimeUtc =', context.bindingData.enqueuedTimeUtc);
    context.log('SequenceNumber =', context.bindingData.sequenceNumber);
    context.log('Offset =', context.bindingData.offset);

    context.done();
};
```

Legen Sie zum Empfangen von Ereignissen in einem Batch in der *function.json*-Datei `cardinality` auf `many` fest, wie im folgenden Beispiel gezeigt.

#### <a name="version-2x-and-higher"></a>Version 2.x und höher

```json
{
  "type": "eventHubTrigger",
  "name": "eventHubMessages",
  "direction": "in",
  "eventHubName": "MyEventHub",
  "cardinality": "many",
  "connection": "myEventHubReadConnectionAppSetting"
}
```

### <a name="version-1x"></a>Version 1.x

```json
{
  "type": "eventHubTrigger",
  "name": "eventHubMessages",
  "direction": "in",
  "path": "MyEventHub",
  "cardinality": "many",
  "connection": "myEventHubReadConnectionAppSetting"
}
```

Der JavaScript-Code sieht wie folgt aus:

```javascript
module.exports = function (context, eventHubMessages) {
    context.log(`JavaScript eventhub trigger function called for message array ${eventHubMessages}`);

    eventHubMessages.forEach((message, index) => {
        context.log(`Processed message ${message}`);
        context.log(`EnqueuedTimeUtc = ${context.bindingData.enqueuedTimeUtcArray[index]}`);
        context.log(`SequenceNumber = ${context.bindingData.sequenceNumberArray[index]}`);
        context.log(`Offset = ${context.bindingData.offsetArray[index]}`);
    });

    context.done();
};
```

# <a name="pythontabpython"></a>[Python](#tab/python)

Das folgende Beispiel zeigt eine Event Hub-Triggerbindung in einer Datei *function.json* sowie eine [Python-Funktion](../articles/azure-functions/functions-reference-python.md), die die Bindung verwendet. Die Funktion liest [Ereignismetadaten](#trigger---event-metadata) und protokolliert die Meldung.

Die folgenden Beispiele zeigen die Event Hubs-Bindungsdaten in der *function.json*-Datei.

```json
{
  "type": "eventHubTrigger",
  "name": "event",
  "direction": "in",
  "eventHubName": "MyEventHub",
  "connection": "myEventHubReadConnectionAppSetting"
}
```

Dies ist der Python-Code:

```python
import logging
import azure.functions as func


def main(event: func.EventHubEvent):
    logging.info('Function triggered to process a message: ', event.get_body())
    logging.info('  EnqueuedTimeUtc =', event.enqueued_time)
    logging.info('  SequenceNumber =', event.sequence_number)
    logging.info('  Offset =', event.offset)
```

# <a name="javatabjava"></a>[Java](#tab/java)

Das folgende Beispiel zeigt eine Event Hub-Triggerbindung, die den Nachrichtentext des Event Hub-Triggers protokolliert.

```java
@FunctionName("ehprocessor")
public void eventHubProcessor(
  @EventHubTrigger(name = "msg",
                  eventHubName = "myeventhubname",
                  connection = "myconnvarname") String message,
       final ExecutionContext context )
       {
          context.getLogger().info(message);
 }
```

 Verwenden Sie die `EventHubTrigger`-Anmerkung in der [Laufzeitbibliothek für Java-Funktionen](/java/api/overview/azure/functions/runtime) für Parameter, deren Wert von Event Hub empfangen wird. Parameter mit diesen Anmerkungen führen dazu, dass die Funktion ausgeführt wird, wenn ein Ereignis empfangen wird.  Diese Anmerkung kann mit nativen Java-Typen, POJOs oder Werten mit `Optional<T>`, die NULL-Werte annehmen können, verwendet werden.

 ---

## <a name="trigger---attributes-and-annotations"></a>Trigger – Attribute und Anmerkungen

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

Verwenden Sie in [C#-Klassenbibliotheken](../articles/azure-functions/functions-dotnet-class-library.md) das Attribut [EventHubTriggerAttribute](https://github.com/Azure/azure-functions-eventhubs-extension/blob/master/src/Microsoft.Azure.WebJobs.Extensions.EventHubs/EventHubTriggerAttribute.cs).

Der Konstruktor des Attributs nimmt den Namen des Event Hubs, den Namen der Consumergruppe und den Namen einer App-Einstellung an, die die Verbindungszeichenfolge enthält. Weitere Informationen zu diesen Einstellungen finden Sie im Abschnitt [Triggerkonfiguration](#trigger---configuration). Dieses Beispiel zeigt ein Attribut `EventHubTriggerAttribute`:

```csharp
[FunctionName("EventHubTriggerCSharp")]
public static void Run([EventHubTrigger("samples-workitems", Connection = "EventHubConnectionAppSetting")] string myEventHubMessage, ILogger log)
{
    ...
}
```

Ein vollständiges Beispiel finden Sie unter [Trigger: C#-Beispiel](#trigger).

# <a name="c-scripttabcsharp-script"></a>[C#-Skript](#tab/csharp-script)

Attribute werden von C#-Skript nicht unterstützt.

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

Attribute werden von JavaScript nicht unterstützt.

# <a name="pythontabpython"></a>[Python](#tab/python)

Attribute werden von Python nicht unterstützt.

# <a name="javatabjava"></a>[Java](#tab/java)

Verwenden Sie die [EventHubTrigger](https://docs.microsoft.com/java/api/com.microsoft.azure.functions.annotation.eventhubtrigger)-Anmerkung aus der [Laufzeitbibliothek für Java-Funktionen](https://docs.microsoft.com/java/api/overview/azure/functions/runtime) für Parameter, deren Wert von Event Hub empfangen wird. Parameter mit diesen Anmerkungen führen dazu, dass die Funktion ausgeführt wird, wenn ein Ereignis empfangen wird. Diese Anmerkung kann mit nativen Java-Typen, POJOs oder Werten mit `Optional<T>`, die NULL-Werte annehmen können, verwendet werden.

---

## <a name="trigger---configuration"></a>Trigger: Konfiguration

Die folgende Tabelle gibt Aufschluss über die Bindungskonfigurationseigenschaften, die Sie in der Datei *function.json* und im Attribut `EventHubTrigger` festlegen:

|Eigenschaft von „function.json“ | Attributeigenschaft |BESCHREIBUNG|
|---------|---------|----------------------|
|**type** | – | Muss auf `eventHubTrigger` festgelegt sein. Diese Eigenschaft wird automatisch festgelegt, wenn Sie den Trigger im Azure Portal erstellen.|
|**direction** | – | Muss auf `in` festgelegt sein. Diese Eigenschaft wird automatisch festgelegt, wenn Sie den Trigger im Azure Portal erstellen. |
|**name** | – | Der Name der Variablen, die das Ereigniselement im Funktionscode darstellt. |
|**path** |**EventHubName** | Nur Functions 1.x. Der Name des Event Hubs. Wenn der Event Hub-Name auch in der Verbindungszeichenfolge enthalten ist, setzt dieser Wert diese Eigenschaft zur Laufzeit außer Kraft. |
|**eventHubName** |**EventHubName** | Functions 2.x und höher Der Name des Event Hubs. Wenn der Event Hub-Name auch in der Verbindungszeichenfolge enthalten ist, setzt dieser Wert diese Eigenschaft zur Laufzeit außer Kraft. Auf den Wert kann über %eventHubName% in den App-Einstellungen verwiesen werden. |
|**consumerGroup** |**ConsumerGroup** | Eine optionale Eigenschaft, die zum Festlegen der [Consumergruppe](../articles/event-hubs/event-hubs-features.md#event-consumers) verwendet wird, mit der Ereignisse im Hub abonniert werden. Wird sie nicht angegeben, wird die Consumergruppe `$Default` verwendet. |
|**cardinality** | – | Für JavaScript. Legen Sie hierfür `many` fest, um Batchverarbeitung zu aktivieren.  Wenn diese Eigenschaft nicht angegeben oder auf `one` festgelegt ist, wird eine einzelne Nachricht an die Funktion übergeben. |
|**connection** |**Connection** | Der Name einer App-Einstellung, die die Zeichenfolge für die Verbindung mit dem Namespace des Event Hubs enthält. Kopieren Sie diese Verbindungszeichenfolge, indem Sie für den [Namespace](../articles/event-hubs/event-hubs-create.md#create-an-event-hubs-namespace) (nicht für den eigentlichen Event Hub) auf die Schaltfläche **Verbindungsinformationen** klicken. Diese Verbindungszeichenfolge muss mindestens über Leseberechtigungen verfügen, um den Trigger zu aktivieren.|

[!INCLUDE [app settings to local.settings.json](../articles/azure-functions/../../includes/functions-app-settings-local.md)]

## <a name="trigger---event-metadata"></a>Trigger – Ereignismetadaten

Der Event Hubs-Trigger stellt mehrere [Metadateneigenschaften](../articles/azure-functions/./functions-bindings-expressions-patterns.md) bereit. Metadateneigenschaften können als Teil der Bindungsausdrücke in anderen Bindungen oder als Parameter im Code verwendet werden. Die Eigenschaften stammen aus der [EventData](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.eventdata)-Klasse.

|Eigenschaft|type|Beschreibung|
|--------|----|-----------|
|`PartitionContext`|[PartitionContext](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.partitioncontext)|Die `PartitionContext`-Instanz.|
|`EnqueuedTimeUtc`|`DateTime`|Die in die Warteschlange eingereihte Uhrzeit in UTC.|
|`Offset`|`string`|Der Offset der Daten relativ zum Event Hub-Partitionsdatenstrom. Der Offset ist ein Marker oder ein Bezeichner für ein Ereignis innerhalb des Event Hubs-Datenstroms. Der Bezeichner ist innerhalb einer Partition des Event Hubs-Datenstroms eindeutig.|
|`PartitionKey`|`string`|Die Partition, an die Ereignisdaten gesendet werden sollen.|
|`Properties`|`IDictionary<String,Object>`|Die Benutzereigenschaften der Ereignisdaten.|
|`SequenceNumber`|`Int64`|Die Protokollfolgenummer des Ereignisses.|
|`SystemProperties`|`IDictionary<String,Object>`|Die Systemeigenschaften, einschließlich der Ereignisdaten.|

[Codebeispiele](#trigger) mit diesen Eigenschaften finden Sie weiter oben in diesem Artikel.

## <a name="trigger---hostjson-properties"></a>Trigger: Eigenschaften von „host.json“

Die Datei [host.json](../articles/azure-functions/functions-host-json.md#eventhub) enthält Einstellungen, die das Verhalten des Event Hubs-Triggers steuern.

[!INCLUDE [functions-host-json-event-hubs](../articles/azure-functions/../../includes/functions-host-json-event-hubs.md)]

## <a name="output"></a>Output

Mit der Event Hubs-Ausgabebindung werden Ereignisse in einen Ereignisdatenstrom geschrieben. Um Ereignisse in einen Event Hub schreiben zu können, müssen Sie über eine Sendeberechtigung verfügen.

Stellen Sie sicher, dass die erforderlichen Paketverweise vorhanden sind, bevor Sie versuchen, eine Ausgabebindung zu implementieren.

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

Das folgende Beispiel zeigt eine [C#-Funktion](../articles/azure-functions/functions-dotnet-class-library.md), die eine Nachricht in einen Event Hub schreibt. Dabei wird der Rückgabewert der Methode als Ausgabe verwendet:

```csharp
[FunctionName("EventHubOutput")]
[return: EventHub("outputEventHubMessage", Connection = "EventHubConnectionAppSetting")]
public static string Run([TimerTrigger("0 */5 * * * *")] TimerInfo myTimer, ILogger log)
{
    log.LogInformation($"C# Timer trigger function executed at: {DateTime.Now}");
    return $"{DateTime.Now}";
}
```

Das folgende Beispiel zeigt, wie Sie die `IAsyncCollector`-Schnittstelle verwenden, um einen Nachrichtenbatch zu senden. Dieses Szenario kommt häufig vor, wenn Sie Nachrichten verarbeiten, die von einem Event Hub kommen, und das Ergebnis an einen anderen Event Hub senden.

```csharp
[FunctionName("EH2EH")]
public static async Task Run(
    [EventHubTrigger("source", Connection = "EventHubConnectionAppSetting")] EventData[] events,
    [EventHub("dest", Connection = "EventHubConnectionAppSetting")]IAsyncCollector<string> outputEvents,
    ILogger log)
{
    foreach (EventData eventData in events)
    {
        // do some processing:
        var myProcessedEvent = DoSomething(eventData);

        // then send the message
        await outputEvents.AddAsync(JsonConvert.SerializeObject(myProcessedEvent));
    }
}
```

# <a name="c-scripttabcsharp-script"></a>[C#-Skript](#tab/csharp-script)

Das folgende Beispiel zeigt eine Event Hub-Triggerbindung in einer Datei *function.json* sowie eine [C#-Skriptfunktion](../articles/azure-functions/functions-reference-csharp.md), die die Bindung verwendet. Die Funktion schreibt eine Nachricht in einen Event Hub.

Die folgenden Beispiele zeigen die Event Hubs-Bindungsdaten in der *function.json*-Datei. Das erste Beispiel gilt für Functions 2.x und höher und das zweite für Functions 1.x. 

```json
{
    "type": "eventHub",
    "name": "outputEventHubMessage",
    "eventHubName": "myeventhub",
    "connection": "MyEventHubSendAppSetting",
    "direction": "out"
}
```

```json
{
    "type": "eventHub",
    "name": "outputEventHubMessage",
    "path": "myeventhub",
    "connection": "MyEventHubSendAppSetting",
    "direction": "out"
}
```

Der folgende C#-Skriptcode erstellt eine Nachricht:

```cs
using System;
using Microsoft.Extensions.Logging;

public static void Run(TimerInfo myTimer, out string outputEventHubMessage, ILogger log)
{
    String msg = $"TimerTriggerCSharp1 executed at: {DateTime.Now}";
    log.LogInformation(msg);   
    outputEventHubMessage = msg;
}
```

Der folgende C#-Skriptcode erstellt mehrere Nachrichten:

```cs
public static void Run(TimerInfo myTimer, ICollector<string> outputEventHubMessage, ILogger log)
{
    string message = $"Message created at: {DateTime.Now}";
    log.LogInformation(message);
    outputEventHubMessage.Add("1 " + message);
    outputEventHubMessage.Add("2 " + message);
}
```

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

Das folgende Beispiel zeigt eine Event Hub-Triggerbindung in einer Datei vom Typ *function.json* sowie eine [JavaScript-Funktion](../articles/azure-functions/functions-reference-node.md), die die Bindung verwendet. Die Funktion schreibt eine Nachricht in einen Event Hub.

Die folgenden Beispiele zeigen die Event Hubs-Bindungsdaten in der *function.json*-Datei. Das erste Beispiel gilt für Functions 2.x und höher und das zweite für Functions 1.x. 

```json
{
    "type": "eventHub",
    "name": "outputEventHubMessage",
    "eventHubName": "myeventhub",
    "connection": "MyEventHubSendAppSetting",
    "direction": "out"
}
```

```json
{
    "type": "eventHub",
    "name": "outputEventHubMessage",
    "path": "myeventhub",
    "connection": "MyEventHubSendAppSetting",
    "direction": "out"
}
```

Der folgende JaveScript-Code sendet eine einzelne Nachricht:

```javascript
module.exports = function (context, myTimer) {
    var timeStamp = new Date().toISOString();
    context.log('Message created at: ', timeStamp);   
    context.bindings.outputEventHubMessage = "Message created at: " + timeStamp;
    context.done();
};
```

Der folgende JaveScript-Code sendet mehrere Nachrichten:

```javascript
module.exports = function(context) {
    var timeStamp = new Date().toISOString();
    var message = 'Message created at: ' + timeStamp;

    context.bindings.outputEventHubMessage = [];

    context.bindings.outputEventHubMessage.push("1 " + message);
    context.bindings.outputEventHubMessage.push("2 " + message);
    context.done();
};
```

# <a name="pythontabpython"></a>[Python](#tab/python)

Das folgende Beispiel zeigt eine Event Hub-Triggerbindung in einer Datei *function.json* sowie eine [Python-Funktion](../articles/azure-functions/functions-reference-python.md), die die Bindung verwendet. Die Funktion schreibt eine Nachricht in einen Event Hub.

Die folgenden Beispiele zeigen die Event Hubs-Bindungsdaten in der *function.json*-Datei.

```json
{
    "type": "eventHub",
    "name": "$return",
    "eventHubName": "myeventhub",
    "connection": "MyEventHubSendAppSetting",
    "direction": "out"
}
```

Der folgende Python-Code sendet eine einzelne Nachricht:

```python
import datetime
import logging
import azure.functions as func


def main(timer: func.TimerRequest) -> str:
    timestamp = datetime.datetime.utcnow()
    logging.info('Message created at: %s', timestamp)
    return 'Message created at: {}'.format(timestamp)
```

# <a name="javatabjava"></a>[Java](#tab/java)

Das folgende Beispiel zeigt eine Java-Funktion, die eine Nachricht mit der aktuellen Uhrzeit in einen Event Hub schreibt.

```java
@FunctionName("sendTime")
@EventHubOutput(name = "event", eventHubName = "samples-workitems", connection = "AzureEventHubConnection")
public String sendTime(
   @TimerTrigger(name = "sendTimeTrigger", schedule = "0 */5 * * * *") String timerInfo)  {
     return LocalDateTime.now().toString();
 }
```

Verwenden Sie die `@EventHubOutput`-Anmerkung in der [Laufzeitbibliothek für Java-Funktionen](/java/api/overview/azure/functions/runtime) für Parameter, deren Wert in Event Hub veröffentlicht wird.  Der Parametertyp sollte `OutputBinding<T>` lauten, wobei „T“ für ein POJO oder einen beliebigen nativen Java-Typ steht.

---

## <a name="output---attributes-and-annotations"></a>Ausgabe – Attribute und Anmerkungen

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

Verwenden Sie für [C#-Klassenbibliotheken](../articles/azure-functions/functions-dotnet-class-library.md) das Attribut [EventHubAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/EventHubs/EventHubAttribute.cs).

Der Konstruktor des Attributs nimmt den Namen des Event Hubs und den Namen einer App-Einstellung an, die die Verbindungszeichenfolge enthält. Weitere Informationen zu diesen Einstellungen finden Sie unter [Ausgabe: Konfiguration](#output---configuration). Dieses Beispiel zeigt ein Attribut `EventHub`:

```csharp
[FunctionName("EventHubOutput")]
[return: EventHub("outputEventHubMessage", Connection = "EventHubConnectionAppSetting")]
public static string Run([TimerTrigger("0 */5 * * * *")] TimerInfo myTimer, ILogger log)
{
    ...
}
```

Ein vollständiges Beispiel finden Sie unter [Ausgabe: C#-Beispiel](#output).

# <a name="c-scripttabcsharp-script"></a>[C#-Skript](#tab/csharp-script)

Attribute werden von C#-Skript nicht unterstützt.

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

Attribute werden von JavaScript nicht unterstützt.

# <a name="pythontabpython"></a>[Python](#tab/python)

Attribute werden von Python nicht unterstützt.

# <a name="javatabjava"></a>[Java](#tab/java)

Verwenden Sie die [EventHubOutput](https://docs.microsoft.com/java/api/com.microsoft.azure.functions.annotation.eventhuboutput)-Anmerkung in der [Laufzeitbibliothek für Java-Funktionen](https://docs.microsoft.com/java/api/overview/azure/functions/runtime) für Parameter, deren Wert in Event Hub veröffentlicht wird. Der Parametertyp sollte `OutputBinding<T>` lauten, wobei `T` für ein POJO oder einen beliebigen nativen Java-Typ steht.

---

## <a name="output---configuration"></a>Ausgabe: Konfiguration

Die folgende Tabelle gibt Aufschluss über die Bindungskonfigurationseigenschaften, die Sie in der Datei *function.json* und im Attribut `EventHub` festlegen:

|Eigenschaft von „function.json“ | Attributeigenschaft |Beschreibung|
|---------|---------|----------------------|
|**type** | – | Muss auf „eventHub“ festgelegt werden. |
|**direction** | – | Muss auf „out“ festgelegt werden. Dieser Parameter wird automatisch festgelegt, wenn Sie die Bindung im Azure Portal erstellen. |
|**name** | – | Der Variablenname, der in Funktionscode verwendet wird, der das Ereignis darstellt. |
|**path** |**EventHubName** | Nur Functions 1.x. Der Name des Event Hubs. Wenn der Event Hub-Name auch in der Verbindungszeichenfolge enthalten ist, setzt dieser Wert diese Eigenschaft zur Laufzeit außer Kraft. |
|**eventHubName** |**EventHubName** | Functions 2.x und höher Der Name des Event Hubs. Wenn der Event Hub-Name auch in der Verbindungszeichenfolge enthalten ist, setzt dieser Wert diese Eigenschaft zur Laufzeit außer Kraft. |
|**connection** |**Connection** | Der Name einer App-Einstellung, die die Zeichenfolge für die Verbindung mit dem Namespace des Event Hubs enthält. Kopieren Sie diese Verbindungszeichenfolge, indem Sie für den *Namespace* (nicht für den eigentlichen Event Hub) auf die Schaltfläche **Verbindungsinformationen** klicken. Diese Verbindungszeichenfolge muss über Sendeberechtigungen zum Senden der Nachricht an den Ereignisstrom verfügen.|

[!INCLUDE [app settings to local.settings.json](../articles/azure-functions/../../includes/functions-app-settings-local.md)]

## <a name="output---usage"></a>Ausgabe: Verwendung

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

Senden Sie Nachrichten mithilfe eines Methodenparameters wie `out string paramName`. In C#-Skripts ist `paramName` der Wert, der in der Eigenschaft `name` von *function.json* angegeben ist. Um mehrere Nachrichten zu schreiben, können Sie `ICollector<string>` oder `IAsyncCollector<string>` anstelle von `out string` verwenden.

# <a name="c-scripttabcsharp-script"></a>[C#-Skript](#tab/csharp-script)

Senden Sie Nachrichten mithilfe eines Methodenparameters wie `out string paramName`. In C#-Skripts ist `paramName` der Wert, der in der Eigenschaft `name` von *function.json* angegeben ist. Um mehrere Nachrichten zu schreiben, können Sie `ICollector<string>` oder `IAsyncCollector<string>` anstelle von `out string` verwenden.

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

Greifen Sie mithilfe von `context.bindings.<name>` auf das Ausgabeereignis zu, wobei `<name>` der in der Eigenschaft `name` von *function.json* angegebene Wert ist.

# <a name="pythontabpython"></a>[Python](#tab/python)

Es gibt zwei Optionen für das Ausgeben einer Event Hub-Nachricht aus einer Funktion:

- **Rückgabewert**: Legen Sie die Eigenschaft `name` in *function.json* auf `$return` fest. Mit dieser Konfiguration wird der Rückgabewert der Funktion als Event Hub-Nachricht beibehalten.

- **Imperativ**: Übergeben Sie einen Wert an die [set](https://docs.microsoft.com/python/api/azure-functions/azure.functions.out?view=azure-python#set-val--t-----none)-Methode des Parameters, der als [Out](https://docs.microsoft.com/python/api/azure-functions/azure.functions.out?view=azure-python)-Typ deklariert ist. Der an `set` übergebene Wert wird als Event Hub-Nachricht beibehalten.

# <a name="javatabjava"></a>[Java](#tab/java)

Es gibt zwei Optionen für das Ausgeben einer Event Hub-Nachricht aus einer Funktion mittels der [EventHubOutput](https://docs.microsoft.com/java/api/com.microsoft.azure.functions.annotation.eventhuboutput)-Anmerkung:

- **Rückgabewert**: Wenn Sie die Anmerkung auf die Funktion selbst anwenden, wird der Rückgabewert der Funktion als Event Hub-Nachricht beibehalten.

- **Imperativ**: Um den Nachrichtenwert explizit festzulegen, wenden Sie die Anmerkung auf einen bestimmten Parameter des Typs [`OutputBinding<T>`](https://docs.microsoft.com/java/api/com.microsoft.azure.functions.OutputBinding) an, wobei `T` ein POJO oder ein beliebiger nativer Java-Typ ist. Bei dieser Konfiguration wird bei Übergabe eines Werts an die `setValue`-Methode der Wert als Event Hub-Nachricht beibehalten.

---

## <a name="exceptions-and-return-codes"></a>Ausnahmen und Rückgabecodes

| Bindung | Verweis |
|---|---|
| Event Hub | [Betriebsleitfaden](https://docs.microsoft.com/rest/api/eventhub/publisher-policy-operations) |

<a name="host-json"></a>  

## <a name="hostjson-settings"></a>Einstellungen für „host.json“

In diesem Abschnitt werden die verfügbaren globalen Konfigurationseinstellungen für diese Bindung in Version 2.x und höheren Versionen beschrieben. Die nachfolgende Beispieldatei „host.json“ enthält nur die Einstellungen für Version 2.x und höhere Versionen für diese Bindung. Weitere Informationen zu globalen Konfigurationseinstellungen in Version 2.x und höheren Versionen finden Sie unter [host.json-Referenz für Azure Functions 2.x](../articles/azure-functions/functions-host-json.md).

> [!NOTE]
> Eine Referenz für „host.json“ in Functions 1.x finden Sie unter [host.json-Referenz für Azure Functions 1.x](../articles/azure-functions/functions-host-json-v1.md).

```json
{
    "version": "2.0",
    "extensions": {
        "eventHubs": {
            "batchCheckpointFrequency": 5,
            "eventProcessorOptions": {
                "maxBatchSize": 256,
                "prefetchCount": 512
            }
        }
    }
}  
```

|Eigenschaft  |Standard | BESCHREIBUNG |
|---------|---------|---------|
|`maxBatchSize`|10|Die maximale Ereignisanzahl, die pro Empfangsschleife empfangen wird.|
|`prefetchCount`|300|Die standardmäßige Vorabrufanzahl, die vom zugrunde liegenden `EventProcessorHost` verwendet wird.|
|`batchCheckpointFrequency`|1|Die Anzahl der zu verarbeitenden Ereignisbatches, bevor ein EventHub-Cursorprüfpunkt erstellt wird.|
