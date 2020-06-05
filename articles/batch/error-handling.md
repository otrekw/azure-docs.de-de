---
title: Fehlerbehandlung und -erkennung in Azure Batch
description: Informieren Sie sich über die Fehlerbehandlung in Batch-Dienstworkflows aus der Perspektive der Entwicklung.
ms.topic: article
ms.date: 05/15/2020
ms.openlocfilehash: 07b9d43ea9bdf21fe3188c4481e6dd0c86374607
ms.sourcegitcommit: a9784a3fd208f19c8814fe22da9e70fcf1da9c93
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/22/2020
ms.locfileid: "83790827"
---
# <a name="error-handling-and-detection-in-azure-batch"></a>Fehlerbehandlung und -erkennung in Azure Batch

Sie werden in Ihrer Batch-Lösung zuweilen sowohl Task- als auch Anwendungsfehler behandeln müssen. In diesem Artikel werden die verschiedenen Arten von Fehlern erläutert und beschrieben, wie Sie diese lösen können.

## <a name="error-codes"></a>Fehlercodes

Allgemeine Arten von Fehlern:

- Netzwerkfehler bei Anforderungen, die Batch niemals erreicht haben, oder wenn die Batch-Antwort den Client nicht rechtzeitig erreicht.
- Interne Serverfehler (standardmäßige HTTP-Antworten mit dem Statuscode 5xx).
- Fehler in Zusammenhang mit der Drosselung, z. B. HTTP-Antworten mit dem Statuscode 429 oder 503 und dem Retry-After-Header.
- 4xx-Fehler, beispielsweise AlreadyExists und InvalidOperation. Dies bedeutet, dass sich die Ressource nicht im richtigen Zustand für den Statusübergang befindet.

Detaillierte Informationen zu bestimmten Fehlercodes z. B. für REST-API, Batch-Dienst und Auftragstasks bzw. -planung finden Sie unter [Status- und Fehlercodes für Batch](https://docs.microsoft.com/rest/api/batchservice/batch-status-and-error-codes).

## <a name="application-failures"></a>Anwendungsfehler

Während der Ausführung generiert eine Anwendung unter Umständen eine Diagnoseausgabe für die Problembehandlung. Wie bereits in [Dateien und Verzeichnisse](files-and-directories.md) erläutert, schreibt der Batch-Dienst eine Standardausgabe und eine Standardfehlerausgabe in die Dateien `stdout.txt` und `stderr.txt` im Taskverzeichnis auf dem Computeknoten.

Diese Dateien können Sie über das Azure-Portal oder über ein Batch SDK herunterladen. So können Sie diese und andere Dateien beispielsweise mithilfe von [ComputeNode.GetNodeFile](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.computenode) und [CloudTask.GetNodeFile](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.cloudtask) in der Batch-Bibliothek für .NET zu Problembehandlungszwecken abrufen.

## <a name="task-errors"></a>Taskfehler

Taskfehler lassen sich in verschiedene Kategorien unterteilen.

### <a name="pre-processing-errors"></a>Fehler bei der Vorverarbeitung

Falls ein Task nicht gestartet werden kann, wird für den Task ein Fehler bei der Vorverarbeitung festgelegt.  

Fehler bei der Vorverarbeitung können auftreten, wenn die Ressourcendateien des Tasks verschoben wurden, das Speicherkonto nicht mehr zur Verfügung steht oder ein anderes Problem aufgetreten ist, das ein erfolgreiches Kopieren von Dateien auf den Knoten verhindert hat.

### <a name="file-upload-errors"></a>Fehler bei Hochladen von Dateien

Falls für einen Task angegebene Dateien nicht hochgeladen werden können, wird für den Task ein Fehler beim Hochladen von Dateien gemeldet.

Fehler beim Hochladen von Dateien können auftreten, wenn die für den Zugriff auf Azure Storage angegebene SAS ungültig ist oder keine Schreibberechtigungen umfasst, das Speicherkonto nicht mehr zur Verfügung steht oder ein anderes Problem aufgetreten ist, das ein erfolgreiches Kopieren von Dateien des Knotens verhindert hat.

### <a name="application-errors"></a>Anwendungsfehler

Ein Fehler kann auch beim in der Befehlszeile des Tasks angegebenen Prozess auftreten. Der Prozess gilt als nicht erfolgreich, wenn der Exitcode, der vom durch den Task ausgeführten Prozess zurückgegeben wird, ungleich Null ist (siehe *Exitcodes für Tasks* im nächsten Abschnitt).

Batch kann so konfiguriert werden, dass der Task im Fall eines Anwendungsfehlers automatisch mit einer bestimmten Häufigkeit wiederholt wird.

### <a name="constraint-errors"></a>Einschränkungsfehler

Mit der Einschränkung *maxWallClockTime*können Sie die maximale Ausführungsdauer für einen Auftrag oder Task angeben. Dies kann nützlich sein für das Beenden von Tasks, bei denen kein Fortschritt stattfindet.

Wenn die maximal zulässige Zeitspanne überschritten wurde, wird der Task als *abgeschlossen* gekennzeichnet. Als Exitcode wird jedoch `0xC000013A` zurückgegeben, und das Feld *schedulingError* wird als `{ category:"ServerError", code="TaskEnded"}` markiert.

## <a name="task-exit-codes"></a>Exitcodes für Tasks

Wie bereits erwähnt, wird ein Task vom Batch-Dienst als nicht erfolgreich gekennzeichnet, wenn der vom Task ausgeführte Prozess einen Exitcode ungleich Null zurückgibt. Wenn ein Task einen Prozess ausführt, füllt Batch die Exitcodeeigenschaft des Tasks mit dem Rückgabecode des Prozesses auf.

Wichtig ist hierbei, dass der Exitcode eines Tasks nicht vom Batch-Dienst bestimmt wird. Der Exitcode eines Tasks wird vom Prozess selbst bestimmt oder von dem Betriebssystem, unter dem der Prozess ausgeführt wird.

## <a name="task-failures-or-interruptions"></a>Taskfehler oder -unterbrechungen

Tasks werden gelegentlich nicht erfolgreich ausgeführt oder unterbrochen. Unter Umständen tritt in der Taskanwendung selbst ein Fehler auf, oder der Knoten, auf dem der Task ausgeführt wird, wird neu gestartet. Außerdem kann der Knoten während einer Änderung der Poolgröße ggf. entfernt werden (wenn die Richtlinie des Pools für die Aufhebung von Zuordnungen so konfiguriert ist, dass Knoten sofort entfernt werden, ohne auf den Abschluss von Tasks zu warten). In diesen Fällen kann der Task von Batch automatisch wieder der Warteschlange hinzugefügt und auf einem anderen Knoten ausgeführt werden.

Außerdem können zeitweilig Probleme auftreten, die dazu führen, dass ein Task nicht mehr reagiert oder zu lange dauert. Sie können das maximale Ausführungsintervall für einen Task festlegen. Wenn das maximale Ausführungsintervall überschritten wird, unterbricht der Batch-Dienst die Taskanwendung.

## <a name="connect-to-compute-nodes"></a>Herstellen einer Verbindung mit Computeknoten

Sie können weitere Debug- und Problembehandlungsmaßnahmen durchführen, indem Sie sich per Remotezugriff an einem Computeknoten anmelden. Sie können das Azure-Portal verwenden, um eine RDP-Datei (Remotedesktopprotokoll) für Windows-Knoten herunterzuladen und Secure Shell (SSH)-Verbindungsinformationen für Linux-Knoten abzurufen. Hierzu können Sie auch die Batch-APIs verwenden – beispielsweise [Batch .NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.computenode) oder [Batch Python](batch-linux-nodes.md#connect-to-linux-nodes-using-ssh).

> [!IMPORTANT]
> Um über RDP oder SSH eine Verbindung mit einem Knoten herzustellen, müssen Sie zuerst einen Benutzer auf dem Knoten erstellen. Zu diesem Zweck können Sie das Azure-Portal verwenden, über die Batch REST-API [einem Knoten ein Benutzerkonto hinzufügen](https://docs.microsoft.com/rest/api/batchservice/computenode/adduser), die Methode [ComputeNode.CreateComputeNodeUser](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.computenode) in Batch .NET aufrufen oder die Methode [add_user](batch-linux-nodes.md#connect-to-linux-nodes-using-ssh) im Batch Python-Modul aufrufen.

Wenn Sie den RDP- oder SSH-Zugriff auf Computeknoten beschränken müssen, helfen Ihnen die Informationen unter [Configure or disable remote access to compute nodes in an Azure Batch pool](pool-endpoint-configuration.md) (Konfigurieren oder Deaktivieren des Remotezugriffs auf Computeknoten in einem Azure Batch-Pool) weiter.

## <a name="troubleshoot-problem-nodes"></a>Problembehandlung bei Knoten

Wenn bei einigen Ihrer Tasks Fehler auftreten, kann Ihre Batch-Clientanwendung oder der Dienst die Metadaten der fehlgeschlagenen Tasks prüfen, um einen fehlerhaften Knoten zu finden. Jeder Knoten in einem Pool erhält eine eindeutige ID, und der Knoten, auf dem ein Task ausgeführt wird, ist in den Metadaten des Tasks angegeben. Nachdem Sie einen „Problemknoten“ identifiziert haben, können Sie dafür verschiedene Aktionen durchführen:

- **Neustarten des Knotens** ([REST](https://docs.microsoft.com/rest/api/batchservice/computenode/reboot) | [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.computenode.reboot))

    Durch einen Neustart des Knotens können latente Probleme wie etwa hängende oder abgestürzte Prozesse behoben werden. Wenn Ihr Pool ein Starttask oder der Auftrag ein Auftragsvorbereitungstask nutzt, werden diese Tasks beim Neustart des Knotens ausgeführt.
- **Reimaging des Knotens** ([REST](https://docs.microsoft.com/rest/api/batchservice/computenode/reimage) | [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.computenode.reimage))

    Dadurch wird das Betriebssystem auf dem Knoten neu installiert. Wie beim Neustart eines Knotens werden Start- und Auftragsvorbereitungstasks nach dem Reimaging des Knotens erneut ausgeführt.
- **Entfernen des Knotens aus dem Pool** ([REST](https://docs.microsoft.com/rest/api/batchservice/pool/removenodes) | [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.pooloperations))

    Manchmal ist es erforderlich, den Knoten aus dem Pool vollständig zu entfernen.
- **Deaktivieren der Taskplanung auf dem Knoten** ([REST](https://docs.microsoft.com/rest/api/batchservice/computenode/disablescheduling) | [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.computenode.disablescheduling))

    Dadurch wird der Knoten offline geschaltet, sodass ihm keine weiteren Tasks mehr zugewiesen werden. Er wird aber weiterhin ausgeführt und verbleibt im Pool. So können Sie die Fehlerursache weiter untersuchen, ohne dass die Daten der fehlgeschlagenen Tasks verloren gehen und durch den Knoten weitere Fehler auftreten. Sie können beispielsweise die Taskplanung auf dem Knoten deaktivieren und sich per Remotezugriff anmelden, um die Ereignisprotokolle des Knotens zu prüfen oder andere Schritte zur Problembehandlung auszuführen. Nach Abschluss der Prüfung können Sie den Knoten wieder online schalten, indem Sie die Taskplanung ([REST](https://docs.microsoft.com/rest/api/batchservice/computenode/enablescheduling) | [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.computenode.enablescheduling)) aktivieren oder eine andere der oben genannten Aktionen ausführen.

> [!IMPORTANT]
> Bei den oben beschriebenen Aktionen können Sie angeben, wie derzeit auf dem Knoten ausgeführte Tasks verarbeitet werden, wenn Sie die Aktion ausführen. Wenn Sie also etwa die Taskplanung auf einem Knoten mithilfe der Batch-Clientbibliothek für .NET deaktivieren, können Sie durch das Festlegen des Enumerationswerts [DisableComputeNodeSchedulingOption](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.common.disablecomputenodeschedulingoption) angeben, ob ausgeführte Tasks **beendet**, für die Planung auf anderen Knoten **wieder in eine Warteschlange eingereiht** oder vor dem Ausführen der Aktion abgeschlossen werden sollen (**TaskCompletion**).

## <a name="retry-after-errors"></a>Retry-After-Fehler

Sie werden von den Batch-APIs benachrichtigt, wenn ein Fehler auftritt. Sie können alle wiederholt werden, und alle enthalten einen globalen Wiederholungshandler für diesen Zweck. Es empfiehlt sich, diesen integrierten Mechanismus zu verwenden.

Nach einem Fehler sollten Sie eine Zeit lang (mehrere Sekunden zwischen Wiederholungsversuchen) warten, bevor Sie den Vorgang wiederholen. Wenn Wiederholungsversuche zu häufig oder zu schnell sind, führt der Wiederholungshandler Drosselung aus.

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie, wie Sie [nach Pool- und Knotenfehlern suchen](batch-pool-node-error-checking.md).
- Erfahren Sie, wie Sie [nach Auftrags- und Taskfehlern suchen](batch-job-task-error-checking.md).
- Sehen Sie sich die Liste der [Status- und Fehlercodes für Batch](https://docs.microsoft.com/rest/api/batchservice/batch-status-and-error-codes) an.
