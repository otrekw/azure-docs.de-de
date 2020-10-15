---
title: Zählen der Zustände für Aufgaben und Knoten
description: Zählen der Status von Azure Batch-Tasks und -Computeknoten zum Verwalten und Überwachen von Batch-Lösungen.
ms.date: 06/18/2020
ms.topic: how-to
ms.custom: seodec18
ms.openlocfilehash: 90f741b9ec5e17da4fd0cc95ef921e116b0c27dc
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "85960587"
---
# <a name="monitor-batch-solutions-by-counting-tasks-and-nodes-by-state"></a>Überwachen von Batch-Lösungen durch Zählen von Tasks und Knoten nach Status

Zum Überwachen und Verwalten umfangreicher Azure Batch-Lösungen müssen Sie möglicherweise die Anzahl der Ressourcen in verschiedenen Status ermitteln. Azure Batch ermöglicht effiziente Vorgänge zum Abrufen von Tasks und Serverknoten in Batch. Sie können diese Vorgänge anstelle von potenziell zeitaufwendigen Listenabfragen verwenden, um ausführliche Informationen zu großen Sammlungen von Tasks oder Knoten zurückzugeben.

- [Get Task Counts](/rest/api/batchservice/job/gettaskcounts) (Taskanzahl abrufen) ruft eine aggregierte Anzahl von aktiven, ausgeführten und abgeschlossenen Tasks in einem Auftrag sowie von erfolgreichen oder nicht erfolgreichen Tasks ab. 

  Dadurch, dass Sie die Anzahl von Tasks jedes Status zählen, können Sie für Benutzer leichter den Fortschrittsstatus des Auftrags anzeigen oder unerwartete Verzögerungen oder Fehler erkennen, die sich möglicherweise auf den Auftrag auswirken. Get Task Counts (Taskanzahl abrufen) ist ab Version 2017-06-01.5.1 der Batch-Dienst-API und verwandten SDKs und Tools verfügbar.

- [List Pool Node Counts](/rest/api/batchservice/account/listpoolnodecounts) (Poolknotenanzahl auflisten) ruft die Anzahl dedizierter Computeknoten mit niedriger Priorität in jedem Pool ab, die sich in verschiedenen Zuständen befinden: „Wird erstellt“, „Im Leerlauf“, „Offline“, „Vorzeitig entfernt“, „Neustart wird ausgeführt“, „Reimaging“, „Wird gestartet“ und Ähnliches.

  Durch Zählen von Knoten in jedem Status können Sie festlegen, wann Sie über ausreichende Computeressourcen für die Ausführung Ihrer Aufträge besitzen und potentielle Probleme mit Ihren Pools identifizieren. List Pool Node Counts (Poolknotenanzahl auflisten) ist ab Version 2018-03-01.6.1 der Batch-Dienst-API und verwandten SDKs und Tools verfügbar.

Beachten Sie, dass die bei diesen Vorgängen zurückgegebenen Zahlen manchmal nicht aktuell sein können. Wenn Sie sichergehen möchten, dass eine Anzahl genau ist, verwenden Sie die Listenabfrage zum Zählen dieser Ressourcen. Über Listenabfragen können Sie außerdem Informationen zu anderen Batch-Ressourcen (z. B. Anwendungen) abrufen. Weitere Informationen zum Anwenden von Filtern auf Listenabfragen finden Sie unter [Erstellen von Abfragen zum effizienten Auflisten von Batch-Ressourcen](batch-efficient-list-queries.md).

## <a name="task-state-counts"></a>Taskstatuszähler

Mit dem Vorgang „Get Task Counts“ (Taskanzahl abrufen) werden Tasks nach den folgenden Status gezählt:

- **Aktiv**: Ein Task, der in die Warteschlange gestellt wurde und ausgeführt werden kann, derzeit aber keinem Computeknoten zugewiesen ist. Ein Task wird auch als `active` gezählt, wenn er von [einem übergeordneten Task](batch-task-dependencies.md) abhängig ist, der noch nicht abgeschlossen ist. 
- **Ausgeführt**: Ein Task, der einem Computeknoten zugewiesen, aber noch nicht abgeschlossen wurde. Ein Task wird als `running` gezählt, wenn sein Zustand entweder `preparing` oder `running` lautet (gemäß Angabe durch den Vorgang zum [Abrufen von Informationen zu einem Task](/rest/api/batchservice/task/get)).
- **Abgeschlossen**: Ein Task, der nicht mehr zur Ausführung berechtigt ist, da er entweder erfolgreich abgeschlossen wurde oder fehlgeschlagen ist und außerdem sein Wiederholungslimit ausgeschöpft hat. 
- **Erfolgreich**: Ein Task, dessen Taskausführungsergebnis `success` lautet. Batch überprüft anhand der Eigenschaft `TaskExecutionResult` der Eigenschaft [executionInfo](/rest/api/batchservice/task/get), ob ein Task erfolgreich war.
- **Fehler**: Ein Task, dessen Taskausführungsergebnis `failure` lautet.

Das folgende .NET-Codebeispiel zeigt, wie Sie die Taskanzahl nach dem Status abrufen:

```csharp
var taskCounts = await batchClient.JobOperations.GetJobTaskCountsAsync("job-1");

Console.WriteLine("Task count in active state: {0}", taskCounts.Active);
Console.WriteLine("Task count in preparing or running state: {0}", taskCounts.Running);
Console.WriteLine("Task count in completed state: {0}", taskCounts.Completed);
Console.WriteLine("Succeeded task count: {0}", taskCounts.Succeeded);
Console.WriteLine("Failed task count: {0}", taskCounts.Failed);
```

Sie können ein ähnliches Muster für REST und andere unterstützte Sprachen verwenden, um die Taskanzahl für einen Auftrag zu erhalten. 

> [!NOTE]
> API-Versionen des Batch-Diensts vor 2018-08-01.7.0 geben auch eine `validationStatus`-Eigenschaft in der Antwort von „Get Task Counts“ zurück. Diese Eigenschaft gibt an, ob Batch die Zustandsanzahl auf Konsistenz mit den von der API zum Auflisten von Tasks angegebenen überprüft hat. Der Wert `validated` bedeutet lediglich, dass Batch mindestens einmal für den Auftrag eine Konsistenzüberprüfung durchgeführt hat. Der Wert der `validationStatus`-Eigenschaft gibt nicht an, ob die von „Get Task Counts“ zurückgegebenen Zahlen auf dem neuesten Stand sind.

## <a name="node-state-counts"></a>Knotenstatuszähler

Der Vorgang „List Pool Node Counts“ (Poolknotenanzahl auflisten) zählt Computeknoten nach den folgenden Status in jedem Pool. Separate aggregierte Zähler werden für dedizierte Knoten und Knoten mit niedriger Priorität in jedem Pool bereitgestellt.

- **Erstellung**: Ein Azure zugeordneter virtueller Computer, der noch nicht gestartet wurde, um einem Pool beizutreten.
- **Im Leerlauf**: Ein verfügbarer Computeknoten, der derzeit keinen Task ausführt.
- **Verlässt Pool**: Ein Knoten, der den Pool verlässt, da er ausdrücklich vom Benutzer entfernt wurde oder da die Größe des Pools geändert wird oder der Pool automatisch herunterskaliert wird.
- **Offline**: Ein Knoten, mit dem Batch keine neuen Tasks planen kann.
- **Vorzeitig entfernt**: Ein Knoten mit niedriger Priorität, der aus dem Pool entfernt wurde, da Azure den virtuellen Computer freigegeben hat. Ein Knoten mit Status `preempted` kann neu initialisiert werden, wenn VM-Kapazität mit niedriger Priorität als Ersatz verfügbar ist.
- **Neustart**: Ein Knoten, der neu gestartet wird.
- **Reimaging**: Ein Knoten, auf dem das Betriebssystem neu installiert wird.
- **Ausführung**: Ein Knoten, der auf dem mindestens ein Task (mit Ausnahme des Starttasks) ausgeführt wird.
- **Wird gestartet**: Ein Knoten, auf dem der Batch-Dienst gestartet wird. 
- **StartTaskFailed**: Ein Knoten, auf dem der [Starttask](/rest/api/batchservice/pool/add#starttask) nicht erfolgreich war, alle Wiederholungsversuche ausgeschöpft wurden und auf dem `waitForSuccess` für den Starttask festgelegt ist. Der Knoten kann nicht zum Ausführen von Tasks verwendet werden.
- **Unbekannt**: Ein Knoten, der die Verbindung mit dem Batch-Dienst verloren hat und dessen Status unbekannt ist.
- **Unbrauchbar**: Ein Knoten, der aufgrund von Fehlern nicht zur Taskausführung verwendet werden kann.
- **WaitingForStartTask**: Ein Knoten, auf dem die Ausführung des Starttasks gestartet wurde, aber `waitForSuccess` festgelegt ist und der Starttask noch nicht abgeschlossen wurde.

Der folgende C#-Codeausschnitt zeigt, wie Sie die Anzahl der Knoten für alle Pools im aktuellen Konto auflisten:

```csharp
foreach (var nodeCounts in batchClient.PoolOperations.ListPoolNodeCounts())
{
    Console.WriteLine("Pool Id: {0}", nodeCounts.PoolId);

    Console.WriteLine("Total dedicated node count: {0}", nodeCounts.Dedicated.Total);

    // Get dedicated node counts in Idle and Offline states; you can get additional states.
    Console.WriteLine("Dedicated node count in Idle state: {0}", nodeCounts.Dedicated.Idle);
    Console.WriteLine("Dedicated node count in Offline state: {0}", nodeCounts.Dedicated.Offline);

    Console.WriteLine("Total low priority node count: {0}", nodeCounts.LowPriority.Total);

    // Get low-priority node counts in Running and Preempted states; you can get additional states.
    Console.WriteLine("Low-priority node count in Running state: {0}", nodeCounts.LowPriority.Running);
    Console.WriteLine("Low-priority node count in Preempted state: {0}", nodeCounts.LowPriority.Preempted);
}
```

Der folgende C#-Codeausschnitt zeigt, wie Sie die Anzahl der Knoten für einen bestimmten Pool im aktuellen Konto auflisten.

```csharp
foreach (var nodeCounts in batchClient.PoolOperations.ListPoolNodeCounts(new ODATADetailLevel(filterClause: "poolId eq 'testpool'")))
{
    Console.WriteLine("Pool Id: {0}", nodeCounts.PoolId);

    Console.WriteLine("Total dedicated node count: {0}", nodeCounts.Dedicated.Total);

    // Get dedicated node counts in Idle and Offline states; you can get additional states.
    Console.WriteLine("Dedicated node count in Idle state: {0}", nodeCounts.Dedicated.Idle);
    Console.WriteLine("Dedicated node count in Offline state: {0}", nodeCounts.Dedicated.Offline);

    Console.WriteLine("Total low priority node count: {0}", nodeCounts.LowPriority.Total);

    // Get low-priority node counts in Running and Preempted states; you can get additional states.
    Console.WriteLine("Low-priority node count in Running state: {0}", nodeCounts.LowPriority.Running);
    Console.WriteLine("Low-priority node count in Preempted state: {0}", nodeCounts.LowPriority.Preempted);
}
```

Sie können ein ähnliches Muster für REST und andere unterstützte Sprachen verwenden, um die Knotenanzahl für Pools zu erhalten.

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über den [Workflow des Batch-Diensts und primäre Ressourcen](batch-service-workflow-features.md) wie Pools, Knoten, Aufträge und Aufgaben.
- Informationen zum Anwenden von Filtern auf Abfragen, die Batch-Ressourcen auflisten, finden Sie unter [Erstellen von Abfragen zum effizienten Auflisten von Batch-Ressourcen](batch-efficient-list-queries.md).
