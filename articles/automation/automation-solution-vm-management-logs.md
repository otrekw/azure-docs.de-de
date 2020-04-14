---
title: Abfragen von Protokollen aus der Lösung zum Starten/Beenden von VMs
description: In diesem Artikel wird beschrieben, wie Sie aus Azure Monitor Protokolldaten abfragen, die von der Lösung zum Starten/Beenden von VMs generiert werden.
services: automation
ms.subservice: process-automation
ms.date: 04/01/2020
ms.topic: conceptual
ms.openlocfilehash: 00f6a5f88a66f0a19943ff3a2c722ae1a9938e9e
ms.sourcegitcommit: 980c3d827cc0f25b94b1eb93fd3d9041f3593036
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/02/2020
ms.locfileid: "80576761"
---
# <a name="how-to-query-logs-from-startstop-vms-solution"></a>Abfragen von Protokollen aus der Lösung zum Starten/Beenden von VMs

Azure Automation leitet zwei Arten von Datensätzen an den verknüpften Log Analytics-Arbeitsbereich weiter: Auftragsprotokolle und Auftragsdatenströme. Diese Daten stehen in Azure Monitor für [Abfragen](../azure-monitor/log-query/log-query-overview.md) zur Verfügung.

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

Die folgende Tabelle enthält Beispiele für Protokollsuchen für Auftragsdatensätze, die mit dieser Lösung erfasst wurden.

|Abfrage | BESCHREIBUNG|
|----------|----------|
|Suchen nach Aufträgen für das Runbook „ScheduledStartStop_Parent“, die erfolgreich abgeschlossen wurden | <code>search Category == "JobLogs" <br>&#124;  where ( RunbookName_s == "ScheduledStartStop_Parent" ) <br>&#124;  where ( ResultType == "Completed" )  <br>&#124;  summarize AggregatedValue = count() by ResultType, bin(TimeGenerated, 1h) <br>&#124;  sort by TimeGenerated desc</code>|
|Suchen nach Aufträgen für das Runbook „ScheduledStartStop_Parent“, die nicht erfolgreich abgeschlossen wurden | <code>search Category == "JobLogs" <br>&#124;  where ( RunbookName_s == "ScheduledStartStop_Parent" ) <br>&#124;  where ( ResultType == "Failed" )  <br>&#124;  summarize AggregatedValue = count() by ResultType, bin(TimeGenerated, 1h) <br>&#124;  sort by TimeGenerated desc</code>|
|Suchen nach Aufträgen für das Runbook „SequencedStartStop_Parent“, die erfolgreich abgeschlossen wurden | <code>search Category == "JobLogs" <br>&#124;  where ( RunbookName_s == "SequencedStartStop_Parent" ) <br>&#124;  where ( ResultType == "Completed" ) <br>&#124;  summarize AggregatedValue = count() by ResultType, bin(TimeGenerated, 1h) <br>&#124;  sort by TimeGenerated desc</code>|
|Suchen nach Aufträgen für das Runbook „SequencedStartStop_Parent“, die nicht erfolgreich abgeschlossen wurden | <code>search Category == "JobLogs" <br>&#124;  where ( RunbookName_s == "SequencedStartStop_Parent" ) <br>&#124;  where ( ResultType == "Failed" ) <br>&#124;  summarize AggregatedValue = count() by ResultType, bin(TimeGenerated, 1h) <br>&#124;  sort by TimeGenerated desc</code>|

## <a name="next-steps"></a>Nächste Schritte

Die Lösung zum Starten/Beenden von VMs außerhalb der Geschäftszeiten umfasst keinen vordefinierten Satz von Warnungen. Informationen zum Erstellen von Warnungen bei fehlgeschlagenen Aufträgen zur Unterstützung Ihrer DevOps oder Betriebsprozesse und -verfahren finden Sie unter [Erstellen von Protokollwarnungen mit Azure Monitor](../azure-monitor/platform/alerts-log.md).