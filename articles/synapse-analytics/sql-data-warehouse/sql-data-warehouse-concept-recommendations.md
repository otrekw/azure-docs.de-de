---
title: Synapse SQL-Empfehlungen
description: Erfahren Sie mehr über Synapse SQL-Empfehlungen und wie sie generiert werden.
services: synapse-analytics
author: kevinvngo
manager: craigg-msft
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 04/30/2020
ms.author: kevin
ms.reviewer: igorstan
ms.custom: azure-synapse
ms.openlocfilehash: 546945d70554adbb28f19a3153faa67495e55f04
ms.sourcegitcommit: 50ef5c2798da04cf746181fbfa3253fca366feaa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/30/2020
ms.locfileid: "82607747"
---
# <a name="synapse-sql-recommendations"></a>Synapse SQL-Empfehlungen

In diesem Artikel werden die Synapse SQL-Empfehlungen beschrieben, die durch Azure Advisor bereitgestellt werden.  

SQL Analytics bietet Empfehlungen, um sicherzustellen, dass Ihr Data Warehouse-Workload für Leistung konsistent optimiert wird. Empfehlungen sind in den [Azure Advisor](../../advisor/advisor-performance-recommendations.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json) eng integriert, um Sie direkt im [Azure-Portal](https://aka.ms/Azureadvisor) mit bewährten Methoden vertraut zu machen. SQL Analytics sammelt Telemetrie- und Oberflächenempfehlungen für Ihre aktive Workload in einem täglichen Rhythmus. Die unterstützten Empfehlungsszenarien werden im Folgenden zusammen mit der Anwendung empfohlener Aktionen beschrieben.

Sie können noch heute [Ihre Empfehlungen überprüfen](https://aka.ms/Azureadvisor)! 

## <a name="data-skew"></a>Datenschiefe

Datenschiefe kann eine zusätzliche Datenverschiebung oder Ressourcenengpässe beim Ausführen Ihrer Workload verursachen. Die folgende Dokumentation beschreibt, wie Datenschiefe identifiziert und ihr Auftreten durch Auswahl eines Schlüssels für die optimale Verteilung verhindert wird.

- [Identifizieren und Entfernen von Datenschiefe](sql-data-warehouse-tables-distribute.md#how-to-tell-if-your-distribution-column-is-a-good-choice)

## <a name="no-or-outdated-statistics"></a>Keine oder veraltete Statistiken

Suboptimale Statistiken können die Abfrageleistung erheblich beeinträchtigen, weil sie dazu führen können, dass der SQL-Abfrageoptimierer suboptimale Abfragepläne generiert. Die folgende Dokumentation beschreibt die bewährten Methoden zum Erstellen und Aktualisieren von Statistiken:

- [Erstellen und Aktualisieren von Tabellenstatistiken](sql-data-warehouse-tables-statistics.md)

Führen Sie zum Anzeigen der Liste der Tabellen, auf die sich diese Empfehlungen auswirken, [dieses T-SQL-Skript](https://github.com/Microsoft/sql-data-warehouse-samples/blob/master/samples/sqlops/MonitoringScripts/ImpactedTables) aus. Der Advisor führt kontinuierlich das gleiche T-SQL-Skript aus, um diese Empfehlungen zu generieren.

## <a name="replicate-tables"></a>Replizieren von Tabellen

Zur Generierung von Empfehlungen für replizierte Tabellen ermittelt der Advisor Tabellenkandidaten auf der Grundlage folgender physischer Merkmale:

- Größe der replizierten Tabelle
- Anzahl von Spalten
- Tabellenverteilungstyp
- Anzahl von Partitionen

Der Advisor nutzt kontinuierlich workloadbasierte Heuristik wie etwa Tabellenzugriffshäufigkeit, durchschnittlich zurückgegebene Zeilen sowie Schwellenwerte für Data Warehouse-Größe und -Aktivität, um hochwertige Empfehlungen zu generieren.

Im anschließenden Abschnitt wird die workloadbasierte Heuristik beschrieben, die ggf. im Azure-Portal für die einzelnen Empfehlungen für replizierte Tabellen verfügbar ist:

- Scan avg (Scandurchschnitt): Der durchschnittliche Prozentsatz von Zeilen, die aus der Tabelle zurückgegeben wurden (für jeden Tabellenzugriff in den letzten sieben Tagen).
- Frequent read, no update (Häufig lesen, keine Aktualisierung): Gibt an, dass die Tabelle in den letzten sieben Tagen nicht aktualisiert wurde, aber Zugriffsaktivitäten festgestellt wurden.
- Read/update ratio (Verhältnis zwischen Lesen und Aktualisieren): Das Verhältnis zwischen Zugriffs- und Aktualisierungsvorgängen für die Tabelle in den letzten sieben Tagen.
- Aktivität: Ermittelt die Nutzung anhand der Zugriffsaktivität. Diese Aktivität vergleicht die Tabellenzugriffsaktivität mit der durchschnittlichen Tabellenzugriffsaktivität innerhalb des gesamten Data Warehouse während der letzten sieben Tage.

Derzeit zeigt der Advisor maximal vier Kandidaten für replizierte Tabellen auf einmal an – mit gruppierten Columnstore-Indizes und unter Priorisierung der höchsten Aktivität.

> [!IMPORTANT]
> Die Empfehlung für replizierte Tabellen ist nicht narrensicher und berücksichtigt keine Datenverschiebungsvorgänge. Wir arbeiten daran, dies als Heuristik hinzuzufügen, bis dahin empfiehlt es sich jedoch, die Workload nach Anwendung der Empfehlung zu überprüfen. Weitere Informationen zu replizierten Tabellen finden Sie in [dieser Dokumentation](design-guidance-for-replicated-tables.md#what-is-a-replicated-table).


## <a name="adaptive-gen2-cache-utilization"></a>Adaptive (Gen2) Cacheauslastung
Wenn Sie über ein großes Workingset verfügen, kann es zu einem niedrigen Prozentsatz der Cachetreffer und einer hohen Cacheauslastung kommen. Für dieses Szenario sollten Sie hochskalieren, um die Cachekapazität zu erhöhen und Ihre Workload erneut auszuführen. Weitere Informationen finden Sie in der folgenden [Dokumentation](https://docs.microsoft.com/azure/synapse-analytics/sql-data-warehouse/sql-data-warehouse-how-to-monitor-cache). 

## <a name="tempdb-contention"></a>TempDB-Konflikt

Die Abfrageleistung kann sich bei hohen tempdb-Konflikten verschlechtern.  Tempdb-Konflikte können über benutzerdefinierte temporäre Tabellen auftreten, oder wenn eine große Menge von Datenverschiebungen vorkommen. In diesem Szenario können Sie für höhere tempdb-Zuordnungen skalieren und [Ressourcenklassen sowie die Workloadverwaltung konfigurieren](https://docs.microsoft.com/azure/synapse-analytics/sql-data-warehouse/sql-data-warehouse-workload-management), um mehr Arbeitsspeicher für Ihre Abfragen bereitzustellen. 
