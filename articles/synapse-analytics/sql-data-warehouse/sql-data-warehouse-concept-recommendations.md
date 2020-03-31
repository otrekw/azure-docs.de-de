---
title: SQL Analytics-Empfehlungen
description: Hier erfahren Sie etwas über SQL Analytics-Empfehlungen und wie sie erzeugt werden.
services: synapse-analytics
author: kevinvngo
manager: craigg-msft
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 02/05/2020
ms.author: kevin
ms.reviewer: igorstan
ms.custom: azure-synapse
ms.openlocfilehash: 6d57fdb035e076c75363d23fbf36d39eeb72bb3f
ms.sourcegitcommit: 8a9c54c82ab8f922be54fb2fcfd880815f25de77
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "80350687"
---
# <a name="sql-analytics-recommendations"></a>SQL Analytics-Empfehlungen

In diesem Artikel werden die SQL Analytics-Empfehlungen beschrieben, die durch Azure Advisor bereitgestellt werden.  

SQL Analytics bietet Empfehlungen, um sicherzustellen, dass Ihr Data Warehouse-Workload für Leistung konsistent optimiert wird. Empfehlungen sind in den [Azure Advisor](https://docs.microsoft.com/azure/advisor/advisor-performance-recommendations) eng integriert, um Sie direkt im [Azure-Portal](https://aka.ms/Azureadvisor) mit bewährten Methoden vertraut zu machen. SQL Analytics sammelt Telemetrie- und Oberflächenempfehlungen für Ihre aktive Workload in einem täglichen Rhythmus. Die unterstützten Empfehlungsszenarien werden im Folgenden zusammen mit der Anwendung empfohlener Aktionen beschrieben.

Sie können noch heute [Ihre Empfehlungen überprüfen](https://aka.ms/Azureadvisor)! Dieses Feature ist zurzeit nur auf Gen2-Data Warehouses anwendbar. 

## <a name="data-skew"></a>Datenschiefe

Datenschiefe kann eine zusätzliche Datenverschiebung oder Ressourcenengpässe beim Ausführen Ihrer Workload verursachen. Die folgende Dokumentation beschreibt, wie Datenschiefe identifiziert und ihr Auftreten durch Auswahl eines Schlüssels für die optimale Verteilung verhindert wird.

- [Identifizieren und Entfernen von Datenschiefe](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-tables-distribute#how-to-tell-if-your-distribution-column-is-a-good-choice) 

## <a name="no-or-outdated-statistics"></a>Keine oder veraltete Statistiken

Suboptimale Statistiken können die Abfrageleistung erheblich beeinträchtigen, weil sie dazu führen können, dass der SQL-Abfrageoptimierer suboptimale Abfragepläne generiert. Die folgende Dokumentation beschreibt die bewährten Methoden zum Erstellen und Aktualisieren von Statistiken:

- [Erstellen und Aktualisieren von Tabellenstatistiken](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-tables-statistics)

Führen Sie zum Anzeigen der Liste der Tabellen, auf die sich diese Empfehlungen auswirken, [dieses T-SQL-Skript](https://github.com/Microsoft/sql-data-warehouse-samples/blob/master/samples/sqlops/MonitoringScripts/ImpactedTables) aus. Der Advisor führt kontinuierlich das gleiche T-SQL-Skript aus, um diese Empfehlungen zu generieren.

## <a name="replicate-tables"></a>Replizieren von Tabellen

Zur Generierung von Empfehlungen für replizierte Tabellen ermittelt der Advisor Tabellenkandidaten auf der Grundlage folgender physischer Merkmale:

- Größe der replizierten Tabelle
- Anzahl von Spalten
- Tabellenverteilungstyp
- Anzahl von Partitionen

Der Advisor nutzt kontinuierlich workloadbasierte Heuristik wie etwa Tabellenzugriffshäufigkeit, durchschnittlich zurückgegebene Zeilen sowie Schwellenwerte für Data Warehouse-Größe und -Aktivität, um hochwertige Empfehlungen zu generieren. 

Im Anschluss wird die workloadbasierte Heuristik beschrieben, die ggf. im Azure-Portal für die einzelnen Empfehlungen für replizierte Tabellen verfügbar ist:

- Scan avg (Scandurchschnitt): Der durchschnittliche Prozentsatz von Zeilen, die aus der Tabelle zurückgegeben wurden (für jeden Tabellenzugriff in den letzten sieben Tagen).
- Frequent read, no update (Häufig lesen, keine Aktualisierung): Gibt an, dass die Tabelle in den letzten sieben Tagen nicht aktualisiert wurde, aber Zugriffsaktivitäten festgestellt wurden.
- Read/update ratio (Verhältnis zwischen Lesen und Aktualisieren): Das Verhältnis zwischen Zugriffs- und Aktualisierungsvorgängen für die Tabelle in den letzten sieben Tagen.
- Aktivität: Ermittelt die Nutzung anhand der Zugriffsaktivität. Die Tabellenzugriffsaktivität wird dabei mit der durchschnittlichen Tabellenzugriffsaktivität innerhalb des gesamten Data Warehouse während der letzten sieben Tage verglichen. 

Derzeit zeigt der Advisor maximal vier Kandidaten für replizierte Tabellen auf einmal an – mit gruppierten Columnstore-Indizes und unter Priorisierung der höchsten Aktivität.

> [!IMPORTANT]
> Die Empfehlung für replizierte Tabellen ist nicht narrensicher und berücksichtigt keine Datenverschiebungsvorgänge. Wir arbeiten daran, dies als Heuristik hinzuzufügen, bis dahin empfiehlt es sich jedoch, die Workload nach Anwendung der Empfehlung zu überprüfen. Wenden Sie sich an sqldwadvisor@service.microsoft.com, wenn Ihnen Empfehlungen für replizierte Tabellen auffallen, die zu einer Verschlechterung Ihrer Workload führen. Weitere Informationen zu replizierten Tabellen finden Sie in [dieser Dokumentation](https://docs.microsoft.com/azure/sql-data-warehouse/design-guidance-for-replicated-tables#what-is-a-replicated-table).
