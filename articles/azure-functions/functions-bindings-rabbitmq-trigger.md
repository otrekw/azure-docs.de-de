---
title: RabbitMQ-Trigger für Azure Functions
description: Erfahren Sie, wie Sie eine Azure-Funktion ausführen, wenn eine RabbitMQ-Nachricht erstellt wird.
author: cachai2
ms.assetid: ''
ms.topic: reference
ms.date: 12/13/2020
ms.author: cachai
ms.custom: ''
ms.openlocfilehash: e7095c08c385457bddf6d70d345c4f47073b4adb
ms.sourcegitcommit: 2ba6303e1ac24287762caea9cd1603848331dd7a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/15/2020
ms.locfileid: "97505666"
---
# <a name="rabbitmq-trigger-for-azure-functions-overview"></a>Übersicht über RabbitMQ-Trigger für Azure Functions

> [!NOTE]
> RabbitMQ-Bindungen werden nur mit **Windows Premium**-Plänen vollständig unterstützt. Im Verbrauchstarif und unter Linux werden sie derzeit nicht unterstützt.

Verwenden Sie den RabbitMQ-Trigger, um auf Nachrichten aus einer RabbitMQ-Warteschlange zu antworten.

Informationen zu Setup- und Konfigurationsdetails finden Sie in der [Übersicht](functions-bindings-rabbitmq.md).

## <a name="example"></a>Beispiel

# <a name="c"></a>[C#](#tab/csharp)

Das folgende Beispiel zeigt eine [C#-Funktion](functions-dotnet-class-library.md), die die RabbitMQ-Nachricht als [RabbitMQ-Ereignis](https://www.rabbitmq.com/releases/rabbitmq-dotnet-client/v3.2.2/rabbitmq-dotnet-client-3.2.2-client-htmldoc/html/type-RabbitMQ.Client.Events.BasicDeliverEventArgs.html) liest und protokolliert:

```cs
[FunctionName("RabbitMQTriggerCSharp")]
public static void RabbitMQTrigger_BasicDeliverEventArgs(
    [RabbitMQTrigger("queue", ConnectionStringSetting = "rabbitMQConnection")] BasicDeliverEventArgs args,
    ILogger logger
    )
{
    logger.LogInformation($"C# RabbitMQ queue trigger function processed message: {Encoding.UTF8.GetString(args.Body)}");
}
```

Im folgenden Beispiel wird veranschaulicht, wie die Nachricht als POCO gelesen wird.

```cs
public class TestClass
{
    public string x { get; set; }
}

[FunctionName("RabbitMQTriggerCSharp")]
public static void RabbitMQTrigger_BasicDeliverEventArgs(
    [RabbitMQTrigger("queue", ConnectionStringSetting = "rabbitMQConnection")] TestClass pocObj,
    ILogger logger
    )
{
    logger.LogInformation($"C# RabbitMQ queue trigger function processed message: {Encoding.UTF8.GetString(pocObj)}");
}
```

Wie bei JSON-Objekten tritt ein Fehler auf, wenn die Nachricht nicht ordnungsgemäß als C#-Objekt formatiert ist. Wenn sie richtig formatiert ist, wird sie an die Variable „pocObj“ gebunden, die für beliebige Aktionen verwendet werden kann.

# <a name="c-script"></a>[C#-Skript](#tab/csharp-script)

Das folgende Beispiel zeigt eine RabbitMQ-Triggerbindung in einer Datei *function.json* sowie eine [C#-Skriptfunktion](functions-reference-csharp.md), die diese Bindung verwendet. Die Funktion liest und protokolliert die RabbitMQ-Nachricht.

Bindungsdaten in der Datei *function.json*:

```json
{
    "bindings": [
        {
            "name": "myQueueItem",
            "type": "rabbitMQTrigger",
            "direction": "in",
            "queueName": "queue",
            "connectionStringSetting": "rabbitMQConnection"
        }
    ]
}
```

Der C#-Skriptcode sieht wie folgt aus:

```csx
using System;

public static void Run(string myQueueItem, ILogger log)
{
    log.LogInformation($"C# Script RabbitMQ trigger function processed: {myQueueItem}");
}
```

# <a name="javascript"></a>[JavaScript](#tab/javascript)

Das folgende Beispiel zeigt eine RabbitMQ-Triggerbindung in einer Datei *function.json* sowie eine [JavaScript-Skriptfunktion](functions-reference-node.md), die diese Bindung verwendet. Die Funktion liest und protokolliert eine RabbitMQ-Nachricht.

Bindungsdaten in der Datei *function.json*:

```json
{
    "bindings": [
        {
            "name": "myQueueItem",
            "type": "rabbitMQTrigger",
            "direction": "in",
            "queueName": "queue",
            "connectionStringSetting": "rabbitMQConnection"
        }
    ]
}
```

Der JavaScript-Skriptcode sieht wie folgt aus:

```javascript
module.exports = async function (context, myQueueItem) {
    context.log('JavaScript RabbitMQ trigger function processed work item', myQueueItem);
};
```

# <a name="python"></a>[Python](#tab/python)

Das folgende Beispiel zeigt, wie Sie eine RabbitMQ-Warteschlangennachricht mittels Trigger lesen.

Eine RabbitMQ-Bindung wird in *function.json* definiert, indem *type* auf `RabbitMQTrigger` festgelegt wird.

```json
{
    "scriptFile": "__init__.py",
    "bindings": [
        {
            "name": "myQueueItem",
            "type": "rabbitMQTrigger",
            "direction": "in",
            "queueName": "",
            "connectionStringSetting": ""
        }
    ]
}
```

Der Code in *_\_init_\_.py* deklariert einen Parameter als `func.RabbitMQMessage`. Dadurch können Sie die Nachricht in Ihrer Funktion lesen.

```python
import logging
import azure.functions as func

def main(myQueueItem) -> None:
    logging.info('Python RabbitMQ trigger function processed a queue item: %s', myQueueItem)
```

# <a name="java"></a>[Java](#tab/java)

Die folgende Java-Funktion verwendet die `@RabbitMQTrigger`-Anmerkung aus den [Java-Typen für RabbitMQ](https://mvnrepository.com/artifact/com.microsoft.azure.functions/azure-functions-java-library-rabbitmq), um die Konfiguration für einen RabbitMQ-Warteschlangentrigger zu beschreiben. Die Funktion ruft die Nachricht aus der Warteschlange ab und fügt sie den Protokollen hinzu.

```java
@FunctionName("RabbitMQTriggerExample")
public void run(
    @RabbitMQTrigger(connectionStringSetting = "rabbitMQConnection", queueName = "queue") String input,
    final ExecutionContext context)
{
    context.getLogger().info("Java HTTP trigger processed a request." + input);
}
```

---

## <a name="attributes-and-annotations"></a>Attribute und Anmerkungen

# <a name="c"></a>[C#](#tab/csharp)

Verwenden Sie in [C#-Klassenbibliotheken](functions-dotnet-class-library.md) das [RabbitMQTrigger](https://github.com/Azure/azure-functions-rabbitmq-extension/blob/dev/src/Trigger/RabbitMQTriggerAttribute.cs)-Attribut.

Dies ist ein `RabbitMQTrigger`-Attribut in einer Methodensignatur:

```csharp
[FunctionName("RabbitMQTest")]
public static void RabbitMQTest([RabbitMQTrigger("queue")] string message, ILogger log)
{
    ...
}
```

Ein vollständiges Beispiel finden Sie unter „C#-Beispiel“.

# <a name="c-script"></a>[C#-Skript](#tab/csharp-script)

Attribute werden von C#-Skript nicht unterstützt.

# <a name="javascript"></a>[JavaScript](#tab/javascript)

Attribute werden von JavaScript nicht unterstützt.

# <a name="python"></a>[Python](#tab/python)

Attribute werden von Python nicht unterstützt.

# <a name="java"></a>[Java](#tab/java)

Mit der `RabbitMQTrigger`-Anmerkung können Sie eine Funktion erstellen, die ausgeführt wird, wenn eine RabbitMQ-Nachricht erstellt wird. Zu den verfügbaren Konfigurationsoptionen zählen Warteschlangenname und Verbindungszeichenfolgenname. Weitere Details zu Parametern finden Sie in den [Java-RabbitMQTrigger-Anmerkungen](https://github.com/Azure/azure-functions-rabbitmq-extension/blob/dev/binding-library/java/src/main/java/com/microsoft/azure/functions/rabbitmq/annotation/RabbitMQTrigger.java).

Weitere Details finden Sie unter [Trigger – Beispiel](#example).

---

## <a name="configuration"></a>Konfiguration

Die folgende Tabelle gibt Aufschluss über die Bindungskonfigurationseigenschaften, die Sie in der Datei *function.json* und im Attribut `RabbitMQTrigger` festlegen:

|Eigenschaft von „function.json“ | Attributeigenschaft |BESCHREIBUNG|
|---------|---------|----------------------|
|**type** | – | Muss auf „RabbitMQTrigger“ festgelegt werden.|
|**direction** | – | Muss auf „in“ festgelegt werden.|
|**name** | – | Der Name der Variablen, die die Warteschlange im Funktionscode darstellt. |
|**queueName**|**QueueName**| Name der Warteschlange, von der Nachrichten empfangen werden. |
|**hostName**|**HostName**|(optional, wenn ConnectionStringSetting verwendet wird) <br>Hostname der Warteschlange (z. B.: 10.26.45.210)|
|**userNameSetting**|**UserNameSetting**|(optional, wenn ConnectionStringSetting verwendet wird) <br>Name für den Zugriff auf die Warteschlange |
|**passwordSetting**|**PasswordSetting**|(optional, wenn ConnectionStringSetting verwendet wird) <br>Kennwort für den Zugriff auf die Warteschlange|
|**connectionStringSetting**|**ConnectionStringSetting**|Name der App-Einstellung, die die Verbindungszeichenfolge für die Warteschlange mit der RabbitMQ-Nachricht enthält. Beachten Sie, dass der Trigger nicht funktioniert, wenn Sie die Verbindungszeichenfolge direkt und nicht über eine App-Einstellung in „local.settings.json“ angeben. (Beispiel: In *function.json*: connectionStringSetting: "rabbitMQConnection" <br> In *local.settings.json*: "rabbitMQConnection" : "<TatsächlicheVerbindungszeichenfolge>")|
|**port**|**Port**|Ruft den verwendeten Port ab oder legt ihn fest. Der Standardwert ist 0.|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="usage"></a>Verwendung

# <a name="c"></a>[C#](#tab/csharp)

Die folgenden Parametertypen sind für die Nachricht verfügbar:

* [RabbitMQ-Ereignis:](https://www.rabbitmq.com/releases/rabbitmq-dotnet-client/v3.2.2/rabbitmq-dotnet-client-3.2.2-client-htmldoc/html/type-RabbitMQ.Client.Events.BasicDeliverEventArgs.html) Standardformat für RabbitMQ-Nachrichten
  * `byte[]`: über die Body-Eigenschaft des RabbitMQ-Ereignisses
* `string`: Es handelt sich bei der Nachricht um Text.
* `An object serializable as JSON`: Die Nachricht wird als gültige JSON-Zeichenfolge übermittelt.
* `POCO`: Die Nachricht ist als C#-Objekt formatiert. Ein vollständiges Beispiel finden Sie unter [C#-Beispiel](#example).

# <a name="c-script"></a>[C#-Skript](#tab/csharp-script)

Die folgenden Parametertypen sind für die Nachricht verfügbar:

* [RabbitMQ-Ereignis:](https://www.rabbitmq.com/releases/rabbitmq-dotnet-client/v3.2.2/rabbitmq-dotnet-client-3.2.2-client-htmldoc/html/type-RabbitMQ.Client.Events.BasicDeliverEventArgs.html) Standardformat für RabbitMQ-Nachrichten
  * `byte[]`: über die Body-Eigenschaft des RabbitMQ-Ereignisses
* `string`: Es handelt sich bei der Nachricht um Text.
* `An object serializable as JSON`: Die Nachricht wird als gültige JSON-Zeichenfolge übermittelt.
* `POCO`: Die Nachricht ist als C#-Objekt formatiert.

# <a name="javascript"></a>[JavaScript](#tab/javascript)

Die RabbitMQ-Nachricht wird als Zeichenfolge oder als JSON-Objekt an die Funktion übergeben.

# <a name="python"></a>[Python](#tab/python)

Die RabbitMQ-Nachricht wird als Zeichenfolge oder als JSON-Objekt an die Funktion übergeben.

# <a name="java"></a>[Java](#tab/java)

Weitere Informationen finden Sie unter den [Attributen und Anmerkungen](#attributes-and-annotations) für Java.

---

## <a name="dead-letter-queues"></a>Warteschlangen für unzustellbare Nachrichten
Warteschlangen für unzustellbare Nachrichten und Austauschvorgänge können nicht über den RabbitMQ-Trigger gesteuert oder konfiguriert werden.  Um Warteschlangen für unzustellbare Nachrichten zu verwenden, konfigurieren Sie vorab die Warteschlange, die vom Trigger in RabbitMQ verwendet wird. Weitere Informationen finden Sie in der [RabbitMQ-Dokumentation](https://www.rabbitmq.com/dlx.html).

## <a name="hostjson-settings"></a>Einstellungen für „host.json“

In diesem Abschnitt werden die verfügbaren globalen Konfigurationseinstellungen für diese Bindung in Version 2.x und höheren Versionen beschrieben. Die nachfolgende Beispieldatei *host.json* enthält nur die Einstellungen für diese Bindung. Weitere Informationen zu globalen Konfigurationseinstellungen finden Sie unter [host.json-Referenz für Azure Functions 2.x](functions-host-json.md).

```json
{
    "version": "2.0",
    "extensions": {
        "rabbitMQ": {
            "prefetchCount": 100,
            "queueName": "queue",
            "connectionString": "amqp://user:password@url:port",
            "port": 10
        }
    }
}
```

|Eigenschaft  |Standard | BESCHREIBUNG |
|---------|---------|---------|
|prefetchCount|30|Ruft die Anzahl der Nachrichten ab, die der Nachrichtenempfänger gleichzeitig anfordern und zwischenspeichern kann, oder legt sie fest.|
|queueName|–| Name der Warteschlange, von der Nachrichten empfangen werden. |
|connectionString|–|Name der App-Einstellung, die die Verbindungszeichenfolge für die Warteschlange mit der RabbitMQ-Nachricht enthält. Beachten Sie, dass der Trigger nicht funktioniert, wenn Sie die Verbindungszeichenfolge direkt und nicht über eine App-Einstellung in „local.settings.json“ angeben.|
|port|0|Die maximale Anzahl von Sitzungen, die parallel pro skalierter Instanz verarbeitet werden können.|

## <a name="local-testing"></a>Lokales Testen

> [!NOTE]
> „connectionString“ hat Vorrang vor „hostName“, „userName“ und „password“. Wenn diese Werte alle festgelegt wurden, setzt „connectionString“ die anderen beiden außer Kraft.

Wenn Sie lokale Tests ohne eine Verbindungszeichenfolge durchführen, sollten Sie die Einstellungen „hostName“ und ggf. „userName“ und „password“ im Abschnitt „rabbitMQ“ in der Datei *host.json* festlegen:

```json
{
    "version": "2.0",
    "extensions": {
        "rabbitMQ": {
            ...
            "hostName": "localhost",
            "username": "<your username>",
            "password": "<your password>"
        }
    }
}
```

|Eigenschaft  |Standard | Beschreibung |
|---------|---------|---------|
|hostName|–|(optional, wenn ConnectionStringSetting verwendet wird) <br>Hostname der Warteschlange (z. B.: 10.26.45.210)|
|userName|–|(optional, wenn ConnectionStringSetting verwendet wird) <br>Name für den Zugriff auf die Warteschlange |
|password|–|(optional, wenn ConnectionStringSetting verwendet wird) <br>Kennwort für den Zugriff auf die Warteschlange|

## <a name="monitoring-rabbitmq-endpoint"></a>Überwachen des RabbitMQ-Endpunkts
So überwachen Sie Warteschlangen und Austauschvorgänge für einen RabbitMQ-Endpunkt

* Aktivieren des [RabbitMQ-Verwaltungs-Plug-Ins](https://www.rabbitmq.com/management.html)
* Navigieren Sie zu http://{Knotenhostname}:15672, und melden Sie sich mit Ihrem Benutzernamen und Kennwort an.

## <a name="next-steps"></a>Nächste Schritte

- [Senden von RabbitMQ-Nachrichten mit Azure Functions (Ausgabebindung)](./functions-bindings-rabbitmq-output.md)
