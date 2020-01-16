---
title: Von Azure Monitor unterstützte Metriken nach Ressourcentyp
description: Liste der Metriken, die mit Azure Monitor für jeden Ressourcentyp zur Verfügung stehen.
author: anirudhcavale
services: azure-monitor
ms.service: azure-monitor
ms.topic: reference
ms.date: 12/18/2019
ms.author: ancav
ms.subservice: metrics
ms.openlocfilehash: 67e2675dbf65daf929407a437447f5d977c7a6c3
ms.sourcegitcommit: 8e9a6972196c5a752e9a0d021b715ca3b20a928f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2020
ms.locfileid: "75750396"
---
# <a name="supported-metrics-with-azure-monitor"></a>Unterstützte Metriken von Azure Monitor

Azure Monitor bietet verschiedene Methoden für die Interaktion mit Metriken, z.B. die Diagrammdarstellung im Portal, den Zugriff über die REST-API oder die Abfrage über PowerShell oder CLI. Unten ist eine vollständige Liste aller Metriken aufgeführt, die derzeit mit der Metrikpipeline von Azure Monitor verfügbar sind. Weitere Metriken stehen möglicherweise im Portal oder über Legacy-APIs zur Verfügung. Die unten angegebene Liste enthält nur Metriken, die über die konsolidierte Azure Monitor-Metrikpipeline verfügbar sind. Verwenden Sie die [API-Version 2018-01-01](https://docs.microsoft.com/rest/api/monitor/metricdefinitions), um diese Metriken abzufragen und darauf zuzugreifen.

> [!NOTE]
> Das Senden mehrdimensionaler Metriken über die Diagnoseeinstellungen wird derzeit nicht unterstützt. Metriken mit Dimensionen werden als vereinfachte eindimensionale Metriken exportiert und dimensionswertübergreifend aggregiert.
>
> *Beispiel*: Die Metrik „Eingehende Nachrichten“ in einem Event Hub kann auf Warteschlangenebene untersucht und in einem Diagramm dargestellt werden. Wenn Sie die Metrik allerdings über die Diagnoseeinstellungen exportieren, umfasst die Darstellung alle eingehenden Nachrichten für alle Warteschlangen im Event Hub.
>
> Eine Liste der Plattformmetriken, die über Diagnoseeinstellungen exportiert werden können, finden Sie in [diesem Artikel](metrics-supported-export-diagnostic-settings.md).




## <a name="microsoftanalysisservicesservers"></a>Microsoft.AnalysisServices/servers

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|qpu_metric|QPU|Anzahl|Average|QPU. Bereich: 0–100 für S1, 0–200 für S2 und 0–400 für S4|ServerResourceType|
|memory_metric|Arbeitsspeicher|Byte|Average|Arbeitsspeicher. Bereich: 0–25 GB für S1, 0–50 GB für S2 und 0–100 GB für S4|ServerResourceType|
|private_bytes_metric|Private Bytes|Byte|Average|Private Bytes.|ServerResourceType|
|virtual_bytes_metric|Virtuelle Bytes|Byte|Average|Virtuelle Bytes.|ServerResourceType|
|TotalConnectionRequests|Total Connection Requests (Verbindungsanforderungen gesamt)|Anzahl|Average|Gesamtanzahl von Verbindungsanforderungen. Dies sind erhaltene Anforderungen.|ServerResourceType|
|SuccessfullConnectionsPerSec|Erfolgreiche Verbindungen pro Sekunde|Anzahl pro Sekunde|Average|Rate der erfolgreichen Verbindungsabschlüsse|ServerResourceType|
|TotalConnectionFailures|Verbindungsfehler gesamt|Anzahl|Average|Gesamtanzahl von fehlerhaften Verbindungsversuchen|ServerResourceType|
|CurrentUserSessions|Aktuelle Benutzersitzungen|Anzahl|Average|Aktuelle Anzahl von eingerichteten Benutzersitzungen|ServerResourceType|
|QueryPoolBusyThreads|Ausgelastete Abfragepoolthreads|Anzahl|Average|Anzahl von ausgelasteten Threads im Abfragethreadpool|ServerResourceType|
|CommandPoolJobQueueLength|Warteschlangenlänge für Aufträge im Befehlspool|Anzahl|Average|Gibt die Anzahl von Aufträgen in der Warteschlange des Befehlsthreadpools an.|ServerResourceType|
|ProcessingPoolJobQueueLength|Warteschlangenlänge für Verarbeitungspoolaufträge|Anzahl|Average|Anzahl von Nicht-E/A-Aufträgen in der Warteschlange des Verarbeitungsthreadpools|ServerResourceType|
|CurrentConnections|Verbindung: Aktuelle Verbindungen|Anzahl|Average|Aktuelle Anzahl hergestellter Clientverbindungen.|ServerResourceType|
|CleanerCurrentPrice|Memory: Bereinigung – aktueller Preis|Anzahl|Average|Aktueller Preis des Arbeitsspeichers, $/Byte/Zeit, normalisiert auf 1000.|ServerResourceType|
|CleanerMemoryShrinkable|Memory: Bereinigung – verkleinerbarer Arbeitsspeicher|Byte|Average|Die Menge des Arbeitsspeichers in Byte, die durch den Hintergrundbereinigungsprozess bereinigt wird.|ServerResourceType|
|CleanerMemoryNonshrinkable|Memory: Bereinigung – nicht verkleinerbarer Arbeitsspeicher|Byte|Average|Die Menge des Arbeitsspeichers in Byte, die nicht durch den Hintergrundbereinigungsprozess bereinigt wird.|ServerResourceType|
|MemoryUsage|Memory: Speicherauslastung|Byte|Average|Speicherauslastung des Serverprozesses, wie bei der Berechnung des Arbeitsspeicherpreises für die Bereinigung verwendet. Entspricht dem Indikator "Process\PrivateBytes" zuzüglich der Größe der im Speicher abgebildeten Daten. Von der xVelocity-Engine für Datenanalyse im Arbeitsspeicher (VertiPaq) abgebildeter oder belegter Arbeitsspeicher, der über die xVelocity-Arbeitsspeichergrenze hinausgeht, wird dabei ignoriert.|ServerResourceType|
|MemoryLimitHard|Memory: Grenzwert für den festen Speicher|Byte|Average|Grenzwert für den festen Speicher gemäß Konfigurationsdatei.|ServerResourceType|
|MemoryLimitHigh|Memory: Obere Arbeitsspeichergrenze|Byte|Average|Oberer Grenzwert für den Arbeitsspeicher gemäß Konfigurationsdatei.|ServerResourceType|
|MemoryLimitLow|Memory: Untere Arbeitsspeichergrenze|Byte|Average|Unterer Grenzwert für den Arbeitsspeicher gemäß Konfigurationsdatei.|ServerResourceType|
|MemoryLimitVertiPaq|Memory: VertiPaq-Arbeitsspeichergrenze|Byte|Average|In-Memory-Grenzwert gemäß Konfigurationsdatei.|ServerResourceType|
|Kontingent|Memory: Kontingent|Byte|Average|Aktuelles Arbeitsspeicherkontingent in Byte. Das Arbeitsspeicherkontingent wird auch als Speicherzuweisung oder Speicherreservierung bezeichnet.|ServerResourceType|
|QuotaBlocked|Memory: Kontingent blockiert|Anzahl|Average|Aktuelle Anzahl von Kontingentanforderungen, die blockiert werden, bis andere Arbeitsspeicherkontingente freigegeben werden.|ServerResourceType|
|VertiPaqNonpaged|Memory: Nicht ausgelagerte VertiPaq-Daten|Byte|Average|Bytes von Arbeitsspeicher, die im Arbeitssatz zur Verwendung durch die In-Memory-Engine gesperrt sind.|ServerResourceType|
|VertiPaqPaged|Memory: Ausgelagerte VertiPaq-Daten|Byte|Average|Bytes von ausgelagertem Arbeitsspeicher, die für In-Memory-Daten verwendet werden.|ServerResourceType|
|RowsReadPerSec|Verarbeitung: Gelesene Zeilen pro Sekunde|Anzahl pro Sekunde|Average|Rate der aus allen relationalen Datenbanken gelesenen Zeilen.|ServerResourceType|
|RowsConvertedPerSec|Verarbeitung: Konvertierte Zeilen pro Sekunde|Anzahl pro Sekunde|Average|Rate der Zeilen, die bei der Verarbeitung konvertiert werden.|ServerResourceType|
|RowsWrittenPerSec|Verarbeitung: Geschriebene Zeilen pro Sekunde|Anzahl pro Sekunde|Average|Rate der Zeilen, die bei der Verarbeitung geschrieben werden.|ServerResourceType|
|CommandPoolBusyThreads|Threads: Ausgelastete Threads im Befehlspool|Anzahl|Average|Anzahl ausgelasteter Threads im Befehlsthreadpool.|ServerResourceType|
|CommandPoolIdleThreads|Threads: Leerlaufthreads im Befehlspool|Anzahl|Average|Anzahl von Leerlaufthreads im Befehlsthreadpool.|ServerResourceType|
|LongParsingBusyThreads|Threads: Ausgelastete lange Analysethreads|Anzahl|Average|Anzahl ausgelasteter Threads im Pool für lange Analysethreads.|ServerResourceType|
|LongParsingIdleThreads|Threads: Lange Analysethreads im Leerlauf|Anzahl|Average|Anzahl von Leerlaufthreads im Pool für lange Analysethreads.|ServerResourceType|
|LongParsingJobQueueLength|Threads: Warteschlangenlänge für lange Analyseaufträge|Anzahl|Average|Anzahl von Aufträgen in der Warteschlange des Pools für lange Analysethreads.|ServerResourceType|
|ProcessingPoolBusyIOJobThreads|Threads: Ausgelastete Threads für E/A-Aufträge im Verarbeitungspool|Anzahl|Average|Anzahl von Threads, die E/A-Aufträge im Verarbeitungsthreadpool ausführen.|ServerResourceType|
|ProcessingPoolBusyNonIOThreads|Threads: Ausgelastete Nicht-E/A-Threads im Verarbeitungspool|Anzahl|Average|Anzahl von Threads, die Nicht-E/A-Aufträge im Verarbeitungsthreadpool ausführen.|ServerResourceType|
|ProcessingPoolIOJobQueueLength|Threads: Warteschlangenlänge für E/A-Aufträge im Verarbeitungspool|Anzahl|Average|Anzahl von E/A-Aufträgen in der Warteschlange des Verarbeitungsthreadpools.|ServerResourceType|
|ProcessingPoolIdleIOJobThreads|Threads: Leerlaufthreads für E/A-Aufträge im Verarbeitungspool|Anzahl|Average|Anzahl von Leerlaufthreads für E/A-Aufträge im Verarbeitungsthreadpool.|ServerResourceType|
|ProcessingPoolIdleNonIOThreads|Threads: Nicht-E/A-Leerlaufthreads im Verarbeitungspool|Anzahl|Average|Anzahl von Leerlaufthreads im Verarbeitungsthreadpool, die für Nicht-E/A-Aufträge vorgesehen sind.|ServerResourceType|
|QueryPoolIdleThreads|Threads: Abfragepoolthreads im Leerlauf|Anzahl|Average|Anzahl von Leerlaufthreads für E/A-Aufträge im Verarbeitungsthreadpool.|ServerResourceType|
|QueryPoolJobQueueLength|Threads: Auftragswarteschlangenlänge für Abfragepool|Anzahl|Average|Anzahl von Aufträgen in der Warteschlange des Abfragethreadpools.|ServerResourceType|
|ShortParsingBusyThreads|Threads: Ausgelastete kurze Analysethreads|Anzahl|Average|Anzahl ausgelasteter Threads im Pool für kurze Analysethreads.|ServerResourceType|
|ShortParsingIdleThreads|Threads: Kurze Analysethreads im Leerlauf|Anzahl|Average|Anzahl von Leerlaufthreads im Pool für kurze Analysethreads.|ServerResourceType|
|ShortParsingJobQueueLength|Threads: Warteschlangenlänge für kurze Analyseaufträge|Anzahl|Average|Anzahl von Aufträgen in der Warteschlange des Pools für kurze Analysethreads.|ServerResourceType|
|memory_thrashing_metric|Arbeitsspeicherüberlastung|Percent|Average|Durchschnittliche Arbeitsspeicherüberlastung.|ServerResourceType|
|mashup_engine_qpu_metric|M-Engine – QPU|Anzahl|Average|QPU-Nutzung durch Mashup-Engine-Prozesse|ServerResourceType|
|mashup_engine_memory_metric|M-Engine – Arbeitsspeicher|Byte|Average|Arbeitsspeichernutzung durch Mashup-Engine-Prozesse|ServerResourceType|
|mashup_engine_private_bytes_metric|Private Bytes für M-Engine|Byte|Average|Nutzung privater Bytes durch Prozesse der Mashup-Engine.|ServerResourceType|
|mashup_engine_virtual_bytes_metric|Virtuelle Bytes für M-Engine|Byte|Average|Nutzung virtueller Bytes durch Prozesse der Mashup-Engine.|ServerResourceType|


## <a name="microsoftapimanagementservice"></a>Microsoft.ApiManagement/service

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|TotalRequests|Gatewayanforderungen insgesamt (veraltet)|Anzahl|Gesamt|Anzahl von Gatewayanforderungen – verwenden Sie stattdessen die multidimensionale Anforderungsmetrik mit der GatewayResponseCodeCategory-Dimension.|Location, Hostname|
|SuccessfulRequests|Erfolgreiche Gatewayanforderungen (veraltet)|Anzahl|Gesamt|Anzahl von erfolgreichen Gatewayanforderungen – verwenden Sie stattdessen die multidimensionale Anforderungsmetrik mit der GatewayResponseCodeCategory-Dimension.|Location, Hostname|
|UnauthorizedRequests|Nicht autorisierte Gatewayanforderungen (veraltet)|Anzahl|Gesamt|Anzahl von nicht autorisierten Gatewayanforderungen – verwenden Sie stattdessen die multidimensionale Anforderungsmetrik mit der GatewayResponseCodeCategory-Dimension.|Location, Hostname|
|FailedRequests|Fehlerhafte Gatewayanforderungen (veraltet)|Anzahl|Gesamt|Anzahl von Fehlern bei Gatewayanforderungen – verwenden Sie stattdessen die multidimensionale Anforderungsmetrik mit der GatewayResponseCodeCategory-Dimension.|Location, Hostname|
|OtherRequests|Andere Gatewayanforderungen (veraltet)|Anzahl|Gesamt|Anzahl von anderen Gatewayanforderungen – verwenden Sie stattdessen die multidimensionale Anforderungsmetrik mit der GatewayResponseCodeCategory-Dimension.|Location, Hostname|
|Duration|Gesamtdauer von Gatewayanforderungen|Millisekunden|Average|Gesamtdauer von Gatewayanforderungen in Millisekunden|Location, Hostname|
|BackendDuration|Dauer von Back-End-Anforderungen|Millisekunden|Average|Dauer von Back-End-Anforderungen in Millisekunden|Location, Hostname|
|Capacity|Capacity|Percent|Average|Auslastungsmetrik für ApiManagement-Dienst|Location|
|EventHubTotalEvents|Gesamtzahl von EventHub-Ereignissen|Anzahl|Gesamt|Anzahl von an EventHub gesendeten Ereignissen|Location|
|EventHubSuccessfulEvents|Erfolgreiche EventHub-Ereignisse|Anzahl|Gesamt|Anzahl erfolgreicher EventHub-Ereignisse|Location|
|EventHubTotalFailedEvents|Fehlerhafte EventHub-Ereignisse|Anzahl|Gesamt|Anzahl fehlerhafter EventHub-Ereignisse|Location|
|EventHubRejectedEvents|Abgelehnte EventHub-Ereignisse|Anzahl|Gesamt|Anzahl abgelehnter EventHub-Ereignisse (falsche Konfiguration oder nicht autorisiert)|Location|
|EventHubThrottledEvents|Gedrosselte EventHub-Ereignisse|Anzahl|Gesamt|Anzahl gedrosselter EventHub-Ereignisse|Location|
|EventHubTimedoutEvents|EventHub-Ereignisse mit Zeitüberschreitung|Anzahl|Gesamt|Anzahl von EventHub-Ereignissen mit Zeitüberschreitung|Location|
|EventHubDroppedEvents|Gelöschte EventHub-Ereignisse|Anzahl|Gesamt|Anzahl übersprungener Ereignisse aufgrund Erreichen der maximalen Warteschlangengröße|Location|
|EventHubTotalBytesSent|Größe von EventHub-Ereignissen|Byte|Gesamt|Gesamtgröße von EventHub-Ereignissen in Byte|Location|
|Requests|Requests|Anzahl|Gesamt|Gatewayanforderungsmetriken mit mehreren Dimensionen|Location, Hostname, LastErrorReason, BackendResponseCode, GatewayResponseCode, BackendResponseCodeCategory, GatewayResponseCodeCategory|


## <a name="microsoftappconfigurationconfigurationstores"></a>Microsoft.AppConfiguration/configurationStores

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|HttpIncomingRequestCount|HttpIncomingRequestCount|Anzahl|Anzahl|Gesamtanzahl eingehender HTTP-Anforderungen|Keine|
|FailedHttpRequestCount|FailedHttpRequestCount|Anzahl|Anzahl|Fehlerhafte HTTP-Anforderungen|Keine|
|HttpIncomingRequestDuration|HttpIncomingRequestDuration|Anzahl|Average|Latenz bei einer HTTP-Anforderung|Keine|


## <a name="microsoftappplatformspring"></a>Microsoft.AppPlatform/Spring

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|SystemCpuUsagePercentage|Prozentualer Anteil der System-CPU-Auslastung|Percent|Average|Aktuelle CPU-Auslastung für das gesamte System|AppName, Pod|
|AppCpuUsagePercentage|Prozentualer Anteil der App-CPU-Auslastung|Percent|Average|Prozentualer Anteil der App-JVM-CPU-Auslastung|AppName, Pod|
|AppMemoryCommitted|Zugewiesener App-Arbeitsspeicher|Byte|Average|JVM zugewiesener Arbeitsspeicher in Byte|AppName, Pod|
|AppMemoryUsed|Verwendeter App-Arbeitsspeicher|Byte|Average|Verwendeter App-Arbeitsspeicher in Byte|AppName, Pod|
|AppMemoryMax|Maximaler App-Arbeitsspeicher|Byte|Maximum|Die maximale Speichermenge (in Byte), die für die Arbeitsspeicherverwaltung verwendet werden kann|AppName, Pod|
|MaxOldGenMemoryPoolBytes|Maximal verfügbare Datengröße der alten Generation|Byte|Average|Maximale Größe des Arbeitsspeicherpools der alten Generation|AppName, Pod|
|OldGenMemoryPoolBytes|Datengröße der alten Generation|Byte|Average|Größe des Arbeitsspeicherpools der alten Generation nach einer vollständigen Garbage Collection|AppName, Pod|
|OldGenPromotedBytes|Hochstufung auf die Datengröße der alten Generation|Byte|Maximum|Anzahl der positiven Erhöhungen der Größe des Speicherpools der alten Generation vor der Garbage Collection gegenüber nach der GC|AppName, Pod|
|YoungGenPromotedBytes|Hochstufung auf die Datengröße der neuen Generation|Byte|Maximum|Inkrementiert zur Erhöhung der Größe des Speicherpools der neuen Generation nach einer Garbage Collection gegenüber dem Zeitpunkt vor der nächsten|AppName, Pod|
|GCPauseTotalCount|Anzahl der GC-Pausen|Anzahl|Gesamt|Anzahl der GC-Pausen|AppName, Pod|
|GCPauseTotalTime|Gesamtzeit der GC-Pausen|Millisekunden|Gesamt|Gesamtzeit der GC-Pausen|AppName, Pod|
|TomcatSentBytes|Insgesamt gesendete Bytes für Tomcat|Byte|Gesamt|Insgesamt gesendete Bytes für Tomcat|AppName, Pod|
|TomcatReceivedBytes|Insgesamt empfangene Bytes für Tomcat|Byte|Gesamt|Insgesamt empfangene Bytes für Tomcat|AppName, Pod|
|TomcatRequestTotalTime|Gesamtzeit für Tomcat-Anforderungen|Millisekunden|Gesamt|Gesamtzeit für Tomcat-Anforderungen|AppName, Pod|
|TomcatRequestTotalCount|Gesamtanzahl von Tomcat-Anforderungen|Anzahl|Gesamt|Gesamtanzahl von Tomcat-Anforderungen|AppName, Pod|
|TomcatResponseAvgTime|Durchschnittliche Zeit für Tomcat-Anforderungen|Millisekunden|Average|Durchschnittliche Zeit für Tomcat-Anforderungen|AppName, Pod|
|TomcatRequestMaxTime|Maximale Zeit für Tomcat-Anforderung|Millisekunden|Maximum|Maximale Zeit für Tomcat-Anforderung|AppName, Pod|
|TomcatErrorCount|Globale Tomcat-Fehler|Anzahl|Gesamt|Globale Tomcat-Fehler|AppName, Pod|
|TomcatSessionActiveMaxCount|Maximale Anzahl aktiver Tomcat-Sitzungen|Anzahl|Gesamt|Maximale Anzahl aktiver Tomcat-Sitzungen|AppName, Pod|
|TomcatSessionAliveMaxTime|Maximale aktive Zeit für Tomcat-Sitzungen|Millisekunden|Maximum|Maximale aktive Zeit für Tomcat-Sitzungen|AppName, Pod|
|TomcatSessionActiveCurrentCount|Anzahl aktiver Tomcat-Sitzungen|Anzahl|Gesamt|Anzahl aktiver Tomcat-Sitzungen|AppName, Pod|
|TomcatSessionCreatedCount|Anzahl erstellter Tomcat-Sitzungen|Anzahl|Gesamt|Anzahl erstellter Tomcat-Sitzungen|AppName, Pod|
|TomcatSessionExpiredCount|Anzahl abgelaufener Tomcat-Sitzungen|Anzahl|Gesamt|Anzahl abgelaufener Tomcat-Sitzungen|AppName, Pod|
|TomcatSessionRejectedCount|Anzahl abgewiesener Tomcat-Sitzungen|Anzahl|Gesamt|Anzahl abgewiesener Tomcat-Sitzungen|AppName, Pod|


## <a name="microsoftautomationautomationaccounts"></a>Microsoft.Automation/automationAccounts

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|TotalJob|Gesamtzahl an Einzelvorgängen (Jobs)|Anzahl|Gesamt|Die Gesamtzahl an Einzelvorgängen (Jobs)|Runbook, Status|
|TotalUpdateDeploymentRuns|Gesamtzahl von Updatebereitstellungsausführungen|Anzahl|Gesamt|Gesamtzahl von Updatebereitstellungsausführungen für Software|SoftwareUpdateConfigurationName,Status|
|TotalUpdateDeploymentMachineRuns|Gesamtzahl von Updatebereitstellungsausführungen auf dem Computer|Anzahl|Gesamt|Gesamtzahl von Updatebereitstellungsausführungen für Software auf dem Computer in einer Updatebereitstellungsausführung für Software|SoftwareUpdateConfigurationName,Status,TargetComputer,SoftwareUpdateConfigurationRunId|


## <a name="microsoftbatchbatchaccounts"></a>Microsoft.Batch/batchAccounts

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|CoreCount|Dedizierte Anzahl von Kernen|Anzahl|Gesamt|Gesamtzahl der dedizierten Kerne im Batch-Konto|Keine|
|TotalNodeCount|Dedizierte Knotenanzahl|Anzahl|Gesamt|Gesamtzahl der dedizierten Knoten im Batch-Konto|Keine|
|LowPriorityCoreCount|Anzahl von LowPriority-Kernen|Anzahl|Gesamt|Gesamtzahl der Kerne mit niedriger Priorität im Batch-Konto|Keine|
|TotalLowPriorityNodeCount|Anzahl der Knoten mit niedriger Priorität|Anzahl|Gesamt|Gesamtzahl der Knoten mit niedriger Priorität im Batchkonto|Keine|
|CreatingNodeCount|Anzahl erstellter Knoten|Anzahl|Gesamt|Anzahl von Knoten, die gerade erstellt werden|Keine|
|StartingNodeCount|Anzahl gestarteter Knoten|Anzahl|Gesamt|Anzahl von Knoten, die gerade gestartet werden|Keine|
|WaitingForStartTaskNodeCount|Anzahl von Knoten, die auf den Starttask warten|Anzahl|Gesamt|Anzahl von Knoten, die auf den Abschluss des Starttasks warten|Keine|
|StartTaskFailedNodeCount|Anzahl von Knoten mit Starttaskfehlern|Anzahl|Gesamt|Anzahl von Knoten, bei denen der Starttask nicht durchgeführt werden konnte|Keine|
|IdleNodeCount|Anzahl von Knoten im Leerlauf|Anzahl|Gesamt|Anzahl von Knoten im Leerlauf|Keine|
|OfflineNodeCount|Anzahl von Offlineknoten|Anzahl|Gesamt|Anzahl offline geschalteter Knoten|Keine|
|RebootingNodeCount|Anzahl neu gestarteter Knoten|Anzahl|Gesamt|Anzahl von Knoten, die neu gestartet werden|Keine|
|ReimagingNodeCount|Anzahl von Knoten mit Reimaging|Anzahl|Gesamt|Anzahl von Knoten, für die ein Reimaging durchgeführt wird|Keine|
|RunningNodeCount|Anzahl ausgeführter Knoten|Anzahl|Gesamt|Anzahl ausgeführter Knoten|Keine|
|LeavingPoolNodeCount|Anzahl von Knoten, die den Pool verlassen|Anzahl|Gesamt|Anzahl von Knoten, die den Pool verlassen|Keine|
|UnusableNodeCount|Anzahl nicht verwendbarer Knoten|Anzahl|Gesamt|Anzahl nicht verwendbarer Knoten|Keine|
|PreemptedNodeCount|Anzahl der vorzeitig entfernten Knoten|Anzahl|Gesamt|Die Anzahl der vorzeitig entfernten Knoten|Keine|
|TaskStartEvent|Taskstartereignisse|Anzahl|Gesamt|Gesamtanzahl gestarteter Tasks|Keine|
|TaskCompleteEvent|Taskabschlussereignisse|Anzahl|Gesamt|Gesamtanzahl abgeschlossener Tasks|Keine|
|TaskFailEvent|Taskfehlerereignisse|Anzahl|Gesamt|Gesamtanzahl von Tasks, die mit Fehlerstatus abgeschlossen wurden|Keine|
|PoolCreateEvent|Poolerstellungsereignisse|Anzahl|Gesamt|Gesamtanzahl erstellter Pools|Keine|
|PoolResizeStartEvent|Ereignisse zum Start der Größenänderung von Pools|Anzahl|Gesamt|Gesamtanzahl gestarteter Größenänderungen von Pools|Keine|
|PoolResizeCompleteEvent|Ereignisse zum Abschluss der Größenänderung von Pools|Anzahl|Gesamt|Gesamtanzahl abgeschlossener Größenänderungen von Pools|Keine|
|PoolDeleteStartEvent|Ereignisse zum Starten des Löschvorgangs von Pools|Anzahl|Gesamt|Gesamtanzahl gestarteter Poollöschvorgänge|Keine|
|PoolDeleteCompleteEvent|Ereignisse zum Abschluss des Löschvorgangs von Pools|Anzahl|Gesamt|Gesamtanzahl abgeschlossener Poollöschvorgänge|Keine|
|JobDeleteCompleteEvent|Ereignisse zum Abschluss des Löschvorgangs von Aufträgen|Anzahl|Gesamt|Gesamtanzahl der erfolgreich gelöschten Aufträge|Keine|
|JobDeleteStartEvent|Ereignisse zum Starten des Löschvorgangs von Aufträgen|Anzahl|Gesamt|Gesamtanzahl der zum Löschen angeforderten Aufträge|Keine|
|JobDisableCompleteEvent|Ereignisse zum Abschluss der Deaktivierung von Aufträgen|Anzahl|Gesamt|Gesamtanzahl der erfolgreich deaktivierten Aufträge|Keine|
|JobDisableStartEvent|Ereignisse zum Starten der Deaktivierung von Aufträgen|Anzahl|Gesamt|Gesamtanzahl der zum Deaktivieren angeforderten Aufträge|Keine|
|JobStartEvent|Ereignisse zum Starten von Aufträgen|Anzahl|Gesamt|Gesamtanzahl der erfolgreich gestarteten Aufträge|Keine|
|JobTerminateCompleteEvent|Ereignisse zum Abschluss der Beendigung von Aufträgen|Anzahl|Gesamt|Gesamtanzahl der erfolgreich beendeten Aufträge|Keine|
|JobTerminateStartEvent|Ereignisse zum Starten der Beendigung von Aufträgen|Anzahl|Gesamt|Gesamtanzahl der zum Beenden angeforderten Aufträge|Keine|


## <a name="microsoftbatchaiworkspaces"></a>Microsoft.BatchAI/workspaces

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|Auftrag übermittelt|Auftrag übermittelt|Anzahl|Gesamt|Anzahl der übermittelten Aufträge|Scenario, ClusterName|
|Auftrag abgeschlossen|Auftrag abgeschlossen|Anzahl|Gesamt|Anzahl der abgeschlossenen Aufträge|Scenario, ClusterName, ResultType|
|Knoten insgesamt|Knoten insgesamt|Anzahl|Average|Gesamtanzahl von Knoten|Scenario, ClusterName|
|Aktive Knoten|Aktive Knoten|Anzahl|Average|Anzahl ausgeführter Knoten|Scenario, ClusterName|
|Knoten im Leerlauf|Knoten im Leerlauf|Anzahl|Average|Anzahl von Knoten im Leerlauf|Scenario, ClusterName|
|Unusable Nodes (Nicht verwendbare Knoten)|Unusable Nodes (Nicht verwendbare Knoten)|Anzahl|Average|Anzahl nicht verwendbarer Knoten|Scenario, ClusterName|
|Vorzeitig entfernte Knoten|Vorzeitig entfernte Knoten|Anzahl|Average|Die Anzahl der vorzeitig entfernten Knoten|Scenario, ClusterName|
|Ausscheidende Knoten|Ausscheidende Knoten|Anzahl|Average|Anzahl der ausscheidenden Knoten|Scenario, ClusterName|
|Kerne insgesamt|Kerne insgesamt|Anzahl|Average|Gesamtanzahl von Kernen|Scenario, ClusterName|
|Aktive Kerne|Aktive Kerne|Anzahl|Average|Gesamtanzahl der aktiven Kerne|Scenario, ClusterName|
|Kerne im Leerlauf|Kerne im Leerlauf|Anzahl|Average|Gesamtanzahl der Kerne im Leerlauf|Scenario, ClusterName|
|Nicht verwendbare Kerne|Nicht verwendbare Kerne|Anzahl|Average|Anzahl der nicht verwendbaren Kerne|Scenario, ClusterName|
|Vorzeitig entfernte Kerne|Vorzeitig entfernte Kerne|Anzahl|Average|Anzahl der vorzeitig entfernten Kerne|Scenario, ClusterName|
|Ausscheidende Kerne|Ausscheidende Kerne|Anzahl|Average|Anzahl der ausscheidenden Kerne|Scenario, ClusterName|
|Quota Utilization Percentage (Prozentsatz der Kontingentnutzung)|Quota Utilization Percentage (Prozentsatz der Kontingentnutzung)|Anzahl|Average|Prozentualer Anteil der genutzten Kontingente|Scenario, ClusterName, VmFamilyName, VmPriority|

## <a name="microsoftblockchainblockchainmembers"></a>Microsoft.Blockchain/blockchainMembers

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|CpuUsagePercentageInDouble|Prozentuale CPU-Auslastung|Percent|Maximum|Prozentuale CPU-Auslastung|Node|
|MemoryUsage|Speicherauslastung|Byte|Average|Speicherauslastung|Node|
|MemoryLimit|Arbeitsspeicherlimit|Byte|Average|Arbeitsspeicherlimit|Node|
|MemoryUsagePercentageInDouble|Prozentuale Arbeitsspeicherauslastung|Percent|Average|Prozentuale Arbeitsspeicherauslastung|Node|
|StorageUsage|Speicherauslastung|Byte|Average|Speicherauslastung|Node|
|IOReadBytes|Gelesene E/A-Bytes|Byte|Gesamt|Gelesene E/A-Bytes|Node|
|IOWriteBytes|Geschriebene E/A-Bytes|Byte|Gesamt|Geschriebene E/A-Bytes|Node|
|ConnectionAccepted|Akzeptierte Verbindungen|Anzahl|Gesamt|Akzeptierte Verbindungen|Node|
|ConnectionHandled|Behandelte Verbindungen|Anzahl|Gesamt|Behandelte Verbindungen|Node|
|ConnectionActive|Die aktiven Verbindungen.|Anzahl|Average|Die aktiven Verbindungen.|Node|
|RequestHandled|Behandelte Anforderungen|Anzahl|Gesamt|Behandelte Anforderungen|Node|
|ProcessedBlocks|Verarbeitete Blöcke|Anzahl|Gesamt|Verarbeitete Blöcke|Node|
|ProcessedTransactions|Verarbeitete Transaktionen|Anzahl|Gesamt|Verarbeitete Transaktionen|Node|
|PendingTransactions|Ausstehende Transaktionen|Anzahl|Average|Ausstehende Transaktionen|Node|
|QueuedTransactions|Transaktionen in Warteschlange|Anzahl|Average|Transaktionen in Warteschlange|Node|



## <a name="microsoftcacheredis"></a>Microsoft.Cache/redis

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|connectedclients|Verbundene Clients|Anzahl|Maximum||ShardId|
|totalcommandsprocessed|Vorgänge gesamt|Anzahl|Gesamt||ShardId|
|cachehits|Cachetreffer|Anzahl|Gesamt||ShardId|
|cachemisses|Cachefehler|Anzahl|Gesamt||ShardId|
|cachemissrate|Cachefehlerrate|Percent|cachemissrate||ShardId|
|getcommands|get-Vorgänge|Anzahl|Gesamt||ShardId|
|setcommands|Sets|Anzahl|Gesamt||ShardId|
|operationsPerSecond|Vorgänge pro Sekunde|Anzahl|Maximum||ShardId|
|evictedkeys|Entfernte Schlüssel|Anzahl|Gesamt||ShardId|
|totalkeys|Schlüssel insgesamt|Anzahl|Maximum||ShardId|
|expiredkeys|Abgelaufene Schlüssel|Anzahl|Gesamt||ShardId|
|usedmemory|Verwendeter Arbeitsspeicher|Byte|Maximum||ShardId|
|usedmemorypercentage|Prozentsatz der Arbeitsspeicherverwendung|Percent|Maximum||ShardId|
|usedmemoryRss|Verwendeter Arbeitsspeicher (RSS)|Byte|Maximum||ShardId|
|serverLoad|Serverlast|Percent|Maximum||ShardId|
|cacheWrite|Cache-Schreibvorgänge|Bytes pro Sekunde|Maximum||ShardId|
|cacheRead|Cache-Lesevorgänge|Bytes pro Sekunde|Maximum||ShardId|
|percentProcessorTime|CPU|Percent|Maximum||ShardId|
|cacheLatency|Cachewartezeit in Mikrosekunden (Vorschau)|Anzahl|Average||ShardId|
|errors|Errors|Anzahl|Maximum||ShardId,ErrorType|
|connectedclients0|Verbundene Clients (Shard 0)|Anzahl|Maximum||Keine|
|totalcommandsprocessed0|Vorgänge gesamt (Shard 0)|Anzahl|Gesamt||Keine|
|cachehits0|Cachetreffer (Shard 0)|Anzahl|Gesamt||Keine|
|cachemisses0|Cachefehler (Shard 0)|Anzahl|Gesamt||Keine|
|getcommands0|Get-Vorgänge (Shard 0)|Anzahl|Gesamt||Keine|
|setcommands0|Set-Vorgänge (Shard 0)|Anzahl|Gesamt||Keine|
|operationsPerSecond0|Vorgänge pro Sekunde (Shard 0)|Anzahl|Maximum||Keine|
|evictedkeys0|Entfernte Schlüssel (Shard 0)|Anzahl|Gesamt||Keine|
|totalkeys0|Schlüssel gesamt (Shard 0)|Anzahl|Maximum||Keine|
|expiredkeys0|Abgelaufene Schlüssel (Shard 0)|Anzahl|Gesamt||Keine|
|usedmemory0|Verwendeter Arbeitsspeicher (Shard 0)|Byte|Maximum||Keine|
|usedmemoryRss0|Verwendeter Arbeitsspeicher (RSS, Shard 0)|Byte|Maximum||Keine|
|serverLoad0|Serverauslastung (Shard 0)|Percent|Maximum||Keine|
|cacheWrite0|Cacheschreibvorgänge (Shard 0)|Bytes pro Sekunde|Maximum||Keine|
|cacheRead0|Cachelesevorgänge (Shard 0)|Bytes pro Sekunde|Maximum||Keine|
|percentProcessorTime0|CPU (Shard 0)|Percent|Maximum||Keine|
|connectedclients1|Verbundene Clients (Shard 1)|Anzahl|Maximum||Keine|
|totalcommandsprocessed1|Vorgänge gesamt (Shard 1)|Anzahl|Gesamt||Keine|
|cachehits1|Cachetreffer (Shard 1)|Anzahl|Gesamt||Keine|
|cachemisses1|Cachefehler (Shard 1)|Anzahl|Gesamt||Keine|
|getcommands1|Get-Vorgänge (Shard 1)|Anzahl|Gesamt||Keine|
|setcommands1|Set-Vorgänge (Shard 1)|Anzahl|Gesamt||Keine|
|operationsPerSecond1|Vorgänge pro Sekunde (Shard 1)|Anzahl|Maximum||Keine|
|evictedkeys1|Entfernte Schlüssel (Shard 1)|Anzahl|Gesamt||Keine|
|totalkeys1|Schlüssel gesamt (Shard 1)|Anzahl|Maximum||Keine|
|expiredkeys1|Abgelaufene Schlüssel (Shard 1)|Anzahl|Gesamt||Keine|
|usedmemory1|Verwendeter Arbeitsspeicher (Shard 1)|Byte|Maximum||Keine|
|usedmemoryRss1|Verwendeter Arbeitsspeicher (RSS, Shard 1)|Byte|Maximum||Keine|
|serverLoad1|Serverauslastung (Shard 1)|Percent|Maximum||Keine|
|cacheWrite1|Cacheschreibvorgänge (Shard 1)|Bytes pro Sekunde|Maximum||Keine|
|cacheRead1|Cachelesevorgänge (Shard 1)|Bytes pro Sekunde|Maximum||Keine|
|percentProcessorTime1|CPU (Shard 1)|Percent|Maximum||Keine|
|connectedclients2|Verbundene Clients (Shard 2)|Anzahl|Maximum||Keine|
|totalcommandsprocessed2|Vorgänge gesamt (Shard 2)|Anzahl|Gesamt||Keine|
|cachehits2|Cachetreffer (Shard 2)|Anzahl|Gesamt||Keine|
|cachemisses2|Cachefehler (Shard 2)|Anzahl|Gesamt||Keine|
|getcommands2|Get-Vorgänge (Shard 2)|Anzahl|Gesamt||Keine|
|setcommands2|Set-Vorgänge (Shard 2)|Anzahl|Gesamt||Keine|
|operationsPerSecond2|Vorgänge pro Sekunde (Shard 2)|Anzahl|Maximum||Keine|
|evictedkeys2|Entfernte Schlüssel (Shard 2)|Anzahl|Gesamt||Keine|
|totalkeys2|Schlüssel gesamt (Shard 2)|Anzahl|Maximum||Keine|
|expiredkeys2|Abgelaufene Schlüssel (Shard 2)|Anzahl|Gesamt||Keine|
|usedmemory2|Verwendeter Arbeitsspeicher (Shard 2)|Byte|Maximum||Keine|
|usedmemoryRss2|Verwendeter Arbeitsspeicher (RSS, Shard 2)|Byte|Maximum||Keine|
|serverLoad2|Serverauslastung (Shard 2)|Percent|Maximum||Keine|
|cacheWrite2|Cacheschreibvorgänge (Shard 2)|Bytes pro Sekunde|Maximum||Keine|
|cacheRead2|Cachelesevorgänge (Shard 2)|Bytes pro Sekunde|Maximum||Keine|
|percentProcessorTime2|CPU (Shard 2)|Percent|Maximum||Keine|
|connectedclients3|Verbundene Clients (Shard 3)|Anzahl|Maximum||Keine|
|totalcommandsprocessed3|Vorgänge gesamt (Shard 3)|Anzahl|Gesamt||Keine|
|cachehits3|Cachetreffer (Shard 3)|Anzahl|Gesamt||Keine|
|cachemisses3|Cachefehler (Shard 3)|Anzahl|Gesamt||Keine|
|getcommands3|Get-Vorgänge (Shard 3)|Anzahl|Gesamt||Keine|
|setcommands3|Set-Vorgänge (Shard 3)|Anzahl|Gesamt||Keine|
|operationsPerSecond3|Vorgänge pro Sekunde (Shard 3)|Anzahl|Maximum||Keine|
|evictedkeys3|Entfernte Schlüssel (Shard 3)|Anzahl|Gesamt||Keine|
|totalkeys3|Schlüssel gesamt (Shard 3)|Anzahl|Maximum||Keine|
|expiredkeys3|Abgelaufene Schlüssel (Shard 3)|Anzahl|Gesamt||Keine|
|usedmemory3|Verwendeter Arbeitsspeicher (Shard 3)|Byte|Maximum||Keine|
|usedmemoryRss3|Verwendeter Arbeitsspeicher (RSS, Shard 3)|Byte|Maximum||Keine|
|serverLoad3|Serverauslastung (Shard 3)|Percent|Maximum||Keine|
|cacheWrite3|Cacheschreibvorgänge (Shard 3)|Bytes pro Sekunde|Maximum||Keine|
|cacheRead3|Cachelesevorgänge (Shard 3)|Bytes pro Sekunde|Maximum||Keine|
|percentProcessorTime3|CPU (Shard 3)|Percent|Maximum||Keine|
|connectedclients4|Verbundene Clients (Shard 4)|Anzahl|Maximum||Keine|
|totalcommandsprocessed4|Vorgänge gesamt (Shard 4)|Anzahl|Gesamt||Keine|
|cachehits4|Cachetreffer (Shard 4)|Anzahl|Gesamt||Keine|
|cachemisses4|Cachefehler (Shard 4)|Anzahl|Gesamt||Keine|
|getcommands4|Get-Vorgänge (Shard 4)|Anzahl|Gesamt||Keine|
|setcommands4|Set-Vorgänge (Shard 4)|Anzahl|Gesamt||Keine|
|operationsPerSecond4|Vorgänge pro Sekunde (Shard 4)|Anzahl|Maximum||Keine|
|evictedkeys4|Entfernte Schlüssel (Shard 4)|Anzahl|Gesamt||Keine|
|totalkeys4|Schlüssel gesamt (Shard 4)|Anzahl|Maximum||Keine|
|expiredkeys4|Abgelaufene Schlüssel (Shard 4)|Anzahl|Gesamt||Keine|
|usedmemory4|Verwendeter Arbeitsspeicher (Shard 4)|Byte|Maximum||Keine|
|usedmemoryRss4|Verwendeter Arbeitsspeicher (RSS, Shard 4)|Byte|Maximum||Keine|
|serverLoad4|Serverauslastung (Shard 4)|Percent|Maximum||Keine|
|cacheWrite4|Cacheschreibvorgänge (Shard 4)|Bytes pro Sekunde|Maximum||Keine|
|cacheRead4|Cachelesevorgänge (Shard 4)|Bytes pro Sekunde|Maximum||Keine|
|percentProcessorTime4|CPU (Shard 4)|Percent|Maximum||Keine|
|connectedclients5|Verbundene Clients (Shard 5)|Anzahl|Maximum||Keine|
|totalcommandsprocessed5|Vorgänge gesamt (Shard 5)|Anzahl|Gesamt||Keine|
|cachehits5|Cachetreffer (Shard 5)|Anzahl|Gesamt||Keine|
|cachemisses5|Cachefehler (Shard 5)|Anzahl|Gesamt||Keine|
|getcommands5|Get-Vorgänge (Shard 5)|Anzahl|Gesamt||Keine|
|setcommands5|Set-Vorgänge (Shard 5)|Anzahl|Gesamt||Keine|
|operationsPerSecond5|Vorgänge pro Sekunde (Shard 5)|Anzahl|Maximum||Keine|
|evictedkeys5|Entfernte Schlüssel (Shard 5)|Anzahl|Gesamt||Keine|
|totalkeys5|Schlüssel gesamt (Shard 5)|Anzahl|Maximum||Keine|
|expiredkeys5|Abgelaufene Schlüssel (Shard 5)|Anzahl|Gesamt||Keine|
|usedmemory5|Verwendeter Arbeitsspeicher (Shard 5)|Byte|Maximum||Keine|
|usedmemoryRss5|Verwendeter Arbeitsspeicher (RSS, Shard 5)|Byte|Maximum||Keine|
|serverLoad5|Serverauslastung (Shard 5)|Percent|Maximum||Keine|
|cacheWrite5|Cacheschreibvorgänge (Shard 5)|Bytes pro Sekunde|Maximum||Keine|
|cacheRead5|Cachelesevorgänge (Shard 5)|Bytes pro Sekunde|Maximum||Keine|
|percentProcessorTime5|CPU (Shard 5)|Percent|Maximum||Keine|
|connectedclients6|Verbundene Clients (Shard 6)|Anzahl|Maximum||Keine|
|totalcommandsprocessed6|Vorgänge gesamt (Shard 6)|Anzahl|Gesamt||Keine|
|cachehits6|Cachetreffer (Shard 6)|Anzahl|Gesamt||Keine|
|cachemisses6|Cachefehler (Shard 6)|Anzahl|Gesamt||Keine|
|getcommands6|Get-Vorgänge (Shard 6)|Anzahl|Gesamt||Keine|
|setcommands6|Set-Vorgänge (Shard 6)|Anzahl|Gesamt||Keine|
|operationsPerSecond6|Vorgänge pro Sekunde (Shard 6)|Anzahl|Maximum||Keine|
|evictedkeys6|Entfernte Schlüssel (Shard 6)|Anzahl|Gesamt||Keine|
|totalkeys6|Schlüssel gesamt (Shard 6)|Anzahl|Maximum||Keine|
|expiredkeys6|Abgelaufene Schlüssel (Shard 6)|Anzahl|Gesamt||Keine|
|usedmemory6|Verwendeter Arbeitsspeicher (Shard 6)|Byte|Maximum||Keine|
|usedmemoryRss6|Verwendeter Arbeitsspeicher (RSS, Shard 6)|Byte|Maximum||Keine|
|serverLoad6|Serverauslastung (Shard 6)|Percent|Maximum||Keine|
|cacheWrite6|Cacheschreibvorgänge (Shard 6)|Bytes pro Sekunde|Maximum||Keine|
|cacheRead6|Cachelesevorgänge (Shard 6)|Bytes pro Sekunde|Maximum||Keine|
|percentProcessorTime6|CPU (Shard 6)|Percent|Maximum||Keine|
|connectedclients7|Verbundene Clients (Shard 7)|Anzahl|Maximum||Keine|
|totalcommandsprocessed7|Vorgänge gesamt (Shard 7)|Anzahl|Gesamt||Keine|
|cachehits7|Cachetreffer (Shard 7)|Anzahl|Gesamt||Keine|
|cachemisses7|Cachefehler (Shard 7)|Anzahl|Gesamt||Keine|
|getcommands7|Get-Vorgänge (Shard 7)|Anzahl|Gesamt||Keine|
|setcommands7|Set-Vorgänge (Shard 7)|Anzahl|Gesamt||Keine|
|operationsPerSecond7|Vorgänge pro Sekunde (Shard 7)|Anzahl|Maximum||Keine|
|evictedkeys7|Entfernte Schlüssel (Shard 7)|Anzahl|Gesamt||Keine|
|totalkeys7|Schlüssel gesamt (Shard 7)|Anzahl|Maximum||Keine|
|expiredkeys7|Abgelaufene Schlüssel (Shard 7)|Anzahl|Gesamt||Keine|
|usedmemory7|Verwendeter Arbeitsspeicher (Shard 7)|Byte|Maximum||Keine|
|usedmemoryRss7|Verwendeter Arbeitsspeicher (RSS, Shard 7)|Byte|Maximum||Keine|
|serverLoad7|Serverauslastung (Shard 7)|Percent|Maximum||Keine|
|cacheWrite7|Cacheschreibvorgänge (Shard 7)|Bytes pro Sekunde|Maximum||Keine|
|cacheRead7|Cachelesevorgänge (Shard 7)|Bytes pro Sekunde|Maximum||Keine|
|percentProcessorTime7|CPU (Shard 7)|Percent|Maximum||Keine|
|connectedclients8|Verbundene Clients (Shard 8)|Anzahl|Maximum||Keine|
|totalcommandsprocessed8|Vorgänge gesamt (Shard 8)|Anzahl|Gesamt||Keine|
|cachehits8|Cachetreffer (Shard 8)|Anzahl|Gesamt||Keine|
|cachemisses8|Cachefehler (Shard 8)|Anzahl|Gesamt||Keine|
|getcommands8|Get-Vorgänge (Shard 8)|Anzahl|Gesamt||Keine|
|setcommands8|Set-Vorgänge (Shard 8)|Anzahl|Gesamt||Keine|
|operationsPerSecond8|Vorgänge pro Sekunde (Shard 8)|Anzahl|Maximum||Keine|
|evictedkeys8|Entfernte Schlüssel (Shard 8)|Anzahl|Gesamt||Keine|
|totalkeys8|Schlüssel gesamt (Shard 8)|Anzahl|Maximum||Keine|
|expiredkeys8|Abgelaufene Schlüssel (Shard 8)|Anzahl|Gesamt||Keine|
|usedmemory8|Verwendeter Arbeitsspeicher (Shard 8)|Byte|Maximum||Keine|
|usedmemoryRss8|Verwendeter Arbeitsspeicher (RSS, Shard 8)|Byte|Maximum||Keine|
|serverLoad8|Serverauslastung (Shard 8)|Percent|Maximum||Keine|
|cacheWrite8|Cacheschreibvorgänge (Shard 8)|Bytes pro Sekunde|Maximum||Keine|
|cacheRead8|Cachelesevorgänge (Shard 8)|Bytes pro Sekunde|Maximum||Keine|
|percentProcessorTime8|CPU (Shard 8)|Percent|Maximum||Keine|
|connectedclients9|Verbundene Clients (Shard 9)|Anzahl|Maximum||Keine|
|totalcommandsprocessed9|Vorgänge gesamt (Shard 9)|Anzahl|Gesamt||Keine|
|cachehits9|Cachetreffer (Shard 9)|Anzahl|Gesamt||Keine|
|cachemisses9|Cachefehler (Shard 9)|Anzahl|Gesamt||Keine|
|getcommands9|Get-Vorgänge (Shard 9)|Anzahl|Gesamt||Keine|
|setcommands9|Set-Vorgänge (Shard 9)|Anzahl|Gesamt||Keine|
|operationsPerSecond9|Vorgänge pro Sekunde (Shard 9)|Anzahl|Maximum||Keine|
|evictedkeys9|Entfernte Schlüssel (Shard 9)|Anzahl|Gesamt||Keine|
|totalkeys9|Schlüssel gesamt (Shard 9)|Anzahl|Maximum||Keine|
|expiredkeys9|Abgelaufene Schlüssel (Shard 9)|Anzahl|Gesamt||Keine|
|usedmemory9|Verwendeter Arbeitsspeicher (Shard 9)|Byte|Maximum||Keine|
|usedmemoryRss9|Verwendeter Arbeitsspeicher (RSS, Shard 9)|Byte|Maximum||Keine|
|serverLoad9|Serverauslastung (Shard 9)|Percent|Maximum||Keine|
|cacheWrite9|Cacheschreibvorgänge (Shard 9)|Bytes pro Sekunde|Maximum||Keine|
|cacheRead9|Cachelesevorgänge (Shard 9)|Bytes pro Sekunde|Maximum||Keine|
|percentProcessorTime9|CPU (Shard 9)|Percent|Maximum||Keine|



## <a name="microsoftcdncdnwebapplicationfirewallpolicies"></a>Microsoft.Cdn/cdnwebapplicationfirewallpolicies

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|WebApplicationFirewallRequestCount|Anforderungsanzahl für die Web Application Firewall|Anzahl|Gesamt|Die Anzahl der von der Web Application Firewall verarbeiteten Clientanforderungen|PolicyName, RuleName, Action|


## <a name="microsoftclassiccomputevirtualmachines"></a>Microsoft.ClassicCompute/virtualMachines

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|CPU in Prozent|CPU in Prozent|Percent|Average|Der Prozentsatz der zugewiesenen Computeeinheiten, die derzeit von den virtuellen Computern verwendet werden|Keine|
|Netzwerk eingehend|Netzwerk eingehend|Byte|Gesamt|Die Anzahl von Bytes, die von den virtuellen Computern an allen Netzwerkschnittstellen empfangen werden (eingehender Datenverkehr)|Keine|
|Netzwerk ausgehend|Netzwerk ausgehend|Byte|Gesamt|Die Anzahl von Bytes, die von den virtuellen Computern an allen Netzwerkschnittstellen gesendet werden (ausgehender Datenverkehr)|Keine|
|Disk Read Bytes/Sec|Datenträgerlesevorgänge|Bytes pro Sekunde|Average|Durchschnitt an Bytes, die während des Überwachungszeitraums vom Datenträger gelesen werden|Keine|
|Disk Write Bytes/Sec|Datenträgerschreibvorgänge|Bytes pro Sekunde|Average|Durchschnitt an Bytes, die während des Überwachungszeitraums auf den Datenträger geschrieben werden|Keine|
|Datenträgerlesevorgänge/Sek.|Datenträgerlesevorgänge/Sek.|Anzahl pro Sekunde|Average|Datenträgerlesevorgänge in IOPS|Keine|
|Datenträgerschreibvorgänge/Sek.|Datenträgerschreibvorgänge/Sek.|Anzahl pro Sekunde|Average|Datenträgerschreibvorgänge in IOPS|Keine|


## <a name="microsoftclassiccomputedomainnamesslotsroles"></a>Microsoft.ClassicCompute/domainNames/slots/roles

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|CPU in Prozent|CPU in Prozent|Percent|Average|Der Prozentsatz der zugewiesenen Computeeinheiten, die derzeit von den virtuellen Computern verwendet werden|RoleInstanceId|
|Netzwerk eingehend|Netzwerk eingehend|Byte|Gesamt|Die Anzahl von Bytes, die von den virtuellen Computern an allen Netzwerkschnittstellen empfangen werden (eingehender Datenverkehr)|RoleInstanceId|
|Netzwerk ausgehend|Netzwerk ausgehend|Byte|Gesamt|Die Anzahl von Bytes, die von den virtuellen Computern an allen Netzwerkschnittstellen gesendet werden (ausgehender Datenverkehr)|RoleInstanceId|
|Disk Read Bytes/Sec|Datenträgerlesevorgänge|Bytes pro Sekunde|Average|Durchschnitt an Bytes, die während des Überwachungszeitraums vom Datenträger gelesen werden|RoleInstanceId|
|Disk Write Bytes/Sec|Datenträgerschreibvorgänge|Bytes pro Sekunde|Average|Durchschnitt an Bytes, die während des Überwachungszeitraums auf den Datenträger geschrieben werden|RoleInstanceId|
|Datenträgerlesevorgänge/Sek.|Datenträgerlesevorgänge/Sek.|Anzahl pro Sekunde|Average|Datenträgerlesevorgänge in IOPS|RoleInstanceId|
|Datenträgerschreibvorgänge/Sek.|Datenträgerschreibvorgänge/Sek.|Anzahl pro Sekunde|Average|Datenträgerschreibvorgänge in IOPS|RoleInstanceId|



## <a name="microsoftclassicstoragestorageaccounts"></a>Microsoft.ClassicStorage/storageAccounts

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|UsedCapacity|Verwendete Kapazität|Byte|Average|Vom Konto verwendete Kapazität|Keine|
|Transaktionen|Transaktionen|Anzahl|Gesamt|Die Anzahl von Anforderungen, die an einen Speicherdienst oder an den angegebenen API-Vorgang gerichtet wurden. Diese Anzahl umfasst erfolgreiche und fehlgeschlagene Anforderungen sowie Anforderungen, die Fehler erzeugt haben. Verwenden Sie die Dimension „ResponseType“ für die Anzahl von verschiedenen Antworttypen.|ResponseType, GeoType, ApiName, Authentication|
|Eingehende Daten|Eingehende Daten|Byte|Gesamt|Die Menge der Eingangsdaten in Byte. Dieser Wert umfasst an Azure Storage gerichtete eingehende Daten von einem externen Client sowie eingehende Daten innerhalb von Azure.|GeoType, ApiName, Authentication|
|Ausgehende Daten|Ausgehende Daten|Byte|Gesamt|Die Menge der Ausgangsdaten in Byte. Dieser Wert umfasst an Azure Storage gerichtete ausgehende Daten von einem externen Client sowie ausgehende Daten innerhalb von Azure. Der Wert stellt somit keine gebührenpflichtigen ausgehenden Daten dar.|GeoType, ApiName, Authentication|
|SuccessServerLatency|Serverlatenz (erfolgreich)|Millisekunden|Average|Die Latenz in Millisekunden für die Verarbeitung einer erfolgreichen Anforderung durch Azure Storage. Dieser Wert beinhaltet nicht die in „SuccessE2ELatency“ angegebene Netzwerklatenz.|GeoType, ApiName, Authentication|
|SuccessE2ELatency|E2E-Latenz (erfolgreich)|Millisekunden|Average|Die End-to-End-Latenz in Millisekunden für erfolgreiche Anforderungen, die an einen Speicherdienst oder den angegebenen API-Vorgang gesendet wurden. Dieser Wert umfasst die erforderliche Verarbeitungszeit in Azure Storage für das Lesen der Anforderung, das Senden der Antwort und das Empfangen der Antwortbestätigung.|GeoType, ApiName, Authentication|
|Verfügbarkeit|Verfügbarkeit|Percent|Average|Die Verfügbarkeit (in Prozent) für den Speicherdienst oder den angegebenen API-Vorgang. Die Verfügbarkeit wird berechnet, indem der Wert „TotalBillableRequests“ durch die Anzahl von zutreffenden Anforderungen, einschließlich der, die unerwartete Fehler erzeugt haben, geteilt wird. Alle unerwarteten Fehler verringern die Verfügbarkeit für den Speicherdienst oder den angegebenen API-Vorgang.|GeoType, ApiName, Authentication|

## <a name="microsoftclassicstoragestorageaccountsblobservices"></a>Microsoft.ClassicStorage/storageAccounts/blobServices

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|BlobCapacity|Blob-Kapazität|Byte|Average|Die Größe des vom Blob-Dienst des Speicherkontos genutzten Speichers in Byte.|BlobType, Tier|
|BlobCount|Anzahl von Blobs|Anzahl|Average|Die Anzahl von Blobs im Blob-Dienst des Speicherkontos.|BlobType, Tier|
|ContainerCount|Anzahl von Blob-Containern|Anzahl|Average|Die Anzahl von Containern im Blob-Dienst des Speicherkontos.|Keine|
|IndexCapacity|Indexkapazität|Byte|Average|Die vom ADLS Gen2-Index (hierarchisch) verwendete Speichermenge in Byte.|Keine|
|Transaktionen|Transaktionen|Anzahl|Gesamt|Die Anzahl von Anforderungen, die an einen Speicherdienst oder an den angegebenen API-Vorgang gerichtet wurden. Diese Anzahl umfasst erfolgreiche und fehlgeschlagene Anforderungen sowie Anforderungen, die Fehler erzeugt haben. Verwenden Sie die Dimension „ResponseType“ für die Anzahl von verschiedenen Antworttypen.|ResponseType, GeoType, ApiName, Authentication|
|Eingehende Daten|Eingehende Daten|Byte|Gesamt|Die Menge der Eingangsdaten in Byte. Dieser Wert umfasst an Azure Storage gerichtete eingehende Daten von einem externen Client sowie eingehende Daten innerhalb von Azure.|GeoType, ApiName, Authentication|
|Ausgehende Daten|Ausgehende Daten|Byte|Gesamt|Die Menge der Ausgangsdaten in Byte. Dieser Wert umfasst an Azure Storage gerichtete ausgehende Daten von einem externen Client sowie ausgehende Daten innerhalb von Azure. Der Wert stellt somit keine gebührenpflichtigen ausgehenden Daten dar.|GeoType, ApiName, Authentication|
|SuccessServerLatency|Serverlatenz (erfolgreich)|Millisekunden|Average|Die Latenz in Millisekunden für die Verarbeitung einer erfolgreichen Anforderung durch Azure Storage. Dieser Wert beinhaltet nicht die in „SuccessE2ELatency“ angegebene Netzwerklatenz.|GeoType, ApiName, Authentication|
|SuccessE2ELatency|E2E-Latenz (erfolgreich)|Millisekunden|Average|Die End-to-End-Latenz in Millisekunden für erfolgreiche Anforderungen, die an einen Speicherdienst oder den angegebenen API-Vorgang gesendet wurden. Dieser Wert umfasst die erforderliche Verarbeitungszeit in Azure Storage für das Lesen der Anforderung, das Senden der Antwort und das Empfangen der Antwortbestätigung.|GeoType, ApiName, Authentication|
|Verfügbarkeit|Verfügbarkeit|Percent|Average|Die Verfügbarkeit (in Prozent) für den Speicherdienst oder den angegebenen API-Vorgang. Die Verfügbarkeit wird berechnet, indem der Wert „TotalBillableRequests“ durch die Anzahl von zutreffenden Anforderungen, einschließlich der, die unerwartete Fehler erzeugt haben, geteilt wird. Alle unerwarteten Fehler verringern die Verfügbarkeit für den Speicherdienst oder den angegebenen API-Vorgang.|GeoType, ApiName, Authentication|

## <a name="microsoftclassicstoragestorageaccountstableservices"></a>Microsoft.ClassicStorage/storageAccounts/tableServices

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|TableCapacity|Tabellenkapazität|Byte|Average|Die Größe des vom Tabellendienst des Speicherkontos genutzten Speichers in Byte.|Keine|
|TableCount|Anzahl von Tabellen|Anzahl|Average|Die Anzahl von Tabellen im Tabellendienst des Speicherkontos|Keine|
|TableEntityCount|Anzahl von Tabellenentitäten|Anzahl|Average|Die Anzahl von Tabellenentitäten im Tabellendienst des Speicherkontos|Keine|
|Transaktionen|Transaktionen|Anzahl|Gesamt|Die Anzahl von Anforderungen, die an einen Speicherdienst oder an den angegebenen API-Vorgang gerichtet wurden. Diese Anzahl umfasst erfolgreiche und fehlgeschlagene Anforderungen sowie Anforderungen, die Fehler erzeugt haben. Verwenden Sie die Dimension „ResponseType“ für die Anzahl von verschiedenen Antworttypen.|ResponseType, GeoType, ApiName, Authentication|
|Eingehende Daten|Eingehende Daten|Byte|Gesamt|Die Menge der Eingangsdaten in Byte. Dieser Wert umfasst an Azure Storage gerichtete eingehende Daten von einem externen Client sowie eingehende Daten innerhalb von Azure.|GeoType, ApiName, Authentication|
|Ausgehende Daten|Ausgehende Daten|Byte|Gesamt|Die Menge der Ausgangsdaten in Byte. Dieser Wert umfasst an Azure Storage gerichtete ausgehende Daten von einem externen Client sowie ausgehende Daten innerhalb von Azure. Der Wert stellt somit keine gebührenpflichtigen ausgehenden Daten dar.|GeoType, ApiName, Authentication|
|SuccessServerLatency|Serverlatenz (erfolgreich)|Millisekunden|Average|Die Latenz in Millisekunden für die Verarbeitung einer erfolgreichen Anforderung durch Azure Storage. Dieser Wert beinhaltet nicht die in „SuccessE2ELatency“ angegebene Netzwerklatenz.|GeoType, ApiName, Authentication|
|SuccessE2ELatency|E2E-Latenz (erfolgreich)|Millisekunden|Average|Die End-to-End-Latenz in Millisekunden für erfolgreiche Anforderungen, die an einen Speicherdienst oder den angegebenen API-Vorgang gesendet wurden. Dieser Wert umfasst die erforderliche Verarbeitungszeit in Azure Storage für das Lesen der Anforderung, das Senden der Antwort und das Empfangen der Antwortbestätigung.|GeoType, ApiName, Authentication|
|Verfügbarkeit|Verfügbarkeit|Percent|Average|Die Verfügbarkeit (in Prozent) für den Speicherdienst oder den angegebenen API-Vorgang. Die Verfügbarkeit wird berechnet, indem der Wert „TotalBillableRequests“ durch die Anzahl von zutreffenden Anforderungen, einschließlich der, die unerwartete Fehler erzeugt haben, geteilt wird. Alle unerwarteten Fehler verringern die Verfügbarkeit für den Speicherdienst oder den angegebenen API-Vorgang.|GeoType, ApiName, Authentication|

## <a name="microsoftclassicstoragestorageaccountsfileservices"></a>Microsoft.ClassicStorage/storageAccounts/fileServices

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|FileCapacity|Dateikapazität|Byte|Average|Die Größe des vom Dateidienst des Speicherkontos genutzten Speichers in Byte.|FileShare|
|FileCount|Dateianzahl|Anzahl|Average|Die Anzahl von Dateien im Dateidienst des Speicherkontos|FileShare|
|FileShareCount|Anzahl von Dateifreigaben|Anzahl|Average|Die Anzahl von Dateifreigaben im Dateidienst des Speicherkontos|Keine|
|FileShareSnapshotCount|Anzahl von Momentaufnahmen in Dateifreigabe|Anzahl|Average|Die Anzahl der enthaltenen Momentaufnahmen im Dateidienst des Speicherkontos in der Freigabe|FileShare|
|FileShareSnapshotSize|Größe der Momentaufnahmen in Dateifreigabe|Byte|Average|Die Speichermenge in Byte, die von den Momentaufnahmen im Dateidienst des Speicherkontos verwendet wird.|FileShare|
|FileShareQuota|Kontingentgröße für Dateifreigabe|Byte|Average|Die Obergrenze für die Speichermenge zur Verwendung durch den Azure Files-Dienst, angegeben in Byte.|FileShare|
|Transaktionen|Transaktionen|Anzahl|Gesamt|Die Anzahl von Anforderungen, die an einen Speicherdienst oder an den angegebenen API-Vorgang gerichtet wurden. Diese Anzahl umfasst erfolgreiche und fehlgeschlagene Anforderungen sowie Anforderungen, die Fehler erzeugt haben. Verwenden Sie die Dimension „ResponseType“ für die Anzahl von verschiedenen Antworttypen.|ResponseType, GeoType, ApiName, Authentication, FileShare|
|Eingehende Daten|Eingehende Daten|Byte|Gesamt|Die Menge der Eingangsdaten in Byte. Dieser Wert umfasst an Azure Storage gerichtete eingehende Daten von einem externen Client sowie eingehende Daten innerhalb von Azure.|GeoType, ApiName, Authentication, FileShare|
|Ausgehende Daten|Ausgehende Daten|Byte|Gesamt|Die Menge der Ausgangsdaten in Byte. Dieser Wert umfasst an Azure Storage gerichtete ausgehende Daten von einem externen Client sowie ausgehende Daten innerhalb von Azure. Der Wert stellt somit keine gebührenpflichtigen ausgehenden Daten dar.|GeoType, ApiName, Authentication, FileShare|
|SuccessServerLatency|Serverlatenz (erfolgreich)|Millisekunden|Average|Die Latenz in Millisekunden für die Verarbeitung einer erfolgreichen Anforderung durch Azure Storage. Dieser Wert beinhaltet nicht die in „SuccessE2ELatency“ angegebene Netzwerklatenz.|GeoType, ApiName, Authentication, FileShare|
|SuccessE2ELatency|E2E-Latenz (erfolgreich)|Millisekunden|Average|Die End-to-End-Latenz in Millisekunden für erfolgreiche Anforderungen, die an einen Speicherdienst oder den angegebenen API-Vorgang gesendet wurden. Dieser Wert umfasst die erforderliche Verarbeitungszeit in Azure Storage für das Lesen der Anforderung, das Senden der Antwort und das Empfangen der Antwortbestätigung.|GeoType, ApiName, Authentication, FileShare|
|Verfügbarkeit|Verfügbarkeit|Percent|Average|Die Verfügbarkeit (in Prozent) für den Speicherdienst oder den angegebenen API-Vorgang. Die Verfügbarkeit wird berechnet, indem der Wert „TotalBillableRequests“ durch die Anzahl von zutreffenden Anforderungen, einschließlich der, die unerwartete Fehler erzeugt haben, geteilt wird. Alle unerwarteten Fehler verringern die Verfügbarkeit für den Speicherdienst oder den angegebenen API-Vorgang.|GeoType, ApiName, Authentication, FileShare|

## <a name="microsoftclassicstoragestorageaccountsqueueservices"></a>Microsoft.ClassicStorage/storageAccounts/queueServices

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|QueueCapacity|Warteschlangenkapazität|Byte|Average|Die Größe des vom Warteschlangendienst des Speicherkontos genutzten Speichers in Byte.|Keine|
|QueueCount|Anzahl von Warteschlangen|Anzahl|Average|Die Anzahl von Warteschlangen im Warteschlangendienst des Speicherkontos|Keine|
|QueueMessageCount|Anzahl von Warteschlangennachrichten|Anzahl|Average|Die ungefähre Anzahl von Warteschlangennachrichten im Warteschlangendienst des Speicherkontos|Keine|
|Transaktionen|Transaktionen|Anzahl|Gesamt|Die Anzahl von Anforderungen, die an einen Speicherdienst oder an den angegebenen API-Vorgang gerichtet wurden. Diese Anzahl umfasst erfolgreiche und fehlgeschlagene Anforderungen sowie Anforderungen, die Fehler erzeugt haben. Verwenden Sie die Dimension „ResponseType“ für die Anzahl von verschiedenen Antworttypen.|ResponseType, GeoType, ApiName, Authentication|
|Eingehende Daten|Eingehende Daten|Byte|Gesamt|Die Menge der Eingangsdaten in Byte. Dieser Wert umfasst an Azure Storage gerichtete eingehende Daten von einem externen Client sowie eingehende Daten innerhalb von Azure.|GeoType, ApiName, Authentication|
|Ausgehende Daten|Ausgehende Daten|Byte|Gesamt|Die Menge der Ausgangsdaten in Byte. Dieser Wert umfasst an Azure Storage gerichtete ausgehende Daten von einem externen Client sowie ausgehende Daten innerhalb von Azure. Der Wert stellt somit keine gebührenpflichtigen ausgehenden Daten dar.|GeoType, ApiName, Authentication|
|SuccessServerLatency|Serverlatenz (erfolgreich)|Millisekunden|Average|Die Latenz in Millisekunden für die Verarbeitung einer erfolgreichen Anforderung durch Azure Storage. Dieser Wert beinhaltet nicht die in „SuccessE2ELatency“ angegebene Netzwerklatenz.|GeoType, ApiName, Authentication|
|SuccessE2ELatency|E2E-Latenz (erfolgreich)|Millisekunden|Average|Die End-to-End-Latenz in Millisekunden für erfolgreiche Anforderungen, die an einen Speicherdienst oder den angegebenen API-Vorgang gesendet wurden. Dieser Wert umfasst die erforderliche Verarbeitungszeit in Azure Storage für das Lesen der Anforderung, das Senden der Antwort und das Empfangen der Antwortbestätigung.|GeoType, ApiName, Authentication|
|Verfügbarkeit|Verfügbarkeit|Percent|Average|Die Verfügbarkeit (in Prozent) für den Speicherdienst oder den angegebenen API-Vorgang. Die Verfügbarkeit wird berechnet, indem der Wert „TotalBillableRequests“ durch die Anzahl von zutreffenden Anforderungen, einschließlich der, die unerwartete Fehler erzeugt haben, geteilt wird. Alle unerwarteten Fehler verringern die Verfügbarkeit für den Speicherdienst oder den angegebenen API-Vorgang.|GeoType, ApiName, Authentication|


## <a name="microsoftcognitiveservicesaccounts"></a>Microsoft.CognitiveServices/accounts

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|TotalCalls|Aufrufe gesamt|Anzahl|Gesamt|Gesamtanzahl von Aufrufen|ApiName, OperationName, Region|
|SuccessfulCalls|Erfolgreiche Aufrufe|Anzahl|Gesamt|Anzahl erfolgreicher Aufrufe|ApiName, OperationName, Region|
|TotalErrors|Fehler insgesamt|Anzahl|Gesamt|Gesamtzahl von Aufrufen mit Fehlerantwort (HTTP-Antwortcode 4xx oder 5xx)|ApiName, OperationName, Region|
|BlockedCalls|Blockierte Aufrufe|Anzahl|Gesamt|Anzahl von Aufrufen, die das Raten- oder Kontingentlimit überschritten haben|ApiName, OperationName, Region|
|ServerErrors|Serverfehler|Anzahl|Gesamt|Anzahl von Aufrufen mit internem Dienstfehler (HTTP-Antwortcode 5xx)|ApiName, OperationName, Region|
|ClientErrors|Clientfehler|Anzahl|Gesamt|Anzahl von Aufrufen mit Fehler auf Clientseite (HTTP-Antwortcode 4xx)|ApiName, OperationName, Region|
|DataIn|Eingehende Daten|Byte|Gesamt|Menge eingehender Daten in Byte|ApiName, OperationName, Region|
|DataOut|Datenausgabe|Byte|Gesamt|Menge ausgehender Daten in Byte|ApiName, OperationName, Region|
|Latency|Latency|Millisekunden|Average|Latenz in Millisekunden|ApiName, OperationName, Region|
|CharactersTranslated|Übersetzte Zeichen|Anzahl|Gesamt|Gesamtanzahl von Zeichen in einer eingehenden Textanforderung|ApiName, OperationName, Region|
|CharactersTrained|Trainierte Zeichen|Anzahl|Gesamt|Gesamtzahl trainierter Zeichen|ApiName, OperationName, Region|
|SpeechSessionDuration|Dauer der Sprachsitzung|Sekunden|Gesamt|Gesamtdauer der Sprachsitzung in Sekunden|ApiName, OperationName, Region|
|TotalTransactions|Transaktionen gesamt|Anzahl|Gesamt|Gesamtanzahl von Transaktionen|Keine|
|TotalTokenCalls|Tokenaufrufe gesamt|Anzahl|Gesamt|Gesamtanzahl von Tokenaufrufen|ApiName, OperationName, Region|

## <a name="microsoftcomputevirtualmachines"></a>Microsoft.Compute/virtualMachines

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|CPU in Prozent|CPU in Prozent|Percent|Average|Der Prozentsatz der zugewiesenen Computeeinheiten, die derzeit von den virtuellen Computern verwendet werden|Keine|
|Netzwerk eingehend|Abrechenbarer eingehender Netzwerkdatenverkehr (veraltet)|Byte|Gesamt|Die Anzahl von abrechenbaren empfangenen Bytes für alle Netzwerkschnittstellen der VMs (eingehender Datenverkehr) (veraltet)|Keine|
|Netzwerk ausgehend|Abrechenbarer ausgehender Netzwerkdatenverkehr (veraltet)|Byte|Gesamt|Die Anzahl von abrechenbaren gesendeten Bytes für alle Netzwerkschnittstellen der VMs (ausgehender Datenverkehr) (veraltet)|Keine|
|Datenträgerlesevorgänge in Bytes|Datenträgerlesevorgänge in Bytes|Byte|Gesamt|Bytes, die während des Überwachungszeitraums vom Datenträger gelesen werden|Keine|
|Datenträgerschreibvorgänge in Bytes|Datenträgerschreibvorgänge in Bytes|Byte|Gesamt|Bytes, die während des Überwachungszeitraums auf den Datenträger geschrieben werden|Keine|
|Datenträgerlesevorgänge/Sek.|Datenträgerlesevorgänge/Sek.|Anzahl pro Sekunde|Average|Datenträgerlesevorgänge in IOPS|Keine|
|Datenträgerschreibvorgänge/Sek.|Datenträgerschreibvorgänge/Sek.|Anzahl pro Sekunde|Average|Datenträgerschreibvorgänge in IOPS|Keine|
|Verbleibende CPU-Guthaben|Verbleibende CPU-Guthaben|Anzahl|Average|Gesamtanzahl von Guthaben, die für den Burst verfügbar sind|Keine|
|Verbrauchte CPU-Guthaben|Verbrauchte CPU-Guthaben|Anzahl|Average|Gesamtanzahl von Guthaben, die vom virtuellen Computer verwendet werden|Keine|
|Gelesene Bytes pro Datenträger/Sek.|Vom Datenträger gelesene Bytes/Sek. (veraltet)|Anzahl pro Sekunde|Average|Bytes, die während des Überwachungszeitraums pro Sekunde auf einem einzelnen Datenträger gelesen wurden|SlotId|
|Geschriebene Bytes pro Datenträger/Sek.|Auf den Datenträger geschriebene Bytes/Sek. (veraltet)|Anzahl pro Sekunde|Average|Bytes, die während des Überwachungszeitraums pro Sekunde auf einen einzelnen Datenträger geschrieben wurden|SlotId|
|Lesevorgänge pro Datenträger/Sek.|Datenträgerlesevorgänge/Sek. (veraltet)|Anzahl pro Sekunde|Average|IOPS, die während des Überwachungszeitraums auf einem einzelnen Datenträger gelesen wurden|SlotId|
|Schreibvorgänge pro Datenträger/Sek.|Datenträgerschreibvorgänge/Sek. (veraltet)|Anzahl pro Sekunde|Average|IOPS, die während des Überwachungszeitraums auf einen einzelnen Datenträger geschrieben wurden|SlotId|
|Warteschlangentiefe pro Datenträger|Warteschlangentiefe für Datenträger (veraltet)|Anzahl|Average|Warteschlangentiefe (oder Warteschlangenlänge) für Datenträger|SlotId|
|Gelesene Bytes pro Betriebssystemdatenträger/Sek.|Vom Betriebssystemdatenträger gelesene Bytes/Sek. (veraltet)|Anzahl pro Sekunde|Average|Bytes, die während des Überwachungszeitraums für den Betriebssystemdatenträger pro Sekunde auf einem einzelnen Datenträger gelesen wurden|Keine|
|Geschriebene Bytes pro Betriebssystemdatenträger/Sek.|Auf den Betriebssystemdatenträger geschriebene Bytes/Sek. (veraltet)|Anzahl pro Sekunde|Average|Bytes, die während des Überwachungszeitraums für einen Betriebssystemdatenträger pro Sekunde auf einen einzelnen Datenträger geschrieben wurden|Keine|
|Lesevorgänge pro Betriebssystemdatenträger/Sek.|Lesevorgänge auf Betriebssystemdatenträger/Sek. (veraltet)|Anzahl pro Sekunde|Average|IOPS, die während des Überwachungszeitraums für einen Betriebssystemdatenträger auf einem einzelnen Datenträger gelesen wurden|Keine|
|Schreibvorgänge pro Betriebssystemdatenträger/Sek.|Schreibvorgänge auf Betriebssystemdatenträger/Sek. (veraltet)|Anzahl pro Sekunde|Average|IOPS, die während des Überwachungszeitraums für einen Betriebssystemdatenträger auf einen einzelnen Datenträger geschrieben wurden|Keine|
|Warteschlangentiefe pro Betriebssystemdatenträger|Warteschlangentiefe für Betriebssystemdatenträger (veraltet)|Anzahl|Average|Warteschlangentiefe (oder Warteschlangenlänge) für Betriebssystemdatenträger|Keine|
|Vom Datenträger gelesene Bytes/Sek.|Vom Datenträger gelesene Bytes/Sek. (Vorschauversion)|Anzahl pro Sekunde|Average|Bytes, die während des Überwachungszeitraums pro Sekunde auf einem einzelnen Datenträger gelesen wurden|LUN|
|Auf den Datenträger geschriebene Bytes/Sek.|Auf den Datenträger geschriebene Bytes/Sek. (Vorschauversion)|Anzahl pro Sekunde|Average|Bytes, die während des Überwachungszeitraums pro Sekunde auf einen einzelnen Datenträger geschrieben wurden|LUN|
|Datenträgerlesevorgänge/Sek.|Datenträgerlesevorgänge/Sek. (Vorschauversion)|Anzahl pro Sekunde|Average|IOPS, die während des Überwachungszeitraums auf einem einzelnen Datenträger gelesen wurden|LUN|
|Datenträgerschreibvorgänge/Sek.|Datenträgerschreibvorgänge/Sek. (Vorschauversion)|Anzahl pro Sekunde|Average|IOPS, die während des Überwachungszeitraums auf einen einzelnen Datenträger geschrieben wurden|LUN|
|Warteschlangentiefe für Datenträger|Warteschlangentiefe für Datenträger (Vorschauversion)|Anzahl|Average|Warteschlangentiefe (oder Warteschlangenlänge) für Datenträger|LUN|
|Vom Betriebssystemdatenträger gelesene Bytes/Sek.|Vom Betriebssystemdatenträger gelesene Bytes/Sek. (Vorschauversion)|Anzahl pro Sekunde|Average|Bytes, die während des Überwachungszeitraums für den Betriebssystemdatenträger pro Sekunde auf einem einzelnen Datenträger gelesen wurden|Keine|
|Auf den Betriebssystemdatenträger geschriebene Bytes/Sek.|Auf den Betriebssystemdatenträger geschriebene Bytes/Sek.|Anzahl pro Sekunde|Average|Bytes, die während des Überwachungszeitraums für einen Betriebssystemdatenträger pro Sekunde auf einen einzelnen Datenträger geschrieben wurden|Keine|
|Lesevorgänge auf Betriebssystemdatenträger/Sek.|Lesevorgänge auf Betriebssystemdatenträger/Sek.|Anzahl pro Sekunde|Average|IOPS, die während des Überwachungszeitraums für einen Betriebssystemdatenträger auf einem einzelnen Datenträger gelesen wurden|Keine|
|Schreibvorgänge auf Betriebssystemdatenträger/Sek.|Schreibvorgänge auf Betriebssystemdatenträger/Sek.|Anzahl pro Sekunde|Average|IOPS, die während des Überwachungszeitraums für einen Betriebssystemdatenträger auf einen einzelnen Datenträger geschrieben wurden|Keine|
|Warteschlangentiefe für Betriebssystemdatenträger|Warteschlangentiefe für Betriebssystemdatenträger (Vorschauversion)|Anzahl|Average|Warteschlangentiefe (oder Warteschlangenlänge) für Betriebssystemdatenträger|Keine|
|Eingehende Datenflüsse|Eingehende Datenflüsse|Anzahl|Average|Eingehende Datenflüsse sind die Anzahl aktueller Datenflüsse in eingehender Richtung (eingehender Datenverkehr im virtuellen Computer)|Keine|
|Ausgehende Datenflüsse|Ausgehende Datenflüsse|Anzahl|Average|Ausgehende Datenflüsse sind die Anzahl aktueller Datenflüsse in ausgehender Richtung (ausgehender Datenverkehr vom virtuellen Computer)|Keine|
|Maximale Erstellungsrate für eingehende Datenflüsse|Maximale Erstellungsrate für eingehende Datenflüsse (Vorschauversion)|Anzahl pro Sekunde|Average|Die maximale Erstellungsrate für eingehende Datenflüsse (eingehender Datenverkehr im virtuellen Computer)|Keine|
|Maximale Erstellungsrate für ausgehende Datenflüsse|Maximale Erstellungsrate für ausgehende Datenflüsse (Vorschauversion)|Anzahl pro Sekunde|Average|Die maximale Erstellungsrate für ausgehende Datenflüsse (ausgehender Datenverkehr vom virtuellen Computer)|Keine|
|Treffer beim Cachelesevorgang auf Premium-Datenträger|Treffer beim Cachelesevorgang auf Premium-Datenträger (Vorschauversion)|Percent|Average|Treffer beim Cachelesevorgang auf Premium-Datenträger|LUN|
|Fehler beim Cachelesevorgang auf Premium-Datenträger|Fehler beim Cachelesevorgang auf Premium-Datenträger (Vorschauversion)|Percent|Average|Fehler beim Cachelesevorgang auf Premium-Datenträger|LUN|
|Treffer beim Cachelesevorgang auf Premium-Betriebssystemdatenträger|Treffer beim Cachelesevorgang auf Premium-Betriebssystemdatenträger (Vorschauversion)|Percent|Average|Treffer beim Cachelesevorgang auf Premium-Betriebssystemdatenträger|Keine|
|Fehler beim Cachelesevorgang auf Premium-Betriebssystemdatenträger|Fehler beim Cachelesevorgang auf Premium-Betriebssystemdatenträger (Vorschauversion)|Percent|Average|Fehler beim Cachelesevorgang auf Premium-Betriebssystemdatenträger|Keine|
|Eingehender Netzwerkverkehr gesamt|Eingehender Netzwerkverkehr gesamt|Byte|Gesamt|Die Anzahl von Bytes, die von den virtuellen Computern an allen Netzwerkschnittstellen empfangen werden (eingehender Datenverkehr)|Keine|
|Ausgehender Netzwerkverkehr gesamt|Ausgehender Netzwerkverkehr gesamt|Byte|Gesamt|Die Anzahl von Bytes, die von den virtuellen Computern an allen Netzwerkschnittstellen gesendet werden (ausgehender Datenverkehr)|Keine|


## <a name="microsoftcomputevirtualmachinescalesets"></a>Microsoft.Compute/virtualMachineScaleSets

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|CPU in Prozent|CPU in Prozent|Percent|Average|Der Prozentsatz der zugewiesenen Computeeinheiten, die derzeit von den virtuellen Computern verwendet werden|VMName|
|Netzwerk eingehend|Abrechenbarer eingehender Netzwerkdatenverkehr (veraltet)|Byte|Gesamt|Die Anzahl von abrechenbaren empfangenen Bytes für alle Netzwerkschnittstellen der VMs (eingehender Datenverkehr) (veraltet)|VMName|
|Netzwerk ausgehend|Abrechenbarer ausgehender Netzwerkdatenverkehr (veraltet)|Byte|Gesamt|Die Anzahl von abrechenbaren gesendeten Bytes für alle Netzwerkschnittstellen der VMs (ausgehender Datenverkehr) (veraltet)|VMName|
|Datenträgerlesevorgänge in Bytes|Datenträgerlesevorgänge in Bytes|Byte|Gesamt|Bytes, die während des Überwachungszeitraums vom Datenträger gelesen werden|VMName|
|Datenträgerschreibvorgänge in Bytes|Datenträgerschreibvorgänge in Bytes|Byte|Gesamt|Bytes, die während des Überwachungszeitraums auf den Datenträger geschrieben werden|VMName|
|Datenträgerlesevorgänge/Sek.|Datenträgerlesevorgänge/Sek.|Anzahl pro Sekunde|Average|Datenträgerlesevorgänge in IOPS|VMName|
|Datenträgerschreibvorgänge/Sek.|Datenträgerschreibvorgänge/Sek.|Anzahl pro Sekunde|Average|Datenträgerschreibvorgänge in IOPS|VMName|
|Verbleibende CPU-Guthaben|Verbleibende CPU-Guthaben|Anzahl|Average|Gesamtanzahl von Guthaben, die für den Burst verfügbar sind|Keine|
|Verbrauchte CPU-Guthaben|Verbrauchte CPU-Guthaben|Anzahl|Average|Gesamtanzahl von Guthaben, die vom virtuellen Computer verwendet werden|Keine|
|Gelesene Bytes pro Datenträger/Sek.|Vom Datenträger gelesene Bytes/Sek. (veraltet)|Anzahl pro Sekunde|Average|Bytes, die während des Überwachungszeitraums pro Sekunde auf einem einzelnen Datenträger gelesen wurden|SlotId|
|Geschriebene Bytes pro Datenträger/Sek.|Auf den Datenträger geschriebene Bytes/Sek. (veraltet)|Anzahl pro Sekunde|Average|Bytes, die während des Überwachungszeitraums pro Sekunde auf einen einzelnen Datenträger geschrieben wurden|SlotId|
|Lesevorgänge pro Datenträger/Sek.|Datenträgerlesevorgänge/Sek. (veraltet)|Anzahl pro Sekunde|Average|IOPS, die während des Überwachungszeitraums auf einem einzelnen Datenträger gelesen wurden|SlotId|
|Schreibvorgänge pro Datenträger/Sek.|Datenträgerschreibvorgänge/Sek. (veraltet)|Anzahl pro Sekunde|Average|IOPS, die während des Überwachungszeitraums auf einen einzelnen Datenträger geschrieben wurden|SlotId|
|Warteschlangentiefe pro Datenträger|Warteschlangentiefe für Datenträger (veraltet)|Anzahl|Average|Warteschlangentiefe (oder Warteschlangenlänge) für Datenträger|SlotId|
|Gelesene Bytes pro Betriebssystemdatenträger/Sek.|Vom Betriebssystemdatenträger gelesene Bytes/Sek. (veraltet)|Anzahl pro Sekunde|Average|Bytes, die während des Überwachungszeitraums für den Betriebssystemdatenträger pro Sekunde auf einem einzelnen Datenträger gelesen wurden|Keine|
|Geschriebene Bytes pro Betriebssystemdatenträger/Sek.|Auf den Betriebssystemdatenträger geschriebene Bytes/Sek. (veraltet)|Anzahl pro Sekunde|Average|Bytes, die während des Überwachungszeitraums für einen Betriebssystemdatenträger pro Sekunde auf einen einzelnen Datenträger geschrieben wurden|Keine|
|Lesevorgänge pro Betriebssystemdatenträger/Sek.|Lesevorgänge auf Betriebssystemdatenträger/Sek. (veraltet)|Anzahl pro Sekunde|Average|IOPS, die während des Überwachungszeitraums für einen Betriebssystemdatenträger auf einem einzelnen Datenträger gelesen wurden|Keine|
|Schreibvorgänge pro Betriebssystemdatenträger/Sek.|Schreibvorgänge auf Betriebssystemdatenträger/Sek. (veraltet)|Anzahl pro Sekunde|Average|IOPS, die während des Überwachungszeitraums für einen Betriebssystemdatenträger auf einen einzelnen Datenträger geschrieben wurden|Keine|
|Warteschlangentiefe pro Betriebssystemdatenträger|Warteschlangentiefe für Betriebssystemdatenträger (veraltet)|Anzahl|Average|Warteschlangentiefe (oder Warteschlangenlänge) für Betriebssystemdatenträger|Keine|
|Vom Datenträger gelesene Bytes/Sek.|Vom Datenträger gelesene Bytes/Sek. (Vorschauversion)|Anzahl pro Sekunde|Average|Bytes, die während des Überwachungszeitraums pro Sekunde auf einem einzelnen Datenträger gelesen wurden|LUN, VMName|
|Auf den Datenträger geschriebene Bytes/Sek.|Auf den Datenträger geschriebene Bytes/Sek. (Vorschauversion)|Anzahl pro Sekunde|Average|Bytes, die während des Überwachungszeitraums pro Sekunde auf einen einzelnen Datenträger geschrieben wurden|LUN, VMName|
|Datenträgerlesevorgänge/Sek.|Datenträgerlesevorgänge/Sek. (Vorschauversion)|Anzahl pro Sekunde|Average|IOPS, die während des Überwachungszeitraums auf einem einzelnen Datenträger gelesen wurden|LUN, VMName|
|Datenträgerschreibvorgänge/Sek.|Datenträgerschreibvorgänge/Sek. (Vorschauversion)|Anzahl pro Sekunde|Average|IOPS, die während des Überwachungszeitraums auf einen einzelnen Datenträger geschrieben wurden|LUN, VMName|
|Warteschlangentiefe für Datenträger|Warteschlangentiefe für Datenträger (Vorschauversion)|Anzahl|Average|Warteschlangentiefe (oder Warteschlangenlänge) für Datenträger|LUN, VMName|
|Vom Betriebssystemdatenträger gelesene Bytes/Sek.|Vom Betriebssystemdatenträger gelesene Bytes/Sek. (Vorschauversion)|Anzahl pro Sekunde|Average|Bytes, die während des Überwachungszeitraums für den Betriebssystemdatenträger pro Sekunde auf einem einzelnen Datenträger gelesen wurden|VMName|
|Auf den Betriebssystemdatenträger geschriebene Bytes/Sek.|Auf den Betriebssystemdatenträger geschriebene Bytes/Sek.|Anzahl pro Sekunde|Average|Bytes, die während des Überwachungszeitraums für einen Betriebssystemdatenträger pro Sekunde auf einen einzelnen Datenträger geschrieben wurden|VMName|
|Lesevorgänge auf Betriebssystemdatenträger/Sek.|Lesevorgänge auf Betriebssystemdatenträger/Sek.|Anzahl pro Sekunde|Average|IOPS, die während des Überwachungszeitraums für einen Betriebssystemdatenträger auf einem einzelnen Datenträger gelesen wurden|VMName|
|Schreibvorgänge auf Betriebssystemdatenträger/Sek.|Schreibvorgänge auf Betriebssystemdatenträger/Sek.|Anzahl pro Sekunde|Average|IOPS, die während des Überwachungszeitraums für einen Betriebssystemdatenträger auf einen einzelnen Datenträger geschrieben wurden|VMName|
|Warteschlangentiefe für Betriebssystemdatenträger|Warteschlangentiefe für Betriebssystemdatenträger (Vorschauversion)|Anzahl|Average|Warteschlangentiefe (oder Warteschlangenlänge) für Betriebssystemdatenträger|VMName|
|Eingehende Datenflüsse|Eingehende Datenflüsse|Anzahl|Average|Eingehende Datenflüsse sind die Anzahl aktueller Datenflüsse in eingehender Richtung (eingehender Datenverkehr im virtuellen Computer)|VMName|
|Ausgehende Datenflüsse|Ausgehende Datenflüsse|Anzahl|Average|Ausgehende Datenflüsse sind die Anzahl aktueller Datenflüsse in ausgehender Richtung (ausgehender Datenverkehr vom virtuellen Computer)|VMName|
|Maximale Erstellungsrate für eingehende Datenflüsse|Maximale Erstellungsrate für eingehende Datenflüsse (Vorschauversion)|Anzahl pro Sekunde|Average|Die maximale Erstellungsrate für eingehende Datenflüsse (eingehender Datenverkehr im virtuellen Computer)|VMName|
|Maximale Erstellungsrate für ausgehende Datenflüsse|Maximale Erstellungsrate für ausgehende Datenflüsse (Vorschauversion)|Anzahl pro Sekunde|Average|Die maximale Erstellungsrate für ausgehende Datenflüsse (ausgehender Datenverkehr vom virtuellen Computer)|VMName|
|Treffer beim Cachelesevorgang auf Premium-Datenträger|Treffer beim Cachelesevorgang auf Premium-Datenträger (Vorschauversion)|Percent|Average|Treffer beim Cachelesevorgang auf Premium-Datenträger|LUN, VMName|
|Fehler beim Cachelesevorgang auf Premium-Datenträger|Fehler beim Cachelesevorgang auf Premium-Datenträger (Vorschauversion)|Percent|Average|Fehler beim Cachelesevorgang auf Premium-Datenträger|LUN, VMName|
|Treffer beim Cachelesevorgang auf Premium-Betriebssystemdatenträger|Treffer beim Cachelesevorgang auf Premium-Betriebssystemdatenträger (Vorschauversion)|Percent|Average|Treffer beim Cachelesevorgang auf Premium-Betriebssystemdatenträger|VMName|
|Fehler beim Cachelesevorgang auf Premium-Betriebssystemdatenträger|Fehler beim Cachelesevorgang auf Premium-Betriebssystemdatenträger (Vorschauversion)|Percent|Average|Fehler beim Cachelesevorgang auf Premium-Betriebssystemdatenträger|VMName|
|Eingehender Netzwerkverkehr gesamt|Eingehender Netzwerkverkehr gesamt|Byte|Gesamt|Die Anzahl von Bytes, die von den virtuellen Computern an allen Netzwerkschnittstellen empfangen werden (eingehender Datenverkehr)|VMName|
|Ausgehender Netzwerkverkehr gesamt|Ausgehender Netzwerkverkehr gesamt|Byte|Gesamt|Die Anzahl von Bytes, die von den virtuellen Computern an allen Netzwerkschnittstellen gesendet werden (ausgehender Datenverkehr)|VMName|


## <a name="microsoftcomputevirtualmachinescalesetsvirtualmachines"></a>Microsoft.Compute/virtualMachineScaleSets/virtualMachines

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|CPU in Prozent|CPU in Prozent|Percent|Average|Der Prozentsatz der zugewiesenen Computeeinheiten, die derzeit von den virtuellen Computern verwendet werden|Keine|
|Netzwerk eingehend|Abrechenbarer eingehender Netzwerkdatenverkehr (veraltet)|Byte|Gesamt|Die Anzahl von abrechenbaren empfangenen Bytes für alle Netzwerkschnittstellen der VMs (eingehender Datenverkehr) (veraltet)|Keine|
|Netzwerk ausgehend|Abrechenbarer ausgehender Netzwerkdatenverkehr (veraltet)|Byte|Gesamt|Die Anzahl von abrechenbaren gesendeten Bytes für alle Netzwerkschnittstellen der VMs (ausgehender Datenverkehr) (veraltet)|Keine|
|Datenträgerlesevorgänge in Bytes|Datenträgerlesevorgänge in Bytes|Byte|Gesamt|Bytes, die während des Überwachungszeitraums vom Datenträger gelesen werden|Keine|
|Datenträgerschreibvorgänge in Bytes|Datenträgerschreibvorgänge in Bytes|Byte|Gesamt|Bytes, die während des Überwachungszeitraums auf den Datenträger geschrieben werden|Keine|
|Datenträgerlesevorgänge/Sek.|Datenträgerlesevorgänge/Sek.|Anzahl pro Sekunde|Average|Datenträgerlesevorgänge in IOPS|Keine|
|Datenträgerschreibvorgänge/Sek.|Datenträgerschreibvorgänge/Sek.|Anzahl pro Sekunde|Average|Datenträgerschreibvorgänge in IOPS|Keine|
|Verbleibende CPU-Guthaben|Verbleibende CPU-Guthaben|Anzahl|Average|Gesamtanzahl von Guthaben, die für den Burst verfügbar sind|Keine|
|Verbrauchte CPU-Guthaben|Verbrauchte CPU-Guthaben|Anzahl|Average|Gesamtanzahl von Guthaben, die vom virtuellen Computer verwendet werden|Keine|
|Gelesene Bytes pro Datenträger/Sek.|Vom Datenträger gelesene Bytes/Sek. (veraltet)|Anzahl pro Sekunde|Average|Bytes, die während des Überwachungszeitraums pro Sekunde auf einem einzelnen Datenträger gelesen wurden|SlotId|
|Geschriebene Bytes pro Datenträger/Sek.|Auf den Datenträger geschriebene Bytes/Sek. (veraltet)|Anzahl pro Sekunde|Average|Bytes, die während des Überwachungszeitraums pro Sekunde auf einen einzelnen Datenträger geschrieben wurden|SlotId|
|Lesevorgänge pro Datenträger/Sek.|Datenträgerlesevorgänge/Sek. (veraltet)|Anzahl pro Sekunde|Average|IOPS, die während des Überwachungszeitraums auf einem einzelnen Datenträger gelesen wurden|SlotId|
|Schreibvorgänge pro Datenträger/Sek.|Datenträgerschreibvorgänge/Sek. (veraltet)|Anzahl pro Sekunde|Average|IOPS, die während des Überwachungszeitraums auf einen einzelnen Datenträger geschrieben wurden|SlotId|
|Warteschlangentiefe pro Datenträger|Warteschlangentiefe für Datenträger (veraltet)|Anzahl|Average|Warteschlangentiefe (oder Warteschlangenlänge) für Datenträger|SlotId|
|Gelesene Bytes pro Betriebssystemdatenträger/Sek.|Vom Betriebssystemdatenträger gelesene Bytes/Sek. (veraltet)|Anzahl pro Sekunde|Average|Bytes, die während des Überwachungszeitraums für den Betriebssystemdatenträger pro Sekunde auf einem einzelnen Datenträger gelesen wurden|Keine|
|Geschriebene Bytes pro Betriebssystemdatenträger/Sek.|Auf den Betriebssystemdatenträger geschriebene Bytes/Sek. (veraltet)|Anzahl pro Sekunde|Average|Bytes, die während des Überwachungszeitraums für einen Betriebssystemdatenträger pro Sekunde auf einen einzelnen Datenträger geschrieben wurden|Keine|
|Lesevorgänge pro Betriebssystemdatenträger/Sek.|Lesevorgänge auf Betriebssystemdatenträger/Sek. (veraltet)|Anzahl pro Sekunde|Average|IOPS, die während des Überwachungszeitraums für einen Betriebssystemdatenträger auf einem einzelnen Datenträger gelesen wurden|Keine|
|Schreibvorgänge pro Betriebssystemdatenträger/Sek.|Schreibvorgänge auf Betriebssystemdatenträger/Sek. (veraltet)|Anzahl pro Sekunde|Average|IOPS, die während des Überwachungszeitraums für einen Betriebssystemdatenträger auf einen einzelnen Datenträger geschrieben wurden|Keine|
|Warteschlangentiefe pro Betriebssystemdatenträger|Warteschlangentiefe für Betriebssystemdatenträger (veraltet)|Anzahl|Average|Warteschlangentiefe (oder Warteschlangenlänge) für Betriebssystemdatenträger|Keine|
|Vom Datenträger gelesene Bytes/Sek.|Vom Datenträger gelesene Bytes/Sek. (Vorschauversion)|Anzahl pro Sekunde|Average|Bytes, die während des Überwachungszeitraums pro Sekunde auf einem einzelnen Datenträger gelesen wurden|LUN|
|Auf den Datenträger geschriebene Bytes/Sek.|Auf den Datenträger geschriebene Bytes/Sek. (Vorschauversion)|Anzahl pro Sekunde|Average|Bytes, die während des Überwachungszeitraums pro Sekunde auf einen einzelnen Datenträger geschrieben wurden|LUN|
|Datenträgerlesevorgänge/Sek.|Datenträgerlesevorgänge/Sek. (Vorschauversion)|Anzahl pro Sekunde|Average|IOPS, die während des Überwachungszeitraums auf einem einzelnen Datenträger gelesen wurden|LUN|
|Datenträgerschreibvorgänge/Sek.|Datenträgerschreibvorgänge/Sek. (Vorschauversion)|Anzahl pro Sekunde|Average|IOPS, die während des Überwachungszeitraums auf einen einzelnen Datenträger geschrieben wurden|LUN|
|Warteschlangentiefe für Datenträger|Warteschlangentiefe für Datenträger (Vorschauversion)|Anzahl|Average|Warteschlangentiefe (oder Warteschlangenlänge) für Datenträger|LUN|
|Vom Betriebssystemdatenträger gelesene Bytes/Sek.|Vom Betriebssystemdatenträger gelesene Bytes/Sek. (Vorschauversion)|Anzahl pro Sekunde|Average|Bytes, die während des Überwachungszeitraums für den Betriebssystemdatenträger pro Sekunde auf einem einzelnen Datenträger gelesen wurden|Keine|
|Auf den Betriebssystemdatenträger geschriebene Bytes/Sek.|Auf den Betriebssystemdatenträger geschriebene Bytes/Sek.|Anzahl pro Sekunde|Average|Bytes, die während des Überwachungszeitraums für einen Betriebssystemdatenträger pro Sekunde auf einen einzelnen Datenträger geschrieben wurden|Keine|
|Lesevorgänge auf Betriebssystemdatenträger/Sek.|Lesevorgänge auf Betriebssystemdatenträger/Sek.|Anzahl pro Sekunde|Average|IOPS, die während des Überwachungszeitraums für einen Betriebssystemdatenträger auf einem einzelnen Datenträger gelesen wurden|Keine|
|Schreibvorgänge auf Betriebssystemdatenträger/Sek.|Schreibvorgänge auf Betriebssystemdatenträger/Sek.|Anzahl pro Sekunde|Average|IOPS, die während des Überwachungszeitraums für einen Betriebssystemdatenträger auf einen einzelnen Datenträger geschrieben wurden|Keine|
|Warteschlangentiefe für Betriebssystemdatenträger|Warteschlangentiefe für Betriebssystemdatenträger (Vorschauversion)|Anzahl|Average|Warteschlangentiefe (oder Warteschlangenlänge) für Betriebssystemdatenträger|Keine|
|Eingehende Datenflüsse|Eingehende Datenflüsse|Anzahl|Average|Eingehende Datenflüsse sind die Anzahl aktueller Datenflüsse in eingehender Richtung (eingehender Datenverkehr im virtuellen Computer)|Keine|
|Ausgehende Datenflüsse|Ausgehende Datenflüsse|Anzahl|Average|Ausgehende Datenflüsse sind die Anzahl aktueller Datenflüsse in ausgehender Richtung (ausgehender Datenverkehr vom virtuellen Computer)|Keine|
|Maximale Erstellungsrate für eingehende Datenflüsse|Maximale Erstellungsrate für eingehende Datenflüsse (Vorschauversion)|Anzahl pro Sekunde|Average|Die maximale Erstellungsrate für eingehende Datenflüsse (eingehender Datenverkehr im virtuellen Computer)|Keine|
|Maximale Erstellungsrate für ausgehende Datenflüsse|Maximale Erstellungsrate für ausgehende Datenflüsse (Vorschauversion)|Anzahl pro Sekunde|Average|Die maximale Erstellungsrate für ausgehende Datenflüsse (ausgehender Datenverkehr vom virtuellen Computer)|Keine|
|Treffer beim Cachelesevorgang auf Premium-Datenträger|Treffer beim Cachelesevorgang auf Premium-Datenträger (Vorschauversion)|Percent|Average|Treffer beim Cachelesevorgang auf Premium-Datenträger|LUN|
|Fehler beim Cachelesevorgang auf Premium-Datenträger|Fehler beim Cachelesevorgang auf Premium-Datenträger (Vorschauversion)|Percent|Average|Fehler beim Cachelesevorgang auf Premium-Datenträger|LUN|
|Treffer beim Cachelesevorgang auf Premium-Betriebssystemdatenträger|Treffer beim Cachelesevorgang auf Premium-Betriebssystemdatenträger (Vorschauversion)|Percent|Average|Treffer beim Cachelesevorgang auf Premium-Betriebssystemdatenträger|Keine|
|Fehler beim Cachelesevorgang auf Premium-Betriebssystemdatenträger|Fehler beim Cachelesevorgang auf Premium-Betriebssystemdatenträger (Vorschauversion)|Percent|Average|Fehler beim Cachelesevorgang auf Premium-Betriebssystemdatenträger|Keine|
|Eingehender Netzwerkverkehr gesamt|Eingehender Netzwerkverkehr gesamt|Byte|Gesamt|Die Anzahl von Bytes, die von den virtuellen Computern an allen Netzwerkschnittstellen empfangen werden (eingehender Datenverkehr)|Keine|
|Ausgehender Netzwerkverkehr gesamt|Ausgehender Netzwerkverkehr gesamt|Byte|Gesamt|Die Anzahl von Bytes, die von den virtuellen Computern an allen Netzwerkschnittstellen gesendet werden (ausgehender Datenverkehr)|Keine|

## <a name="microsoftcontainerinstancecontainergroups"></a>Microsoft.ContainerInstance/containerGroups

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|CpuUsage|CPU-Auslastung|Anzahl|Average|CPU-Auslastung für alle Kerne in Millicores|containerName|
|MemoryUsage|Speicherauslastung|Byte|Average|Gesamtspeicherauslastung in Byte|containerName|
|NetworkBytesReceivedPerSecond|Empfangene Netzwerkbytes pro Sekunde|Byte|Average|Die pro Sekunde empfangenen Netzwerkbytes.|Keine|
|NetworkBytesTransmittedPerSecond|Übertragene Netzwerkbytes pro Sekunde|Byte|Average|Die pro Sekunde übertragenen Netzwerkbytes|Keine|

## <a name="microsoftcontainerregistryregistries"></a>Microsoft.ContainerRegistry/registries

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|TotalPullCount|Gesamtzahl von Pullvorgängen|Anzahl|Average|Anzahl von Image-Pullvorgängen insgesamt|Keine|
|SuccessfulPullCount|Anzahl erfolgreicher Pullvorgänge|Anzahl|Average|Anzahl erfolgreicher Image-Pullvorgänge|Keine|
|TotalPushCount|Gesamtzahl von Pushvorgängen|Anzahl|Average|Anzahl von Image-Pushvorgängen insgesamt|Keine|
|SuccessfulPushCount|Anzahl erfolgreicher Puchvorgänge|Anzahl|Average|Anzahl erfolgreicher Image-Pushvorgänge|Keine|
|RunDuration|Ausführungsdauer|Millisekunden|Gesamt|Ausführungsdauer in Millisekunden|Keine|


## <a name="microsoftcontainerservicemanagedclusters"></a>Microsoft.ContainerService/managedClusters

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|kube_node_status_allocatable_cpu_cores|Gesamtzahl der verfügbaren CPU-Kerne in einem verwalteten Cluster|Anzahl|Average|Gesamtzahl der verfügbaren CPU-Kerne in einem verwalteten Cluster|Keine|
|kube_node_status_allocatable_memory_bytes|Gesamtzahl des verfügbaren Speicherplatzes in einem verwalteten Cluster|Byte|Average|Gesamtzahl des verfügbaren Speicherplatzes in einem verwalteten Cluster|Keine|
|kube_pod_status_ready|Anzahl der Pods mit dem Status „Bereit“|Anzahl|Average|Anzahl der Pods mit dem Status „Bereit“|namespace, pod|
|kube_node_status_condition|Status für verschiedene Knotenbedingungen|Anzahl|Average|Status für verschiedene Knotenbedingungen|condition, status, status2, node|
|kube_pod_status_phase|Anzahl der Pods nach Phase|Anzahl|Average|Anzahl der Pods nach Phase|phase, namespace, pod|



## <a name="microsoftcustomprovidersresourceproviders"></a>Microsoft.CustomProviders/resourceproviders

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|SuccessfullRequests|Erfolgreiche Anforderungen|Anzahl|Gesamt|Erfolgreiche Anforderungen durch den benutzerdefinierten Anbieter|HttpMethod, CallPath, StatusCode|
|FailedRequests|Anforderungsfehler|Anzahl|Gesamt|Ruft die verfügbaren Protokolle für benutzerdefinierte Ressourcenanbieter ab|HttpMethod, CallPath, StatusCode|

## <a name="microsoftdataboxedgedataboxedgedevices"></a>Microsoft.DataBoxEdge/dataBoxEdgeDevices

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|NICReadThroughput|Lesedurchsatz (Netzwerk)|Bytes pro Sekunde|Average|Der Lesedurchsatz der Netzwerkschnittstelle auf dem Gerät im Berichtszeitraum für alle Volumes im Gateway.|InstanceName|
|NICWriteThroughput|Schreibdurchsatz (Netzwerk)|Bytes pro Sekunde|Average|Der Schreibdurchsatz der Netzwerkschnittstelle auf dem Gerät im Berichtszeitraum für alle Volumes im Gateway.|InstanceName|
|CloudReadThroughputPerShare|Clouddownloaddurchsatz (Freigabe)|Bytes pro Sekunde|Average|Der Downloaddurchsatz in Azure von einer Freigabe während des Berichtszeitraums.|Freigabe|
|CloudUploadThroughputPerShare|Clouduploaddurchsatz (Freigabe)|Bytes pro Sekunde|Average|Der Uploaddurchsatz in Azure von einer Freigabe während des Berichtszeitraums.|Freigabe|
|BytesUploadedToCloudPerShare|Hochgeladene Cloudbytes (Freigabe)|Byte|Average|Die Gesamtzahl von Bytes, die in Azure von einer Freigabe während des Berichtszeitraum hochgeladen wird.|Freigabe|
|TotalCapacity|Gesamtkapazität|Byte|Average|Gesamtkapazität|Keine|
|AvailableCapacity|Verfügbare Kapazität|Byte|Average|Die verfügbare Kapazität in Byte während des Berichtszeitraums.|Keine|
|CloudUploadThroughput|Clouduploaddurchsatz|Bytes pro Sekunde|Average|Der Clouduploaddurchsatz in Azure während des Berichtszeitraums.|Keine|
|CloudReadThroughput|Clouddownloaddurchsatz|Bytes pro Sekunde|Average|Der Clouddownloaddurchsatz in Azure während des Berichtszeitraums.|Keine|
|BytesUploadedToCloud|Hochgeladene Cloudbytes (Gerät)|Byte|Average|Die Gesamtzahl von Bytes, die in Azure von einem Gerät während des Berichtszeitraum hochgeladen wird.|Keine|
|HyperVVirtualProcessorUtilization|Edgecomputing – CPU in Prozent|Percent|Average|Prozentsatz der CPU-Auslastung|InstanceName|
|HyperVMemoryUtilization|Edgecomputing – Arbeitsspeichernutzung|Percent|Average|Größe des genutzten Arbeitsspeichers|InstanceName|


## <a name="microsoftdatafactorydatafactories"></a>Microsoft.DataFactory/datafactories

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|FailedRuns|Fehlerhafte Ausführungen|Anzahl|Gesamt||pipelineName,activityName|
|SuccessfulRuns|Erfolgreiche Ausführungen|Anzahl|Gesamt||pipelineName,activityName|


## <a name="microsoftdatafactoryfactories"></a>Microsoft.DataFactory/factories

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|PipelineFailedRuns|Metriken zu fehlerhaften Pipelineausführungen|Anzahl|Gesamt||FailureType, Name|
|PipelineSucceededRuns|Metriken zu erfolgreichen Pipelineausführungen|Anzahl|Gesamt||FailureType, Name|
|PipelineCancelledRuns|Metriken zu abgebrochenen Pipelineausführungen|Anzahl|Gesamt||FailureType, Name|
|ActivityFailedRuns|Metriken zu fehlerhaften Aktivitätsausführungen|Anzahl|Gesamt||ActivityType, PipelineName, FailureType, Name|
|ActivitySucceededRuns|Metriken zu erfolgreichen Aktivitätsausführungen|Anzahl|Gesamt||ActivityType, PipelineName, FailureType, Name|
|ActivityCancelledRuns|Metriken zu abgebrochenen Aktivitätsausführungen|Anzahl|Gesamt||ActivityType, PipelineName, FailureType, Name|
|TriggerFailedRuns|Metriken zu fehlerhaften Triggerausführungen|Anzahl|Gesamt||Name, FailureType|
|TriggerSucceededRuns|Metriken zu erfolgreichen Triggerausführungen|Anzahl|Gesamt||Name, FailureType|
|TriggerCancelledRuns|Metriken zu abgebrochenen Triggerausführungen|Anzahl|Gesamt||Name, FailureType|
|IntegrationRuntimeCpuPercentage|CPU-Auslastung der Integration Runtime|Percent|Average||IntegrationRuntimeName, NodeName|
|IntegrationRuntimeAvailableMemory|Verfügbarer Speicher für Integration Runtime|Byte|Average||IntegrationRuntimeName, NodeName|
|IntegrationRuntimeAverageTaskPickupDelay|Dauer der Integration Runtime-Warteschlange|Sekunden|Average||IntegrationRuntimeName|
|IntegrationRuntimeQueueLength|Länge der Integration Runtime-Warteschlange|Anzahl|Average||IntegrationRuntimeName|
|IntegrationRuntimeAvailableNodeNumber|Anzahl verfügbarer Knoten für Integration Runtime|Anzahl|Average||IntegrationRuntimeName|
|MaxAllowedResourceCount|Anzahl maximal zulässiger Entitäten|Anzahl|Maximum||Keine|
|MaxAllowedFactorySizeInGbUnits|Maximal zulässige Factorygröße (Einheit: GB)|Anzahl|Maximum||Keine|
|ResourceCount|Gesamtzahl von Entitäten|Anzahl|Maximum||Keine|
|FactorySizeInGbUnits|Gesamtgröße der Factory (Einheit: GB)|Anzahl|Maximum||Keine|

## <a name="microsoftdatalakeanalyticsaccounts"></a>Microsoft.DataLakeAnalytics/accounts

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|JobEndedSuccess|Erfolgreiche Vorgänge|Anzahl|Gesamt|Die Anzahl der erfolgreichen Vorgänge|Keine|
|JobEndedFailure|Fehlgeschlagene Vorgänge|Anzahl|Gesamt|Die Anzahl der fehlgeschlagenen Vorgänge|Keine|
|JobEndedCancelled|Abgebrochene Vorgänge|Anzahl|Gesamt|Die Anzahl der abgebrochenen Vorgänge|Keine|
|JobAUEndedSuccess|Erfolgreiche AU-Zeit|Sekunden|Gesamt|Die Gesamt-AU-Zeit für erfolgreiche Vorgänge|Keine|
|JobAUEndedFailure|Fehlgeschlagene AU-Zeit|Sekunden|Gesamt|Die Gesamt-AU-Zeit für fehlgeschlagene Vorgänge|Keine|
|JobAUEndedCancelled|Abgebrochene AU-Zeit|Sekunden|Gesamt|Die Gesamt-AU-Zeit für abgebrochene Vorgänge|Keine|


## <a name="microsoftdatalakestoreaccounts"></a>Microsoft.DataLakeStore/accounts

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|TotalStorage|Gesamtspeicher|Byte|Maximum|Gesamtmenge der im Konto gespeicherten Daten|Keine|
|DataWritten|Geschriebene Daten|Byte|Gesamt|Gesamtmenge der in das Konto geschriebenen Daten|Keine|
|DataRead|Gelesene Daten|Byte|Gesamt|Gesamtmenge der im Konto gelesenen Daten|Keine|
|WriteRequests|Schreibanforderungen|Anzahl|Gesamt|Anzahl der Datenschreibanforderungen für das Konto|Keine|
|ReadRequests|Leseanforderungen|Anzahl|Gesamt|Anzahl der Datenleseanforderungen für das Konto|Keine|


## <a name="microsoftdbformariadbservers"></a>Microsoft.DBforMariaDB/servers

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|cpu_percent|CPU in Prozent|Percent|Average|CPU in Prozent|Keine|
|memory_percent|Arbeitsspeicher in Prozent|Percent|Average|Arbeitsspeicher in Prozent|Keine|
|io_consumption_percent|E/A in Prozent|Percent|Average|E/A in Prozent|Keine|
|storage_percent|Speicher in Prozent|Percent|Average|Speicher in Prozent|Keine|
|storage_used|Verwendeter Speicher|Byte|Average|Verwendeter Speicher|Keine|
|storage_limit|Speicherbegrenzung|Byte|Maximum|Speicherbegrenzung|Keine|
|serverlog_storage_percent|Serverprotokollspeicher in Prozent|Percent|Average|Serverprotokollspeicher in Prozent|Keine|
|serverlog_storage_usage|Verwendeter Serverprotokollspeicher|Byte|Average|Verwendeter Serverprotokollspeicher|Keine|
|serverlog_storage_limit|Begrenzung des Serverprotokollspeichers|Byte|Average|Begrenzung des Serverprotokollspeichers|Keine|
|active_connections|Die aktiven Verbindungen.|Anzahl|Average|Die aktiven Verbindungen.|Keine|
|connections_failed|Verbindungsfehler|Anzahl|Gesamt|Verbindungsfehler|Keine|
|seconds_behind_master|Replikationsverzögerung in Sekunden|Anzahl|Maximum|Replikationsverzögerung in Sekunden|Keine|
|backup_storage_used|Verwendeter Sicherungsspeicher|Byte|Average|Verwendeter Sicherungsspeicher|Keine|
|network_bytes_egress|Netzwerk ausgehend|Byte|Gesamt|Ausgehender Netzwerkdatenverkehr über aktive Verbindungen|Keine|
|network_bytes_ingress|Netzwerk eingehend|Byte|Gesamt|Eingehender Netzwerkdatenverkehr über aktive Verbindungen|Keine|


## <a name="microsoftdbformysqlservers"></a>Microsoft.DBforMySQL/servers

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|cpu_percent|CPU in Prozent|Percent|Average|CPU in Prozent|Keine|
|memory_percent|Arbeitsspeicher in Prozent|Percent|Average|Arbeitsspeicher in Prozent|Keine|
|io_consumption_percent|E/A in Prozent|Percent|Average|E/A in Prozent|Keine|
|storage_percent|Speicher in Prozent|Percent|Average|Speicher in Prozent|Keine|
|storage_used|Verwendeter Speicher|Byte|Average|Verwendeter Speicher|Keine|
|storage_limit|Speicherbegrenzung|Byte|Maximum|Speicherbegrenzung|Keine|
|serverlog_storage_percent|Serverprotokollspeicher in Prozent|Percent|Average|Serverprotokollspeicher in Prozent|Keine|
|serverlog_storage_usage|Verwendeter Serverprotokollspeicher|Byte|Average|Verwendeter Serverprotokollspeicher|Keine|
|serverlog_storage_limit|Begrenzung des Serverprotokollspeichers|Byte|Maximum|Begrenzung des Serverprotokollspeichers|Keine|
|active_connections|Die aktiven Verbindungen.|Anzahl|Average|Die aktiven Verbindungen.|Keine|
|connections_failed|Verbindungsfehler|Anzahl|Gesamt|Verbindungsfehler|Keine|
|seconds_behind_master|Replikationsverzögerung in Sekunden|Anzahl|Maximum|Replikationsverzögerung in Sekunden|Keine|
|backup_storage_used|Verwendeter Sicherungsspeicher|Byte|Average|Verwendeter Sicherungsspeicher|Keine|
|network_bytes_egress|Netzwerk ausgehend|Byte|Gesamt|Ausgehender Netzwerkdatenverkehr über aktive Verbindungen|Keine|
|network_bytes_ingress|Netzwerk eingehend|Byte|Gesamt|Eingehender Netzwerkdatenverkehr über aktive Verbindungen|Keine|


## <a name="microsoftdbforpostgresqlservers"></a>Microsoft.DBforPostgreSQL/servers

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|cpu_percent|CPU in Prozent|Percent|Average|CPU in Prozent|Keine|
|memory_percent|Arbeitsspeicher in Prozent|Percent|Average|Arbeitsspeicher in Prozent|Keine|
|io_consumption_percent|E/A in Prozent|Percent|Average|E/A in Prozent|Keine|
|storage_percent|Speicher in Prozent|Percent|Average|Speicher in Prozent|Keine|
|storage_used|Verwendeter Speicher|Byte|Average|Verwendeter Speicher|Keine|
|storage_limit|Speicherbegrenzung|Byte|Maximum|Speicherbegrenzung|Keine|
|serverlog_storage_percent|Serverprotokollspeicher in Prozent|Percent|Average|Serverprotokollspeicher in Prozent|Keine|
|serverlog_storage_usage|Verwendeter Serverprotokollspeicher|Byte|Average|Verwendeter Serverprotokollspeicher|Keine|
|serverlog_storage_limit|Begrenzung des Serverprotokollspeichers|Byte|Maximum|Begrenzung des Serverprotokollspeichers|Keine|
|active_connections|Die aktiven Verbindungen.|Anzahl|Average|Die aktiven Verbindungen.|Keine|
|connections_failed|Verbindungsfehler|Anzahl|Gesamt|Verbindungsfehler|Keine|
|backup_storage_used|Verwendeter Sicherungsspeicher|Byte|Average|Verwendeter Sicherungsspeicher|Keine|
|network_bytes_egress|Netzwerk ausgehend|Byte|Gesamt|Ausgehender Netzwerkdatenverkehr über aktive Verbindungen|Keine|
|network_bytes_ingress|Netzwerk eingehend|Byte|Gesamt|Eingehender Netzwerkdatenverkehr über aktive Verbindungen|Keine|
|pg_replica_log_delay_in_seconds|Replikatverzögerung|Sekunden|Maximum|Replikatverzögerung in Sekunden|Keine|
|pg_replica_log_delay_in_bytes|Maximale Verzögerung zwischen Replikaten|Byte|Maximum|Verzögerung in Byte beim Replikat mit der größten Verzögerung|Keine|


## <a name="microsoftdbforpostgresqlserversv2"></a>Microsoft.DBforPostgreSQL/serversv2

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|cpu_percent|CPU in Prozent|Percent|Average|CPU in Prozent|Keine|
|memory_percent|Arbeitsspeicher in Prozent|Percent|Average|Arbeitsspeicher in Prozent|Keine|
|iops|IOPS|Anzahl|Average|E/A-Vorgänge pro Sekunde|Keine|
|storage_percent|Speicher in Prozent|Percent|Average|Speicher in Prozent|Keine|
|storage_used|Verwendeter Speicher|Byte|Average|Verwendeter Speicher|Keine|
|active_connections|Die aktiven Verbindungen.|Anzahl|Average|Die aktiven Verbindungen.|Keine|
|network_bytes_egress|Netzwerk ausgehend|Byte|Gesamt|Ausgehender Netzwerkdatenverkehr über aktive Verbindungen|Keine|
|network_bytes_ingress|Netzwerk eingehend|Byte|Gesamt|Eingehender Netzwerkdatenverkehr über aktive Verbindungen|Keine|





## <a name="microsoftdevicesiothubs"></a>Microsoft.Devices/IotHubs

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|d2c.telemetry.Ingress.allProtocol|Telemetry message send attempts (Sendeversuche für Telemetrienachrichten)|Anzahl|Gesamt|Anzahl von Telemetrienachrichten vom Gerät an die Cloud, die an Ihren IoT Hub gesendet werden sollten|Keine|
|d2c.telemetry.ingress.success|Telemetry messages sent (Gesendete Telemetrienachrichten)|Anzahl|Gesamt|Anzahl von Telemetrienachrichten vom Gerät an die Cloud, die erfolgreich an Ihren IoT Hub gesendet wurden|Keine|
|c2d.commands.egress.complete.success|Abgeschlossene C2D-Nachrichtenübermittlungen|Anzahl|Gesamt|Anzahl von Cloud-zu-Gerät-Nachrichtenübermittlungen, die vom Gerät erfolgreich abgeschlossen wurden|Keine|
|c2d.commands.egress.abandon.success|Abgebrochene C2D-Nachrichten|Anzahl|Gesamt|Anzahl von Cloud-zu-Gerät-Nachrichten, die vom Gerät abgebrochen wurden|Keine|
|c2d.commands.egress.reject.success|Abgelehnte C2D-Nachrichten|Anzahl|Gesamt|Anzahl von Cloud-zu-Gerät-Nachrichten, die vom Gerät abgelehnt wurden|Keine|
|C2DMessagesExpired|Abgelaufene C2D-Nachrichten (Vorschau)|Anzahl|Gesamt|Anzahl von abgelaufenen Cloud-zu-Gerät-Nachrichten|Keine|
|devices.totalDevices|Geräte gesamt (veraltet)|Anzahl|Gesamt|Die Anzahl von Geräten, die beim IoT Hub registriert sind|Keine|
|devices.connectedDevices.allProtocol|Verbundene Geräte (veraltet) |Anzahl|Gesamt|Die Anzahl von Geräten, die mit dem IoT Hub verbunden sind|Keine|
|d2c.telemetry.egress.success|Routing: Übermittelte Telemetrienachrichten|Anzahl|Gesamt|Die Anzahl der erfolgreichen Nachrichtenübermittlungen an alle Endpunkte über das IoT Hub-Routing Wenn eine Nachricht an mehrere Endpunkte weitergeleitet wird, erhöht sich dieser Wert für jede erfolgreiche Übermittlung um eins Wenn eine Nachricht mehrmals an denselben Endpunkt übermittelt wird, erhöht sich dieser Wert für jede erfolgreiche Übermittlung um eins|Keine|
|d2c.telemetry.egress.dropped|Routing: Verworfene Telemetrienachrichten |Anzahl|Gesamt|Die Anzahl der Nachrichten, die vom IoT Hub-Routing aufgrund von inaktiven Endpunkten gelöscht wurden. Dieser Wert zählt nicht die Nachrichten, die an die Fallbackroute übermittelt werden, da gelöschte Nachrichten dort nicht übermittelt werden.|Keine|
|d2c.telemetry.egress.orphaned|Routing: Verwaiste Telemetrienachrichten |Anzahl|Gesamt|Die Häufigkeit, mit der Nachrichten durch das IoT Hub-Routing verwaist wurden, da sie mit keinen Routingregeln (einschließlich der Fallbackregel) übereinstimmten. |Keine|
|d2c.telemetry.egress.invalid|Routing: Nicht kompatible Telemetrienachrichten|Anzahl|Gesamt|Die Häufigkeit, mit der das IoT Hub-Routing Nachrichten aufgrund einer Inkompatibilität mit dem Endpunkt nicht übermitteln konnte. Dieser Wert umfasst keine Wiederholungen.|Keine|
|d2c.telemetry.egress.fallback|Routing: An den Fallback übermittelte Nachrichten|Anzahl|Gesamt|Die Häufigkeit, mit der das IoT Hub-Routing Nachrichten an den mit der Fallbackroute verbundenen Endpunkt übermittelt hat.|Keine|
|d2c.endpoints.egress.eventHubs|Routing: An Event Hub übermittelte Nachrichten|Anzahl|Gesamt|Die Häufigkeit, mit der das IoT Hub-Routing erfolgreich Nachrichten an Event Hub-Endpunkte übermittelt hat.|Keine|
|d2c.endpoints.latency.eventHubs|Routing: Nachrichtenwartezeit für Event Hub|Millisekunden|Average|Durchschnittliche Wartezeit (Millisekunden) zwischen dem Eingang der Nachricht beim IoT Hub und dem Eingang der Nachricht bei einem Event Hub-Endpunkt.|Keine|
|d2c.endpoints.egress.serviceBusQueues|Routing: An Service Bus-Warteschlange übermittelte Nachrichten|Anzahl|Gesamt|Die Häufigkeit, mit der das IoT Hub-Routing erfolgreich Nachrichten an Service Bus-Warteschlangenendpunkte übermittelt hat.|Keine|
|d2c.endpoints.latency.serviceBusQueues|Routing: Nachrichtenwartezeit für Service Bus-Warteschlange|Millisekunden|Average|Durchschnittliche Wartezeit (Millisekunden) zwischen dem Eingang der Nachricht beim IoT Hub und dem Eingang der Nachricht bei einem Service Bus-Warteschlangenendpunkt.|Keine|
|d2c.endpoints.egress.serviceBusTopics|Routing: An Service Bus-Thema übermittelte Nachrichten|Anzahl|Gesamt|Die Häufigkeit, mit der das IoT Hub-Routing erfolgreich Nachrichten an Service Bus-Themaendpunkte übermittelt hat.|Keine|
|d2c.endpoints.latency.serviceBusTopics|Routing: Nachrichtenwartezeit für Service Bus-Thema|Millisekunden|Average|Durchschnittliche Wartezeit (Millisekunden) zwischen dem Eingang der Nachricht beim IoT Hub und dem Eingang der Nachricht bei einem Service Bus-Themaendpunkt.|Keine|
|d2c.endpoints.egress.builtIn.events|Routing: An Nachrichten/Ereignisse übermittelte Nachrichten|Anzahl|Gesamt|Die Häufigkeit, mit der das IoT Hub-Routing erfolgreich Nachrichten an den integrierter Endpunkt (Nachrichten/Ereignisse) übermittelt hat.|Keine|
|d2c.endpoints.latency.builtIn.events|Routing: Nachrichtenwartezeit für Nachrichten/Ereignisse|Millisekunden|Average|Durchschnittliche Wartezeit (Millisekunden) zwischen dem Eingang der Nachricht beim IoT Hub und dem Eingang der Telemetrienachricht beim integrierten Endpunkt (Nachrichten/Ereignisse).|Keine|
|d2c.endpoints.egress.storage|Routing: An den Speicher übermittelte Nachrichten|Anzahl|Gesamt|Die Häufigkeit, mit der das IoT Hub-Routing erfolgreich Nachrichten an Speicherendpunkte übermittelt hat.|Keine|
|d2c.endpoints.latency.storage|Routing: Nachrichtenwartezeit für Speicher|Millisekunden|Average|Durchschnittliche Wartezeit (Millisekunden) zwischen dem Eingang der Nachricht beim IoT Hub und dem Eingang der Nachricht bei einem Speicherendpunkt.|Keine|
|d2c.endpoints.egress.storage.bytes|Routing: An den Speicher übermittelte Daten|Byte|Gesamt|Die Datenmenge (Bytes), die das IoT Hub-Routing an die Speicherendpunkte übermittelt.|Keine|
|d2c.endpoints.egress.storage.blobs|Routing: An den Speicher übermittelte Blobs|Anzahl|Gesamt|Die Häufigkeit, mit der das IoT Hub-Routing Blobs an Speicherendpunkte übermittelt hat.|Keine|
|EventGridDeliveries|Event Grid-Übermittlungen (Vorschau)|Anzahl|Gesamt|Die Anzahl von IoT Hub-Ereignissen, die in Event Grid veröffentlicht werden. Verwenden Sie die Dimension „Result“ für die Anzahl der erfolgreichen und fehlerhaften Anforderungen. Die „EventType“-Dimension zeigt den Typ des Ereignisses an(https://aka.ms/ioteventgrid).|ResourceId,Result,EventType|
|EventGridLatency|Event Grid-Wartezeit (Vorschau)|Millisekunden|Average|Die durchschnittliche Wartezeit (in Millisekunden) für den Zeitraum zwischen der Generierung des Iot Hub-Ereignisses und der Veröffentlichung des Ereignisses in Event Grid. Diese Zahl ist ein Durchschnittswert für alle Ereignistypen. Verwenden Sie die Dimension „EventType“, um die Wartezeit für einen bestimmten Ereignistyp anzuzeigen.|ResourceId,EventType|
|d2c.twin.read.success|Successful twin reads from devices (Erfolgreiche Zwillingslesevorgänge von Geräten)|Anzahl|Gesamt|Gibt die Anzahl von erfolgreichen Zwillingslesevorgängen an, die vom Gerät initiiert wurden.|Keine|
|d2c.twin.read.failure|Failed twin reads from devices (Nicht erfolgreiche Zwillingslesevorgänge von Geräten)|Anzahl|Gesamt|Gibt die Anzahl von nicht erfolgreichen Zwillingslesevorgängen an, die vom Gerät initiiert wurden.|Keine|
|d2c.twin.read.size|Response size of twin reads from devices (Antwortgröße von Zwillingslesevorgängen von Geräten)|Byte|Average|Durchschnitts-, Minimal- und Maximalwert für alle erfolgreichen Zwillingslesevorgänge, die vom Gerät initiiert wurden.|Keine|
|d2c.twin.update.success|Successful twin updates from devices (Erfolgreiche Zwillingsaktualisierungen von Geräten)|Anzahl|Gesamt|Gibt die Anzahl von erfolgreichen Zwillingsaktualisierungen an, die vom Gerät initiiert wurden.|Keine|
|d2c.twin.update.failure|Failed twin updates from devices (Nicht erfolgreiche Zwillingsaktualisierungen von Geräten)|Anzahl|Gesamt|Gibt die Anzahl von nicht erfolgreichen Zwillingsaktualisierungen an, die vom Gerät initiiert wurden.|Keine|
|d2c.twin.update.size|Size of twin updates from devices (Größe der Zwillingsaktualisierungen von Geräten)|Byte|Average|Durchschnitts-, Minimal- und Maximalgröße für alle erfolgreichen Zwillingsaktualisierungen, die vom Gerät initiiert wurden.|Keine|
|c2d.methods.success|Successful direct method invocations (Erfolgreiche direkte Methodenaufrufvorgänge)|Anzahl|Gesamt|Gibt an, wie viele direkte Methodenaufrufe erfolgreich durchgeführt wurden.|Keine|
|c2d.methods.failure|Failed direct method invocations (Nicht erfolgreiche direkte Methodenaufrufe)|Anzahl|Gesamt|Gibt an, wie viele direkte Methodenaufrufe nicht erfolgreich waren.|Keine|
|c2d.methods.requestSize|Request size of direct method invocations (Anforderungsgröße von direkten Methodenaufrufen)|Byte|Average|Gibt den Durchschnitts-, Minimal- und Maximalwert für alle erfolgreichen direkten Methodenaufrufe an.|Keine|
|c2d.methods.responseSize|Response size of direct method invocations (Antwortgröße von direkten Methodenaufrufen)|Byte|Average|Gibt den Durchschnitts-, Minimal- und Maximalwert für alle erfolgreichen Antworten für die direkte Methode an.|Keine|
|c2d.twin.read.success|Successful twin reads from back end (Erfolgreiche Zwillingslesevorgänge vom Back-End)|Anzahl|Gesamt|Gibt die Anzahl von erfolgreichen Zwillingslesevorgängen an, die vom Back-End initiiert wurden.|Keine|
|c2d.twin.read.failure|Failed twin reads from back end (Nicht erfolgreiche Zwillingslesevorgänge vom Back-End)|Anzahl|Gesamt|Gibt die Anzahl von nicht erfolgreichen Zwillingslesevorgängen an, die vom Back-End initiiert wurden.|Keine|
|c2d.twin.read.size|Response size of twin reads from back end (Antwortgröße von Zwillingslesevorgängen vom Back-End)|Byte|Average|Durchschnitts-, Minimal- und Maximalwert für alle erfolgreichen Zwillingslesevorgänge, die vom Back-End initiiert wurden.|Keine|
|c2d.twin.update.success|Successful twin updates from back end (Erfolgreiche Zwillingsaktualisierungen vom Back-End)|Anzahl|Gesamt|Gibt die Anzahl von erfolgreichen Zwillingsaktualisierungen an, die vom Back-End initiiert wurden.|Keine|
|c2d.twin.update.failure|Failed twin updates from back end (Nicht erfolgreiche Zwillingsaktualisierungen vom Back-End)|Anzahl|Gesamt|Gibt die Anzahl von nicht erfolgreichen Zwillingsaktualisierungen an, die vom Back-End initiiert wurden.|Keine|
|c2d.twin.update.size|Size of twin updates from back end (Größe der Zwillingsaktualisierungen vom Back-End)|Byte|Average|Durchschnitts-, Minimal- und Maximalgröße für alle erfolgreichen Zwillingsaktualisierungen, die vom Back-End initiiert wurden.|Keine|
|twinQueries.success|Successful twin queries (Erfolgreiche Zwillingsabfragen)|Anzahl|Gesamt|Gibt an, wie viele erfolgreiche Zwillingsabfragen durchgeführt wurden.|Keine|
|twinQueries.failure|Failed twin queries (Nicht erfolgreiche Zwillingsabfragen)|Anzahl|Gesamt|Gibt an, wie viele nicht erfolgreiche Zwillingsabfragen durchgeführt wurden.|Keine|
|twinQueries.resultSize|Twin queries result size (Ergebnisgröße von Zwillingsabfragen)|Byte|Average|Durchschnitts-, Minimal- und Maximalwert der Ergebnisgröße aller erfolgreichen Zwillingsabfragen.|Keine|
|jobs.createTwinUpdateJob.success|Successful creations of twin update jobs (Erfolgreiche Erstellungen von Zwillingsaktualisierungsaufträgen)|Anzahl|Gesamt|Gibt die Anzahl von allen erfolgreichen Erstellungen von Zwillingsaktualisierungsaufträgen an.|Keine|
|jobs.createTwinUpdateJob.failure|Failed creations of twin update jobs (Nicht erfolgreiche Erstellungen von Zwillingsaktualisierungsaufträgen)|Anzahl|Gesamt|Gibt die Anzahl von allen nicht erfolgreichen Erstellungen von Zwillingsaktualisierungsaufträgen an.|Keine|
|jobs.createDirectMethodJob.success|Successful creations of method invocation jobs (Erfolgreiche Erstellungen von Methodenaufrufaufträgen)|Anzahl|Gesamt|Gibt die Anzahl von erfolgreichen Erstellungen von Aufträgen für direkte Methodenaufrufe an.|Keine|
|jobs.createDirectMethodJob.failure|Failed creations of method invocation jobs (Nicht erfolgreiche Erstellungen von Methodenaufrufaufträgen)|Anzahl|Gesamt|Gibt die Anzahl von nicht erfolgreichen Erstellungen von Aufträgen für direkte Methodenaufrufe an.|Keine|
|jobs.listJobs.success|Successful calls to list jobs (Erfolgreiche Aufrufe von Auflistungsaufträgen)|Anzahl|Gesamt|Gibt an, wie viele erfolgreiche Aufrufe von Auflistungsaufträgen durchgeführt wurden.|Keine|
|jobs.listJobs.failure|Failed calls to list jobs (Nicht erfolgreiche Aufrufe von Auflistungsaufträgen)|Anzahl|Gesamt|Gibt an, wie viele nicht erfolgreiche Aufrufe von Auflistungsaufträgen durchgeführt wurden.|Keine|
|jobs.cancelJob.success|Successful job cancellations (Erfolgreiche Auftragsabbrüche)|Anzahl|Gesamt|Gibt an, wie viele erfolgreiche Aufrufe von Auftragsabbrüchen durchgeführt wurden.|Keine|
|jobs.cancelJob.failure|Failed job cancellations (Nicht erfolgreiche Auftragsabbrüche)|Anzahl|Gesamt|Gibt an, wie viele nicht erfolgreiche Aufrufe von Auftragsabbrüchen durchgeführt wurden.|Keine|
|jobs.queryJobs.success|Successful job queries (Erfolgreiche Auftragsabfragen)|Anzahl|Gesamt|Gibt an, wie viele erfolgreiche Aufrufe von Abfrageaufträgen durchgeführt wurden.|Keine|
|jobs.queryJobs.failure|Failed job queries (Nicht erfolgreiche Auftragsabfragen)|Anzahl|Gesamt|Gibt an, wie viele nicht erfolgreiche Aufrufe von Abfrageaufträgen durchgeführt wurden.|Keine|
|jobs.completed|Abgeschlossene Aufträge|Anzahl|Gesamt|Gibt die Anzahl von abgeschlossenen Aufträgen an.|Keine|
|jobs.failed|Fehlerhafte Aufträge|Anzahl|Gesamt|Gibt die Anzahl aller fehlerhaften Aufträge an.|Keine|
|d2c.telemetry.ingress.sendThrottle|Anzahl von Drosselungsfehlern|Anzahl|Gesamt|Anzahl von Drosselungsfehlern aufgrund von Drosselungen des Gerätedurchsatzes|Keine|
|dailyMessageQuotaUsed|Gesamtzahl verwendeter Nachrichten|Anzahl|Average|Gesamtzahl der heute verwendeten Nachrichten|Keine|
|deviceDataUsage|Gerätedatennutzung gesamt|Byte|Gesamt|Zu und von allen mit IotHub verbundenen Geräten übertragene Bytes|Keine|
|deviceDataUsageV2|Gerätedatennutzung gesamt (Vorschau)|Byte|Gesamt|Zu und von allen mit IotHub verbundenen Geräten übertragene Bytes|Keine|
|totalDeviceCount|Geräte gesamt (Vorschau)|Anzahl|Average|Die Anzahl von Geräten, die beim IoT Hub registriert sind|Keine|
|connectedDeviceCount|Verbundene Geräte (Vorschau)|Anzahl|Average|Die Anzahl von Geräten, die mit dem IoT Hub verbunden sind|Keine|
|Konfigurationen|Konfigurationsmetriken|Anzahl|Gesamt|Metriken für Konfigurationsvorgänge|Keine|


## <a name="microsoftdevicesprovisioningservices"></a>Microsoft.Devices/provisioningServices

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|RegistrationAttempts|Registrierungsversuche|Anzahl|Gesamt|Anzahl von versuchten Geräteregistrierungen|ProvisioningServiceName,IotHubName,Status|
|DeviceAssignments|Zugewiesene Geräte|Anzahl|Gesamt|Anzahl von Geräten, die einem IoT-Hub zugewiesen sind|ProvisioningServiceName,IotHubName|
|AttestationAttempts|Nachweisversuche|Anzahl|Gesamt|Anzahl von versuchten Gerätenachweisen|ProvisioningServiceName,Status,Protocol|


## <a name="microsoftdocumentdbdatabaseaccounts"></a>Microsoft.DocumentDB/databaseAccounts

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|AddRegion|Region hinzugefügt|Anzahl|Anzahl|Region hinzugefügt|Region|
|AvailableStorage|Verfügbarer Speicher|Byte|Gesamt|Gemeldeter Gesamtspeicher mit 5-Minuten-Granularität|CollectionName, DatabaseName, Region|
|CassandraConnectionClosures|Abschluss von Cassandra-Verbindungen|Anzahl|Gesamt|Anzahl von Cassandra-Verbindungen, die geschlossen wurden, gemeldet mit einer Granularität von einer Minute|APIType, Region, ClosureReason|
|CassandraRequestCharges|Gebühren für Cassandra-Anforderungen|Anzahl|Gesamt|Genutzte Anforderungseinheiten für gesendete Cassandra-Anforderungen|APIType, DatabaseName, CollectionName, Region, OperationType, ResourceType|
|CassandraRequests|Cassandra-Anforderungen|Anzahl|Anzahl|Anzahl ausgeführter Cassandra-Anforderungen|APIType, DatabaseName, CollectionName, Region, OperationType, ResourceType, ErrorCode|
|CreateAccount|Konto erstellt|Anzahl|Anzahl|Konto erstellt|Keine|
|DataUsage|Datennutzung|Byte|Gesamt|Gemeldete Gesamtdatennutzung mit 5-Minuten-Granularität|CollectionName, DatabaseName, Region|
|DeleteAccount|Konto gelöscht|Anzahl|Anzahl|Konto gelöscht|Keine|
|DocumentCount|Dokumentanzahl|Anzahl|Gesamt|Gemeldete Gesamtdokumentanzahl mit 5-Minuten-Granularität|CollectionName, DatabaseName, Region|
|DocumentQuota|Dokumentenkontingent|Byte|Gesamt|Gemeldetes Gesamtspeicherkontingent mit 5-Minuten-Granularität|CollectionName, DatabaseName, Region|
|IndexUsage|Indexnutzung|Byte|Gesamt|Gemeldete Gesamtindexnutzung mit 5-Minuten-Granularität|CollectionName, DatabaseName, Region|
|MetadataRequests|Anforderungen von Metadaten|Anzahl|Anzahl|Anzahl der Metadatenanforderungen. Cosmos DB unterhält eine Sammlung von Systemmetadaten für jedes Konto, wodurch Sie Sammlungen, Datenbanken usw. und deren Konfigurationen ohne anfallende Kosten auflisten können.|DatabaseName, CollectionName, Region, StatusCode, Role|
|MongoRequestCharge|Kosten der Mongo-Anforderung|Anzahl|Gesamt|Verbrauchte Mongo-Anforderungseinheiten|DatabaseName, CollectionName, Region, CommandName, ErrorCode|
|MongoRequests|Mongo-Anforderungen|Anzahl|Anzahl|Anzahl der ausgegebenen Mongo-Anforderungen|DatabaseName, CollectionName, Region, CommandName, ErrorCode|
|MongoRequestsCount|Mongo-Anforderungsrate|Anzahl pro Sekunde|Average|Anzahl von Mongo-Anforderungen pro Sekunde|DatabaseName, CollectionName, Region, CommandName, ErrorCode|
|MongoRequestsDelete|Mongo Delete Request Rate (Rate von Mongo-Löschanforderung)|Anzahl pro Sekunde|Average|Mongo-Löschanforderungen pro Sekunde|DatabaseName, CollectionName, Region, CommandName, ErrorCode|
|MongoRequestsInsert|Mongo Insert Request Rate (Rate von Mongo-Einfügeanforderung)|Anzahl pro Sekunde|Average|Anzahl von Mongo-Einfügungen pro Sekunde|DatabaseName, CollectionName, Region, CommandName, ErrorCode|
|MongoRequestsQuery|Mongo Query Request Rate (Rate von Mongo-Abfrageanforderung)|Anzahl pro Sekunde|Average|Mongo-Abfrageanforderungen pro Sekunde|DatabaseName, CollectionName, Region, CommandName, ErrorCode|
|MongoRequestsUpdate|Mongo-Aktualisierungsanforderungsrate|Anzahl pro Sekunde|Average|Mongo-Aktualisierungsanforderungen pro Sekunde|DatabaseName, CollectionName, Region, CommandName, ErrorCode|
|ProvisionedThroughput|Bereitgestellter Durchsatz|Anzahl|Maximum|Bereitgestellter Durchsatz|DatabaseName, CollectionName|
|RegionFailover|Failover der Region|Anzahl|Anzahl|Failover der Region|Keine|
|RemoveRegion|Region entfernt|Anzahl|Anzahl|Region entfernt|Region|
|ReplicationLatency|P99-Replikationswartezeit|Millisekunden|Average|P99-Replikationswartezeit für Quell- und Zielregionen für geofähiges Konto|SourceRegion, TargetRegion|
|ServiceAvailability|Dienstverfügbarkeit|Percent|Average|Verfügbarkeit der Kontoanforderungen mit einer Granularität von einer Stunde, einem Tag oder einem Monat|Keine|
|TotalRequestUnits|Anforderungseinheiten gesamt|Anzahl|Gesamt|Verbrauchte Anforderungseinheiten|DatabaseName, CollectionName, Region, StatusCode, OperationType|
|TotalRequests|Anzahl von Anforderungen|Anzahl|Anzahl|Anzahl von gesendeten Anforderungen|DatabaseName, CollectionName, Region, StatusCode, OperationType|
|UpdateAccountKeys|Kontoschlüssel aktualisiert|Anzahl|Anzahl|Kontoschlüssel aktualisiert|KeyType|
|UpdateAccountNetworkSettings|Netzwerkeinstellungen für Konto aktualisiert|Anzahl|Anzahl|Netzwerkeinstellungen für Konto aktualisiert|Keine|
|UpdateAccountReplicationSettings|Replikationseinstellungen für Konto aktualisiert|Anzahl|Anzahl|Replikationseinstellungen für Konto aktualisiert|Keine|



## <a name="microsoftenterpriseknowledgegraphservices"></a>Microsoft.EnterpriseKnowledgeGraph/services

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|TransactionCount|Anzahl von Transaktionen|Anzahl|Anzahl|Transaktionsanzahl insgesamt|TransactionCount|
|SuccessCount|Anzahl von Erfolgen|Anzahl|Anzahl|Anzahl erfolgreicher Transaktionen|SuccessCount|
|FailureCount|Anzahl von Fehlern|Anzahl|Anzahl|Anzahl Transaktionsfehler|FailureCount|
|SuccessLatency|Latenz bei Erfolg|Millisekunden|Average|Latenz von erfolgreichen Transaktionen|SuccessCount|

## <a name="microsofteventgriddomains"></a>Microsoft.EventGrid/domains

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|PublishSuccessCount|Veröffentlichte Ereignisse|Anzahl|Gesamt|Gesamtanzahl der zu diesem Thema veröffentlichten Ereignisse|Thema|
|PublishFailCount|Ereignisse mit Veröffentlichungsfehler|Anzahl|Gesamt|Gesamtanzahl der zu diesem Thema fehlerhaft veröffentlichten Ereignisse|Topic, ErrorType, Error|
|PublishSuccessLatencyInMs|Latenz erfolgreicher Veröffentlichungen|Millisekunden|Gesamt|Latenz erfolgreicher Veröffentlichungen in Millisekunden|Keine|
|MatchedEventCount|Übereinstimmende Ereignisse|Anzahl|Gesamt|Gesamtzahl der Ereignisse, die diesem Ereignisabonnement zugeordnet sind|Topic, EventSubscriptionName, DomainEventSubscriptionName|
|DeliveryAttemptFailCount|Ereignisse mit Übermittlungsfehler|Anzahl|Gesamt|Gesamtzahl der Ereignisse, die nicht an dieses Ereignisabonnement übermittelt werden konnten|Topic,EventSubscriptionName,DomainEventSubscriptionName,Error,ErrorType|
|DeliverySuccessCount|Übermittelte Ereignisse|Anzahl|Gesamt|Gesamtzahl der Ereignisse, die an dieses Ereignisabonnement übermittelt wurden|Topic, EventSubscriptionName, DomainEventSubscriptionName|
|DestinationProcessingDurationInMs|Zielverarbeitungsdauer|Millisekunden|Average|Zielverarbeitungsdauer in Millisekunden|Topic, EventSubscriptionName, DomainEventSubscriptionName|
|DroppedEventCount|Gelöschte Ereignisse|Anzahl|Gesamt|Gesamtanzahl der gelöschten Ereignisse, die mit diesem Ereignisabonnement übereinstimmen|Topic, EventSubscriptionName, DomainEventSubscriptionName, DropReason|
|DeadLetteredCount|Unzustellbare Ereignisse|Anzahl|Gesamt|Gesamtanzahl der unzustellbaren Ereignisse, die mit diesem Ereignisabonnement übereinstimmen|Topic,EventSubscriptionName,DomainEventSubscriptionName,DeadLetterReason|

## <a name="microsofteventgridtopics"></a>Microsoft.EventGrid/topics

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|PublishSuccessCount|Veröffentlichte Ereignisse|Anzahl|Gesamt|Gesamtanzahl der zu diesem Thema veröffentlichten Ereignisse|Keine|
|PublishFailCount|Ereignisse mit Veröffentlichungsfehler|Anzahl|Gesamt|Gesamtanzahl der zu diesem Thema fehlerhaft veröffentlichten Ereignisse|ErrorType, Error|
|UnmatchedEventCount|Ereignisse ohne Übereinstimmung|Anzahl|Gesamt|Gesamtzahl der Ereignisse, die mit keinem der Ereignisabonnements für dieses Thema übereinstimmen|Keine|
|PublishSuccessLatencyInMs|Latenz erfolgreicher Veröffentlichungen|Millisekunden|Gesamt|Latenz erfolgreicher Veröffentlichungen in Millisekunden|Keine|
|MatchedEventCount|Übereinstimmende Ereignisse|Anzahl|Gesamt|Gesamtzahl der Ereignisse, die diesem Ereignisabonnement zugeordnet sind|EventSubscriptionName|
|DeliveryAttemptFailCount|Ereignisse mit Übermittlungsfehler|Anzahl|Gesamt|Gesamtzahl der Ereignisse, die nicht an dieses Ereignisabonnement übermittelt werden konnten|Error,ErrorType,EventSubscriptionName|
|DeliverySuccessCount|Übermittelte Ereignisse|Anzahl|Gesamt|Gesamtzahl der Ereignisse, die an dieses Ereignisabonnement übermittelt wurden|EventSubscriptionName|
|DestinationProcessingDurationInMs|Zielverarbeitungsdauer|Millisekunden|Average|Zielverarbeitungsdauer in Millisekunden|EventSubscriptionName|
|DroppedEventCount|Gelöschte Ereignisse|Anzahl|Gesamt|Gesamtanzahl der gelöschten Ereignisse, die mit diesem Ereignisabonnement übereinstimmen|DropReason, EventSubscriptionName|
|DeadLetteredCount|Unzustellbare Ereignisse|Anzahl|Gesamt|Gesamtanzahl der unzustellbaren Ereignisse, die mit diesem Ereignisabonnement übereinstimmen|DeadLetterReason,EventSubscriptionName|

## <a name="microsofteventgrideventsubscriptions"></a>Microsoft.EventGrid/eventSubscriptions

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|MatchedEventCount|Übereinstimmende Ereignisse|Anzahl|Gesamt|Gesamtzahl der Ereignisse, die diesem Ereignisabonnement zugeordnet sind|Keine|
|DeliveryAttemptFailCount|Ereignisse mit Übermittlungsfehler|Anzahl|Gesamt|Gesamtzahl der Ereignisse, die nicht an dieses Ereignisabonnement übermittelt werden konnten|Error, ErrorType|
|DeliverySuccessCount|Übermittelte Ereignisse|Anzahl|Gesamt|Gesamtzahl der Ereignisse, die an dieses Ereignisabonnement übermittelt wurden|Keine|
|DestinationProcessingDurationInMs|Zielverarbeitungsdauer|Millisekunden|Average|Zielverarbeitungsdauer in Millisekunden|Keine|
|DroppedEventCount|Gelöschte Ereignisse|Anzahl|Gesamt|Gesamtanzahl der gelöschten Ereignisse, die mit diesem Ereignisabonnement übereinstimmen|DropReason|
|DeadLetteredCount|Unzustellbare Ereignisse|Anzahl|Gesamt|Gesamtanzahl der unzustellbaren Ereignisse, die mit diesem Ereignisabonnement übereinstimmen|DeadLetterReason|

## <a name="microsofteventgridextensiontopics"></a>Microsoft.EventGrid/extensionTopics

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|PublishSuccessCount|Veröffentlichte Ereignisse|Anzahl|Gesamt|Gesamtanzahl der zu diesem Thema veröffentlichten Ereignisse|Keine|
|PublishFailCount|Ereignisse mit Veröffentlichungsfehler|Anzahl|Gesamt|Gesamtanzahl der zu diesem Thema fehlerhaft veröffentlichten Ereignisse|ErrorType, Error|
|UnmatchedEventCount|Ereignisse ohne Übereinstimmung|Anzahl|Gesamt|Gesamtzahl der Ereignisse, die mit keinem der Ereignisabonnements für dieses Thema übereinstimmen|Keine|
|PublishSuccessLatencyInMs|Latenz erfolgreicher Veröffentlichungen|Millisekunden|Gesamt|Latenz erfolgreicher Veröffentlichungen in Millisekunden|Keine|



## <a name="microsofteventhubnamespaces"></a>Microsoft.EventHub/namespaces

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|SuccessfulRequests|Erfolgreiche Anforderungen|Anzahl|Gesamt|Erfolgreiche Anforderungen für Microsoft.EventHub.|EntityName, OperationResult|
|ServerErrors|Serverfehler.|Anzahl|Gesamt|Serverfehler für Microsoft.EventHub.|EntityName, OperationResult|
|UserErrors|Benutzerfehler.|Anzahl|Gesamt|Benutzerfehler für Microsoft.EventHub.|EntityName, OperationResult|
|QuotaExceededErrors|Fehler aufgrund von Kontingentüberschreitung.|Anzahl|Gesamt|Fehler aufgrund von Kontingentüberschreitung für Microsoft.EventHub.|EntityName, OperationResult|
|ThrottledRequests|Gedrosselte Anforderungen.|Anzahl|Gesamt|Gedrosselte Anforderungen für Microsoft.EventHub.|EntityName, OperationResult|
|IncomingRequests|Eingehende Anforderungen|Anzahl|Gesamt|Eingehende Anforderungen für Microsoft.EventHub.|EntityName|
|IncomingMessages|Eingehende Nachrichten|Anzahl|Gesamt|Eingehende Nachrichten für Microsoft.EventHub.|EntityName|
|OutgoingMessages|Ausgehende Nachrichten|Anzahl|Gesamt|Ausgehende Nachrichten für Microsoft.EventHub.|EntityName|
|IncomingBytes|Eingehende Bytes.|Byte|Gesamt|Eingehende Bytes für Microsoft.EventHub.|EntityName|
|OutgoingBytes|Ausgehende Bytes.|Byte|Gesamt|Ausgehende Bytes für Microsoft.EventHub.|EntityName|
|ActiveConnections|ActiveConnections|Anzahl|Average|Aktive Verbindungen gesamt für Microsoft.EventHub.|Keine|
|ConnectionsOpened|Geöffnete Verbindungen.|Anzahl|Average|Geöffnete Verbindungen für Microsoft.EventHub.|EntityName|
|ConnectionsClosed|Geschlossene Verbindungen.|Anzahl|Average|Geschlossene Verbindungen für Microsoft.EventHub.|EntityName|
|CaptureBacklog|Backlog erfassen.|Anzahl|Gesamt|Erfassen des Backlogs für Microsoft.EventHub.|EntityName|
|CapturedMessages|Erfasste Nachrichten.|Anzahl|Gesamt|Erfasste Nachrichten für Microsoft.EventHub.|EntityName|
|CapturedBytes|Erfasste Bytes.|Byte|Gesamt|Erfasste Bytes für Microsoft.EventHub.|EntityName|
|Size|Size|Byte|Average|Größe eines EventHub in Bytes.|EntityName|
|INREQS|Eingehende Anforderungen (veraltet)|Anzahl|Gesamt|Gesamtzahl eingehender Sendeanforderungen für einen Namespace (veraltet)|Keine|
|SUCCREQ|Erfolgreiche Anforderungen (veraltet)|Anzahl|Gesamt|Gesamtzahl erfolgreicher Anforderungen für einen Namespace (veraltet)|Keine|
|FAILREQ|Fehlerhafte Anforderungen (veraltet)|Anzahl|Gesamt|Gesamtzahl fehlerhafter Anforderungen für einen Namespace (veraltet)|Keine|
|SVRBSY|Fehler durch ausgelasteten Server (veraltet)|Anzahl|Gesamt|Gesamtzahl der Fehler durch ausgelasteten Server für einen Namespace (veraltet)|Keine|
|INTERR|Interne Serverfehler (veraltet)|Anzahl|Gesamt|Gesamtzahl interner Serverfehler für einen Namespace (veraltet)|Keine|
|MISCERR|Andere Fehler (veraltet)|Anzahl|Gesamt|Gesamtzahl fehlerhafter Anforderungen für einen Namespace (veraltet)|Keine|
|INMSGS|Eingehende Nachrichten (veraltet)|Anzahl|Gesamt|Gesamtzahl von eingehenden Nachrichten für einen Namespace Diese Metrik ist veraltet. Verwenden Sie stattdessen die Metrik für eingehende Nachrichten. (veraltet)|Keine|
|EHINMSGS|Eingehende Nachrichten (veraltet)|Anzahl|Gesamt|Gesamtzahl von eingehenden Nachrichten für einen Namespace (veraltet)|Keine|
|OUTMSGS|Ausgehende Nachrichten (veraltet)|Anzahl|Gesamt|Gesamtzahl von ausgehenden Nachrichten für einen Namespace Diese Metrik ist veraltet. Verwenden Sie stattdessen die Metrik für ausgehende Nachrichten. (veraltet)|Keine|
|EHOUTMSGS|Ausgehende Nachrichten (veraltet)|Anzahl|Gesamt|Gesamtzahl von ausgehenden Nachrichten für einen Namespace (veraltet)|Keine|
|EHINMBS|Eingehende Bytes (veraltet)|Byte|Gesamt|Event Hub-Durchsatz eingehender Nachrichten für einen Namespace Diese Metrik ist veraltet. Verwenden Sie stattdessen die Metrik für eingehende Bytes. (veraltet)|Keine|
|EHINBYTES|Eingehende Bytes (veraltet)|Byte|Gesamt|Event Hub-Durchsatz eingehender Nachrichten für einen Namespace (veraltet)|Keine|
|EHOUTMBS|Ausgehende Bytes (veraltet)|Byte|Gesamt|Event Hub-Durchsatz ausgehender Nachrichten für einen Namespace Diese Metrik ist veraltet. Verwenden Sie stattdessen die Metrik für ausgehende Bytes. (veraltet)|Keine|
|EHOUTBYTES|Ausgehende Bytes (veraltet)|Byte|Gesamt|Event Hub-Durchsatz ausgehender Nachrichten für einen Namespace (veraltet)|Keine|
|EHABL|Archivierte Backlognachrichten (veraltet)|Anzahl|Gesamt|Event Hub-Archivnachrichten im Backlog für einen Namespace (veraltet)|Keine|
|EHAMSGS|Archivnachrichten (veraltet)|Anzahl|Gesamt|Event Hub-Archivnachrichten in einem Namespace (veraltet)|Keine|
|EHAMBS|Durchsatz von Archivnachrichten (veraltet)|Byte|Gesamt|Event Hub-Durchsatz archivierter Nachrichten in einem Namespace (veraltet)|Keine|

## <a name="microsofteventhubclusters"></a>Microsoft.EventHub/clusters

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|SuccessfulRequests|Erfolgreiche Anforderungen|Anzahl|Gesamt|Erfolgreiche Anforderungen für Microsoft.EventHub.|OperationResult|
|ServerErrors|Serverfehler.|Anzahl|Gesamt|Serverfehler für Microsoft.EventHub.|OperationResult|
|UserErrors|Benutzerfehler.|Anzahl|Gesamt|Benutzerfehler für Microsoft.EventHub.|OperationResult|
|QuotaExceededErrors|Fehler aufgrund von Kontingentüberschreitung.|Anzahl|Gesamt|Fehler aufgrund von Kontingentüberschreitung für Microsoft.EventHub.|OperationResult|
|ThrottledRequests|Gedrosselte Anforderungen.|Anzahl|Gesamt|Gedrosselte Anforderungen für Microsoft.EventHub.|OperationResult|
|IncomingRequests|Eingehende Anforderungen|Anzahl|Gesamt|Eingehende Anforderungen für Microsoft.EventHub.|Keine|
|IncomingMessages|Eingehende Nachrichten|Anzahl|Gesamt|Eingehende Nachrichten für Microsoft.EventHub.|Keine|
|OutgoingMessages|Ausgehende Nachrichten|Anzahl|Gesamt|Ausgehende Nachrichten für Microsoft.EventHub.|Keine|
|IncomingBytes|Eingehende Bytes.|Byte|Gesamt|Eingehende Bytes für Microsoft.EventHub.|Keine|
|OutgoingBytes|Ausgehende Bytes.|Byte|Gesamt|Ausgehende Bytes für Microsoft.EventHub.|Keine|
|ActiveConnections|ActiveConnections|Anzahl|Average|Aktive Verbindungen gesamt für Microsoft.EventHub.|Keine|
|ConnectionsOpened|Geöffnete Verbindungen.|Anzahl|Average|Geöffnete Verbindungen für Microsoft.EventHub.|Keine|
|ConnectionsClosed|Geschlossene Verbindungen.|Anzahl|Average|Geschlossene Verbindungen für Microsoft.EventHub.|Keine|
|CaptureBacklog|Backlog erfassen.|Anzahl|Gesamt|Erfassen des Backlogs für Microsoft.EventHub.|Keine|
|CapturedMessages|Erfasste Nachrichten.|Anzahl|Gesamt|Erfasste Nachrichten für Microsoft.EventHub.|Keine|
|CapturedBytes|Erfasste Bytes.|Byte|Gesamt|Erfasste Bytes für Microsoft.EventHub.|Keine|
|CPU|CPU|Percent|Maximum|CPU-Auslastung für den Event Hub-Cluster in Prozent|Role|
|AvailableMemory|Verfügbarer Arbeitsspeicher|Percent|Maximum|Verfügbarer Arbeitsspeicher für den Event Hub-Cluster als Prozentsatz des Gesamtarbeitsspeichers.|Role|


## <a name="microsofthdinsightclusters"></a>Microsoft.HDInsight/clusters

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|GatewayRequests|Gatewayanforderungen|Anzahl|Gesamt|Anzahl von Gatewayanforderungen|HttpStatus|
|CategorizedGatewayRequests|Kategorisierte Gatewayanforderungen|Anzahl|Gesamt|Anzahl von Gatewayanforderungen nach Kategorien (1xx/2xx/3xx/4xx/5xx)|HttpStatus|
|NumActiveWorkers|Anzahl aktiver Worker|Anzahl|Maximum|Anzahl aktiver Worker|MetricName|


## <a name="microsoftinsightsautoscalesettings"></a>Microsoft.Insights/AutoscaleSettings

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|ObservedMetricValue|Beobachteter Metrikwert|Anzahl|Average|Der Wert, der beim Ausführen der automatischen Skalierung berechnet wurde.|MetricTriggerSource|
|MetricThreshold|Schwellenwert der Metrik|Anzahl|Average|Der bei der Ausführung der automatischen Skalierung konfigurierte Schwellenwert für die automatische Skalierung.|MetricTriggerRule|
|ObservedCapacity|Beobachtete Kapazität|Anzahl|Average|Die Kapazität, die beim Ausführen der automatischen Skalierung gemeldet wurde.|Keine|
|ScaleActionsInitiated|Initiierte Skalierungsaktionen|Anzahl|Gesamt|Die Richtung des Skalierungsvorgangs.|ScaleDirection|




## <a name="microsoftinsightscomponents"></a>Microsoft.Insights/Components

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|availabilityResults/availabilityPercentage|Verfügbarkeit|Percent|Average|Prozentsatz erfolgreich abgeschlossener Verfügbarkeitstests|availabilityResult/name, availabilityResult/location|
|availabilityResults/count|Verfügbarkeitstests|Anzahl|Anzahl|Anzahl von Verfügbarkeitstests|availabilityResult/name, availabilityResult/location, availabilityResult/success|
|availabilityResults/duration|Dauer von Verfügbarkeitstests|Millisekunden|Average|Dauer von Verfügbarkeitstests|availabilityResult/name, availabilityResult/location, availabilityResult/success|
|browserTimings/networkDuration|Netzwerkverbindungszeit zum Laden der Seite|Millisekunden|Average|Zeit zwischen Benutzeranforderung und Netzwerkverbindung. Umfasst DNS-Suche und Transportverbindung.|Keine|
|browserTimings/processingDuration|Clientverarbeitungszeit|Millisekunden|Average|Zeit zwischen dem Empfang des letzten Byte eines Dokuments und dem Laden des DOM. Asynchrone Anforderungen werden möglicherweise immer noch verarbeitet.|Keine|
|browserTimings/receiveDuration|Empfängt Antwortzeit|Millisekunden|Average|Zeit zwischen dem ersten und letzten Byte, oder bis zur Verbindungstrennung.|Keine|
|browserTimings/sendDuration|Anforderungszeit senden|Millisekunden|Average|Zeit zwischen der Netzwerkverbindung und dem Empfang des ersten Byte.|Keine|
|browserTimings/totalDuration|Browser-Seitenladezeit|Millisekunden|Average|Zeit ab der Benutzeranforderung, bis DOM, Stylesheets, Skripts und Bilder geladen werden.|Keine|
|dependencies/count|Abhängigkeitsaufrufe|Anzahl|Anzahl|Die Anzahl der von der Anwendung an externe Ressourcen ausgeführten Aufrufe.|dependency/type, dependency/performanceBucket, dependency/success, dependency/target, operation/synthetic, cloud/roleInstance, cloud/roleName|
|dependencies/duration|Dauer der Abhängigkeit|Millisekunden|Average|Die Dauer der von der Anwendung an externe Ressourcen ausgeführten Aufrufe.|dependency/type, dependency/performanceBucket, dependency/success, dependency/target, operation/synthetic, cloud/roleInstance, cloud/roleName|
|dependencies/failed|Fehler bei Abhängigkeitsaufrufen|Anzahl|Anzahl|Anzahl fehlerhafter Abhängigkeitsaufrufe von der Anwendung an externe Ressourcen.|dependency/type, dependency/performanceBucket, dependency/success, dependency/target, operation/synthetic, cloud/roleInstance, cloud/roleName|
|pageViews/count|Seitenaufrufe|Anzahl|Anzahl|Anzahl der Seitenaufrufe|operation/synthetic, cloud/roleName|
|pageViews/duration|Ladezeit der Seitenansicht|Millisekunden|Average|Ladezeit der Seitenansicht|operation/synthetic, cloud/roleName|
|performanceCounters/requestExecutionTime|Ausführungszeit der HTTP-Anforderung|Millisekunden|Average|Ausführungszeit der aktuellen Anforderung.|cloud/roleInstance|
|performanceCounters/requestsInQueue|HTTP-Anforderungen in der Anwendungswarteschlange|Anzahl|Average|Länge der Anwendungsanforderungswarteschleife.|cloud/roleInstance|
|performanceCounters/requestsPerSecond|HTTP-Anforderungsrate|Anzahl pro Sekunde|Average|Rate aller Anforderungen an die Anwendung pro Sekunde von ASP.NET.|cloud/roleInstance|
|performanceCounters/exceptionsPerSecond|Ausnahmerate|Anzahl pro Sekunde|Average|Die Anzahl der behandelten Ausnahmen und Ausnahmefehler, die an Windows gemeldet werden, einschließlich .NET-Ausnahmen und nicht verwalteten Ausnahmen, die in .NET-Ausnahmen konvertiert werden.|cloud/roleInstance|
|performanceCounters/processIOBytesPerSecond|E/A-Rate für Prozess|Bytes pro Sekunde|Average|Gesamtanzahl von pro Sekunde in Dateien, im Netzwerk und auf Geräten gelesenen und geschriebenen Bytes.|cloud/roleInstance|
|performanceCounters/processCpuPercentage|Prozess-CPU|Percent|Average|Der Prozentsatz der verstrichenen Zeit für alle Prozessthreads, die den Prozessor zur Ausführung von Anweisungen verwendet haben. Dies kann zwischen 0 und 100 variieren. Diese Metrik gibt ausschließlich die Leistung des w3wp-Prozesses an.|cloud/roleInstance|
|performanceCounters/processorCpuPercentage|Prozessorzeit|Percent|Average|Der Prozentsatz der Zeit, die der Prozessor nicht im Leerlauf in Threads verbringt.|cloud/roleInstance|
|performanceCounters/memoryAvailableBytes|Verfügbarer Arbeitsspeicher|Byte|Average|Physischer Speicher ist sofort für die Zuordnung zu einem Prozess oder für die Systemnutzung verfügbar.|cloud/roleInstance|
|performanceCounters/processPrivateBytes|Private Bytes für Prozess|Byte|Average|Speicher, der exklusiv den überwachten Anwendungsprozessen zugewiesen ist.|cloud/roleInstance|
|requests/duration|Serverantwortzeit|Millisekunden|Average|Zeit zwischen dem Empfang einer HTTP-Anforderung und dem Abschluss des Sendevorgangs der Antwort.|request/performanceBucket,request/resultCode,operation/synthetic,cloud/roleInstance,request/success,cloud/roleName|
|requests/count|Serveranforderungen|Anzahl|Anzahl|Anzahl der abgeschlossenen HTTP-Anforderungen.|request/performanceBucket,request/resultCode,operation/synthetic,cloud/roleInstance,request/success,cloud/roleName|
|requests/failed|Anforderungsfehler|Anzahl|Anzahl|Anzahl der als fehlerhaft markierten HTTP-Anforderungen. In den meisten Fällen sind dies Anforderungen mit dem Antwortcode >= 400 und ungleich 401.|request/performanceBucket,request/resultCode,request/success,operation/synthetic,cloud/roleInstance,cloud/roleName|
|requests/rate|Serveranforderungsrate|Anzahl pro Sekunde|Average|Rate der Serveranforderungen pro Sekunde|request/performanceBucket,request/resultCode,operation/synthetic,cloud/roleInstance,request/success,cloud/roleName|
|exceptions/count|Ausnahmen|Anzahl|Anzahl|Die kombinierte Anzahl nicht abgefangener Ausnahmen.|cloud/roleName,cloud/roleInstance,client/type|
|exceptions/browser|Browserausnahmen|Anzahl|Anzahl|Anzahl nicht erfasster Ausnahmen, die im Browser ausgelöst wurden.|client/isServer,cloud/roleName|
|exceptions/server|Serverausnahmen|Anzahl|Anzahl|Anzahl nicht erfasster Ausnahmen, die in der Serveranwendung ausgelöst wurden.|client/isServer,cloud/roleName,cloud/roleInstance|
|traces/count|Traces|Anzahl|Anzahl|Dokumentanzahl nachverfolgen|trace/severityLevel,operation/synthetic,cloud/roleName,cloud/roleInstance|



## <a name="microsoftkeyvaultvaults"></a>Microsoft.KeyVault/vaults

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|ServiceApiHit|Dienst-API-Treffer, gesamt|Anzahl|Anzahl|Gesamtanzahl der Dienst-API-Treffer|ActivityType, ActivityName|
|ServiceApiLatency|Gesamtwartezeit für Dienst-API|Millisekunden|Average|Gesamtwartezeit für Dienst-API-Anforderungen|ActivityType,ActivityName,StatusCode,StatusCodeClass|
|ServiceApiResult|Dienst-API-Ergebnisse, gesamt|Anzahl|Anzahl|Gesamtanzahl der Dienst-API-Ergebnisse|ActivityType,ActivityName,StatusCode,StatusCodeClass|
|SaturationShoebox|Tresorauslastung insgesamt|Percent|Average|Verwendete Tresorkapazität|ActivityType,ActivityName,TransactionType|
|Verfügbarkeit|Tresorverfügbarkeit insgesamt|Percent|Average|Verfügbarkeit von Tresoranforderungen|ActivityType,ActivityName,StatusCode,StatusCodeClass|

## <a name="microsoftkustoclusters"></a>Microsoft.Kusto/Clusters

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|CacheUtilization|Cacheauslastung|Percent|Average|Auslastungsgrad im Clusterbereich|Keine|
|QueryDuration|Abfragedauer|Millisekunden|Average|Abfragedauer in Sekunden|QueryStatus|
|IngestionUtilization|Datenerfassungsauslastung|Percent|Average|Verhältnis der verwendeten Datenerfassungsslots im Cluster|Keine|
|KeepAlive|Keep-Alive|Anzahl|Average|Die Integritätsprüfung zeigt an, dass der Cluster auf Abfragen reagiert.|Keine|
|IngestionVolumeInMB|Datenerfassungsvolumen (in MB)|Anzahl|Gesamt|Gesamtvolumen der im Cluster erfassten Daten (in MB)|Datenbank|
|IngestionLatencyInSeconds|Erfassungslatenz (in Sekunden)|Sekunden|Average|Erfassungszeit von der Quelle (z. B. Nachricht in EventHub) zum Cluster in Sekunden|Keine|
|EventsProcessedForEventHubs|Verarbeitete Ereignisse (für Event/IoT Hub)|Anzahl|Gesamt|Anzahl der vom Cluster verarbeiteten Ereignisse beim Erfassen aus Event/IoT Hub|EventStatus|
|IngestionResult|Ergebnis der Datenerfassung|Anzahl|Anzahl|Anzahl der Erfassungsvorgänge|IngestionResultDetails|
|CPU|CPU|Percent|Average|CPU-Auslastungsgrad|Keine|
|ContinuousExportNumOfRecordsExported|Fortlaufender Export – Anzahl der exportierten Datensätze|Anzahl|Gesamt|Anzahl der exportierten Datensätze, ausgelöst für jedes Speicherartefakt, das während des Exportvorgangs geschrieben wird|Keine|
|ExportUtilization|Exportauslastung|Percent|Maximum|Exportauslastung|Keine|
|ContinuousExportPendingCount|ContinuousExportPendingCount|Anzahl|Maximum|Die Anzahl ausstehender fortlaufender Exportaufträge, die zur Ausführung bereit sind.|Keine|
|ContinuousExportMaxLatenessMinutes|Maximale Wartezeit in Minuten für fortlaufenden Export|Anzahl|Maximum|Maximale Zeit in Minuten aller fortlaufenden Exporte, die ausstehen und für die Ausführung bereit sind|Keine|
|ContinuousExportResult|Ergebnis des fortlaufenden Exports|Anzahl|Anzahl|Gibt an, ob der fortlaufende Export erfolgreich war|ContinuousExportName, Result, Database|
|StreamingIngestDuration|Dauer der Streamingerfassung|Millisekunden|Average|Dauer der Streamingerfassung in Millisekunden|Keine|
|StreamingIngestDataRate|Datenrate der Streamingerfassung|Anzahl|Average|Datenrate der Streamingerfassung (MB pro Sekunde)|Keine|
|SteamingIngestRequestRate|Anforderungsrate der Streamingerfassung|Anzahl|RateRequestsPerSecond|Anforderungsrate der Streamingerfassung (Anforderungen pro Sekunde)|Keine|
|StreamingIngestResults|Ergebnis der Streamingerfassung|Anzahl|Average|Ergebnis der Streamingerfassung|Ergebnis|
|TotalNumberOfConcurrentQueries|Gesamtanzahl gleichzeitiger Abfragen|Anzahl|Gesamt|Gesamtanzahl gleichzeitiger Abfragen|Keine|
|TotalNumberOfThrottledQueries|Gesamtanzahl gedrosselter Abfragen|Anzahl|Gesamt|Gesamtanzahl gedrosselter Abfragen|Keine|
|TotalNumberOfThrottledCommands|Gesamtanzahl gedrosselter Befehle|Anzahl|Gesamt|Gesamtanzahl gedrosselter Befehle|CommandType|
|TotalNumberOfExtents|Gesamtanzahl von Erweiterungen|Anzahl|Gesamt|Gesamtanzahl von Datenerweiterungen|Keine|


## <a name="microsoftlogicworkflows"></a>Microsoft.Logic/workflows

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|RunsStarted|Gestartete Ausführungen|Anzahl|Gesamt|Anzahl gestarteter Workflowausführungen.|Keine|
|RunsCompleted|Abgeschlossene Ausführungen|Anzahl|Gesamt|Anzahl abgeschlossener Workflowausführungen.|Keine|
|RunsSucceeded|Erfolgreiche Ausführungen|Anzahl|Gesamt|Anzahl erfolgreicher Workflowausführungen.|Keine|
|RunsFailed|Ausführungsfehler|Anzahl|Gesamt|Anzahl fehlerhafter Workflowausführungen.|Keine|
|RunsCancelled|Abgebrochene Ausführungen|Anzahl|Gesamt|Anzahl abgebrochener Workflowausführungen.|Keine|
|RunLatency|Ausführungslatenz|Sekunden|Average|Latenz abgeschlossener Workflowausführungen.|Keine|
|RunSuccessLatency|Latenz erfolgreicher Ausführungen|Sekunden|Average|Latenz erfolgreicher Workflowausführungen.|Keine|
|RunThrottledEvents|Ereignisse zu gedrosselten Ausführungen|Anzahl|Gesamt|Anzahl von Ereignissen zu gedrosselten Workflowaktionen oder Triggern.|Keine|
|RunStartThrottledEvents|Ereignisse zu gedrosselten Ausführungsstarts|Anzahl|Gesamt|Anzahl von Ereignissen zu gedrosselten Workflow-Ausführungsstarts.|Keine|
|RunFailurePercentage|Prozentsatz der Ausführungsfehler|Percent|Gesamt|Prozentsatz fehlerhafter Workflowausführungen|Keine|
|ActionsStarted|Gestartete Aktionen |Anzahl|Gesamt|Anzahl gestarteter Workflowaktionen.|Keine|
|ActionsCompleted|Abgeschlossene Aktionen |Anzahl|Gesamt|Anzahl abgeschlossener Workflowaktionen.|Keine|
|ActionsSucceeded|Erfolgreiche Aktionen |Anzahl|Gesamt|Anzahl erfolgreicher Workflowaktionen.|Keine|
|ActionsFailed|Aktionsfehler |Anzahl|Gesamt|Anzahl fehlerhafter Workflowaktionen.|Keine|
|ActionsSkipped|Übersprungene Aktionen |Anzahl|Gesamt|Anzahl übersprungener Workflowaktionen.|Keine|
|ActionLatency|Aktionslatenz |Sekunden|Average|Latenz abgeschlossener Workflowaktionen.|Keine|
|ActionSuccessLatency|Latenz erfolgreicher Aktionen |Sekunden|Average|Latenz erfolgreicher Workflowaktionen.|Keine|
|ActionThrottledEvents|Ereignisse zu gedrosselten Aktionen|Anzahl|Gesamt|Anzahl von Ereignissen zu gedrosselten Workflowaktionen.|Keine|
|TriggersStarted|Gestartete Trigger |Anzahl|Gesamt|Anzahl gestarteter Workflowtrigger.|Keine|
|TriggersCompleted|Abgeschlossene Trigger |Anzahl|Gesamt|Anzahl abgeschlossener Workflowtrigger.|Keine|
|TriggersSucceeded|Erfolgreiche Trigger |Anzahl|Gesamt|Anzahl erfolgreicher Workflowtrigger.|Keine|
|TriggersFailed|Triggerfehler |Anzahl|Gesamt|Anzahl fehlerhafter Workflowtrigger.|Keine|
|TriggersSkipped|Übersprungene Trigger|Anzahl|Gesamt|Anzahl übersprungener Workflowtrigger.|Keine|
|TriggersFired|Ausgelöste Trigger |Anzahl|Gesamt|Anzahl ausgelöster Workflowtrigger.|Keine|
|TriggerLatency|Triggerlatenz |Sekunden|Average|Latenz abgeschlossener Workflowtrigger.|Keine|
|TriggerFireLatency|Latenz beim Auslösen von Triggern |Sekunden|Average|Latenz ausgelöster Workflowtrigger.|Keine|
|TriggerSuccessLatency|Latenz erfolgreicher Trigger |Sekunden|Average|Latenz erfolgreicher Workflowtrigger.|Keine|
|TriggerThrottledEvents|Ereignisse zu gedrosselten Triggern|Anzahl|Gesamt|Anzahl von Ereignissen zu gedrosselten Workflowtriggern.|Keine|
|BillableActionExecutions|Ausführungen abrechenbarer Aktionen|Anzahl|Gesamt|Anzahl der Workflowaktionsausführungen, die in Rechnung gestellt werden|Keine|
|BillableTriggerExecutions|Ausführungen abrechenbarer Trigger|Anzahl|Gesamt|Anzahl der Workflowtriggerausführungen, die in Rechnung gestellt werden|Keine|
|TotalBillableExecutions|Gesamtzahl der abrechenbaren Ausführungen|Anzahl|Gesamt|Anzahl der Workflowausführungen, die in Rechnung gestellt werden|Keine|
|BillingUsageNativeOperation|Nutzungsabrechnung für Ausführungen nativer Vorgänge|Anzahl|Gesamt|Anzahl von Ausführungen nativer Vorgänge, die in Rechnung gestellt werden.|Keine|
|BillingUsageStandardConnector|Nutzungsabrechnung für Ausführungen standardmäßiger Connectors|Anzahl|Gesamt|Anzahl von Ausführungen standardmäßiger Connectors, die in Rechnung gestellt werden.|Keine|
|BillingUsageStorageConsumption|Nutzungsabrechnung für Ausführungen mit Speicherverbrauch|Anzahl|Gesamt|Anzahl von Ausführungen mit Speicherverbrauch, die in Rechnung gestellt werden.|Keine|
|BillingUsageNativeOperation|Nutzungsabrechnung für Ausführungen nativer Vorgänge|Anzahl|Gesamt|Anzahl von Ausführungen nativer Vorgänge, die in Rechnung gestellt werden.|Keine|
|BillingUsageStandardConnector|Nutzungsabrechnung für Ausführungen standardmäßiger Connectors|Anzahl|Gesamt|Anzahl von Ausführungen standardmäßiger Connectors, die in Rechnung gestellt werden.|Keine|
|BillingUsageStorageConsumption|Nutzungsabrechnung für Ausführungen mit Speicherverbrauch|Anzahl|Gesamt|Anzahl von Ausführungen mit Speicherverbrauch, die in Rechnung gestellt werden.|Keine|



## <a name="microsoftlogicintegrationserviceenvironments"></a>Microsoft.Logic/integrationServiceEnvironments

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|RunsStarted|Gestartete Ausführungen|Anzahl|Gesamt|Anzahl gestarteter Workflowausführungen.|Keine|
|RunsCompleted|Abgeschlossene Ausführungen|Anzahl|Gesamt|Anzahl abgeschlossener Workflowausführungen.|Keine|
|RunsSucceeded|Erfolgreiche Ausführungen|Anzahl|Gesamt|Anzahl erfolgreicher Workflowausführungen.|Keine|
|RunsFailed|Ausführungsfehler|Anzahl|Gesamt|Anzahl fehlerhafter Workflowausführungen.|Keine|
|RunsCancelled|Abgebrochene Ausführungen|Anzahl|Gesamt|Anzahl abgebrochener Workflowausführungen.|Keine|
|RunLatency|Ausführungslatenz|Sekunden|Average|Latenz abgeschlossener Workflowausführungen.|Keine|
|RunSuccessLatency|Latenz erfolgreicher Ausführungen|Sekunden|Average|Latenz erfolgreicher Workflowausführungen.|Keine|
|RunThrottledEvents|Ereignisse zu gedrosselten Ausführungen|Anzahl|Gesamt|Anzahl von Ereignissen zu gedrosselten Workflowaktionen oder Triggern.|Keine|
|RunStartThrottledEvents|Ereignisse zu gedrosselten Ausführungsstarts|Anzahl|Gesamt|Anzahl von Ereignissen zu gedrosselten Workflow-Ausführungsstarts.|Keine|
|RunFailurePercentage|Prozentsatz der Ausführungsfehler|Percent|Gesamt|Prozentsatz fehlerhafter Workflowausführungen|Keine|
|ActionsStarted|Gestartete Aktionen |Anzahl|Gesamt|Anzahl gestarteter Workflowaktionen.|Keine|
|ActionsCompleted|Abgeschlossene Aktionen |Anzahl|Gesamt|Anzahl abgeschlossener Workflowaktionen.|Keine|
|ActionsSucceeded|Erfolgreiche Aktionen |Anzahl|Gesamt|Anzahl erfolgreicher Workflowaktionen.|Keine|
|ActionsFailed|Aktionsfehler |Anzahl|Gesamt|Anzahl fehlerhafter Workflowaktionen.|Keine|
|ActionsSkipped|Übersprungene Aktionen |Anzahl|Gesamt|Anzahl übersprungener Workflowaktionen.|Keine|
|ActionLatency|Aktionslatenz |Sekunden|Average|Latenz abgeschlossener Workflowaktionen.|Keine|
|ActionSuccessLatency|Latenz erfolgreicher Aktionen |Sekunden|Average|Latenz erfolgreicher Workflowaktionen.|Keine|
|ActionThrottledEvents|Ereignisse zu gedrosselten Aktionen|Anzahl|Gesamt|Anzahl von Ereignissen zu gedrosselten Workflowaktionen.|Keine|
|TriggersStarted|Gestartete Trigger |Anzahl|Gesamt|Anzahl gestarteter Workflowtrigger.|Keine|
|TriggersCompleted|Abgeschlossene Trigger |Anzahl|Gesamt|Anzahl abgeschlossener Workflowtrigger.|Keine|
|TriggersSucceeded|Erfolgreiche Trigger |Anzahl|Gesamt|Anzahl erfolgreicher Workflowtrigger.|Keine|
|TriggersFailed|Triggerfehler |Anzahl|Gesamt|Anzahl fehlerhafter Workflowtrigger.|Keine|
|TriggersSkipped|Übersprungene Trigger|Anzahl|Gesamt|Anzahl übersprungener Workflowtrigger.|Keine|
|TriggersFired|Ausgelöste Trigger |Anzahl|Gesamt|Anzahl ausgelöster Workflowtrigger.|Keine|
|TriggerLatency|Triggerlatenz |Sekunden|Average|Latenz abgeschlossener Workflowtrigger.|Keine|
|TriggerFireLatency|Latenz beim Auslösen von Triggern |Sekunden|Average|Latenz ausgelöster Workflowtrigger.|Keine|
|TriggerSuccessLatency|Latenz erfolgreicher Trigger |Sekunden|Average|Latenz erfolgreicher Workflowtrigger.|Keine|
|TriggerThrottledEvents|Ereignisse zu gedrosselten Triggern|Anzahl|Gesamt|Anzahl von Ereignissen zu gedrosselten Workflowtriggern.|Keine|
|IntegrationServiceEnvironmentWorkflowProcessorUsage|Workflow-Prozessorauslastung für Integrationsdienstumgebung|Percent|Average|Workflow-Prozessorauslastung für Integrationsdienstumgebung|Keine|
|IntegrationServiceEnvironmentWorkflowMemoryUsage|Workflow-Arbeitsspeichernutzung für Integrationsdienstumgebung|Percent|Average|Workflow-Arbeitsspeichernutzung für Integrationsdienstumgebung|Keine|
|IntegrationServiceEnvironmentConnectorProcessorUsage|Connector-Prozessorauslastung für Integrationsdienstumgebung|Percent|Average|Connector-Prozessorauslastung für Integrationsdienstumgebung|Keine|
|IntegrationServiceEnvironmentConnectorMemoryUsage|Connector-Arbeitsspeichernutzung für Integrationsdienstumgebung|Percent|Average|Connector-Arbeitsspeichernutzung für Integrationsdienstumgebung|Keine|

## <a name="microsoftmachinelearningservicesworkspaces"></a>Microsoft.MachineLearningServices/workspaces

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|Abgeschlossene Ausführungen|Abgeschlossene Ausführungen|Anzahl|Gesamt|Anzahl erfolgreich abgeschlossener Ausführungen für diesen Arbeitsbereich|Scenario, RunType, PublishedPipelineId, ComputeType, PipelineStepType|
|Gestartete Ausführungen|Gestartete Ausführungen|Anzahl|Gesamt|Anzahl gestarteter Ausführungen für diesen Arbeitsbereich|Scenario, RunType, PublishedPipelineId, ComputeType, PipelineStepType|
|Fehlerhafte Ausführungen|Fehlerhafte Ausführungen|Anzahl|Gesamt|Anzahl fehlerhafter Ausführungen für diesen Arbeitsbereich|Scenario, RunType, PublishedPipelineId, ComputeType, PipelineStepType|
|Erfolgreiche Modellregistrierungen|Erfolgreiche Modellregistrierungen|Anzahl|Gesamt|Anzahl der erfolgreichen Modellregistrierungen in diesem Arbeitsbereich|Szenario|
|Fehler bei der Modellregistrierung|Fehler bei der Modellregistrierung|Anzahl|Gesamt|Anzahl der fehlerhaften Modellregistrierungen in diesem Arbeitsbereich|Scenario, StatusCode|
|Gestartete Modellimplementierungen|Gestartete Modellimplementierungen|Anzahl|Gesamt|Anzahl der gestarteten Modellimplementierungen in diesem Arbeitsbereich|Szenario|
|Erfolgreiche Modellimplementierungen|Erfolgreiche Modellimplementierungen|Anzahl|Gesamt|Anzahl der erfolgreichen Modellimplementierungen in diesem Arbeitsbereich|Szenario|
|Model Deploy Failed (Fehler bei der Modellimplementierung)|Model Deploy Failed (Fehler bei der Modellimplementierung)|Anzahl|Gesamt|Anzahl der fehlerhaften Modellimplementierungen in diesem Arbeitsbereich|Scenario, StatusCode|
|Knoten insgesamt|Knoten insgesamt|Anzahl|Average|Gesamtanzahl von Knoten. Diese Summe umfasst Teile von aktiven Knoten, Leerlaufknoten, nicht verwendbaren Knoten, vorab erstellten Knoten, ausscheidenden Knoten.|Scenario, ClusterName|
|Aktive Knoten|Aktive Knoten|Anzahl|Average|Anzahl der aktiven Knoten. Dabei handelt es sich um die Knoten, auf denen aktiv ein Auftrag ausgeführt wird.|Scenario, ClusterName|
|Knoten im Leerlauf|Knoten im Leerlauf|Anzahl|Average|Anzahl von Knoten im Leerlauf. Knoten im Leerlauf sind die Knoten, auf denen keine Aufträge ausgeführt werden, die aber einen neuen Auftrag bei Verfügbarkeit akzeptieren können.|Scenario, ClusterName|
|Unusable Nodes (Nicht verwendbare Knoten)|Unusable Nodes (Nicht verwendbare Knoten)|Anzahl|Average|Anzahl nicht verwendbarer Knoten. Nicht verwendbare Knoten sind aufgrund eines nicht auflösbaren Problems nicht funktionsfähig. Azure recycelt diese Knoten.|Scenario, ClusterName|
|Vorzeitig entfernte Knoten|Vorzeitig entfernte Knoten|Anzahl|Average|Anzahl der vorzeitig entfernten Knoten. Dabei handelt es sich um die Knoten mit niedriger Priorität, die aus dem Pool verfügbarer Knoten entfernt werden.|Scenario, ClusterName|
|Ausscheidende Knoten|Ausscheidende Knoten|Anzahl|Average|Anzahl der ausscheidenden Knoten. Ausscheidende Knoten sind die Knoten, auf denen die Verarbeitung eines Auftrags gerade abgeschlossen wurde und die in den Leerlaufzustand wechseln.|Scenario, ClusterName|
|Kerne insgesamt|Kerne insgesamt|Anzahl|Average|Gesamtanzahl von Kernen|Scenario, ClusterName|
|Aktive Kerne|Aktive Kerne|Anzahl|Average|Gesamtanzahl der aktiven Kerne|Scenario, ClusterName|
|Kerne im Leerlauf|Kerne im Leerlauf|Anzahl|Average|Gesamtanzahl der Kerne im Leerlauf|Scenario, ClusterName|
|Nicht verwendbare Kerne|Nicht verwendbare Kerne|Anzahl|Average|Anzahl der nicht verwendbaren Kerne|Scenario, ClusterName|
|Vorzeitig entfernte Kerne|Vorzeitig entfernte Kerne|Anzahl|Average|Anzahl der vorzeitig entfernten Kerne|Scenario, ClusterName|
|Ausscheidende Kerne|Ausscheidende Kerne|Anzahl|Average|Anzahl der ausscheidenden Kerne|Scenario, ClusterName|
|Quota Utilization Percentage (Prozentsatz der Kontingentnutzung)|Quota Utilization Percentage (Prozentsatz der Kontingentnutzung)|Anzahl|Average|Prozentualer Anteil der genutzten Kontingente|Scenario, ClusterName, VmFamilyName, VmPriority|
|CpuUtilization|CpuUtilization|Anzahl|Average|CPU (Vorschau)|Scenario, runId, NodeId, CreatedTime|
|GpuUtilization|GpuUtilization|Anzahl|Average|GPU (Vorschau)|Scenario, runId, NodeId, CreatedTime, DeviceId|


## <a name="microsoftmapsaccounts"></a>Microsoft.Maps/accounts

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|Verwendung|Verwendung|Anzahl|Anzahl|Anzahl von API-Aufrufen|ApiCategory, ApiName, ResultType, ResponseCode|
|Verfügbarkeit|Verfügbarkeit|Percent|Average|Verfügbarkeit der APIs|ApiCategory, ApiName|

## <a name="microsoftmediamediaservicesstreamingendpoints"></a>Microsoft.Media/mediaservices/streamingEndpoints

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|Ausgehende Daten|Ausgehende Daten|Byte|Gesamt|Menge der Ausgangsdaten in Byte.|OutputFormat|
|SuccessE2ELatency|End-to-End-Wartezeit bei Erfolg|Millisekunden|Average|Durchschnittliche Latenz für erfolgreiche Anforderungen in Millisekunden.|OutputFormat|
|Requests|Requests|Anzahl|Gesamt|Anforderungen an einen Streamingendpunkt|OutputFormat,HttpStatusCode,ErrorCode|


## <a name="microsoftmediamediaservices"></a>Microsoft.Media/mediaservices

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|AssetQuota|Kontingent der Medienobjekte|Anzahl|Average|Anzahl der zulässigen Ressourcen für das aktuelle Media Services-Konto|Keine|
|AssetCount|Anzahl der Medienobjekte|Anzahl|Average|Anzahl der bereits im aktuellen Media Services-Konto erstellten Ressourcen|Keine|
|AssetQuotaUsedPercentage|Prozentsatz des verwendeten Kontingents der Medienobjekte|Percent|Average|Prozentualer Anteil der verwendeten Ressourcen im aktuellen Media Services-Konto|Keine|
|ContentKeyPolicyQuota|Kontingent der Richtlinien für Inhaltsschlüssel|Anzahl|Average|Anzahl der zulässigen Inhaltsschlüssel-Richtlinien für das aktuelle Media Services-Konto|Keine|
|ContentKeyPolicyCount|Anzahl der Richtlinien für Inhaltsschlüssel|Anzahl|Average|Anzahl der bereits im aktuellen Media Services-Konto erstellten Inhaltsschlüssel-Richtlinien|Keine|
|ContentKeyPolicyQuotaUsedPercentage|Prozentsatz des Kontingents der Richtlinien für Inhaltsschlüssel|Percent|Average|Prozentualer Anteil der verwendeten Inhaltsschlüssel-Richtlinien im aktuellen Media Services-Konto|Keine|
|StreamingPolicyQuota|Kontingent der Streamingrichtlinien|Anzahl|Average|Anzahl der zulässigen Streamingrichtlinien für das aktuelle Media Services-Konto|Keine|
|StreamingPolicyCount|Anzahl der Streamingrichtlinien|Anzahl|Average|Anzahl der im aktuellen Media Services-Konto bereits erstellten Streamingrichtlinien|Keine|
|StreamingPolicyQuotaUsedPercentage|Prozentsatz des verwendeten Kontingents der Streamingrichtlinien|Percent|Average|Prozentualer Anteil der verwendeten Streamingrichtlinien im aktuellen Media Services-Konto|Keine|

## <a name="microsoftnetappnetappaccountscapacitypoolsvolumes"></a>Microsoft.NetApp/netAppAccounts/capacityPools/volumes

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|AverageReadLatency|Durchschnittliche Wartezeit beim Lesevorgang|Millisekunden|Average|Durchschnittliche Wartezeit beim Lesen in Millisekunden pro Vorgang|Keine|
|AverageWriteLatency|Durchschnittliche Wartezeit beim Schreibvorgang|Millisekunden|Average|Durchschnittliche Wartezeit beim Schreiben in Millisekunden pro Vorgang|Keine|
|VolumeLogicalSize|Logische Größe des Volume|Byte|Average|Logische Größe des Volume (verwendete Bytes)|Keine|
|VolumeSnapshotSize|Größe der Volumenmomentaufnahme|Byte|Average|Größe aller Momentaufnahmen im Volumen|Keine|
|ReadIops|Lese-IOPS|Anzahl pro Sekunde|Average|E/A-Vorgänge lesen pro Sekunde|Keine|
|WriteIops|Schreib-IOPS|Anzahl pro Sekunde|Average|E/A-Vorgänge schreiben pro Sekunde|Keine|

## <a name="microsoftnetappnetappaccountscapacitypools"></a>Microsoft.NetApp/netAppAccounts/capacityPools

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|VolumePoolAllocatedUsed|Verwendeter zugeordneter Volumepool|Byte|Average|Zugeordnete verwendete Größe des Pools|Keine|
|VolumePoolTotalLogicalSize|Gesamte logische Größe des Volumepools|Byte|Average|Summe der logischen Größe aller zum Pool gehörenden Volumes|Keine|

## <a name="microsoftnetworknetworkinterfaces"></a>Microsoft.Network/networkInterfaces

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|BytesSentRate|Gesendete Bytes|Byte|Gesamt|Anzahl der durch die Netzwerkschnittstelle gesendeten Bytes|Keine|
|BytesReceivedRate|Empfangene Bytes|Byte|Gesamt|Anzahl der durch die Netzwerkschnittstelle empfangenen Bytes|Keine|
|PacketsSentRate|Gesendete Pakete|Anzahl|Gesamt|Anzahl der durch die Netzwerkschnittstelle gesendeten Pakete|Keine|
|PacketsReceivedRate|Empfangene Pakete|Anzahl|Gesamt|Anzahl der durch die Netzwerkschnittstelle empfangenen Pakete|Keine|

## <a name="microsoftnetworkloadbalancers"></a>Microsoft.Network/loadBalancers

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|VipAvailability|Datenpfadverfügbarkeit|Anzahl|Average|Durchschnittliche Load Balancer-Datenpfadverfügbarkeit pro Zeitdauer|FrontendIPAddress,FrontendPort|
|DipAvailability|Integritätsteststatus|Anzahl|Average|Durchschnittlicher Load Balancer-Integritätsteststatus pro Zeitdauer|ProtocolType,BackendPort,FrontendIPAddress,FrontendPort,BackendIPAddress|
|ByteCount|Byteanzahl|Anzahl|Gesamt|Gesamtanzahl von Bytes, die im Zeitraum übertragen wurden|FrontendIPAddress,FrontendPort,Direction|
|PacketCount|Paketzahl|Anzahl|Gesamt|Gesamtanzahl von Paketen, die im Zeitraum übertragen wurden|FrontendIPAddress,FrontendPort,Direction|
|SYNCount|SYN-Anzahl|Anzahl|Gesamt|Gesamtanzahl von SYN-Paketen, die im Zeitraum übertragen wurden|FrontendIPAddress,FrontendPort,Direction|
|SnatConnectionCount|Anzahl von SNAT-Verbindungen|Anzahl|Gesamt|Gesamtanzahl von neuen SNAT-Verbindungen, die innerhalb des Zeitraums erstellt wurden|FrontendIPAddress, BackendIPAddress, ConnectionState|
|AllocatedSnatPorts|Zugeordnete SNAT-Ports (Vorschau)|Anzahl|Gesamt|Gesamtanzahl von SNAT-Ports, die im Zeitraum zugeordnet wurden|FrontendIPAddress,BackendIPAddress,ProtocolType,IsAwaitingRemoval|
|UsedSnatPorts|Verwendete SNAT-Ports (Vorschau)|Anzahl|Gesamt|Gesamtanzahl von SNAT-Ports, die im Zeitraum verwendet wurden|FrontendIPAddress,BackendIPAddress,ProtocolType,IsAwaitingRemoval|


## <a name="microsoftnetworkdnszones"></a>Microsoft.Network/dnszones

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|QueryVolume|Abfragevolume|Anzahl|Gesamt|Anzahl von Abfragen für eine DNS-Zone|Keine|
|RecordSetCount|Anzahl von Datensatzgruppen|Anzahl|Maximum|Anzahl von Datensatzgruppen in einer DNS-Zone|Keine|
|RecordSetCapacityUtilization|Kapazitätsauslastung von Datensatzgruppen|Percent|Maximum|Von einer DNS-Zone genutzte Kapazität einer Datensatzgruppe in Prozent|Keine|


## <a name="microsoftnetworkpublicipaddresses"></a>Microsoft.Network/publicIPAddresses

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|PacketsInDDoS|Als DDoS eingehende Pakete|Anzahl pro Sekunde|Maximum|Als DDoS eingehende Pakete|Keine|
|PacketsDroppedDDoS|Als DDoS eingehende gelöschte Pakete|Anzahl pro Sekunde|Maximum|Als DDoS eingehende gelöschte Pakete|Keine|
|PacketsForwardedDDoS|Weitergeleitete als DDoS eingehende Pakete|Anzahl pro Sekunde|Maximum|Weitergeleitete als DDoS eingehende Pakete|Keine|
|TCPPacketsInDDoS|Als DDoS eingehende TCP-Pakete|Anzahl pro Sekunde|Maximum|Als DDoS eingehende TCP-Pakete|Keine|
|TCPPacketsDroppedDDoS|Als DDoS eingehende gelöschte TCP-Pakete|Anzahl pro Sekunde|Maximum|Als DDoS eingehende gelöschte TCP-Pakete|Keine|
|TCPPacketsForwardedDDoS|Weitergeleitete als DDoS eingehende TCP-Pakete|Anzahl pro Sekunde|Maximum|Weitergeleitete als DDoS eingehende TCP-Pakete|Keine|
|UDPPacketsInDDoS|Als DDoS eingehende UDP-Pakete|Anzahl pro Sekunde|Maximum|Als DDoS eingehende UDP-Pakete|Keine|
|UDPPacketsDroppedDDoS|Als DDoS eingehende gelöschte UDP-Pakete|Anzahl pro Sekunde|Maximum|Als DDoS eingehende gelöschte UDP-Pakete|Keine|
|UDPPacketsForwardedDDoS|Weitergeleitete als DDoS eingehende UDP-Pakete|Anzahl pro Sekunde|Maximum|Weitergeleitete als DDoS eingehende UDP-Pakete|Keine|
|BytesInDDoS|Als DDoS eingehende Bytes|Bytes pro Sekunde|Maximum|Als DDoS eingehende Bytes|Keine|
|BytesDroppedDDoS|Als DDoS eingehende gelöschte Bytes|Bytes pro Sekunde|Maximum|Als DDoS eingehende gelöschte Bytes|Keine|
|BytesForwardedDDoS|Weitergeleitete als DDoS eingehende Bytes|Bytes pro Sekunde|Maximum|Weitergeleitete als DDoS eingehende Bytes|Keine|
|TCPBytesInDDoS|Als DDoS eingehende TCP-Bytes|Bytes pro Sekunde|Maximum|Als DDoS eingehende TCP-Bytes|Keine|
|TCPBytesDroppedDDoS|Als DDoS eingehende gelöschte TCP-Bytes|Bytes pro Sekunde|Maximum|Als DDoS eingehende gelöschte TCP-Bytes|Keine|
|TCPBytesForwardedDDoS|Weitergeleitete als DDoS eingehende TCP-Bytes|Bytes pro Sekunde|Maximum|Weitergeleitete als DDoS eingehende TCP-Bytes|Keine|
|UDPBytesInDDoS|Als DDoS eingehende UDP-Bytes|Bytes pro Sekunde|Maximum|Als DDoS eingehende UDP-Bytes|Keine|
|UDPBytesDroppedDDoS|Als DDoS eingehende gelöschte UDP-Bytes|Bytes pro Sekunde|Maximum|Als DDoS eingehende gelöschte UDP-Bytes|Keine|
|UDPBytesForwardedDDoS|Weitergeleitete als DDoS eingehende UDP-Bytes|Bytes pro Sekunde|Maximum|Weitergeleitete als DDoS eingehende UDP-Bytes|Keine|
|IfUnderDDoSAttack|Unter DDoS-Angriff oder nicht|Anzahl|Maximum|Unter DDoS-Angriff oder nicht|Keine|
|DDoSTriggerTCPPackets|Eingehende TCP-Pakete, um die DDoS-Entschärfung auszulösen|Anzahl pro Sekunde|Maximum|Eingehende TCP-Pakete, um die DDoS-Entschärfung auszulösen|Keine|
|DDoSTriggerUDPPackets|Eingehende UDP-Pakete, um die DDoS-Entschärfung auszulösen|Anzahl pro Sekunde|Maximum|Eingehende UDP-Pakete, um die DDoS-Entschärfung auszulösen|Keine|
|DDoSTriggerSYNPackets|Eingehende SYN-Pakete, um die DDoS-Entschärfung auszulösen|Anzahl pro Sekunde|Maximum|Eingehende SYN-Pakete, um die DDoS-Entschärfung auszulösen|Keine|
|VipAvailability|Datenpfadverfügbarkeit|Anzahl|Average|Durchschnittliche Verfügbarkeit der IP-Adresse pro Zeitdauer|Port|
|ByteCount|Byteanzahl|Anzahl|Gesamt|Gesamtanzahl von Bytes, die im Zeitraum übertragen wurden|Port, Direction|
|PacketCount|Paketzahl|Anzahl|Gesamt|Gesamtanzahl von Paketen, die im Zeitraum übertragen wurden|Port, Direction|
|SynCount|SYN-Anzahl|Anzahl|Gesamt|Gesamtanzahl von SYN-Paketen, die im Zeitraum übertragen wurden|Port, Direction|



## <a name="microsoftnetworkvirtualnetworks"></a>Microsoft.Network/virtualNetworks

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|PingMeshAverageRoundtripMs|Roundtripzeit für Pings an eine VM|Millisekunden|Average|Roundtripzeit für an eine Ziel-VM gesendete Pings|SourceCustomerAddress, DestinationCustomerAddress|
|PingMeshProbesFailedPercent|Fehlerhafte Pings an einen virtuellen Computer|Percent|Average|Prozentualer Anteil der Anzahl fehlerhafter Pings an der Gesamtanzahl der gesendeten Pings einer Ziel-VM|SourceCustomerAddress, DestinationCustomerAddress|


## <a name="microsoftnetworkazurefirewalls"></a>Microsoft.Network/azurefirewalls

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|ApplicationRuleHit|Trefferanzahl für Anwendungsregeln|Anzahl|Gesamt|Häufigkeit, mit der Anwendungsregeln aufgerufen wurden|Status, Reason, Protocol|
|NetworkRuleHit|Trefferanzahl für Netzwerkregeln|Anzahl|Gesamt|Häufigkeit, mit der Netzwerkregeln aufgerufen wurden|Status, Reason, Protocol|
|FirewallHealth|Integritätszustand der Firewall|Percent|Average|Integritätszustand der Firewall|Status, Reason|
|DataProcessed|Verarbeitete Datenmenge|Byte|Gesamt|Gesamtmenge der von der Firewall verarbeiteten Daten|Keine|
|SNATPortUtilization|SNAT-Portnutzung|Percent|Average|SNAT-Portnutzung|Keine|


## <a name="microsoftnetworkapplicationgateways"></a>Microsoft.Network/applicationGateways

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|Throughput|Throughput|Bytes pro Sekunde|Average|Anzahl von Bytes pro Sekunde, die das Application Gateway bereitgestellt hat|Keine|
|UnhealthyHostCount|Anzahl von fehlerhaften Hosts|Anzahl|Average|Anzahl von fehlerhaften Back-End-Hosts|BackendSettingsPool|
|HealthyHostCount|Anzahl von fehlerfreien Hosts|Anzahl|Average|Anzahl von fehlerfreien Back-End-Hosts|BackendSettingsPool|
|TotalRequests|Anzahl von Anforderungen|Anzahl|Gesamt|Anzahl von erfolgreichen Anforderungen über Application Gateway|BackendSettingsPool|
|AvgRequestCountPerHealthyHost|Anforderungen pro Minute pro fehlerfreiem Host|Anzahl|Average|Durchschnittliche Anzahl von Anforderungen pro Minute pro fehlerfreiem Back-End-Host in einem Pool|BackendSettingsPool|
|FailedRequests|Anforderungsfehler|Anzahl|Gesamt|Anzahl von fehlerhaften Anforderungen über Application Gateway|BackendSettingsPool|
|ResponseStatus|Antwortstatus|Anzahl|Gesamt|Von Application Gateway zurückgegebener HTTP-Antwortstatus|HttpStatusGroup|
|CurrentConnections|Aktuelle Verbindungen|Anzahl|Gesamt|Anzahl von aktuellen Verbindungen mit Application Gateway|Keine|
|CpuUtilization|CPU-Auslastung|Percent|Average|Aktuelle CPU-Auslastung der Application Gateway-Instanz|Keine|
|CapacityUnits|Aktuelle Kapazitätseinheiten|Anzahl|Average|Verbrauchte Kapazitätseinheiten|Keine|
|ComputeUnits|Aktuelle Compute-Einheiten|Anzahl|Average|Verbrauchte Compute-Einheiten|Keine|
|BackendResponseStatus|Back-End-Antwortstatus|Anzahl|Gesamt|Anzahl der von den Back-End-Membern generierten HTTP-Antwortcodes. Dies schließt nicht die von Application Gateway generierten Antwortcodes ein.|BackendServer,BackendPool,BackendHttpSetting,HttpStatusGroup|
|TlsProtocol|Client-TLS-Protokoll|Anzahl|Gesamt|Anzahl von TLS- und Nicht-TLS-Anforderungen, die von dem Client initiiert wurden, der eine Verbindung mit Application Gateway hergestellt hat. Um die Verteilung des TLS-Protokolls anzuzeigen, filtern Sie nach dem Dimensions-TLS-Protokoll.|Listener, TlsProtocol|
|BytesSent|Gesendete Bytes|Byte|Gesamt|Gesamtanzahl der von Application Gateway an die Clients gesendeten Bytes|Listener|
|BytesReceived|Empfangene Bytes|Byte|Gesamt|Gesamtanzahl der von Application Gateway von den Clients empfangenen Bytes|Listener|
|ClientRtt|Client-RTT|Millisekunden|Average|Durchschnittliche Roundtripzeit zwischen Clients und Application Gateway. Diese Metrik gibt an, wie lange es dauert, Verbindungen herzustellen und Bestätigungen zurückzugeben.|Listener|
|ApplicationGatewayTotalTime|Application Gateway-Gesamtzeit|Millisekunden|Average|Durchschnittliche Zeit, bis eine Anforderung verarbeitet und die zugehörige Antwort gesendet wurde. Dies wird berechnet als durchschnittliches Intervall zwischen dem Zeitpunkt, zu dem Application Gateway das erste Byte einer HTTP-Anforderung empfängt, bis zu dem Zeitpunkt, zu dem der Vorgang zum Senden der Antwort abgeschlossen ist. Beachten Sie, dass dies in der Regel die Application Gateway-Verarbeitungszeit, die Zeit für das Durchlaufen des Netzwerks durch die Anforderungs- und Antwortpakete und die Zeit bis zur Antwort des Back-End-Servers einschließt.|Listener|
|BackendConnectTime|Back-End-Verbindungszeit|Millisekunden|Average|Zeitaufwand für das Herstellen einer Verbindung mit einem Back-End-Server|Listener,BackendServer,BackendPool,BackendHttpSetting|
|BackendFirstByteResponseTime|Antwortzeit für erstes Byte des Back-Ends|Millisekunden|Average|Das Zeitintervall zwischen dem Herstellen einer Verbindung mit dem Back-End-Server und dem Empfang des ersten Bytes des Antwortheaders und somit eine Abschätzung der Verarbeitungszeit des Back-End-Servers|Listener,BackendServer,BackendPool,BackendHttpSetting|
|BackendLastByteResponseTime|Antwortzeit für letztes Byte des Back-Ends|Millisekunden|Average|Das Zeitintervall zwischen dem Herstellen einer Verbindung mit dem Back-End-Server und dem Empfang des letzten Bytes des Antworttexts|Listener,BackendServer,BackendPool,BackendHttpSetting|
|MatchedCount|Gesamtregelverteilung in Web Application Firewall|Anzahl|Gesamt|Gesamtregelverteilung in Web Application Firewall für eingehenden Datenverkehr|RuleGroup, RuleId|
|BlockedCount|Regelverteilung für blockierte Web Application Firewall-Anforderungen|Anzahl|Gesamt|Regelverteilung für blockierte Web Application Firewall-Anforderungen|RuleGroup, RuleId|
|BlockedReqCount|Anzahl blockierter Web Application Firewall-Anforderungen|Anzahl|Gesamt|Anzahl blockierter Web Application Firewall-Anforderungen|Keine|

## <a name="microsoftnetworkvirtualnetworkgateways"></a>Microsoft.Network/virtualNetworkGateways

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|AverageBandwidth|S2S-Gatewaybandbreite|Bytes pro Sekunde|Average|Durchschnittliche Site-to-Site-Bandbreite eines Gateways in Bytes pro Sekunde|Keine|
|P2SBandwidth|P2S-Gatewaybandbreite|Bytes pro Sekunde|Average|Durchschnittliche Point-to-Site-Bandbreite eines Gateways in Bytes pro Sekunde|Keine|
|P2SConnectionCount|Anzahl der P2S-Verbindungen|Anzahl|Maximum|Anzahl der Point-to-Site-Verbindung eines Gateways|Protocol|
|TunnelAverageBandwidth|Bandbreite des Tunnels|Bytes pro Sekunde|Average|Durchschnittliche Bandbreite eines Tunnels in Bytes pro Sekunde|ConnectionName, RemoteIP|
|TunnelEgressBytes|Vom Tunnel ausgehende Bytes|Byte|Gesamt|Ausgehende Bytes eines Tunnels|ConnectionName, RemoteIP|
|TunnelIngressBytes|In Tunnel eingehende Bytes|Byte|Gesamt|Eingehende Bytes eines Tunnels|ConnectionName, RemoteIP|
|TunnelEgressPackets|Vom Tunnel ausgehende Pakete|Anzahl|Gesamt|Ausgehende Paketanzahl für einen Tunnel|ConnectionName, RemoteIP|
|TunnelIngressPackets|In Tunnel eingehende Pakete|Anzahl|Gesamt|Eingehende Paketanzahl für einen Tunnel|ConnectionName, RemoteIP|
|TunnelEgressPacketDropTSMismatch|Ausgehende gelöschte Pakete eines Tunnels durch einen TS-Konflikt|Anzahl|Gesamt|Anzahl der durch einen Konflikt mit dem Datenverkehrsselektor eines Tunnels gelöschten ausgehenden Pakete|ConnectionName, RemoteIP|
|TunnelIngressPacketDropTSMismatch|Eingehende gelöschte Pakete eines Tunnels durch einen TS-Konflikt|Anzahl|Gesamt|Anzahl der durch einen Konflikt mit dem Datenverkehrsselektor eines Tunnels gelöschten eingehenden Pakete|ConnectionName, RemoteIP|


## <a name="microsoftnetworkexpressrouteports"></a>Microsoft.Network/expressRoutePorts

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|RxLightLevel|RxLightLevel|Anzahl|Average|Empfangener Lichtpegel in dBm|Link, Lane|
|TxLightLevel|TxLightLevel|Anzahl|Average|Gesendeter Lichtpegel in dBm|Link, Lane|
|AdminState|AdminState|Anzahl|Average|Administratorstatus des Ports|Link|
|LineProtocol|LineProtocol|Anzahl|Average|Zeilenprotokollstatus des Ports|Link|
|PortBitsInPerSecond|BitsInPerSecond|Anzahl pro Sekunde|Average|In Azure eingehende Bits pro Sekunde|Link|
|PortBitsOutPerSecond|BitsOutPerSecond|Anzahl pro Sekunde|Average|Aus Azure ausgehende Bits pro Sekunde|Link|

## <a name="microsoftnetworkexpressroutecircuits"></a>Microsoft.Network/expressRouteCircuits

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|BitsInPerSecond|BitsInPerSecond|Anzahl pro Sekunde|Average|In Azure eingehende Bits pro Sekunde|PeeringType|
|BitsOutPerSecond|BitsOutPerSecond|Anzahl pro Sekunde|Average|Aus Azure ausgehende Bits pro Sekunde|PeeringType|
|GlobalReachBitsInPerSecond|GlobalReachBitsInPerSecond|Anzahl pro Sekunde|Average|In Azure eingehende Bits pro Sekunde|PeeredCircuitSKey|
|GlobalReachBitsOutPerSecond|GlobalReachBitsOutPerSecond|Anzahl pro Sekunde|Average|Aus Azure ausgehende Bits pro Sekunde|PeeredCircuitSKey|
|BgpAvailability|BGP-Verfügbarkeit|Percent|Average|BGP-Verfügbarkeit von MSEE für alle Peers.|PeeringType, Peer|
|ArpAvailability|ARP-Verfügbarkeit|Percent|Average|ARP-Verfügbarkeit von MSEE für alle Peers.|PeeringType, Peer|
|QosDropBitsInPerSecond|DroppedInBitsPerSecond|Anzahl pro Sekunde|Average|Verworfene Dateneingangsbits pro Sekunde|Keine|
|QosDropBitsOutPerSecond|DroppedOutBitsPerSecond|Anzahl pro Sekunde|Average|Verworfene Datenausgangsbits pro Sekunde|Keine|

## <a name="microsoftnetworkexpressroutecircuitspeerings"></a>Microsoft.Network/expressRouteCircuits/peerings

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|BitsInPerSecond|BitsInPerSecond|Anzahl pro Sekunde|Average|In Azure eingehende Bits pro Sekunde|Keine|
|BitsOutPerSecond|BitsOutPerSecond|Anzahl pro Sekunde|Average|Aus Azure ausgehende Bits pro Sekunde|Keine|

## <a name="microsoftnetworkconnections"></a>Microsoft.Network/connections

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|BitsInPerSecond|BitsInPerSecond|Anzahl pro Sekunde|Average|In Azure eingehende Bits pro Sekunde|Keine|
|BitsOutPerSecond|BitsOutPerSecond|Anzahl pro Sekunde|Average|Aus Azure ausgehende Bits pro Sekunde|Keine|

## <a name="microsoftnetworkexpressroutegateways"></a>Microsoft.Network/expressRouteGateways

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|ErGatewayConnectionBitsInPerSecond|BitsInPerSecond|Anzahl pro Sekunde|Average|In Azure eingehende Bits pro Sekunde|ConnectionName|
|ErGatewayConnectionBitsOutPerSecond|BitsOutPerSecond|Anzahl pro Sekunde|Average|Aus Azure ausgehende Bits pro Sekunde|ConnectionName|

## <a name="microsoftnetworktrafficmanagerprofiles"></a>Microsoft.Network/trafficManagerProfiles

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|QpsByEndpoint|Abfragen nach zurückgegebenem Endpunkt|Anzahl|Gesamt|Häufigkeit, mit der ein Endpunkt des Traffic Managers innerhalb des angegebenen Zeitrahmens zurückgegeben wurde|EndpointName|
|ProbeAgentCurrentEndpointStateByProfileResourceId|Endpunktstatus nach Endpunkt|Anzahl|Maximum|1, wenn der Teststatus des Endpunkts „Aktiviert“ ist, andernfalls 0.|EndpointName|



## <a name="microsoftnetworknetworkwatchersconnectionmonitors"></a>Microsoft.Network/networkWatchers/connectionMonitors

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|ProbesFailedPercent|Fehlerhafte Tests in Prozent|Percent|Average|Fehlerhafte Tests bei Konnektivitätsüberwachung in Prozent|Keine|
|AverageRoundtripMs|Durchschn. Roundtripzeit (ms)|Millisekunden|Average|Durchschnittliche Netzwerk-Roundtripzeit (ms) für die Konnektivitätsüberwachung von Testdatenverkehr, der zwischen Quelle und Ziel gesendet wurde.|Keine|
|ChecksFailedPercent|Fehler bei Überprüfungen in Prozent (Vorschau)|Percent|Average|Fehlerhafte Tests bei Konnektivitätsüberwachung in Prozent|SourceAddress,SourceName,SourceResourceId,SourceType,Protocol,DestinationAddress,DestinationName,DestinationResourceId,DestinationType,DestinationPort,TestGroupName,TestConfigurationName|
|RoundTripTimeMs|Roundtripzeit (ms) (Vorschau)|Millisekunden|Average|Roundtripzeit in Millisekunden für die Tests bei Konnektivitätsüberwachung|SourceAddress,SourceName,SourceResourceId,SourceType,Protocol,DestinationAddress,DestinationName,DestinationResourceId,DestinationType,DestinationPort,TestGroupName,TestConfigurationName|


## <a name="microsoftnetworkfrontdoors"></a>Microsoft.Network/frontdoors

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|RequestCount|Anzahl der Anforderungen|Anzahl|Gesamt|Die Anzahl der vom HTTP/S-Proxy bereitgestellten Clientanforderungen|HttpStatus, HttpStatusGroup, ClientRegion, ClientCountry|
|RequestSize|Anforderungsgröße|Byte|Gesamt|Die Anzahl der von Clients als Anforderungen an den HTTP/S-Proxy gesendeten Bytes|HttpStatus, HttpStatusGroup, ClientRegion, ClientCountry|
|ResponseSize|Antwortgröße|Byte|Gesamt|Die Anzahl der vom HTTP/S-Proxy als Antworten an Clients gesendeten Bytes|HttpStatus, HttpStatusGroup, ClientRegion, ClientCountry|
|BillableResponseSize|Größe von abrechnungsfähigen Antworten|Byte|Gesamt|Die Anzahl der abrechnungsfähigen Bytes (mindestens 2 KB pro Anforderung), die als Antworten vom HTTP/S-Proxy an Clients gesendet wurden.|HttpStatus, HttpStatusGroup, ClientRegion, ClientCountry|
|BackendRequestCount|Back-End-Anforderungsanzahl|Anzahl|Gesamt|Die Anzahl der vom HTTP/S-Proxy an Back-Ends gesendeten Anforderungen|HttpStatus, HttpStatusGroup, Backend|
|BackendRequestLatency|Latenz der Back-End-Anforderung|Millisekunden|Average|Die Zeitspanne zwischen dem Senden der Anforderung durch den HTTP/S-Proxy an das Back-End und dem Empfangen des letzten Antwortbytes durch den HTTP/S-Proxy vom Back-End|Back-End|
|TotalLatency|Gesamtlatenz|Millisekunden|Average|Die Zeitspanne zwischen dem Empfang der Clientanforderung durch den HTTP/S-Proxy und der Bestätigung des letzten Antwortbytes vom HTTP/S-Proxy durch den Client|HttpStatus, HttpStatusGroup, ClientRegion, ClientCountry|
|BackendHealthPercentage|Prozentsatz der Back-End-Integrität|Percent|Average|Der Prozentsatz der erfolgreichen Integritätstests vom HTTP/S-Proxy zu Back-Ends|Backend, BackendPool|
|WebApplicationFirewallRequestCount|Anforderungsanzahl für die Web Application Firewall|Anzahl|Gesamt|Die Anzahl der von der Web Application Firewall verarbeiteten Clientanforderungen|PolicyName, RuleName, Action|


## <a name="microsoftnotificationhubsnamespacesnotificationhubs"></a>Microsoft.NotificationHubs/Namespaces/NotificationHubs

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|registration.all|Registrierungsvorgänge|Anzahl|Gesamt|Gibt die Anzahl von allen erfolgreichen Registrierungsvorgängen an (Erstellungen, Aktualisierungen, Abfragen und Löschungen). |Keine|
|registration.create|Registrierungserstellungsvorgänge|Anzahl|Gesamt|Gibt die Anzahl von allen erfolgreichen Registrierungserstellungen an.|Keine|
|registration.update|Registrierungsaktualisierungsvorgänge|Anzahl|Gesamt|Gibt die Anzahl von allen erfolgreichen Registrierungsaktualisierungen an.|Keine|
|registration.get|Registrierungslesevorgänge|Anzahl|Gesamt|Gibt die Anzahl von allen erfolgreichen Registrierungsabfragen an.|Keine|
|registration.delete|Registrierungslöschvorgänge|Anzahl|Gesamt|Gibt die Anzahl von allen erfolgreichen Registrierungslöschungen an.|Keine|
|incoming|Eingehende Nachrichten|Anzahl|Gesamt|Gibt die Anzahl von allen erfolgreichen Übermittlungs-API-Aufrufen an. |Keine|
|incoming.scheduled|Geplante Pushbenachrichtigungen (gesendet)|Anzahl|Gesamt|Geplante Pushbenachrichtigungen (abgebrochen)|Keine|
|incoming.scheduled.cancel|Geplante Pushbenachrichtigungen (abgebrochen)|Anzahl|Gesamt|Geplante Pushbenachrichtigungen (abgebrochen)|Keine|
|scheduled.pending|Ausstehende Pushbenachrichtigungen|Anzahl|Gesamt|Ausstehende Pushbenachrichtigungen|Keine|
|installation.all|Installationsverwaltungsvorgänge|Anzahl|Gesamt|Installationsverwaltungsvorgänge|Keine|
|installation.get|Installationsvorgänge abrufen|Anzahl|Gesamt|Installationsvorgänge abrufen|Keine|
|installation.upsert|Installationsvorgänge erstellen oder aktualisieren|Anzahl|Gesamt|Installationsvorgänge erstellen oder aktualisieren|Keine|
|installation.patch|Patchinstallationsvorgänge|Anzahl|Gesamt|Patchinstallationsvorgänge|Keine|
|installation.delete|Installationsvorgänge löschen|Anzahl|Gesamt|Installationsvorgänge löschen|Keine|
|outgoing.allpns.success|Erfolgreiche Benachrichtigungen|Anzahl|Gesamt|Gibt die Anzahl von erfolgreichen Benachrichtigungen an.|Keine|
|outgoing.allpns.invalidpayload|Nutzlastfehler|Anzahl|Gesamt|Gibt die Anzahl von Pushvorgängen an, die nicht erfolgreich waren, weil vom PNS ein Nutzlastfehler zurückgegeben wurde.|Keine|
|outgoing.allpns.pnserror|Fehler des externen Benachrichtigungssystems|Anzahl|Gesamt|Gibt die Anzahl von Pushvorgängen an, die nicht erfolgreich waren, weil ein Problem bei der Kommunikation mit dem PNS vorlag (ausschließlich Authentifizierungsprobleme).|Keine|
|outgoing.allpns.channelerror|Kanalfehler|Anzahl|Gesamt|Gibt die Anzahl von Pushvorgängen an, die nicht erfolgreich waren, weil der Kanal ungültig, nicht der richtigen App zugeordnet, gedrosselt oder abgelaufen war.|Keine|
|outgoing.allpns.badorexpiredchannel|Kanalfehler (unzulässig oder abgelaufen)|Anzahl|Gesamt|Gibt die Anzahl von Pushvorgängen an, die nicht erfolgreich waren, weil der Kanal, das Token oder die Registrierungs-ID in der Registrierung abgelaufen oder ungültig war.|Keine|
|outgoing.wns.success|Erfolgreiche WNS-Benachrichtigungen|Anzahl|Gesamt|Gibt die Anzahl von erfolgreichen Benachrichtigungen an.|Keine|
|outgoing.wns.invalidcredentials|WNS-Autorisierungsfehler (ungültige Anmeldeinformationen)|Anzahl|Gesamt|Gibt die Anzahl von Pushvorgängen an, die nicht erfolgreich waren, weil das PNS die angegebenen Anmeldeinformationen nicht akzeptiert hat oder die Anmeldeinformationen blockiert wurden. (Windows Live erkennt die Anmeldeinformationen nicht.)|Keine|
|outgoing.wns.badchannel|WNS-Fehler durch fehlerhaften Kanal|Anzahl|Gesamt|Gibt die Anzahl von Pushvorgängen an, die nicht erfolgreich waren, weil der ChannelURI in der Registrierung nicht erkannt wurde (WNS-Status: 404 – Nicht gefunden).|Keine|
|outgoing.wns.expiredchannel|WNS-Fehler durch abgelaufenen Kanal|Anzahl|Gesamt|Die Anzahl von nicht erfolgreichen Pushvorgängen, weil der ChannelURI abgelaufen ist (WNS-Status: 410 – Fehlend).|Keine|
|outgoing.wns.throttled|Gedrosselte WNS-Benachrichtigungen|Anzahl|Gesamt|Gibt die Anzahl von Pushvorgängen an, die nicht erfolgreich waren, weil diese App vom WNS gedrosselt wird (WNS-Status: 406 – Nicht annehmbar).|Keine|
|outgoing.wns.tokenproviderunreachable|WNS-Autorisierungsfehler (nicht erreichbar)|Anzahl|Gesamt|Windows Live ist nicht erreichbar.|Keine|
|outgoing.wns.invalidtoken|WNS-Autorisierungsfehler (ungültiges Token)|Anzahl|Gesamt|Das für WNS bereitgestellte Token ist nicht gültig (WNS-Status: 401 – Nicht autorisiert).|Keine|
|outgoing.wns.wrongtoken|WNS-Autorisierungsfehler (falsches Token)|Anzahl|Gesamt|Das für WNS bereitgestellte Token ist gültig, gilt aber für eine andere Anwendung (WNS-Status: 403 – Verboten). Dieser Fall kann eintreten, wenn der ChannelURI in der Registrierung einer anderen App zugeordnet wird. Stellen Sie sicher, dass die Client-App der App zugeordnet ist, deren Anmeldeinformationen sich im Notification Hub befinden.|Keine|
|outgoing.wns.invalidnotificationformat|Ungültiges WNS-Benachrichtigungsformat|Anzahl|Gesamt|Das Format der Benachrichtigung ist ungültig (WNS-Status: 400). Beachten Sie, dass von WNS nicht alle ungültigen Nutzlasten abgelehnt werden.|Keine|
|outgoing.wns.invalidnotificationsize|WNS-Fehler durch ungültige Benachrichtigungsgröße|Anzahl|Gesamt|Die Benachrichtigungsnutzlast ist zu groß (WNS-Status: 413).|Keine|
|outgoing.wns.channelthrottled|WNS-Kanal gedrosselt|Anzahl|Gesamt|Die Benachrichtigung wurde verworfen, weil der ChannelURI in der Registrierung gedrosselt wurde (WNS-Antwortheader: X-WNS-NotificationStatus:channelThrottled).|Keine|
|outgoing.wns.channeldisconnected|WNS-Kanal nicht verbunden|Anzahl|Gesamt|Die Benachrichtigung wurde verworfen, weil der ChannelURI in der Registrierung gedrosselt wurde (WNS-Antwortheader: X-WNS-DeviceConnectionStatus: disconnected).|Keine|
|outgoing.wns.dropped|Verworfene WNS-Benachrichtigungen|Anzahl|Gesamt|Die Benachrichtigung wurde verworfen, weil der ChannelURI in der Registrierung gedrosselt wurde („X-WNS-NotificationStatus: dropped“, aber nicht „X-WNS-DeviceConnectionStatus: disconnected“).|Keine|
|outgoing.wns.pnserror|WNS-Fehler|Anzahl|Gesamt|Die Benachrichtigung wurde aufgrund von Fehlern bei der Kommunikation mit WNS nicht übermittelt.|Keine|
|outgoing.wns.authenticationerror|WNS-Authentifizierungsfehler|Anzahl|Gesamt|Die Benachrichtigung wurde nicht übermittelt, weil Fehler bei der Kommunikation mit Windows Live aufgetreten sind, die Anmeldeinformationen ungültig waren oder das falsche Token verwendet wurde.|Keine|
|outgoing.apns.success|Erfolgreiche APNS-Benachrichtigungen|Anzahl|Gesamt|Gibt die Anzahl von erfolgreichen Benachrichtigungen an.|Keine|
|outgoing.apns.invalidcredentials|APNS-Autorisierungsfehler|Anzahl|Gesamt|Gibt die Anzahl von Pushvorgängen an, die nicht erfolgreich waren, weil das PNS die angegebenen Anmeldeinformationen nicht akzeptiert hat oder die Anmeldeinformationen blockiert wurden.|Keine|
|outgoing.apns.badchannel|APNS-Fehler durch fehlerhaften Kanal|Anzahl|Gesamt|Gibt die Anzahl von Pushvorgängen an, die nicht erfolgreich waren, weil das Token ungültig ist (APNS-Statuscode: 8).|Keine|
|outgoing.apns.expiredchannel|APNS-Fehler durch abgelaufenen Kanal|Anzahl|Gesamt|Die Anzahl von Token, die aufgrund des APNS-Feedbackkanals ungültig gemacht wurden.|Keine|
|outgoing.apns.invalidnotificationsize|APNS-Fehler durch ungültige Benachrichtigungsgröße|Anzahl|Gesamt|Gibt die Anzahl von Pushvorgängen an, die nicht erfolgreich waren, weil die Nutzlast zu groß war (APNS-Statuscode: 7).|Keine|
|outgoing.apns.pnserror|APNS-Fehler|Anzahl|Gesamt|Gibt die Anzahl von Pushvorgängen an, die nicht erfolgreich waren, weil Fehler bei der Kommunikation mit APNS aufgetreten sind.|Keine|
|outgoing.gcm.success|Erfolgreiche GCM-Benachrichtigungen|Anzahl|Gesamt|Gibt die Anzahl von erfolgreichen Benachrichtigungen an.|Keine|
|outgoing.gcm.invalidcredentials|GCM-Autorisierungsfehler (ungültige Anmeldeinformationen)|Anzahl|Gesamt|Gibt die Anzahl von Pushvorgängen an, die nicht erfolgreich waren, weil das PNS die angegebenen Anmeldeinformationen nicht akzeptiert hat oder die Anmeldeinformationen blockiert wurden.|Keine|
|outgoing.gcm.badchannel|GCM-Fehler durch fehlerhaften Kanal|Anzahl|Gesamt|Gibt die Anzahl von Pushvorgängen an, die nicht erfolgreich waren, weil die registrationId in der Registrierung nicht erkannt wurde (GCM-Ergebnis: Ungültige Registrierung).|Keine|
|outgoing.gcm.expiredchannel|GCM-Fehler durch abgelaufenen Kanal|Anzahl|Gesamt|Gibt die Anzahl von Pushvorgängen an, die nicht erfolgreich waren, weil die registrationId in der Registrierung abgelaufen war (GCM-Ergebnis: NotRegistered).|Keine|
|outgoing.gcm.throttled|Gedrosselte GCM-Benachrichtigungen|Anzahl|Gesamt|Gibt die Anzahl von Pushvorgängen an, die nicht erfolgreich waren, weil diese App von GCM gedrosselt wurde (GCM-Statuscode: 501 - 599 oder result:Unavailable).|Keine|
|outgoing.gcm.invalidnotificationformat|Ungültiges GCM-Benachrichtigungsformat|Anzahl|Gesamt|Gibt die Anzahl von Pushvorgängen an, die nicht erfolgreich waren, weil die Nutzlast nicht richtig formatiert war (GCM-Ergebnis: InvalidDataKey oder InvalidTtl).|Keine|
|outgoing.gcm.invalidnotificationsize|GCM-Fehler durch ungültige Benachrichtigungsgröße|Anzahl|Gesamt|Gibt die Anzahl von Pushvorgängen an, die nicht erfolgreich waren, weil die Nutzlast zu groß war (GCM-Ergebnis: MessageTooBig).|Keine|
|outgoing.gcm.wrongchannel|GCM-Fehler durch falschen Kanal|Anzahl|Gesamt|Gibt die Anzahl von Pushvorgängen an, die nicht erfolgreich waren, weil die registrationId in der Registrierung nicht der aktuellen App zugeordnet war (GCM-Ergebnis: InvalidPackageName).|Keine|
|outgoing.gcm.pnserror|GCM-Fehler|Anzahl|Gesamt|Gibt die Anzahl von Pushvorgängen an, die nicht erfolgreich waren, weil Fehler bei der Kommunikation mit GCM aufgetreten sind.|Keine|
|outgoing.gcm.authenticationerror|GCM-Authentifizierungsfehler|Anzahl|Gesamt|Gibt die Anzahl von Pushvorgängen an, die nicht erfolgreich waren, weil die angegebenen Anmeldeinformationen vom PNS nicht akzeptiert wurden, die Anmeldeinformationen blockiert wurden oder die SenderId in der App nicht richtig konfiguriert wurde (GCM-Ergebnis: MismatchedSenderId).|Keine|
|outgoing.mpns.success|Erfolgreiche MPNS-Benachrichtigungen|Anzahl|Gesamt|Gibt die Anzahl von erfolgreichen Benachrichtigungen an.|Keine|
|outgoing.mpns.invalidcredentials|Ungültige MPNS-Anmeldeinformationen|Anzahl|Gesamt|Gibt die Anzahl von Pushvorgängen an, die nicht erfolgreich waren, weil das PNS die angegebenen Anmeldeinformationen nicht akzeptiert hat oder die Anmeldeinformationen blockiert wurden.|Keine|
|outgoing.mpns.badchannel|MPNS-Fehler durch fehlerhaften Kanal|Anzahl|Gesamt|Gibt die Anzahl von Pushvorgängen an, die nicht erfolgreich waren, weil der ChannelURI in der Registrierung nicht erkannt wurde (MPNS-Status: 404 – Nicht gefunden).|Keine|
|outgoing.mpns.throttled|Gedrosselte MPNS-Benachrichtigungen|Anzahl|Gesamt|Gibt die Anzahl von Pushvorgängen an, die nicht erfolgreich waren, weil diese App vom MPNS gedrosselt wird (WNS-MPNS: 406 – Nicht annehmbar).|Keine|
|outgoing.mpns.invalidnotificationformat|Ungültiges MPNS-Benachrichtigungsformat|Anzahl|Gesamt|Gibt die Anzahl von Pushvorgängen an, die nicht erfolgreich waren, weil die Nutzlast der Benachrichtigung zu groß war.|Keine|
|outgoing.mpns.channeldisconnected|MPNS-Kanal nicht verbunden|Anzahl|Gesamt|Gibt die Anzahl von Pushvorgängen an, die nicht erfolgreich waren, weil die Verbindung für den ChannelURI in der Registrierung getrennt wurde (MPNS-Status: 412 – Nicht gefunden).|Keine|
|outgoing.mpns.dropped|Verworfene MPNS-Benachrichtigungen|Anzahl|Gesamt|Gibt die Anzahl von Pushvorgängen an, die von MPNS verworfen wurden (MPNS-Antwortheader: X-NotificationStatus: QueueFull oder Suppressed).|Keine|
|outgoing.mpns.pnserror|MPNS-Fehler|Anzahl|Gesamt|Gibt die Anzahl von Pushvorgängen an, die nicht erfolgreich waren, weil Fehler bei der Kommunikation mit MPNS aufgetreten sind.|Keine|
|outgoing.mpns.authenticationerror|MPNS-Authentifizierungsfehler|Anzahl|Gesamt|Gibt die Anzahl von Pushvorgängen an, die nicht erfolgreich waren, weil das PNS die angegebenen Anmeldeinformationen nicht akzeptiert hat oder die Anmeldeinformationen blockiert wurden.|Keine|
|notificationhub.pushes|Alle ausgehenden Benachrichtigungen|Anzahl|Gesamt|Alle ausgehenden Benachrichtigungen des Benachrichtigungs-Hub|Keine|
|incoming.all.requests|Alle eingehenden Anforderungen|Anzahl|Gesamt|Gesamtzahl eingehender Anforderungen für einen Notification Hub|Keine|
|incoming.all.failedrequests|Alle eingehenden fehlerhaften Anforderungen|Anzahl|Gesamt|Gesamtzahl eingehender fehlerhafter Anforderungen für einen Notification Hub|Keine|

## <a name="microsoftoperationalinsightsworkspaces"></a>Microsoft.OperationalInsights/workspaces

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|Average_% Free Inodes|% freie Inodes|Anzahl|Average|Average_% Free Inodes|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_% Free Space|% freier Speicher|Anzahl|Average|Average_% Free Space|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_% Used Inodes|% verwendete Inodes|Anzahl|Average|Average_% Used Inodes|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_% Used Space|% verwendeter Speicher|Anzahl|Average|Average_% Used Space|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_Disk Read Bytes/sec|Byte gelesen/s|Anzahl|Average|Average_Disk Read Bytes/sec|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_Disk Reads/sec|Lesevorgänge/s|Anzahl|Average|Average_Disk Reads/sec|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_Disk Transfers/sec|Übertragungen/s|Anzahl|Average|Average_Disk Transfers/sec|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_Disk Write Bytes/sec|Byte geschrieben/s|Anzahl|Average|Average_Disk Write Bytes/sec|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_Disk Writes/sec|Schreibvorgänge/s|Anzahl|Average|Average_Disk Writes/sec|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_Free Megabytes|Freie Megabytes|Anzahl|Average|Average_Free Megabytes|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_Logical Disk Bytes/sec|Logischer Datenträger Bytes/s|Anzahl|Average|Average_Logical Disk Bytes/sec|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_% Available Memory|% verfügbarer Speicher|Anzahl|Average|Average_% Available Memory|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_% Available Swap Space|% verfügbarer Auslagerungsbereich|Anzahl|Average|Average_% Available Swap Space|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_% Used Memory|% verwendeter Arbeitsspeicher|Anzahl|Average|Average_% Used Memory|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_% Used Swap Space|% verwendeter Auslagerungsbereich|Anzahl|Average|Average_% Used Swap Space|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_Available MBytes Memory|Verfügbarer Speicher in MB|Anzahl|Average|Average_Available MBytes Memory|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_Available MBytes Swap|Verfügbarer Auslagerungsbereich in MB|Anzahl|Average|Average_Available MBytes Swap|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_Page Reads/sec|Seitenlesevorgänge/s|Anzahl|Average|Average_Page Reads/sec|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_Page Writes/sec|Schreibvorgänge/s|Anzahl|Average|Average_Page Writes/sec|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_Pages/sec|Seiten/s|Anzahl|Average|Average_Pages/sec|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_Used MBytes Swap Space|Verwendeter Auslagerungsbereich in MB|Anzahl|Average|Average_Used MBytes Swap Space|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_Used Memory MBytes|Verwendeter Speicher in MB|Anzahl|Average|Average_Used Memory MBytes|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_Total Bytes Transmitted|Summe übertragene Bytes|Anzahl|Average|Average_Total Bytes Transmitted|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_Total Bytes Received|Summe empfangene Bytes|Anzahl|Average|Average_Total Bytes Received|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_Total Bytes|Summe Bytes|Anzahl|Average|Average_Total Bytes|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_Total Packets Transmitted|Summe übermittelte Pakete|Anzahl|Average|Average_Total Packets Transmitted|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_Total Packets Received|Summe empfangene Pakete|Anzahl|Average|Average_Total Packets Received|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_Total Rx Errors|Summe Rx-Fehler|Anzahl|Average|Average_Total Rx Errors|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_Total Tx Errors|Summe Tx-Fehler|Anzahl|Average|Average_Total Tx Errors|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_Total Collisions|Summe Konflikte|Anzahl|Average|Average_Total Collisions|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_Avg. Datenträger s/gelesen|Durchschn. Datenträger s/gelesen|Anzahl|Average|Average_Avg. Datenträger s/gelesen|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_Avg. Datenträger s/übertragen|Durchschn. Datenträger s/übertragen|Anzahl|Average|Average_Avg. Datenträger s/übertragen|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_Avg. Datenträger s/geschrieben|Durchschn. Datenträger s/geschrieben|Anzahl|Average|Average_Avg. Datenträger s/geschrieben|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_Physical Disk Bytes/sec|Physischer Datenträger Bytes/s|Anzahl|Average|Average_Physical Disk Bytes/sec|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_Pct Privileged Time|PCT-privilegierte Zeit|Anzahl|Average|Average_Pct Privileged Time|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_Pct User Time|PCT-Benutzerzeit|Anzahl|Average|Average_Pct User Time|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_Used Memory kBytes|Verwendeter Arbeitsspeicher in KB|Anzahl|Average|Average_Used Memory kBytes|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_Virtual Shared Memory|Virtueller gemeinsamer Speicher|Anzahl|Average|Average_Virtual Shared Memory|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_% DPC Time|% DPC-Zeit|Anzahl|Average|Average_% DPC Time|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_% Idle Time|% Leerlaufzeit|Anzahl|Average|Average_% Idle Time|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_% Interrupt Time|% Interruptzeit|Anzahl|Average|Average_% Interrupt Time|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_% IO Wait Time|% E/a-Wartezeit|Anzahl|Average|Average_% IO Wait Time|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_% Nice Time|% Nice-Zeit|Anzahl|Average|Average_% Nice Time|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_% Privileged Time|% privilegierte Zeit|Anzahl|Average|Average_% Privileged Time|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_% Processor Time|% Prozessorzeit|Anzahl|Average|Average_% Processor Time|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_% User Time|% Benutzerzeit|Anzahl|Average|Average_% User Time|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_Free Physical Memory|Freier physischer Speicher|Anzahl|Average|Average_Free Physical Memory|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_Free Space in Paging Files|Freier Speicherplatz in Auslagerungsdateien|Anzahl|Average|Average_Free Space in Paging Files|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_Free Virtual Memory|Freier virtueller Arbeitsspeicher|Anzahl|Average|Average_Free Virtual Memory|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_Processes|Prozesse|Anzahl|Average|Average_Processes|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_Size Stored In Paging Files|Genutzter Speicherplatz in Auslagerungsdateien|Anzahl|Average|Average_Size Stored In Paging Files|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_Uptime|Betriebszeit|Anzahl|Average|Average_Uptime|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_Users|Benutzer|Anzahl|Average|Average_Users|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_Avg. Datenträger s/gelesen|Durchschn. Datenträger s/gelesen|Anzahl|Average|Average_Avg. Datenträger s/gelesen|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_Avg. Datenträger s/geschrieben|Durchschn. Datenträger s/geschrieben|Anzahl|Average|Average_Avg. Datenträger s/geschrieben|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_Current Disk Queue Length|Aktuelle Warteschlangenlänge|Anzahl|Average|Average_Current Disk Queue Length|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_Disk Reads/sec|Lesevorgänge/s|Anzahl|Average|Average_Disk Reads/sec|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_Disk Transfers/sec|Übertragungen/s|Anzahl|Average|Average_Disk Transfers/sec|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_Disk Writes/sec|Schreibvorgänge/s|Anzahl|Average|Average_Disk Writes/sec|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_Free Megabytes|Freie Megabytes|Anzahl|Average|Average_Free Megabytes|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_% Free Space|% freier Speicher|Anzahl|Average|Average_% Free Space|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_Available MBytes|Verfügbare MB|Anzahl|Average|Average_Available MBytes|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_% Committed Bytes In Use|Zugesicherte verwendete Bytes (%)|Anzahl|Average|Average_% Committed Bytes In Use|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_Bytes Received/sec|Empfangene Byte/Sek.|Anzahl|Average|Average_Bytes Received/sec|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_Bytes Sent/sec|Gesendete Byte/Sek.|Anzahl|Average|Average_Bytes Sent/sec|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_Bytes Total/sec|Byte gesamt/Sek.|Anzahl|Average|Average_Bytes Total/sec|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_% Processor Time|% Prozessorzeit|Anzahl|Average|Average_% Processor Time|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_Processor Queue Length|Länge der Prozessorwarteschlange|Anzahl|Average|Average_Processor Queue Length|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Heartbeat|Heartbeat|Anzahl|Gesamt|Heartbeat|Computer,OSType,Version,SourceComputerId|
|Aktualisieren|Aktualisieren|Anzahl|Average|Aktualisieren|Computer, Product, Classification, UpdateState, Optional, Approved|
|Ereignis|Ereignis|Anzahl|Average|Ereignis|Source, EventLog, Computer, EventCategory, EventLevel, EventLevelName, EventID|

## <a name="microsoftpeeringpeeringservices"></a>Microsoft.Peering/peeringServices

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|PrefixLatency|Präfixlatenz|Millisekunden|Average|Medianwert der Präfixlatenz|PrefixName|

## <a name="microsoftpeeringpeerings"></a>Microsoft.Peering/peerings

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|SessionAvailabilityV4|V4-Sitzungsverfügbarkeit|Percent|Average|Verfügbarkeit der V4-Sitzung|ConnectionId|
|SessionAvailabilityV6|Sitzungsverfügbarkeit V6|Percent|Average|Verfügbarkeit der V6-Sitzung|ConnectionId|
|IngressTrafficRate|Rate des eingehenden Datenverkehrs|BitsPerSecond|Average|Rate des eingehenden Datenverkehrs in Bit pro Sekunde|ConnectionId|
|EgressTrafficRate|Rate des ausgehenden Datenverkehrs|BitsPerSecond|Average|Rate des ausgehenden Datenverkehrs in Bit pro Sekunde|ConnectionId|

## <a name="microsoftpowerbidedicatedcapacities"></a>Microsoft.PowerBIDedicated/capacities

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|QueryDuration|Abfragedauer|Millisekunden|Average|DAX-Abfragedauer im letzten Intervall|Keine Dimensionen|
|QueryPoolJobQueueLength|Threads: Auftragswarteschlangenlänge für Abfragepool|Anzahl|Average|Anzahl von Aufträgen in der Warteschlange des Abfragethreadpools.|Keine Dimensionen|
|qpu_high_utilization_metric|Hohe QPU-Auslastung|Anzahl|Gesamt|Hohe QPU-Auslastung in der letzten Minute; 1 bei hoher QPU-Auslastung, andernfalls 0|Keine Dimensionen|
|memory_metric|Arbeitsspeicher|Byte|Average|Arbeitsspeicher. Bereich von 0–3 GB für A1, 0–5 GB für A2, 0–10 GB für A3, 0–25 GB für A4, 0–50 GB für A5 und 0–100 GB für A6|Keine Dimensionen|
|memory_thrashing_metric|Arbeitsspeicherüberlastung|Percent|Average|Durchschnittliche Arbeitsspeicherüberlastung.|Keine Dimensionen|



## <a name="microsoftrelaynamespaces"></a>Microsoft.Relay/namespaces

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|ListenerConnections-Success|ListenerConnections-Success|Anzahl|Gesamt|Erfolgreiche ListenerConnections für Microsoft.Relay.|EntityName, OperationResult|
|ListenerConnections-ClientError|ListenerConnections-ClientError|Anzahl|Gesamt|ClientError bei ListenerConnections für Microsoft.Relay.|EntityName, OperationResult|
|ListenerConnections-ServerError|ListenerConnections-ServerError|Anzahl|Gesamt|ServerError bei ListenerConnections für Microsoft.Relay.|EntityName, OperationResult|
|SenderConnections-Success|SenderConnections-Success|Anzahl|Gesamt|Erfolgreiche SenderConnections für Microsoft.Relay.|EntityName, OperationResult|
|SenderConnections-ClientError|SenderConnections-ClientError|Anzahl|Gesamt|ClientError bei SenderConnections für Microsoft.Relay.|EntityName, OperationResult|
|SenderConnections-ServerError|SenderConnections-ServerError|Anzahl|Gesamt|ServerError bei SenderConnections für Microsoft.Relay.|EntityName, OperationResult|
|ListenerConnections-TotalRequests|ListenerConnections-TotalRequests|Anzahl|Gesamt|Gesamtzahl der ListenerConnections für Microsoft.Relay.|EntityName|
|SenderConnections-TotalRequests|SenderConnections-TotalRequests|Anzahl|Gesamt|Gesamtzahl der SenderConnections-Anforderungen für Microsoft.Relay.|EntityName|
|ActiveConnections|ActiveConnections|Anzahl|Gesamt|Gesamtzahl der ActiveConnections für Microsoft.Relay.|EntityName|
|ActiveListeners|ActiveListeners|Anzahl|Gesamt|Gesamtzahl der ActiveListeners für Microsoft.Relay.|EntityName|
|BytesTransferred|BytesTransferred|Anzahl|Gesamt|Gesamtzahl der BytesTransferred für Microsoft.Relay.|EntityName|
|ListenerDisconnects|ListenerDisconnects|Anzahl|Gesamt|Gesamtzahl der ListenerDisconnects für Microsoft.Relay.|EntityName|
|SenderDisconnects|SenderDisconnects|Anzahl|Gesamt|Gesamtzahl der SenderDisconnects für Microsoft.Relay.|EntityName|


## <a name="microsoftsearchsearchservices"></a>Microsoft.Search/searchServices

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|SearchLatency|Suchlatenz|Sekunden|Average|Durchschnittliche Suchlatenz für den Suchdienst|Keine|
|SearchQueriesPerSecond|Suchabfragen pro Sekunde|Anzahl pro Sekunde|Average|Suchabfragen pro Sekunde für den Suchdienst|Keine|
|ThrottledSearchQueriesPercentage|Gedrosselte Suchabfragen in Prozent|Percent|Average|Prozentsatz der Suchabfragen, die für den Suchdienst gedrosselt wurden|Keine|


## <a name="microsoftservicebusnamespaces"></a>Microsoft.ServiceBus/namespaces

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|SuccessfulRequests|Erfolgreiche Anforderungen|Anzahl|Gesamt|Gesamtzahl der erfolgreichen Anforderungen für einen Namespace|EntityName, OperationResult|
|ServerErrors|Serverfehler.|Anzahl|Gesamt|Serverfehler für Microsoft.ServiceBus.|EntityName, OperationResult|
|UserErrors|Benutzerfehler.|Anzahl|Gesamt|Benutzerfehler für Microsoft.ServiceBus.|EntityName, OperationResult|
|ThrottledRequests|Gedrosselte Anforderungen.|Anzahl|Gesamt|Gedrosselte Anforderungen für Microsoft.ServiceBus.|EntityName, OperationResult|
|IncomingRequests|Eingehende Anforderungen|Anzahl|Gesamt|Eingehende Anforderungen für Microsoft.ServiceBus.|EntityName|
|IncomingMessages|Eingehende Nachrichten|Anzahl|Gesamt|Eingehende Nachrichten für Microsoft.ServiceBus.|EntityName|
|OutgoingMessages|Ausgehende Nachrichten|Anzahl|Gesamt|Ausgehende Nachrichten für Microsoft.ServiceBus.|EntityName|
|ActiveConnections|ActiveConnections|Anzahl|Gesamt|Aktive Verbindungen gesamt für Microsoft.ServiceBus.|Keine|
|ConnectionsOpened|Geöffnete Verbindungen.|Anzahl|Average|Geöffnete Verbindungen für Microsoft.ServiceBus.|EntityName|
|ConnectionsClosed|Geschlossene Verbindungen.|Anzahl|Average|Geschlossene Verbindungen für Microsoft.ServiceBus.|EntityName|
|Size|Size|Byte|Average|Größe einer Warteschlange/eines Themas in Bytes|EntityName|
|Meldungen|Anzahl von Nachrichten in einer Warteschlange/einem Thema|Anzahl|Average|Anzahl von Nachrichten in einer Warteschlange/einem Thema|EntityName|
|ActiveMessages|Anzahl von aktiven Nachrichten in einer Warteschlange/einem Thema|Anzahl|Average|Anzahl von aktiven Nachrichten in einer Warteschlange/einem Thema|EntityName|
|DeadletteredMessages|Anzahl von unzustellbaren Nachrichten in einer Warteschlange/einem Thema|Anzahl|Average|Anzahl von unzustellbaren Nachrichten in einer Warteschlange/einem Thema|EntityName|
|ScheduledMessages|Anzahl von geplanten Nachrichten in einer Warteschlange/einem Thema|Anzahl|Average|Anzahl von geplanten Nachrichten in einer Warteschlange/einem Thema|EntityName|
|NamespaceCpuUsage|CPU|Percent|Maximum|CPU-Auslastungsmetrik für Service Bus-Premium-Namespace|Replikat|
|NamespaceMemoryUsage|Speicherauslastung|Percent|Maximum|Speicherauslastungsmetrik für Service Bus-Premium-Namespace|Replikat|
|CPUXNS|CPU (veraltet)|Percent|Maximum|CPU-Auslastungsmetrik für Service Bus-Premium-Namespace Diese Metrik ist veraltet. Verwenden Sie stattdessen die CPU-Metrik (NamespaceCpuUsage).|Replikat|
|WSXNS|Speicherauslastung (Deprecated)|Percent|Maximum|Speicherauslastungsmetrik für Service Bus-Premium-Namespace Diese Metrik ist veraltet. Verwenden Sie stattdessen die Speicherauslastingsmetrik (NamespaceMemoryUsage).|Replikat|


## <a name="microsoftservicefabricmeshapplications"></a>Microsoft.ServiceFabricMesh/applications

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|AllocatedCpu|AllocatedCpu|Anzahl|Average|Diesem Container zugeordnete CPU in Millicores|ApplicationName,ServiceName,CodePackageName,ServiceReplicaName|
|AllocatedMemory|AllocatedMemory|Byte|Average|Diesem Container zugeordneter Arbeitsspeicher in MB|ApplicationName,ServiceName,CodePackageName,ServiceReplicaName|
|ActualCpu|ActualCpu|Anzahl|Average|Tatsächliche CPU-Auslastung in Millicores|ApplicationName,ServiceName,CodePackageName,ServiceReplicaName|
|ActualMemory|ActualMemory|Byte|Average|Tatsächliche Arbeitsspeichernutzung in MB|ApplicationName,ServiceName,CodePackageName,ServiceReplicaName|
|CpuUtilization|CpuUtilization|Percent|Average|Auslastung der CPU für diesen Container als Prozentsatz von AllocatedCpu|ApplicationName,ServiceName,CodePackageName,ServiceReplicaName|
|MemoryUtilization|MemoryUtilization|Percent|Average|Auslastung der CPU für diesen Container als Prozentsatz von AllocatedCpu|ApplicationName,ServiceName,CodePackageName,ServiceReplicaName|
|ApplicationStatus|ApplicationStatus|Anzahl|Average|Status der Service Fabric Mesh-Anwendung|ApplicationName,Status|
|ServiceStatus|ServiceStatus|Anzahl|Average|Integritätsstatus eines Diensts in Service Fabric Mesh-Anwendung|ApplicationName,Status,ServiceName|
|ServiceReplicaStatus|ServiceReplicaStatus|Anzahl|Average|Integritätsstatus eines Dienstreplikats in Service Fabric Mesh-Anwendung|ApplicationName,Status,ServiceName,ServiceReplicaName|
|ContainerStatus|ContainerStatus|Anzahl|Average|Status des Containers in der Service Fabric Mesh-Anwendung|ApplicationName,ServiceName,CodePackageName,ServiceReplicaName,Status|
|RestartCount|RestartCount|Anzahl|Average|Anzahl Neustarts eines Containers in Service Fabric Mesh-Anwendung|ApplicationName,Status,ServiceName,ServiceReplicaName,CodePackageName|

## <a name="microsoftsignalrservicesignalr"></a>Microsoft.SignalRService/SignalR

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|ConnectionCount|Anzahl der Verbindungen|Anzahl|Maximum|Die Anzahl der Benutzerverbindungen.|Endpunkt|
|MessageCount|Nachrichtenanzahl|Anzahl|Gesamt|Die Gesamtmenge der Nachrichten.|Keine|
|InboundTraffic|Eingehender Datenverkehr|Byte|Gesamt|Der eingehende Datenverkehr des Diensts|Keine|
|OutboundTraffic|Ausgehender Datenverkehr|Byte|Gesamt|Der ausgehende Datenverkehr des Diensts|Keine|
|UserErrors|Benutzerfehler|Percent|Maximum|Der Prozentsatz der Benutzerfehler|Keine|
|SystemErrors|Systemfehler|Percent|Maximum|Der Prozentsatz der Systemfehler|Keine|





## <a name="microsoftsqlserversdatabases"></a>Microsoft.Sql/servers/databases

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|cpu_percent|CPU-Prozentsatz|Percent|Average|CPU-Prozentsatz|Keine|
|physical_data_read_percent|E/A-Prozentsatz für Daten|Percent|Average|E/A-Prozentsatz für Daten|Keine|
|log_write_percent|E/A-Prozentsatz für Protokoll|Percent|Average|E/A-Prozentsatz für Protokoll. Gilt nicht für Data Warehouses.|Keine|
|dtu_consumption_percent|DTU-Prozentsatz|Percent|Average|DTU-Prozentsatz. Gilt für DTU-basierte Datenbanken.|Keine|
|storage|Genutzter Datenspeicherplatz|Byte|Maximum|Genutzter Datenspeicherplatz. Gilt nicht für Data Warehouses.|Keine|
|connection_successful|Erfolgreiche Verbindungen|Anzahl|Gesamt|Erfolgreiche Verbindungen|Keine|
|connection_failed|Verbindungsfehler|Anzahl|Gesamt|Verbindungsfehler|Keine|
|blocked_by_firewall|Von der Firewall blockiert|Anzahl|Gesamt|Von der Firewall blockiert|Keine|
|deadlock|Deadlocks|Anzahl|Gesamt|Deadlocks. Gilt nicht für Data Warehouses.|Keine|
|storage_percent|Genutzter Datenspeicherplatz in Prozent|Percent|Maximum|Genutzter Datenspeicherplatz in Prozent. Gilt nicht für Data Warehouses oder Hyperscale-Datenbanken.|Keine|
|xtp_storage_percent|In-Memory-OLTP-Speicher in Prozent|Percent|Average|In-Memory-OLTP-Speicher in Prozent. Gilt nicht für Data Warehouses.|Keine|
|workers_percent|Worker in Prozent|Percent|Average|Worker in Prozent. Gilt nicht für Data Warehouses.|Keine|
|sessions_percent|Sitzungen in Prozent|Percent|Average|Sitzungen in Prozent. Gilt nicht für Data Warehouses.|Keine|
|dtu_limit|DTU-Grenzwert|Anzahl|Average|DTU-Grenzwert. Gilt für DTU-basierte Datenbanken.|Keine|
|dtu_used|DTU-Verbrauch|Anzahl|Average|DTU-Verbrauch. Gilt für DTU-basierte Datenbanken.|Keine|
|cpu_limit|CPU-Grenzwert|Anzahl|Average|CPU-Grenzwert. Gilt für V-Kern-basierte Datenbanken.|Keine|
|cpu_used|Verwendete CPU|Anzahl|Average|Verwendete CPU. Gilt für V-Kern-basierte Datenbanken.|Keine|
|dwu_limit|DWU-Grenzwert|Anzahl|Maximum|DWU-Grenzwert. Gilt nur für Data Warehouses.|Keine|
|dwu_consumption_percent|DWU in Prozent|Percent|Maximum|DWU in Prozent. Gilt nur für Data Warehouses.|Keine|
|dwu_used|DWU-Verbrauch|Anzahl|Maximum|DWU-Verbrauch. Gilt nur für Data Warehouses.|Keine|
|cache_hit_percent|Prozentsatz der Cachetreffer|Percent|Maximum|Prozentsatz der Cachetreffer. Gilt nur für Data Warehouses.|Keine|
|cache_used_percent|Cacheverwendung in Prozent|Percent|Maximum|Cacheverwendung in Prozent. Gilt nur für Data Warehouses.|Keine|
|sqlserver_process_core_percent|SQL Server-Prozess: Kern (in Prozent)|Percent|Maximum|CPU-Auslastung als Prozentsatz des SQL-DB-Prozesses. Gilt nicht für Data Warehouses.|Keine|
|sqlserver_process_memory_percent|SQL Server-Prozess: Arbeitsspeicher (in Prozent)|Percent|Maximum|Speicherauslastung als Prozentsatz des SQL-DB-Prozesses. Gilt nicht für Data Warehouses.|Keine|
|tempdb_data_size|Größe der tempdb-Datendatei in Kilobytes|Anzahl|Maximum|Die Größe der tempdb-Datendatei in Kilobytes. Gilt nicht für Data Warehouses.|Keine|
|tempdb_log_size|Größe der tempdb-Protokolldatei in Kilobytes|Anzahl|Maximum|Die Größe der tempdb-Protokolldatei in Kilobytes. Gilt nicht für Data Warehouses.|Keine|
|tempdb_log_used_percent|Nutzung des tempdb-Protokolls in Prozent|Percent|Maximum|Die Nutzung des tempdb-Protokolls in Prozent. Gilt nicht für Data Warehouses.|Keine|
|local_tempdb_usage_percent|Lokaler tempdb-Prozentsatz|Percent|Average|Lokaler tempdb-Prozentsatz. Gilt nur für Data Warehouses.|Keine|
|app_cpu_billed|Abgerechnete App-CPU|Anzahl|Gesamt|Abgerechnete App-CPU. Gilt für serverlose Datenbanken.|Keine|
|app_cpu_percent|App-CPU-Prozentsatz|Percent|Average|App-CPU-Prozentsatz. Gilt für serverlose Datenbanken.|Keine|
|app_memory_percent|App-Arbeitsspeicherprozentsatz|Percent|Average|App-Arbeitsspeicherprozentsatz. Gilt für serverlose Datenbanken.|Keine|
|allocated_data_storage|Zugeordneter Datenspeicherplatz|Byte|Average|Zugeordneter Datenspeicher. Gilt nicht für Data Warehouses.|Keine|
|memory_usage_percent|Arbeitsspeicherprozentsatz|Percent|Maximum|Arbeitsspeicherprozentsatz. Gilt nur für Data Warehouses.|Keine|
|full_backup_size_bytes|Speichergröße für vollständige Sicherungen|Byte|Maximum|Speichergröße für kumulative vollständige Sicherungen Gilt für V-Kern-basierte Datenbanken. Gilt nicht für Hyperscale-Datenbanken.|Keine|
|diff_backup_size_bytes|Speichergröße für differenzielle Sicherungen|Byte|Maximum|Speichergröße für kumulative differenzielle Sicherungen Gilt für V-Kern-basierte Datenbanken. Gilt nicht für Hyperscale-Datenbanken.|Keine|
|log_backup_size_bytes|Speichergröße für Protokollsicherungen|Byte|Maximum|Speichergröße für kumulative Protokollsicherungen Gilt für V-Kern-basierte Datenbanken. Gilt nicht für Hyperscale-Datenbanken.|Keine|


## <a name="microsoftsqlserverselasticpools"></a>Microsoft.Sql/servers/elasticPools

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|cpu_percent|CPU-Prozentsatz|Percent|Average|CPU-Prozentsatz|Keine|
|database_cpu_percent|CPU-Prozentsatz|Percent|Average|CPU-Prozentsatz|DatabaseResourceId|
|physical_data_read_percent|E/A-Prozentsatz für Daten|Percent|Average|E/A-Prozentsatz für Daten|Keine|
|database_physical_data_read_percent|E/A-Prozentsatz für Daten|Percent|Average|E/A-Prozentsatz für Daten|DatabaseResourceId|
|log_write_percent|E/A-Prozentsatz für Protokoll|Percent|Average|E/A-Prozentsatz für Protokoll|Keine|
|database_log_write_percent|E/A-Prozentsatz für Protokoll|Percent|Average|E/A-Prozentsatz für Protokoll|DatabaseResourceId|
|dtu_consumption_percent|DTU-Prozentsatz|Percent|Average|DTU-Prozentsatz. Gilt für DTU-basierte Pools für elastische Datenbanken.|Keine|
|database_dtu_consumption_percent|DTU-Prozentsatz|Percent|Average|DTU-Prozentsatz|DatabaseResourceId|
|storage_percent|Genutzter Datenspeicherplatz in Prozent|Percent|Average|Genutzter Datenspeicherplatz in Prozent|Keine|
|workers_percent|Worker in Prozent|Percent|Average|Worker in Prozent|Keine|
|database_workers_percent|Worker in Prozent|Percent|Average|Worker in Prozent|DatabaseResourceId|
|sessions_percent|Sitzungen in Prozent|Percent|Average|Sitzungen in Prozent|Keine|
|database_sessions_percent|Sitzungen in Prozent|Percent|Average|Sitzungen in Prozent|DatabaseResourceId|
|eDTU_limit|eDTU-Grenzwert|Anzahl|Average|eDTU-Grenzwert. Gilt für DTU-basierte Pools für elastische Datenbanken.|Keine|
|storage_limit|Maximale Datengröße|Byte|Average|Maximale Datengröße|Keine|
|eDTU_used|eDTU-Verbrauch|Anzahl|Average|eDTU-Verbrauch. Gilt für DTU-basierte Pools für elastische Datenbanken.|Keine|
|database_eDTU_used|eDTU-Verbrauch|Anzahl|Average|eDTU-Verbrauch|DatabaseResourceId|
|storage_used|Genutzter Datenspeicherplatz|Byte|Average|Genutzter Datenspeicherplatz|Keine|
|database_storage_used|Genutzter Datenspeicherplatz|Byte|Average|Genutzter Datenspeicherplatz|DatabaseResourceId|
|xtp_storage_percent|In-Memory-OLTP-Speicher in Prozent|Percent|Average|In-Memory-OLTP-Speicher in Prozent|Keine|
|cpu_limit|CPU-Grenzwert|Anzahl|Average|CPU-Grenzwert. Gilt für V-Kern-basierte Pools für elastische Datenbanken.|Keine|
|database_cpu_limit|CPU-Grenzwert|Anzahl|Average|CPU-Grenzwert|DatabaseResourceId|
|cpu_used|Verwendete CPU|Anzahl|Average|Verwendete CPU. Gilt für V-Kern-basierte Pools für elastische Datenbanken.|Keine|
|database_cpu_used|Verwendete CPU|Anzahl|Average|Verwendete CPU|DatabaseResourceId|
|sqlserver_process_core_percent|SQL Server-Prozess: Kern (in Prozent)|Percent|Maximum|CPU-Auslastung als Prozentsatz des SQL-DB-Prozesses. Bezieht sich auf Pools für elastische Datenbanken.|Keine|
|sqlserver_process_memory_percent|SQL Server-Prozess: Arbeitsspeicher (in Prozent)|Percent|Maximum|Speicherauslastung als Prozentsatz des SQL-DB-Prozesses. Bezieht sich auf Pools für elastische Datenbanken.|Keine|
|tempdb_data_size|Größe der tempdb-Datendatei in Kilobytes|Anzahl|Maximum|Größe der tempdb-Datendatei in Kilobytes|Keine|
|tempdb_log_size|Größe der tempdb-Protokolldatei in Kilobytes|Anzahl|Maximum|Größe der tempdb-Protokolldatei in Kilobytes|Keine|
|tempdb_log_used_percent|Nutzung des tempdb-Protokolls in Prozent|Percent|Maximum|Nutzung des tempdb-Protokolls in Prozent|Keine|
|allocated_data_storage|Zugeordneter Datenspeicherplatz|Byte|Average|Zugeordneter Datenspeicherplatz|Keine|
|database_allocated_data_storage|Zugeordneter Datenspeicherplatz|Byte|Average|Zugeordneter Datenspeicherplatz|DatabaseResourceId|
|allocated_data_storage_percent|Zugeordneter Datenspeicherplatz in Prozent|Percent|Maximum|Zugeordneter Datenspeicherplatz in Prozent|Keine|

## <a name="microsoftsqlservers"></a>Microsoft.Sql/servers

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|dtu_consumption_percent|DTU-Prozentsatz|Percent|Average|DTU-Prozentsatz|ElasticPoolResourceId|
|database_dtu_consumption_percent|DTU-Prozentsatz|Percent|Average|DTU-Prozentsatz|DatabaseResourceId,ElasticPoolResourceId|
|storage_used|Genutzter Datenspeicherplatz|Byte|Average|Genutzter Datenspeicherplatz|ElasticPoolResourceId|
|database_storage_used|Genutzter Datenspeicherplatz|Byte|Average|Genutzter Datenspeicherplatz|DatabaseResourceId,ElasticPoolResourceId|
|dtu_used|DTU-Verbrauch|Anzahl|Average|DTU-Verbrauch|DatabaseResourceId|

## <a name="microsoftsqlmanagedinstances"></a>Microsoft.Sql/managedInstances

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|virtual_core_count|Anzahl virtueller Kerne|Anzahl|Average|Anzahl virtueller Kerne|Keine|
|avg_cpu_percent|Durchschnittlicher CPU-Prozentsatz|Percent|Average|Durchschnittlicher CPU-Prozentsatz|Keine|
|reserved_storage_mb|Reservierter Speicherplatz|Anzahl|Average|Reservierter Speicherplatz|Keine|
|storage_space_used_mb|Belegter Speicherplatz|Anzahl|Average|Belegter Speicherplatz|Keine|
|io_requests|Anzahl von E/A-Anforderungen|Anzahl|Average|Anzahl von E/A-Anforderungen|Keine|
|io_bytes_read|Gelesene E/A-Bytes|Byte|Average|Gelesene E/A-Bytes|Keine|
|io_bytes_written|Geschriebene E/A-Bytes|Byte|Average|Geschriebene E/A-Bytes|Keine|



## <a name="microsoftstoragestorageaccounts"></a>Microsoft.Storage/storageAccounts

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|UsedCapacity|Verwendete Kapazität|Byte|Average|Vom Konto verwendete Kapazität|Keine|
|Transaktionen|Transaktionen|Anzahl|Gesamt|Die Anzahl von Anforderungen, die an einen Speicherdienst oder an den angegebenen API-Vorgang gerichtet wurden. Diese Anzahl umfasst erfolgreiche und fehlgeschlagene Anforderungen sowie Anforderungen, die Fehler erzeugt haben. Verwenden Sie die Dimension „ResponseType“ für die Anzahl von verschiedenen Antworttypen.|ResponseType, GeoType, ApiName, Authentication|
|Eingehende Daten|Eingehende Daten|Byte|Gesamt|Die Menge der Eingangsdaten in Byte. Dieser Wert umfasst an Azure Storage gerichtete eingehende Daten von einem externen Client sowie eingehende Daten innerhalb von Azure.|GeoType, ApiName, Authentication|
|Ausgehende Daten|Ausgehende Daten|Byte|Gesamt|Die Menge der Ausgangsdaten in Byte. Dieser Wert umfasst an Azure Storage gerichtete ausgehende Daten von einem externen Client sowie ausgehende Daten innerhalb von Azure. Der Wert stellt somit keine gebührenpflichtigen ausgehenden Daten dar.|GeoType, ApiName, Authentication|
|SuccessServerLatency|Serverlatenz (erfolgreich)|Millisekunden|Average|Die durchschnittliche Latenz in Millisekunden, die von Azure Storage verwendet wurde, um eine erfolgreiche Anforderung zu verarbeiten. Dieser Wert enthält nicht die in „AverageE2ELatency“ angegebene Netzwerklatenz.|GeoType, ApiName, Authentication|
|SuccessE2ELatency|E2E-Latenz (erfolgreich)|Millisekunden|Average|Die durchschnittliche End-to-End-Latenz für erfolgreiche Anforderungen in Millisekunden, die an einen Speicherdienst oder den angegebenen API-Vorgang gesendet wurden. Dieser Wert umfasst die erforderliche Verarbeitungszeit in Azure Storage für das Lesen der Anforderung, das Senden der Antwort und das Empfangen der Antwortbestätigung.|GeoType, ApiName, Authentication|
|Verfügbarkeit|Verfügbarkeit|Percent|Average|Die Verfügbarkeit (in Prozent) für den Speicherdienst oder den angegebenen API-Vorgang. Die Verfügbarkeit wird berechnet, indem der Wert „TotalBillableRequests“ durch die Anzahl von zutreffenden Anforderungen, einschließlich der, die unerwartete Fehler erzeugt haben, geteilt wird. Alle unerwarteten Fehler verringern die Verfügbarkeit für den Speicherdienst oder den angegebenen API-Vorgang.|GeoType, ApiName, Authentication|

## <a name="microsoftstoragestorageaccountsblobservices"></a>Microsoft.Storage/storageAccounts/blobServices

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|BlobCapacity|Blob-Kapazität|Byte|Average|Die Größe des vom Blob-Dienst des Speicherkontos genutzten Speichers in Byte.|BlobType, Tier|
|BlobCount|Anzahl von Blobs|Anzahl|Average|Die Anzahl von Blobs im Blob-Dienst des Speicherkontos.|BlobType, Tier|
|ContainerCount|Anzahl von Blob-Containern|Anzahl|Average|Die Anzahl von Containern im Blob-Dienst des Speicherkontos.|Keine|
|IndexCapacity|Indexkapazität|Byte|Average|Die vom ADLS Gen2-Index (hierarchisch) verwendete Speichermenge in Byte.|Keine|
|Transaktionen|Transaktionen|Anzahl|Gesamt|Die Anzahl von Anforderungen, die an einen Speicherdienst oder an den angegebenen API-Vorgang gerichtet wurden. Diese Anzahl umfasst erfolgreiche und fehlgeschlagene Anforderungen sowie Anforderungen, die Fehler erzeugt haben. Verwenden Sie die Dimension „ResponseType“ für die Anzahl von verschiedenen Antworttypen.|ResponseType, GeoType, ApiName, Authentication|
|Eingehende Daten|Eingehende Daten|Byte|Gesamt|Die Menge der Eingangsdaten in Byte. Dieser Wert umfasst an Azure Storage gerichtete eingehende Daten von einem externen Client sowie eingehende Daten innerhalb von Azure.|GeoType, ApiName, Authentication|
|Ausgehende Daten|Ausgehende Daten|Byte|Gesamt|Die Menge der Ausgangsdaten in Byte. Dieser Wert umfasst an Azure Storage gerichtete ausgehende Daten von einem externen Client sowie ausgehende Daten innerhalb von Azure. Der Wert stellt somit keine gebührenpflichtigen ausgehenden Daten dar.|GeoType, ApiName, Authentication|
|SuccessServerLatency|Serverlatenz (erfolgreich)|Millisekunden|Average|Die durchschnittliche Latenz in Millisekunden, die von Azure Storage verwendet wurde, um eine erfolgreiche Anforderung zu verarbeiten. Dieser Wert enthält nicht die in „AverageE2ELatency“ angegebene Netzwerklatenz.|GeoType, ApiName, Authentication|
|SuccessE2ELatency|E2E-Latenz (erfolgreich)|Millisekunden|Average|Die durchschnittliche End-to-End-Latenz für erfolgreiche Anforderungen in Millisekunden, die an einen Speicherdienst oder den angegebenen API-Vorgang gesendet wurden. Dieser Wert umfasst die erforderliche Verarbeitungszeit in Azure Storage für das Lesen der Anforderung, das Senden der Antwort und das Empfangen der Antwortbestätigung.|GeoType, ApiName, Authentication|
|Verfügbarkeit|Verfügbarkeit|Percent|Average|Die Verfügbarkeit (in Prozent) für den Speicherdienst oder den angegebenen API-Vorgang. Die Verfügbarkeit wird berechnet, indem der Wert „TotalBillableRequests“ durch die Anzahl von zutreffenden Anforderungen, einschließlich der, die unerwartete Fehler erzeugt haben, geteilt wird. Alle unerwarteten Fehler verringern die Verfügbarkeit für den Speicherdienst oder den angegebenen API-Vorgang.|GeoType, ApiName, Authentication|

## <a name="microsoftstoragestorageaccountstableservices"></a>Microsoft.Storage/storageAccounts/tableServices

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|TableCapacity|Tabellenkapazität|Byte|Average|Die Größe des vom Tabellendienst des Speicherkontos genutzten Speichers in Byte.|Keine|
|TableCount|Anzahl von Tabellen|Anzahl|Average|Die Anzahl von Tabellen im Tabellendienst des Speicherkontos|Keine|
|TableEntityCount|Anzahl von Tabellenentitäten|Anzahl|Average|Die Anzahl von Tabellenentitäten im Tabellendienst des Speicherkontos|Keine|
|Transaktionen|Transaktionen|Anzahl|Gesamt|Die Anzahl von Anforderungen, die an einen Speicherdienst oder an den angegebenen API-Vorgang gerichtet wurden. Diese Anzahl umfasst erfolgreiche und fehlgeschlagene Anforderungen sowie Anforderungen, die Fehler erzeugt haben. Verwenden Sie die Dimension „ResponseType“ für die Anzahl von verschiedenen Antworttypen.|ResponseType, GeoType, ApiName, Authentication|
|Eingehende Daten|Eingehende Daten|Byte|Gesamt|Die Menge der Eingangsdaten in Byte. Dieser Wert umfasst an Azure Storage gerichtete eingehende Daten von einem externen Client sowie eingehende Daten innerhalb von Azure.|GeoType, ApiName, Authentication|
|Ausgehende Daten|Ausgehende Daten|Byte|Gesamt|Die Menge der Ausgangsdaten in Byte. Dieser Wert umfasst an Azure Storage gerichtete ausgehende Daten von einem externen Client sowie ausgehende Daten innerhalb von Azure. Der Wert stellt somit keine gebührenpflichtigen ausgehenden Daten dar.|GeoType, ApiName, Authentication|
|SuccessServerLatency|Serverlatenz (erfolgreich)|Millisekunden|Average|Die durchschnittliche Latenz in Millisekunden, die von Azure Storage verwendet wurde, um eine erfolgreiche Anforderung zu verarbeiten. Dieser Wert enthält nicht die in „AverageE2ELatency“ angegebene Netzwerklatenz.|GeoType, ApiName, Authentication|
|SuccessE2ELatency|E2E-Latenz (erfolgreich)|Millisekunden|Average|Die durchschnittliche End-to-End-Latenz für erfolgreiche Anforderungen in Millisekunden, die an einen Speicherdienst oder den angegebenen API-Vorgang gesendet wurden. Dieser Wert umfasst die erforderliche Verarbeitungszeit in Azure Storage für das Lesen der Anforderung, das Senden der Antwort und das Empfangen der Antwortbestätigung.|GeoType, ApiName, Authentication|
|Verfügbarkeit|Verfügbarkeit|Percent|Average|Die Verfügbarkeit (in Prozent) für den Speicherdienst oder den angegebenen API-Vorgang. Die Verfügbarkeit wird berechnet, indem der Wert „TotalBillableRequests“ durch die Anzahl von zutreffenden Anforderungen, einschließlich der, die unerwartete Fehler erzeugt haben, geteilt wird. Alle unerwarteten Fehler verringern die Verfügbarkeit für den Speicherdienst oder den angegebenen API-Vorgang.|GeoType, ApiName, Authentication|

## <a name="microsoftstoragestorageaccountsfileservices"></a>Microsoft.Storage/storageAccounts/fileServices

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|FileCapacity|Dateikapazität|Byte|Average|Die Größe des vom Dateidienst des Speicherkontos genutzten Speichers in Byte.|FileShare|
|FileCount|Dateianzahl|Anzahl|Average|Die Anzahl von Dateien im Dateidienst des Speicherkontos|FileShare|
|FileShareCount|Anzahl von Dateifreigaben|Anzahl|Average|Die Anzahl von Dateifreigaben im Dateidienst des Speicherkontos|Keine|
|FileShareSnapshotCount|Anzahl von Momentaufnahmen in Dateifreigabe|Anzahl|Average|Die Anzahl der enthaltenen Momentaufnahmen im Dateidienst des Speicherkontos in der Freigabe|FileShare|
|FileShareSnapshotSize|Größe der Momentaufnahmen in Dateifreigabe|Byte|Average|Die Speichermenge in Byte, die von den Momentaufnahmen im Dateidienst des Speicherkontos verwendet wird.|FileShare|
|FileShareQuota|Kontingentgröße für Dateifreigabe|Byte|Average|Die Obergrenze für die Speichermenge zur Verwendung durch den Azure Files-Dienst, angegeben in Byte.|FileShare|
|Transaktionen|Transaktionen|Anzahl|Gesamt|Die Anzahl von Anforderungen, die an einen Speicherdienst oder an den angegebenen API-Vorgang gerichtet wurden. Diese Anzahl umfasst erfolgreiche und fehlgeschlagene Anforderungen sowie Anforderungen, die Fehler erzeugt haben. Verwenden Sie die Dimension „ResponseType“ für die Anzahl von verschiedenen Antworttypen.|ResponseType, GeoType, ApiName, Authentication, FileShare|
|Eingehende Daten|Eingehende Daten|Byte|Gesamt|Die Menge der Eingangsdaten in Byte. Dieser Wert umfasst an Azure Storage gerichtete eingehende Daten von einem externen Client sowie eingehende Daten innerhalb von Azure.|GeoType, ApiName, Authentication, FileShare|
|Ausgehende Daten|Ausgehende Daten|Byte|Gesamt|Die Menge der Ausgangsdaten in Byte. Dieser Wert umfasst an Azure Storage gerichtete ausgehende Daten von einem externen Client sowie ausgehende Daten innerhalb von Azure. Der Wert stellt somit keine gebührenpflichtigen ausgehenden Daten dar.|GeoType, ApiName, Authentication, FileShare|
|SuccessServerLatency|Serverlatenz (erfolgreich)|Millisekunden|Average|Die durchschnittliche Latenz in Millisekunden, die von Azure Storage verwendet wurde, um eine erfolgreiche Anforderung zu verarbeiten. Dieser Wert enthält nicht die in „AverageE2ELatency“ angegebene Netzwerklatenz.|GeoType, ApiName, Authentication, FileShare|
|SuccessE2ELatency|E2E-Latenz (erfolgreich)|Millisekunden|Average|Die durchschnittliche End-to-End-Latenz für erfolgreiche Anforderungen in Millisekunden, die an einen Speicherdienst oder den angegebenen API-Vorgang gesendet wurden. Dieser Wert umfasst die erforderliche Verarbeitungszeit in Azure Storage für das Lesen der Anforderung, das Senden der Antwort und das Empfangen der Antwortbestätigung.|GeoType, ApiName, Authentication, FileShare|
|Verfügbarkeit|Verfügbarkeit|Percent|Average|Die Verfügbarkeit (in Prozent) für den Speicherdienst oder den angegebenen API-Vorgang. Die Verfügbarkeit wird berechnet, indem der Wert „TotalBillableRequests“ durch die Anzahl von zutreffenden Anforderungen, einschließlich der, die unerwartete Fehler erzeugt haben, geteilt wird. Alle unerwarteten Fehler verringern die Verfügbarkeit für den Speicherdienst oder den angegebenen API-Vorgang.|GeoType, ApiName, Authentication, FileShare|

## <a name="microsoftstoragestorageaccountsqueueservices"></a>Microsoft.Storage/storageAccounts/queueServices

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|QueueCapacity|Warteschlangenkapazität|Byte|Average|Die Größe des vom Warteschlangendienst des Speicherkontos genutzten Speichers in Byte.|Keine|
|QueueCount|Anzahl von Warteschlangen|Anzahl|Average|Die Anzahl von Warteschlangen im Warteschlangendienst des Speicherkontos|Keine|
|QueueMessageCount|Anzahl von Warteschlangennachrichten|Anzahl|Average|Die ungefähre Anzahl von Warteschlangennachrichten im Warteschlangendienst des Speicherkontos|Keine|
|Transaktionen|Transaktionen|Anzahl|Gesamt|Die Anzahl von Anforderungen, die an einen Speicherdienst oder an den angegebenen API-Vorgang gerichtet wurden. Diese Anzahl umfasst erfolgreiche und fehlgeschlagene Anforderungen sowie Anforderungen, die Fehler erzeugt haben. Verwenden Sie die Dimension „ResponseType“ für die Anzahl von verschiedenen Antworttypen.|ResponseType, GeoType, ApiName, Authentication|
|Eingehende Daten|Eingehende Daten|Byte|Gesamt|Die Menge der Eingangsdaten in Byte. Dieser Wert umfasst an Azure Storage gerichtete eingehende Daten von einem externen Client sowie eingehende Daten innerhalb von Azure.|GeoType, ApiName, Authentication|
|Ausgehende Daten|Ausgehende Daten|Byte|Gesamt|Die Menge der Ausgangsdaten in Byte. Dieser Wert umfasst an Azure Storage gerichtete ausgehende Daten von einem externen Client sowie ausgehende Daten innerhalb von Azure. Der Wert stellt somit keine gebührenpflichtigen ausgehenden Daten dar.|GeoType, ApiName, Authentication|
|SuccessServerLatency|Serverlatenz (erfolgreich)|Millisekunden|Average|Die durchschnittliche Latenz in Millisekunden, die von Azure Storage verwendet wurde, um eine erfolgreiche Anforderung zu verarbeiten. Dieser Wert enthält nicht die in „AverageE2ELatency“ angegebene Netzwerklatenz.|GeoType, ApiName, Authentication|
|SuccessE2ELatency|E2E-Latenz (erfolgreich)|Millisekunden|Average|Die durchschnittliche End-to-End-Latenz für erfolgreiche Anforderungen in Millisekunden, die an einen Speicherdienst oder den angegebenen API-Vorgang gesendet wurden. Dieser Wert umfasst die erforderliche Verarbeitungszeit in Azure Storage für das Lesen der Anforderung, das Senden der Antwort und das Empfangen der Antwortbestätigung.|GeoType, ApiName, Authentication|
|Verfügbarkeit|Verfügbarkeit|Percent|Average|Die Verfügbarkeit (in Prozent) für den Speicherdienst oder den angegebenen API-Vorgang. Die Verfügbarkeit wird berechnet, indem der Wert „TotalBillableRequests“ durch die Anzahl von zutreffenden Anforderungen, einschließlich der, die unerwartete Fehler erzeugt haben, geteilt wird. Alle unerwarteten Fehler verringern die Verfügbarkeit für den Speicherdienst oder den angegebenen API-Vorgang.|GeoType, ApiName, Authentication|





## <a name="microsoftstoragecachecaches"></a>Microsoft.StorageCache/caches

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|ClientIOPS|Client-IOPS insgesamt|Anzahl|Average|Die Rate der vom Cache verarbeiteten Clientdateivorgänge.|Keine|
|ClientLatency|Durchschnittliche Clientlatenz|Millisekunden|Average|Durchschnittliche Latenz von Clientdateivorgängen im Speichercache.|Keine|
|ClientReadIOPS|Lese-IOPS auf Client|Anzahl pro Sekunde|Average|Clientlesevorgänge pro Sekunde.|Keine|
|ClientReadThroughput|Durchschnittlicher Durchsatz von Cachelesevorgängen|Bytes pro Sekunde|Average|Übertragungsrate für Clientlesedaten.|Keine|
|ClientWriteIOPS|Schreib-IOPS auf Client|Anzahl pro Sekunde|Average|Clientschreibvorgänge pro Sekunde.|Keine|
|ClientWriteThroughput|Durchschnittlicher Durchsatz von Cacheschreibvorgängen|Bytes pro Sekunde|Average|Übertragungsrate für Clientschreibdaten.|Keine|
|ClientMetadataReadIOPS|IOPS für Metadatenlesevorgänge durch Client|Anzahl pro Sekunde|Average|Die Rate der an den Cache gesendeten Clientdateivorgänge, ausgenommen Datenlesevorgänge, die den persistenten Zustand nicht ändern.|Keine|
|ClientMetadataWriteIOPS|IOPS für Metadatenschreibvorgänge durch Client|Anzahl pro Sekunde|Average|Die Rate der an den Cache gesendeten Clientdateivorgänge, ausgenommen Datenschreibvorgänge, die den persistenten Zustand ändern.|Keine|
|ClientLockIOPS|IOPS für Clientsperren|Anzahl pro Sekunde|Average|Clientdatei-Sperrvorgänge pro Sekunde.|Keine|
|StorageTargetHealth|Integrität des Speicherziels|Anzahl|Average|Boolesche Ergebnisse des Konnektivitätstests zwischen dem Cache und den Speicherzielen.|Keine|
|Betriebszeit|Betriebszeit|Anzahl|Average|Boolesche Ergebnisse des Konnektivitätstests zwischen dem Cache und dem Überwachungssystem.|Keine|
|StorageTargetIOPS|Speicherziel-IOPS insgesamt|Anzahl|Average|Die Rate aller Dateivorgänge, die der Cache an ein bestimmtes Speicherziel sendet.|StorageTarget|
|StorageTargetWriteIOPS|Schreib-IOPS im Speicherziel|Anzahl|Average|Die Rate der Dateischreibvorgänge, die der Cache an ein bestimmtes Speicherziel sendet.|StorageTarget|
|StorageTargetAsyncWriteThroughput|Asynchroner Schreibdurchsatz im Speicherziel|Bytes pro Sekunde|Average|Die Rate, mit der der Cache Daten asynchron in ein bestimmtes Speicherziel schreibt. Dabei handelt es sich um opportunistische Schreibvorgänge, durch die Clients nicht blockiert werden.|StorageTarget|
|StorageTargetSyncWriteThroughput|Synchroner Schreibdurchsatz im Speicherziel|Bytes pro Sekunde|Average|Die Rate, mit der der Cache Daten synchron in ein bestimmtes Speicherziel schreibt. Dabei handelt es sich um Schreibvorgänge, durch die Clients blockiert werden.|StorageTarget|
|StorageTargetTotalWriteThroughput|Schreibdurchsatz im Speicherziel insgesamt|Bytes pro Sekunde|Average|Die Gesamtrate, mit der der Cache Daten in ein bestimmtes Speicherziel schreibt.|StorageTarget|
|StorageTargetLatency|Speicherziellatenz|Millisekunden|Average|Die durchschnittliche Roundtriplatenz aller Dateivorgänge, die der Cache an ein bestimmtes Speicherziel sendet.|StorageTarget|
|StorageTargetMetadataReadIOPS|Lese-IOPS für Metadaten im Speicherziel|Anzahl pro Sekunde|Average|Die Rate der Dateivorgänge mit Ausnahme des Lesevorgangs, die den persistenten Zustand nicht ändern und die der Cache an ein bestimmtes Speicherziel sendet.|StorageTarget|
|StorageTargetMetadataWriteIOPS|Schreib-IOPS für Metadaten im Speicherziel|Anzahl pro Sekunde|Average|Die Rate der Dateivorgänge mit Ausnahme des Schreibvorgangs, die den persistenten Zustand ändern und die der Cache an ein bestimmtes Speicherziel sendet.|StorageTarget|
|StorageTargetReadIOPS|Lese-IOPS im Speicherziel|Anzahl pro Sekunde|Average|Die Rate der Dateilesevorgänge, die der Cache an ein bestimmtes Speicherziel sendet.|StorageTarget|
|StorageTargetReadAheadThroughput|Read-Ahead-Durchsatz im Speicherziel|Bytes pro Sekunde|Average|Die Rate, mit der der Cache Daten opportunistisch aus dem Speicherziel liest.|StorageTarget|
|StorageTargetFillThroughput|Füllungsdurchsatz im Speicherziel|Bytes pro Sekunde|Average|Die Rate, mit der der Cache Daten aus dem Speicherziel liest, um einen Cachefehler zu verarbeiten.|StorageTarget|
|StorageTargetTotalReadThroughput|Lesedurchsatz im Speicherziel insgesamt|Bytes pro Sekunde|Average|Die Gesamtrate, mit der der Cache Daten aus einem bestimmten Speicherziel liest.|StorageTarget|

## <a name="microsoftstoragesyncstoragesyncservices"></a>microsoft.storagesync/storageSyncServices

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|ServerSyncSessionResult|Ergebnis der Synchronisierungssitzung|Anzahl|Average|Metrik, die jedes Mal dann einen Wert von 1 protokolliert, wenn der Serverendpunkt eine Synchronisierungssitzung mit dem Cloudendpunkt erfolgreich abgeschlossen hat.|SyncGroupName,ServerEndpointName,SyncDirection|
|StorageSyncSyncSessionAppliedFilesCount|Synchronisierte Dateien|Anzahl|Gesamt|Anzahl synchronisierter Dateien|SyncGroupName,ServerEndpointName,SyncDirection|
|StorageSyncSyncSessionPerItemErrorsCount|Dateien ohne Synchronisierung|Anzahl|Gesamt|Anzahl von Dateien mit fehlerhafter Synchronisierung|SyncGroupName,ServerEndpointName,SyncDirection|
|StorageSyncBatchTransferredFileBytes|Bytes synchronisiert|Byte|Gesamt|Für Synchronisierungssitzungen übertragene Gesamtdateigröße|SyncGroupName,ServerEndpointName,SyncDirection|
|StorageSyncServerHeartbeat|Onlinestatus des Servers|Anzahl|Maximum|Metrik, die jedes Mal den Wert 1 protokolliert, wenn der registrierte Server erfolgreich einen Heartbeat mit dem Cloudendpunkt erfasst|ServerName|
|StorageSyncRecallIOTotalSizeBytes|Cloudtieringrückruf|Byte|Gesamt|Gesamtgröße der vom Server zurückgerufenen Daten|ServerName|
|StorageSyncRecalledTotalNetworkBytes|Cloudtiering-Rückrufgröße|Byte|Gesamt|Größe der zurückgerufenen Daten|SyncGroupName,ServerName|
|StorageSyncRecallThroughputBytesPerSecond|Cloudtiering-Rückrufdurchsatz|Bytes pro Sekunde|Average|Größe des Datenrückruf-Durchsatzes|SyncGroupName,ServerName|
|StorageSyncRecalledNetworkBytesByApplication|Cloudtiering-Rückrufgröße nach Anwendung|Byte|Gesamt|Größe der zurückgerufenen Daten nach Anwendung|SyncGroupName, ServerName, ApplicationName|

## <a name="microsoftstoragesyncstoragesyncservicessyncgroups"></a>microsoft.storagesync/storageSyncServices/syncGroups

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|SyncGroupSyncSessionAppliedFilesCount|Synchronisierte Dateien|Anzahl|Gesamt|Anzahl synchronisierter Dateien|SyncGroupName,ServerEndpointName,SyncDirection|
|SyncGroupSyncSessionPerItemErrorsCount|Dateien ohne Synchronisierung|Anzahl|Gesamt|Anzahl von Dateien mit fehlerhafter Synchronisierung|SyncGroupName,ServerEndpointName,SyncDirection|
|SyncGroupBatchTransferredFileBytes|Bytes synchronisiert|Byte|Gesamt|Für Synchronisierungssitzungen übertragene Gesamtdateigröße|SyncGroupName,ServerEndpointName,SyncDirection|

## <a name="microsoftstoragesyncstoragesyncservicessyncgroupsserverendpoints"></a>microsoft.storagesync/storageSyncServices/syncGroups/serverEndpoints

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|ServerEndpointSyncSessionAppliedFilesCount|Synchronisierte Dateien|Anzahl|Gesamt|Anzahl synchronisierter Dateien|ServerEndpointName,SyncDirection|
|ServerEndpointSyncSessionPerItemErrorsCount|Dateien ohne Synchronisierung|Anzahl|Gesamt|Anzahl von Dateien mit fehlerhafter Synchronisierung|ServerEndpointName,SyncDirection|
|ServerEndpointBatchTransferredFileBytes|Bytes synchronisiert|Byte|Gesamt|Für Synchronisierungssitzungen übertragene Gesamtdateigröße|ServerEndpointName,SyncDirection|

## <a name="microsoftstoragesyncstoragesyncservicesregisteredservers"></a>microsoft.storagesync/storageSyncServices/registeredServers

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|ServerHeartbeat|Onlinestatus des Servers|Anzahl|Maximum|Metrik, die jedes Mal den Wert 1 protokolliert, wenn der registrierte Server erfolgreich einen Heartbeat mit dem Cloudendpunkt erfasst|ServerResourceId,ServerName|
|ServerRecallIOTotalSizeBytes|Cloudtieringrückruf|Byte|Gesamt|Gesamtgröße der vom Server zurückgerufenen Daten|ServerResourceId,ServerName|



## <a name="microsoftstreamanalyticsstreamingjobs"></a>Microsoft.StreamAnalytics/streamingjobs

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|ResourceUtilization|Speichereinheitnutzung in %|Percent|Maximum|Speichereinheitnutzung in %|LogicalName, PartitionId|
|InputEvents|Eingabeereignisse|Anzahl|Gesamt|Eingabeereignisse|LogicalName, PartitionId|
|InputEventBytes|Eingabeereignisbytes|Byte|Gesamt|Eingabeereignisbytes|LogicalName, PartitionId|
|LateInputEvents|Ereignisse bei verspäteter Eingabe|Anzahl|Gesamt|Ereignisse bei verspäteter Eingabe|LogicalName, PartitionId|
|OutputEvents|Ausgabeereignisse|Anzahl|Gesamt|Ausgabeereignisse|LogicalName, PartitionId|
|ConversionErrors|Konvertierungsfehler|Anzahl|Gesamt|Konvertierungsfehler|LogicalName, PartitionId|
|Errors|Laufzeitfehler|Anzahl|Gesamt|Laufzeitfehler|LogicalName, PartitionId|
|DroppedOrAdjustedEvents|Ereignisse für falsche Reihenfolge|Anzahl|Gesamt|Ereignisse für falsche Reihenfolge|LogicalName, PartitionId|
|AMLCalloutRequests|Funktionsanforderungen|Anzahl|Gesamt|Funktionsanforderungen|LogicalName, PartitionId|
|AMLCalloutFailedRequests|Fehlerhafte Funktionsanforderungen|Anzahl|Gesamt|Fehlerhafte Funktionsanforderungen|LogicalName, PartitionId|
|AMLCalloutInputEvents|Funktionsereignisse|Anzahl|Gesamt|Funktionsereignisse|LogicalName, PartitionId|
|DeserializationError|Eingabefehler bei Deserialisierung|Anzahl|Gesamt|Eingabefehler bei Deserialisierung|LogicalName, PartitionId|
|EarlyInputEvents|Frühe Eingabeereignisse|Anzahl|Gesamt|Frühe Eingabeereignisse|LogicalName, PartitionId|
|OutputWatermarkDelaySeconds|Wasserzeichenverzögerung|Sekunden|Maximum|Wasserzeichenverzögerung|LogicalName, PartitionId|
|InputEventsSourcesBacklogged|Eingabeereignisse im Rückstand|Anzahl|Maximum|Eingabeereignisse im Rückstand|LogicalName, PartitionId|
|InputEventsSourcesPerSecond|Empfangene Eingabequellen|Anzahl|Gesamt|Empfangene Eingabequellen|LogicalName, PartitionId|

## <a name="microsofttimeseriesinsightsenvironments"></a>Microsoft.TimeSeriesInsights/environments

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|IngressReceivedMessages|Eingang empfangene Nachrichten|Anzahl|Gesamt|Die Anzahl der aus allen Event Hub- oder IoT Hub-Ereignisquellen gelesenen Nachrichten|Keine|
|IngressReceivedInvalidMessages|Eingang empfangene ungültige Nachrichten|Anzahl|Gesamt|Die Anzahl der aus allen Event Hub- oder IoT Hub-Ereignisquellen gelesenen ungültigen Nachrichten|Keine|
|IngressReceivedBytes|Eingang empfangene Bytes|Byte|Gesamt|Die Anzahl der aus sämtlichen Ereignisquellen gelesenen Bytes|Keine|
|IngressStoredBytes|Eingang gespeicherte Bytes|Byte|Gesamt|Die Gesamtgröße der erfolgreich verarbeiteten und für Abfragen verfügbaren Ereignisse|Keine|
|IngressStoredEvents|Gespeicherte Ingress-Ereignisse|Anzahl|Gesamt|Die Gesamtgröße der vereinfachten und für Abfragen verfügbaren Ereignisse|Keine|
|IngressReceivedMessagesTimeLag|Ingress-Zeitabstand empfangener Nachrichten|Sekunden|Maximum|Der Unterschied zwischen dem Zeitpunkt, zu dem die Nachricht in der Ereignisquelle in die Warteschlange eingereiht wird, und dem Zeitpunkt der Verarbeitung in Ingress|Keine|
|IngressReceivedMessagesCountLag|Ingress-Anzahlrückstand empfangener Nachrichten|Anzahl|Average|Unterschied zwischen der Sequenznummer der Nachricht, die in der Ereignisquelle zuletzt in die Warteschlange eingereiht wurde, und der Sequenznummer der am Eingang verarbeiteten Nachricht|Keine|
|WarmStorageMaxProperties|Maximale Eigenschaften von Warm Storage|Anzahl|Maximum|Maximale Anzahl von Eigenschaften, die von der Umgebung für S1/S2-SKU zugelassen werden, und maximale Anzahl von Eigenschaften, die von Warm Store für PAYG-SKU zugelassen werden|Keine|
|WarmStorageUsedProperties|Verwendete Eigenschaften von Warm Storage |Anzahl|Maximum|Anzahl von Eigenschaften, die von der Umgebung für S1/S2-SKU verwendet werden, und Anzahl von Eigenschaften, die von Warm Store für PAYG-SKU verwendet werden|Keine|

## <a name="microsofttimeseriesinsightsenvironmentseventsources"></a>Microsoft.TimeSeriesInsights/environments/eventsources

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|IngressReceivedMessages|Eingang empfangene Nachrichten|Anzahl|Gesamt|Die Anzahl der von der Ereignisquelle gelesenen Nachrichten|Keine|
|IngressReceivedInvalidMessages|Eingang empfangene ungültige Nachrichten|Anzahl|Gesamt|Die Anzahl der von der Ereignisquelle gelesenen ungültigen Nachrichten|Keine|
|IngressReceivedBytes|Eingang empfangene Bytes|Byte|Gesamt|Die Anzahl der von der Ereignisquelle gelesenen Bytes|Keine|
|IngressStoredBytes|Eingang gespeicherte Bytes|Byte|Gesamt|Die Gesamtgröße der erfolgreich verarbeiteten und für Abfragen verfügbaren Ereignisse|Keine|
|IngressStoredEvents|Gespeicherte Ingress-Ereignisse|Anzahl|Gesamt|Die Gesamtgröße der vereinfachten und für Abfragen verfügbaren Ereignisse|Keine|
|IngressReceivedMessagesTimeLag|Ingress-Zeitabstand empfangener Nachrichten|Sekunden|Maximum|Der Unterschied zwischen dem Zeitpunkt, zu dem die Nachricht in der Ereignisquelle in die Warteschlange eingereiht wird, und dem Zeitpunkt der Verarbeitung in Ingress|Keine|
|IngressReceivedMessagesCountLag|Ingress-Anzahlrückstand empfangener Nachrichten|Anzahl|Average|Unterschied zwischen der Sequenznummer der Nachricht, die in der Ereignisquelle zuletzt in die Warteschlange eingereiht wurde, und der Sequenznummer der am Eingang verarbeiteten Nachricht|Keine|
|WarmStorageMaxProperties|Maximale Eigenschaften von Warm Storage|Anzahl|Maximum|Maximale Anzahl von Eigenschaften, die von der Umgebung für S1/S2-SKU zugelassen werden, und maximale Anzahl von Eigenschaften, die von Warm Store für PAYG-SKU zugelassen werden|Keine|
|WarmStorageUsedProperties|Verwendete Eigenschaften von Warm Storage |Anzahl|Maximum|Anzahl von Eigenschaften, die von der Umgebung für S1/S2-SKU verwendet werden, und Anzahl von Eigenschaften, die von Warm Store für PAYG-SKU verwendet werden|Keine|

## <a name="microsoftvmwarecloudsimplevirtualmachines"></a>Microsoft.VMwareCloudSimple/virtualMachines

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|DiskReadBytesPerSecond|Disk Read Bytes/Sec|Bytes pro Sekunde|Average|Durchschnittlicher Datenträgerdurchsatz aufgrund von Lesevorgängen über den Stichprobenzeitraum.|Keine|
|DiskWriteBytesPerSecond|Disk Write Bytes/Sec|Bytes pro Sekunde|Average|Durchschnittlicher Datenträgerdurchsatz aufgrund von Schreibvorgängen über den Stichprobenzeitraum.|Keine|
|Datenträgerlesevorgänge in Bytes|Datenträgerlesevorgänge in Bytes|Byte|Gesamt|Gesamter Datenträgerdurchsatz aufgrund von Lesevorgängen über den Stichprobenzeitraum.|Keine|
|Datenträgerschreibvorgänge in Bytes|Datenträgerschreibvorgänge in Bytes|Byte|Gesamt|Gesamter Datenträgerdurchsatz aufgrund von Schreibvorgängen über den Stichprobenzeitraum.|Keine|
|DiskReadOperations|Datenträgerlesevorgänge|Anzahl|Gesamt|Die Anzahl von E/A-Lesevorgängen im vorherigen Stichprobenzeitraum. Beachten Sie, dass diese Vorgänge unterschiedlich groß sein können.|Keine|
|DiskWriteOperations|Datenträgerschreibvorgänge|Anzahl|Gesamt|Die Anzahl von E/A-Schreibvorgängen im vorherigen Stichprobenzeitraum. Beachten Sie, dass diese Vorgänge unterschiedlich groß sein können.|Keine|
|Datenträgerlesevorgänge/Sek.|Datenträgerlesevorgänge/Sek.|Anzahl pro Sekunde|Average|Die durchschnittliche Anzahl von E/A-Lesevorgängen im vorherigen Stichprobenzeitraum. Beachten Sie, dass diese Vorgänge unterschiedlich groß sein können.|Keine|
|Datenträgerschreibvorgänge/Sek.|Datenträgerschreibvorgänge/Sek.|Anzahl pro Sekunde|Average|Die durchschnittliche Anzahl von E/A-Schreibvorgängen im vorherigen Stichprobenzeitraum. Beachten Sie, dass diese Vorgänge unterschiedlich groß sein können.|Keine|
|DiskReadLatency|Wartezeit beim Datenträgerlesevorgang|Millisekunden|Average|Gesamtwartezeit beim Lesevorgang. Die Summe der Wartezeiten bei Lesevorgängen auf Gerät und Kernel.|Keine|
|DiskWriteLatency|Wartezeit beim Datenträgerschreibvorgang|Millisekunden|Average|Gesamtwartezeit beim Schreibvorgang. Die Summe der Wartezeiten bei Schreibvorgängen auf Gerät und Kernel.|Keine|
|NetworkInBytesPerSecond|Eingehender Netzwerkverkehr in Bytes/Sek.|Bytes pro Sekunde|Average|Durchschnittliche Netzwerkdurchsatz für empfangenen Datenverkehr.|Keine|
|NetworkOutBytesPerSecond|Ausgehender Netzwerkverkehr in Bytes/Sek.|Bytes pro Sekunde|Average|Durchschnittliche Netzwerkdurchsatz für gesendeten Datenverkehr.|Keine|
|Netzwerk eingehend|Netzwerk eingehend|Byte|Gesamt|Gesamter Netzwerkdurchsatz für empfangenen Datenverkehr.|Keine|
|Netzwerk ausgehend|Netzwerk ausgehend|Byte|Gesamt|Gesamter Netzwerkdurchsatz für gesendeten Datenverkehr.|Keine|
|MemoryUsed|Verwendeter Arbeitsspeicher|Byte|Average|Die Menge an Computerarbeitsspeicher, die vom virtuellen Computer verwendet wird.|Keine|
|MemoryGranted|Zugewiesener Arbeitsspeicher|Byte|Average|Die Menge an Arbeitsspeicher, die dem virtuellen Computer vom Host zugewiesen wurde. Arbeitsspeicher wird dem Host erst bei der ersten Verwendung zugewiesen, und zugewiesener Speicher kann ausgelagert oder ausgeweitet werden, wenn der VMkernel den Speicher benötigt.|Keine|
|MemoryActive|Aktiver Arbeitsspeicher|Byte|Average|Die Menge an Arbeitsspeicher, die vom virtuellen Computer im letzten kleinen Zeitfenster verwendet wurde. Dies ist die „wirkliche“ Anzahl des derzeit vom virtuellen Computer benötigten Arbeitsspeichers. Zusätzlicher, ungenutzter Arbeitsspeicher kann ausgelagert oder ausgeweitet werden, ohne dass dies Auswirkungen auf die Leistung für den Gast hat.|Keine|
|CPU in Prozent|CPU in Prozent|Percent|Average|Die CPU-Auslastung. Dieser Wert wird mit 100 % gemeldet und stellt alle Prozessorkerne im System dar. Beispielsweise verwendet ein virtueller 2-Wege-Computer, der 50 % eines Systems mit vier Kernen nutzt, zwei dieser Kerne vollständig.|Keine|
|PercentageCpuReady|Prozentsatz der CPU-Bereitschaft|Millisekunden|Gesamt|Die Bereitschaftszeit ist die Zeit, über die im letzten Updateintervall auf die Verfügbarkeit von CPUs gewartet wurde.|Keine|










## <a name="microsoftwebserverfarms"></a>Microsoft.Web/serverfarms

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|CpuPercentage|CPU-Prozentsatz|Percent|Average|CPU-Prozentsatz|Instanz|
|MemoryPercentage|Arbeitsspeicherprozentsatz|Percent|Average|Arbeitsspeicherprozentsatz|Instanz|
|DiskQueueLength|Warteschlangenlänge des Datenträgers|Anzahl|Average|Warteschlangenlänge des Datenträgers|Instanz|
|HttpQueueLength|Länge der HTTP-Warteschlange|Anzahl|Average|Länge der HTTP-Warteschlange|Instanz|
|BytesReceived|Eingehende Daten|Byte|Gesamt|Eingehende Daten|Instanz|
|BytesSent|Datenausgabe|Byte|Gesamt|Datenausgabe|Instanz|
|TcpSynSent|TCP-Synchronisierung gesendet|Anzahl|Average|TCP-Synchronisierung gesendet|Instanz|
|TcpSynReceived|TCP-Synchronisierung empfangen|Anzahl|Average|TCP-Synchronisierung empfangen|Instanz|
|TcpEstablished|TCP-Verbindung hergestellt|Anzahl|Average|TCP-Verbindung hergestellt|Instanz|
|TcpFinWait1|Warten 1 auf Beendigung der TCP-Verbindung|Anzahl|Average|Warten 1 auf Beendigung der TCP-Verbindung|Instanz|
|TcpFinWait2|Warten 2 auf Beendigung der TCP-Verbindung|Anzahl|Average|Warten 2 auf Beendigung der TCP-Verbindung|Instanz|
|TcpClosing|TCP-Verbindung wird geschlossen|Anzahl|Average|TCP-Verbindung wird geschlossen|Instanz|
|TcpCloseWait|Warten auf Schließen der TCP-Verbindung|Anzahl|Average|Warten auf Schließen der TCP-Verbindung|Instanz|
|TcpLastAck|Letzte TCP-Bestätigung|Anzahl|Average|Letzte TCP-Bestätigung|Instanz|
|TcpTimeWait|TCP-Wartezeit|Anzahl|Average|TCP-Wartezeit|Instanz|

## <a name="microsoftwebsites-excluding-functions"></a>Microsoft.Web/sites (Funktionen ausgenommen)

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|CpuTime|CPU-Zeit|Sekunden|Gesamt|CPU-Zeit|Instanz|
|Requests|Requests|Anzahl|Gesamt|Requests|Instanz|
|BytesReceived|Eingehende Daten|Byte|Gesamt|Eingehende Daten|Instanz|
|BytesSent|Datenausgabe|Byte|Gesamt|Datenausgabe|Instanz|
|Http101|HTTP 101|Anzahl|Gesamt|HTTP 101|Instanz|
|Http2xx|HTTP 2xx|Anzahl|Gesamt|HTTP 2xx|Instanz|
|Http3xx|HTTP 3xx|Anzahl|Gesamt|HTTP 3xx|Instanz|
|Http401|HTTP 401|Anzahl|Gesamt|HTTP 401|Instanz|
|Http403|HTTP 403|Anzahl|Gesamt|HTTP 403|Instanz|
|Http404|HTTP 404|Anzahl|Gesamt|HTTP 404|Instanz|
|Http406|HTTP 406|Anzahl|Gesamt|HTTP 406|Instanz|
|Http4xx|HTTP 4xx|Anzahl|Gesamt|HTTP 4xx|Instanz|
|Http5xx|HTTP-Serverfehler|Anzahl|Gesamt|HTTP-Serverfehler|Instanz|
|MemoryWorkingSet|Arbeitssatz für Arbeitsspeicher|Byte|Average|Arbeitssatz für Arbeitsspeicher|Instanz|
|AverageMemoryWorkingSet|Durchschnittlicher Arbeitssatz für Arbeitsspeicher|Byte|Average|Durchschnittlicher Arbeitssatz für Arbeitsspeicher|Instanz|
|AverageResponseTime|Durchschnittliche Antwortzeit|Sekunden|Average|Durchschnittliche Antwortzeit|Instanz|
|AppConnections|Verbindungen|Anzahl|Average|Verbindungen|Instanz|
|Ziehpunkte|Anzahl von Handles|Anzahl|Average|Anzahl von Handles|Instanz|
|Threads|Threadanzahl|Anzahl|Average|Threadanzahl|Instanz|
|PrivateBytes|Private Bytes|Byte|Average|Private Bytes|Instanz|
|IoReadBytesPerSecond|E/A: Gelesene Bytes pro Sekunde|Bytes pro Sekunde|Gesamt|E/A: Gelesene Bytes pro Sekunde|Instanz|
|IoWriteBytesPerSecond|E/A: Geschriebene Bytes pro Sekunde|Bytes pro Sekunde|Gesamt|E/A: Geschriebene Bytes pro Sekunde|Instanz|
|IoOtherBytesPerSecond|E/A: Andere Bytes pro Sekunde|Bytes pro Sekunde|Gesamt|E/A: Andere Bytes pro Sekunde|Instanz|
|IoReadOperationsPerSecond|E/A: Lesevorgänge pro Sekunde|Bytes pro Sekunde|Gesamt|E/A: Lesevorgänge pro Sekunde|Instanz|
|IoWriteOperationsPerSecond|E/A: Schreibvorgänge pro Sekunde|Bytes pro Sekunde|Gesamt|E/A: Schreibvorgänge pro Sekunde|Instanz|
|IoOtherOperationsPerSecond|E/A: Andere Vorgänge pro Sekunde|Bytes pro Sekunde|Gesamt|E/A: Andere Vorgänge pro Sekunde|Instanz|
|RequestsInApplicationQueue|Anforderungen in der Anwendungswarteschlange|Anzahl|Average|Anforderungen in der Anwendungswarteschlange|Instanz|
|CurrentAssemblies|Aktuelle Assemblys|Anzahl|Average|Aktuelle Assemblys|Instanz|
|TotalAppDomains|App-Domänen insgesamt|Anzahl|Average|App-Domänen insgesamt|Instanz|
|TotalAppDomainsUnloaded|Entladene App-Domänen insgesamt|Anzahl|Average|Entladene App-Domänen insgesamt|Instanz|
|Gen0Collections|Garbage Collections der Generation 0|Anzahl|Gesamt|Garbage Collections der Generation 0|Instanz|
|Gen1Collections|Garbage Collections der Generation 1|Anzahl|Gesamt|Garbage Collections der Generation 1|Instanz|
|Gen2Collections|Garbage Collections der Generation 2|Anzahl|Gesamt|Garbage Collections der Generation 2|Instanz|
|HealthCheckStatus|Status der Integritätsüberprüfung|Anzahl|Average|Status der Integritätsüberprüfung|Instanz|
|FileSystemUsage|Dateisystemnutzung|Byte|Average|Dateisystemnutzung|Keine|

## <a name="microsoftwebsites-functions"></a>Microsoft.Web/sites (Funktionen)

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|BytesReceived|Eingehende Daten|Byte|Gesamt|Eingehende Daten|Instanz|
|BytesSent|Datenausgabe|Byte|Gesamt|Datenausgabe|Instanz|
|Http5xx|HTTP-Serverfehler|Anzahl|Gesamt|HTTP-Serverfehler|Instanz|
|MemoryWorkingSet|Arbeitssatz für Arbeitsspeicher|Byte|Average|Arbeitssatz für Arbeitsspeicher|Instanz|
|AverageMemoryWorkingSet|Durchschnittlicher Arbeitssatz für Arbeitsspeicher|Byte|Average|Durchschnittlicher Arbeitssatz für Arbeitsspeicher|Instanz|
|FunctionExecutionUnits|Ausführungseinheiten für Funktion|MB/Millisekunden|Gesamt|[Ausführungseinheiten für Funktion](https://github.com/Azure/Azure-Functions/wiki/Consumption-Plan-Cost-Billing-FAQ#how-can-i-view-graphs-of-execution-count-and-gb-seconds)|Instanz|
|FunctionExecutionCount|Ausführungsanzahl für Funktion|Anzahl|Gesamt|Ausführungsanzahl für Funktion|Instanz|
|PrivateBytes|Private Bytes|Byte|Average|Private Bytes|Instanz|
|IoReadBytesPerSecond|E/A: Gelesene Bytes pro Sekunde|Bytes pro Sekunde|Gesamt|E/A: Gelesene Bytes pro Sekunde|Instanz|
|IoWriteBytesPerSecond|E/A: Geschriebene Bytes pro Sekunde|Bytes pro Sekunde|Gesamt|E/A: Geschriebene Bytes pro Sekunde|Instanz|
|IoOtherBytesPerSecond|E/A: Andere Bytes pro Sekunde|Bytes pro Sekunde|Gesamt|E/A: Andere Bytes pro Sekunde|Instanz|
|IoReadOperationsPerSecond|E/A: Lesevorgänge pro Sekunde|Bytes pro Sekunde|Gesamt|E/A: Lesevorgänge pro Sekunde|Instanz|
|IoWriteOperationsPerSecond|E/A: Schreibvorgänge pro Sekunde|Bytes pro Sekunde|Gesamt|E/A: Schreibvorgänge pro Sekunde|Instanz|
|IoOtherOperationsPerSecond|E/A: Andere Vorgänge pro Sekunde|Bytes pro Sekunde|Gesamt|E/A: Andere Vorgänge pro Sekunde|Instanz|
|RequestsInApplicationQueue|Anforderungen in der Anwendungswarteschlange|Anzahl|Average|Anforderungen in der Anwendungswarteschlange|Instanz|
|CurrentAssemblies|Aktuelle Assemblys|Anzahl|Average|Aktuelle Assemblys|Instanz|
|TotalAppDomains|App-Domänen insgesamt|Anzahl|Average|App-Domänen insgesamt|Instanz|
|TotalAppDomainsUnloaded|Entladene App-Domänen insgesamt|Anzahl|Average|Entladene App-Domänen insgesamt|Instanz|
|Gen0Collections|Garbage Collections der Generation 0|Anzahl|Gesamt|Garbage Collections der Generation 0|Instanz|
|Gen1Collections|Garbage Collections der Generation 1|Anzahl|Gesamt|Garbage Collections der Generation 1|Instanz|
|Gen2Collections|Garbage Collections der Generation 2|Anzahl|Gesamt|Garbage Collections der Generation 2|Instanz|
|HealthCheckStatus|Status der Integritätsüberprüfung|Anzahl|Average|Status der Integritätsüberprüfung|Instanz|
|FileSystemUsage|Dateisystemnutzung|Byte|Average|Dateisystemnutzung|Keine|


## <a name="microsoftwebsitesslots"></a>Microsoft.Web/sites/slots

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|CpuTime|CPU-Zeit|Sekunden|Gesamt|CPU-Zeit|Instanz|
|Requests|Requests|Anzahl|Gesamt|Requests|Instanz|
|BytesReceived|Eingehende Daten|Byte|Gesamt|Eingehende Daten|Instanz|
|BytesSent|Datenausgabe|Byte|Gesamt|Datenausgabe|Instanz|
|Http101|HTTP 101|Anzahl|Gesamt|HTTP 101|Instanz|
|Http2xx|HTTP 2xx|Anzahl|Gesamt|HTTP 2xx|Instanz|
|Http3xx|HTTP 3xx|Anzahl|Gesamt|HTTP 3xx|Instanz|
|Http401|HTTP 401|Anzahl|Gesamt|HTTP 401|Instanz|
|Http403|HTTP 403|Anzahl|Gesamt|HTTP 403|Instanz|
|Http404|HTTP 404|Anzahl|Gesamt|HTTP 404|Instanz|
|Http406|HTTP 406|Anzahl|Gesamt|HTTP 406|Instanz|
|Http4xx|HTTP 4xx|Anzahl|Gesamt|HTTP 4xx|Instanz|
|Http5xx|HTTP-Serverfehler|Anzahl|Gesamt|HTTP-Serverfehler|Instanz|
|MemoryWorkingSet|Arbeitssatz für Arbeitsspeicher|Byte|Average|Arbeitssatz für Arbeitsspeicher|Instanz|
|AverageMemoryWorkingSet|Durchschnittlicher Arbeitssatz für Arbeitsspeicher|Byte|Average|Durchschnittlicher Arbeitssatz für Arbeitsspeicher|Instanz|
|AverageResponseTime|Durchschnittliche Antwortzeit|Sekunden|Average|Durchschnittliche Antwortzeit|Instanz|
|HttpResponseTime|Antwortzeit|Sekunden|Average|Antwortzeit|Instanz|
|FunctionExecutionUnits|Ausführungseinheiten für Funktion|Anzahl|Gesamt|Ausführungseinheiten für Funktion|Instanz|
|FunctionExecutionCount|Ausführungsanzahl für Funktion|Anzahl|Gesamt|Ausführungsanzahl für Funktion|Instanz|
|AppConnections|Verbindungen|Anzahl|Average|Verbindungen|Instanz|
|Ziehpunkte|Anzahl von Handles|Anzahl|Average|Anzahl von Handles|Instanz|
|Threads|Threadanzahl|Anzahl|Average|Threadanzahl|Instanz|
|PrivateBytes|Private Bytes|Byte|Average|Private Bytes|Instanz|
|IoReadBytesPerSecond|E/A: Gelesene Bytes pro Sekunde|Bytes pro Sekunde|Gesamt|E/A: Gelesene Bytes pro Sekunde|Instanz|
|IoWriteBytesPerSecond|E/A: Geschriebene Bytes pro Sekunde|Bytes pro Sekunde|Gesamt|E/A: Geschriebene Bytes pro Sekunde|Instanz|
|IoOtherBytesPerSecond|E/A: Andere Bytes pro Sekunde|Bytes pro Sekunde|Gesamt|E/A: Andere Bytes pro Sekunde|Instanz|
|IoReadOperationsPerSecond|E/A: Lesevorgänge pro Sekunde|Bytes pro Sekunde|Gesamt|E/A: Lesevorgänge pro Sekunde|Instanz|
|IoWriteOperationsPerSecond|E/A: Schreibvorgänge pro Sekunde|Bytes pro Sekunde|Gesamt|E/A: Schreibvorgänge pro Sekunde|Instanz|
|IoOtherOperationsPerSecond|E/A: Andere Vorgänge pro Sekunde|Bytes pro Sekunde|Gesamt|E/A: Andere Vorgänge pro Sekunde|Instanz|
|RequestsInApplicationQueue|Anforderungen in der Anwendungswarteschlange|Anzahl|Average|Anforderungen in der Anwendungswarteschlange|Instanz|
|CurrentAssemblies|Aktuelle Assemblys|Anzahl|Average|Aktuelle Assemblys|Instanz|
|TotalAppDomains|App-Domänen insgesamt|Anzahl|Average|App-Domänen insgesamt|Instanz|
|TotalAppDomainsUnloaded|Entladene App-Domänen insgesamt|Anzahl|Average|Entladene App-Domänen insgesamt|Instanz|
|Gen0Collections|Garbage Collections der Generation 0|Anzahl|Gesamt|Garbage Collections der Generation 0|Instanz|
|Gen1Collections|Garbage Collections der Generation 1|Anzahl|Gesamt|Garbage Collections der Generation 1|Instanz|
|Gen2Collections|Garbage Collections der Generation 2|Anzahl|Gesamt|Garbage Collections der Generation 2|Instanz|
|HealthCheckStatus|Status der Integritätsüberprüfung|Anzahl|Average|Status der Integritätsüberprüfung|Instanz|
|FileSystemUsage|Dateisystemnutzung|Byte|Average|Dateisystemnutzung|Keine|

## <a name="microsoftwebhostingenvironmentsmultirolepools"></a>Microsoft.Web/hostingEnvironments/multiRolePools

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|Requests|Requests|Anzahl|Gesamt|Requests|Instanz|
|BytesReceived|Eingehende Daten|Byte|Gesamt|Eingehende Daten|Instanz|
|BytesSent|Datenausgabe|Byte|Gesamt|Datenausgabe|Instanz|
|Http101|HTTP 101|Anzahl|Gesamt|HTTP 101|Instanz|
|Http2xx|HTTP 2xx|Anzahl|Gesamt|HTTP 2xx|Instanz|
|Http3xx|HTTP 3xx|Anzahl|Gesamt|HTTP 3xx|Instanz|
|Http401|HTTP 401|Anzahl|Gesamt|HTTP 401|Instanz|
|Http403|HTTP 403|Anzahl|Gesamt|HTTP 403|Instanz|
|Http404|HTTP 404|Anzahl|Gesamt|HTTP 404|Instanz|
|Http406|HTTP 406|Anzahl|Gesamt|HTTP 406|Instanz|
|Http4xx|HTTP 4xx|Anzahl|Gesamt|HTTP 4xx|Instanz|
|Http5xx|HTTP-Serverfehler|Anzahl|Gesamt|HTTP-Serverfehler|Instanz|
|AverageResponseTime|Durchschnittliche Antwortzeit|Sekunden|Average|Durchschnittliche Antwortzeit|Instanz|
|CpuPercentage|CPU-Prozentsatz|Percent|Average|CPU-Prozentsatz|Instanz|
|MemoryPercentage|Arbeitsspeicherprozentsatz|Percent|Average|Arbeitsspeicherprozentsatz|Instanz|
|DiskQueueLength|Warteschlangenlänge des Datenträgers|Anzahl|Average|Warteschlangenlänge des Datenträgers|Instanz|
|HttpQueueLength|Länge der HTTP-Warteschlange|Anzahl|Average|Länge der HTTP-Warteschlange|Instanz|
|ActiveRequests|Aktive Anforderungen|Anzahl|Gesamt|Aktive Anforderungen|Instanz|
|TotalFrontEnds|Gesamtanzahl an Front-Ends|Anzahl|Average|Gesamtanzahl an Front-Ends|Keine|
|SmallAppServicePlanInstances|Kleine Worker im App Service-Plan|Anzahl|Average|Kleine Worker im App Service-Plan|Keine|
|MediumAppServicePlanInstances|Mittlere Worker im App Service-Plan|Anzahl|Average|Mittlere Worker im App Service-Plan|Keine|
|LargeAppServicePlanInstances|Große Worker im App Service-Plan|Anzahl|Average|Große Worker im App Service-Plan|Keine|

## <a name="microsoftwebhostingenvironmentsworkerpools"></a>Microsoft.Web/hostingEnvironments/workerPools

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|WorkersTotal|Gesamtanzahl an Workern|Anzahl|Average|Gesamtanzahl an Workern|Keine|
|WorkersAvailable|Verfügbare Worker|Anzahl|Average|Verfügbare Worker|Keine|
|WorkersUsed|Verwendete Worker|Anzahl|Average|Verwendete Worker|Keine|
|CpuPercentage|CPU-Prozentsatz|Percent|Average|CPU-Prozentsatz|Instanz|
|MemoryPercentage|Arbeitsspeicherprozentsatz|Percent|Average|Arbeitsspeicherprozentsatz|Instanz|
## <a name="next-steps"></a>Nächste Schritte
* [Informationen zu Metriken in Azure Monitor](data-platform.md)
* [Erstellen von Warnungen zu Metriken](alerts-overview.md)
* [Exportieren von Metriken in Storage, Event Hub oder Log Analytics](platform-logs-overview.md)
