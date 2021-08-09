---
title: Untergeordnete Orchestrierungen für Durable Functions – Azure
description: Es wird beschrieben, wie Sie Orchestrierungen aus Orchestrierungen in der Erweiterung „Durable Functions“ für Azure Functions aufrufen.
ms.topic: conceptual
ms.date: 11/03/2019
ms.author: azfuncdf
ms.openlocfilehash: b68f1235c07c5f3548dba1dbd6e46db91804fecc
ms.sourcegitcommit: a9f131fb59ac8dc2f7b5774de7aae9279d960d74
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2021
ms.locfileid: "110189430"
---
# <a name="sub-orchestrations-in-durable-functions-azure-functions"></a>Untergeordnete Orchestrierungen in Durable Functions (Azure Functions)

Orchestratorfunktionen können nicht nur Aktivitätsfunktionen aufrufen, sondern auch andere Orchestratorfunktionen. Beispielsweise können Sie aus einer kleineren Bibliothek mit Orchestratorfunktionen eine größere Orchestrierung erstellen. Oder Sie können mehrere Instanzen einer Orchestratorfunktion parallel ausführen.

Eine Orchestratorfunktion kann eine andere Orchestratorfunktion mithilfe der Methode `CallSubOrchestratorAsync` oder `CallSubOrchestratorWithRetryAsync` in .NET, der Methode `callSubOrchestrator` oder `callSubOrchestratorWithRetry` in JavaScript oder der Methode `call_sub_orchestrator` oder `call_sub_orchestrator_with_retry` in Python aufrufen. Der Artikel zur [Fehlerbehandlung und -kompensierung](durable-functions-error-handling.md#automatic-retry-on-failure) enthält weitere Informationen zur automatischen Wiederholung.

Untergeordnete Orchestratorfunktionen verhalten sich aus Sicht des Aufrufers genauso wie Aktivitätsfunktionen. Sie können einen Wert zurückgeben und eine Ausnahme auslösen, und die übergeordnete Orchestratorfunktion kann sie erwarten. 

> [!NOTE]
> Unterorchestrierungen werden derzeit in .NET, JavaScript und Python unterstützt.

## <a name="example"></a>Beispiel

Im folgenden Beispiel wird ein IoT-Szenario (Internet der Dinge) veranschaulicht, in dem mehrere Geräte bereitgestellt werden müssen. Die folgende Funktion stellt den Bereitstellungsworkflow dar, der für jedes Gerät ausgeführt werden muss:

# <a name="c"></a>[C#](#tab/csharp)

```csharp
public static async Task DeviceProvisioningOrchestration(
    [OrchestrationTrigger] IDurableOrchestrationContext context)
{
    string deviceId = context.GetInput<string>();

    // Step 1: Create an installation package in blob storage and return a SAS URL.
    Uri sasUrl = await context.CallActivityAsync<Uri>("CreateInstallationPackage", deviceId);

    // Step 2: Notify the device that the installation package is ready.
    await context.CallActivityAsync("SendPackageUrlToDevice", Tuple.Create(deviceId, sasUrl));

    // Step 3: Wait for the device to acknowledge that it has downloaded the new package.
    await context.WaitForExternalEvent<bool>("DownloadCompletedAck");

    // Step 4: ...
}
```

# <a name="javascript"></a>[JavaScript](#tab/javascript)

```javascript
const df = require("durable-functions");

module.exports = df.orchestrator(function*(context) {
    const deviceId = context.df.getInput();

    // Step 1: Create an installation package in blob storage and return a SAS URL.
    const sasUrl = yield context.df.callActivity("CreateInstallationPackage", deviceId);

    // Step 2: Notify the device that the installation package is ready.
    yield context.df.callActivity("SendPackageUrlToDevice", { id: deviceId, url: sasUrl });

    // Step 3: Wait for the device to acknowledge that it has downloaded the new package.
    yield context.df.waitForExternalEvent("DownloadCompletedAck");

    // Step 4: ...
});
```

# <a name="python"></a>[Python](#tab/python)

```python
import azure.functions as func
import azure.durable_functions as df

def orchestrator_function(context: df.DurableOrchestrationContext):
    device_id = context.get_input()

    # Step 1: Create an installation package in blob storage and return a SAS URL.
    sas_url = yield context.call_activity"CreateInstallationPackage", device_id)

    # Step 2: Notify the device that the installation package is ready.
    yield context.call_activity("SendPackageUrlToDevice", { "id": device_id, "url": sas_url })

    # Step 3: Wait for the device to acknowledge that it has downloaded the new package.
    yield context.call_activity("DownloadCompletedAck")

    # Step 4: ...
```

---

Diese Orchestratorfunktion kann für eine einmalige Gerätebereitstellung unverändert verwendet werden oder Teil einer größeren Orchestrierung sein. Im letzteren Fall kann die übergeordnete Orchestratorfunktion Instanzen von `DeviceProvisioningOrchestration` mit der `CallSubOrchestratorAsync`-API (.NET), der `callSubOrchestrator`-API (JavaScript) oder der `call_sub_orchestrator`-API (Python) planen.

In diesem Beispiel wird veranschaulicht, wie Sie mehrere Orchestratorfunktionen parallel ausführen.

# <a name="c"></a>[C#](#tab/csharp)

```csharp
[FunctionName("ProvisionNewDevices")]
public static async Task ProvisionNewDevices(
    [OrchestrationTrigger] IDurableOrchestrationContext context)
{
    string[] deviceIds = await context.CallActivityAsync<string[]>("GetNewDeviceIds");

    // Run multiple device provisioning flows in parallel
    var provisioningTasks = new List<Task>();
    foreach (string deviceId in deviceIds)
    {
        Task provisionTask = context.CallSubOrchestratorAsync("DeviceProvisioningOrchestration", deviceId);
        provisioningTasks.Add(provisionTask);
    }

    await Task.WhenAll(provisioningTasks);

    // ...
}
```

> [!NOTE]
> Die vorherigen C#-Beispiele gelten für Durable Functions 2.x. Für Durable Functions 1.x müssen Sie `DurableOrchestrationContext` anstelle von `IDurableOrchestrationContext` verwenden. Weitere Informationen zu den Unterschieden zwischen den Versionen finden Sie im Artikel [Durable Functions-Versionen](durable-functions-versions.md).

# <a name="javascript"></a>[JavaScript](#tab/javascript)

```javascript
const df = require("durable-functions");

module.exports = df.orchestrator(function*(context) {
    const deviceIds = yield context.df.callActivity("GetNewDeviceIds");

    // Run multiple device provisioning flows in parallel
    const provisioningTasks = [];
    var id = 0;
    for (const deviceId of deviceIds) {
        const child_id = context.df.instanceId+`:${id}`;
        const provisionTask = context.df.callSubOrchestrator("DeviceProvisioningOrchestration", deviceId, child_id);
        provisioningTasks.push(provisionTask);
        id++;
    }

    yield context.df.Task.all(provisioningTasks);

    // ...
});
```


# <a name="python"></a>[Python](#tab/python)

```Python
import azure.functions as func
import azure.durable_functions as df

def orchestrator_function(context: df.DurableOrchestrationContext):

    device_IDs = yield context.call_activity("GetNewDeviceIds")

    # Run multiple device provisioning flows in parallel
    provisioning_tasks = []
    id_ = 0
    for device_id in device_IDs:
        child_id = context.instance_id + ":" + id_
        provision_task = context.call_sub_orchestrator("DeviceProvisioningOrchestration", device_id, child_id)
        provisioning_tasks.append(provision_task)
        id_ += 1

    yield context.task_all(provisioning_tasks)

    # ...
```
---

> [!NOTE]
> Unterorchestrierungen müssen in derselben Funktions-App wie die übergeordnete Orchestrierung definiert werden. Wenn Sie Orchestrierungen in einer anderen Funktionsanwendung aufrufen und auf diese warten müssen, sollten Sie die integrierte Unterstützung für HTTP-APIs und das HTTP 202- Abfrageconsumermuster verwenden. Weitere Informationen finden Sie im Thema [HTTP-Funktionen](durable-functions-http-features.md).

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Erfahren Sie, wie ein benutzerdefiniertee Orchestrierungsstatus festgelegt wird](durable-functions-custom-orchestration-status.md)
