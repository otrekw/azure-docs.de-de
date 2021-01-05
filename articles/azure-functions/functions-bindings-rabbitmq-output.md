---
title: RabbitMQ-Ausgabebindungen für Azure Functions
description: Erfahren Sie, wie Sie RabbitMQ-Nachrichten mit Azure Functions senden.
author: cachai2
ms.assetid: ''
ms.topic: reference
ms.date: 12/13/2020
ms.author: cachai
ms.custom: ''
ms.openlocfilehash: 212bfcee09cd63b6ff09faaba4d99e4b4c583fe8
ms.sourcegitcommit: 2ba6303e1ac24287762caea9cd1603848331dd7a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/15/2020
ms.locfileid: "97505667"
---
# <a name="rabbitmq-output-binding-for-azure-functions-overview"></a>RabbitMQ-Ausgabebindung für Azure Functions – Übersicht

> [!NOTE]
> RabbitMQ-Bindungen werden nur mit **Windows Premium**-Plänen vollständig unterstützt. Im Verbrauchstarif und unter Linux werden sie derzeit nicht unterstützt.

Verwenden Sie die RabbitMQ-Ausgabebindung, um Nachrichten an eine RabbitMQ-Warteschlange zu senden.

Informationen zu Setup- und Konfigurationsdetails finden Sie in der [Übersicht](functions-bindings-rabbitmq-output.md).

## <a name="example"></a>Beispiel

# <a name="c"></a>[C#](#tab/csharp)

Das folgende Beispiel zeigt eine [C#-Funktion](functions-dotnet-class-library.md), die eine RabbitMQ-Nachricht sendet, wenn sie alle fünf Minuten durch einen TimerTrigger ausgelöst wird, wobei der Rückgabewert der Methode als Ausgabe verwendet wird:

```cs
[FunctionName("RabbitMQOutput")]
[return: RabbitMQ("outputQueue", ConnectionStringSetting = "ConnectionStringSetting")]
public static string Run([TimerTrigger("0 */5 * * * *")] TimerInfo myTimer, ILogger log)
{
    log.LogInformation($"C# Timer trigger function executed at: {DateTime.Now}");
    return $"{DateTime.Now}";
}
```

Das folgende Beispiel zeigt, wie Sie die IAsyncCollector-Schnittstelle verwenden, um Nachrichten zu senden.

```cs
[FunctionName("RabbitMQOutput")]
public static async Task Run(
[RabbitMQTrigger("sourceQueue", ConnectionStringSetting = "TriggerConnectionString")] string rabbitMQEvent,
[RabbitMQ("destinationQueue", ConnectionStringSetting = "OutputConnectionString")]IAsyncCollector<string> outputEvents,
ILogger log)
{
    // processing:
    var myProcessedEvent = DoSomething(rabbitMQEvent);
    
     // send the message
    await outputEvents.AddAsync(JsonConvert.SerializeObject(myProcessedEvent));
}
```

Im folgenden Beispiel wird veranschaulicht, wie die Nachrichten als POCO gelesen werden.

```cs
public class TestClass
{
    public string x { get; set; }
}

[FunctionName("RabbitMQOutput")]
public static async Task Run(
[RabbitMQTrigger("sourceQueue", ConnectionStringSetting = "TriggerConnectionString")] TestClass rabbitMQEvent,
[RabbitMQ("destinationQueue", ConnectionStringSetting = "OutputConnectionString")]IAsyncCollector<TestClass> outputPocObj,
ILogger log)
{
    // send the message
    await outputPocObj.Add(rabbitMQEvent);
}
```

# <a name="c-script"></a>[C#-Skript](#tab/csharp-script)

Das folgende Beispiel zeigt eine RabbitMQ-Ausgabebindung in einer Datei *function.json* sowie eine [C#-Skriptfunktion](functions-reference-csharp.md), die die Bindung verwendet. Die Funktion liest in der Nachricht von einem HTTP-Trigger und gibt sie in der RabbitMQ-Warteschlange aus.

Bindungsdaten in der Datei *function.json*:

```json
{
    "bindings": [
        {
            "type": "httpTrigger",
            "direction": "in",
            "authLevel": "function",
            "name": "input",
            "methods": [
                "get",
                "post"
            ]
        },
        {
            "type": "rabbitMQ",
            "name": "outputMessage",
            "queueName": "outputQueue",
            "connectionStringSetting": "connectionStringAppSetting",
            "direction": "out"
        }
    ]
}
```

Der C#-Skriptcode sieht wie folgt aus:

```csx
using System;
using Microsoft.Extensions.Logging;

public static void Run(string input, out string outputMessage, ILogger log)
{
    log.LogInformation(input);
    outputMessage = input;
}
```

# <a name="javascript"></a>[JavaScript](#tab/javascript)

Das folgende Beispiel zeigt eine RabbitMQ-Ausgabebindung in einer Datei *function.json* sowie eine [JavaScript-Funktion](functions-reference-node.md), die die Bindung verwendet. Die Funktion liest in der Nachricht von einem HTTP-Trigger und gibt sie in der RabbitMQ-Warteschlange aus.

Bindungsdaten in der Datei *function.json*:

```json
{
    "bindings": [
        {
            "type": "httpTrigger",
            "direction": "in",
            "authLevel": "function",
            "name": "input",
            "methods": [
                "get",
                "post"
            ]
        },
        {
            "type": "rabbitMQ",
            "name": "outputMessage",
            "queueName": "outputQueue",
            "connectionStringSetting": "connectionStringAppSetting",
            "direction": "out"
        }
    ]
}
```

Der JavaScript-Code sieht wie folgt aus:

```javascript
module.exports = function (context, input) {
    context.bindings.myQueueItem = input.body;
    context.done();
};
```

# <a name="python"></a>[Python](#tab/python)

Das folgende Beispiel zeigt eine RabbitMQ-Ausgabebindung in einer Datei *function.json* sowie eine Python-Funktion, die die Bindung verwendet. Die Funktion liest in der Nachricht von einem HTTP-Trigger und gibt sie in der RabbitMQ-Warteschlange aus.

Bindungsdaten in der Datei *function.json*:

```json
{
    "scriptFile": "__init__.py",
    "bindings": [
        {
            "authLevel": "function",
            "type": "httpTrigger",
            "direction": "in",
            "name": "req",
            "methods": [
                "get",
                "post"
            ]
        },
        {
            "type": "http",
            "direction": "out",
            "name": "$return"
        },
        {
            "type": "rabbitMQ",
            "name": "outputMessage",
            "queueName": "outputQueue",
            "connectionStringSetting": "connectionStringAppSetting",
            "direction": "out"
        }
    ]
}
```

In *_\_init_\_.py* können Sie eine Nachricht als Ausgabe in die Warteschlange schreiben, indem Sie einen Wert an die `set`-Methode übergeben.

```python
import azure.functions as func

def main(req: func.HttpRequest, msg: func.Out[str]) -> func.HttpResponse:
    input_msg = req.params.get('message')
    msg.set(input_msg)
    return 'OK'
```

# <a name="java"></a>[Java](#tab/java)

Das folgende Beispiel zeigt eine Java-Funktion, die eine Nachricht an die RabbitMQ-Warteschlange sendet, wenn sie durch einen TimerTrigger alle 5 Minuten ausgelöst wird.

```java
@FunctionName("RabbitMQOutputExample")
public void run(
@TimerTrigger(name = "keepAliveTrigger", schedule = "0 */5 * * * *") String timerInfo,
@RabbitMQOutput(connectionStringSetting = "rabbitMQ", queueName = "hello") OutputBinding<String> output,
final ExecutionContext context) {
    output.setValue("Some string");
}
```

---

## <a name="attributes-and-annotations"></a>Attribute und Anmerkungen

# <a name="c"></a>[C#](#tab/csharp)

Verwenden Sie in [C#-Klassenbibliotheken](functions-dotnet-class-library.md) das [RabbitMQAttribute](https://github.com/Azure/azure-functions-rabbitmq-extension/blob/dev/src/RabbitMQAttribute.cs).

Dies ist ein `RabbitMQAttribute`-Attribut in einer Methodensignatur:

```csharp
[FunctionName("RabbitMQOutput")]
public static async Task Run(
[RabbitMQTrigger("SourceQueue", ConnectionStringSetting = "TriggerConnectionString")] string rabbitMQEvent,
[RabbitMQ("DestinationQueue", ConnectionStringSetting = "OutputConnectionString")]IAsyncCollector<string> outputEvents,
ILogger log)
{
    ...
}
```

Ein vollständiges C#-Beispiel finden Sie [hier](#example).

# <a name="c-script"></a>[C#-Skript](#tab/csharp-script)

Attribute werden von C#-Skript nicht unterstützt.

# <a name="javascript"></a>[JavaScript](#tab/javascript)

Attribute werden von JavaScript nicht unterstützt.

# <a name="python"></a>[Python](#tab/python)

Attribute werden von Python nicht unterstützt.

# <a name="java"></a>[Java](#tab/java)

Mit der `RabbitMQOutput`-Anmerkung können Sie eine Funktion erstellen, die ausgeführt wird, wenn eine RabbitMQ-Nachricht gesendet wird. Zu den verfügbaren Konfigurationsoptionen zählen Warteschlangenname und Verbindungszeichenfolgenname. Weitere Details zu Parametern finden Sie in den [Java-RabbitMQOutput-Anmerkungen](https://github.com/Azure/azure-functions-rabbitmq-extension/blob/dev/binding-library/java/src/main/java/com/microsoft/azure/functions/rabbitmq/annotation/RabbitMQOutput.java).

Weitere Details finden Sie im [Beispiel](#example) für die Ausgabebindung.

---

## <a name="configuration"></a>Konfiguration

Die folgende Tabelle gibt Aufschluss über die Bindungskonfigurationseigenschaften, die Sie in der Datei *function.json* und im Attribut `RabbitMQ` festlegen:

|Eigenschaft von „function.json“ | Attributeigenschaft |BESCHREIBUNG|
|---------|---------|----------------------|
|**type** | – | Muss auf „RabbitMQ“ festgelegt werden.|
|**direction** | – | Muss auf „out“ festgelegt werden. |
|**name** | – | Der Name der Variablen, die die Warteschlange im Funktionscode darstellt. |
|**queueName**|**QueueName**| Name der Warteschlange, an die Nachrichten gesendet werden sollen. |
|**hostName**|**HostName**|(optional, wenn ConnectionStringSetting verwendet wird) <br>Hostname der Warteschlange (z. B.: 10.26.45.210)|
|**userNameSetting**|**UserNameSetting**|(optional, wenn ConnectionStringSetting verwendet wird) <br>Name für den Zugriff auf die Warteschlange |
|**passwordSetting**|**PasswordSetting**|(optional, wenn ConnectionStringSetting verwendet wird) <br>Kennwort für den Zugriff auf die Warteschlange|
|**connectionStringSetting**|**ConnectionStringSetting**|Name der App-Einstellung, die die Verbindungszeichenfolge für die Warteschlange mit der RabbitMQ-Nachricht enthält. Beachten Sie, dass der Trigger nicht funktioniert, wenn Sie die Verbindungszeichenfolge direkt und nicht über eine App-Einstellung in „local.settings.json“ angeben. (Beispiel: In *function.json*: connectionStringSetting: "rabbitMQConnection" <br> In *local.settings.json*: "rabbitMQConnection" : "<tatsächlicheVerbindungszeichenfolge>")|
|**port**|**Port**|Ruft den verwendeten Port ab oder legt ihn fest. Der Standardwert ist 0.|

## <a name="usage"></a>Verwendung

# <a name="c"></a>[C#](#tab/csharp)

Verwenden Sie die folgenden Parametertypen für die Ausgabebindung:

* `byte[]`: Wenn der Parameterwert NULL ist, sobald die Funktion beendet wird, erstellt Functions keine Nachricht.
* `string`: Wenn der Parameterwert NULL ist, sobald die Funktion beendet wird, erstellt Functions keine Nachricht.
* `POCO`: Wenn der Parameterwert nicht als C#-Objekt formatiert ist, wird eine Fehlermeldung ausgegeben. Ein vollständiges C#-Beispiel finden Sie [hier](#example).

Beim Arbeiten mit C# Funktionen:

* Asynchrone Funktionen benötigen anstelle eines `out`-Parameters einen Rückgabewert oder `IAsyncCollector`.

# <a name="c-script"></a>[C#-Skript](#tab/csharp-script)

Verwenden Sie die folgenden Parametertypen für die Ausgabebindung:

* `byte[]`: Wenn der Parameterwert NULL ist, sobald die Funktion beendet wird, erstellt Functions keine Nachricht.
* `string`: Wenn der Parameterwert NULL ist, sobald die Funktion beendet wird, erstellt Functions keine Nachricht.
* `POCO`: Wenn der Parameterwert nicht als C#-Objekt formatiert ist, wird eine Fehlermeldung ausgegeben.

Beim Arbeiten mit C#-Skriptfunktionen:

* Asynchrone Funktionen benötigen anstelle eines `out`-Parameters einen Rückgabewert oder `IAsyncCollector`.

# <a name="javascript"></a>[JavaScript](#tab/javascript)

Die RabbitMQ-Nachricht wird über eine Zeichenfolge gesendet.

# <a name="python"></a>[Python](#tab/python)

Die RabbitMQ-Nachricht wird über eine Zeichenfolge gesendet.

# <a name="java"></a>[Java](#tab/java)

Verwenden Sie die folgenden Parametertypen für die Ausgabebindung:

* `byte[]`: Wenn der Parameterwert NULL ist, sobald die Funktion beendet wird, erstellt Functions keine Nachricht.
* `string`: Wenn der Parameterwert NULL ist, sobald die Funktion beendet wird, erstellt Functions keine Nachricht.
* `POJO`: Wenn der Parameterwert nicht als Java-Objekt formatiert ist, wird eine Fehlermeldung ausgegeben.

---

## <a name="next-steps"></a>Nächste Schritte

- [Ausführen einer Funktion, wenn eine RabbitMQ-Nachricht erstellt wird (Trigger)](./functions-bindings-rabbitmq-trigger.md)
