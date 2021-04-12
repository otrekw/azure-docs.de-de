---
title: Abfragen von Protokollen aus der Azure Automation-Lösung „VMs außerhalb der Geschäftszeiten starten/beenden“
description: In diesem Artikel erfahren Sie, wie Sie mit Azure Monitor Protokolldaten abfragen, die von der Lösung „VMs außerhalb der Geschäftszeiten starten/beenden“ generiert werden.
services: automation
ms.subservice: process-automation
ms.date: 04/01/2020
ms.topic: conceptual
ms.openlocfilehash: 3e9e924d6626d9f0dcd2db8a5e8b8f90a0aa01ce
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "100593849"
---
# <a name="query-logs-from-startstop-vms-during-off-hours"></a>Abfragen von Protokollen über „VMs außerhalb der Geschäftszeiten starten/beenden“

Azure Automation leitet zwei Arten von Datensätzen an den verknüpften Log Analytics-Arbeitsbereich weiter: Auftragsprotokolle und Auftragsdatenströme. In diesem Artikel werden die für die [Abfrage](../azure-monitor/logs/log-query-overview.md) in Azure Monitor verfügbaren Daten überprüft.

## <a name="job-logs"></a>Auftragsprotokolle

|Eigenschaft | BESCHREIBUNG|
|----------|----------|
|Caller |  Der Benutzer oder das System, von dem der Vorgang initiiert wurde. Mögliche Werte sind entweder eine E-Mail-Adresse oder, bei geplanten Aufträgen, ein System.|
|Category | Klassifizierung des Datentyps. Für Automation lautet der Wert „JobLogs“.|
|CorrelationId | Die GUID, bei der es sich um die Korrelations-ID des Runbookauftrags handelt.|
|JobId | Die GUID, bei der es sich um die ID des Runbookauftrags handelt.|
|operationName | Gibt den Typ des in Azure ausgeführten Vorgangs an. Für Automation lautet der Wert „Job“.|
|resourceId | Gibt den Ressourcentyp in Azure an. Für Automation ist der Wert das dem Runbook zugeordnete Automation-Konto.|
|ResourceGroup | Gibt den Namen der Ressourcengruppe des Runbookauftrags an.|
|ResourceProvider | Gibt den Azure-Dienst an, über den Sie die Ressourcen beziehen, die Sie bereitstellen und verwalten können. Für Automation lautet der Wert „Azure Automation“.|
|ResourceType | Gibt den Ressourcentyp in Azure an. Für Automation ist der Wert das dem Runbook zugeordnete Automation-Konto.|
|resultType | Der Status des Runbookauftrags. Mögliche Werte:<br>- Gestartet<br>- Beendet<br>- Ausgesetzt<br>- Fehler<br>- Erfolgreich|
|resultDescription | Beschreibt den resultierenden Zustand des Runbookauftrags. Mögliche Werte:<br>- Auftrag gestartet<br>- Fehler beim Ausführen des Auftrags<br>- Auftrag abgeschlossen|
|RunbookName | Gibt den Namen des Runbooks an.|
|SourceSystem | Gibt das Quellsystem für die gesendeten Daten an. Für Automation lautet der Wert „OpsManager“.|
|StreamType | Gibt den Typ des Ereignisses an. Mögliche Werte:<br>- Ausführlich<br>- Ausgabe<br>- Fehler<br>- Warnung|
|SubscriptionId | Gibt die Abonnement-ID des Auftrags an.
|Time | Ausführungsdatum und -uhrzeit des Runbookauftrags.|

## <a name="job-streams"></a>Auftragsdatenströme

|Eigenschaft | BESCHREIBUNG|
|----------|----------|
|Caller |  Der Benutzer oder das System, von dem der Vorgang initiiert wurde. Mögliche Werte sind entweder eine E-Mail-Adresse oder, bei geplanten Aufträgen, ein System.|
|Category | Klassifizierung des Datentyps. Für Automation lautet der Wert „JobStreams“.|
|JobId | Die GUID, bei der es sich um die ID des Runbookauftrags handelt.|
|operationName | Gibt den Typ des in Azure ausgeführten Vorgangs an. Für Automation lautet der Wert „Job“.|
|ResourceGroup | Gibt den Namen der Ressourcengruppe des Runbookauftrags an.|
|resourceId | Gibt die Ressourcen-ID in Azure an. Für Automation ist der Wert das dem Runbook zugeordnete Automation-Konto.|
|ResourceProvider | Gibt den Azure-Dienst an, über den Sie die Ressourcen beziehen, die Sie bereitstellen und verwalten können. Für Automation lautet der Wert „Azure Automation“.|
|ResourceType | Gibt den Ressourcentyp in Azure an. Für Automation ist der Wert das dem Runbook zugeordnete Automation-Konto.|
|resultType | Das Ergebnis des Runbookauftrags zum Zeitpunkt der Ereignisgenerierung. Ein möglicher Wert ist:<br>- InProgress|
|resultDescription | Enthält den Ausgabedatenstrom des Runbooks.|
|RunbookName | Der Name des Runbooks.|
|SourceSystem | Gibt das Quellsystem für die gesendeten Daten an. Für Automation lautet der Wert „OpsManager“.|
|StreamType | Der Typ des Auftragsstreams. Mögliche Werte:<br>- Status<br>- Ausgabe<br>- Warnung<br>- Fehler<br>- Debuggen<br>- Ausführlich|
|Time | Ausführungsdatum und -uhrzeit des Runbookauftrags.|

Wenn Sie Protokollsuchen durchführen, bei denen Kategoriedatensätze für **JobLogs** oder **JobStreams** zurückgegeben werden, können Sie die Ansicht **JobLogs** oder **JobStreams** wählen. Es werden dann mehrere Kacheln angezeigt, mit denen die von der Suche zurückgegebenen Updates zusammengefasst werden.

## <a name="sample-log-searches"></a>Beispiele für Protokollsuchen

Die folgende Tabelle enthält Beispiele für Protokollsuchen für Auftragsdatensätze, die mit der Lösung „VMs außerhalb der Geschäftszeiten starten/beenden“ erfasst wurden:

|Abfrage | BESCHREIBUNG|
|----------|----------|
|Suchen nach Aufträgen für das Runbook „ScheduledStartStop_Parent“, die erfolgreich abgeschlossen wurden | <code>search Category == "JobLogs" <br>&#124;  where ( RunbookName_s == "ScheduledStartStop_Parent" ) <br>&#124;  where ( ResultType == "Completed" )  <br>&#124;  summarize AggregatedValue = count() by ResultType, bin(TimeGenerated, 1h) <br>&#124;  sort by TimeGenerated desc</code>|
|Suchen nach Aufträgen für das Runbook „ScheduledStartStop_Parent“, die nicht erfolgreich abgeschlossen wurden | <code>search Category == "JobLogs" <br>&#124;  where ( RunbookName_s == "ScheduledStartStop_Parent" ) <br>&#124;  where ( ResultType == "Failed" )  <br>&#124;  summarize AggregatedValue = count() by ResultType, bin(TimeGenerated, 1h) <br>&#124;  sort by TimeGenerated desc</code>|
|Suchen nach Aufträgen für das Runbook „SequencedStartStop_Parent“, die erfolgreich abgeschlossen wurden | <code>search Category == "JobLogs" <br>&#124;  where ( RunbookName_s == "SequencedStartStop_Parent" ) <br>&#124;  where ( ResultType == "Completed" ) <br>&#124;  summarize AggregatedValue = count() by ResultType, bin(TimeGenerated, 1h) <br>&#124;  sort by TimeGenerated desc</code>|
|Suchen nach Aufträgen für das Runbook „SequencedStartStop_Parent“, die nicht erfolgreich abgeschlossen wurden | <code>search Category == "JobLogs" <br>&#124;  where ( RunbookName_s == "SequencedStartStop_Parent" ) <br>&#124;  where ( ResultType == "Failed" ) <br>&#124;  summarize AggregatedValue = count() by ResultType, bin(TimeGenerated, 1h) <br>&#124;  sort by TimeGenerated desc</code>|

## <a name="next-steps"></a>Nächste Schritte

* Informationen zum Einrichten des Features finden Sie unter [Konfigurieren der Lösung „VMs außerhalb der Geschäftszeiten starten/beenden“](automation-solution-vm-management-config.md).
* Informationen zu Protokollwarnungen während der Featurebereitstellung finden Sie unter [Erstellen, Anzeigen und Verwalten von Protokollwarnungen mithilfe von Azure Monitor](../azure-monitor/alerts/alerts-log.md).
* Informationen zum Beheben von Problemen mit dem Feature finden Sie unter [Troubleshooting für „VMs außerhalb der Geschäftszeiten starten/beenden“](troubleshoot/start-stop-vm.md).