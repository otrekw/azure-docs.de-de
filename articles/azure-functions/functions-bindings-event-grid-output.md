---
title: Azure Event Grid-Ausgabebindung für Azure Functions
description: Erfahren Sie, wie Sie ein Event Grid-Ereignis in Azure Functions senden.
author: craigshoemaker
ms.topic: reference
ms.date: 02/14/2020
ms.author: cshoe
ms.custom: fasttrack-edit
ms.openlocfilehash: e7a2611312ffc33703dd5cc9d0a2d7142ddb0532
ms.sourcegitcommit: f97f086936f2c53f439e12ccace066fca53e8dc3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/15/2020
ms.locfileid: "77368927"
---
# <a name="azure-event-grid-output-binding-for-azure-functions"></a>Azure Event Grid-Ausgabebindung für Azure Functions

Verwenden Sie die Event Grid-Ausgabebindung, um Ereignisse in ein benutzerdefiniertes Thema zu schreiben. Sie müssen einen gültigen [Zugriffsschlüssel für das benutzerdefinierte Thema](../event-grid/security-authentication.md#custom-topic-publishing) besitzen.

Informationen zu Setup- und Konfigurationsdetails finden Sie in der [Übersicht](./functions-bindings-event-grid.md).

> [!NOTE]
> Die Event Grid-Ausgabebindung unterstützt keine SAS-Token (Shared Access Signature). Sie müssen den Zugriffsschlüssel des Themas verwenden.

> [!IMPORTANT]
> Die Event Grid-Ausgabebindung ist nur für Functions 2. x und höher verfügbar.

## <a name="example"></a>Beispiel

# <a name="c"></a>[C#](#tab/csharp)

Das folgende Beispiel zeigt eine [C#-Funktion](functions-dotnet-class-library.md), die eine Nachricht in ein benutzerdefiniertes Event Grid-Thema schreibt. Dabei wird der Rückgabewert der Methode als Ausgabe verwendet:

```csharp
[FunctionName("EventGridOutput")]
[return: EventGrid(TopicEndpointUri = "MyEventGridTopicUriSetting", TopicKeySetting = "MyEventGridTopicKeySetting")]
public static EventGridEvent Run([TimerTrigger("0 */5 * * * *")] TimerInfo myTimer, ILogger log)
{
    return new EventGridEvent("message-id", "subject-name", "event-data", "event-type", DateTime.UtcNow, "1.0");
}
```

Das folgende Beispiel zeigt, wie Sie die `IAsyncCollector`-Schnittstelle verwenden, um einen Nachrichtenbatch zu senden.

```csharp
[FunctionName("EventGridAsyncOutput")]
public static async Task Run(
    [TimerTrigger("0 */5 * * * *")] TimerInfo myTimer,
    [EventGrid(TopicEndpointUri = "MyEventGridTopicUriSetting", TopicKeySetting = "MyEventGridTopicKeySetting")]IAsyncCollector<EventGridEvent> outputEvents,
    ILogger log)
{
    for (var i = 0; i < 3; i++)
    {
        var myEvent = new EventGridEvent("message-id-" + i, "subject-name", "event-data", "event-type", DateTime.UtcNow, "1.0");
        await outputEvents.AddAsync(myEvent);
    }
}
```

# <a name="c-script"></a>[C#-Skript](#tab/csharp-script)

Das folgende Beispiel zeigt die Event Grid-Ausgabebindungsdaten in der Datei *function.json*.

```json
{
    "type": "eventGrid",
    "name": "outputEvent",
    "topicEndpointUri": "MyEventGridTopicUriSetting",
    "topicKeySetting": "MyEventGridTopicKeySetting",
    "direction": "out"
}
```

Der folgende C#-Skriptcode erstellt ein einzelnes Ereignis:

```cs
#r "Microsoft.Azure.EventGrid"
using System;
using Microsoft.Azure.EventGrid.Models;
using Microsoft.Extensions.Logging;

public static void Run(TimerInfo myTimer, out EventGridEvent outputEvent, ILogger log)
{
    outputEvent = new EventGridEvent("message-id", "subject-name", "event-data", "event-type", DateTime.UtcNow, "1.0");
}
```

Der folgende C#-Skriptcode erstellt mehrere Ereignisse:

```cs
#r "Microsoft.Azure.EventGrid"
using System;
using Microsoft.Azure.EventGrid.Models;
using Microsoft.Extensions.Logging;

public static void Run(TimerInfo myTimer, ICollector<EventGridEvent> outputEvent, ILogger log)
{
    outputEvent.Add(new EventGridEvent("message-id-1", "subject-name", "event-data", "event-type", DateTime.UtcNow, "1.0"));
    outputEvent.Add(new EventGridEvent("message-id-2", "subject-name", "event-data", "event-type", DateTime.UtcNow, "1.0"));
}
```

# <a name="javascript"></a>[JavaScript](#tab/javascript)

Das folgende Beispiel zeigt die Event Grid-Ausgabebindungsdaten in der Datei *function.json*.

```json
{
    "type": "eventGrid",
    "name": "outputEvent",
    "topicEndpointUri": "MyEventGridTopicUriSetting",
    "topicKeySetting": "MyEventGridTopicKeySetting",
    "direction": "out"
}
```

Der folgende JavaScript-Code erstellt ein einzelnes Ereignis:

```javascript
module.exports = async function (context, myTimer) {
    var timeStamp = new Date().toISOString();

    context.bindings.outputEvent = {
        id: 'message-id',
        subject: 'subject-name',
        dataVersion: '1.0',
        eventType: 'event-type',
        data: "event-data",
        eventTime: timeStamp
    };
    context.done();
};
```

Der folgende JavaScript-Code erstellt mehrere Ereignisse:

```javascript
module.exports = function(context) {
    var timeStamp = new Date().toISOString();

    context.bindings.outputEvent = [];

    context.bindings.outputEvent.push({
        id: 'message-id-1',
        subject: 'subject-name',
        dataVersion: '1.0',
        eventType: 'event-type',
        data: "event-data",
        eventTime: timeStamp
    });
    context.bindings.outputEvent.push({
        id: 'message-id-2',
        subject: 'subject-name',
        dataVersion: '1.0',
        eventType: 'event-type',
        data: "event-data",
        eventTime: timeStamp
    });
    context.done();
};
```

# <a name="python"></a>[Python](#tab/python)

Die Event Grid-Ausgabebindung ist nicht für Python verfügbar.

# <a name="java"></a>[Java](#tab/java)

Die Event Grid-Ausgabebindung ist nicht für Java verfügbar.

---

## <a name="attributes-and-annotations"></a>Attribute und Anmerkungen

# <a name="c"></a>[C#](#tab/csharp)

Verwenden Sie für [C#-Klassenbibliotheken](functions-dotnet-class-library.md) das Attribut [EventGridAttribute](https://github.com/Azure/azure-functions-eventgrid-extension/blob/dev/src/EventGridExtension/OutputBinding/EventGridAttribute.cs).

Der Konstruktor des Attributs nimmt den Namen einer App-Einstellung an, die den Namen des benutzerdefinierten Themas enthält, sowie den Namen einer App-Einstellung, die den Themenschlüssel enthält. Weitere Informationen zu diesen Einstellungen finden Sie unter [Ausgabe: Konfiguration](#configuration). Dieses Beispiel zeigt ein Attribut `EventGrid`:

```csharp
[FunctionName("EventGridOutput")]
[return: EventGrid(TopicEndpointUri = "MyEventGridTopicUriSetting", TopicKeySetting = "MyEventGridTopicKeySetting")]
public static string Run([TimerTrigger("0 */5 * * * *")] TimerInfo myTimer, ILogger log)
{
    ...
}
```

Ein vollständiges Beispiel finden Sie unter [Beispiel](#example).

# <a name="c-script"></a>[C#-Skript](#tab/csharp-script)

Attribute werden von C#-Skript nicht unterstützt.

# <a name="javascript"></a>[JavaScript](#tab/javascript)

Attribute werden von JavaScript nicht unterstützt.

# <a name="python"></a>[Python](#tab/python)

Die Event Grid-Ausgabebindung ist nicht für Python verfügbar.

# <a name="java"></a>[Java](#tab/java)

Die Event Grid-Ausgabebindung ist nicht für Java verfügbar.

---

## <a name="configuration"></a>Konfiguration

Die folgende Tabelle gibt Aufschluss über die Bindungskonfigurationseigenschaften, die Sie in der Datei *function.json* und im Attribut `EventGrid` festlegen:

|Eigenschaft von „function.json“ | Attributeigenschaft |BESCHREIBUNG|
|---------|---------|----------------------|
|**type** | – | Muss auf „eventGrid“ festgelegt werden. |
|**direction** | – | Muss auf „out“ festgelegt werden. Dieser Parameter wird automatisch festgelegt, wenn Sie die Bindung im Azure Portal erstellen. |
|**name** | – | Der Variablenname, der in Funktionscode verwendet wird, der das Ereignis darstellt. |
|**topicEndpointUri** |**TopicEndpointUri** | Der Name einer App-Einstellung, die den URI für das benutzerdefinierte Thema enthält, z. B. `MyTopicEndpointUri`. |
|**topicKeySetting** |**TopicKeySetting** | Der Name einer App-Einstellung, die einen Zugriffsschlüssel für das benutzerdefinierte Thema enthält. |

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

> [!IMPORTANT]
> Stellen Sie sicher, dass Sie den Wert der Konfigurationseigenschaft `TopicEndpointUri` auf den Namen einer App-Einstellung festlegen, die den URI des benutzerdefinierten Themas enthält. Geben Sie den URI des benutzerdefinierten Themas nicht direkt in dieser Eigenschaft an.

## <a name="usage"></a>Verwendung

# <a name="c"></a>[C#](#tab/csharp)

Senden Sie Nachrichten mithilfe eines Methodenparameters wie `out EventGridEvent paramName`. Um mehrere Nachrichten zu schreiben, können Sie `ICollector<EventGridEvent>` oder `IAsyncCollector<EventGridEvent>` anstelle von `out EventGridEvent` verwenden.

# <a name="c-script"></a>[C#-Skript](#tab/csharp-script)

Senden Sie Nachrichten mithilfe eines Methodenparameters wie `out EventGridEvent paramName`. In C#-Skripts ist `paramName` der Wert, der in der Eigenschaft `name` von *function.json* angegeben ist. Um mehrere Nachrichten zu schreiben, können Sie `ICollector<EventGridEvent>` oder `IAsyncCollector<EventGridEvent>` anstelle von `out EventGridEvent` verwenden.

# <a name="javascript"></a>[JavaScript](#tab/javascript)

Greifen Sie mithilfe von `context.bindings.<name>` auf das Ausgabeereignis zu, wobei `<name>` der in der Eigenschaft `name` von *function.json* angegebene Wert ist.

# <a name="python"></a>[Python](#tab/python)

Die Event Grid-Ausgabebindung ist nicht für Python verfügbar.

# <a name="java"></a>[Java](#tab/java)

Die Event Grid-Ausgabebindung ist nicht für Java verfügbar.

---

## <a name="next-steps"></a>Nächste Schritte

* [Versenden eines Event Grid-Ereignisses](./functions-bindings-event-grid-trigger.md)
