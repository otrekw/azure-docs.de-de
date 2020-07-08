---
title: Überwachen von Servermetriken in Azure Analysis Services | Microsoft-Dokumentation
description: Erfahren Sie, wie Analysis Services Sie mit dem Azure Metrik-Explorer, einem kostenlosen Tool im Portal, beim Überwachen von Leistung und Integrität Ihres Servers unterstützt.
author: minewiskan
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 03/04/2020
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: aaa3a6d128fe7dd466f6f60ab515f05fa38ba63b
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "84690375"
---
# <a name="monitor-server-metrics"></a>Überwachen von Servermetriken

Analysis Services bietet Metriken zum Überwachen von Leistung und Integrität Ihres Servers. Diese Metriken werden im Azure Metrik-Explorer, einem kostenlosen Tool im Portal, angezeigt. Überwachen Sie z.B. Arbeitsspeicher- und CPU-Nutzung, Anzahl der Clientverbindungen und Ressourcenverbrauch für Abfragen. Analysis Services verwendet das gleiche Überwachungsframework wie die meisten anderen Azure-Dienste. Weitere Informationen finden Sie unter [Erste Schritte mit dem Azure-Metrik-Explorer](../azure-monitor/platform/metrics-getting-started.md).

Um eine ausführlichere Diagnose auszuführen, Leistung nachzuverfolgen und mehrere Dienstressourcen in einer Ressourcengruppe oder einem Abonnement übergreifende Trends zu identifizieren, verwenden Sie [Azure Monitor](../azure-monitor/overview.md). Die Leistungen des Diensts Azure Monitor werden möglicherweise in Rechnung gestellt.


## <a name="to-monitor-metrics-for-an-analysis-services-server"></a>So überwachen Sie Metriken für einen Analysis Services-Server

1. Wählen Sie im Azure-Portal die Option **Metriken** aus.

    ![Überwachen im Azure-Portal](./media/analysis-services-monitor/aas-monitor-portal.png)

2. Wählen Sie in **Metriken** die Metriken aus, die Sie in Ihr Diagramm aufnehmen möchten. 

    ![Überwachungsdiagramm](./media/analysis-services-monitor/aas-monitor-chart.png)

<a id="#server-metrics"></a>

## <a name="server-metrics"></a>Servermetriken

Bestimmen Sie mit dieser Tabelle, welche Metriken für Ihr Überwachungsszenario am besten geeignet sind. Nur Metriken derselben Einheit können im selben Diagramm angezeigt werden.

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|BESCHREIBUNG|
|---|---|---|---|---|
|CommandPoolJobQueueLength|Warteschlangenlänge für Aufträge im Befehlspool|Anzahl|Average|Gibt die Anzahl von Aufträgen in der Warteschlange des Befehlsthreadpools an.|
|CurrentConnections|Verbindung: Aktuelle Verbindungen|Anzahl|Average|Aktuelle Anzahl hergestellter Clientverbindungen.|
|CurrentUserSessions|Aktuelle Benutzersitzungen|Anzahl|Average|Aktuelle Anzahl von eingerichteten Benutzersitzungen|
|mashup_engine_memory_metric|M-Engine – Arbeitsspeicher|Byte|Average|Arbeitsspeichernutzung durch Mashup-Engine-Prozesse|
|mashup_engine_qpu_metric|M-Engine – QPU|Anzahl|Average|QPU-Nutzung durch Mashup-Engine-Prozesse|
|memory_metric|Arbeitsspeicher|Byte|Average|Arbeitsspeicher. Bereich: 0–25 GB für S1, 0–50 GB für S2 und 0–100 GB für S4|
|memory_thrashing_metric|Arbeitsspeicherüberlastung|Percent|Average|Durchschnittliche Arbeitsspeicherüberlastung.|
|CleanerCurrentPrice|Arbeitsspeicher: Bereinigung – aktueller Preis|Anzahl|Average|Aktueller Preis des Arbeitsspeichers, $/Byte/Zeit, normalisiert auf 1000.|
|CleanerMemoryNonshrinkable|Arbeitsspeicher: Bereinigung – nicht verkleinerbarer Arbeitsspeicher|Byte|Average|Die Menge des Arbeitsspeichers in Byte, die nicht durch den Hintergrundbereinigungsprozess bereinigt wird.|
|CleanerMemoryShrinkable|Arbeitsspeicher: Bereinigung – verkleinerbarer Arbeitsspeicher|Byte|Average|Die Menge des Arbeitsspeichers in Byte, die durch den Hintergrundbereinigungsprozess bereinigt wird.|
|MemoryLimitHard|Arbeitsspeicher: Grenzwert für den festen Speicher|Byte|Average|Grenzwert für den festen Speicher gemäß Konfigurationsdatei.|
|MemoryLimitHigh|Arbeitsspeicher: Obere Arbeitsspeichergrenze|Byte|Average|Oberer Grenzwert für den Arbeitsspeicher gemäß Konfigurationsdatei.|
|MemoryLimitLow|Arbeitsspeicher: Untere Arbeitsspeichergrenze|Byte|Average|Unterer Grenzwert für den Arbeitsspeicher gemäß Konfigurationsdatei.|
|MemoryLimitVertiPaq|Arbeitsspeicher: VertiPaq-Arbeitsspeichergrenze|Byte|Average|In-Memory-Grenzwert gemäß Konfigurationsdatei.|
|MemoryUsage|Arbeitsspeicher: Speicherauslastung|Byte|Average|Speicherauslastung des Serverprozesses, wie bei der Berechnung des Arbeitsspeicherpreises für die Bereinigung verwendet. Entspricht dem Indikator „Process\PrivateBytes“ zuzüglich der Größe der im Speicher abgebildeten Daten. Von der In-Memory-Analyse-Engine (VertiPaq) abgebildeter oder belegter Arbeitsspeicher, der über die Arbeitsspeichergrenze der Engine hinausgeht, wird dabei ignoriert.|
|private_bytes_metric|Private Bytes |Byte|Average|Die Gesamtmenge an Speicher, die der Analysis Services-Engineprozess und die Mashupcontainerprozesse zugewiesen haben, ohne den mit anderen Prozessen gemeinsam genutzten Speicher.|
|virtual_bytes_metric|Virtuelle Bytes |Byte|Average|Die aktuelle Größe des virtuellen Adressraums, den der Analysis Services-Engineprozess und die Mashupcontainerprozesse verwenden.|
|mashup_engine_private_bytes_metric|Private Bytes für M-Engine |Byte|Average|Die Gesamtmenge an Speicher, die die Mashupcontainerprozesse zugewiesen haben, ohne den mit anderen Prozessen gemeinsam genutzten Speicher.|
|mashup_engine_virtual_bytes_metric|Virtuelle Bytes für M-Engine |Byte|Average|Die aktuelle Größe des virtuellen Adressraums, den die Mashupcontainerprozesse verwenden.|
|Kontingent|Arbeitsspeicher: Kontingent|Byte|Average|Aktuelles Arbeitsspeicherkontingent in Byte. Das Arbeitsspeicherkontingent wird auch als Speicherzuweisung oder Speicherreservierung bezeichnet.|
|QuotaBlocked|Arbeitsspeicher: Kontingent blockiert|Anzahl|Average|Aktuelle Anzahl von Kontingentanforderungen, die blockiert werden, bis andere Arbeitsspeicherkontingente freigegeben werden.|
|VertiPaqNonpaged|Arbeitsspeicher: Nicht ausgelagerte VertiPaq-Daten|Byte|Average|Bytes von Arbeitsspeicher, die im Arbeitssatz zur Verwendung durch die In-Memory-Engine gesperrt sind.|
|VertiPaqPaged|Arbeitsspeicher: Ausgelagerte VertiPaq-Daten|Byte|Average|Bytes von ausgelagertem Arbeitsspeicher, die für In-Memory-Daten verwendet werden.|
|ProcessingPoolJobQueueLength|Warteschlangenlänge für Verarbeitungspoolaufträge|Anzahl|Average|Anzahl von Nicht-E/A-Aufträgen in der Warteschlange des Verarbeitungsthreadpools|
|RowsConvertedPerSec|Verarbeitung: Konvertierte Zeilen pro Sekunde|Anzahl pro Sekunde|Average|Rate der Zeilen, die bei der Verarbeitung konvertiert werden.|
|RowsReadPerSec|Verarbeitung: Gelesene Zeilen pro Sekunde|Anzahl pro Sekunde|Average|Rate der aus allen relationalen Datenbanken gelesenen Zeilen.|
|RowsWrittenPerSec|Verarbeitung: Geschriebene Zeilen pro Sekunde|Anzahl pro Sekunde|Average|Rate der Zeilen, die bei der Verarbeitung geschrieben werden.|
|qpu_metric|QPU|Anzahl|Average|QPU. Bereich: 0–100 für S1, 0–200 für S2 und 0–400 für S4|
|QueryPoolBusyThreads|Ausgelastete Abfragepoolthreads|Anzahl|Average|Anzahl von ausgelasteten Threads im Abfragethreadpool|
|SuccessfullConnectionsPerSec|Erfolgreiche Verbindungen pro Sekunde|Anzahl pro Sekunde|Average|Rate der erfolgreichen Verbindungsabschlüsse|
|CommandPoolBusyThreads|Threads: Ausgelastete Threads im Befehlspool|Anzahl|Average|Anzahl ausgelasteter Threads im Befehlsthreadpool.|
|CommandPoolIdleThreads|Threads: Leerlaufthreads im Befehlspool|Anzahl|Average|Anzahl von Leerlaufthreads im Befehlsthreadpool.|
|LongParsingBusyThreads|Threads: Ausgelastete lange Analysethreads|Anzahl|Average|Anzahl ausgelasteter Threads im Pool für lange Analysethreads.|
|LongParsingIdleThreads|Threads: Lange Analysethreads im Leerlauf|Anzahl|Average|Anzahl von Leerlaufthreads im Pool für lange Analysethreads.|
|LongParsingJobQueueLength|Threads: Warteschlangenlänge für lange Analyseaufträge|Anzahl|Average|Anzahl von Aufträgen in der Warteschlange des Pools für lange Analysethreads.|
|ProcessingPoolIOJobQueueLength|Threads: Warteschlangenlänge für E/A-Aufträge im Verarbeitungspool|Anzahl|Average|Anzahl von E/A-Aufträgen in der Warteschlange des Verarbeitungsthreadpools.|
|ProcessingPoolBusyIOJobThreads|Threads: Ausgelastete Threads für E/A-Aufträge im Verarbeitungspool|Anzahl|Average|Anzahl von Threads, die E/A-Aufträge im Verarbeitungsthreadpool ausführen.|
|ProcessingPoolBusyNonIOThreads|Threads: Ausgelastete Nicht-E/A-Threads im Verarbeitungspool|Anzahl|Average|Anzahl von Threads, die Nicht-E/A-Aufträge im Verarbeitungsthreadpool ausführen.|
|ProcessingPoolIdleIOJobThreads|Threads: Leerlaufthreads für E/A-Aufträge im Verarbeitungspool|Anzahl|Average|Anzahl von Leerlaufthreads für E/A-Aufträge im Verarbeitungsthreadpool.|
|ProcessingPoolIdleNonIOThreads|Threads: Nicht-E/A-Leerlaufthreads im Verarbeitungspool|Anzahl|Average|Anzahl von Leerlaufthreads im Verarbeitungsthreadpool, die für Nicht-E/A-Aufträge vorgesehen sind.|
|QueryPoolIdleThreads|Threads: Abfragepoolthreads im Leerlauf|Anzahl|Average|Anzahl von Leerlaufthreads für E/A-Aufträge im Verarbeitungsthreadpool.|
|QueryPoolJobQueueLength|Threads: Auftragswarteschlangenlänge für Abfragepool|Anzahl|Average|Anzahl von Aufträgen in der Warteschlange des Abfragethreadpools.|
|ShortParsingBusyThreads|Threads: Ausgelastete kurze Analysethreads|Anzahl|Average|Anzahl ausgelasteter Threads im Pool für kurze Analysethreads.|
|ShortParsingIdleThreads|Threads: Kurze Analysethreads im Leerlauf|Anzahl|Average|Anzahl von Leerlaufthreads im Pool für kurze Analysethreads.|
|ShortParsingJobQueueLength|Threads: Warteschlangenlänge für kurze Analyseaufträge|Anzahl|Average|Anzahl von Aufträgen in der Warteschlange des Pools für kurze Analysethreads.|
|TotalConnectionFailures|Verbindungsfehler gesamt|Anzahl|Average|Gesamtanzahl von fehlerhaften Verbindungsversuchen|
|TotalConnectionRequests|Total Connection Requests (Verbindungsanforderungen gesamt)|Anzahl|Average|Gesamtanzahl von Verbindungsanforderungen. |

## <a name="next-steps"></a>Nächste Schritte
[Azure Monitor – Übersicht](../azure-monitor/overview.md)      
[Erste Schritte mit dem Azure-Metrik-Explorer](../azure-monitor/platform/metrics-getting-started.md)      
[Metriken](/rest/api/monitor/metrics)
