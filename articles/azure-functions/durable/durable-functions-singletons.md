---
title: Singletons für Durable Functions – Azure
description: Es wird beschrieben, wie Sie Singletons in der Erweiterung „Durable Functions“ für Azure Functions verwenden.
author: cgillum
ms.topic: conceptual
ms.date: 11/03/2019
ms.author: azfuncdf
ms.openlocfilehash: 4eff7c4c91ed664fcf1f4fc7a8be2d43d24e5c6b
ms.sourcegitcommit: 2a2af81e79a47510e7dea2efb9a8efb616da41f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/17/2020
ms.locfileid: "76262808"
---
# <a name="singleton-orchestrators-in-durable-functions-azure-functions"></a>Singleton-Orchestratoren in Durable Functions (Azure Functions)

Für Hintergrundaufträge müssen Sie häufig sicherstellen, dass nur jeweils eine Instanz eines bestimmten Orchestrators ausgeführt wird. Sie können diese Art des Verhaltens von Singletons in [Durable Functions](durable-functions-overview.md) sicherstellen, indem einem Orchestrator bei der Erstellung eine spezifische Instanz-ID zugewiesen wird.

## <a name="singleton-example"></a>Singleton-Beispiel

Im folgenden Beispiel wird eine HTTP-Triggerfunktion veranschaulicht, mit der eine Orchestrierung für einen Singleton-Hintergrundauftrag erstellt wird. Der Code stellt sicher, dass für eine angegebene Instanz-ID nur eine Instanz vorhanden ist.

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

```cs
[FunctionName("HttpStartSingle")]
public static async Task<HttpResponseMessage> RunSingle(
    [HttpTrigger(AuthorizationLevel.Function, methods: "post", Route = "orchestrators/{functionName}/{instanceId}")] HttpRequestMessage req,
    [DurableClient] IDurableOrchestrationClient starter,
    string functionName,
    string instanceId,
    ILogger log)
{
    // Check if an instance with the specified ID already exists.
    var existingInstance = await starter.GetStatusAsync(instanceId);
    if (existingInstance == null)
    {
        // An instance with the specified ID doesn't exist, create one.
        dynamic eventData = await req.Content.ReadAsAsync<object>();
        await starter.StartNewAsync(functionName, instanceId, eventData);
        log.LogInformation($"Started orchestration with ID = '{instanceId}'.");
        return starter.CreateCheckStatusResponse(req, instanceId);
    }
    else
    {
        // An instance with the specified ID exists, don't create one.
        return req.CreateErrorResponse(
            HttpStatusCode.Conflict,
            $"An instance with ID '{instanceId}' already exists.");
    }
}
```

> [!NOTE]
> Der vorherige C#-Code ist für Durable Functions 2.x vorgesehen. Für Durable Functions 1.x müssen Sie das `OrchestrationClient`-Attribut anstelle des `DurableClient`-Attributs verwenden, und Sie müssen den `DurableOrchestrationClient`-Parametertyp anstelle von `IDurableOrchestrationClient` verwenden. Weitere Informationen zu den Unterschieden zwischen den Versionen finden Sie im Artikel [Durable Functions-Versionen](durable-functions-versions.md).

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

**function.json**

```json
{
  "bindings": [
    {
      "authLevel": "function",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in",
      "route": "orchestrators/{functionName}/{instanceId}",
      "methods": ["post"]
    },
    {
      "name": "starter",
      "type": "orchestrationClient",
      "direction": "in"
    },
    {
      "name": "$return",
      "type": "http",
      "direction": "out"
    }
  ]
}
```

**index.js**

```javascript
const df = require("durable-functions");

module.exports = async function(context, req) {
    const client = df.getClient(context);

    const instanceId = req.params.instanceId;
    const functionName = req.params.functionName;

    // Check if an instance with the specified ID already exists.
    const existingInstance = await client.getStatus(instanceId);
    if (!existingInstance) {
        // An instance with the specified ID doesn't exist, create one.
        const eventData = req.body;
        await client.startNew(functionName, instanceId, eventData);
        context.log(`Started orchestration with ID = '${instanceId}'.`);
        return client.createCheckStatusResponse(req, instanceId);
    } else {
        // An instance with the specified ID exists, don't create one.
        return {
            status: 409,
            body: `An instance with ID '${instanceId}' already exists.`,
        };
    }
};
```

---

Standardmäßig handelt es sich bei Instanz-IDs um zufällig generierte GUIDs. Im vorherigen Beispiel wird die Instanz-ID jedoch in den Routendaten von der URL übergeben. Der Code ruft `GetStatusAsync` (C#) oder `getStatus` (JavaScript) auf, um zu überprüfen, ob bereits eine Instanz mit der angegebenen ID ausgeführt wird. Wenn eine solche Instanz nicht ausgeführt wird, wird eine neue Instanz mit dieser ID erstellt.

> [!NOTE]
> In diesem Beispiel gibt es eine potenzielle Racebedingung. Wenn zwei Instanzen von **HttpStartSingle** gleichzeitig ausgeführt werden, sind beide Funktionsaufrufe erfolgreich, aber nur eine Orchestrierungsinstanz wird gestartet. Je nach Ihren Anforderungen kann dies unerwünschte Nebenwirkungen haben. Aus diesem Grund muss sichergestellt werden, dass diese Triggerfunktion nicht von zwei Anforderungen gleichzeitig ausgeführt werden kann.

Die Implementierungsdetails der Orchestratorfunktion sind hier nicht wichtig. Es kann eine reguläre Orchestratorfunktion sein, die gestartet und abgeschlossen wird, oder es kann eine Orchestratorfunktion mit unendlicher Ausführung sein (also eine [endlose Orchestrierung](durable-functions-eternal-orchestrations.md)). Der wichtigste Punkt hierbei ist, dass nur jeweils eine Instanz ausgeführt wird.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Erfahren Sie mehr über die nativen HTTP-Funktionen von Orchestrierungen](durable-functions-http-features.md)
