---
title: Ermitteln von Auftrags- und Aufgabenfehlern
description: Fehler, auf die geprüft werden sollte, und Problembehandlung von Aufträgen und Tasks
author: mscurrell
ms.topic: how-to
ms.date: 03/10/2019
ms.author: markscu
ms.openlocfilehash: 368b803ae3e62c0e27da9e52d9df9842037757ff
ms.sourcegitcommit: 6fd8dbeee587fd7633571dfea46424f3c7e65169
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/21/2020
ms.locfileid: "83726620"
---
# <a name="job-and-task-error-checking"></a>Überprüfung auf Auftrags- und Taskfehler

Beim Hinzufügen von Aufträgen und Tasks können verschiedene Fehler auftreten. Die Erkennung von Fehlern für diese Vorgänge ist einfach, da alle Fehler sofort von der API, CLI oder Benutzeroberfläche zurückgegeben werden.  Es gibt jedoch Fehler, die später auftreten können, wenn Aufträge und Tasks geplant und ausgeführt werden.

In diesem Artikel werden die Fehler beschrieben, die nach dem Übermitteln von Aufträgen und Tasks auftreten können. Es werden die Fehler aufgeführt und erläutert, die überprüft und behandelt werden müssen.

## <a name="jobs"></a>Aufträge

Ein Auftrag ist eine Gruppierung einer oder mehrerer Tasks, wobei die Tasks jeweils die auszuführenden Befehlszeilen angeben.

Beim Hinzufügen eines Auftrags können die folgenden Parameter angegeben werden, die beeinflussen können, wie der Auftrag fehlschlagen kann:

- [Auftragseinschränkungen](https://docs.microsoft.com/rest/api/batchservice/job/add#jobconstraints)
  - Die `maxWallClockTime`-Eigenschaft kann optional angegeben werden, um die maximale Zeitspanne festzulegen, die ein Auftrag aktiv sein oder ausgeführt werden kann. Bei Überschreitung wird der Auftrag mit der in der [executionInfo](https://docs.microsoft.com/rest/api/batchservice/job/get#cloudjob) für den Auftrag festgelegten Eigenschaft `terminateReason` beendet.
- [Task zur Auftragsvorbereitung](https://docs.microsoft.com/rest/api/batchservice/job/add#jobpreparationtask)
  - Wenn angegeben, wird bei der ersten Ausführung eines Tasks für einen Auftrag auf einem Knoten ein Auftragsvorbereitungstask ausgeführt. Der Auftragsvorbereitungstask kann fehlschlagen, was dazu führt, dass der Task nicht ausgeführt und der Auftrag nicht abgeschlossen wird.
- [Task zur Auftragsfreigabe](https://docs.microsoft.com/rest/api/batchservice/job/add#jobreleasetask)
  - Ein Auftragsfreigabetask kann nur angegeben werden, wenn ein Auftragsvorbereitungstask konfiguriert ist. Wenn ein Auftrag beendet wird, wird der Auftragsfreigabetask auf jedem der Poolknoten ausgeführt, auf denen ein Auftragsvorbereitungstask ausgeführt wurde. Ein Auftragsfreigabetask kann fehlschlagen, aber der Auftrag erhält trotzdem den Status `completed`.

### <a name="job-properties"></a>Auftragseigenschaften

Die folgenden Auftragseigenschaften sollten auf Fehler überprüft werden:

- „[executionInfo](https://docs.microsoft.com/rest/api/batchservice/job/get#jobexecutioninformation)“:
  - Die `terminateReason`-Eigenschaft kann Werte aufweisen, um anzugeben, dass die in den Auftragseinschränkungen angegebene `maxWallClockTime`überschritten und der Auftrag daher beendet wurde. Sie kann auch festgelegt werden, um anzugeben, dass ein Task fehlgeschlagen ist, wenn die Auftragseigenschaft `onTaskFailure` entsprechend festgelegt wurde.
  - Die [schedulingError](https://docs.microsoft.com/rest/api/batchservice/job/get#jobschedulingerror)-Eigenschaft wird festgelegt, wenn ein Zeitplanungsfehler aufgetreten ist.
 
### <a name="job-preparation-tasks"></a>Task zur Auftragsvorbereitung

Wenn ein Auftragsvorbereitungstask für einen Auftrag angegeben wird, wird eine Instanz dieses Tasks ausgeführt, wenn ein Task für den Auftrag zum ersten Mal auf einem Knoten ausgeführt wird. Den für den Auftrag konfigurierten Auftragsvorbereitungstask können Sie sich als Taskvorlage vorstellen, wobei mehrere Taskinstanzen zur Auftragsvorbereitung bis zur Anzahl der Knoten in einem Pool ausgeführt werden.

Die Taskinstanzen für die Auftragsvorbereitung sollten überprüft werden, um festzustellen, ob Fehler aufgetreten sind:
- Wenn ein Auftragsvorbereitungstask ausgeführt wird, dann wird der Task, der den Auftragsvorbereitungstask ausgelöst hat, in einen [Zustand](https://docs.microsoft.com/rest/api/batchservice/task/get#taskstate) von `preparing` versetzt. Wenn der Auftragsvorbereitungstask dann fehlschlägt, kehrt der auslösende Task in den Zustand `active` zurück und wird nicht ausgeführt.  
- Alle Instanzen des Auftragsvorbereitungstasks, die ausgeführt wurden, können mithilfe der API zum [Auflisten des Vorbereitungs- und Freigabetaskstatus](https://docs.microsoft.com/rest/api/batchservice/job/listpreparationandreleasetaskstatus) aus dem Auftrag abgerufen werden. Wie bei jedem Task sind für die Eigenschaften wie `failureInfo`, `exitCode` und `result`[Ausführungsinformationen](https://docs.microsoft.com/rest/api/batchservice/job/listpreparationandreleasetaskstatus#jobpreparationandreleasetaskexecutioninformation) verfügbar.
- Wenn Auftragsvorbereitungstasks fehlschlagen, werden die auslösenden Auftragstasks nicht ausgeführt, der Auftrag wird nicht abgeschlossen und ist blockiert. Der Pool kann ungenutzt bleiben, wenn es keine anderen Aufträge mit Tasks gibt, die geplant werden können.

### <a name="job-release-tasks"></a>Tasks zur Auftragsfreigabe

Wenn ein Auftragsfreigabetask für einen Auftrag angegeben ist, wird beim Beenden eines Auftrags eine Instanz des Auftragsfreigabetasks auf jedem der Poolknoten ausgeführt, auf denen ein Auftragsvorbereitungstask ausgeführt wurde.  Die Taskinstanzen für die Auftragsfreigabe sollten überprüft werden, um festzustellen, ob Fehler aufgetreten sind:
- Alle Instanzen des Auftragsfreigabetasks, die ausgeführt wurden, können mithilfe der API zum [Auflisten des Vorbereitungs- und Freigabetaskstatus](https://docs.microsoft.com/rest/api/batchservice/job/listpreparationandreleasetaskstatus) aus dem Auftrag abgerufen werden. Wie bei jedem Task sind für die Eigenschaften wie `failureInfo`, `exitCode` und `result`[Ausführungsinformationen](https://docs.microsoft.com/rest/api/batchservice/job/listpreparationandreleasetaskstatus#jobpreparationandreleasetaskexecutioninformation) verfügbar.
- Wenn mindestens eine Auftragsfreigabetask fehlschlägt, wird der Auftrag dennoch beendet und wechselt in den Status `completed`.

## <a name="tasks"></a>Aufgaben

Auftragstasks können aus mehreren Gründen fehlschlagen:

- Die Taskbefehlszeile schlägt fehl und gibt einen Exitcode ungleich NULL zurück.
- Für einen Task sind `resourceFiles` angegeben, es ist jedoch ein Fehler aufgetreten, der bedeutet, dass mindestens eine Datei nicht heruntergeladen wurde.
- Für einen Task sind `outputFiles` angegeben, es ist jedoch ein Fehler aufgetreten, der bedeutet, dass mindestens eine Datei nicht hochgeladen wurde.
- Die verstrichene Zeit für den Task, die durch die `maxWallClockTime`-Eigenschaft in den [Einschränkungen](https://docs.microsoft.com/rest/api/batchservice/task/add#taskconstraints) des Tasks angegeben wurde, wurde überschritten.

In allen Fällen müssen die folgenden Eigenschaften auf Fehler und Informationen zu den Fehlern überprüft werden:
- Die Eigenschaft [executionInfo](https://docs.microsoft.com/rest/api/batchservice/task/get#taskexecutioninformation) des Tasks enthält mehrere Eigenschaften, die Informationen zu einem Fehler bereitstellen. [result](https://docs.microsoft.com/rest/api/batchservice/task/get#taskexecutionresult) gibt an, ob der Task aus einem bestimmten Grund fehlgeschlagen ist, wobei `exitCode` und `failureInfo` weitere Informationen zum Fehler bereitstellen.
- Der Task wechselt immer in den [Status](https://docs.microsoft.com/rest/api/batchservice/task/get#taskstate) `completed`, und zwar unabhängig davon, ob er erfolgreich war oder fehlgeschlagen ist.

Die Auswirkungen von Taskfehlern auf den Auftrag und alle Taskabhängigkeiten müssen berücksichtigt werden.  Die [exitConditions](https://docs.microsoft.com/rest/api/batchservice/task/add#exitconditions)-Eigenschaft kann für einen Task angegeben werden, um eine Aktion für Abhängigkeiten und für den Auftrag zu konfigurieren.
- Für Abhängigkeiten steuert [dependencyAction-](https://docs.microsoft.com/rest/api/batchservice/task/add#dependencyaction), ob die vom fehlgeschlagenen Task abhängigen Tasks blockiert sind oder ausgeführt werden.
- Für den Auftrag steuert [jobAction](https://docs.microsoft.com/rest/api/batchservice/task/add#jobaction), ob der fehlgeschlagene Task dazu führt, dass der Auftrag deaktiviert, beendet oder unverändert gelassen wird.

### <a name="task-command-line-failures"></a>Fehler bei Taskbefehlszeilen

Wird die Taskbefehlszeile ausgeführt, wird die Ausgabe in `stderr.txt` und `stdout.txt` geschrieben. Außerdem kann die Anwendung in anwendungsspezifische Protokolldateien schreiben.

Wenn der Poolknoten, auf dem ein Task ausgeführt wurde, noch vorhanden ist, können die Protokolldateien abgerufen und angezeigt werden. Beispielsweise listet das Azure-Portal die Protokolldateien für einen Task oder einen Poolknoten auf und kann diese anzeigen. Mit mehreren APIs können auch Taskdateien aufgelistet und abgerufen werden, z. B. [Get from Task](https://docs.microsoft.com/rest/api/batchservice/file/getfromtask).

Da Pools und Poolknoten häufig kurzlebig sind und Knoten ständig hinzugefügt und gelöscht werden, empfiehlt es sich, Protokolldateien persistent zu speichern. [Taskausgabedateien](https://docs.microsoft.com/azure/batch/batch-task-output-files) sind eine bequeme Möglichkeit, Protokolldateien in Azure Storage zu speichern.

### <a name="output-file-failures"></a>Fehler bei Ausgabedateien
Bei jedem Dateiupload schreibt Batch zwei Protokolldateien in den Computeknoten: `fileuploadout.txt` und `fileuploaderr.txt`. Sie können diese Protokolldateien überprüfen, um weitere Informationen zu einem bestimmten Fehler zu erhalten. In Fällen, in denen nie versucht wurde, die Datei hochzuladen, da z. B. der Task selbst nicht ausgeführt werden konnte, sind diese Protokolldateien nicht vorhanden.  

## <a name="next-steps"></a>Nächste Schritte

Überprüfen Sie, ob Ihre Anwendung eine umfassende Fehlerüberprüfung implementiert. Es kann wichtig sein, Probleme schnell zu erkennen und zu diagnostizieren.
