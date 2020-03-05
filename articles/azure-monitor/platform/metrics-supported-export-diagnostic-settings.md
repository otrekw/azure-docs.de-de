---
title: Über Diagnoseeinstellungen exportierbare Azure Monitor-Plattformmetriken
description: Liste der Metriken, die mit Azure Monitor für jeden Ressourcentyp zur Verfügung stehen.
services: azure-monitor
ms.topic: reference
ms.date: 02/10/2020
ms.subservice: metrics
ms.openlocfilehash: 7a75655d1707dd2491065974ed8addc4c2da1a6a
ms.sourcegitcommit: 747a20b40b12755faa0a69f0c373bd79349f39e3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2020
ms.locfileid: "77661361"
---
# <a name="azure-monitor-platform-metrics-exportable-via-diagnostic-settings"></a>Über Diagnoseeinstellungen exportierbare Azure Monitor-Plattformmetriken

Azure Monitor stellt [Plattformmetriken](data-platform-metrics.md) standardmäßig ohne Konfiguration bereit. Es gibt verschiedene Methoden für die Interaktion mit Plattformmetriken, z.B. die Diagrammdarstellung im Portal, den Zugriff über die REST-API oder die Abfrage über PowerShell oder CLI. Eine vollständige Liste der Plattformmetriken, die derzeit mit der konsolidierten Metrikpipeline von Azure Monitor zur Verfügung stehen, finden Sie unter [Unterstützte Metriken](metrics-supported.md). Verwenden Sie die [API-Version 2018-01-01](https://docs.microsoft.com/rest/api/monitor/metricdefinitions), um diese Metriken abzufragen und darauf zuzugreifen. Weitere Metriken stehen möglicherweise im Portal oder über Legacy-APIs zur Verfügung.

Sie können die Plattformmetriken auf eine von zwei Arten aus der Azure Monitor-Pipeline an andere Speicherorte exportieren.
1. Mit [Diagnoseeinstellungen](diagnostic-settings.md) können Sie Metriken an Log Analytics, Event Hubs oder Azure Storage senden.
2. Sie können die [REST-API für Metriken](https://docs.microsoft.com/rest/api/monitor/metrics/list) verwenden.

Aufgrund von Feinheiten im Azure Monitor-Back-End können nicht alle Metriken mithilfe von Diagnoseeinstellungen exportiert werden. In der folgenden Tabelle ist angegeben, welche Metriken mithilfe von Diagnoseeinstellungen exportiert bzw. nicht exportiert werden können.

## <a name="change-to-behavior-for-nulls-and-zero-values"></a>Änderung am Verhalten für NULL-Werte und 0-Werte 
 
Bei den Plattformmetriken, die über Diagnoseeinstellungen exportiert werden können, gibt es einige Metriken, bei denen in Azure Monitor der Wert „0“ als NULL-Wert interpretiert wird. Dies führte zu einer gewissen Verwirrung zwischen den tatsächlichen „0“-Werten (von der Ressource ausgegeben) und den interpretierten „0“-Werten (NULL-Werte). Ab dem **1. April 2020** werden bei über Diagnoseeinstellungen exportierten Plattformmetriken keine „0“-Werte mehr exportiert, es sei denn, sie wurden tatsächlich von der zugrunde liegenden Ressource ausgegeben. Hinweis:

1.  Wenn Sie eine Ressourcengruppe oder eine bestimmte Ressource löschen, werden Metrikdaten aus den betroffenen Ressourcen nicht mehr an die Exportziele der Diagnoseeinstellungen gesendet. Das heißt, sie werden nicht mehr in Event Hubs, Speicherkonten und Log Analytics-Arbeitsbereichen angezeigt.
2.  Diese Verbesserung ist in allen öffentlichen und privaten Clouds verfügbar.
3.  Diese Änderung wirkt sich nicht auf das Verhalten der folgenden Elemente und Funktionen aus: 
   - Plattformressourcenprotokolle, die über Diagnoseeinstellungen exportiert werden
   - Metrikdiagramme in Metrik-Explorer
   - Warnungen zu Plattformmetriken
 
## <a name="metrics-exportable-table"></a>Exportierbare Tabelle für Metriken 

Die Tabelle enthält die folgenden Spalten. 
- Über Diagnoseeinstellungen exportierbar? 
- Betroffen durch NULL/0 
- ResourceType 
- Metrik 
- MetricDisplayName
- Einheit 
- AggregationType


> [!NOTE]
> In der folgenden Tabelle kann unten eine horizontale Scrollleiste angezeigt werden. Wenn Sie der Ansicht sind, dass Informationen fehlen, überprüfen Sie, ob die Scrollleiste ganz links steht.  


| Über Diagnoseeinstellungen exportierbar?  | Gibt NULLs aus |  ResourceType  |  Metrik  |  MetricDisplayName  |  Einheit  |  AggregationType | 
|---|---| ---- | ----- | ------ | ---- | ---- | 
| ****Ja****  | Nein |  Microsoft.AnalysisServices/servers  |  CleanerCurrentPrice  |  Memory: Bereinigung – aktueller Preis  |  Anzahl  |  Average | 
| ****Ja****  | Nein |  Microsoft.AnalysisServices/servers  |  CleanerMemoryNonshrinkable  |  Memory: Bereinigung – nicht verkleinerbarer Arbeitsspeicher  |  Byte  |  Average | 
| ****Ja****  | Nein |  Microsoft.AnalysisServices/servers  |  CleanerMemoryShrinkable  |  Memory: Bereinigung – verkleinerbarer Arbeitsspeicher  |  Byte  |  Average | 
| ****Ja****  | Nein |  Microsoft.AnalysisServices/servers  |  CommandPoolBusyThreads  |  Threads: Ausgelastete Threads im Befehlspool  |  Anzahl  |  Average | 
| ****Ja****  | Nein |  Microsoft.AnalysisServices/servers  |  CommandPoolIdleThreads  |  Threads: Leerlaufthreads im Befehlspool  |  Anzahl  |  Average | 
| ****Ja****  | Nein |  Microsoft.AnalysisServices/servers  |  CommandPoolJobQueueLength  |  Warteschlangenlänge für Aufträge im Befehlspool  |  Anzahl  |  Average | 
| ****Ja****  | Nein |  Microsoft.AnalysisServices/servers  |  CurrentConnections  |  Verbindung: Aktuelle Verbindungen  |  Anzahl  |  Average | 
| ****Ja****  | Nein |  Microsoft.AnalysisServices/servers  |  CurrentUserSessions  |  Aktuelle Benutzersitzungen  |  Anzahl  |  Average | 
| ****Ja****  | Nein |  Microsoft.AnalysisServices/servers  |  LongParsingBusyThreads  |  Threads: Ausgelastete lange Analysethreads  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.AnalysisServices/servers  |  LongParsingIdleThreads  |  Threads: Lange Analysethreads im Leerlauf  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.AnalysisServices/servers  |  LongParsingJobQueueLength  |  Threads: Warteschlangenlänge für lange Analyseaufträge  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.AnalysisServices/servers  |  mashup_engine_memory_metric  |  M-Engine – Arbeitsspeicher  |  Byte  |  Average | 
| **Ja**  | Nein |  Microsoft.AnalysisServices/servers  |  mashup_engine_private_bytes_metric  |  Private Bytes für M-Engine  |  Byte  |  Average | 
| **Ja**  | Nein |  Microsoft.AnalysisServices/servers  |  mashup_engine_qpu_metric  |  M-Engine – QPU  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.AnalysisServices/servers  |  mashup_engine_virtual_bytes_metric  |  Virtuelle Bytes für M-Engine  |  Byte  |  Average | 
| **Ja**  | Nein |  Microsoft.AnalysisServices/servers  |  memory_metric  |  Arbeitsspeicher  |  Byte  |  Average | 
| **Ja**  | Nein |  Microsoft.AnalysisServices/servers  |  memory_thrashing_metric  |  Arbeitsspeicherüberlastung  |  Percent  |  Average | 
| **Ja**  | Nein |  Microsoft.AnalysisServices/servers  |  MemoryLimitHard  |  Memory: Grenzwert für den festen Speicher  |  Byte  |  Average | 
| **Ja**  | Nein |  Microsoft.AnalysisServices/servers  |  MemoryLimitHigh  |  Memory: Obere Arbeitsspeichergrenze  |  Byte  |  Average | 
| **Ja**  | Nein |  Microsoft.AnalysisServices/servers  |  MemoryLimitLow  |  Memory: Untere Arbeitsspeichergrenze  |  Byte  |  Average | 
| **Ja**  | Nein |  Microsoft.AnalysisServices/servers  |  MemoryLimitVertiPaq  |  Memory: VertiPaq-Arbeitsspeichergrenze  |  Byte  |  Average | 
| **Ja**  | Nein |  Microsoft.AnalysisServices/servers  |  MemoryUsage  |  Memory: Speicherauslastung  |  Byte  |  Average | 
| **Ja**  | Nein |  Microsoft.AnalysisServices/servers  |  private_bytes_metric  |  Private Bytes  |  Byte  |  Average | 
| **Ja**  | Nein |  Microsoft.AnalysisServices/servers  |  ProcessingPoolBusyIOJobThreads  |  Threads: Ausgelastete Threads für E/A-Aufträge im Verarbeitungspool  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.AnalysisServices/servers  |  ProcessingPoolBusyNonIOThreads  |  Threads: Ausgelastete Nicht-E/A-Threads im Verarbeitungspool  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.AnalysisServices/servers  |  ProcessingPoolIdleIOJobThreads  |  Threads: Leerlaufthreads für E/A-Aufträge im Verarbeitungspool  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.AnalysisServices/servers  |  ProcessingPoolIdleNonIOThreads  |  Threads: Nicht-E/A-Leerlaufthreads im Verarbeitungspool  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.AnalysisServices/servers  |  ProcessingPoolIOJobQueueLength  |  Threads: Warteschlangenlänge für E/A-Aufträge im Verarbeitungspool  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.AnalysisServices/servers  |  ProcessingPoolJobQueueLength  |  Warteschlangenlänge für Verarbeitungspoolaufträge  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.AnalysisServices/servers  |  qpu_metric  |  QPU  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.AnalysisServices/servers  |  QueryPoolBusyThreads  |  Ausgelastete Abfragepoolthreads  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.AnalysisServices/servers  |  QueryPoolIdleThreads  |  Threads: Abfragepoolthreads im Leerlauf  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.AnalysisServices/servers  |  QueryPoolJobQueueLength  |  Threads: Auftragswarteschlangenlänge für Abfragepool  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.AnalysisServices/servers  |  Kontingent  |  Memory: Kontingent  |  Byte  |  Average | 
| **Ja**  | Nein |  Microsoft.AnalysisServices/servers  |  QuotaBlocked  |  Memory: Kontingent blockiert  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.AnalysisServices/servers  |  RowsConvertedPerSec  |  Verarbeitung: Konvertierte Zeilen pro Sekunde  |  Anzahl pro Sekunde  |  Average | 
| **Ja**  | Nein |  Microsoft.AnalysisServices/servers  |  RowsReadPerSec  |  Verarbeitung: Gelesene Zeilen pro Sekunde  |  Anzahl pro Sekunde  |  Average | 
| **Ja**  | Nein |  Microsoft.AnalysisServices/servers  |  RowsWrittenPerSec  |  Verarbeitung: Geschriebene Zeilen pro Sekunde  |  Anzahl pro Sekunde  |  Average | 
| **Ja**  | Nein |  Microsoft.AnalysisServices/servers  |  ShortParsingBusyThreads  |  Threads: Ausgelastete kurze Analysethreads  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.AnalysisServices/servers  |  ShortParsingIdleThreads  |  Threads: Kurze Analysethreads im Leerlauf  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.AnalysisServices/servers  |  ShortParsingJobQueueLength  |  Threads: Warteschlangenlänge für kurze Analyseaufträge  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.AnalysisServices/servers  |  SuccessfullConnectionsPerSec  |  Erfolgreiche Verbindungen pro Sekunde  |  Anzahl pro Sekunde  |  Average | 
| **Ja**  | Nein |  Microsoft.AnalysisServices/servers  |  TotalConnectionFailures  |  Verbindungsfehler gesamt  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.AnalysisServices/servers  |  TotalConnectionRequests  |  Total Connection Requests (Verbindungsanforderungen gesamt)  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.AnalysisServices/servers  |  VertiPaqNonpaged  |  Memory: Nicht ausgelagerte VertiPaq-Daten  |  Byte  |  Average | 
| **Ja**  | Nein |  Microsoft.AnalysisServices/servers  |  VertiPaqPaged  |  Memory: Ausgelagerte VertiPaq-Daten  |  Byte  |  Average | 
| **Ja**  | Nein |  Microsoft.AnalysisServices/servers  |  virtual_bytes_metric  |  Virtuelle Bytes  |  Byte  |  Average | 
| **Ja**  | **Ja** |  Microsoft.ApiManagement/service  |  BackendDuration  |  Dauer von Back-End-Anforderungen  |  Millisekunden  |  Average | 
| **Ja**  | Nein |  Microsoft.ApiManagement/service  |  Capacity  |  Capacity  |  Percent  |  Average | 
| **Ja**  | **Ja** |  Microsoft.ApiManagement/service  |  Duration  |  Gesamtdauer von Gatewayanforderungen  |  Millisekunden  |  Average | 
| **Ja**  | **Ja** |  Microsoft.ApiManagement/service  |  EventHubDroppedEvents  |  Gelöschte EventHub-Ereignisse  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.ApiManagement/service  |  EventHubRejectedEvents  |  Abgelehnte EventHub-Ereignisse  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.ApiManagement/service  |  EventHubSuccessfulEvents  |  Erfolgreiche EventHub-Ereignisse  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.ApiManagement/service  |  EventHubThrottledEvents  |  Gedrosselte EventHub-Ereignisse  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.ApiManagement/service  |  EventHubTimedoutEvents  |  EventHub-Ereignisse mit Zeitüberschreitung  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.ApiManagement/service  |  EventHubTotalBytesSent  |  Größe von EventHub-Ereignissen  |  Byte  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.ApiManagement/service  |  EventHubTotalEvents  |  Gesamtzahl von EventHub-Ereignissen  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.ApiManagement/service  |  EventHubTotalFailedEvents  |  Fehlerhafte EventHub-Ereignisse  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.ApiManagement/service  |  FailedRequests  |  Fehlerhafte Gatewayanforderungen (veraltet)  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.ApiManagement/service  |  OtherRequests  |  Andere Gatewayanforderungen (veraltet)  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.ApiManagement/service  |  Requests  |  Requests  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.ApiManagement/service  |  SuccessfulRequests  |  Erfolgreiche Gatewayanforderungen (veraltet)  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.ApiManagement/service  |  TotalRequests  |  Gatewayanforderungen insgesamt (veraltet)  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.ApiManagement/service  |  UnauthorizedRequests  |  Nicht autorisierte Gatewayanforderungen (veraltet)  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.AppPlatform/Spring  |  AppCpuUsagePercentage  |  Prozentualer Anteil der App-CPU-Auslastung  |  Percent  |  Average | 
| **Ja**  | Nein |  Microsoft.AppPlatform/Spring  |  AppMemoryCommitted  |  Zugewiesener App-Arbeitsspeicher  |  Byte  |  Average | 
| **Ja**  | Nein |  Microsoft.AppPlatform/Spring  |  AppMemoryMax  |  Maximaler App-Arbeitsspeicher  |  Byte  |  Maximum | 
| **Ja**  | Nein |  Microsoft.AppPlatform/Spring  |  AppMemoryUsed  |  Verwendeter App-Arbeitsspeicher  |  Byte  |  Average | 
| **Ja**  | Nein |  Microsoft.AppPlatform/Spring  |  GCPauseTotalCount  |  Anzahl der GC-Pausen  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.AppPlatform/Spring  |  GCPauseTotalTime  |  Gesamtzeit der GC-Pausen  |  Millisekunden  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.AppPlatform/Spring  |  MaxOldGenMemoryPoolBytes  |  Maximal verfügbare Datengröße der alten Generation  |  Byte  |  Average | 
| **Ja**  | Nein |  Microsoft.AppPlatform/Spring  |  OldGenMemoryPoolBytes  |  Datengröße der alten Generation  |  Byte  |  Average | 
| **Ja**  | Nein |  Microsoft.AppPlatform/Spring  |  OldGenPromotedBytes  |  Hochstufung auf die Datengröße der alten Generation  |  Byte  |  Maximum | 
| **Ja**  | Nein |  Microsoft.AppPlatform/Spring  |  SystemCpuUsagePercentage  |  Prozentualer Anteil der System-CPU-Auslastung  |  Percent  |  Average | 
| **Ja**  | Nein |  Microsoft.AppPlatform/Spring  |  TomcatErrorCount  |  Globale Tomcat-Fehler  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.AppPlatform/Spring  |  TomcatReceivedBytes  |  Insgesamt empfangene Bytes für Tomcat  |  Byte  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.AppPlatform/Spring  |  TomcatRequestMaxTime  |  Maximale Zeit für Tomcat-Anforderung  |  Millisekunden  |  Maximum | 
| **Ja**  | Nein |  Microsoft.AppPlatform/Spring  |  TomcatRequestTotalCount  |  Gesamtanzahl von Tomcat-Anforderungen  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.AppPlatform/Spring  |  TomcatRequestTotalTime  |  Gesamtzeit für Tomcat-Anforderungen  |  Millisekunden  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.AppPlatform/Spring  |  TomcatResponseAvgTime  |  Durchschnittliche Zeit für Tomcat-Anforderungen  |  Millisekunden  |  Average | 
| **Ja**  | Nein |  Microsoft.AppPlatform/Spring  |  TomcatSentBytes  |  Insgesamt gesendete Bytes für Tomcat  |  Byte  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.AppPlatform/Spring  |  TomcatSessionActiveCurrentCount  |  Anzahl aktiver Tomcat-Sitzungen  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.AppPlatform/Spring  |  TomcatSessionActiveMaxCount  |  Maximale Anzahl aktiver Tomcat-Sitzungen  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.AppPlatform/Spring  |  TomcatSessionAliveMaxTime  |  Maximale aktive Zeit für Tomcat-Sitzungen  |  Millisekunden  |  Maximum | 
| **Ja**  | Nein |  Microsoft.AppPlatform/Spring  |  TomcatSessionCreatedCount  |  Anzahl erstellter Tomcat-Sitzungen  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.AppPlatform/Spring  |  TomcatSessionExpiredCount  |  Anzahl abgelaufener Tomcat-Sitzungen  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.AppPlatform/Spring  |  TomcatSessionRejectedCount  |  Anzahl abgewiesener Tomcat-Sitzungen  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.AppPlatform/Spring  |  YoungGenPromotedBytes  |  Hochstufung auf die Datengröße der neuen Generation  |  Byte  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Automation/automationAccounts  |  TotalJob  |  Gesamtzahl an Einzelvorgängen (Jobs)  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Automation/automationAccounts  |  TotalUpdateDeploymentMachineRuns  |  Gesamtzahl von Updatebereitstellungsausführungen auf dem Computer  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Automation/automationAccounts  |  TotalUpdateDeploymentRuns  |  Gesamtzahl von Updatebereitstellungsausführungen  |  Anzahl  |  Gesamt | 
| Nein  | Nein |  Microsoft.Batch/batchAccounts  |  CoreCount  |  Dedizierte Anzahl von Kernen  |  Anzahl  |  Gesamt | 
| Nein  | Nein |  Microsoft.Batch/batchAccounts  |  CreatingNodeCount  |  Anzahl erstellter Knoten  |  Anzahl  |  Gesamt | 
| Nein  | Nein |  Microsoft.Batch/batchAccounts  |  IdleNodeCount  |  Anzahl von Knoten im Leerlauf  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Batch/batchAccounts  |  JobDeleteCompleteEvent  |  Ereignisse zum Abschluss des Löschvorgangs von Aufträgen  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Batch/batchAccounts  |  JobDeleteStartEvent  |  Ereignisse zum Starten des Löschvorgangs von Aufträgen  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Batch/batchAccounts  |  JobDisableCompleteEvent  |  Ereignisse zum Abschluss der Deaktivierung von Aufträgen  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Batch/batchAccounts  |  JobDisableStartEvent  |  Ereignisse zum Starten der Deaktivierung von Aufträgen  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Batch/batchAccounts  |  JobStartEvent  |  Ereignisse zum Starten von Aufträgen  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Batch/batchAccounts  |  JobTerminateCompleteEvent  |  Ereignisse zum Abschluss der Beendigung von Aufträgen  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Batch/batchAccounts  |  JobTerminateStartEvent  |  Ereignisse zum Starten der Beendigung von Aufträgen  |  Anzahl  |  Gesamt | 
| Nein  | Nein |  Microsoft.Batch/batchAccounts  |  LeavingPoolNodeCount  |  Anzahl von Knoten, die den Pool verlassen  |  Anzahl  |  Gesamt | 
| Nein  | Nein |  Microsoft.Batch/batchAccounts  |  LowPriorityCoreCount  |  Anzahl von LowPriority-Kernen  |  Anzahl  |  Gesamt | 
| Nein  | Nein |  Microsoft.Batch/batchAccounts  |  OfflineNodeCount  |  Anzahl von Offlineknoten  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Batch/batchAccounts  |  PoolCreateEvent  |  Poolerstellungsereignisse  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Batch/batchAccounts  |  PoolDeleteCompleteEvent  |  Ereignisse zum Abschluss des Löschvorgangs von Pools  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Batch/batchAccounts  |  PoolDeleteStartEvent  |  Ereignisse zum Starten des Löschvorgangs von Pools  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Batch/batchAccounts  |  PoolResizeCompleteEvent  |  Ereignisse zum Abschluss der Größenänderung von Pools  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Batch/batchAccounts  |  PoolResizeStartEvent  |  Ereignisse zum Start der Größenänderung von Pools  |  Anzahl  |  Gesamt | 
| Nein  | Nein |  Microsoft.Batch/batchAccounts  |  PreemptedNodeCount  |  Anzahl der vorzeitig entfernten Knoten  |  Anzahl  |  Gesamt | 
| Nein  | Nein |  Microsoft.Batch/batchAccounts  |  RebootingNodeCount  |  Anzahl neu gestarteter Knoten  |  Anzahl  |  Gesamt | 
| Nein  | Nein |  Microsoft.Batch/batchAccounts  |  ReimagingNodeCount  |  Anzahl von Knoten mit Reimaging  |  Anzahl  |  Gesamt | 
| Nein  | Nein |  Microsoft.Batch/batchAccounts  |  RunningNodeCount  |  Anzahl ausgeführter Knoten  |  Anzahl  |  Gesamt | 
| Nein  | Nein |  Microsoft.Batch/batchAccounts  |  StartingNodeCount  |  Anzahl gestarteter Knoten  |  Anzahl  |  Gesamt | 
| Nein  | Nein |  Microsoft.Batch/batchAccounts  |  StartTaskFailedNodeCount  |  Anzahl von Knoten mit Starttaskfehlern  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Batch/batchAccounts  |  TaskCompleteEvent  |  Taskabschlussereignisse  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Batch/batchAccounts  |  TaskFailEvent  |  Taskfehlerereignisse  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Batch/batchAccounts  |  TaskStartEvent  |  Taskstartereignisse  |  Anzahl  |  Gesamt | 
| Nein  | Nein |  Microsoft.Batch/batchAccounts  |  TotalLowPriorityNodeCount  |  Anzahl der Knoten mit niedriger Priorität  |  Anzahl  |  Gesamt | 
| Nein  | Nein |  Microsoft.Batch/batchAccounts  |  TotalNodeCount  |  Dedizierte Knotenanzahl  |  Anzahl  |  Gesamt | 
| Nein  | Nein |  Microsoft.Batch/batchAccounts  |  UnusableNodeCount  |  Anzahl nicht verwendbarer Knoten  |  Anzahl  |  Gesamt | 
| Nein  | Nein |  Microsoft.Batch/batchAccounts  |  WaitingForStartTaskNodeCount  |  Anzahl von Knoten, die auf den Starttask warten  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.BatchAI/workspaces  |  Aktive Kerne  |  Aktive Kerne  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.BatchAI/workspaces  |  Aktive Knoten  |  Aktive Knoten  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.BatchAI/workspaces  |  Kerne im Leerlauf  |  Kerne im Leerlauf  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.BatchAI/workspaces  |  Knoten im Leerlauf  |  Knoten im Leerlauf  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.BatchAI/workspaces  |  Auftrag abgeschlossen  |  Auftrag abgeschlossen  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.BatchAI/workspaces  |  Auftrag übermittelt  |  Auftrag übermittelt  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.BatchAI/workspaces  |  Verbleibende Kerne  |  Ausscheidende Kerne  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.BatchAI/workspaces  |  Verbleibende Knoten  |  Ausscheidende Knoten  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.BatchAI/workspaces  |  Vorzeitig entfernte Kerne  |  Vorzeitig entfernte Kerne  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.BatchAI/workspaces  |  Vorzeitig entfernte Knoten  |  Vorzeitig entfernte Knoten  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.BatchAI/workspaces  |  Quota Utilization Percentage (Prozentsatz der Kontingentnutzung)  |  Quota Utilization Percentage (Prozentsatz der Kontingentnutzung)  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.BatchAI/workspaces  |  Kerne insgesamt  |  Kerne insgesamt  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.BatchAI/workspaces  |  Knoten insgesamt  |  Knoten insgesamt  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.BatchAI/workspaces  |  Nicht verwendbare Kerne  |  Nicht verwendbare Kerne  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.BatchAI/workspaces  |  Unusable Nodes (Nicht verwendbare Knoten)  |  Unusable Nodes (Nicht verwendbare Knoten)  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.Blockchain/blockchainMembers  |  ConnectionAccepted  |  Akzeptierte Verbindungen  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Blockchain/blockchainMembers  |  ConnectionActive  |  Die aktiven Verbindungen.  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.Blockchain/blockchainMembers  |  ConnectionHandled  |  Behandelte Verbindungen  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Blockchain/blockchainMembers  |  CpuUsagePercentageInDouble  |  Prozentuale CPU-Auslastung  |  Percent  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Blockchain/blockchainMembers  |  IOReadBytes  |  Gelesene E/A-Bytes  |  Byte  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Blockchain/blockchainMembers  |  IOWriteBytes  |  Geschriebene E/A-Bytes  |  Byte  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Blockchain/blockchainMembers  |  MemoryLimit  |  Arbeitsspeicherlimit  |  Byte  |  Average | 
| **Ja**  | Nein |  Microsoft.Blockchain/blockchainMembers  |  MemoryUsage  |  Speicherauslastung  |  Byte  |  Average | 
| **Ja**  | Nein |  Microsoft.Blockchain/blockchainMembers  |  MemoryUsagePercentageInDouble  |  Prozentuale Arbeitsspeicherauslastung  |  Percent  |  Average | 
| **Ja**  | Nein |  Microsoft.Blockchain/blockchainMembers  |  PendingTransactions  |  Ausstehende Transaktionen  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.Blockchain/blockchainMembers  |  ProcessedBlocks  |  Verarbeitete Blöcke  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Blockchain/blockchainMembers  |  ProcessedTransactions  |  Verarbeitete Transaktionen  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Blockchain/blockchainMembers  |  QueuedTransactions  |  Transaktionen in Warteschlange  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.Blockchain/blockchainMembers  |  RequestHandled  |  Behandelte Anforderungen  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Blockchain/blockchainMembers  |  StorageUsage  |  Speicherauslastung  |  Byte  |  Average | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  cachehits  |  Cachetreffer  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  cachehits0  |  Cachetreffer (Shard 0)  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  cachehits1  |  Cachetreffer (Shard 1)  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  cachehits2  |  Cachetreffer (Shard 2)  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  cachehits3  |  Cachetreffer (Shard 3)  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  cachehits4  |  Cachetreffer (Shard 4)  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  cachehits5  |  Cachetreffer (Shard 5)  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  cachehits6  |  Cachetreffer (Shard 6)  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  cachehits7  |  Cachetreffer (Shard 7)  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  cachehits8  |  Cachetreffer (Shard 8)  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  cachehits9  |  Cachetreffer (Shard 9)  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  cacheLatency  |  Cachewartezeit in Mikrosekunden (Vorschau)  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  cachemisses  |  Cachefehler  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  cachemisses0  |  Cachefehler (Shard 0)  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  cachemisses1  |  Cachefehler (Shard 1)  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  cachemisses2  |  Cachefehler (Shard 2)  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  cachemisses3  |  Cachefehler (Shard 3)  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  cachemisses4  |  Cachefehler (Shard 4)  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  cachemisses5  |  Cachefehler (Shard 5)  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  cachemisses6  |  Cachefehler (Shard 6)  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  cachemisses7  |  Cachefehler (Shard 7)  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  cachemisses8  |  Cachefehler (Shard 8)  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  cachemisses9  |  Cachefehler (Shard 9)  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  cacheRead  |  Cache-Lesevorgänge  |  Bytes pro Sekunde  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  cacheRead0  |  Cachelesevorgänge (Shard 0)  |  Bytes pro Sekunde  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  cacheRead1  |  Cachelesevorgänge (Shard 1)  |  Bytes pro Sekunde  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  cacheRead2  |  Cachelesevorgänge (Shard 2)  |  Bytes pro Sekunde  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  cacheRead3  |  Cachelesevorgänge (Shard 3)  |  Bytes pro Sekunde  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  cacheRead4  |  Cachelesevorgänge (Shard 4)  |  Bytes pro Sekunde  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  cacheRead5  |  Cachelesevorgänge (Shard 5)  |  Bytes pro Sekunde  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  cacheRead6  |  Cachelesevorgänge (Shard 6)  |  Bytes pro Sekunde  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  cacheRead7  |  Cachelesevorgänge (Shard 7)  |  Bytes pro Sekunde  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  cacheRead8  |  Cachelesevorgänge (Shard 8)  |  Bytes pro Sekunde  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  cacheRead9  |  Cachelesevorgänge (Shard 9)  |  Bytes pro Sekunde  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  cacheWrite  |  Cache-Schreibvorgänge  |  Bytes pro Sekunde  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  cacheWrite0  |  Cacheschreibvorgänge (Shard 0)  |  Bytes pro Sekunde  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  cacheWrite1  |  Cacheschreibvorgänge (Shard 1)  |  Bytes pro Sekunde  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  cacheWrite2  |  Cacheschreibvorgänge (Shard 2)  |  Bytes pro Sekunde  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  cacheWrite3  |  Cacheschreibvorgänge (Shard 3)  |  Bytes pro Sekunde  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  cacheWrite4  |  Cacheschreibvorgänge (Shard 4)  |  Bytes pro Sekunde  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  cacheWrite5  |  Cacheschreibvorgänge (Shard 5)  |  Bytes pro Sekunde  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  cacheWrite6  |  Cacheschreibvorgänge (Shard 6)  |  Bytes pro Sekunde  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  cacheWrite7  |  Cacheschreibvorgänge (Shard 7)  |  Bytes pro Sekunde  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  cacheWrite8  |  Cacheschreibvorgänge (Shard 8)  |  Bytes pro Sekunde  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  cacheWrite9  |  Cacheschreibvorgänge (Shard 9)  |  Bytes pro Sekunde  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  connectedclients  |  Verbundene Clients  |  Anzahl  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  connectedclients0  |  Verbundene Clients (Shard 0)  |  Anzahl  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  connectedclients1  |  Verbundene Clients (Shard 1)  |  Anzahl  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  connectedclients2  |  Verbundene Clients (Shard 2)  |  Anzahl  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  connectedclients3  |  Verbundene Clients (Shard 3)  |  Anzahl  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  connectedclients4  |  Verbundene Clients (Shard 4)  |  Anzahl  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  connectedclients5  |  Verbundene Clients (Shard 5)  |  Anzahl  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  connectedclients6  |  Verbundene Clients (Shard 6)  |  Anzahl  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  connectedclients7  |  Verbundene Clients (Shard 7)  |  Anzahl  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  connectedclients8  |  Verbundene Clients (Shard 8)  |  Anzahl  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  connectedclients9  |  Verbundene Clients (Shard 9)  |  Anzahl  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  errors  |  Errors  |  Anzahl  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  evictedkeys  |  Entfernte Schlüssel  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  evictedkeys0  |  Entfernte Schlüssel (Shard 0)  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  evictedkeys1  |  Entfernte Schlüssel (Shard 1)  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  evictedkeys2  |  Entfernte Schlüssel (Shard 2)  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  evictedkeys3  |  Entfernte Schlüssel (Shard 3)  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  evictedkeys4  |  Entfernte Schlüssel (Shard 4)  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  evictedkeys5  |  Entfernte Schlüssel (Shard 5)  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  evictedkeys6  |  Entfernte Schlüssel (Shard 6)  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  evictedkeys7  |  Entfernte Schlüssel (Shard 7)  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  evictedkeys8  |  Entfernte Schlüssel (Shard 8)  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  evictedkeys9  |  Entfernte Schlüssel (Shard 9)  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  expiredkeys  |  Abgelaufene Schlüssel  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  expiredkeys0  |  Abgelaufene Schlüssel (Shard 0)  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  expiredkeys1  |  Abgelaufene Schlüssel (Shard 1)  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  expiredkeys2  |  Abgelaufene Schlüssel (Shard 2)  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  expiredkeys3  |  Abgelaufene Schlüssel (Shard 3)  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  expiredkeys4  |  Abgelaufene Schlüssel (Shard 4)  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  expiredkeys5  |  Abgelaufene Schlüssel (Shard 5)  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  expiredkeys6  |  Abgelaufene Schlüssel (Shard 6)  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  expiredkeys7  |  Abgelaufene Schlüssel (Shard 7)  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  expiredkeys8  |  Abgelaufene Schlüssel (Shard 8)  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  expiredkeys9  |  Abgelaufene Schlüssel (Shard 9)  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  getcommands  |  get-Vorgänge  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  getcommands0  |  Get-Vorgänge (Shard 0)  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  getcommands1  |  Get-Vorgänge (Shard 1)  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  getcommands2  |  Get-Vorgänge (Shard 2)  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  getcommands3  |  Get-Vorgänge (Shard 3)  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  getcommands4  |  Get-Vorgänge (Shard 4)  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  getcommands5  |  Get-Vorgänge (Shard 5)  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  getcommands6  |  Get-Vorgänge (Shard 6)  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  getcommands7  |  Get-Vorgänge (Shard 7)  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  getcommands8  |  Get-Vorgänge (Shard 8)  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  getcommands9  |  Get-Vorgänge (Shard 9)  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  operationsPerSecond  |  Vorgänge pro Sekunde  |  Anzahl  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  operationsPerSecond0  |  Vorgänge pro Sekunde (Shard 0)  |  Anzahl  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  operationsPerSecond1  |  Vorgänge pro Sekunde (Shard 1)  |  Anzahl  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  operationsPerSecond2  |  Vorgänge pro Sekunde (Shard 2)  |  Anzahl  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  operationsPerSecond3  |  Vorgänge pro Sekunde (Shard 3)  |  Anzahl  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  operationsPerSecond4  |  Vorgänge pro Sekunde (Shard 4)  |  Anzahl  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  operationsPerSecond5  |  Vorgänge pro Sekunde (Shard 5)  |  Anzahl  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  operationsPerSecond6  |  Vorgänge pro Sekunde (Shard 6)  |  Anzahl  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  operationsPerSecond7  |  Vorgänge pro Sekunde (Shard 7)  |  Anzahl  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  operationsPerSecond8  |  Vorgänge pro Sekunde (Shard 8)  |  Anzahl  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  operationsPerSecond9  |  Vorgänge pro Sekunde (Shard 9)  |  Anzahl  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  percentProcessorTime  |  CPU  |  Percent  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  percentProcessorTime0  |  CPU (Shard 0)  |  Percent  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  percentProcessorTime1  |  CPU (Shard 1)  |  Percent  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  percentProcessorTime2  |  CPU (Shard 2)  |  Percent  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  percentProcessorTime3  |  CPU (Shard 3)  |  Percent  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  percentProcessorTime4  |  CPU (Shard 4)  |  Percent  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  percentProcessorTime5  |  CPU (Shard 5)  |  Percent  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  percentProcessorTime6  |  CPU (Shard 6)  |  Percent  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  percentProcessorTime7  |  CPU (Shard 7)  |  Percent  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  percentProcessorTime8  |  CPU (Shard 8)  |  Percent  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  percentProcessorTime9  |  CPU (Shard 9)  |  Percent  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  serverLoad  |  Serverlast  |  Percent  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  serverLoad0  |  Serverauslastung (Shard 0)  |  Percent  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  serverLoad1  |  Serverauslastung (Shard 1)  |  Percent  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  serverLoad2  |  Serverauslastung (Shard 2)  |  Percent  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  serverLoad3  |  Serverauslastung (Shard 3)  |  Percent  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  serverLoad4  |  Serverauslastung (Shard 4)  |  Percent  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  serverLoad5  |  Serverauslastung (Shard 5)  |  Percent  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  serverLoad6  |  Serverauslastung (Shard 6)  |  Percent  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  serverLoad7  |  Serverauslastung (Shard 7)  |  Percent  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  serverLoad8  |  Serverauslastung (Shard 8)  |  Percent  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  serverLoad9  |  Serverauslastung (Shard 9)  |  Percent  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  setcommands  |  Sets  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  setcommands0  |  Set-Vorgänge (Shard 0)  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  setcommands1  |  Set-Vorgänge (Shard 1)  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  setcommands2  |  Set-Vorgänge (Shard 2)  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  setcommands3  |  Set-Vorgänge (Shard 3)  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  setcommands4  |  Set-Vorgänge (Shard 4)  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  setcommands5  |  Set-Vorgänge (Shard 5)  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  setcommands6  |  Set-Vorgänge (Shard 6)  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  setcommands7  |  Set-Vorgänge (Shard 7)  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  setcommands8  |  Set-Vorgänge (Shard 8)  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  setcommands9  |  Set-Vorgänge (Shard 9)  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  totalcommandsprocessed  |  Vorgänge gesamt  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  totalcommandsprocessed0  |  Vorgänge gesamt (Shard 0)  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  totalcommandsprocessed1  |  Vorgänge gesamt (Shard 1)  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  totalcommandsprocessed2  |  Vorgänge gesamt (Shard 2)  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  totalcommandsprocessed3  |  Vorgänge gesamt (Shard 3)  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  totalcommandsprocessed4  |  Vorgänge gesamt (Shard 4)  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  totalcommandsprocessed5  |  Vorgänge gesamt (Shard 5)  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  totalcommandsprocessed6  |  Vorgänge gesamt (Shard 6)  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  totalcommandsprocessed7  |  Vorgänge gesamt (Shard 7)  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  totalcommandsprocessed8  |  Vorgänge gesamt (Shard 8)  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  totalcommandsprocessed9  |  Vorgänge gesamt (Shard 9)  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  totalkeys  |  Schlüssel insgesamt  |  Anzahl  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  totalkeys0  |  Schlüssel gesamt (Shard 0)  |  Anzahl  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  totalkeys1  |  Schlüssel gesamt (Shard 1)  |  Anzahl  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  totalkeys2  |  Schlüssel gesamt (Shard 2)  |  Anzahl  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  totalkeys3  |  Schlüssel gesamt (Shard 3)  |  Anzahl  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  totalkeys4  |  Schlüssel gesamt (Shard 4)  |  Anzahl  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  totalkeys5  |  Schlüssel gesamt (Shard 5)  |  Anzahl  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  totalkeys6  |  Schlüssel gesamt (Shard 6)  |  Anzahl  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  totalkeys7  |  Schlüssel gesamt (Shard 7)  |  Anzahl  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  totalkeys8  |  Schlüssel gesamt (Shard 8)  |  Anzahl  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  totalkeys9  |  Schlüssel gesamt (Shard 9)  |  Anzahl  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  usedmemory  |  Verwendeter Arbeitsspeicher  |  Byte  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  usedmemory0  |  Verwendeter Arbeitsspeicher (Shard 0)  |  Byte  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  usedmemory1  |  Verwendeter Arbeitsspeicher (Shard 1)  |  Byte  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  usedmemory2  |  Verwendeter Arbeitsspeicher (Shard 2)  |  Byte  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  usedmemory3  |  Verwendeter Arbeitsspeicher (Shard 3)  |  Byte  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  usedmemory4  |  Verwendeter Arbeitsspeicher (Shard 4)  |  Byte  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  usedmemory5  |  Verwendeter Arbeitsspeicher (Shard 5)  |  Byte  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  usedmemory6  |  Verwendeter Arbeitsspeicher (Shard 6)  |  Byte  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  usedmemory7  |  Verwendeter Arbeitsspeicher (Shard 7)  |  Byte  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  usedmemory8  |  Verwendeter Arbeitsspeicher (Shard 8)  |  Byte  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  usedmemory9  |  Verwendeter Arbeitsspeicher (Shard 9)  |  Byte  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  usedmemorypercentage  |  Prozentsatz der Arbeitsspeicherverwendung  |  Percent  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  usedmemoryRss  |  Verwendeter Arbeitsspeicher (RSS)  |  Byte  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  usedmemoryRss0  |  Verwendeter Arbeitsspeicher (RSS, Shard 0)  |  Byte  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  usedmemoryRss1  |  Verwendeter Arbeitsspeicher (RSS, Shard 1)  |  Byte  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  usedmemoryRss2  |  Verwendeter Arbeitsspeicher (RSS, Shard 2)  |  Byte  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  usedmemoryRss3  |  Verwendeter Arbeitsspeicher (RSS, Shard 3)  |  Byte  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  usedmemoryRss4  |  Verwendeter Arbeitsspeicher (RSS, Shard 4)  |  Byte  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  usedmemoryRss5  |  Verwendeter Arbeitsspeicher (RSS, Shard 5)  |  Byte  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  usedmemoryRss6  |  Verwendeter Arbeitsspeicher (RSS, Shard 6)  |  Byte  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  usedmemoryRss7  |  Verwendeter Arbeitsspeicher (RSS, Shard 7)  |  Byte  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  usedmemoryRss8  |  Verwendeter Arbeitsspeicher (RSS, Shard 8)  |  Byte  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Cache/redis  |  usedmemoryRss9  |  Verwendeter Arbeitsspeicher (RSS, Shard 9)  |  Byte  |  Maximum | 
| Nein  | Nein |  Microsoft.ClassicCompute/domainNames/slots/roles  |  Disk Read Bytes/Sec  |  Datenträgerlesevorgänge  |  Bytes pro Sekunde  |  Average | 
| **Ja**  | Nein |  Microsoft.ClassicCompute/domainNames/slots/roles  |  Datenträgerlesevorgänge/Sek.  |  Datenträgerlesevorgänge/Sek.  |  Anzahl pro Sekunde  |  Average | 
| Nein  | Nein |  Microsoft.ClassicCompute/domainNames/slots/roles  |  Disk Write Bytes/Sec  |  Datenträgerschreibvorgänge  |  Bytes pro Sekunde  |  Average | 
| **Ja**  | Nein |  Microsoft.ClassicCompute/domainNames/slots/roles  |  Datenträgerschreibvorgänge/Sek.  |  Datenträgerschreibvorgänge/Sek.  |  Anzahl pro Sekunde  |  Average | 
| **Ja**  | Nein |  Microsoft.ClassicCompute/domainNames/slots/roles  |  Netzwerk eingehend  |  Netzwerk eingehend  |  Byte  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.ClassicCompute/domainNames/slots/roles  |  Netzwerk ausgehend  |  Netzwerk ausgehend  |  Byte  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.ClassicCompute/domainNames/slots/roles  |  CPU in Prozent  |  CPU in Prozent  |  Percent  |  Average | 
| Nein  | Nein |  Microsoft.ClassicCompute/virtualMachines  |  Disk Read Bytes/Sec  |  Datenträgerlesevorgänge  |  Bytes pro Sekunde  |  Average | 
| **Ja**  | Nein |  Microsoft.ClassicCompute/virtualMachines  |  Datenträgerlesevorgänge/Sek.  |  Datenträgerlesevorgänge/Sek.  |  Anzahl pro Sekunde  |  Average | 
| Nein  | Nein |  Microsoft.ClassicCompute/virtualMachines  |  Disk Write Bytes/Sec  |  Datenträgerschreibvorgänge  |  Bytes pro Sekunde  |  Average | 
| **Ja**  | Nein |  Microsoft.ClassicCompute/virtualMachines  |  Datenträgerschreibvorgänge/Sek.  |  Datenträgerschreibvorgänge/Sek.  |  Anzahl pro Sekunde  |  Average | 
| **Ja**  | Nein |  Microsoft.ClassicCompute/virtualMachines  |  Netzwerk eingehend  |  Netzwerk eingehend  |  Byte  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.ClassicCompute/virtualMachines  |  Netzwerk ausgehend  |  Netzwerk ausgehend  |  Byte  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.ClassicCompute/virtualMachines  |  CPU in Prozent  |  CPU in Prozent  |  Percent  |  Average | 
| **Ja**  | Nein |  Microsoft.ClassicStorage/storageAccounts  |  Verfügbarkeit  |  Verfügbarkeit  |  Percent  |  Average | 
| **Ja**  | Nein |  Microsoft.ClassicStorage/storageAccounts  |  Ausgehende Daten  |  Ausgehende Daten  |  Byte  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.ClassicStorage/storageAccounts  |  Eingehende Daten  |  Eingehende Daten  |  Byte  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.ClassicStorage/storageAccounts  |  SuccessE2ELatency  |  E2E-Latenz (erfolgreich)  |  Millisekunden  |  Average | 
| **Ja**  | Nein |  Microsoft.ClassicStorage/storageAccounts  |  SuccessServerLatency  |  Serverlatenz (erfolgreich)  |  Millisekunden  |  Average | 
| **Ja**  | Nein |  Microsoft.ClassicStorage/storageAccounts  |  Transaktionen  |  Transaktionen  |  Anzahl  |  Gesamt | 
| Nein  | Nein |  Microsoft.ClassicStorage/storageAccounts  |  UsedCapacity  |  Verwendete Kapazität  |  Byte  |  Average | 
| **Ja**  | Nein |  Microsoft.ClassicStorage/storageAccounts/blobServices  |  Verfügbarkeit  |  Verfügbarkeit  |  Percent  |  Average | 
| Nein  | Nein |  Microsoft.ClassicStorage/storageAccounts/blobServices  |  BlobCapacity  |  Blob-Kapazität  |  Byte  |  Average | 
| Nein  | Nein |  Microsoft.ClassicStorage/storageAccounts/blobServices  |  BlobCount  |  Anzahl von Blobs  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.ClassicStorage/storageAccounts/blobServices  |  ContainerCount  |  Anzahl von Blob-Containern  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.ClassicStorage/storageAccounts/blobServices  |  Ausgehende Daten  |  Ausgehende Daten  |  Byte  |  Gesamt | 
| Nein  | Nein |  Microsoft.ClassicStorage/storageAccounts/blobServices  |  IndexCapacity  |  Indexkapazität  |  Byte  |  Average | 
| **Ja**  | Nein |  Microsoft.ClassicStorage/storageAccounts/blobServices  |  Eingehende Daten  |  Eingehende Daten  |  Byte  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.ClassicStorage/storageAccounts/blobServices  |  SuccessE2ELatency  |  E2E-Latenz (erfolgreich)  |  Millisekunden  |  Average | 
| **Ja**  | Nein |  Microsoft.ClassicStorage/storageAccounts/blobServices  |  SuccessServerLatency  |  Serverlatenz (erfolgreich)  |  Millisekunden  |  Average | 
| **Ja**  | Nein |  Microsoft.ClassicStorage/storageAccounts/blobServices  |  Transaktionen  |  Transaktionen  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.ClassicStorage/storageAccounts/fileServices  |  Verfügbarkeit  |  Verfügbarkeit  |  Percent  |  Average | 
| **Ja**  | Nein |  Microsoft.ClassicStorage/storageAccounts/fileServices  |  Ausgehende Daten  |  Ausgehende Daten  |  Byte  |  Gesamt | 
| Nein  | Nein |  Microsoft.ClassicStorage/storageAccounts/fileServices  |  FileCapacity  |  Dateikapazität  |  Byte  |  Average | 
| Nein  | Nein |  Microsoft.ClassicStorage/storageAccounts/fileServices  |  FileCount  |  Dateianzahl  |  Anzahl  |  Average | 
| Nein  | Nein |  Microsoft.ClassicStorage/storageAccounts/fileServices  |  FileShareCount  |  Anzahl von Dateifreigaben  |  Anzahl  |  Average | 
| Nein  | Nein |  Microsoft.ClassicStorage/storageAccounts/fileServices  |  FileShareQuota  |  Kontingentgröße für Dateifreigabe  |  Byte  |  Average | 
| Nein  | Nein |  Microsoft.ClassicStorage/storageAccounts/fileServices  |  FileShareSnapshotCount  |  Anzahl von Momentaufnahmen in Dateifreigabe  |  Anzahl  |  Average | 
| Nein  | Nein |  Microsoft.ClassicStorage/storageAccounts/fileServices  |  FileShareSnapshotSize  |  Größe der Momentaufnahmen in Dateifreigabe  |  Byte  |  Average | 
| **Ja**  | Nein |  Microsoft.ClassicStorage/storageAccounts/fileServices  |  Eingehende Daten  |  Eingehende Daten  |  Byte  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.ClassicStorage/storageAccounts/fileServices  |  SuccessE2ELatency  |  E2E-Latenz (erfolgreich)  |  Millisekunden  |  Average | 
| **Ja**  | Nein |  Microsoft.ClassicStorage/storageAccounts/fileServices  |  SuccessServerLatency  |  Serverlatenz (erfolgreich)  |  Millisekunden  |  Average | 
| **Ja**  | Nein |  Microsoft.ClassicStorage/storageAccounts/fileServices  |  Transaktionen  |  Transaktionen  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.ClassicStorage/storageAccounts/queueServices  |  Verfügbarkeit  |  Verfügbarkeit  |  Percent  |  Average | 
| **Ja**  | Nein |  Microsoft.ClassicStorage/storageAccounts/queueServices  |  Ausgehende Daten  |  Ausgehende Daten  |  Byte  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.ClassicStorage/storageAccounts/queueServices  |  Eingehende Daten  |  Eingehende Daten  |  Byte  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.ClassicStorage/storageAccounts/queueServices  |  QueueCapacity  |  Warteschlangenkapazität  |  Byte  |  Average | 
| **Ja**  | Nein |  Microsoft.ClassicStorage/storageAccounts/queueServices  |  QueueCount  |  Anzahl von Warteschlangen  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.ClassicStorage/storageAccounts/queueServices  |  QueueMessageCount  |  Anzahl von Warteschlangennachrichten  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.ClassicStorage/storageAccounts/queueServices  |  SuccessE2ELatency  |  E2E-Latenz (erfolgreich)  |  Millisekunden  |  Average | 
| **Ja**  | Nein |  Microsoft.ClassicStorage/storageAccounts/queueServices  |  SuccessServerLatency  |  Serverlatenz (erfolgreich)  |  Millisekunden  |  Average | 
| **Ja**  | Nein |  Microsoft.ClassicStorage/storageAccounts/queueServices  |  Transaktionen  |  Transaktionen  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.ClassicStorage/storageAccounts/tableServices  |  Verfügbarkeit  |  Verfügbarkeit  |  Percent  |  Average | 
| **Ja**  | Nein |  Microsoft.ClassicStorage/storageAccounts/tableServices  |  Ausgehende Daten  |  Ausgehende Daten  |  Byte  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.ClassicStorage/storageAccounts/tableServices  |  Eingehende Daten  |  Eingehende Daten  |  Byte  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.ClassicStorage/storageAccounts/tableServices  |  SuccessE2ELatency  |  E2E-Latenz (erfolgreich)  |  Millisekunden  |  Average | 
| **Ja**  | Nein |  Microsoft.ClassicStorage/storageAccounts/tableServices  |  SuccessServerLatency  |  Serverlatenz (erfolgreich)  |  Millisekunden  |  Average | 
| **Ja**  | Nein |  Microsoft.ClassicStorage/storageAccounts/tableServices  |  TableCapacity  |  Tabellenkapazität  |  Byte  |  Average | 
| **Ja**  | Nein |  Microsoft.ClassicStorage/storageAccounts/tableServices  |  TableCount  |  Anzahl von Tabellen  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.ClassicStorage/storageAccounts/tableServices  |  TableEntityCount  |  Anzahl von Tabellenentitäten  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.ClassicStorage/storageAccounts/tableServices  |  Transaktionen  |  Transaktionen  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.CognitiveServices/accounts  |  BlockedCalls  |  Blockierte Aufrufe  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.CognitiveServices/accounts  |  CharactersTrained  |  Trainierte Zeichen  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.CognitiveServices/accounts  |  CharactersTranslated  |  Übersetzte Zeichen  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.CognitiveServices/accounts  |  ClientErrors  |  Clientfehler  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.CognitiveServices/accounts  |  DataIn  |  Eingehende Daten  |  Byte  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.CognitiveServices/accounts  |  DataOut  |  Datenausgabe  |  Byte  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.CognitiveServices/accounts  |  Latency  |  Latency  |  Millisekunden  |  Average | 
| **Ja**  | Nein |  Microsoft.CognitiveServices/accounts  |  ServerErrors  |  Serverfehler  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.CognitiveServices/accounts  |  SpeechSessionDuration  |  Dauer der Sprachsitzung  |  Sekunden  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.CognitiveServices/accounts  |  SuccessfulCalls  |  Erfolgreiche Aufrufe  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.CognitiveServices/accounts  |  TotalCalls  |  Aufrufe gesamt  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.CognitiveServices/accounts  |  TotalErrors  |  Fehler insgesamt  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.CognitiveServices/accounts  |  TotalTokenCalls  |  Tokenaufrufe gesamt  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.CognitiveServices/accounts  |  TotalTransactions  |  Transaktionen gesamt  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Compute/virtualMachines  |  Verbrauchte CPU-Guthaben  |  Verbrauchte CPU-Guthaben  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.Compute/virtualMachines  |  Verbleibende CPU-Guthaben  |  Verbleibende CPU-Guthaben  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.Compute/virtualMachines  |  Warteschlangentiefe für Datenträger  |  Warteschlangentiefe für Datenträger (Vorschauversion)  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.Compute/virtualMachines  |  Vom Datenträger gelesene Bytes/Sek.  |  Vom Datenträger gelesene Bytes/Sek. (Vorschauversion)  |  Anzahl pro Sekunde  |  Average | 
| **Ja**  | Nein |  Microsoft.Compute/virtualMachines  |  Datenträgerlesevorgänge/Sek.  |  Datenträgerlesevorgänge/Sek. (Vorschauversion)  |  Anzahl pro Sekunde  |  Average | 
| **Ja**  | Nein |  Microsoft.Compute/virtualMachines  |  Auf den Datenträger geschriebene Bytes/Sek.  |  Auf den Datenträger geschriebene Bytes/Sek. (Vorschauversion)  |  Anzahl pro Sekunde  |  Average | 
| **Ja**  | Nein |  Microsoft.Compute/virtualMachines  |  Datenträgerschreibvorgänge/Sek.  |  Datenträgerschreibvorgänge/Sek. (Vorschauversion)  |  Anzahl pro Sekunde  |  Average | 
| **Ja**  | Nein |  Microsoft.Compute/virtualMachines  |  Datenträgerlesevorgänge in Bytes  |  Datenträgerlesevorgänge in Bytes  |  Byte  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Compute/virtualMachines  |  Datenträgerlesevorgänge/Sek.  |  Datenträgerlesevorgänge/Sek.  |  Anzahl pro Sekunde  |  Average | 
| **Ja**  | Nein |  Microsoft.Compute/virtualMachines  |  Datenträgerschreibvorgänge in Bytes  |  Datenträgerschreibvorgänge in Bytes  |  Byte  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Compute/virtualMachines  |  Datenträgerschreibvorgänge/Sek.  |  Datenträgerschreibvorgänge/Sek.  |  Anzahl pro Sekunde  |  Average | 
| **Ja**  | Nein |  Microsoft.Compute/virtualMachines  |  Eingehende Datenflüsse  |  Eingehende Datenflüsse  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.Compute/virtualMachines  |  Maximale Erstellungsrate für eingehende Datenflüsse  |  Maximale Erstellungsrate für eingehende Datenflüsse (Vorschauversion)  |  Anzahl pro Sekunde  |  Average | 
| **Ja**  | Nein |  Microsoft.Compute/virtualMachines  |  Netzwerk eingehend  |  Abrechenbarer eingehender Netzwerkdatenverkehr (veraltet)  |  Byte  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Compute/virtualMachines  |  Eingehender Netzwerkverkehr gesamt  |  Eingehender Netzwerkverkehr gesamt  |  Byte  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Compute/virtualMachines  |  Netzwerk ausgehend  |  Abrechenbarer ausgehender Netzwerkdatenverkehr (veraltet)  |  Byte  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Compute/virtualMachines  |  Ausgehender Netzwerkverkehr gesamt  |  Ausgehender Netzwerkverkehr gesamt  |  Byte  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Compute/virtualMachines  |  Warteschlangentiefe für Betriebssystemdatenträger  |  Warteschlangentiefe für Betriebssystemdatenträger (Vorschauversion)  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.Compute/virtualMachines  |  Vom Betriebssystemdatenträger gelesene Bytes/Sek.  |  Vom Betriebssystemdatenträger gelesene Bytes/Sek. (Vorschauversion)  |  Anzahl pro Sekunde  |  Average | 
| **Ja**  | Nein |  Microsoft.Compute/virtualMachines  |  Lesevorgänge auf Betriebssystemdatenträger/Sek.  |  Lesevorgänge auf Betriebssystemdatenträger/Sek.  |  Anzahl pro Sekunde  |  Average | 
| **Ja**  | Nein |  Microsoft.Compute/virtualMachines  |  Auf den Betriebssystemdatenträger geschriebene Bytes/Sek.  |  Auf den Betriebssystemdatenträger geschriebene Bytes/Sek.  |  Anzahl pro Sekunde  |  Average | 
| **Ja**  | Nein |  Microsoft.Compute/virtualMachines  |  Schreibvorgänge auf Betriebssystemdatenträger/Sek.  |  Schreibvorgänge auf Betriebssystemdatenträger/Sek.  |  Anzahl pro Sekunde  |  Average | 
| **Ja**  | Nein |  Microsoft.Compute/virtualMachines  |  Warteschlangentiefe pro Betriebssystemdatenträger  |  Warteschlangentiefe für Betriebssystemdatenträger (veraltet)  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.Compute/virtualMachines  |  Gelesene Bytes pro Betriebssystemdatenträger/Sek.  |  Vom Betriebssystemdatenträger gelesene Bytes/Sek. (veraltet)  |  Anzahl pro Sekunde  |  Average | 
| **Ja**  | Nein |  Microsoft.Compute/virtualMachines  |  Lesevorgänge pro Betriebssystemdatenträger/Sek.  |  Lesevorgänge auf Betriebssystemdatenträger/Sek. (veraltet)  |  Anzahl pro Sekunde  |  Average | 
| **Ja**  | Nein |  Microsoft.Compute/virtualMachines  |  Geschriebene Bytes pro Betriebssystemdatenträger/Sek.  |  Auf den Betriebssystemdatenträger geschriebene Bytes/Sek. (veraltet)  |  Anzahl pro Sekunde  |  Average | 
| **Ja**  | Nein |  Microsoft.Compute/virtualMachines  |  Schreibvorgänge pro Betriebssystemdatenträger/Sek.  |  Schreibvorgänge auf Betriebssystemdatenträger/Sek. (veraltet)  |  Anzahl pro Sekunde  |  Average | 
| **Ja**  | Nein |  Microsoft.Compute/virtualMachines  |  Ausgehende Datenflüsse  |  Ausgehende Datenflüsse  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.Compute/virtualMachines  |  Maximale Erstellungsrate für ausgehende Datenflüsse  |  Maximale Erstellungsrate für ausgehende Datenflüsse (Vorschauversion)  |  Anzahl pro Sekunde  |  Average | 
| **Ja**  | Nein |  Microsoft.Compute/virtualMachines  |  Warteschlangentiefe pro Datenträger  |  Warteschlangentiefe für Datenträger (veraltet)  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.Compute/virtualMachines  |  Gelesene Bytes pro Datenträger/Sek.  |  Vom Datenträger gelesene Bytes/Sek. (veraltet)  |  Anzahl pro Sekunde  |  Average | 
| **Ja**  | Nein |  Microsoft.Compute/virtualMachines  |  Lesevorgänge pro Datenträger/Sek.  |  Datenträgerlesevorgänge/Sek. (veraltet)  |  Anzahl pro Sekunde  |  Average | 
| **Ja**  | Nein |  Microsoft.Compute/virtualMachines  |  Geschriebene Bytes pro Datenträger/Sek.  |  Auf den Datenträger geschriebene Bytes/Sek. (veraltet)  |  Anzahl pro Sekunde  |  Average | 
| **Ja**  | Nein |  Microsoft.Compute/virtualMachines  |  Schreibvorgänge pro Datenträger/Sek.  |  Datenträgerschreibvorgänge/Sek. (veraltet)  |  Anzahl pro Sekunde  |  Average | 
| **Ja**  | Nein |  Microsoft.Compute/virtualMachines  |  CPU in Prozent  |  CPU in Prozent  |  Percent  |  Average | 
| **Ja**  | Nein |  Microsoft.Compute/virtualMachines  |  Treffer beim Cachelesevorgang auf Premium-Datenträger  |  Treffer beim Cachelesevorgang auf Premium-Datenträger (Vorschauversion)  |  Percent  |  Average | 
| **Ja**  | Nein |  Microsoft.Compute/virtualMachines  |  Fehler beim Cachelesevorgang auf Premium-Datenträger  |  Fehler beim Cachelesevorgang auf Premium-Datenträger (Vorschauversion)  |  Percent  |  Average | 
| **Ja**  | Nein |  Microsoft.Compute/virtualMachines  |  Treffer beim Cachelesevorgang auf Premium-Betriebssystemdatenträger  |  Treffer beim Cachelesevorgang auf Premium-Betriebssystemdatenträger (Vorschauversion)  |  Percent  |  Average | 
| **Ja**  | Nein |  Microsoft.Compute/virtualMachines  |  Fehler beim Cachelesevorgang auf Premium-Betriebssystemdatenträger  |  Fehler beim Cachelesevorgang auf Premium-Betriebssystemdatenträger (Vorschauversion)  |  Percent  |  Average | 
| **Ja**  | Nein |  Microsoft.Compute/virtualMachineScaleSets  |  Verbrauchte CPU-Guthaben  |  Verbrauchte CPU-Guthaben  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.Compute/virtualMachineScaleSets  |  Verbleibende CPU-Guthaben  |  Verbleibende CPU-Guthaben  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.Compute/virtualMachineScaleSets  |  Warteschlangentiefe für Datenträger  |  Warteschlangentiefe für Datenträger (Vorschauversion)  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.Compute/virtualMachineScaleSets  |  Vom Datenträger gelesene Bytes/Sek.  |  Vom Datenträger gelesene Bytes/Sek. (Vorschauversion)  |  Anzahl pro Sekunde  |  Average | 
| **Ja**  | Nein |  Microsoft.Compute/virtualMachineScaleSets  |  Datenträgerlesevorgänge/Sek.  |  Datenträgerlesevorgänge/Sek. (Vorschauversion)  |  Anzahl pro Sekunde  |  Average | 
| **Ja**  | Nein |  Microsoft.Compute/virtualMachineScaleSets  |  Auf den Datenträger geschriebene Bytes/Sek.  |  Auf den Datenträger geschriebene Bytes/Sek. (Vorschauversion)  |  Anzahl pro Sekunde  |  Average | 
| **Ja**  | Nein |  Microsoft.Compute/virtualMachineScaleSets  |  Datenträgerschreibvorgänge/Sek.  |  Datenträgerschreibvorgänge/Sek. (Vorschauversion)  |  Anzahl pro Sekunde  |  Average | 
| **Ja**  | Nein |  Microsoft.Compute/virtualMachineScaleSets  |  Datenträgerlesevorgänge in Bytes  |  Datenträgerlesevorgänge in Bytes  |  Byte  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Compute/virtualMachineScaleSets  |  Datenträgerlesevorgänge/Sek.  |  Datenträgerlesevorgänge/Sek.  |  Anzahl pro Sekunde  |  Average | 
| **Ja**  | Nein |  Microsoft.Compute/virtualMachineScaleSets  |  Datenträgerschreibvorgänge in Bytes  |  Datenträgerschreibvorgänge in Bytes  |  Byte  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Compute/virtualMachineScaleSets  |  Datenträgerschreibvorgänge/Sek.  |  Datenträgerschreibvorgänge/Sek.  |  Anzahl pro Sekunde  |  Average | 
| **Ja**  | Nein |  Microsoft.Compute/virtualMachineScaleSets  |  Eingehende Datenflüsse  |  Eingehende Datenflüsse  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.Compute/virtualMachineScaleSets  |  Maximale Erstellungsrate für eingehende Datenflüsse  |  Maximale Erstellungsrate für eingehende Datenflüsse (Vorschauversion)  |  Anzahl pro Sekunde  |  Average | 
| **Ja**  | Nein |  Microsoft.Compute/virtualMachineScaleSets  |  Netzwerk eingehend  |  Abrechenbarer eingehender Netzwerkdatenverkehr (veraltet)  |  Byte  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Compute/virtualMachineScaleSets  |  Eingehender Netzwerkverkehr gesamt  |  Eingehender Netzwerkverkehr gesamt  |  Byte  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Compute/virtualMachineScaleSets  |  Netzwerk ausgehend  |  Abrechenbarer ausgehender Netzwerkdatenverkehr (veraltet)  |  Byte  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Compute/virtualMachineScaleSets  |  Ausgehender Netzwerkverkehr gesamt  |  Ausgehender Netzwerkverkehr gesamt  |  Byte  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Compute/virtualMachineScaleSets  |  Warteschlangentiefe für Betriebssystemdatenträger  |  Warteschlangentiefe für Betriebssystemdatenträger (Vorschauversion)  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.Compute/virtualMachineScaleSets  |  Vom Betriebssystemdatenträger gelesene Bytes/Sek.  |  Vom Betriebssystemdatenträger gelesene Bytes/Sek. (Vorschauversion)  |  Anzahl pro Sekunde  |  Average | 
| **Ja**  | Nein |  Microsoft.Compute/virtualMachineScaleSets  |  Lesevorgänge auf Betriebssystemdatenträger/Sek.  |  Lesevorgänge auf Betriebssystemdatenträger/Sek.  |  Anzahl pro Sekunde  |  Average | 
| **Ja**  | Nein |  Microsoft.Compute/virtualMachineScaleSets  |  Auf den Betriebssystemdatenträger geschriebene Bytes/Sek.  |  Auf den Betriebssystemdatenträger geschriebene Bytes/Sek.  |  Anzahl pro Sekunde  |  Average | 
| **Ja**  | Nein |  Microsoft.Compute/virtualMachineScaleSets  |  Schreibvorgänge auf Betriebssystemdatenträger/Sek.  |  Schreibvorgänge auf Betriebssystemdatenträger/Sek.  |  Anzahl pro Sekunde  |  Average | 
| **Ja**  | Nein |  Microsoft.Compute/virtualMachineScaleSets  |  Warteschlangentiefe pro Betriebssystemdatenträger  |  Warteschlangentiefe für Betriebssystemdatenträger (veraltet)  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.Compute/virtualMachineScaleSets  |  Gelesene Bytes pro Betriebssystemdatenträger/Sek.  |  Vom Betriebssystemdatenträger gelesene Bytes/Sek. (veraltet)  |  Anzahl pro Sekunde  |  Average | 
| **Ja**  | Nein |  Microsoft.Compute/virtualMachineScaleSets  |  Lesevorgänge pro Betriebssystemdatenträger/Sek.  |  Lesevorgänge auf Betriebssystemdatenträger/Sek. (veraltet)  |  Anzahl pro Sekunde  |  Average | 
| **Ja**  | Nein |  Microsoft.Compute/virtualMachineScaleSets  |  Geschriebene Bytes pro Betriebssystemdatenträger/Sek.  |  Auf den Betriebssystemdatenträger geschriebene Bytes/Sek. (veraltet)  |  Anzahl pro Sekunde  |  Average | 
| **Ja**  | Nein |  Microsoft.Compute/virtualMachineScaleSets  |  Schreibvorgänge pro Betriebssystemdatenträger/Sek.  |  Schreibvorgänge auf Betriebssystemdatenträger/Sek. (veraltet)  |  Anzahl pro Sekunde  |  Average | 
| **Ja**  | Nein |  Microsoft.Compute/virtualMachineScaleSets  |  Ausgehende Datenflüsse  |  Ausgehende Datenflüsse  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.Compute/virtualMachineScaleSets  |  Maximale Erstellungsrate für ausgehende Datenflüsse  |  Maximale Erstellungsrate für ausgehende Datenflüsse (Vorschauversion)  |  Anzahl pro Sekunde  |  Average | 
| **Ja**  | Nein |  Microsoft.Compute/virtualMachineScaleSets  |  Warteschlangentiefe pro Datenträger  |  Warteschlangentiefe für Datenträger (veraltet)  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.Compute/virtualMachineScaleSets  |  Gelesene Bytes pro Datenträger/Sek.  |  Vom Datenträger gelesene Bytes/Sek. (veraltet)  |  Anzahl pro Sekunde  |  Average | 
| **Ja**  | Nein |  Microsoft.Compute/virtualMachineScaleSets  |  Lesevorgänge pro Datenträger/Sek.  |  Datenträgerlesevorgänge/Sek. (veraltet)  |  Anzahl pro Sekunde  |  Average | 
| **Ja**  | Nein |  Microsoft.Compute/virtualMachineScaleSets  |  Geschriebene Bytes pro Datenträger/Sek.  |  Auf den Datenträger geschriebene Bytes/Sek. (veraltet)  |  Anzahl pro Sekunde  |  Average | 
| **Ja**  | Nein |  Microsoft.Compute/virtualMachineScaleSets  |  Schreibvorgänge pro Datenträger/Sek.  |  Datenträgerschreibvorgänge/Sek. (veraltet)  |  Anzahl pro Sekunde  |  Average | 
| **Ja**  | Nein |  Microsoft.Compute/virtualMachineScaleSets  |  CPU in Prozent  |  CPU in Prozent  |  Percent  |  Average | 
| **Ja**  | Nein |  Microsoft.Compute/virtualMachineScaleSets  |  Treffer beim Cachelesevorgang auf Premium-Datenträger  |  Treffer beim Cachelesevorgang auf Premium-Datenträger (Vorschauversion)  |  Percent  |  Average | 
| **Ja**  | Nein |  Microsoft.Compute/virtualMachineScaleSets  |  Fehler beim Cachelesevorgang auf Premium-Datenträger  |  Fehler beim Cachelesevorgang auf Premium-Datenträger (Vorschauversion)  |  Percent  |  Average | 
| **Ja**  | Nein |  Microsoft.Compute/virtualMachineScaleSets  |  Treffer beim Cachelesevorgang auf Premium-Betriebssystemdatenträger  |  Treffer beim Cachelesevorgang auf Premium-Betriebssystemdatenträger (Vorschauversion)  |  Percent  |  Average | 
| **Ja**  | Nein |  Microsoft.Compute/virtualMachineScaleSets  |  Fehler beim Cachelesevorgang auf Premium-Betriebssystemdatenträger  |  Fehler beim Cachelesevorgang auf Premium-Betriebssystemdatenträger (Vorschauversion)  |  Percent  |  Average | 
| **Ja**  | Nein |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Verbrauchte CPU-Guthaben  |  Verbrauchte CPU-Guthaben  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Verbleibende CPU-Guthaben  |  Verbleibende CPU-Guthaben  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Warteschlangentiefe für Datenträger  |  Warteschlangentiefe für Datenträger (Vorschauversion)  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Vom Datenträger gelesene Bytes/Sek.  |  Vom Datenträger gelesene Bytes/Sek. (Vorschauversion)  |  Anzahl pro Sekunde  |  Average | 
| **Ja**  | Nein |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Datenträgerlesevorgänge/Sek.  |  Datenträgerlesevorgänge/Sek. (Vorschauversion)  |  Anzahl pro Sekunde  |  Average | 
| **Ja**  | Nein |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Auf den Datenträger geschriebene Bytes/Sek.  |  Auf den Datenträger geschriebene Bytes/Sek. (Vorschauversion)  |  Anzahl pro Sekunde  |  Average | 
| **Ja**  | Nein |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Datenträgerschreibvorgänge/Sek.  |  Datenträgerschreibvorgänge/Sek. (Vorschauversion)  |  Anzahl pro Sekunde  |  Average | 
| **Ja**  | Nein |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Datenträgerlesevorgänge in Bytes  |  Datenträgerlesevorgänge in Bytes  |  Byte  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Datenträgerlesevorgänge/Sek.  |  Datenträgerlesevorgänge/Sek.  |  Anzahl pro Sekunde  |  Average | 
| **Ja**  | Nein |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Datenträgerschreibvorgänge in Bytes  |  Datenträgerschreibvorgänge in Bytes  |  Byte  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Datenträgerschreibvorgänge/Sek.  |  Datenträgerschreibvorgänge/Sek.  |  Anzahl pro Sekunde  |  Average | 
| **Ja**  | Nein |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Eingehende Datenflüsse  |  Eingehende Datenflüsse  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Maximale Erstellungsrate für eingehende Datenflüsse  |  Maximale Erstellungsrate für eingehende Datenflüsse (Vorschauversion)  |  Anzahl pro Sekunde  |  Average | 
| **Ja**  | Nein |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Netzwerk eingehend  |  Abrechenbarer eingehender Netzwerkdatenverkehr (veraltet)  |  Byte  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Eingehender Netzwerkverkehr gesamt  |  Eingehender Netzwerkverkehr gesamt  |  Byte  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Netzwerk ausgehend  |  Abrechenbarer ausgehender Netzwerkdatenverkehr (veraltet)  |  Byte  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Ausgehender Netzwerkverkehr gesamt  |  Ausgehender Netzwerkverkehr gesamt  |  Byte  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Warteschlangentiefe für Betriebssystemdatenträger  |  Warteschlangentiefe für Betriebssystemdatenträger (Vorschauversion)  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Vom Betriebssystemdatenträger gelesene Bytes/Sek.  |  Vom Betriebssystemdatenträger gelesene Bytes/Sek. (Vorschauversion)  |  Anzahl pro Sekunde  |  Average | 
| **Ja**  | Nein |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Lesevorgänge auf Betriebssystemdatenträger/Sek.  |  Lesevorgänge auf Betriebssystemdatenträger/Sek.  |  Anzahl pro Sekunde  |  Average | 
| **Ja**  | Nein |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Auf den Betriebssystemdatenträger geschriebene Bytes/Sek.  |  Auf den Betriebssystemdatenträger geschriebene Bytes/Sek.  |  Anzahl pro Sekunde  |  Average | 
| **Ja**  | Nein |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Schreibvorgänge auf Betriebssystemdatenträger/Sek.  |  Schreibvorgänge auf Betriebssystemdatenträger/Sek.  |  Anzahl pro Sekunde  |  Average | 
| **Ja**  | Nein |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Warteschlangentiefe pro Betriebssystemdatenträger  |  Warteschlangentiefe für Betriebssystemdatenträger (veraltet)  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Gelesene Bytes pro Betriebssystemdatenträger/Sek.  |  Vom Betriebssystemdatenträger gelesene Bytes/Sek. (veraltet)  |  Anzahl pro Sekunde  |  Average | 
| **Ja**  | Nein |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Lesevorgänge pro Betriebssystemdatenträger/Sek.  |  Lesevorgänge auf Betriebssystemdatenträger/Sek. (veraltet)  |  Anzahl pro Sekunde  |  Average | 
| **Ja**  | Nein |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Geschriebene Bytes pro Betriebssystemdatenträger/Sek.  |  Auf den Betriebssystemdatenträger geschriebene Bytes/Sek. (veraltet)  |  Anzahl pro Sekunde  |  Average | 
| **Ja**  | Nein |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Schreibvorgänge pro Betriebssystemdatenträger/Sek.  |  Schreibvorgänge auf Betriebssystemdatenträger/Sek. (veraltet)  |  Anzahl pro Sekunde  |  Average | 
| **Ja**  | Nein |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Ausgehende Datenflüsse  |  Ausgehende Datenflüsse  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Maximale Erstellungsrate für ausgehende Datenflüsse  |  Maximale Erstellungsrate für ausgehende Datenflüsse (Vorschauversion)  |  Anzahl pro Sekunde  |  Average | 
| **Ja**  | Nein |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Warteschlangentiefe pro Datenträger  |  Warteschlangentiefe für Datenträger (veraltet)  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Gelesene Bytes pro Datenträger/Sek.  |  Vom Datenträger gelesene Bytes/Sek. (veraltet)  |  Anzahl pro Sekunde  |  Average | 
| **Ja**  | Nein |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Lesevorgänge pro Datenträger/Sek.  |  Datenträgerlesevorgänge/Sek. (veraltet)  |  Anzahl pro Sekunde  |  Average | 
| **Ja**  | Nein |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Geschriebene Bytes pro Datenträger/Sek.  |  Auf den Datenträger geschriebene Bytes/Sek. (veraltet)  |  Anzahl pro Sekunde  |  Average | 
| **Ja**  | Nein |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Schreibvorgänge pro Datenträger/Sek.  |  Datenträgerschreibvorgänge/Sek. (veraltet)  |  Anzahl pro Sekunde  |  Average | 
| **Ja**  | Nein |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  CPU in Prozent  |  CPU in Prozent  |  Percent  |  Average | 
| **Ja**  | Nein |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Treffer beim Cachelesevorgang auf Premium-Datenträger  |  Treffer beim Cachelesevorgang auf Premium-Datenträger (Vorschauversion)  |  Percent  |  Average | 
| **Ja**  | Nein |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Fehler beim Cachelesevorgang auf Premium-Datenträger  |  Fehler beim Cachelesevorgang auf Premium-Datenträger (Vorschauversion)  |  Percent  |  Average | 
| **Ja**  | Nein |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Treffer beim Cachelesevorgang auf Premium-Betriebssystemdatenträger  |  Treffer beim Cachelesevorgang auf Premium-Betriebssystemdatenträger (Vorschauversion)  |  Percent  |  Average | 
| **Ja**  | Nein |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Fehler beim Cachelesevorgang auf Premium-Betriebssystemdatenträger  |  Fehler beim Cachelesevorgang auf Premium-Betriebssystemdatenträger (Vorschauversion)  |  Percent  |  Average | 
| **Ja**  | Nein |  Microsoft.ContainerInstance/containerGroups  |  CpuUsage  |  CPU-Auslastung  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.ContainerInstance/containerGroups  |  MemoryUsage  |  Speicherauslastung  |  Byte  |  Average | 
| **Ja**  | Nein |  Microsoft.ContainerInstance/containerGroups  |  NetworkBytesReceivedPerSecond  |  Empfangene Netzwerkbytes pro Sekunde  |  Byte  |  Average | 
| **Ja**  | Nein |  Microsoft.ContainerInstance/containerGroups  |  NetworkBytesTransmittedPerSecond  |  Übertragene Netzwerkbytes pro Sekunde  |  Byte  |  Average | 
| **Ja**  | Nein |  Microsoft.ContainerRegistry/registries  |  RunDuration  |  Ausführungsdauer  |  Millisekunden  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.ContainerRegistry/registries  |  SuccessfulPullCount  |  Anzahl erfolgreicher Pullvorgänge  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.ContainerRegistry/registries  |  SuccessfulPushCount  |  Anzahl erfolgreicher Puchvorgänge  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.ContainerRegistry/registries  |  TotalPullCount  |  Gesamtzahl von Pullvorgängen  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.ContainerRegistry/registries  |  TotalPushCount  |  Gesamtzahl von Pushvorgängen  |  Anzahl  |  Average | 
| Nein  | Nein |  Microsoft.ContainerService/managedClusters  |  kube_node_status_allocatable_cpu_cores  |  Gesamtzahl der verfügbaren CPU-Kerne in einem verwalteten Cluster  |  Anzahl  |  Average | 
| Nein  | Nein |  Microsoft.ContainerService/managedClusters  |  kube_node_status_allocatable_memory_bytes  |  Gesamtzahl des verfügbaren Speicherplatzes in einem verwalteten Cluster  |  Byte  |  Average | 
| Nein  | Nein |  Microsoft.ContainerService/managedClusters  |  kube_node_status_condition  |  Status für verschiedene Knotenbedingungen  |  Anzahl  |  Average | 
| Nein  | Nein |  Microsoft.ContainerService/managedClusters  |  kube_pod_status_phase  |  Anzahl der Pods nach Phase  |  Anzahl  |  Average | 
| Nein  | Nein |  Microsoft.ContainerService/managedClusters  |  kube_pod_status_ready  |  Anzahl der Pods mit dem Status „Bereit“  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.DataBoxEdge/dataBoxEdgeDevices  |  AvailableCapacity  |  Verfügbare Kapazität  |  Byte  |  Average | 
| **Ja**  | Nein |  Microsoft.DataBoxEdge/dataBoxEdgeDevices  |  BytesUploadedToCloud  |  Hochgeladene Cloudbytes (Gerät)  |  Byte  |  Average | 
| **Ja**  | Nein |  Microsoft.DataBoxEdge/dataBoxEdgeDevices  |  BytesUploadedToCloudPerShare  |  Hochgeladene Cloudbytes (Freigabe)  |  Byte  |  Average | 
| **Ja**  | Nein |  Microsoft.DataBoxEdge/dataBoxEdgeDevices  |  CloudReadThroughput  |  Clouddownloaddurchsatz  |  Bytes pro Sekunde  |  Average | 
| **Ja**  | Nein |  Microsoft.DataBoxEdge/dataBoxEdgeDevices  |  CloudReadThroughputPerShare  |  Clouddownloaddurchsatz (Freigabe)  |  Bytes pro Sekunde  |  Average | 
| **Ja**  | Nein |  Microsoft.DataBoxEdge/dataBoxEdgeDevices  |  CloudUploadThroughput  |  Clouduploaddurchsatz  |  Bytes pro Sekunde  |  Average | 
| **Ja**  | Nein |  Microsoft.DataBoxEdge/dataBoxEdgeDevices  |  CloudUploadThroughputPerShare  |  Clouduploaddurchsatz (Freigabe)  |  Bytes pro Sekunde  |  Average | 
| **Ja**  | Nein |  Microsoft.DataBoxEdge/dataBoxEdgeDevices  |  HyperVMemoryUtilization  |  Edgecomputing – Arbeitsspeichernutzung  |  Percent  |  Average | 
| **Ja**  | Nein |  Microsoft.DataBoxEdge/dataBoxEdgeDevices  |  HyperVVirtualProcessorUtilization  |  Edgecomputing – CPU in Prozent  |  Percent  |  Average | 
| **Ja**  | Nein |  Microsoft.DataBoxEdge/dataBoxEdgeDevices  |  NICReadThroughput  |  Lesedurchsatz (Netzwerk)  |  Bytes pro Sekunde  |  Average | 
| **Ja**  | Nein |  Microsoft.DataBoxEdge/dataBoxEdgeDevices  |  NICWriteThroughput  |  Schreibdurchsatz (Netzwerk)  |  Bytes pro Sekunde  |  Average | 
| **Ja**  | Nein |  Microsoft.DataBoxEdge/dataBoxEdgeDevices  |  TotalCapacity  |  Gesamtkapazität  |  Byte  |  Average | 
| **Ja**  | Nein |  Microsoft.DataFactory/datafactories  |  FailedRuns  |  Fehlerhafte Ausführungen  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.DataFactory/datafactories  |  SuccessfulRuns  |  Erfolgreiche Ausführungen  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.DataFactory/factories  |  ActivityCancelledRuns  |  Metriken zu abgebrochenen Aktivitätsausführungen  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.DataFactory/factories  |  ActivityFailedRuns  |  Metriken zu fehlerhaften Aktivitätsausführungen  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.DataFactory/factories  |  ActivitySucceededRuns  |  Metriken zu erfolgreichen Aktivitätsausführungen  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.DataFactory/factories  |  FactorySizeInGbUnits  |  Gesamtgröße der Factory (Einheit: GB)  |  Anzahl  |  Maximum | 
| **Ja**  | Nein |  Microsoft.DataFactory/factories  |  IntegrationRuntimeAvailableMemory  |  Verfügbarer Speicher für Integration Runtime  |  Byte  |  Average | 
| **Ja**  | Nein |  Microsoft.DataFactory/factories  |  IntegrationRuntimeAverageTaskPickupDelay  |  Dauer der Integration Runtime-Warteschlange  |  Sekunden  |  Average | 
| **Ja**  | Nein |  Microsoft.DataFactory/factories  |  IntegrationRuntimeCpuPercentage  |  CPU-Auslastung der Integration Runtime  |  Percent  |  Average | 
| **Ja**  | Nein |  Microsoft.DataFactory/factories  |  IntegrationRuntimeQueueLength  |  Länge der Integration Runtime-Warteschlange  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.DataFactory/factories  |  MaxAllowedFactorySizeInGbUnits  |  Maximal zulässige Factorygröße (Einheit: GB)  |  Anzahl  |  Maximum | 
| **Ja**  | Nein |  Microsoft.DataFactory/factories  |  MaxAllowedResourceCount  |  Anzahl maximal zulässiger Entitäten  |  Anzahl  |  Maximum | 
| **Ja**  | Nein |  Microsoft.DataFactory/factories  |  PipelineCancelledRuns  |  Metriken zu abgebrochenen Pipelineausführungen  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.DataFactory/factories  |  PipelineFailedRuns  |  Metriken zu fehlerhaften Pipelineausführungen  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.DataFactory/factories  |  PipelineSucceededRuns  |  Metriken zu erfolgreichen Pipelineausführungen  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.DataFactory/factories  |  ResourceCount  |  Gesamtzahl von Entitäten  |  Anzahl  |  Maximum | 
| **Ja**  | Nein |  Microsoft.DataFactory/factories  |  TriggerCancelledRuns  |  Metriken zu abgebrochenen Triggerausführungen  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.DataFactory/factories  |  TriggerFailedRuns  |  Metriken zu fehlerhaften Triggerausführungen  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.DataFactory/factories  |  TriggerSucceededRuns  |  Metriken zu erfolgreichen Triggerausführungen  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.DataLakeAnalytics/accounts  |  JobAUEndedCancelled  |  Abgebrochene AU-Zeit  |  Sekunden  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.DataLakeAnalytics/accounts  |  JobAUEndedFailure  |  Fehlgeschlagene AU-Zeit  |  Sekunden  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.DataLakeAnalytics/accounts  |  JobAUEndedSuccess  |  Erfolgreiche AU-Zeit  |  Sekunden  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.DataLakeAnalytics/accounts  |  JobEndedCancelled  |  Abgebrochene Vorgänge  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.DataLakeAnalytics/accounts  |  JobEndedFailure  |  Fehlgeschlagene Vorgänge  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.DataLakeAnalytics/accounts  |  JobEndedSuccess  |  Erfolgreiche Vorgänge  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.DataLakeStore/accounts  |  DataRead  |  Gelesene Daten  |  Byte  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.DataLakeStore/accounts  |  DataWritten  |  Geschriebene Daten  |  Byte  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.DataLakeStore/accounts  |  ReadRequests  |  Leseanforderungen  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.DataLakeStore/accounts  |  TotalStorage  |  Gesamtspeicher  |  Byte  |  Maximum | 
| **Ja**  | Nein |  Microsoft.DataLakeStore/accounts  |  WriteRequests  |  Schreibanforderungen  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.DBforMariaDB/servers  |  active_connections  |  Die aktiven Verbindungen.  |  Anzahl  |  Average | 
| **Ja**  | **Ja** |  Microsoft.DBforMariaDB/servers  |  backup_storage_used  |  Verwendeter Sicherungsspeicher  |  Byte  |  Average | 
| **Ja**  | **Ja** |  Microsoft.DBforMariaDB/servers  |  connections_failed  |  Verbindungsfehler  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.DBforMariaDB/servers  |  cpu_percent  |  CPU in Prozent  |  Percent  |  Average | 
| **Ja**  | **Ja** |  Microsoft.DBforMariaDB/servers  |  io_consumption_percent  |  E/A in Prozent  |  Percent  |  Average | 
| **Ja**  | **Ja** |  Microsoft.DBforMariaDB/servers  |  memory_percent  |  Arbeitsspeicher in Prozent  |  Percent  |  Average | 
| **Ja**  | **Ja** |  Microsoft.DBforMariaDB/servers  |  network_bytes_egress  |  Netzwerk ausgehend  |  Byte  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.DBforMariaDB/servers  |  network_bytes_ingress  |  Netzwerk eingehend  |  Byte  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.DBforMariaDB/servers  |  seconds_behind_master  |  Replikationsverzögerung in Sekunden  |  Anzahl  |  Maximum | 
| **Ja**  | **Ja** |  Microsoft.DBforMariaDB/servers  |  serverlog_storage_limit  |  Begrenzung des Serverprotokollspeichers  |  Byte  |  Average | 
| **Ja**  | **Ja** |  Microsoft.DBforMariaDB/servers  |  serverlog_storage_percent  |  Serverprotokollspeicher in Prozent  |  Percent  |  Average | 
| **Ja**  | **Ja** |  Microsoft.DBforMariaDB/servers  |  serverlog_storage_usage  |  Verwendeter Serverprotokollspeicher  |  Byte  |  Average | 
| **Ja**  | **Ja** |  Microsoft.DBforMariaDB/servers  |  storage_limit  |  Speicherbegrenzung  |  Byte  |  Maximum | 
| **Ja**  | **Ja** |  Microsoft.DBforMariaDB/servers  |  storage_percent  |  Speicher in Prozent  |  Percent  |  Average | 
| **Ja**  | **Ja** |  Microsoft.DBforMariaDB/servers  |  storage_used  |  Verwendeter Speicher  |  Byte  |  Average | 
| **Ja**  | **Ja** |  Microsoft.DBforMySQL/servers  |  active_connections  |  Die aktiven Verbindungen.  |  Anzahl  |  Average | 
| **Ja**  | **Ja** |  Microsoft.DBforMySQL/servers  |  backup_storage_used  |  Verwendeter Sicherungsspeicher  |  Byte  |  Average | 
| **Ja**  | **Ja** |  Microsoft.DBforMySQL/servers  |  connections_failed  |  Verbindungsfehler  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.DBforMySQL/servers  |  cpu_percent  |  CPU in Prozent  |  Percent  |  Average | 
| **Ja**  | **Ja** |  Microsoft.DBforMySQL/servers  |  io_consumption_percent  |  E/A in Prozent  |  Percent  |  Average | 
| **Ja**  | **Ja** |  Microsoft.DBforMySQL/servers  |  memory_percent  |  Arbeitsspeicher in Prozent  |  Percent  |  Average | 
| **Ja**  | **Ja** |  Microsoft.DBforMySQL/servers  |  network_bytes_egress  |  Netzwerk ausgehend  |  Byte  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.DBforMySQL/servers  |  network_bytes_ingress  |  Netzwerk eingehend  |  Byte  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.DBforMySQL/servers  |  seconds_behind_master  |  Replikationsverzögerung in Sekunden  |  Anzahl  |  Maximum | 
| **Ja**  | **Ja** |  Microsoft.DBforMySQL/servers  |  serverlog_storage_limit  |  Begrenzung des Serverprotokollspeichers  |  Byte  |  Maximum | 
| **Ja**  | **Ja** |  Microsoft.DBforMySQL/servers  |  serverlog_storage_percent  |  Serverprotokollspeicher in Prozent  |  Percent  |  Average | 
| **Ja**  | **Ja** |  Microsoft.DBforMySQL/servers  |  serverlog_storage_usage  |  Verwendeter Serverprotokollspeicher  |  Byte  |  Average | 
| **Ja**  | **Ja** |  Microsoft.DBforMySQL/servers  |  storage_limit  |  Speicherbegrenzung  |  Byte  |  Maximum | 
| **Ja**  | **Ja** |  Microsoft.DBforMySQL/servers  |  storage_percent  |  Speicher in Prozent  |  Percent  |  Average | 
| **Ja**  | **Ja** |  Microsoft.DBforMySQL/servers  |  storage_used  |  Verwendeter Speicher  |  Byte  |  Average | 
| **Ja**  | Nein |  Microsoft.DBforPostgreSQL/servers  |  active_connections  |  Die aktiven Verbindungen.  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.DBforPostgreSQL/servers  |  backup_storage_used  |  Verwendeter Sicherungsspeicher  |  Byte  |  Average | 
| **Ja**  | Nein |  Microsoft.DBforPostgreSQL/servers  |  connections_failed  |  Verbindungsfehler  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.DBforPostgreSQL/servers  |  cpu_percent  |  CPU in Prozent  |  Percent  |  Average | 
| **Ja**  | Nein |  Microsoft.DBforPostgreSQL/servers  |  io_consumption_percent  |  E/A in Prozent  |  Percent  |  Average | 
| **Ja**  | Nein |  Microsoft.DBforPostgreSQL/servers  |  memory_percent  |  Arbeitsspeicher in Prozent  |  Percent  |  Average | 
| **Ja**  | Nein |  Microsoft.DBforPostgreSQL/servers  |  network_bytes_egress  |  Netzwerk ausgehend  |  Byte  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.DBforPostgreSQL/servers  |  network_bytes_ingress  |  Netzwerk eingehend  |  Byte  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.DBforPostgreSQL/servers  |  pg_replica_log_delay_in_bytes  |  Maximale Verzögerung zwischen Replikaten  |  Byte  |  Maximum | 
| **Ja**  | Nein |  Microsoft.DBforPostgreSQL/servers  |  pg_replica_log_delay_in_seconds  |  Replikatverzögerung  |  Sekunden  |  Maximum | 
| **Ja**  | Nein |  Microsoft.DBforPostgreSQL/servers  |  serverlog_storage_limit  |  Begrenzung des Serverprotokollspeichers  |  Byte  |  Maximum | 
| **Ja**  | Nein |  Microsoft.DBforPostgreSQL/servers  |  serverlog_storage_percent  |  Serverprotokollspeicher in Prozent  |  Percent  |  Average | 
| **Ja**  | Nein |  Microsoft.DBforPostgreSQL/servers  |  serverlog_storage_usage  |  Verwendeter Serverprotokollspeicher  |  Byte  |  Average | 
| **Ja**  | Nein |  Microsoft.DBforPostgreSQL/servers  |  storage_limit  |  Speicherbegrenzung  |  Byte  |  Maximum | 
| **Ja**  | Nein |  Microsoft.DBforPostgreSQL/servers  |  storage_percent  |  Speicher in Prozent  |  Percent  |  Average | 
| **Ja**  | Nein |  Microsoft.DBforPostgreSQL/servers  |  storage_used  |  Verwendeter Speicher  |  Byte  |  Average | 
| **Ja**  | Nein |  Microsoft.DBforPostgreSQL/serversv2  |  active_connections  |  Die aktiven Verbindungen.  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.DBforPostgreSQL/serversv2  |  cpu_percent  |  CPU in Prozent  |  Percent  |  Average | 
| **Ja**  | Nein |  Microsoft.DBforPostgreSQL/serversv2  |  iops  |  IOPS  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.DBforPostgreSQL/serversv2  |  memory_percent  |  Arbeitsspeicher in Prozent  |  Percent  |  Average | 
| **Ja**  | Nein |  Microsoft.DBforPostgreSQL/serversv2  |  network_bytes_egress  |  Netzwerk ausgehend  |  Byte  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.DBforPostgreSQL/serversv2  |  network_bytes_ingress  |  Netzwerk eingehend  |  Byte  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.DBforPostgreSQL/serversv2  |  storage_percent  |  Speicher in Prozent  |  Percent  |  Average | 
| **Ja**  | Nein |  Microsoft.DBforPostgreSQL/serversv2  |  storage_used  |  Verwendeter Speicher  |  Byte  |  Average | 
| **Ja**  | **Ja** |  Microsoft.Devices/Account  |  digitaltwins.telemetry.nodes  |  Platzhalter für Digital Twins-Knotentelemetrie  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Devices/IotHubs  |  c2d.commands.egress.abandon.success  |  Abgebrochene C2D-Nachrichten  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Devices/IotHubs  |  c2d.commands.egress.complete.success  |  Abgeschlossene C2D-Nachrichtenübermittlungen  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Devices/IotHubs  |  c2d.commands.egress.reject.success  |  Abgelehnte C2D-Nachrichten  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Devices/IotHubs  |  c2d.methods.failure  |  Failed direct method invocations (Nicht erfolgreiche direkte Methodenaufrufe)  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Devices/IotHubs  |  c2d.methods.requestSize  |  Request size of direct method invocations (Anforderungsgröße von direkten Methodenaufrufen)  |  Byte  |  Average | 
| **Ja**  | **Ja** |  Microsoft.Devices/IotHubs  |  c2d.methods.responseSize  |  Response size of direct method invocations (Antwortgröße von direkten Methodenaufrufen)  |  Byte  |  Average | 
| **Ja**  | **Ja** |  Microsoft.Devices/IotHubs  |  c2d.methods.success  |  Successful direct method invocations (Erfolgreiche direkte Methodenaufrufvorgänge)  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Devices/IotHubs  |  c2d.twin.read.failure  |  Failed twin reads from back end (Nicht erfolgreiche Zwillingslesevorgänge vom Back-End)  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Devices/IotHubs  |  c2d.twin.read.size  |  Response size of twin reads from back end (Antwortgröße von Zwillingslesevorgängen vom Back-End)  |  Byte  |  Average | 
| **Ja**  | **Ja** |  Microsoft.Devices/IotHubs  |  c2d.twin.read.success  |  Successful twin reads from back end (Erfolgreiche Zwillingslesevorgänge vom Back-End)  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Devices/IotHubs  |  c2d.twin.update.failure  |  Failed twin updates from back end (Nicht erfolgreiche Zwillingsaktualisierungen vom Back-End)  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Devices/IotHubs  |  c2d.twin.update.size  |  Size of twin updates from back end (Größe der Zwillingsaktualisierungen vom Back-End)  |  Byte  |  Average | 
| **Ja**  | **Ja** |  Microsoft.Devices/IotHubs  |  c2d.twin.update.success  |  Successful twin updates from back end (Erfolgreiche Zwillingsaktualisierungen vom Back-End)  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Devices/IotHubs  |  C2DMessagesExpired  |  Abgelaufene C2D-Nachrichten (Vorschau)  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Devices/IotHubs  |  Konfigurationen  |  Konfigurationsmetriken  |  Anzahl  |  Gesamt | 
| Nein  | Nein |  Microsoft.Devices/IotHubs  |  connectedDeviceCount  |  Verbundene Geräte (Vorschau)  |  Anzahl  |  Average | 
| **Ja**  | **Ja** |  Microsoft.Devices/IotHubs  |  d2c.endpoints.egress.builtIn.events  |  Routing: An Nachrichten/Ereignisse übermittelte Nachrichten  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Devices/IotHubs  |  d2c.endpoints.egress.eventHubs  |  Routing: An Event Hub übermittelte Nachrichten  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Devices/IotHubs  |  d2c.endpoints.egress.serviceBusQueues  |  Routing: An Service Bus-Warteschlange übermittelte Nachrichten  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Devices/IotHubs  |  d2c.endpoints.egress.serviceBusTopics  |  Routing: An Service Bus-Thema übermittelte Nachrichten  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Devices/IotHubs  |  d2c.endpoints.egress.storage  |  Routing: An den Speicher übermittelte Nachrichten  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Devices/IotHubs  |  d2c.endpoints.egress.storage.blobs  |  Routing: An den Speicher übermittelte Blobs  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Devices/IotHubs  |  d2c.endpoints.egress.storage.bytes  |  Routing: An den Speicher übermittelte Daten  |  Byte  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Devices/IotHubs  |  d2c.endpoints.latency.builtIn.events  |  Routing: Nachrichtenwartezeit für Nachrichten/Ereignisse  |  Millisekunden  |  Average | 
| **Ja**  | **Ja** |  Microsoft.Devices/IotHubs  |  d2c.endpoints.latency.eventHubs  |  Routing: Nachrichtenwartezeit für Event Hub  |  Millisekunden  |  Average | 
| **Ja**  | **Ja** |  Microsoft.Devices/IotHubs  |  d2c.endpoints.latency.serviceBusQueues  |  Routing: Nachrichtenwartezeit für Service Bus-Warteschlange  |  Millisekunden  |  Average | 
| **Ja**  | **Ja** |  Microsoft.Devices/IotHubs  |  d2c.endpoints.latency.serviceBusTopics  |  Routing: Nachrichtenwartezeit für Service Bus-Thema  |  Millisekunden  |  Average | 
| **Ja**  | **Ja** |  Microsoft.Devices/IotHubs  |  d2c.endpoints.latency.storage  |  Routing: Nachrichtenwartezeit für Speicher  |  Millisekunden  |  Average | 
| **Ja**  | **Ja** |  Microsoft.Devices/IotHubs  |  d2c.telemetry.egress.dropped  |  Routing: Verworfene Telemetrienachrichten   |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Devices/IotHubs  |  d2c.telemetry.egress.fallback  |  Routing: An den Fallback übermittelte Nachrichten  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Devices/IotHubs  |  d2c.telemetry.egress.invalid  |  Routing: Nicht kompatible Telemetrienachrichten  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Devices/IotHubs  |  d2c.telemetry.egress.orphaned  |  Routing: Verwaiste Telemetrienachrichten   |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Devices/IotHubs  |  d2c.telemetry.egress.success  |  Routing: Übermittelte Telemetrienachrichten  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Devices/IotHubs  |  d2c.telemetry.Ingress.allProtocol  |  Telemetry message send attempts (Sendeversuche für Telemetrienachrichten)  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Devices/IotHubs  |  d2c.telemetry.ingress.sendThrottle  |  Anzahl von Drosselungsfehlern  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Devices/IotHubs  |  d2c.telemetry.ingress.success  |  Telemetry messages sent (Gesendete Telemetrienachrichten)  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Devices/IotHubs  |  d2c.twin.read.failure  |  Failed twin reads from devices (Nicht erfolgreiche Zwillingslesevorgänge von Geräten)  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Devices/IotHubs  |  d2c.twin.read.size  |  Response size of twin reads from devices (Antwortgröße von Zwillingslesevorgängen von Geräten)  |  Byte  |  Average | 
| **Ja**  | **Ja** |  Microsoft.Devices/IotHubs  |  d2c.twin.read.success  |  Successful twin reads from devices (Erfolgreiche Zwillingslesevorgänge von Geräten)  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Devices/IotHubs  |  d2c.twin.update.failure  |  Failed twin updates from devices (Nicht erfolgreiche Zwillingsaktualisierungen von Geräten)  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Devices/IotHubs  |  d2c.twin.update.size  |  Size of twin updates from devices (Größe der Zwillingsaktualisierungen von Geräten)  |  Byte  |  Average | 
| **Ja**  | **Ja** |  Microsoft.Devices/IotHubs  |  d2c.twin.update.success  |  Successful twin updates from devices (Erfolgreiche Zwillingsaktualisierungen von Geräten)  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Devices/IotHubs  |  dailyMessageQuotaUsed  |  Gesamtzahl verwendeter Nachrichten  |  Anzahl  |  Average | 
| **Ja**  | **Ja** |  Microsoft.Devices/IotHubs  |  deviceDataUsage  |  Gerätedatennutzung gesamt  |  Byte  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Devices/IotHubs  |  deviceDataUsageV2  |  Gerätedatennutzung gesamt (Vorschau)  |  Byte  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Devices/IotHubs  |  devices.connectedDevices.allProtocol  |  Verbundene Geräte (veraltet)   |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Devices/IotHubs  |  devices.totalDevices  |  Geräte gesamt (veraltet)  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Devices/IotHubs  |  EventGridDeliveries  |  Event Grid-Übermittlungen (Vorschau)  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Devices/IotHubs  |  EventGridLatency  |  Event Grid-Wartezeit (Vorschau)  |  Millisekunden  |  Average | 
| **Ja**  | **Ja** |  Microsoft.Devices/IotHubs  |  jobs.cancelJob.failure  |  Failed job cancellations (Nicht erfolgreiche Auftragsabbrüche)  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Devices/IotHubs  |  jobs.cancelJob.success  |  Successful job cancellations (Erfolgreiche Auftragsabbrüche)  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Devices/IotHubs  |  jobs.completed  |  Abgeschlossene Aufträge  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Devices/IotHubs  |  jobs.createDirectMethodJob.failure  |  Failed creations of method invocation jobs (Nicht erfolgreiche Erstellungen von Methodenaufrufaufträgen)  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Devices/IotHubs  |  jobs.createDirectMethodJob.success  |  Successful creations of method invocation jobs (Erfolgreiche Erstellungen von Methodenaufrufaufträgen)  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Devices/IotHubs  |  jobs.createTwinUpdateJob.failure  |  Failed creations of twin update jobs (Nicht erfolgreiche Erstellungen von Zwillingsaktualisierungsaufträgen)  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Devices/IotHubs  |  jobs.createTwinUpdateJob.success  |  Successful creations of twin update jobs (Erfolgreiche Erstellungen von Zwillingsaktualisierungsaufträgen)  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Devices/IotHubs  |  jobs.failed  |  Fehlerhafte Aufträge  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Devices/IotHubs  |  jobs.listJobs.failure  |  Failed calls to list jobs (Nicht erfolgreiche Aufrufe von Auflistungsaufträgen)  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Devices/IotHubs  |  jobs.listJobs.success  |  Successful calls to list jobs (Erfolgreiche Aufrufe von Auflistungsaufträgen)  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Devices/IotHubs  |  jobs.queryJobs.failure  |  Failed job queries (Nicht erfolgreiche Auftragsabfragen)  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Devices/IotHubs  |  jobs.queryJobs.success  |  Successful job queries (Erfolgreiche Auftragsabfragen)  |  Anzahl  |  Gesamt | 
| Nein  | Nein |  Microsoft.Devices/IotHubs  |  totalDeviceCount  |  Geräte gesamt (Vorschau)  |  Anzahl  |  Average | 
| **Ja**  | **Ja** |  Microsoft.Devices/IotHubs  |  twinQueries.failure  |  Failed twin queries (Nicht erfolgreiche Zwillingsabfragen)  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Devices/IotHubs  |  twinQueries.resultSize  |  Twin queries result size (Ergebnisgröße von Zwillingsabfragen)  |  Byte  |  Average | 
| **Ja**  | **Ja** |  Microsoft.Devices/IotHubs  |  twinQueries.success  |  Successful twin queries (Erfolgreiche Zwillingsabfragen)  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Devices/provisioningServices  |  AttestationAttempts  |  Nachweisversuche  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Devices/provisioningServices  |  DeviceAssignments  |  Zugewiesene Geräte  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Devices/provisioningServices  |  RegistrationAttempts  |  Registrierungsversuche  |  Anzahl  |  Gesamt | 
| Nein  | Nein |  Microsoft.DocumentDB/databaseAccounts  |  AvailableStorage  |  Verfügbarer Speicher  |  Byte  |  Gesamt | 
| Nein  | Nein |  Microsoft.DocumentDB/databaseAccounts  |  CassandraConnectionClosures  |  Abschluss von Cassandra-Verbindungen  |  Anzahl  |  Gesamt | 
| Nein  | Nein |  Microsoft.DocumentDB/databaseAccounts  |  CassandraRequestCharges  |  Gebühren für Cassandra-Anforderungen  |  Anzahl  |  Gesamt | 
| Nein  | Nein |  Microsoft.DocumentDB/databaseAccounts  |  CassandraRequests  |  Cassandra-Anforderungen  |  Anzahl  |  Anzahl | 
| Nein  | Nein |  Microsoft.DocumentDB/databaseAccounts  |  DataUsage  |  Datennutzung  |  Byte  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.DocumentDB/databaseAccounts  |  DeleteVirtualNetwork  |  DeleteVirtualNetwork  |  Anzahl  |  Anzahl | 
| Nein  | Nein |  Microsoft.DocumentDB/databaseAccounts  |  DocumentCount  |  Dokumentanzahl  |  Anzahl  |  Gesamt | 
| Nein  | Nein |  Microsoft.DocumentDB/databaseAccounts  |  DocumentQuota  |  Dokumentenkontingent  |  Byte  |  Gesamt | 
| Nein  | Nein |  Microsoft.DocumentDB/databaseAccounts  |  IndexUsage  |  Indexnutzung  |  Byte  |  Gesamt | 
| Nein  | Nein |  Microsoft.DocumentDB/databaseAccounts  |  MetadataRequests  |  Anforderungen von Metadaten  |  Anzahl  |  Anzahl | 
| **Ja**  | **Ja** |  Microsoft.DocumentDB/databaseAccounts  |  MongoRequestCharge  |  Kosten der Mongo-Anforderung  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.DocumentDB/databaseAccounts  |  MongoRequests  |  Mongo-Anforderungen  |  Anzahl  |  Anzahl | 
| Nein  | Nein |  Microsoft.DocumentDB/databaseAccounts  |  MongoRequestsCount  |  Mongo-Anforderungsrate  |  Anzahl pro Sekunde  |  Average | 
| Nein  | Nein |  Microsoft.DocumentDB/databaseAccounts  |  MongoRequestsDelete  |  Mongo Delete Request Rate (Rate von Mongo-Löschanforderung)  |  Anzahl pro Sekunde  |  Average | 
| Nein  | Nein |  Microsoft.DocumentDB/databaseAccounts  |  MongoRequestsInsert  |  Mongo Insert Request Rate (Rate von Mongo-Einfügeanforderung)  |  Anzahl pro Sekunde  |  Average | 
| Nein  | Nein |  Microsoft.DocumentDB/databaseAccounts  |  MongoRequestsQuery  |  Mongo Query Request Rate (Rate von Mongo-Abfrageanforderung)  |  Anzahl pro Sekunde  |  Average | 
| Nein  | Nein |  Microsoft.DocumentDB/databaseAccounts  |  MongoRequestsUpdate  |  Mongo-Aktualisierungsanforderungsrate  |  Anzahl pro Sekunde  |  Average | 
| Nein  | Nein |  Microsoft.DocumentDB/databaseAccounts  |  ProvisionedThroughput  |  Bereitgestellter Durchsatz  |  Anzahl  |  Maximum | 
| **Ja**  | Nein |  Microsoft.DocumentDB/databaseAccounts  |  ReplicationLatency  |  P99-Replikationswartezeit  |  Millisekunden  |  Average | 
| Nein  | Nein |  Microsoft.DocumentDB/databaseAccounts  |  ServiceAvailability  |  Dienstverfügbarkeit  |  Percent  |  Average | 
| **Ja**  | **Ja** |  Microsoft.DocumentDB/databaseAccounts  |  TotalRequests  |  Anzahl von Anforderungen  |  Anzahl  |  Anzahl | 
| **Ja**  | **Ja** |  Microsoft.DocumentDB/databaseAccounts  |  TotalRequestUnits  |  Anforderungseinheiten gesamt  |  Anzahl  |  Gesamt | 
| Nein  | Nein |  Microsoft.EnterpriseKnowledgeGraph/services  |  FailureCount  |  Anzahl von Fehlern  |  Anzahl  |  Anzahl | 
| Nein  | Nein |  Microsoft.EnterpriseKnowledgeGraph/services  |  SuccessCount  |  Anzahl von Erfolgen  |  Anzahl  |  Anzahl | 
| Nein  | Nein |  Microsoft.EnterpriseKnowledgeGraph/services  |  SuccessLatency  |  Latenz bei Erfolg  |  Millisekunden  |  Average | 
| Nein  | Nein |  Microsoft.EnterpriseKnowledgeGraph/services  |  TransactionCount  |  Anzahl von Transaktionen  |  Anzahl  |  Anzahl | 
| **Ja**  | **Ja** |  Microsoft.EventGrid/domains  |  DeadLetteredCount  |  Unzustellbare Ereignisse  |  Anzahl  |  Gesamt | 
| Nein  | Nein |  Microsoft.EventGrid/domains  |  DeliveryAttemptFailCount  |  Ereignisse mit Übermittlungsfehler  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.EventGrid/domains  |  DeliverySuccessCount  |  Übermittelte Ereignisse  |  Anzahl  |  Gesamt | 
| Nein  | Nein |  Microsoft.EventGrid/domains  |  DestinationProcessingDurationInMs  |  Zielverarbeitungsdauer  |  Millisekunden  |  Average | 
| **Ja**  | **Ja** |  Microsoft.EventGrid/domains  |  DroppedEventCount  |  Gelöschte Ereignisse  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.EventGrid/domains  |  MatchedEventCount  |  Übereinstimmende Ereignisse  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.EventGrid/domains  |  PublishFailCount  |  Ereignisse mit Veröffentlichungsfehler  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.EventGrid/domains  |  PublishSuccessCount  |  Veröffentlichte Ereignisse  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.EventGrid/domains  |  PublishSuccessLatencyInMs  |  Latenz erfolgreicher Veröffentlichungen  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.EventGrid/eventSubscriptions  |  DeadLetteredCount  |  Unzustellbare Ereignisse  |  Anzahl  |  Gesamt | 
| Nein  | Nein |  Microsoft.EventGrid/eventSubscriptions  |  DeliveryAttemptFailCount  |  Ereignisse mit Übermittlungsfehler  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.EventGrid/eventSubscriptions  |  DeliverySuccessCount  |  Übermittelte Ereignisse  |  Anzahl  |  Gesamt | 
| Nein  | Nein |  Microsoft.EventGrid/eventSubscriptions  |  DestinationProcessingDurationInMs  |  Zielverarbeitungsdauer  |  Millisekunden  |  Average | 
| **Ja**  | **Ja** |  Microsoft.EventGrid/eventSubscriptions  |  DroppedEventCount  |  Gelöschte Ereignisse  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.EventGrid/eventSubscriptions  |  MatchedEventCount  |  Übereinstimmende Ereignisse  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.EventGrid/extensionTopics  |  PublishFailCount  |  Ereignisse mit Veröffentlichungsfehler  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.EventGrid/extensionTopics  |  PublishSuccessCount  |  Veröffentlichte Ereignisse  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.EventGrid/extensionTopics  |  PublishSuccessLatencyInMs  |  Latenz erfolgreicher Veröffentlichungen  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.EventGrid/extensionTopics  |  UnmatchedEventCount  |  Ereignisse ohne Übereinstimmung  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.EventGrid/topics  |  PublishFailCount  |  Ereignisse mit Veröffentlichungsfehler  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.EventGrid/topics  |  PublishSuccessCount  |  Veröffentlichte Ereignisse  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.EventGrid/topics  |  PublishSuccessLatencyInMs  |  Latenz erfolgreicher Veröffentlichungen  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.EventGrid/topics  |  UnmatchedEventCount  |  Ereignisse ohne Übereinstimmung  |  Anzahl  |  Gesamt | 
| Nein  | Nein |  Microsoft.EventHub/clusters  |  ActiveConnections  |  ActiveConnections  |  Anzahl  |  Average | 
| Nein  | Nein |  Microsoft.EventHub/clusters  |  AvailableMemory  |  Verfügbarer Arbeitsspeicher  |  Percent  |  Maximum | 
| Nein  | Nein |  Microsoft.EventHub/clusters  |  CaptureBacklog  |  Backlog erfassen.  |  Anzahl  |  Gesamt | 
| Nein  | Nein |  Microsoft.EventHub/clusters  |  CapturedBytes  |  Erfasste Bytes.  |  Byte  |  Gesamt | 
| Nein  | Nein |  Microsoft.EventHub/clusters  |  CapturedMessages  |  Erfasste Nachrichten.  |  Anzahl  |  Gesamt | 
| Nein  | Nein |  Microsoft.EventHub/clusters  |  ConnectionsClosed  |  Geschlossene Verbindungen.  |  Anzahl  |  Average | 
| Nein  | Nein |  Microsoft.EventHub/clusters  |  ConnectionsOpened  |  Geöffnete Verbindungen.  |  Anzahl  |  Average | 
| Nein  | Nein |  Microsoft.EventHub/clusters  |  CPU  |  CPU  |  Percent  |  Maximum | 
| **Ja**  | **Ja** |  Microsoft.EventHub/clusters  |  IncomingBytes  |  Eingehende Bytes.  |  Byte  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.EventHub/clusters  |  IncomingMessages  |  Eingehende Nachrichten  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.EventHub/clusters  |  IncomingRequests  |  Eingehende Anforderungen  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.EventHub/clusters  |  OutgoingBytes  |  Ausgehende Bytes.  |  Byte  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.EventHub/clusters  |  OutgoingMessages  |  Ausgehende Nachrichten  |  Anzahl  |  Gesamt | 
| Nein  | Nein |  Microsoft.EventHub/clusters  |  QuotaExceededErrors  |  Fehler aufgrund von Kontingentüberschreitung.  |  Anzahl  |  Gesamt | 
| Nein  | Nein |  Microsoft.EventHub/clusters  |  ServerErrors  |  Serverfehler.  |  Anzahl  |  Gesamt | 
| Nein  | Nein |  Microsoft.EventHub/clusters  |  SuccessfulRequests  |  Erfolgreiche Anforderungen  |  Anzahl  |  Gesamt | 
| Nein  | Nein |  Microsoft.EventHub/clusters  |  ThrottledRequests  |  Gedrosselte Anforderungen.  |  Anzahl  |  Gesamt | 
| Nein  | Nein |  Microsoft.EventHub/clusters  |  UserErrors  |  Benutzerfehler.  |  Anzahl  |  Gesamt | 
| Nein  | Nein |  Microsoft.EventHub/namespaces  |  ActiveConnections  |  ActiveConnections  |  Anzahl  |  Average | 
| Nein  | Nein |  Microsoft.EventHub/namespaces  |  CaptureBacklog  |  Backlog erfassen.  |  Anzahl  |  Gesamt | 
| Nein  | Nein |  Microsoft.EventHub/namespaces  |  CapturedBytes  |  Erfasste Bytes.  |  Byte  |  Gesamt | 
| Nein  | Nein |  Microsoft.EventHub/namespaces  |  CapturedMessages  |  Erfasste Nachrichten.  |  Anzahl  |  Gesamt | 
| Nein  | Nein |  Microsoft.EventHub/namespaces  |  ConnectionsClosed  |  Geschlossene Verbindungen.  |  Anzahl  |  Average | 
| Nein  | Nein |  Microsoft.EventHub/namespaces  |  ConnectionsOpened  |  Geöffnete Verbindungen.  |  Anzahl  |  Average | 
| **Ja**  | **Ja** |  Microsoft.EventHub/namespaces  |  EHABL  |  Archivierte Backlognachrichten (veraltet)  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.EventHub/namespaces  |  EHAMBS  |  Durchsatz von Archivnachrichten (veraltet)  |  Byte  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.EventHub/namespaces  |  EHAMSGS  |  Archivnachrichten (veraltet)  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.EventHub/namespaces  |  EHINBYTES  |  Eingehende Bytes (veraltet)  |  Byte  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.EventHub/namespaces  |  EHINMBS  |  Eingehende Bytes (veraltet)  |  Byte  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.EventHub/namespaces  |  EHINMSGS  |  Eingehende Nachrichten (veraltet)  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.EventHub/namespaces  |  EHOUTBYTES  |  Ausgehende Bytes (veraltet)  |  Byte  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.EventHub/namespaces  |  EHOUTMBS  |  Ausgehende Bytes (veraltet)  |  Byte  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.EventHub/namespaces  |  EHOUTMSGS  |  Ausgehende Nachrichten (veraltet)  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.EventHub/namespaces  |  FAILREQ  |  Fehlerhafte Anforderungen (veraltet)  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.EventHub/namespaces  |  IncomingBytes  |  Eingehende Bytes.  |  Byte  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.EventHub/namespaces  |  IncomingMessages  |  Eingehende Nachrichten  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.EventHub/namespaces  |  IncomingRequests  |  Eingehende Anforderungen  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.EventHub/namespaces  |  INMSGS  |  Eingehende Nachrichten (veraltet)  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.EventHub/namespaces  |  INREQS  |  Eingehende Anforderungen (veraltet)  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.EventHub/namespaces  |  INTERR  |  Interne Serverfehler (veraltet)  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.EventHub/namespaces  |  MISCERR  |  Andere Fehler (veraltet)  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.EventHub/namespaces  |  OutgoingBytes  |  Ausgehende Bytes.  |  Byte  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.EventHub/namespaces  |  OutgoingMessages  |  Ausgehende Nachrichten  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.EventHub/namespaces  |  OUTMSGS  |  Ausgehende Nachrichten (veraltet)  |  Anzahl  |  Gesamt | 
| Nein  | Nein |  Microsoft.EventHub/namespaces  |  QuotaExceededErrors  |  Fehler aufgrund von Kontingentüberschreitung.  |  Anzahl  |  Gesamt | 
| Nein  | Nein |  Microsoft.EventHub/namespaces  |  ServerErrors  |  Serverfehler.  |  Anzahl  |  Gesamt | 
| Nein  | Nein |  Microsoft.EventHub/namespaces  |  Size  |  Size  |  Byte  |  Average | 
| Nein  | Nein |  Microsoft.EventHub/namespaces  |  SuccessfulRequests  |  Erfolgreiche Anforderungen  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.EventHub/namespaces  |  SUCCREQ  |  Erfolgreiche Anforderungen (veraltet)  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.EventHub/namespaces  |  SVRBSY  |  Fehler durch ausgelasteten Server (veraltet)  |  Anzahl  |  Gesamt | 
| Nein  | Nein |  Microsoft.EventHub/namespaces  |  ThrottledRequests  |  Gedrosselte Anforderungen.  |  Anzahl  |  Gesamt | 
| Nein  | Nein |  Microsoft.EventHub/namespaces  |  UserErrors  |  Benutzerfehler.  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.HDInsight/clusters  |  CategorizedGatewayRequests  |  Kategorisierte Gatewayanforderungen  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.HDInsight/clusters  |  GatewayRequests  |  Gatewayanforderungen  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.HDInsight/clusters  |  NumActiveWorkers  |  Anzahl aktiver Worker  |  Anzahl  |  Maximum | 
| **Ja**  | Nein |  Microsoft.HDInsight/clusters  |  ScalingRequests  |  Skalierungsanforderungen  |  Anzahl  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Insights/AutoscaleSettings  |  MetricThreshold  |  Schwellenwert der Metrik  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.Insights/AutoscaleSettings  |  ObservedCapacity  |  Beobachtete Kapazität  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.Insights/AutoscaleSettings  |  ObservedMetricValue  |  Beobachteter Metrikwert  |  Anzahl  |  Average | 
| **Ja**  | **Ja** |  Microsoft.Insights/AutoscaleSettings  |  ScaleActionsInitiated  |  Initiierte Skalierungsaktionen  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Insights/Components  |  availabilityResults/availabilityPercentage  |  Verfügbarkeit  |  Percent  |  Average | 
| Nein  | Nein |  Microsoft.Insights/Components  |  availabilityResults/count  |  Verfügbarkeitstests  |  Anzahl  |  Anzahl | 
| **Ja**  | Nein |  Microsoft.Insights/Components  |  availabilityResults/duration  |  Dauer von Verfügbarkeitstests  |  Millisekunden  |  Average | 
| **Ja**  | Nein |  Microsoft.Insights/Components  |  browserTimings/networkDuration  |  Netzwerkverbindungszeit zum Laden der Seite  |  Millisekunden  |  Average | 
| **Ja**  | Nein |  Microsoft.Insights/Components  |  browserTimings/processingDuration  |  Clientverarbeitungszeit  |  Millisekunden  |  Average | 
| **Ja**  | Nein |  Microsoft.Insights/Components  |  browserTimings/receiveDuration  |  Empfängt Antwortzeit  |  Millisekunden  |  Average | 
| **Ja**  | Nein |  Microsoft.Insights/Components  |  browserTimings/sendDuration  |  Anforderungszeit senden  |  Millisekunden  |  Average | 
| **Ja**  | Nein |  Microsoft.Insights/Components  |  browserTimings/totalDuration  |  Browser-Seitenladezeit  |  Millisekunden  |  Average | 
| Nein  | Nein |  Microsoft.Insights/Components  |  dependencies/count  |  Abhängigkeitsaufrufe  |  Anzahl  |  Anzahl | 
| **Ja**  | Nein |  Microsoft.Insights/Components  |  dependencies/duration  |  Dauer der Abhängigkeit  |  Millisekunden  |  Average | 
| Nein  | Nein |  Microsoft.Insights/Components  |  dependencies/failed  |  Fehler bei Abhängigkeitsaufrufen  |  Anzahl  |  Anzahl | 
| Nein  | Nein |  Microsoft.Insights/Components  |  exceptions/browser  |  Browserausnahmen  |  Anzahl  |  Anzahl | 
| **Ja**  | **Ja** |  Microsoft.Insights/Components  |  exceptions/count  |  Ausnahmen  |  Anzahl  |  Anzahl | 
| Nein  | Nein |  Microsoft.Insights/Components  |  exceptions/server  |  Serverausnahmen  |  Anzahl  |  Anzahl | 
| **Ja**  | **Ja** |  Microsoft.Insights/Components  |  pageViews/count  |  Seitenaufrufe  |  Anzahl  |  Anzahl | 
| **Ja**  | Nein |  Microsoft.Insights/Components  |  pageViews/duration  |  Ladezeit der Seitenansicht  |  Millisekunden  |  Average | 
| **Ja**  | **Ja** |  Microsoft.Insights/Components  |  performanceCounters/exceptionsPerSecond  |  Ausnahmerate  |  Anzahl pro Sekunde  |  Average | 
| **Ja**  | Nein |  Microsoft.Insights/Components  |  performanceCounters/memoryAvailableBytes  |  Verfügbarer Arbeitsspeicher  |  Byte  |  Average | 
| **Ja**  | Nein |  Microsoft.Insights/Components  |  performanceCounters/processCpuPercentage  |  Prozess-CPU  |  Percent  |  Average | 
| **Ja**  | Nein |  Microsoft.Insights/Components  |  performanceCounters/processIOBytesPerSecond  |  E/A-Rate für Prozess  |  Bytes pro Sekunde  |  Average | 
| **Ja**  | Nein |  Microsoft.Insights/Components  |  performanceCounters/processorCpuPercentage  |  Prozessorzeit  |  Percent  |  Average | 
| **Ja**  | Nein |  Microsoft.Insights/Components  |  performanceCounters/processPrivateBytes  |  Private Bytes für Prozess  |  Byte  |  Average | 
| **Ja**  | Nein |  Microsoft.Insights/Components  |  performanceCounters/requestExecutionTime  |  Ausführungszeit der HTTP-Anforderung  |  Millisekunden  |  Average | 
| **Ja**  | **Ja** |  Microsoft.Insights/Components  |  performanceCounters/requestsInQueue  |  HTTP-Anforderungen in der Anwendungswarteschlange  |  Anzahl  |  Average | 
| **Ja**  | **Ja** |  Microsoft.Insights/Components  |  performanceCounters/requestsPerSecond  |  HTTP-Anforderungsrate  |  Anzahl pro Sekunde  |  Average | 
| Nein  | Nein |  Microsoft.Insights/Components  |  requests/count  |  Serveranforderungen  |  Anzahl  |  Anzahl | 
| **Ja**  | Nein |  Microsoft.Insights/Components  |  requests/duration  |  Serverantwortzeit  |  Millisekunden  |  Average | 
| Nein  | Nein |  Microsoft.Insights/Components  |  requests/failed  |  Anforderungsfehler  |  Anzahl  |  Anzahl | 
| Nein  | Nein |  Microsoft.Insights/Components  |  requests/rate  |  Serveranforderungsrate  |  Anzahl pro Sekunde  |  Average | 
| **Ja**  | **Ja** |  Microsoft.Insights/Components  |  traces/count  |  Traces  |  Anzahl  |  Anzahl | 
| **Ja**  | Nein |  Microsoft.KeyVault/vaults  |  ServiceApiHit  |  Dienst-API-Treffer, gesamt  |  Anzahl  |  Anzahl | 
| **Ja**  | Nein |  Microsoft.KeyVault/vaults  |  ServiceApiLatency  |  Gesamtwartezeit für Dienst-API  |  Millisekunden  |  Average | 
| **Ja**  | Nein |  Microsoft.KeyVault/vaults  |  ServiceApiResult  |  Dienst-API-Ergebnisse, gesamt  |  Anzahl  |  Anzahl | 
| **Ja**  | **Ja** |  Microsoft.Kusto/Clusters  |  CacheUtilization  |  Cacheauslastung  |  Percent  |  Average | 
| **Ja**  | **Ja** |  Microsoft.Kusto/Clusters  |  ContinuousExportMaxLatenessMinutes  |  Maximale Wartezeit in Minuten für fortlaufenden Export  |  Anzahl  |  Maximum | 
| **Ja**  | **Ja** |  Microsoft.Kusto/Clusters  |  ContinuousExportNumOfRecordsExported  |  Fortlaufender Export – Anzahl der exportierten Datensätze  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Kusto/Clusters  |  ContinuousExportPendingCount  |  ContinuousExportPendingCount  |  Anzahl  |  Maximum | 
| **Ja**  | **Ja** |  Microsoft.Kusto/Clusters  |  ContinuousExportResult  |  Ergebnis des fortlaufenden Exports  |  Anzahl  |  Anzahl | 
| **Ja**  | **Ja** |  Microsoft.Kusto/Clusters  |  CPU  |  CPU  |  Percent  |  Average | 
| **Ja**  | **Ja** |  Microsoft.Kusto/Clusters  |  EventsProcessedForEventHubs  |  Verarbeitete Ereignisse (für Event/IoT Hub)  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Kusto/Clusters  |  ExportUtilization  |  Exportauslastung  |  Percent  |  Maximum | 
| **Ja**  | **Ja** |  Microsoft.Kusto/Clusters  |  IngestionLatencyInSeconds  |  Erfassungslatenz (in Sekunden)  |  Sekunden  |  Average | 
| **Ja**  | **Ja** |  Microsoft.Kusto/Clusters  |  IngestionResult  |  Ergebnis der Datenerfassung  |  Anzahl  |  Anzahl | 
| **Ja**  | **Ja** |  Microsoft.Kusto/Clusters  |  IngestionUtilization  |  Datenerfassungsauslastung  |  Percent  |  Average | 
| **Ja**  | **Ja** |  Microsoft.Kusto/Clusters  |  IngestionVolumeInMB  |  Datenerfassungsvolumen (in MB)  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Kusto/Clusters  |  KeepAlive  |  Keep-Alive  |  Anzahl  |  Average | 
| **Ja**  | **Ja** |  Microsoft.Kusto/Clusters  |  QueryDuration  |  Abfragedauer  |  Millisekunden  |  Average | 
| **Ja**  | **Ja** |  Microsoft.Kusto/Clusters  |  SteamingIngestRequestRate  |  Anforderungsrate der Streamingerfassung  |  Anzahl  |  RateRequestsPerSecond | 
| **Ja**  | **Ja** |  Microsoft.Kusto/Clusters  |  StreamingIngestDataRate  |  Datenrate der Streamingerfassung  |  Anzahl  |  Average | 
| **Ja**  | **Ja** |  Microsoft.Kusto/Clusters  |  StreamingIngestDuration  |  Dauer der Streamingerfassung  |  Millisekunden  |  Average | 
| **Ja**  | **Ja** |  Microsoft.Kusto/Clusters  |  StreamingIngestResults  |  Ergebnis der Streamingerfassung  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.Logic/integrationServiceEnvironments  |  ActionLatency  |  Aktionslatenz   |  Sekunden  |  Average | 
| **Ja**  | **Ja** |  Microsoft.Logic/integrationServiceEnvironments  |  ActionsCompleted  |  Abgeschlossene Aktionen   |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Logic/integrationServiceEnvironments  |  ActionsFailed  |  Aktionsfehler   |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Logic/integrationServiceEnvironments  |  ActionsSkipped  |  Übersprungene Aktionen   |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Logic/integrationServiceEnvironments  |  ActionsStarted  |  Gestartete Aktionen   |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Logic/integrationServiceEnvironments  |  ActionsSucceeded  |  Erfolgreiche Aktionen   |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Logic/integrationServiceEnvironments  |  ActionSuccessLatency  |  Latenz erfolgreicher Aktionen   |  Sekunden  |  Average | 
| **Ja**  | **Ja** |  Microsoft.Logic/integrationServiceEnvironments  |  ActionThrottledEvents  |  Ereignisse zu gedrosselten Aktionen  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Logic/integrationServiceEnvironments  |  IntegrationServiceEnvironmentConnectorMemoryUsage  |  Connector-Arbeitsspeichernutzung für Integrationsdienstumgebung  |  Percent  |  Average | 
| **Ja**  | Nein |  Microsoft.Logic/integrationServiceEnvironments  |  IntegrationServiceEnvironmentConnectorProcessorUsage  |  Connector-Prozessorauslastung für Integrationsdienstumgebung  |  Percent  |  Average | 
| **Ja**  | Nein |  Microsoft.Logic/integrationServiceEnvironments  |  IntegrationServiceEnvironmentWorkflowMemoryUsage  |  Workflow-Arbeitsspeichernutzung für Integrationsdienstumgebung  |  Percent  |  Average | 
| **Ja**  | Nein |  Microsoft.Logic/integrationServiceEnvironments  |  IntegrationServiceEnvironmentWorkflowProcessorUsage  |  Workflow-Prozessorauslastung für Integrationsdienstumgebung  |  Percent  |  Average | 
| **Ja**  | **Ja** |  Microsoft.Logic/integrationServiceEnvironments  |  RunFailurePercentage  |  Prozentsatz der Ausführungsfehler  |  Percent  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Logic/integrationServiceEnvironments  |  RunLatency  |  Ausführungslatenz  |  Sekunden  |  Average | 
| **Ja**  | **Ja** |  Microsoft.Logic/integrationServiceEnvironments  |  RunsCancelled  |  Abgebrochene Ausführungen  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Logic/integrationServiceEnvironments  |  RunsCompleted  |  Abgeschlossene Ausführungen  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Logic/integrationServiceEnvironments  |  RunsFailed  |  Ausführungsfehler  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Logic/integrationServiceEnvironments  |  RunsStarted  |  Gestartete Ausführungen  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Logic/integrationServiceEnvironments  |  RunsSucceeded  |  Erfolgreiche Ausführungen  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Logic/integrationServiceEnvironments  |  RunStartThrottledEvents  |  Ereignisse zu gedrosselten Ausführungsstarts  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Logic/integrationServiceEnvironments  |  RunSuccessLatency  |  Latenz erfolgreicher Ausführungen  |  Sekunden  |  Average | 
| **Ja**  | **Ja** |  Microsoft.Logic/integrationServiceEnvironments  |  RunThrottledEvents  |  Ereignisse zu gedrosselten Ausführungen  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Logic/integrationServiceEnvironments  |  TriggerFireLatency  |  Latenz beim Auslösen von Triggern   |  Sekunden  |  Average | 
| **Ja**  | Nein |  Microsoft.Logic/integrationServiceEnvironments  |  TriggerLatency  |  Triggerlatenz   |  Sekunden  |  Average | 
| **Ja**  | **Ja** |  Microsoft.Logic/integrationServiceEnvironments  |  TriggersCompleted  |  Abgeschlossene Trigger   |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Logic/integrationServiceEnvironments  |  TriggersFailed  |  Triggerfehler   |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Logic/integrationServiceEnvironments  |  TriggersFired  |  Ausgelöste Trigger   |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Logic/integrationServiceEnvironments  |  TriggersSkipped  |  Übersprungene Trigger  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Logic/integrationServiceEnvironments  |  TriggersStarted  |  Gestartete Trigger   |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Logic/integrationServiceEnvironments  |  TriggersSucceeded  |  Erfolgreiche Trigger   |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Logic/integrationServiceEnvironments  |  TriggerSuccessLatency  |  Latenz erfolgreicher Trigger   |  Sekunden  |  Average | 
| **Ja**  | **Ja** |  Microsoft.Logic/integrationServiceEnvironments  |  TriggerThrottledEvents  |  Ereignisse zu gedrosselten Triggern  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Logic/workflows  |  ActionLatency  |  Aktionslatenz   |  Sekunden  |  Average | 
| **Ja**  | **Ja** |  Microsoft.Logic/workflows  |  ActionsCompleted  |  Abgeschlossene Aktionen   |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Logic/workflows  |  ActionsFailed  |  Aktionsfehler   |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Logic/workflows  |  ActionsSkipped  |  Übersprungene Aktionen   |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Logic/workflows  |  ActionsStarted  |  Gestartete Aktionen   |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Logic/workflows  |  ActionsSucceeded  |  Erfolgreiche Aktionen   |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Logic/workflows  |  ActionSuccessLatency  |  Latenz erfolgreicher Aktionen   |  Sekunden  |  Average | 
| **Ja**  | **Ja** |  Microsoft.Logic/workflows  |  ActionThrottledEvents  |  Ereignisse zu gedrosselten Aktionen  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Logic/workflows  |  BillableActionExecutions  |  Ausführungen abrechenbarer Aktionen  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Logic/workflows  |  BillableTriggerExecutions  |  Ausführungen abrechenbarer Trigger  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Logic/workflows  |  BillingUsageNativeOperation  |  Nutzungsabrechnung für Ausführungen nativer Vorgänge  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Logic/workflows  |  BillingUsageNativeOperation  |  Nutzungsabrechnung für Ausführungen nativer Vorgänge  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Logic/workflows  |  BillingUsageStandardConnector  |  Nutzungsabrechnung für Ausführungen standardmäßiger Connectors  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Logic/workflows  |  BillingUsageStandardConnector  |  Nutzungsabrechnung für Ausführungen standardmäßiger Connectors  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Logic/workflows  |  BillingUsageStorageConsumption  |  Nutzungsabrechnung für Ausführungen mit Speicherverbrauch  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Logic/workflows  |  BillingUsageStorageConsumption  |  Nutzungsabrechnung für Ausführungen mit Speicherverbrauch  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Logic/workflows  |  RunFailurePercentage  |  Prozentsatz der Ausführungsfehler  |  Percent  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Logic/workflows  |  RunLatency  |  Ausführungslatenz  |  Sekunden  |  Average | 
| **Ja**  | **Ja** |  Microsoft.Logic/workflows  |  RunsCancelled  |  Abgebrochene Ausführungen  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Logic/workflows  |  RunsCompleted  |  Abgeschlossene Ausführungen  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Logic/workflows  |  RunsFailed  |  Ausführungsfehler  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Logic/workflows  |  RunsStarted  |  Gestartete Ausführungen  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Logic/workflows  |  RunsSucceeded  |  Erfolgreiche Ausführungen  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Logic/workflows  |  RunStartThrottledEvents  |  Ereignisse zu gedrosselten Ausführungsstarts  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Logic/workflows  |  RunSuccessLatency  |  Latenz erfolgreicher Ausführungen  |  Sekunden  |  Average | 
| **Ja**  | **Ja** |  Microsoft.Logic/workflows  |  RunThrottledEvents  |  Ereignisse zu gedrosselten Ausführungen  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Logic/workflows  |  TotalBillableExecutions  |  Gesamtzahl der abrechenbaren Ausführungen  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Logic/workflows  |  TriggerFireLatency  |  Latenz beim Auslösen von Triggern   |  Sekunden  |  Average | 
| **Ja**  | Nein |  Microsoft.Logic/workflows  |  TriggerLatency  |  Triggerlatenz   |  Sekunden  |  Average | 
| **Ja**  | **Ja** |  Microsoft.Logic/workflows  |  TriggersCompleted  |  Abgeschlossene Trigger   |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Logic/workflows  |  TriggersFailed  |  Triggerfehler   |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Logic/workflows  |  TriggersFired  |  Ausgelöste Trigger   |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Logic/workflows  |  TriggersSkipped  |  Übersprungene Trigger  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Logic/workflows  |  TriggersStarted  |  Gestartete Trigger   |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Logic/workflows  |  TriggersSucceeded  |  Erfolgreiche Trigger   |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Logic/workflows  |  TriggerSuccessLatency  |  Latenz erfolgreicher Trigger   |  Sekunden  |  Average | 
| **Ja**  | **Ja** |  Microsoft.Logic/workflows  |  TriggerThrottledEvents  |  Ereignisse zu gedrosselten Triggern  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.MachineLearningServices/workspaces  |  Aktive Kerne  |  Aktive Kerne  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.MachineLearningServices/workspaces  |  Aktive Knoten  |  Aktive Knoten  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.MachineLearningServices/workspaces  |  Abgeschlossene Ausführungen  |  Abgeschlossene Ausführungen  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.MachineLearningServices/workspaces  |  Fehlerhafte Ausführungen  |  Fehlerhafte Ausführungen  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.MachineLearningServices/workspaces  |  Kerne im Leerlauf  |  Kerne im Leerlauf  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.MachineLearningServices/workspaces  |  Knoten im Leerlauf  |  Knoten im Leerlauf  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.MachineLearningServices/workspaces  |  Verbleibende Kerne  |  Ausscheidende Kerne  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.MachineLearningServices/workspaces  |  Verbleibende Knoten  |  Ausscheidende Knoten  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.MachineLearningServices/workspaces  |  Model Deploy Failed (Fehler bei der Modellimplementierung)  |  Model Deploy Failed (Fehler bei der Modellimplementierung)  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.MachineLearningServices/workspaces  |  Gestartete Modellimplementierungen  |  Gestartete Modellimplementierungen  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.MachineLearningServices/workspaces  |  Erfolgreiche Modellimplementierungen  |  Erfolgreiche Modellimplementierungen  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.MachineLearningServices/workspaces  |  Fehler bei der Modellregistrierung  |  Fehler bei der Modellregistrierung  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.MachineLearningServices/workspaces  |  Erfolgreiche Modellregistrierungen  |  Erfolgreiche Modellregistrierungen  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.MachineLearningServices/workspaces  |  Vorzeitig entfernte Kerne  |  Vorzeitig entfernte Kerne  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.MachineLearningServices/workspaces  |  Vorzeitig entfernte Knoten  |  Vorzeitig entfernte Knoten  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.MachineLearningServices/workspaces  |  Quota Utilization Percentage (Prozentsatz der Kontingentnutzung)  |  Quota Utilization Percentage (Prozentsatz der Kontingentnutzung)  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.MachineLearningServices/workspaces  |  Gestartete Ausführungen  |  Gestartete Ausführungen  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.MachineLearningServices/workspaces  |  Kerne insgesamt  |  Kerne insgesamt  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.MachineLearningServices/workspaces  |  Knoten insgesamt  |  Knoten insgesamt  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.MachineLearningServices/workspaces  |  Nicht verwendbare Kerne  |  Nicht verwendbare Kerne  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.MachineLearningServices/workspaces  |  Unusable Nodes (Nicht verwendbare Knoten)  |  Unusable Nodes (Nicht verwendbare Knoten)  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.Maps/accounts  |  Verfügbarkeit  |  Verfügbarkeit  |  Percent  |  Average | 
| Nein  | Nein |  Microsoft.Maps/accounts  |  Verwendung  |  Verwendung  |  Anzahl  |  Anzahl | 
| **Ja**  | Nein |  Microsoft.Media/mediaservices  |  AssetCount  |  Anzahl der Medienobjekte  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.Media/mediaservices  |  AssetQuota  |  Kontingent der Medienobjekte  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.Media/mediaservices  |  AssetQuotaUsedPercentage  |  Prozentsatz des verwendeten Kontingents der Medienobjekte  |  Percent  |  Average | 
| **Ja**  | Nein |  Microsoft.Media/mediaservices  |  ContentKeyPolicyCount  |  Anzahl der Richtlinien für Inhaltsschlüssel  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.Media/mediaservices  |  ContentKeyPolicyQuota  |  Kontingent der Richtlinien für Inhaltsschlüssel  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.Media/mediaservices  |  ContentKeyPolicyQuotaUsedPercentage  |  Prozentsatz des Kontingents der Richtlinien für Inhaltsschlüssel  |  Percent  |  Average | 
| **Ja**  | Nein |  Microsoft.Media/mediaservices  |  StreamingPolicyCount  |  Anzahl der Streamingrichtlinien  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.Media/mediaservices  |  StreamingPolicyQuota  |  Kontingent der Streamingrichtlinien  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.Media/mediaservices  |  StreamingPolicyQuotaUsedPercentage  |  Prozentsatz des verwendeten Kontingents der Streamingrichtlinien  |  Percent  |  Average | 
| **Ja**  | Nein |  Microsoft.Media/mediaservices/streamingEndpoints  |  Ausgehende Daten  |  Ausgehende Daten  |  Byte  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Media/mediaservices/streamingEndpoints  |  Requests  |  Requests  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Media/mediaservices/streamingEndpoints  |  SuccessE2ELatency  |  End-to-End-Wartezeit bei Erfolg  |  Millisekunden  |  Average | 
| **Ja**  | Nein |  Microsoft.Microservices4Spring/appClusters  |  GCPauseTotalCount  |  Anzahl der GC-Pausen  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Microservices4Spring/appClusters  |  GCPauseTotalTime  |  Gesamtzeit der GC-Pausen  |  Millisekunden  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Microservices4Spring/appClusters  |  MaxOldGenMemoryPoolBytes  |  Maximal verfügbare Datengröße der alten Generation  |  Byte  |  Average | 
| **Ja**  | Nein |  Microsoft.Microservices4Spring/appClusters  |  OldGenMemoryPoolBytes  |  Datengröße der alten Generation  |  Byte  |  Average | 
| **Ja**  | Nein |  Microsoft.Microservices4Spring/appClusters  |  OldGenPromotedBytes  |  Hochstufung auf die Datengröße der alten Generation  |  Byte  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Microservices4Spring/appClusters  |  ServiceCpuUsagePercentage  |  Prozentualer Anteil der Dienst-CPU-Auslastung  |  Percent  |  Average | 
| **Ja**  | Nein |  Microsoft.Microservices4Spring/appClusters  |  ServiceMemoryCommitted  |  Zugewiesener Dienstarbeitsspeicher  |  Byte  |  Average | 
| **Ja**  | Nein |  Microsoft.Microservices4Spring/appClusters  |  ServiceMemoryMax  |  Maximaler Dienstarbeitsspeicher  |  Byte  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Microservices4Spring/appClusters  |  ServiceMemoryUsed  |  Verwendeter Dienstarbeitsspeicher  |  Byte  |  Average | 
| **Ja**  | Nein |  Microsoft.Microservices4Spring/appClusters  |  SystemCpuUsagePercentage  |  Prozentualer Anteil der System-CPU-Auslastung  |  Percent  |  Average | 
| **Ja**  | Nein |  Microsoft.Microservices4Spring/appClusters  |  TomcatErrorCount  |  Globale Tomcat-Fehler  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Microservices4Spring/appClusters  |  TomcatReceivedBytes  |  Insgesamt empfangene Bytes für Tomcat  |  Byte  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Microservices4Spring/appClusters  |  TomcatRequestMaxTime  |  Maximale Zeit für Tomcat-Anforderung  |  Millisekunden  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Microservices4Spring/appClusters  |  TomcatRequestTotalCount  |  Gesamtanzahl von Tomcat-Anforderungen  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Microservices4Spring/appClusters  |  TomcatRequestTotalTime  |  Gesamtzeit für Tomcat-Anforderungen  |  Millisekunden  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Microservices4Spring/appClusters  |  TomcatResponseAvgTime  |  Durchschnittliche Zeit für Tomcat-Anforderungen  |  Millisekunden  |  Average | 
| **Ja**  | Nein |  Microsoft.Microservices4Spring/appClusters  |  TomcatSentBytes  |  Insgesamt gesendete Bytes für Tomcat  |  Byte  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Microservices4Spring/appClusters  |  TomcatSessionActiveCurrentCount  |  Anzahl aktiver Tomcat-Sitzungen  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Microservices4Spring/appClusters  |  TomcatSessionActiveMaxCount  |  Maximale Anzahl aktiver Tomcat-Sitzungen  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Microservices4Spring/appClusters  |  TomcatSessionAliveMaxTime  |  Maximale aktive Zeit für Tomcat-Sitzungen  |  Millisekunden  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Microservices4Spring/appClusters  |  TomcatSessionCreatedCount  |  Anzahl erstellter Tomcat-Sitzungen  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Microservices4Spring/appClusters  |  TomcatSessionExpiredCount  |  Anzahl abgelaufener Tomcat-Sitzungen  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Microservices4Spring/appClusters  |  TomcatSessionRejectedCount  |  Anzahl abgewiesener Tomcat-Sitzungen  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Microservices4Spring/appClusters  |  YoungGenPromotedBytes  |  Hochstufung auf die Datengröße der neuen Generation  |  Byte  |  Maximum | 
| **Ja**  | Nein |  Microsoft.NetApp/netAppAccounts/capacityPools  |  VolumePoolAllocatedUsed  |  Verwendeter zugeordneter Volumepool  |  Byte  |  Average | 
| **Ja**  | Nein |  Microsoft.NetApp/netAppAccounts/capacityPools  |  VolumePoolTotalLogicalSize  |  Gesamte logische Größe des Volumepools  |  Byte  |  Average | 
| **Ja**  | Nein |  Microsoft.NetApp/netAppAccounts/capacityPools/volumes  |  AverageReadLatency  |  Durchschnittliche Wartezeit beim Lesevorgang  |  Millisekunden  |  Average | 
| **Ja**  | Nein |  Microsoft.NetApp/netAppAccounts/capacityPools/volumes  |  AverageWriteLatency  |  Durchschnittliche Wartezeit beim Schreibvorgang  |  Millisekunden  |  Average | 
| **Ja**  | Nein |  Microsoft.NetApp/netAppAccounts/capacityPools/volumes  |  ReadIops  |  Lese-IOPS  |  Anzahl pro Sekunde  |  Average | 
| **Ja**  | Nein |  Microsoft.NetApp/netAppAccounts/capacityPools/volumes  |  VolumeLogicalSize  |  Logische Größe des Volume  |  Byte  |  Average | 
| **Ja**  | Nein |  Microsoft.NetApp/netAppAccounts/capacityPools/volumes  |  VolumeSnapshotSize  |  Größe der Volumenmomentaufnahme  |  Byte  |  Average | 
| **Ja**  | Nein |  Microsoft.NetApp/netAppAccounts/capacityPools/volumes  |  WriteIops  |  Schreib-IOPS  |  Anzahl pro Sekunde  |  Average | 
| Nein  | Nein |  Microsoft.Network/applicationGateways  |  ApplicationGatewayTotalTime  |  Application Gateway-Gesamtzeit  |  Millisekunden  |  Average | 
| Nein  | Nein |  Microsoft.Network/applicationGateways  |  AvgRequestCountPerHealthyHost  |  Anforderungen pro Minute pro fehlerfreiem Host  |  Anzahl  |  Average | 
| Nein  | Nein |  Microsoft.Network/applicationGateways  |  BackendConnectTime  |  Back-End-Verbindungszeit  |  Millisekunden  |  Average | 
| Nein  | Nein |  Microsoft.Network/applicationGateways  |  BackendFirstByteResponseTime  |  Antwortzeit für erstes Byte des Back-Ends  |  Millisekunden  |  Average | 
| Nein  | Nein |  Microsoft.Network/applicationGateways  |  BackendLastByteResponseTime  |  Antwortzeit für letztes Byte des Back-Ends  |  Millisekunden  |  Average | 
| **Ja**  | Nein |  Microsoft.Network/applicationGateways  |  BackendResponseStatus  |  Back-End-Antwortstatus  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Network/applicationGateways  |  BlockedCount  |  Regelverteilung für blockierte Web Application Firewall-Anforderungen  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Network/applicationGateways  |  BlockedReqCount  |  Anzahl blockierter Web Application Firewall-Anforderungen  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Network/applicationGateways  |  BytesReceived  |  Empfangene Bytes  |  Byte  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Network/applicationGateways  |  BytesSent  |  Gesendete Bytes  |  Byte  |  Gesamt | 
| Nein  | Nein |  Microsoft.Network/applicationGateways  |  CapacityUnits  |  Aktuelle Kapazitätseinheiten  |  Anzahl  |  Average | 
| Nein  | Nein |  Microsoft.Network/applicationGateways  |  ClientRtt  |  Client-RTT  |  Millisekunden  |  Average | 
| Nein  | Nein |  Microsoft.Network/applicationGateways  |  ComputeUnits  |  Aktuelle Compute-Einheiten  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.Network/applicationGateways  |  CurrentConnections  |  Aktuelle Verbindungen  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Network/applicationGateways  |  FailedRequests  |  Anforderungsfehler  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Network/applicationGateways  |  HealthyHostCount  |  Anzahl von fehlerfreien Hosts  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.Network/applicationGateways  |  MatchedCount  |  Gesamtregelverteilung in Web Application Firewall  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Network/applicationGateways  |  ResponseStatus  |  Antwortstatus  |  Anzahl  |  Gesamt | 
| Nein  | Nein |  Microsoft.Network/applicationGateways  |  Throughput  |  Throughput  |  Bytes pro Sekunde  |  Average | 
| **Ja**  | Nein |  Microsoft.Network/applicationGateways  |  TlsProtocol  |  Client-TLS-Protokoll  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Network/applicationGateways  |  TotalRequests  |  Anzahl von Anforderungen  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Network/applicationGateways  |  UnhealthyHostCount  |  Anzahl von fehlerhaften Hosts  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.Network/azurefirewalls  |  ApplicationRuleHit  |  Trefferanzahl für Anwendungsregeln  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Network/azurefirewalls  |  DataProcessed  |  Verarbeitete Datenmenge  |  Byte  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Network/azurefirewalls  |  FirewallHealth  |  Integritätszustand der Firewall  |  Percent  |  Average | 
| **Ja**  | Nein |  Microsoft.Network/azurefirewalls  |  NetworkRuleHit  |  Trefferanzahl für Netzwerkregeln  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Network/azurefirewalls  |  SNATPortUtilization  |  SNAT-Portnutzung  |  Percent  |  Average | 
| **Ja**  | Nein |  Microsoft.Network/connections  |  BitsInPerSecond  |  BitsInPerSecond  |  Anzahl pro Sekunde  |  Average | 
| **Ja**  | Nein |  Microsoft.Network/connections  |  BitsOutPerSecond  |  BitsOutPerSecond  |  Anzahl pro Sekunde  |  Average | 
| **Ja**  | Nein |  Microsoft.Network/dnszones  |  QueryVolume  |  Abfragevolume  |  Anzahl  |  Gesamt | 
| Nein  | Nein |  Microsoft.Network/dnszones  |  RecordSetCapacityUtilization  |  Kapazitätsauslastung von Datensatzgruppen  |  Percent  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Network/dnszones  |  RecordSetCount  |  Anzahl von Datensatzgruppen  |  Anzahl  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Network/expressRouteCircuits  |  ArpAvailability  |  ARP-Verfügbarkeit  |  Percent  |  Average | 
| **Ja**  | Nein |  Microsoft.Network/expressRouteCircuits  |  BgpAvailability  |  BGP-Verfügbarkeit  |  Percent  |  Average | 
| Nein  | Nein |  Microsoft.Network/expressRouteCircuits  |  BitsInPerSecond  |  BitsInPerSecond  |  Anzahl pro Sekunde  |  Average | 
| Nein  | Nein |  Microsoft.Network/expressRouteCircuits  |  BitsOutPerSecond  |  BitsOutPerSecond  |  Anzahl pro Sekunde  |  Average | 
| Nein  | Nein |  Microsoft.Network/expressRouteCircuits  |  GlobalReachBitsInPerSecond  |  GlobalReachBitsInPerSecond  |  Anzahl pro Sekunde  |  Average | 
| Nein  | Nein |  Microsoft.Network/expressRouteCircuits  |  GlobalReachBitsOutPerSecond  |  GlobalReachBitsOutPerSecond  |  Anzahl pro Sekunde  |  Average | 
| Nein  | Nein |  Microsoft.Network/expressRouteCircuits  |  QosDropBitsInPerSecond  |  DroppedInBitsPerSecond  |  Anzahl pro Sekunde  |  Average | 
| Nein  | Nein |  Microsoft.Network/expressRouteCircuits  |  QosDropBitsOutPerSecond  |  DroppedOutBitsPerSecond  |  Anzahl pro Sekunde  |  Average | 
| **Ja**  | Nein |  Microsoft.Network/expressRouteCircuits/peerings  |  BitsInPerSecond  |  BitsInPerSecond  |  Anzahl pro Sekunde  |  Average | 
| **Ja**  | Nein |  Microsoft.Network/expressRouteCircuits/peerings  |  BitsOutPerSecond  |  BitsOutPerSecond  |  Anzahl pro Sekunde  |  Average | 
| Nein  | Nein |  Microsoft.Network/expressRouteGateways  |  ErGatewayConnectionBitsInPerSecond  |  BitsInPerSecond  |  Anzahl pro Sekunde  |  Average | 
| Nein  | Nein |  Microsoft.Network/expressRouteGateways  |  ErGatewayConnectionBitsOutPerSecond  |  BitsOutPerSecond  |  Anzahl pro Sekunde  |  Average | 
| **Ja**  | Nein |  Microsoft.Network/expressRoutePorts  |  AdminState  |  AdminState  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.Network/expressRoutePorts  |  LineProtocol  |  LineProtocol  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.Network/expressRoutePorts  |  PortBitsInPerSecond  |  BitsInPerSecond  |  Anzahl pro Sekunde  |  Average | 
| **Ja**  | Nein |  Microsoft.Network/expressRoutePorts  |  PortBitsOutPerSecond  |  BitsOutPerSecond  |  Anzahl pro Sekunde  |  Average | 
| **Ja**  | Nein |  Microsoft.Network/expressRoutePorts  |  RxLightLevel  |  RxLightLevel  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.Network/expressRoutePorts  |  TxLightLevel  |  TxLightLevel  |  Anzahl  |  Average | 
| **Ja**  | **Ja** |  Microsoft.Network/frontdoors  |  BackendHealthPercentage  |  Prozentsatz der Back-End-Integrität  |  Percent  |  Average | 
| **Ja**  | **Ja** |  Microsoft.Network/frontdoors  |  BackendRequestCount  |  Back-End-Anforderungsanzahl  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Network/frontdoors  |  BackendRequestLatency  |  Latenz der Back-End-Anforderung  |  Millisekunden  |  Average | 
| **Ja**  | **Ja** |  Microsoft.Network/frontdoors  |  BillableResponseSize  |  Größe von abrechnungsfähigen Antworten  |  Byte  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Network/frontdoors  |  RequestCount  |  Anzahl der Anforderungen  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Network/frontdoors  |  RequestSize  |  Anforderungsgröße  |  Byte  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Network/frontdoors  |  ResponseSize  |  Antwortgröße  |  Byte  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Network/frontdoors  |  TotalLatency  |  Gesamtlatenz  |  Millisekunden  |  Average | 
| **Ja**  | **Ja** |  Microsoft.Network/frontdoors  |  WebApplicationFirewallRequestCount  |  Anforderungsanzahl für die Web Application Firewall  |  Anzahl  |  Gesamt | 
| Nein  | Nein |  Microsoft.Network/loadBalancers  |  AllocatedSnatPorts  |  Zugeordnete SNAT-Ports (Vorschau)  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Network/loadBalancers  |  ByteCount  |  Byteanzahl  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Network/loadBalancers  |  DipAvailability  |  Integritätsteststatus  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.Network/loadBalancers  |  PacketCount  |  Paketzahl  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Network/loadBalancers  |  SnatConnectionCount  |  Anzahl von SNAT-Verbindungen  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Network/loadBalancers  |  SYNCount  |  SYN-Anzahl  |  Anzahl  |  Gesamt | 
| Nein  | Nein |  Microsoft.Network/loadBalancers  |  UsedSnatPorts  |  Verwendete SNAT-Ports (Vorschau)  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Network/loadBalancers  |  VipAvailability  |  Datenpfadverfügbarkeit  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.Network/networkInterfaces  |  BytesReceivedRate  |  Empfangene Bytes  |  Byte  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Network/networkInterfaces  |  BytesSentRate  |  Gesendete Bytes  |  Byte  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Network/networkInterfaces  |  PacketsReceivedRate  |  Empfangene Pakete  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Network/networkInterfaces  |  PacketsSentRate  |  Gesendete Pakete  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Network/networkWatchers/connectionMonitors  |  AverageRoundtripMs  |  Durchschn. Roundtripzeit (ms)  |  Millisekunden  |  Average | 
| **Ja**  | Nein |  Microsoft.Network/networkWatchers/connectionMonitors  |  ChecksFailedPercent  |  Fehler bei Überprüfungen in Prozent (Vorschau)  |  Percent  |  Average | 
| **Ja**  | Nein |  Microsoft.Network/networkWatchers/connectionMonitors  |  ProbesFailedPercent  |  Fehlerhafte Tests in Prozent  |  Percent  |  Average | 
| **Ja**  | Nein |  Microsoft.Network/networkWatchers/connectionMonitors  |  RoundTripTimeMs  |  Roundtripzeit (ms) (Vorschau)  |  Millisekunden  |  Average | 
| **Ja**  | Nein |  Microsoft.Network/publicIPAddresses  |  ByteCount  |  Byteanzahl  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Network/publicIPAddresses  |  BytesDroppedDDoS  |  Als DDoS eingehende gelöschte Bytes  |  Bytes pro Sekunde  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Network/publicIPAddresses  |  BytesForwardedDDoS  |  Weitergeleitete als DDoS eingehende Bytes  |  Bytes pro Sekunde  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Network/publicIPAddresses  |  BytesInDDoS  |  Als DDoS eingehende Bytes  |  Bytes pro Sekunde  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Network/publicIPAddresses  |  DDoSTriggerSYNPackets  |  Eingehende SYN-Pakete, um die DDoS-Entschärfung auszulösen  |  Anzahl pro Sekunde  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Network/publicIPAddresses  |  DDoSTriggerTCPPackets  |  Eingehende TCP-Pakete, um die DDoS-Entschärfung auszulösen  |  Anzahl pro Sekunde  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Network/publicIPAddresses  |  DDoSTriggerUDPPackets  |  Eingehende UDP-Pakete, um die DDoS-Entschärfung auszulösen  |  Anzahl pro Sekunde  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Network/publicIPAddresses  |  IfUnderDDoSAttack  |  Unter DDoS-Angriff oder nicht  |  Anzahl  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Network/publicIPAddresses  |  PacketCount  |  Paketzahl  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Network/publicIPAddresses  |  PacketsDroppedDDoS  |  Als DDoS eingehende gelöschte Pakete  |  Anzahl pro Sekunde  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Network/publicIPAddresses  |  PacketsForwardedDDoS  |  Weitergeleitete als DDoS eingehende Pakete  |  Anzahl pro Sekunde  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Network/publicIPAddresses  |  PacketsInDDoS  |  Als DDoS eingehende Pakete  |  Anzahl pro Sekunde  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Network/publicIPAddresses  |  SynCount  |  SYN-Anzahl  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Network/publicIPAddresses  |  TCPBytesDroppedDDoS  |  Als DDoS eingehende gelöschte TCP-Bytes  |  Bytes pro Sekunde  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Network/publicIPAddresses  |  TCPBytesForwardedDDoS  |  Weitergeleitete als DDoS eingehende TCP-Bytes  |  Bytes pro Sekunde  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Network/publicIPAddresses  |  TCPBytesInDDoS  |  Als DDoS eingehende TCP-Bytes  |  Bytes pro Sekunde  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Network/publicIPAddresses  |  TCPPacketsDroppedDDoS  |  Als DDoS eingehende gelöschte TCP-Pakete  |  Anzahl pro Sekunde  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Network/publicIPAddresses  |  TCPPacketsForwardedDDoS  |  Weitergeleitete als DDoS eingehende TCP-Pakete  |  Anzahl pro Sekunde  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Network/publicIPAddresses  |  TCPPacketsInDDoS  |  Als DDoS eingehende TCP-Pakete  |  Anzahl pro Sekunde  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Network/publicIPAddresses  |  UDPBytesDroppedDDoS  |  Als DDoS eingehende gelöschte UDP-Bytes  |  Bytes pro Sekunde  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Network/publicIPAddresses  |  UDPBytesForwardedDDoS  |  Weitergeleitete als DDoS eingehende UDP-Bytes  |  Bytes pro Sekunde  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Network/publicIPAddresses  |  UDPBytesInDDoS  |  Als DDoS eingehende UDP-Bytes  |  Bytes pro Sekunde  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Network/publicIPAddresses  |  UDPPacketsDroppedDDoS  |  Als DDoS eingehende gelöschte UDP-Pakete  |  Anzahl pro Sekunde  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Network/publicIPAddresses  |  UDPPacketsForwardedDDoS  |  Weitergeleitete als DDoS eingehende UDP-Pakete  |  Anzahl pro Sekunde  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Network/publicIPAddresses  |  UDPPacketsInDDoS  |  Als DDoS eingehende UDP-Pakete  |  Anzahl pro Sekunde  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Network/publicIPAddresses  |  VipAvailability  |  Datenpfadverfügbarkeit  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.Network/trafficManagerProfiles  |  ProbeAgentCurrentEndpointStateByProfileResourceId  |  Endpunktstatus nach Endpunkt  |  Anzahl  |  Maximum | 
| **Ja**  | **Ja** |  Microsoft.Network/trafficManagerProfiles  |  QpsByEndpoint  |  Abfragen nach zurückgegebenem Endpunkt  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Network/virtualNetworkGateways  |  AverageBandwidth  |  S2S-Gatewaybandbreite  |  Bytes pro Sekunde  |  Average | 
| **Ja**  | Nein |  Microsoft.Network/virtualNetworkGateways  |  P2SBandwidth  |  P2S-Gatewaybandbreite  |  Bytes pro Sekunde  |  Average | 
| **Ja**  | Nein |  Microsoft.Network/virtualNetworkGateways  |  P2SConnectionCount  |  Anzahl der P2S-Verbindungen  |  Anzahl  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Network/virtualNetworkGateways  |  TunnelAverageBandwidth  |  Bandbreite des Tunnels  |  Bytes pro Sekunde  |  Average | 
| **Ja**  | Nein |  Microsoft.Network/virtualNetworkGateways  |  TunnelEgressBytes  |  Vom Tunnel ausgehende Bytes  |  Byte  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Network/virtualNetworkGateways  |  TunnelEgressPacketDropTSMismatch  |  Ausgehende gelöschte Pakete eines Tunnels durch einen TS-Konflikt  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Network/virtualNetworkGateways  |  TunnelEgressPackets  |  Vom Tunnel ausgehende Pakete  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Network/virtualNetworkGateways  |  TunnelIngressBytes  |  In Tunnel eingehende Bytes  |  Byte  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Network/virtualNetworkGateways  |  TunnelIngressPacketDropTSMismatch  |  Eingehende gelöschte Pakete eines Tunnels durch einen TS-Konflikt  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Network/virtualNetworkGateways  |  TunnelIngressPackets  |  In Tunnel eingehende Pakete  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Network/virtualNetworks  |  PingMeshAverageRoundtripMs  |  Roundtripzeit für Pings an eine VM  |  Millisekunden  |  Average | 
| **Ja**  | Nein |  Microsoft.Network/virtualNetworks  |  PingMeshProbesFailedPercent  |  Fehlerhafte Pings an einen virtuellen Computer  |  Percent  |  Average | 
| **Ja**  | **Ja** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  incoming  |  Eingehende Nachrichten  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  incoming.all.failedrequests  |  Alle eingehenden fehlerhaften Anforderungen  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  incoming.all.requests  |  Alle eingehenden Anforderungen  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  incoming.scheduled  |  Geplante Pushbenachrichtigungen (gesendet)  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  incoming.scheduled.cancel  |  Geplante Pushbenachrichtigungen (abgebrochen)  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  installation.all  |  Installationsverwaltungsvorgänge  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  installation.delete  |  Installationsvorgänge löschen  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  installation.get  |  Installationsvorgänge abrufen  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  installation.patch  |  Patchinstallationsvorgänge  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  installation.upsert  |  Installationsvorgänge erstellen oder aktualisieren  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  notificationhub.pushes  |  Alle ausgehenden Benachrichtigungen  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.allpns.badorexpiredchannel  |  Kanalfehler (unzulässig oder abgelaufen)  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.allpns.channelerror  |  Kanalfehler  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.allpns.invalidpayload  |  Nutzlastfehler  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.allpns.pnserror  |  Fehler des externen Benachrichtigungssystems  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.allpns.success  |  Erfolgreiche Benachrichtigungen  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.apns.badchannel  |  APNS-Fehler durch fehlerhaften Kanal  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.apns.expiredchannel  |  APNS-Fehler durch abgelaufenen Kanal  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.apns.invalidcredentials  |  APNS-Autorisierungsfehler  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.apns.invalidnotificationsize  |  APNS-Fehler durch ungültige Benachrichtigungsgröße  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.apns.pnserror  |  APNS-Fehler  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.apns.success  |  Erfolgreiche APNS-Benachrichtigungen  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.gcm.authenticationerror  |  GCM-Authentifizierungsfehler  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.gcm.badchannel  |  GCM-Fehler durch fehlerhaften Kanal  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.gcm.expiredchannel  |  GCM-Fehler durch abgelaufenen Kanal  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.gcm.invalidcredentials  |  GCM-Autorisierungsfehler (ungültige Anmeldeinformationen)  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.gcm.invalidnotificationformat  |  Ungültiges GCM-Benachrichtigungsformat  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.gcm.invalidnotificationsize  |  GCM-Fehler durch ungültige Benachrichtigungsgröße  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.gcm.pnserror  |  GCM-Fehler  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.gcm.success  |  Erfolgreiche GCM-Benachrichtigungen  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.gcm.throttled  |  Gedrosselte GCM-Benachrichtigungen  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.gcm.wrongchannel  |  GCM-Fehler durch falschen Kanal  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.mpns.authenticationerror  |  MPNS-Authentifizierungsfehler  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.mpns.badchannel  |  MPNS-Fehler durch fehlerhaften Kanal  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.mpns.channeldisconnected  |  MPNS-Kanal nicht verbunden  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.mpns.dropped  |  Verworfene MPNS-Benachrichtigungen  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.mpns.invalidcredentials  |  Ungültige MPNS-Anmeldeinformationen  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.mpns.invalidnotificationformat  |  Ungültiges MPNS-Benachrichtigungsformat  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.mpns.pnserror  |  MPNS-Fehler  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.mpns.success  |  Erfolgreiche MPNS-Benachrichtigungen  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.mpns.throttled  |  Gedrosselte MPNS-Benachrichtigungen  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.wns.authenticationerror  |  WNS-Authentifizierungsfehler  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.wns.badchannel  |  WNS-Fehler durch fehlerhaften Kanal  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.wns.channeldisconnected  |  WNS-Kanal nicht verbunden  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.wns.channelthrottled  |  WNS-Kanal gedrosselt  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.wns.dropped  |  Verworfene WNS-Benachrichtigungen  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.wns.expiredchannel  |  WNS-Fehler durch abgelaufenen Kanal  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.wns.invalidcredentials  |  WNS-Autorisierungsfehler (ungültige Anmeldeinformationen)  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.wns.invalidnotificationformat  |  Ungültiges WNS-Benachrichtigungsformat  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.wns.invalidnotificationsize  |  WNS-Fehler durch ungültige Benachrichtigungsgröße  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.wns.invalidtoken  |  WNS-Autorisierungsfehler (ungültiges Token)  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.wns.pnserror  |  WNS-Fehler  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.wns.success  |  Erfolgreiche WNS-Benachrichtigungen  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.wns.throttled  |  Gedrosselte WNS-Benachrichtigungen  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.wns.tokenproviderunreachable  |  WNS-Autorisierungsfehler (nicht erreichbar)  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.wns.wrongtoken  |  WNS-Autorisierungsfehler (falsches Token)  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  registration.all  |  Registrierungsvorgänge  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  registration.create  |  Registrierungserstellungsvorgänge  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  registration.delete  |  Registrierungslöschvorgänge  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  registration.get  |  Registrierungslesevorgänge  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  registration.update  |  Registrierungsaktualisierungsvorgänge  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  scheduled.pending  |  Ausstehende Pushbenachrichtigungen  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.OperationalInsights/workspaces  |  Average_% Available Memory  |  % verfügbarer Speicher  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.OperationalInsights/workspaces  |  Average_% Available Swap Space  |  % verfügbarer Auslagerungsbereich  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.OperationalInsights/workspaces  |  Average_% Committed Bytes In Use  |  Zugesicherte verwendete Bytes (%)  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.OperationalInsights/workspaces  |  Average_% DPC Time  |  % DPC-Zeit  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.OperationalInsights/workspaces  |  Average_% Free Inodes  |  % freie Inodes  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.OperationalInsights/workspaces  |  Average_% Free Space  |  % freier Speicher  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.OperationalInsights/workspaces  |  Average_% Free Space  |  % freier Speicher  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.OperationalInsights/workspaces  |  Average_% Idle Time  |  % Leerlaufzeit  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.OperationalInsights/workspaces  |  Average_% Interrupt Time  |  % Interruptzeit  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.OperationalInsights/workspaces  |  Average_% IO Wait Time  |  % E/a-Wartezeit  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.OperationalInsights/workspaces  |  Average_% Nice Time  |  % Nice-Zeit  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.OperationalInsights/workspaces  |  Average_% Privileged Time  |  % privilegierte Zeit  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.OperationalInsights/workspaces  |  Average_% Processor Time  |  % Prozessorzeit  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.OperationalInsights/workspaces  |  Average_% Processor Time  |  % Prozessorzeit  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.OperationalInsights/workspaces  |  Average_% Used Inodes  |  % verwendete Inodes  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.OperationalInsights/workspaces  |  Average_% Used Memory  |  % verwendeter Arbeitsspeicher  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.OperationalInsights/workspaces  |  Average_% Used Space  |  % verwendeter Speicher  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.OperationalInsights/workspaces  |  Average_% Used Swap Space  |  % verwendeter Auslagerungsbereich  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.OperationalInsights/workspaces  |  Average_% User Time  |  % Benutzerzeit  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.OperationalInsights/workspaces  |  Average_Available MBytes  |  Verfügbare MB  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.OperationalInsights/workspaces  |  Average_Available MBytes Memory  |  Verfügbarer Speicher in MB  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.OperationalInsights/workspaces  |  Average_Available MBytes Swap  |  Verfügbarer Auslagerungsbereich in MB  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.OperationalInsights/workspaces  |  Average_Avg. Datenträger s/gelesen  |  Durchschn. Datenträger s/gelesen  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.OperationalInsights/workspaces  |  Average_Avg. Datenträger s/gelesen  |  Durchschn. Datenträger s/gelesen  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.OperationalInsights/workspaces  |  Average_Avg. Datenträger s/übertragen  |  Durchschn. Datenträger s/übertragen  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.OperationalInsights/workspaces  |  Average_Avg. Datenträger s/geschrieben  |  Durchschn. Datenträger s/geschrieben  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.OperationalInsights/workspaces  |  Average_Avg. Datenträger s/geschrieben  |  Durchschn. Datenträger s/geschrieben  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.OperationalInsights/workspaces  |  Average_Bytes Received/sec  |  Empfangene Byte/Sek.  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.OperationalInsights/workspaces  |  Average_Bytes Sent/sec  |  Gesendete Byte/Sek.  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.OperationalInsights/workspaces  |  Average_Bytes Total/sec  |  Byte gesamt/Sek.  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.OperationalInsights/workspaces  |  Average_Current Disk Queue Length  |  Aktuelle Warteschlangenlänge  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.OperationalInsights/workspaces  |  Average_Disk Read Bytes/sec  |  Byte gelesen/s  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.OperationalInsights/workspaces  |  Average_Disk Reads/sec  |  Lesevorgänge/s  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.OperationalInsights/workspaces  |  Average_Disk Reads/sec  |  Lesevorgänge/s  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.OperationalInsights/workspaces  |  Average_Disk Transfers/sec  |  Übertragungen/s  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.OperationalInsights/workspaces  |  Average_Disk Transfers/sec  |  Übertragungen/s  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.OperationalInsights/workspaces  |  Average_Disk Write Bytes/sec  |  Byte geschrieben/s  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.OperationalInsights/workspaces  |  Average_Disk Writes/sec  |  Schreibvorgänge/s  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.OperationalInsights/workspaces  |  Average_Disk Writes/sec  |  Schreibvorgänge/s  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.OperationalInsights/workspaces  |  Average_Free Megabytes  |  Freie Megabytes  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.OperationalInsights/workspaces  |  Average_Free Megabytes  |  Freie Megabytes  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.OperationalInsights/workspaces  |  Average_Free Physical Memory  |  Freier physischer Speicher  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.OperationalInsights/workspaces  |  Average_Free Space in Paging Files  |  Freier Speicherplatz in Auslagerungsdateien  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.OperationalInsights/workspaces  |  Average_Free Virtual Memory  |  Freier virtueller Arbeitsspeicher  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.OperationalInsights/workspaces  |  Average_Logical Disk Bytes/sec  |  Logischer Datenträger Bytes/s  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.OperationalInsights/workspaces  |  Average_Page Reads/sec  |  Seitenlesevorgänge/s  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.OperationalInsights/workspaces  |  Average_Page Writes/sec  |  Schreibvorgänge/s  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.OperationalInsights/workspaces  |  Average_Pages/sec  |  Seiten/s  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.OperationalInsights/workspaces  |  Average_Pct Privileged Time  |  PCT-privilegierte Zeit  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.OperationalInsights/workspaces  |  Average_Pct User Time  |  PCT-Benutzerzeit  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.OperationalInsights/workspaces  |  Average_Physical Disk Bytes/sec  |  Physischer Datenträger Bytes/s  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.OperationalInsights/workspaces  |  Average_Processes  |  Prozesse  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.OperationalInsights/workspaces  |  Average_Processor Queue Length  |  Länge der Prozessorwarteschlange  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.OperationalInsights/workspaces  |  Average_Size Stored In Paging Files  |  Genutzter Speicherplatz in Auslagerungsdateien  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.OperationalInsights/workspaces  |  Average_Total Bytes  |  Summe Bytes  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.OperationalInsights/workspaces  |  Average_Total Bytes Received  |  Summe empfangene Bytes  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.OperationalInsights/workspaces  |  Average_Total Bytes Transmitted  |  Summe übertragene Bytes  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.OperationalInsights/workspaces  |  Average_Total Collisions  |  Summe Konflikte  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.OperationalInsights/workspaces  |  Average_Total Packets Received  |  Summe empfangene Pakete  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.OperationalInsights/workspaces  |  Average_Total Packets Transmitted  |  Summe übermittelte Pakete  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.OperationalInsights/workspaces  |  Average_Total Rx Errors  |  Summe Rx-Fehler  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.OperationalInsights/workspaces  |  Average_Total Tx Errors  |  Summe Tx-Fehler  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.OperationalInsights/workspaces  |  Average_Uptime  |  Betriebszeit  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.OperationalInsights/workspaces  |  Average_Used MBytes Swap Space  |  Verwendeter Auslagerungsbereich in MB  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.OperationalInsights/workspaces  |  Average_Used Memory kBytes  |  Verwendeter Arbeitsspeicher in KB  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.OperationalInsights/workspaces  |  Average_Used Memory MBytes  |  Verwendeter Speicher in MB  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.OperationalInsights/workspaces  |  Average_Users  |  Benutzer  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.OperationalInsights/workspaces  |  Average_Virtual Shared Memory  |  Virtueller gemeinsamer Speicher  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.OperationalInsights/workspaces  |  Ereignis  |  Ereignis  |  Anzahl  |  Average | 
| **Ja**  | **Ja** |  Microsoft.OperationalInsights/workspaces  |  Heartbeat  |  Heartbeat  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.OperationalInsights/workspaces  |  Aktualisieren  |  Aktualisieren  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.PowerBIDedicated/capacities  |  memory_metric  |  Arbeitsspeicher  |  Byte  |  Average | 
| **Ja**  | Nein |  Microsoft.PowerBIDedicated/capacities  |  memory_thrashing_metric  |  Arbeitsspeicherüberlastung (Datasets)  |  Percent  |  Average | 
| **Ja**  | Nein |  Microsoft.PowerBIDedicated/capacities  |  qpu_high_utilization_metric  |  Hohe QPU-Auslastung  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.PowerBIDedicated/capacities  |  QueryDuration  |  Abfragedauer (Datasets)  |  Millisekunden  |  Average | 
| **Ja**  | Nein |  Microsoft.PowerBIDedicated/capacities  |  QueryPoolJobQueueLength  |  Auftragswarteschlangenlänge für Abfragepool (Datasets)  |  Anzahl  |  Average | 
| Nein  | Nein |  Microsoft.Relay/namespaces  |  ActiveConnections  |  ActiveConnections  |  Anzahl  |  Gesamt | 
| Nein  | Nein |  Microsoft.Relay/namespaces  |  ActiveListeners  |  ActiveListeners  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Relay/namespaces  |  BytesTransferred  |  BytesTransferred  |  Anzahl  |  Gesamt | 
| Nein  | Nein |  Microsoft.Relay/namespaces  |  ListenerConnections-ClientError  |  ListenerConnections-ClientError  |  Anzahl  |  Gesamt | 
| Nein  | Nein |  Microsoft.Relay/namespaces  |  ListenerConnections-ServerError  |  ListenerConnections-ServerError  |  Anzahl  |  Gesamt | 
| Nein  | Nein |  Microsoft.Relay/namespaces  |  ListenerConnections-Success  |  ListenerConnections-Success  |  Anzahl  |  Gesamt | 
| Nein  | Nein |  Microsoft.Relay/namespaces  |  ListenerConnections-TotalRequests  |  ListenerConnections-TotalRequests  |  Anzahl  |  Gesamt | 
| Nein  | Nein |  Microsoft.Relay/namespaces  |  ListenerDisconnects  |  ListenerDisconnects  |  Anzahl  |  Gesamt | 
| Nein  | Nein |  Microsoft.Relay/namespaces  |  SenderConnections-ClientError  |  SenderConnections-ClientError  |  Anzahl  |  Gesamt | 
| Nein  | Nein |  Microsoft.Relay/namespaces  |  SenderConnections-ServerError  |  SenderConnections-ServerError  |  Anzahl  |  Gesamt | 
| Nein  | Nein |  Microsoft.Relay/namespaces  |  SenderConnections-Success  |  SenderConnections-Success  |  Anzahl  |  Gesamt | 
| Nein  | Nein |  Microsoft.Relay/namespaces  |  SenderConnections-TotalRequests  |  SenderConnections-TotalRequests  |  Anzahl  |  Gesamt | 
| Nein  | Nein |  Microsoft.Relay/namespaces  |  SenderDisconnects  |  SenderDisconnects  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Search/searchServices  |  SearchLatency  |  Suchlatenz  |  Sekunden  |  Average | 
| **Ja**  | Nein |  Microsoft.Search/searchServices  |  SearchQueriesPerSecond  |  Suchabfragen pro Sekunde  |  Anzahl pro Sekunde  |  Average | 
| **Ja**  | Nein |  Microsoft.Search/searchServices  |  ThrottledSearchQueriesPercentage  |  Gedrosselte Suchabfragen in Prozent  |  Percent  |  Average | 
| Nein  | Nein |  Microsoft.ServiceBus/namespaces  |  ActiveConnections  |  ActiveConnections  |  Anzahl  |  Gesamt | 
| Nein  | Nein |  Microsoft.ServiceBus/namespaces  |  ActiveMessages  |  Anzahl von aktiven Nachrichten in einer Warteschlange/einem Thema  |  Anzahl  |  Average | 
| Nein  | Nein |  Microsoft.ServiceBus/namespaces  |  ConnectionsClosed  |  Geschlossene Verbindungen.  |  Anzahl  |  Average | 
| Nein  | Nein |  Microsoft.ServiceBus/namespaces  |  ConnectionsOpened  |  Geöffnete Verbindungen.  |  Anzahl  |  Average | 
| Nein  | Nein |  Microsoft.ServiceBus/namespaces  |  CPUXNS  |  CPU (veraltet)  |  Percent  |  Maximum | 
| Nein  | Nein |  Microsoft.ServiceBus/namespaces  |  DeadletteredMessages  |  Anzahl von unzustellbaren Nachrichten in einer Warteschlange/einem Thema  |  Anzahl  |  Average | 
| **Ja**  | **Ja** |  Microsoft.ServiceBus/namespaces  |  IncomingMessages  |  Eingehende Nachrichten  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.ServiceBus/namespaces  |  IncomingRequests  |  Eingehende Anforderungen  |  Anzahl  |  Gesamt | 
| Nein  | Nein |  Microsoft.ServiceBus/namespaces  |  Meldungen  |  Anzahl von Nachrichten in einer Warteschlange/einem Thema  |  Anzahl  |  Average | 
| Nein  | Nein |  Microsoft.ServiceBus/namespaces  |  NamespaceCpuUsage  |  CPU  |  Percent  |  Maximum | 
| Nein  | Nein |  Microsoft.ServiceBus/namespaces  |  NamespaceMemoryUsage  |  Speicherauslastung  |  Percent  |  Maximum | 
| **Ja**  | **Ja** |  Microsoft.ServiceBus/namespaces  |  OutgoingMessages  |  Ausgehende Nachrichten  |  Anzahl  |  Gesamt | 
| Nein  | Nein |  Microsoft.ServiceBus/namespaces  |  ScheduledMessages  |  Anzahl von geplanten Nachrichten in einer Warteschlange/einem Thema  |  Anzahl  |  Average | 
| Nein  | Nein |  Microsoft.ServiceBus/namespaces  |  ServerErrors  |  Serverfehler.  |  Anzahl  |  Gesamt | 
| Nein  | Nein |  Microsoft.ServiceBus/namespaces  |  Size  |  Size  |  Byte  |  Average | 
| Nein  | Nein |  Microsoft.ServiceBus/namespaces  |  SuccessfulRequests  |  Erfolgreiche Anforderungen  |  Anzahl  |  Gesamt | 
| Nein  | Nein |  Microsoft.ServiceBus/namespaces  |  ThrottledRequests  |  Gedrosselte Anforderungen.  |  Anzahl  |  Gesamt | 
| Nein  | Nein |  Microsoft.ServiceBus/namespaces  |  UserErrors  |  Benutzerfehler.  |  Anzahl  |  Gesamt | 
| Nein  | Nein |  Microsoft.ServiceBus/namespaces  |  WSXNS  |  Speicherauslastung (Deprecated)  |  Percent  |  Maximum | 
| Nein  | Nein |  Microsoft.ServiceFabricMesh/applications  |  ActualCpu  |  ActualCpu  |  Anzahl  |  Average | 
| Nein  | Nein |  Microsoft.ServiceFabricMesh/applications  |  ActualMemory  |  ActualMemory  |  Byte  |  Average | 
| Nein  | Nein |  Microsoft.ServiceFabricMesh/applications  |  AllocatedCpu  |  AllocatedCpu  |  Anzahl  |  Average | 
| Nein  | Nein |  Microsoft.ServiceFabricMesh/applications  |  AllocatedMemory  |  AllocatedMemory  |  Byte  |  Average | 
| Nein  | Nein |  Microsoft.ServiceFabricMesh/applications  |  ApplicationStatus  |  ApplicationStatus  |  Anzahl  |  Average | 
| Nein  | Nein |  Microsoft.ServiceFabricMesh/applications  |  ContainerStatus  |  ContainerStatus  |  Anzahl  |  Average | 
| Nein  | Nein |  Microsoft.ServiceFabricMesh/applications  |  CpuUtilization  |  CpuUtilization  |  Percent  |  Average | 
| Nein  | Nein |  Microsoft.ServiceFabricMesh/applications  |  MemoryUtilization  |  MemoryUtilization  |  Percent  |  Average | 
| Nein  | Nein |  Microsoft.ServiceFabricMesh/applications  |  RestartCount  |  RestartCount  |  Anzahl  |  Average | 
| Nein  | Nein |  Microsoft.ServiceFabricMesh/applications  |  ServiceReplicaStatus  |  ServiceReplicaStatus  |  Anzahl  |  Average | 
| Nein  | Nein |  Microsoft.ServiceFabricMesh/applications  |  ServiceStatus  |  ServiceStatus  |  Anzahl  |  Average | 
| **Ja**  | **Ja** |  Microsoft.SignalRService/SignalR  |  ConnectionCount  |  Anzahl der Verbindungen  |  Anzahl  |  Maximum | 
| **Ja**  | **Ja** |  Microsoft.SignalRService/SignalR  |  InboundTraffic  |  Eingehender Datenverkehr  |  Byte  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.SignalRService/SignalR  |  MessageCount  |  Nachrichtenanzahl  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.SignalRService/SignalR  |  OutboundTraffic  |  Ausgehender Datenverkehr  |  Byte  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.SignalRService/SignalR  |  SystemErrors  |  Systemfehler  |  Percent  |  Maximum | 
| **Ja**  | **Ja** |  Microsoft.SignalRService/SignalR  |  UserErrors  |  Benutzerfehler  |  Percent  |  Maximum | 
| **Ja**  | **Ja** |  Microsoft.Sql/managedInstances  |  avg_cpu_percent  |  Durchschnittlicher CPU-Prozentsatz  |  Percent  |  Average | 
| **Ja**  | Nein |  Microsoft.Sql/managedInstances  |  io_bytes_read  |  Gelesene E/A-Bytes  |  Byte  |  Average | 
| **Ja**  | Nein |  Microsoft.Sql/managedInstances  |  io_bytes_written  |  Geschriebene E/A-Bytes  |  Byte  |  Average | 
| **Ja**  | Nein |  Microsoft.Sql/managedInstances  |  io_requests  |  Anzahl von E/A-Anforderungen  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.Sql/managedInstances  |  reserved_storage_mb  |  Reservierter Speicherplatz  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.Sql/managedInstances  |  storage_space_used_mb  |  Belegter Speicherplatz  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.Sql/managedInstances  |  virtual_core_count  |  Anzahl virtueller Kerne  |  Anzahl  |  Average | 
| Nein  | Nein |  Microsoft.Sql/servers  |  database_dtu_consumption_percent  |  DTU-Prozentsatz  |  Percent  |  Average | 
| Nein  | Nein |  Microsoft.Sql/servers  |  database_storage_used  |  Genutzter Datenspeicherplatz  |  Byte  |  Average | 
| **Ja**  | **Ja** |  Microsoft.Sql/servers  |  dtu_consumption_percent  |  DTU-Prozentsatz  |  Percent  |  Average | 
| **Ja**  | **Ja** |  Microsoft.Sql/servers  |  dtu_used  |  DTU-Verbrauch  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.Sql/servers  |  storage_used  |  Genutzter Datenspeicherplatz  |  Byte  |  Average | 
| **Ja**  | Nein |  Microsoft.Sql/servers/databases  |  allocated_data_storage  |  Zugeordneter Datenspeicherplatz  |  Byte  |  Average | 
| **Ja**  | Nein |  Microsoft.Sql/servers/databases  |  app_cpu_billed  |  Abgerechnete App-CPU  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Sql/servers/databases  |  app_cpu_percent  |  App-CPU-Prozentsatz  |  Percent  |  Average | 
| **Ja**  | Nein |  Microsoft.Sql/servers/databases  |  app_memory_percent  |  App-Arbeitsspeicherprozentsatz  |  Percent  |  Average | 
| **Ja**  | **Ja** |  Microsoft.Sql/servers/databases  |  blocked_by_firewall  |  Von der Firewall blockiert  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Sql/servers/databases  |  cache_hit_percent  |  Prozentsatz der Cachetreffer  |  Percent  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Sql/servers/databases  |  cache_used_percent  |  Cacheverwendung in Prozent  |  Percent  |  Maximum | 
| **Ja**  | **Ja** |  Microsoft.Sql/servers/databases  |  connection_failed  |  Verbindungsfehler  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Sql/servers/databases  |  connection_successful  |  Erfolgreiche Verbindungen  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Sql/servers/databases  |  cpu_limit  |  CPU-Grenzwert  |  Anzahl  |  Average | 
| **Ja**  | **Ja** |  Microsoft.Sql/servers/databases  |  cpu_percent  |  CPU-Prozentsatz  |  Percent  |  Average | 
| **Ja**  | **Ja** |  Microsoft.Sql/servers/databases  |  cpu_used  |  Verwendete CPU  |  Anzahl  |  Average | 
| **Ja**  | **Ja** |  Microsoft.Sql/servers/databases  |  deadlock  |  Deadlocks  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Sql/servers/databases  |  dtu_consumption_percent  |  DTU-Prozentsatz  |  Percent  |  Average | 
| **Ja**  | Nein |  Microsoft.Sql/servers/databases  |  dtu_limit  |  DTU-Grenzwert  |  Anzahl  |  Average | 
| **Ja**  | **Ja** |  Microsoft.Sql/servers/databases  |  dtu_used  |  DTU-Verbrauch  |  Anzahl  |  Average | 
| **Ja**  | **Ja** |  Microsoft.Sql/servers/databases  |  dwu_consumption_percent  |  DWU in Prozent  |  Percent  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Sql/servers/databases  |  dwu_limit  |  DWU-Grenzwert  |  Anzahl  |  Maximum | 
| **Ja**  | **Ja** |  Microsoft.Sql/servers/databases  |  dwu_used  |  DWU-Verbrauch  |  Anzahl  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Sql/servers/databases  |  local_tempdb_usage_percent  |  Lokaler tempdb-Prozentsatz  |  Percent  |  Average | 
| **Ja**  | **Ja** |  Microsoft.Sql/servers/databases  |  log_write_percent  |  E/A-Prozentsatz für Protokoll  |  Percent  |  Average | 
| **Ja**  | Nein |  Microsoft.Sql/servers/databases  |  memory_usage_percent  |  Arbeitsspeicherprozentsatz  |  Percent  |  Maximum | 
| **Ja**  | **Ja** |  Microsoft.Sql/servers/databases  |  physical_data_read_percent  |  E/A-Prozentsatz für Daten  |  Percent  |  Average | 
| **Ja**  | **Ja** |  Microsoft.Sql/servers/databases  |  sessions_percent  |  Sitzungen in Prozent  |  Percent  |  Average | 
| **Ja**  | Nein |  Microsoft.Sql/servers/databases  |  storage  |  Genutzter Datenspeicherplatz  |  Byte  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Sql/servers/databases  |  storage_percent  |  Genutzter Datenspeicherplatz in Prozent  |  Percent  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Sql/servers/databases  |  tempdb_data_size  |  Größe der tempdb-Datendatei in Kilobytes  |  Anzahl  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Sql/servers/databases  |  tempdb_log_size  |  Größe der tempdb-Protokolldatei in Kilobytes  |  Anzahl  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Sql/servers/databases  |  tempdb_log_used_percent  |  Nutzung des tempdb-Protokolls in Prozent  |  Percent  |  Maximum | 
| **Ja**  | **Ja** |  Microsoft.Sql/servers/databases  |  workers_percent  |  Worker in Prozent  |  Percent  |  Average | 
| **Ja**  | **Ja** |  Microsoft.Sql/servers/databases  |  xtp_storage_percent  |  In-Memory-OLTP-Speicher in Prozent  |  Percent  |  Average | 
| **Ja**  | Nein |  Microsoft.Sql/servers/elasticPools  |  allocated_data_storage  |  Zugeordneter Datenspeicherplatz  |  Byte  |  Average | 
| **Ja**  | Nein |  Microsoft.Sql/servers/elasticPools  |  allocated_data_storage_percent  |  Zugeordneter Datenspeicherplatz in Prozent  |  Percent  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Sql/servers/elasticPools  |  cpu_limit  |  CPU-Grenzwert  |  Anzahl  |  Average | 
| **Ja**  | **Ja** |  Microsoft.Sql/servers/elasticPools  |  cpu_percent  |  CPU-Prozentsatz  |  Percent  |  Average | 
| **Ja**  | Nein |  Microsoft.Sql/servers/elasticPools  |  cpu_used  |  Verwendete CPU  |  Anzahl  |  Average | 
| Nein  | Nein |  Microsoft.Sql/servers/elasticPools  |  database_allocated_data_storage  |  Zugeordneter Datenspeicherplatz  |  Byte  |  Average | 
| Nein  | Nein |  Microsoft.Sql/servers/elasticPools  |  database_cpu_limit  |  CPU-Grenzwert  |  Anzahl  |  Average | 
| Nein  | Nein |  Microsoft.Sql/servers/elasticPools  |  database_cpu_percent  |  CPU-Prozentsatz  |  Percent  |  Average | 
| Nein  | Nein |  Microsoft.Sql/servers/elasticPools  |  database_cpu_used  |  Verwendete CPU  |  Anzahl  |  Average | 
| Nein  | Nein |  Microsoft.Sql/servers/elasticPools  |  database_dtu_consumption_percent  |  DTU-Prozentsatz  |  Percent  |  Average | 
| Nein  | Nein |  Microsoft.Sql/servers/elasticPools  |  database_eDTU_used  |  eDTU-Verbrauch  |  Anzahl  |  Average | 
| Nein  | Nein |  Microsoft.Sql/servers/elasticPools  |  database_log_write_percent  |  E/A-Prozentsatz für Protokoll  |  Percent  |  Average | 
| Nein  | Nein |  Microsoft.Sql/servers/elasticPools  |  database_physical_data_read_percent  |  E/A-Prozentsatz für Daten  |  Percent  |  Average | 
| Nein  | Nein |  Microsoft.Sql/servers/elasticPools  |  database_sessions_percent  |  Sitzungen in Prozent  |  Percent  |  Average | 
| Nein  | Nein |  Microsoft.Sql/servers/elasticPools  |  database_storage_used  |  Genutzter Datenspeicherplatz  |  Byte  |  Average | 
| Nein  | Nein |  Microsoft.Sql/servers/elasticPools  |  database_workers_percent  |  Worker in Prozent  |  Percent  |  Average | 
| **Ja**  | **Ja** |  Microsoft.Sql/servers/elasticPools  |  dtu_consumption_percent  |  DTU-Prozentsatz  |  Percent  |  Average | 
| **Ja**  | Nein |  Microsoft.Sql/servers/elasticPools  |  eDTU_limit  |  eDTU-Grenzwert  |  Anzahl  |  Average | 
| **Ja**  | **Ja** |  Microsoft.Sql/servers/elasticPools  |  eDTU_used  |  eDTU-Verbrauch  |  Anzahl  |  Average | 
| **Ja**  | **Ja** |  Microsoft.Sql/servers/elasticPools  |  log_write_percent  |  E/A-Prozentsatz für Protokoll  |  Percent  |  Average | 
| **Ja**  | **Ja** |  Microsoft.Sql/servers/elasticPools  |  physical_data_read_percent  |  E/A-Prozentsatz für Daten  |  Percent  |  Average | 
| **Ja**  | **Ja** |  Microsoft.Sql/servers/elasticPools  |  sessions_percent  |  Sitzungen in Prozent  |  Percent  |  Average | 
| **Ja**  | Nein |  Microsoft.Sql/servers/elasticPools  |  storage_limit  |  Maximale Datengröße  |  Byte  |  Average | 
| **Ja**  | Nein |  Microsoft.Sql/servers/elasticPools  |  storage_percent  |  Genutzter Datenspeicherplatz in Prozent  |  Percent  |  Average | 
| **Ja**  | Nein |  Microsoft.Sql/servers/elasticPools  |  storage_used  |  Genutzter Datenspeicherplatz  |  Byte  |  Average | 
| **Ja**  | Nein |  Microsoft.Sql/servers/elasticPools  |  tempdb_data_size  |  Größe der tempdb-Datendatei in Kilobytes  |  Anzahl  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Sql/servers/elasticPools  |  tempdb_log_size  |  Größe der tempdb-Protokolldatei in Kilobytes  |  Anzahl  |  Maximum | 
| **Ja**  | Nein |  Microsoft.Sql/servers/elasticPools  |  tempdb_log_used_percent  |  Nutzung des tempdb-Protokolls in Prozent  |  Percent  |  Maximum | 
| **Ja**  | **Ja** |  Microsoft.Sql/servers/elasticPools  |  workers_percent  |  Worker in Prozent  |  Percent  |  Average | 
| **Ja**  | **Ja** |  Microsoft.Sql/servers/elasticPools  |  xtp_storage_percent  |  In-Memory-OLTP-Speicher in Prozent  |  Percent  |  Average | 
| **Ja**  | Nein |  Microsoft.Storage/storageAccounts  |  Verfügbarkeit  |  Verfügbarkeit  |  Percent  |  Average | 
| **Ja**  | Nein |  Microsoft.Storage/storageAccounts  |  Ausgehende Daten  |  Ausgehende Daten  |  Byte  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Storage/storageAccounts  |  Eingehende Daten  |  Eingehende Daten  |  Byte  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Storage/storageAccounts  |  SuccessE2ELatency  |  E2E-Latenz (erfolgreich)  |  Millisekunden  |  Average | 
| **Ja**  | Nein |  Microsoft.Storage/storageAccounts  |  SuccessServerLatency  |  Serverlatenz (erfolgreich)  |  Millisekunden  |  Average | 
| **Ja**  | Nein |  Microsoft.Storage/storageAccounts  |  Transaktionen  |  Transaktionen  |  Anzahl  |  Gesamt | 
| Nein  | Nein |  Microsoft.Storage/storageAccounts  |  UsedCapacity  |  Verwendete Kapazität  |  Byte  |  Average | 
| **Ja**  | Nein |  Microsoft.Storage/storageAccounts/blobServices  |  Verfügbarkeit  |  Verfügbarkeit  |  Percent  |  Average | 
| Nein  | Nein |  Microsoft.Storage/storageAccounts/blobServices  |  BlobCapacity  |  Blob-Kapazität  |  Byte  |  Average | 
| Nein  | Nein |  Microsoft.Storage/storageAccounts/blobServices  |  BlobCount  |  Anzahl von Blobs  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.Storage/storageAccounts/blobServices  |  ContainerCount  |  Anzahl von Blob-Containern  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.Storage/storageAccounts/blobServices  |  Ausgehende Daten  |  Ausgehende Daten  |  Byte  |  Gesamt | 
| Nein  | Nein |  Microsoft.Storage/storageAccounts/blobServices  |  IndexCapacity  |  Indexkapazität  |  Byte  |  Average | 
| **Ja**  | Nein |  Microsoft.Storage/storageAccounts/blobServices  |  Eingehende Daten  |  Eingehende Daten  |  Byte  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Storage/storageAccounts/blobServices  |  SuccessE2ELatency  |  E2E-Latenz (erfolgreich)  |  Millisekunden  |  Average | 
| **Ja**  | Nein |  Microsoft.Storage/storageAccounts/blobServices  |  SuccessServerLatency  |  Serverlatenz (erfolgreich)  |  Millisekunden  |  Average | 
| **Ja**  | Nein |  Microsoft.Storage/storageAccounts/blobServices  |  Transaktionen  |  Transaktionen  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Storage/storageAccounts/fileServices  |  Verfügbarkeit  |  Verfügbarkeit  |  Percent  |  Average | 
| **Ja**  | Nein |  Microsoft.Storage/storageAccounts/fileServices  |  Ausgehende Daten  |  Ausgehende Daten  |  Byte  |  Gesamt | 
| Nein  | Nein |  Microsoft.Storage/storageAccounts/fileServices  |  FileCapacity  |  Dateikapazität  |  Byte  |  Average | 
| Nein  | Nein |  Microsoft.Storage/storageAccounts/fileServices  |  FileCount  |  Dateianzahl  |  Anzahl  |  Average | 
| Nein  | Nein |  Microsoft.Storage/storageAccounts/fileServices  |  FileShareCount  |  Anzahl von Dateifreigaben  |  Anzahl  |  Average | 
| Nein  | Nein |  Microsoft.Storage/storageAccounts/fileServices  |  FileShareQuota  |  Kontingentgröße für Dateifreigabe  |  Byte  |  Average | 
| Nein  | Nein |  Microsoft.Storage/storageAccounts/fileServices  |  FileShareSnapshotCount  |  Anzahl von Momentaufnahmen in Dateifreigabe  |  Anzahl  |  Average | 
| Nein  | Nein |  Microsoft.Storage/storageAccounts/fileServices  |  FileShareSnapshotSize  |  Größe der Momentaufnahmen in Dateifreigabe  |  Byte  |  Average | 
| **Ja**  | Nein |  Microsoft.Storage/storageAccounts/fileServices  |  Eingehende Daten  |  Eingehende Daten  |  Byte  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Storage/storageAccounts/fileServices  |  SuccessE2ELatency  |  E2E-Latenz (erfolgreich)  |  Millisekunden  |  Average | 
| **Ja**  | Nein |  Microsoft.Storage/storageAccounts/fileServices  |  SuccessServerLatency  |  Serverlatenz (erfolgreich)  |  Millisekunden  |  Average | 
| **Ja**  | Nein |  Microsoft.Storage/storageAccounts/fileServices  |  Transaktionen  |  Transaktionen  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Storage/storageAccounts/queueServices  |  Verfügbarkeit  |  Verfügbarkeit  |  Percent  |  Average | 
| **Ja**  | Nein |  Microsoft.Storage/storageAccounts/queueServices  |  Ausgehende Daten  |  Ausgehende Daten  |  Byte  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Storage/storageAccounts/queueServices  |  Eingehende Daten  |  Eingehende Daten  |  Byte  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Storage/storageAccounts/queueServices  |  QueueCapacity  |  Warteschlangenkapazität  |  Byte  |  Average | 
| **Ja**  | Nein |  Microsoft.Storage/storageAccounts/queueServices  |  QueueCount  |  Anzahl von Warteschlangen  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.Storage/storageAccounts/queueServices  |  QueueMessageCount  |  Anzahl von Warteschlangennachrichten  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.Storage/storageAccounts/queueServices  |  SuccessE2ELatency  |  E2E-Latenz (erfolgreich)  |  Millisekunden  |  Average | 
| **Ja**  | Nein |  Microsoft.Storage/storageAccounts/queueServices  |  SuccessServerLatency  |  Serverlatenz (erfolgreich)  |  Millisekunden  |  Average | 
| **Ja**  | Nein |  Microsoft.Storage/storageAccounts/queueServices  |  Transaktionen  |  Transaktionen  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Storage/storageAccounts/tableServices  |  Verfügbarkeit  |  Verfügbarkeit  |  Percent  |  Average | 
| **Ja**  | Nein |  Microsoft.Storage/storageAccounts/tableServices  |  Ausgehende Daten  |  Ausgehende Daten  |  Byte  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Storage/storageAccounts/tableServices  |  Eingehende Daten  |  Eingehende Daten  |  Byte  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.Storage/storageAccounts/tableServices  |  SuccessE2ELatency  |  E2E-Latenz (erfolgreich)  |  Millisekunden  |  Average | 
| **Ja**  | Nein |  Microsoft.Storage/storageAccounts/tableServices  |  SuccessServerLatency  |  Serverlatenz (erfolgreich)  |  Millisekunden  |  Average | 
| **Ja**  | Nein |  Microsoft.Storage/storageAccounts/tableServices  |  TableCapacity  |  Tabellenkapazität  |  Byte  |  Average | 
| **Ja**  | Nein |  Microsoft.Storage/storageAccounts/tableServices  |  TableCount  |  Anzahl von Tabellen  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.Storage/storageAccounts/tableServices  |  TableEntityCount  |  Anzahl von Tabellenentitäten  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.Storage/storageAccounts/tableServices  |  Transaktionen  |  Transaktionen  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.StorageCache/caches  |  ClientIOPS  |  Client-IOPS insgesamt  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.StorageCache/caches  |  ClientLatency  |  Durchschnittliche Clientlatenz  |  Millisekunden  |  Average | 
| **Ja**  | Nein |  Microsoft.StorageCache/caches  |  ClientLockIOPS  |  IOPS für Clientsperren  |  Anzahl pro Sekunde  |  Average | 
| **Ja**  | Nein |  Microsoft.StorageCache/caches  |  ClientMetadataReadIOPS  |  IOPS für Metadatenlesevorgänge durch Client  |  Anzahl pro Sekunde  |  Average | 
| **Ja**  | Nein |  Microsoft.StorageCache/caches  |  ClientMetadataWriteIOPS  |  IOPS für Metadatenschreibvorgänge durch Client  |  Anzahl pro Sekunde  |  Average | 
| **Ja**  | Nein |  Microsoft.StorageCache/caches  |  ClientReadIOPS  |  Lese-IOPS auf Client  |  Anzahl pro Sekunde  |  Average | 
| **Ja**  | Nein |  Microsoft.StorageCache/caches  |  ClientReadThroughput  |  Durchschnittlicher Durchsatz von Cachelesevorgängen  |  Bytes pro Sekunde  |  Average | 
| **Ja**  | Nein |  Microsoft.StorageCache/caches  |  ClientWriteIOPS  |  Schreib-IOPS auf Client  |  Anzahl pro Sekunde  |  Average | 
| **Ja**  | Nein |  Microsoft.StorageCache/caches  |  ClientWriteThroughput  |  Durchschnittlicher Durchsatz von Cacheschreibvorgängen  |  Bytes pro Sekunde  |  Average | 
| **Ja**  | Nein |  Microsoft.StorageCache/caches  |  StorageTargetAsyncWriteThroughput  |  Asynchroner Schreibdurchsatz im Speicherziel  |  Bytes pro Sekunde  |  Average | 
| **Ja**  | Nein |  Microsoft.StorageCache/caches  |  StorageTargetFillThroughput  |  Füllungsdurchsatz im Speicherziel  |  Bytes pro Sekunde  |  Average | 
| **Ja**  | Nein |  Microsoft.StorageCache/caches  |  StorageTargetHealth  |  Integrität des Speicherziels  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.StorageCache/caches  |  StorageTargetIOPS  |  Speicherziel-IOPS insgesamt  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.StorageCache/caches  |  StorageTargetLatency  |  Speicherziellatenz  |  Millisekunden  |  Average | 
| **Ja**  | Nein |  Microsoft.StorageCache/caches  |  StorageTargetMetadataReadIOPS  |  Lese-IOPS für Metadaten im Speicherziel  |  Anzahl pro Sekunde  |  Average | 
| **Ja**  | Nein |  Microsoft.StorageCache/caches  |  StorageTargetMetadataWriteIOPS  |  Schreib-IOPS für Metadaten im Speicherziel  |  Anzahl pro Sekunde  |  Average | 
| **Ja**  | Nein |  Microsoft.StorageCache/caches  |  StorageTargetReadAheadThroughput  |  Read-Ahead-Durchsatz im Speicherziel  |  Bytes pro Sekunde  |  Average | 
| **Ja**  | Nein |  Microsoft.StorageCache/caches  |  StorageTargetReadIOPS  |  Lese-IOPS im Speicherziel  |  Anzahl pro Sekunde  |  Average | 
| **Ja**  | Nein |  Microsoft.StorageCache/caches  |  StorageTargetSyncWriteThroughput  |  Synchroner Schreibdurchsatz im Speicherziel  |  Bytes pro Sekunde  |  Average | 
| **Ja**  | Nein |  Microsoft.StorageCache/caches  |  StorageTargetTotalReadThroughput  |  Lesedurchsatz im Speicherziel insgesamt  |  Bytes pro Sekunde  |  Average | 
| **Ja**  | Nein |  Microsoft.StorageCache/caches  |  StorageTargetTotalWriteThroughput  |  Schreibdurchsatz im Speicherziel insgesamt  |  Bytes pro Sekunde  |  Average | 
| **Ja**  | Nein |  Microsoft.StorageCache/caches  |  StorageTargetWriteIOPS  |  Schreib-IOPS im Speicherziel  |  Anzahl  |  Average | 
| **Ja**  | Nein |  Microsoft.StorageCache/caches  |  Betriebszeit  |  Betriebszeit  |  Anzahl  |  Average | 
| **Ja**  | Nein |  microsoft.storagesync/storageSyncServices  |  ServerSyncSessionResult  |  Ergebnis der Synchronisierungssitzung  |  Anzahl  |  Average | 
| **Ja**  | Nein |  microsoft.storagesync/storageSyncServices  |  StorageSyncBatchTransferredFileBytes  |  Bytes synchronisiert  |  Byte  |  Gesamt | 
| **Ja**  | Nein |  microsoft.storagesync/storageSyncServices  |  StorageSyncRecalledNetworkBytesByApplication  |  Cloudtiering-Rückrufgröße nach Anwendung  |  Byte  |  Gesamt | 
| **Ja**  | Nein |  microsoft.storagesync/storageSyncServices  |  StorageSyncRecalledTotalNetworkBytes  |  Cloudtiering-Rückrufgröße  |  Byte  |  Gesamt | 
| **Ja**  | Nein |  microsoft.storagesync/storageSyncServices  |  StorageSyncRecallIOTotalSizeBytes  |  Cloudtieringrückruf  |  Byte  |  Gesamt | 
| **Ja**  | Nein |  microsoft.storagesync/storageSyncServices  |  StorageSyncRecallThroughputBytesPerSecond  |  Cloudtiering-Rückrufdurchsatz  |  Bytes pro Sekunde  |  Average | 
| **Ja**  | Nein |  microsoft.storagesync/storageSyncServices  |  StorageSyncServerHeartbeat  |  Onlinestatus des Servers  |  Anzahl  |  Maximum | 
| **Ja**  | Nein |  microsoft.storagesync/storageSyncServices  |  StorageSyncSyncSessionAppliedFilesCount  |  Synchronisierte Dateien  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  microsoft.storagesync/storageSyncServices  |  StorageSyncSyncSessionPerItemErrorsCount  |  Dateien ohne Synchronisierung  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  microsoft.storagesync/storageSyncServices/registeredServers  |  ServerHeartbeat  |  Onlinestatus des Servers  |  Anzahl  |  Maximum | 
| **Ja**  | Nein |  microsoft.storagesync/storageSyncServices/registeredServers  |  ServerRecallIOTotalSizeBytes  |  Cloudtieringrückruf  |  Byte  |  Gesamt | 
| **Ja**  | Nein |  microsoft.storagesync/storageSyncServices/syncGroups  |  SyncGroupBatchTransferredFileBytes  |  Bytes synchronisiert  |  Byte  |  Gesamt | 
| **Ja**  | Nein |  microsoft.storagesync/storageSyncServices/syncGroups  |  SyncGroupSyncSessionAppliedFilesCount  |  Synchronisierte Dateien  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  microsoft.storagesync/storageSyncServices/syncGroups  |  SyncGroupSyncSessionPerItemErrorsCount  |  Dateien ohne Synchronisierung  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  microsoft.storagesync/storageSyncServices/syncGroups/serverEndpoints  |  ServerEndpointBatchTransferredFileBytes  |  Bytes synchronisiert  |  Byte  |  Gesamt | 
| **Ja**  | Nein |  microsoft.storagesync/storageSyncServices/syncGroups/serverEndpoints  |  ServerEndpointSyncSessionAppliedFilesCount  |  Synchronisierte Dateien  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  microsoft.storagesync/storageSyncServices/syncGroups/serverEndpoints  |  ServerEndpointSyncSessionPerItemErrorsCount  |  Dateien ohne Synchronisierung  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.StreamAnalytics/streamingjobs  |  AMLCalloutFailedRequests  |  Fehlerhafte Funktionsanforderungen  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.StreamAnalytics/streamingjobs  |  AMLCalloutInputEvents  |  Funktionsereignisse  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.StreamAnalytics/streamingjobs  |  AMLCalloutRequests  |  Funktionsanforderungen  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.StreamAnalytics/streamingjobs  |  ConversionErrors  |  Konvertierungsfehler  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.StreamAnalytics/streamingjobs  |  DeserializationError  |  Eingabefehler bei Deserialisierung  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.StreamAnalytics/streamingjobs  |  DroppedOrAdjustedEvents  |  Ereignisse für falsche Reihenfolge  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.StreamAnalytics/streamingjobs  |  EarlyInputEvents  |  Frühe Eingabeereignisse  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.StreamAnalytics/streamingjobs  |  Errors  |  Laufzeitfehler  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.StreamAnalytics/streamingjobs  |  InputEventBytes  |  Eingabeereignisbytes  |  Byte  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.StreamAnalytics/streamingjobs  |  InputEvents  |  Eingabeereignisse  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.StreamAnalytics/streamingjobs  |  InputEventsSourcesBacklogged  |  Eingabeereignisse im Rückstand  |  Anzahl  |  Maximum | 
| **Ja**  | Nein |  Microsoft.StreamAnalytics/streamingjobs  |  InputEventsSourcesPerSecond  |  Empfangene Eingabequellen  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.StreamAnalytics/streamingjobs  |  LateInputEvents  |  Ereignisse bei verspäteter Eingabe  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.StreamAnalytics/streamingjobs  |  OutputEvents  |  Ausgabeereignisse  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.StreamAnalytics/streamingjobs  |  OutputWatermarkDelaySeconds  |  Wasserzeichenverzögerung  |  Sekunden  |  Maximum | 
| **Ja**  | Nein |  Microsoft.StreamAnalytics/streamingjobs  |  ResourceUtilization  |  Speichereinheitnutzung in %  |  Percent  |  Maximum | 
| **Ja**  | Nein |  Microsoft.VMwareCloudSimple/virtualMachines  |  Datenträgerlesevorgänge in Bytes  |  Datenträgerlesevorgänge in Bytes  |  Byte  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.VMwareCloudSimple/virtualMachines  |  Datenträgerlesevorgänge/Sek.  |  Datenträgerlesevorgänge/Sek.  |  Anzahl pro Sekunde  |  Average | 
| **Ja**  | Nein |  Microsoft.VMwareCloudSimple/virtualMachines  |  Datenträgerschreibvorgänge in Bytes  |  Datenträgerschreibvorgänge in Bytes  |  Byte  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.VMwareCloudSimple/virtualMachines  |  Datenträgerschreibvorgänge/Sek.  |  Datenträgerschreibvorgänge/Sek.  |  Anzahl pro Sekunde  |  Average | 
| **Ja**  | Nein |  Microsoft.VMwareCloudSimple/virtualMachines  |  DiskReadBytesPerSecond  |  Disk Read Bytes/Sec  |  Bytes pro Sekunde  |  Average | 
| **Ja**  | Nein |  Microsoft.VMwareCloudSimple/virtualMachines  |  DiskReadLatency  |  Wartezeit beim Datenträgerlesevorgang  |  Millisekunden  |  Average | 
| **Ja**  | Nein |  Microsoft.VMwareCloudSimple/virtualMachines  |  DiskReadOperations  |  Datenträgerlesevorgänge  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.VMwareCloudSimple/virtualMachines  |  DiskWriteBytesPerSecond  |  Disk Write Bytes/Sec  |  Bytes pro Sekunde  |  Average | 
| **Ja**  | Nein |  Microsoft.VMwareCloudSimple/virtualMachines  |  DiskWriteLatency  |  Wartezeit beim Datenträgerschreibvorgang  |  Millisekunden  |  Average | 
| **Ja**  | Nein |  Microsoft.VMwareCloudSimple/virtualMachines  |  DiskWriteOperations  |  Datenträgerschreibvorgänge  |  Anzahl  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.VMwareCloudSimple/virtualMachines  |  MemoryActive  |  Aktiver Arbeitsspeicher  |  Byte  |  Average | 
| **Ja**  | Nein |  Microsoft.VMwareCloudSimple/virtualMachines  |  MemoryGranted  |  Zugewiesener Arbeitsspeicher  |  Byte  |  Average | 
| **Ja**  | Nein |  Microsoft.VMwareCloudSimple/virtualMachines  |  MemoryUsed  |  Verwendeter Arbeitsspeicher  |  Byte  |  Average | 
| **Ja**  | Nein |  Microsoft.VMwareCloudSimple/virtualMachines  |  Netzwerk eingehend  |  Netzwerk eingehend  |  Byte  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.VMwareCloudSimple/virtualMachines  |  Netzwerk ausgehend  |  Netzwerk ausgehend  |  Byte  |  Gesamt | 
| **Ja**  | Nein |  Microsoft.VMwareCloudSimple/virtualMachines  |  NetworkInBytesPerSecond  |  Eingehender Netzwerkverkehr in Bytes/Sek.  |  Bytes pro Sekunde  |  Average | 
| **Ja**  | Nein |  Microsoft.VMwareCloudSimple/virtualMachines  |  NetworkOutBytesPerSecond  |  Ausgehender Netzwerkverkehr in Bytes/Sek.  |  Bytes pro Sekunde  |  Average | 
| **Ja**  | Nein |  Microsoft.VMwareCloudSimple/virtualMachines  |  CPU in Prozent  |  CPU in Prozent  |  Percent  |  Average | 
| **Ja**  | Nein |  Microsoft.VMwareCloudSimple/virtualMachines  |  PercentageCpuReady  |  Prozentsatz der CPU-Bereitschaft  |  Millisekunden  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Web/hostingEnvironments/multiRolePools  |  ActiveRequests  |  Aktive Anforderungen  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Web/hostingEnvironments/multiRolePools  |  AverageResponseTime  |  Durchschnittliche Antwortzeit  |  Sekunden  |  Average | 
| **Ja**  | **Ja** |  Microsoft.Web/hostingEnvironments/multiRolePools  |  BytesReceived  |  Eingehende Daten  |  Byte  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Web/hostingEnvironments/multiRolePools  |  BytesSent  |  Datenausgabe  |  Byte  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Web/hostingEnvironments/multiRolePools  |  CpuPercentage  |  CPU-Prozentsatz  |  Percent  |  Average | 
| **Ja**  | **Ja** |  Microsoft.Web/hostingEnvironments/multiRolePools  |  DiskQueueLength  |  Warteschlangenlänge des Datenträgers  |  Anzahl  |  Average | 
| **Ja**  | **Ja** |  Microsoft.Web/hostingEnvironments/multiRolePools  |  Http101  |  HTTP 101  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Web/hostingEnvironments/multiRolePools  |  Http2xx  |  HTTP 2xx  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Web/hostingEnvironments/multiRolePools  |  Http3xx  |  HTTP 3xx  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Web/hostingEnvironments/multiRolePools  |  Http401  |  HTTP 401  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Web/hostingEnvironments/multiRolePools  |  Http403  |  HTTP 403  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Web/hostingEnvironments/multiRolePools  |  Http404  |  HTTP 404  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Web/hostingEnvironments/multiRolePools  |  Http406  |  HTTP 406  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Web/hostingEnvironments/multiRolePools  |  Http4xx  |  HTTP 4xx  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Web/hostingEnvironments/multiRolePools  |  Http5xx  |  HTTP-Serverfehler  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Web/hostingEnvironments/multiRolePools  |  HttpQueueLength  |  Länge der HTTP-Warteschlange  |  Anzahl  |  Average | 
| **Ja**  | **Ja** |  Microsoft.Web/hostingEnvironments/multiRolePools  |  LargeAppServicePlanInstances  |  Große Worker im App Service-Plan  |  Anzahl  |  Average | 
| **Ja**  | **Ja** |  Microsoft.Web/hostingEnvironments/multiRolePools  |  MediumAppServicePlanInstances  |  Mittlere Worker im App Service-Plan  |  Anzahl  |  Average | 
| **Ja**  | **Ja** |  Microsoft.Web/hostingEnvironments/multiRolePools  |  MemoryPercentage  |  Arbeitsspeicherprozentsatz  |  Percent  |  Average | 
| **Ja**  | **Ja** |  Microsoft.Web/hostingEnvironments/multiRolePools  |  Requests  |  Requests  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Web/hostingEnvironments/multiRolePools  |  SmallAppServicePlanInstances  |  Kleine Worker im App Service-Plan  |  Anzahl  |  Average | 
| **Ja**  | **Ja** |  Microsoft.Web/hostingEnvironments/multiRolePools  |  TotalFrontEnds  |  Gesamtanzahl an Front-Ends  |  Anzahl  |  Average | 
| **Ja**  | **Ja** |  Microsoft.Web/hostingEnvironments/workerPools  |  CpuPercentage  |  CPU-Prozentsatz  |  Percent  |  Average | 
| **Ja**  | **Ja** |  Microsoft.Web/hostingEnvironments/workerPools  |  MemoryPercentage  |  Arbeitsspeicherprozentsatz  |  Percent  |  Average | 
| **Ja**  | **Ja** |  Microsoft.Web/hostingEnvironments/workerPools  |  WorkersAvailable  |  Verfügbare Worker  |  Anzahl  |  Average | 
| **Ja**  | **Ja** |  Microsoft.Web/hostingEnvironments/workerPools  |  WorkersTotal  |  Gesamtanzahl an Workern  |  Anzahl  |  Average | 
| **Ja**  | **Ja** |  Microsoft.Web/hostingEnvironments/workerPools  |  WorkersUsed  |  Verwendete Worker  |  Anzahl  |  Average | 
| **Ja**  | **Ja** |  Microsoft.Web/serverfarms  |  BytesReceived  |  Eingehende Daten  |  Byte  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Web/serverfarms  |  BytesSent  |  Datenausgabe  |  Byte  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Web/serverfarms  |  CpuPercentage  |  CPU-Prozentsatz  |  Percent  |  Average | 
| **Ja**  | **Ja** |  Microsoft.Web/serverfarms  |  DiskQueueLength  |  Warteschlangenlänge des Datenträgers  |  Anzahl  |  Average | 
| **Ja**  | **Ja** |  Microsoft.Web/serverfarms  |  HttpQueueLength  |  Länge der HTTP-Warteschlange  |  Anzahl  |  Average | 
| **Ja**  | **Ja** |  Microsoft.Web/serverfarms  |  MemoryPercentage  |  Arbeitsspeicherprozentsatz  |  Percent  |  Average | 
| **Ja**  | **Ja** |  Microsoft.Web/serverfarms  |  TcpCloseWait  |  Warten auf Schließen der TCP-Verbindung  |  Anzahl  |  Average | 
| **Ja**  | **Ja** |  Microsoft.Web/serverfarms  |  TcpClosing  |  TCP-Verbindung wird geschlossen  |  Anzahl  |  Average | 
| **Ja**  | **Ja** |  Microsoft.Web/serverfarms  |  TcpEstablished  |  TCP-Verbindung hergestellt  |  Anzahl  |  Average | 
| **Ja**  | **Ja** |  Microsoft.Web/serverfarms  |  TcpFinWait1  |  Warten 1 auf Beendigung der TCP-Verbindung  |  Anzahl  |  Average | 
| **Ja**  | **Ja** |  Microsoft.Web/serverfarms  |  TcpFinWait2  |  Warten 2 auf Beendigung der TCP-Verbindung  |  Anzahl  |  Average | 
| **Ja**  | **Ja** |  Microsoft.Web/serverfarms  |  TcpLastAck  |  Letzte TCP-Bestätigung  |  Anzahl  |  Average | 
| **Ja**  | **Ja** |  Microsoft.Web/serverfarms  |  TcpSynReceived  |  TCP-Synchronisierung empfangen  |  Anzahl  |  Average | 
| **Ja**  | **Ja** |  Microsoft.Web/serverfarms  |  TcpSynSent  |  TCP-Synchronisierung gesendet  |  Anzahl  |  Average | 
| **Ja**  | **Ja** |  Microsoft.Web/serverfarms  |  TcpTimeWait  |  TCP-Wartezeit  |  Anzahl  |  Average | 
| **Ja**  | **Ja** |  Microsoft.Web/sites  |  AppConnections  |  Verbindungen  |  Anzahl  |  Average | 
| **Ja**  | **Ja** |  Microsoft.Web/sites  |  AverageMemoryWorkingSet  |  Durchschnittlicher Arbeitssatz für Arbeitsspeicher  |  Byte  |  Average | 
| **Ja**  | **Ja** |  Microsoft.Web/sites  |  AverageResponseTime  |  Durchschnittliche Antwortzeit  |  Sekunden  |  Average | 
| **Ja**  | **Ja** |  Microsoft.Web/sites  |  BytesReceived  |  Eingehende Daten  |  Byte  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Web/sites  |  BytesSent  |  Datenausgabe  |  Byte  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Web/sites  |  CpuTime  |  CPU-Zeit  |  Sekunden  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Web/sites  |  CurrentAssemblies  |  Aktuelle Assemblys  |  Anzahl  |  Average | 
| **Ja**  | **Ja** |  Microsoft.Web/sites  |  FunctionExecutionCount  |  Ausführungsanzahl für Funktion  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Web/sites  |  FunctionExecutionUnits  |  Ausführungseinheiten für Funktion  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Web/sites  |  Gen0Collections  |  Garbage Collections der Generation 0  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Web/sites  |  Gen1Collections  |  Garbage Collections der Generation 1  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Web/sites  |  Gen2Collections  |  Garbage Collections der Generation 2  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Web/sites  |  Ziehpunkte  |  Anzahl von Handles  |  Anzahl  |  Average | 
| **Ja**  | **Ja** |  Microsoft.Web/sites  |  HealthCheckStatus  |  Status der Integritätsüberprüfung  |  Anzahl  |  Average | 
| **Ja**  | **Ja** |  Microsoft.Web/sites  |  Http101  |  HTTP 101  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Web/sites  |  Http2xx  |  HTTP 2xx  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Web/sites  |  Http3xx  |  HTTP 3xx  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Web/sites  |  Http401  |  HTTP 401  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Web/sites  |  Http403  |  HTTP 403  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Web/sites  |  Http404  |  HTTP 404  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Web/sites  |  Http406  |  HTTP 406  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Web/sites  |  Http4xx  |  HTTP 4xx  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Web/sites  |  Http5xx  |  HTTP-Serverfehler  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Web/sites  |  HttpResponseTime  |  Antwortzeit  |  Sekunden  |  Average | 
| **Ja**  | **Ja** |  Microsoft.Web/sites  |  IoOtherBytesPerSecond  |  E/A: Andere Bytes pro Sekunde  |  Bytes pro Sekunde  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Web/sites  |  IoOtherOperationsPerSecond  |  E/A: Andere Vorgänge pro Sekunde  |  Bytes pro Sekunde  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Web/sites  |  IoReadBytesPerSecond  |  E/A: Gelesene Bytes pro Sekunde  |  Bytes pro Sekunde  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Web/sites  |  IoReadOperationsPerSecond  |  E/A: Lesevorgänge pro Sekunde  |  Bytes pro Sekunde  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Web/sites  |  IoWriteBytesPerSecond  |  E/A: Geschriebene Bytes pro Sekunde  |  Bytes pro Sekunde  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Web/sites  |  IoWriteOperationsPerSecond  |  E/A: Schreibvorgänge pro Sekunde  |  Bytes pro Sekunde  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Web/sites  |  MemoryWorkingSet  |  Arbeitssatz für Arbeitsspeicher  |  Byte  |  Average | 
| **Ja**  | **Ja** |  Microsoft.Web/sites  |  PrivateBytes  |  Private Bytes  |  Byte  |  Average | 
| **Ja**  | **Ja** |  Microsoft.Web/sites  |  Requests  |  Requests  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Web/sites  |  RequestsInApplicationQueue  |  Anforderungen in der Anwendungswarteschlange  |  Anzahl  |  Average | 
| **Ja**  | **Ja** |  Microsoft.Web/sites  |  Threads  |  Threadanzahl  |  Anzahl  |  Average | 
| **Ja**  | **Ja** |  Microsoft.Web/sites  |  TotalAppDomains  |  App-Domänen insgesamt  |  Anzahl  |  Average | 
| **Ja**  | **Ja** |  Microsoft.Web/sites  |  TotalAppDomainsUnloaded  |  Entladene App-Domänen insgesamt  |  Anzahl  |  Average | 
| **Ja**  | **Ja** |  Microsoft.Web/sites/slots  |  AppConnections  |  Verbindungen  |  Anzahl  |  Average | 
| **Ja**  | **Ja** |  Microsoft.Web/sites/slots  |  AverageMemoryWorkingSet  |  Durchschnittlicher Arbeitssatz für Arbeitsspeicher  |  Byte  |  Average | 
| **Ja**  | **Ja** |  Microsoft.Web/sites/slots  |  AverageResponseTime  |  Durchschnittliche Antwortzeit  |  Sekunden  |  Average | 
| **Ja**  | **Ja** |  Microsoft.Web/sites/slots  |  BytesReceived  |  Eingehende Daten  |  Byte  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Web/sites/slots  |  BytesSent  |  Datenausgabe  |  Byte  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Web/sites/slots  |  CpuTime  |  CPU-Zeit  |  Sekunden  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Web/sites/slots  |  CurrentAssemblies  |  Aktuelle Assemblys  |  Anzahl  |  Average | 
| **Ja**  | **Ja** |  Microsoft.Web/sites/slots  |  FunctionExecutionCount  |  Ausführungsanzahl für Funktion  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Web/sites/slots  |  FunctionExecutionUnits  |  Ausführungseinheiten für Funktion  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Web/sites/slots  |  Gen0Collections  |  Garbage Collections der Generation 0  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Web/sites/slots  |  Gen1Collections  |  Garbage Collections der Generation 1  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Web/sites/slots  |  Gen2Collections  |  Garbage Collections der Generation 2  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Web/sites/slots  |  Ziehpunkte  |  Anzahl von Handles  |  Anzahl  |  Average | 
| **Ja**  | **Ja** |  Microsoft.Web/sites/slots  |  HealthCheckStatus  |  Status der Integritätsüberprüfung  |  Anzahl  |  Average | 
| **Ja**  | **Ja** |  Microsoft.Web/sites/slots  |  Http101  |  HTTP 101  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Web/sites/slots  |  Http2xx  |  HTTP 2xx  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Web/sites/slots  |  Http3xx  |  HTTP 3xx  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Web/sites/slots  |  Http401  |  HTTP 401  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Web/sites/slots  |  Http403  |  HTTP 403  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Web/sites/slots  |  Http404  |  HTTP 404  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Web/sites/slots  |  Http406  |  HTTP 406  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Web/sites/slots  |  Http4xx  |  HTTP 4xx  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Web/sites/slots  |  Http5xx  |  HTTP-Serverfehler  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Web/sites/slots  |  HttpResponseTime  |  Antwortzeit  |  Sekunden  |  Average | 
| **Ja**  | **Ja** |  Microsoft.Web/sites/slots  |  IoOtherBytesPerSecond  |  E/A: Andere Bytes pro Sekunde  |  Bytes pro Sekunde  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Web/sites/slots  |  IoOtherOperationsPerSecond  |  E/A: Andere Vorgänge pro Sekunde  |  Bytes pro Sekunde  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Web/sites/slots  |  IoReadBytesPerSecond  |  E/A: Gelesene Bytes pro Sekunde  |  Bytes pro Sekunde  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Web/sites/slots  |  IoReadOperationsPerSecond  |  E/A: Lesevorgänge pro Sekunde  |  Bytes pro Sekunde  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Web/sites/slots  |  IoWriteBytesPerSecond  |  E/A: Geschriebene Bytes pro Sekunde  |  Bytes pro Sekunde  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Web/sites/slots  |  IoWriteOperationsPerSecond  |  E/A: Schreibvorgänge pro Sekunde  |  Bytes pro Sekunde  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Web/sites/slots  |  MemoryWorkingSet  |  Arbeitssatz für Arbeitsspeicher  |  Byte  |  Average | 
| **Ja**  | **Ja** |  Microsoft.Web/sites/slots  |  PrivateBytes  |  Private Bytes  |  Byte  |  Average | 
| **Ja**  | **Ja** |  Microsoft.Web/sites/slots  |  Requests  |  Requests  |  Anzahl  |  Gesamt | 
| **Ja**  | **Ja** |  Microsoft.Web/sites/slots  |  RequestsInApplicationQueue  |  Anforderungen in der Anwendungswarteschlange  |  Anzahl  |  Average | 
| **Ja**  | **Ja** |  Microsoft.Web/sites/slots  |  Threads  |  Threadanzahl  |  Anzahl  |  Average | 
| **Ja**  | **Ja** |  Microsoft.Web/sites/slots  |  TotalAppDomains  |  App-Domänen insgesamt  |  Anzahl  |  Average | 
| **Ja**  | **Ja** |  Microsoft.Web/sites/slots  |  TotalAppDomainsUnloaded  |  Entladene App-Domänen insgesamt  |  Anzahl  |  Average | 
