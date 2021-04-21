---
title: Gleichzeitige Ausführung von Tasks zur optimalen Nutzung von Batch-Computeknoten
description: Steigern der Effizienz und Reduzieren von Kosten durch Verringerung der Anzahl von Computeknoten und parallele Ausführung von Aufgaben auf jedem Knoten in einem Azure Batch-Pool
ms.topic: how-to
ms.date: 04/13/2021
ms.custom: H1Hack27Feb2017, devx-track-csharp
ms.openlocfilehash: 81648f30a7a02f702dcb189aa7df27e5a82e2b07
ms.sourcegitcommit: aa00fecfa3ad1c26ab6f5502163a3246cfb99ec3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/14/2021
ms.locfileid: "107389297"
---
# <a name="run-tasks-concurrently-to-maximize-usage-of-batch-compute-nodes"></a>Gleichzeitige Ausführung von Tasks zur optimalen Nutzung von Batch-Computeknoten

Sie können die Ressourcenauslastung auf einer kleineren Zahl von Computeknoten in Ihrem Pool maximieren, indem Sie auf jedem Knoten mehrere Aufgaben gleichzeitig ausführen.

In einigen Fällen ist es zwar am besten, wenn alle Ressourcen eines Knotens dediziert für nur eine Aufgabe genutzt werden, aber für bestimmte Workloads kann ggf. eine Verringerung der Auftragsdauer und der Kosten erzielt werden, wenn diese Ressourcen für mehrere Aufgaben gemeinsam verwendet werden. Betrachten Sie die folgenden Szenarien:

- **Minimieren Sie den Datenübertragungsaufwand** für Aufgaben, bei denen Daten gemeinsam genutzt werden können. Sie können die Gebühren für die Datenübertragung deutlich reduzieren, indem Sie freigegebene Daten auf eine kleinere Zahl von Knoten kopieren und anschließend Aufgaben parallel auf jedem Knoten ausführen. Dies trifft besonders dann zu, wenn die zu jedem Knoten zu kopierenden Daten zwischen verschiedenen geografischen Regionen übertragen werden müssen.
- **Maximieren Sie die Speicherauslastung** für Aufgaben, die zwar viel Speicherplatz beanspruchen, aber nur für kurze Zeiträume und zu unterschiedlichen Zeiten während der Ausführung. Sie können weniger, aber dafür größere Computeknoten mit mehr Arbeitsspeicher einsetzen, um solche Spitzen effizient zu bewältigen. Auf diesen Knoten werden dann mehrere Aufgaben parallel ausgeführt, doch jede Aufgabe kann den reichlich vorhandenen Arbeitsspeicher zu verschiedenen Zeiten nutzen.
- **Verringern Sie die Beschränkungen der Knotenanzahl**, wenn die Kommunikation zwischen Knoten in einem Pool erforderlich ist. Derzeit sind Pools, die für die Kommunikation zwischen Knoten konfiguriert sind, auf 50 Computeknoten beschränkt. Wenn jeder Knoten in einem Pool Aufgaben parallel ausführen kann, kann also eine größere Anzahl von Aufgaben gleichzeitig ausgeführt werden.
- **Replizieren Sie einen lokalen Computecluster**, z. B. beim ersten Auslagern einer Compute-Umgebung in Azure. Wenn Ihre aktuelle lokale Lösung mehrere Aufgaben pro Computeknoten ausführt, können Sie die maximale Anzahl von Knotenaufgaben erhöhen, um diese Konfiguration besser widerzuspiegeln.

## <a name="example-scenario"></a>Beispielszenario

Stellen Sie sich beispielsweise eine Aufgabenanwendung mit CPU- und Arbeitsspeicheranforderungen vor, für die Knoten vom Typ [Standard\_D1](../cloud-services/cloud-services-sizes-specs.md#d-series) ausreichen. Um den Auftrag in der geforderten Zeit abzuschließen, sind aber 1.000 dieser Knoten nötig.

Anstatt Standard\_D1-Knoten mit einem CPU-Kern zu verwenden, können Sie [Standard\_D14](../cloud-services/cloud-services-sizes-specs.md#d-series)-Knoten mit je 16 Kernen verwenden und so die parallele Ausführung von Aufgaben ermöglichen. Dies bedeutet, dass etwa ein Sechzehntel der Knoten ausreicht. Es wären nicht 1.000 Knoten erforderlich, sondern nur 63. Falls große Anwendungsdateien oder umfangreiche Verweisdaten für jeden Knoten erforderlich sind, ergibt sich für die Auftragsdauer und Effizienz eine Verbesserung, da die Daten nur auf 63 Knoten kopiert werden.

## <a name="enable-parallel-task-execution"></a>Aktivieren der parallelen Aufgabenausführung

Sie konfigurieren die Computeknoten für die parallele Aufgabenausführung auf der Pool-Ebene. Bei der Batch-.NET-Bibliothek legen Sie die Eigenschaft [CloudPool.TaskSlotsPerNode](/dotnet/api/microsoft.azure.batch.cloudpool.taskslotspernode) fest, wenn Sie einen Pool erstellen. Bei Verwendung der Batch-REST-API legen Sie das [taskSlotsPerNode](/rest/api/batchservice/pool/add)-Element im Anforderungstext fest, wenn Sie einen Pool erstellen.

> [!NOTE]
> Sie können das Element `taskSlotsPerNode` und die [TaskSlotsPerNode](/dotnet/api/microsoft.azure.batch.cloudpool)-Eigenschaft nur zum Zeitpunkt der Poolerstellung festlegen. Nach der Erstellung eines Pools können diese Elemente nicht mehr geändert werden.

In Azure Batch können Sie die Anzahl von Aufgabenslots pro Knoten auf die bis zu vierfache Anzahl der Knotenkerne festlegen. Ist der Pool beispielsweise mit Knoten der Größe „Groß“ (vier Kerne) konfiguriert, kann für „ `taskSlotsPerNode` ” 16 festgelegt werden. Unabhängig von der Anzahl von Kernen, über die der Knoten verfügt, können maximal 256 Aufgabenslots pro Knoten existieren. Ausführliche Informationen zur Anzahl der Kerne für jede Knotengröße finden Sie unter [Größen für Clouddienste](../cloud-services/cloud-services-sizes-specs.md). Weitere Informationen zu den Grenzen des Dienstes finden Sie in [Kontingente und Einschränkungen für den Azure Batch-Dienst](batch-quota-limit.md).

> [!TIP]
> Berücksichtigen Sie unbedingt den Wert `taskSlotsPerNode`, wenn Sie für Ihren Pool eine [Formel für das automatische Skalieren](/rest/api/batchservice/pool/enableautoscale) erstellen. Beispielsweise könnte eine Formel zum Auswerten von `$RunningTasks` erheblich von einer Steigerung der Aufgaben pro Knoten betroffen sein. Weitere Informationen finden Sie unter [Automatisches Skalieren von Computeknoten in einem Azure Batch-Pool](batch-automatic-scaling.md).

## <a name="specify-task-distribution"></a>Angeben der Aufgabenverteilung

Beim Aktivieren von gleichzeitigen Aufgaben ist es wichtig, dass Sie angeben, wie die Aufgaben auf die Knoten des Pools verteilt werden sollen.

Mithilfe der [CloudPool.TaskSchedulingPolicy](/dotnet/api/microsoft.azure.batch.cloudpool.taskschedulingpolicy)-Eigenschaft können Sie angeben, dass Aufgaben gleichmäßig über alle Knoten im Pool zugewiesen werden sollen („Verteilen“). Oder Sie können angeben, dass jedem Knoten so viele Aufgaben wie möglich zugewiesen werden sollen, bevor sie einem anderen Knoten im Pool zugewiesen werden („Packen“).

Stellen Sie sich beispielsweise vor, dass der Pool mit [Standard\_D14](../cloud-services/cloud-services-sizes-specs.md#d-series)-Knoten (aus dem obigen Beispiel) für [CloudPool.TaskSlotsPerNode](/dotnet/api/microsoft.azure.batch.cloudpool.taskslotspernode) den Wert „16“ aufweist. Bei Konfiguration von [CloudPool.TaskSchedulingPolicy](/dotnet/api/microsoft.azure.batch.cloudpool.taskschedulingpolicy) mit einem [ComputeNodeFillType](/dotnet/api/microsoft.azure.batch.common.computenodefilltype) der Art *Pack* wird die Auslastung aller 16 Kerne für die einzelnen Knoten maximiert, und es wird zugelassen, dass von einem [Pool mit automatischer Skalierung](batch-automatic-scaling.md) nicht verwendete Knoten (Knoten ohne zugewiesene Aufgaben) aus dem Pool entfernt werden. Dies minimiert die Ressourcenverwendung und spart Geld.

## <a name="define-variable-slots-per-task"></a>Definieren von variablen Slots pro Aufgabe

Eine Aufgabe kann mit der [CloudTask.RequiredSlots](/dotnet/api/microsoft.azure.batch.cloudtask.requiredslots)-Eigenschaft definiert werden, um anzugeben, wie viele Slots zur Ausführung auf einem Computeknoten erforderlich sind. Der Standardwert ist 1. Sie können variable Slots pro Aufgabe festlegen, falls Ihre Aufgaben über unterschiedliche Gewichtungen in Bezug auf die Ressourcennutzung auf dem Computeknoten verfügen. So wird es ermöglicht, dass jeder Computeknoten über eine angemessene Anzahl von gleichzeitig ausgeführten Aufgaben verfügt, ohne dass Systemressourcen wie die CPU oder der Arbeitsspeicher überlastet werden.

Beispielsweise können Sie für einen Pool mit der Eigenschaft `taskSlotsPerNode = 8` CPU-intensive Aufgaben, die mehrere Kerne benötigen, mit `requiredSlots = 8` übermitteln und für andere Aufgaben die Einstellung `requiredSlots = 1` verwenden. Wenn diese gemischte Workload geplant wird, werden die CPU-intensiven Aufgaben ausschließlich auf den zugehörigen Computeknoten ausgeführt. Andere Aufgaben können gleichzeitig (maximal acht parallel) auf anderen Knoten ausgeführt werden. So können Sie die Workload auf mehrere Computeknoten verteilen und die Effizienz der Ressourcennutzung verbessern.

Stellen Sie sicher, dass Sie für die `requiredSlots`-Eigenschaft einer Aufgabe keinen größeren Wert als für die `taskSlotsPerNode`-Eigenschaft des Pools angeben. Dies würde dazu führen, dass die Aufgabe niemals ausgeführt werden kann. Vom Batch-Dienst wird dieser Konflikt derzeit nicht überprüft, wenn Sie Aufgaben übermitteln. Der Grund ist, dass ein Auftrag bei der Übermittlung ggf. nicht über einen angebundenen Pool verfügt oder dass durch das Deaktivieren und anschließende erneute Aktivieren ggf. eine Änderung in einen anderen Pool erfolgt.

> [!TIP]
> Bei variablen Aufgabenslots kann es passieren, dass umfangreiche Aufgaben, die mehr Slots erfordern, vorübergehend nicht geplant werden können, weil auf keinem Computeknoten genügend Slots vorhanden sind. Dies gilt auch dann, wenn einige andere Knoten noch über ungenutzte Slots verfügen. Sie können die Auftragspriorität für diese Aufgaben erhöhen, damit diese eine bessere Chance haben, die auf Knoten verfügbaren Slots zu erhalten.
>
> Der Batch-Dienst gibt [TaskScheduleFailEvent](batch-task-schedule-fail-event.md) aus, wenn die Ausführung einer Aufgabe nicht geplant werden kann. Die Planung wird so lange wiederholt, bis die erforderlichen Slots zur Verfügung stehen. Sie können auf dieses Ereignis lauschen, um potenzielle Probleme bei der Aufgabenplanung zu erkennen und entsprechend zu beheben.

## <a name="batch-net-example"></a>Beispiel für Batch .NET

Die folgenden Codeausschnitte der [Batch .NET](/dotnet/api/microsoft.azure.batch)-API zeigen, wie Sie einen Pool mit mehreren Aufgabenslots pro Knoten erstellen und eine Aufgabe mit den erforderlichen Slots übermitteln.

### <a name="create-a-pool-with-multiple-task-slots-per-node"></a>Erstellen eines Pools mit mehreren Aufgabenslots pro Knoten

Dieser Codeausschnitt zeigt eine Anforderung zum Erstellen eines Pools aus vier Knoten mit vier zulässigen Aufgabenslots pro Knoten. Er gibt eine Richtlinie für die Aufgabenplanung vor, die besagt, dass jeder Knoten mit Aufgaben gefüllt werden soll, bevor diese den anderen Knoten im Pool zugewiesen werden.

Weitere Informationen zum Hinzufügen von Pools mit der Batch .NET-API finden Sie unter [BatchClient.PoolOperations.CreatePool](/dotnet/api/microsoft.azure.batch.pooloperations.createpool).

```csharp
CloudPool pool =
    batchClient.PoolOperations.CreatePool(
        poolId: "mypool",
        targetDedicatedComputeNodes: 4
        virtualMachineSize: "standard_d1_v2",
        VirtualMachineConfiguration: new VirtualMachineConfiguration(
            imageReference: new ImageReference(
                                publisher: "MicrosoftWindowsServer",
                                offer: "WindowsServer",
                                sku: "2019-datacenter-core",
                                version: "latest"),
            nodeAgentSkuId: "batch.node.windows amd64");

pool.TaskSlotsPerNode = 4;
pool.TaskSchedulingPolicy = new TaskSchedulingPolicy(ComputeNodeFillType.Pack);
pool.Commit();
```

### <a name="create-a-task-with-required-slots"></a>Erstellen einer Aufgabe mit erforderlichen Slots

Dieser Codeausschnitt erstellt eine Aufgabe mit einem nicht standardmäßigen `requiredSlots`-Wert. Diese Aufgabe wird nur ausgeführt, wenn auf einem Computeknoten genügend freie Slots verfügbar sind.

```csharp
CloudTask task = new CloudTask(taskId, taskCommandLine)
{
    RequiredSlots = 2
};
```

### <a name="list-compute-nodes-with-counts-for-running-tasks-and-slots"></a>Auflisten von Computeknoten mit der Anzahl ausgeführter Aufgaben und Slots

Dieser Codeausschnitt listet alle Computeknoten im Pool auf und gibt die Anzahl der ausgeführten Aufgaben und Aufgabenslots pro Knoten aus.

```csharp
ODATADetailLevel nodeDetail = new ODATADetailLevel(selectClause: "id,runningTasksCount,runningTaskSlotsCount");
IPagedEnumerable<ComputeNode> nodes = batchClient.PoolOperations.ListComputeNodes(poolId, nodeDetail);

await nodes.ForEachAsync(node =>
{
    Console.WriteLine(node.Id + " :");
    Console.WriteLine($"RunningTasks = {node.RunningTasksCount}, RunningTaskSlots = {node.RunningTaskSlotsCount}");

}).ConfigureAwait(continueOnCapturedContext: false);
```

### <a name="list-task-counts-for-the-job"></a>Auflisten der Anzahl von Aufgaben für den Auftrag

Mit diesem Codeausschnitt wird die Anzahl von Aufgaben für den Auftrag abgerufen. Diese Zahl umfasst sowohl Aufgaben als auch Aufgabenslots pro Aufgabenzustand.

```csharp
TaskCountsResult result = await batchClient.JobOperations.GetJobTaskCountsAsync(jobId);

Console.WriteLine("\t\tActive\tRunning\tCompleted");
Console.WriteLine($"TaskCounts:\t{result.TaskCounts.Active}\t{result.TaskCounts.Running}\t{result.TaskCounts.Completed}");
Console.WriteLine($"TaskSlotCounts:\t{result.TaskSlotCounts.Active}\t{result.TaskSlotCounts.Running}\t{result.TaskSlotCounts.Completed}");
```

## <a name="batch-rest-example"></a>Beispiel für Batch REST

Die folgenden Codeausschnitte der [Batch-REST](/rest/api/batchservice/)-API zeigen, wie Sie einen Pool mit mehreren Aufgabenslots pro Knoten erstellen und eine Aufgabe mit den erforderlichen Slots übermitteln.

### <a name="create-a-pool-with-multiple-task-slots-per-node"></a>Erstellen eines Pools mit mehreren Aufgabenslots pro Knoten

Dieser Codeausschnitt zeigt eine Anforderung zum Erstellen eines Pools aus zwei großen Knoten mit maximal vier Aufgaben pro Knoten.

Weitere Informationen zum Hinzufügen von Pools mit der REST-API finden Sie unter [Add a pool to an account](/rest/api/batchservice/pool/add) (Hinzufügen eines Pools zu einem Konto).

```json
{
  "odata.metadata":"https://myaccount.myregion.batch.azure.com/$metadata#pools/@Element",
  "id":"mypool",
  "vmSize":"large",
  "virtualMachineConfiguration": {
    "imageReference": {
      "publisher": "canonical",
      "offer": "ubuntuserver",
      "sku": "18.04-lts"
    },
    "nodeAgentSKUId": "batch.node.ubuntu 16.04"
  },
  "targetDedicatedComputeNodes":2,
  "taskSlotsPerNode":4,
  "enableInterNodeCommunication":true,
}
```

### <a name="create-a-task-with-required-slots"></a>Erstellen einer Aufgabe mit erforderlichen Slots

Dieser Codeausschnitt zeigt eine Anforderung zum Hinzufügen einer Aufgabe mit einem nicht standardmäßigen `requiredSlots`-Wert. Diese Aufgabe wird nur ausgeführt, wenn auf dem Computeknoten genügend freie Slots verfügbar sind.

```json
{
  "id": "taskId",
  "commandLine": "bash -c 'echo hello'",
  "userIdentity": {
    "autoUser": {
      "scope": "task",
      "elevationLevel": "nonadmin"
    }
  },
  "requiredSLots": 2
}
```

## <a name="code-sample-on-github"></a>Codebeispiel auf GitHub

Das [ParallelTasks](https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/ParallelTasks)-Projekt auf GitHub veranschaulicht die Verwendung der [CloudPool.TaskSlotsPerNode](/dotnet/api/microsoft.azure.batch.cloudpool.taskslotspernode)-Eigenschaft.

Diese C#-Konsolenanwendung verwendet die [Batch-Bibliothek für .NET](/dotnet/api/microsoft.azure.batch) zum Erstellen eines Pools mit einem oder mehreren Serverknoten. Sie führt eine konfigurierbare Anzahl von Aufgaben auf diesen Knoten aus, um eine variable Auslastung zu simulieren. Die Ausgabe der Anwendung enthält die Informationen dazu, welcher Knoten welche Aufgabe ausgeführt hat. Die Anwendung liefert auch eine Zusammenfassung der Aufgabenparameter und der -dauer.

Als Beispiel ist unten der Zusammenfassungsteil der Ausgabe von zwei unterschiedlichen Ausführungen der ParallelTasks-Beispielanwendungen angegeben. Die hier dargestellten Auftragsdauern enthalten nicht die Zeit für die Poolerstellung. Der Grund ist, dass jeder Auftrag an einen zuvor erstellten Pool übermittelt wurde, dessen Computeknoten sich bei der Übermittlung im *Leerlaufzustand* befunden haben.

Die erste Ausführung der Beispielanwendung zeigt, dass die Aufgabe bei Nutzung eines einzigen Knotens im Pool und einer Aufgabe pro Knoten mehr als 30 Minuten dauert.

```
Nodes: 1
Node size: large
Task slots per node: 1
Max slots per task: 1
Tasks: 32
Duration: 00:30:01.4638023
```

Die zweite Ausführung des Beispiels zeigt eine deutliche Verringerung der Aufgabendauer. Das liegt daran, dass der Pool mit vier Aufgaben pro Knoten konfiguriert wurde, wodurch die parallele Aufgabenausführung in etwa einem Viertel der Zeit ermöglicht wird.

```
Nodes: 1
Node size: large
Task slots per node: 4
Max slots per task: 1
Tasks: 32
Duration: 00:08:48.2423500
```

## <a name="next-steps"></a>Nächste Schritte

- Probieren Sie das Wärmebild für [Batch Explorer](https://azure.github.io/BatchExplorer/) aus. Der Batch Explorer ist ein kostenloses eigenständiges Clienttool mit zahlreichen Features, das Sie beim Erstellen, Debuggen und Überwachen von Azure Batch-Anwendungen unterstützt. Wenn Sie die Beispielanwendung [ParallelTasks](https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/ParallelTasks) ausführen, können Sie das Wärmebildfeature von Batch Explorer für eine einfache Visualisierung der Ausführung paralleler Aufgaben auf jedem Knoten verwenden.
- Erkunden Sie die [Azure Batch-Beispiele auf GitHub](https://github.com/Azure/azure-batch-samples).
- Informieren Sie sich über [Taskabhängigkeiten in Batch](batch-task-dependencies.md).
