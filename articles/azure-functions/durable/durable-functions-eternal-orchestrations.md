---
title: Endlose Orchestrierungen in Durable Functions – Azure
description: Es wird beschrieben, wie Sie endlose Orchestrierungen implementieren, indem Sie die Erweiterung „Durable Functions“ für Azure Functions verwenden.
author: cgillum
ms.topic: conceptual
ms.date: 11/02/2019
ms.author: azfuncdf
ms.openlocfilehash: d55e08fecbd1338284607ac59fe354c6fa8cb1ea
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "80478822"
---
# <a name="eternal-orchestrations-in-durable-functions-azure-functions"></a>Endlose Orchestrierungen in Durable Functions (Azure Functions)

*Endlose Orchestrierungen* sind Orchestratorfunktionen, die niemals enden. Sie sind nützlich, wenn Sie [Durable Functions](durable-functions-overview.md) für Aggregatoren und Szenarien einsetzen möchten, für die eine Endlosschleife erforderlich ist.

## <a name="orchestration-history"></a>Orchestrierungsverlauf

Wie im Thema [Orchestrierungsverlauf](durable-functions-orchestrations.md#orchestration-history) beschrieben wird, verfolgt das Durable Task-Framework den Verlauf jeder einzelnen Funktionsorchestrierung nach. Dieser Verlauf nimmt ständig an Umfang zu, solange die Orchestratorfunktion neue Arbeit plant. Wenn die Orchestratorfunktion in eine Endlosschleife eintritt und fortlaufend Arbeit plant, kann dieser Verlauf eine kritische Größe erreichen und zu erheblichen Leistungsproblemen führen. Das Konzept der *endlosen Orchestrierung* wurde entworfen, um diese Art von Problemen für Anwendungen zu lösen, die Endlosschleifen benötigen.

## <a name="resetting-and-restarting"></a>Zurücksetzen und Neustarten

Anstatt Endlosschleifen zu verwenden, setzen Orchestratorfunktionen ihren Status zurück, indem sie die Methode `ContinueAsNew` (.NET) oder `continueAsNew` (JavaScript) der [Orchestrierungstriggerbindung](durable-functions-bindings.md#orchestration-trigger) aufrufen. Bei dieser Methode wird ein einzelner Parameter verwendet, der per JSON serialisierbar ist. Er wird zur neuen Eingabe für die nächste Generierung einer Orchestratorfunktion.

Wenn `ContinueAsNew` aufgerufen wird, reiht die Instanz vor dem Beenden eine Nachricht an sich selbst in die Warteschlange ein. Die Nachricht bewirkt, dass die Instanz mit dem neuen Eingabewert neu gestartet wird. Es wird dieselbe Instanz-ID beibehalten, aber der Verlauf der Orchestratorfunktion wird praktisch abgeschnitten.

> [!NOTE]
> Das Durable Task Framework behält dieselbe Instanz-ID bei, aber intern wird eine neue *Ausführungs-ID* für die Orchestratorfunktion erstellt, die mit `ContinueAsNew` zurückgesetzt wird. Diese Ausführungs-ID wird im Allgemeinen nicht extern verfügbar gemacht, aber es kann hilfreich sein, sie zu kennen, wenn das Debuggen der Orchestrierungsausführung erfolgen soll.

## <a name="periodic-work-example"></a>Beispiel für regelmäßige Arbeit

Ein Anwendungsfall für endlose Orchestrierungen ist Code, der unendlich lange regelmäßig Arbeitsschritte ausführen muss.

# <a name="c"></a>[C#](#tab/csharp)

```csharp
[FunctionName("Periodic_Cleanup_Loop")]
public static async Task Run(
    [OrchestrationTrigger] IDurableOrchestrationContext context)
{
    await context.CallActivityAsync("DoCleanup", null);

    // sleep for one hour between cleanups
    DateTime nextCleanup = context.CurrentUtcDateTime.AddHours(1);
    await context.CreateTimer(nextCleanup, CancellationToken.None);

    context.ContinueAsNew(null);
}
```

> [!NOTE]
> Das vorherige C#-Beispiel gilt für Durable Functions 2.x. Für Durable Functions 1.x müssen Sie `DurableOrchestrationContext` anstelle von `IDurableOrchestrationContext` verwenden. Weitere Informationen zu den Unterschieden zwischen den Versionen finden Sie im Artikel [Durable Functions-Versionen](durable-functions-versions.md).

# <a name="javascript"></a>[JavaScript](#tab/javascript)

```javascript
const df = require("durable-functions");
const moment = require("moment");

module.exports = df.orchestrator(function*(context) {
    yield context.df.callActivity("DoCleanup");

    // sleep for one hour between cleanups
    const nextCleanup = moment.utc(context.df.currentUtcDateTime).add(1, "h");
    yield context.df.createTimer(nextCleanup.toDate());

    context.df.continueAsNew(undefined);
});
```

---

Der Unterschied zwischen diesem Beispiel und einer per Timer ausgelösten Funktion besteht darin, dass Bereinigungstriggerzeiten hier nicht auf einem Zeitplan basieren. Beispiel: Ein CRON-Zeitplan, für den stündlich eine Funktion ausgeführt wird (um 1:00, 2:00, 3:00 Uhr usw.), können ggf. Probleme mit Überlappungen auftreten. Wenn die Bereinigung in diesem Beispiel aber 30 Minuten dauert, wird sie um 1:00, 2:30, 4:00 Uhr usw. geplant, sodass es nicht zu Überlappungen kommt.

## <a name="starting-an-eternal-orchestration"></a>Starten einer endlosen Orchestrierung

Verwenden Sie die Methode `StartNewAsync` (.NET) oder `startNew` (JavaScript), um eine ewige Orchestrierung zu starten, genauso wie bei jeder anderen Orchestrierungsfunktion.  

> [!NOTE]
> Wenn Sie sicherstellen müssen, dass eine ewige Singleton-Orchestrierung ausgeführt wird, ist es wichtig, dass Sie beim Starten der Orchestrierung dieselbe `id` der beibehalten. Weitere Informationen finden Sie unter [Instanzverwaltung](durable-functions-instance-management.md).

# <a name="c"></a>[C#](#tab/csharp)

```csharp
[FunctionName("Trigger_Eternal_Orchestration")]
public static async Task<HttpResponseMessage> OrchestrationTrigger(
    [HttpTrigger(AuthorizationLevel.Function, "post", Route = null)] HttpRequestMessage request,
    [DurableClient] IDurableOrchestrationClient client)
{
    string instanceId = "StaticId";

    await client.StartNewAsync("Periodic_Cleanup_Loop", instanceId); 
    return client.CreateCheckStatusResponse(request, instanceId);
}
```

> [!NOTE]
> Der vorherige Code ist für Durable Functions 2.x vorgesehen. Für Durable Functions 1.x müssen Sie das `OrchestrationClient`-Attribut anstelle des `DurableClient`-Attributs verwenden, und Sie müssen den `DurableOrchestrationClient`-Parametertyp anstelle von `IDurableOrchestrationClient` verwenden. Weitere Informationen zu den Unterschieden zwischen den Versionen finden Sie im Artikel [Durable Functions-Versionen](durable-functions-versions.md).

# <a name="javascript"></a>[JavaScript](#tab/javascript)

```javascript
const df = require("durable-functions");

module.exports = async function (context, req) {
    const client = df.getClient(context);
    const instanceId = "StaticId";
    
    // null is used as the input, since there is no input in "Periodic_Cleanup_Loop".
    await client.startNew("Periodic_Cleanup_Loop", instanceId, null);

    context.log(`Started orchestration with ID = '${instanceId}'.`);
    return client.createCheckStatusResponse(context.bindingData.req, instanceId);
};
```

---

## <a name="exit-from-an-eternal-orchestration"></a>Beenden einer endlosen Orchestrierung

Wenn eine Orchestratorfunktion schließlich abgeschlossen werden soll, müssen Sie lediglich *nicht*`ContinueAsNew` aufrufen und die Funktion enden lassen.

Wenn sich eine Orchestratorfunktion in einer Endlosschleife befindet und beendet werden muss, verwenden Sie die Methode `TerminateAsync` (.NET) oder `terminate` (JavaScript) der [Orchestrierungsclientbindung](durable-functions-bindings.md#orchestration-client), um die Funktion zu beenden. Weitere Informationen finden Sie unter [Instanzverwaltung](durable-functions-instance-management.md).

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Singleton-Orchestratoren in Durable Functions (Azure Functions)](durable-functions-singletons.md)
