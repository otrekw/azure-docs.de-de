---
title: Überwachung des Arbeitsauslastungsverwaltungsportals
description: Leitfaden zur Überwachung des Arbeitsauslastungsverwaltungsportals in Azure Synapse Analytics.
services: synapse-analytics
author: ronortloff
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 02/04/2020
ms.author: rortloff
ms.reviewer: jrasnick
ms.custom: azure-synapse
ms.openlocfilehash: 31533eefbfae63e0eb4049d2eabaf6b853340636
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2020
ms.locfileid: "83590246"
---
# <a name="azure-synapse-analytics--workload-management-portal-monitoring"></a>Azure Synapse Analytics – Überwachung des Arbeitsauslastungsverwaltungsportals

In diesem Artikel wird erläutert, wie Sie die Ressourcenverwendung und Abfrageaktivität der [Arbeitsauslastungsgruppe](sql-data-warehouse-workload-isolation.md#workload-groups) überwachen.
Ausführliche Informationen zum Konfigurieren des Azure-Metrik-Explorers finden Sie im Artikel [Erste Schritte mit dem Azure-Metrik-Explorer](../../azure-monitor/platform/metrics-getting-started.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json).  Ausführliche Informationen zum Überwachen der Systemressourcenverwendung finden Sie in der Dokumentation zur Überwachung von Azure Synapse Analytics im Abschnitt [Ressourcenverwendung](sql-data-warehouse-concept-resource-utilization-query-activity.md#resource-utilization).
Zum Überwachen der Arbeitsauslastungsverwaltung werden zwei verschiedene Kategorien von Arbeitsauslastungsgruppen-Metriken bereitgestellt: Ressourcenzuordnung und Abfrageaktivität.  Diese Metriken können nach Arbeitsauslastungsgruppen aufgeteilt und gefiltert werden.  Die Metriken können danach aufgeteilt und gefiltert werden, ob sie systemdefiniert sind (Ressourcenklassen-Arbeitsauslastungsgruppen) oder benutzerdefiniert (vom Benutzer mit [CREATE WORKLOAD GROUP](/sql/t-sql/statements/create-workload-group-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)-Syntax erstellt).

## <a name="workload-management-metric-definitions"></a>Metrikdefinitionen der Arbeitsauslastungsverwaltung

|Metrikname                    |BESCHREIBUNG  |Aggregationstyp |
|-------------------------------|-------------|-----------------|
|Effektives Ressourcenlimit (Prozent) | *Effektives Ressourcenlimit (Prozent)* ist eine feste Beschränkung des Prozentsatzes der Ressourcen, auf die die Arbeitsauslastungsgruppe zugreifen kann, unter Berücksichtigung des Wertes, der anderen Arbeitsauslastungsgruppen für *Effektive Mindestanzahl von Ressourcen (Prozent)* zugeordnet ist. Die Metrik *Effektives Ressourcenlimit (Prozent)* wird mithilfe des `CAP_PERCENTAGE_RESOURCE`-Parameters in der [CREATE WORKLOAD GROUP](/sql/t-sql/statements/create-workload-group-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)-Syntax konfiguriert.  Der effektive Wert wird hier beschrieben.<br><br>Wenn beispielsweise eine Arbeitsauslastungsgruppe `DataLoads` mit `CAP_PERCENTAGE_RESOURCE` = 100 erstellt wird und eine andere Arbeitsauslastungsgruppe mit einem effektiven minimalen Ressourcenprozentsatz von 25 %, beträgt der Wert von *Effektives Ressourcenlimit (Prozent)* für die Arbeitsauslastungsgruppe `DataLoads` 75 %.<br><br>*Effektives Ressourcenlimit (Prozent)* bestimmt die obere Grenze der Parallelität (und somit des potenziellen Durchsatzes), die eine Arbeitsauslastungsgruppe erreichen kann.  Wenn zusätzlicher Durchsatz erforderlich ist, der über den derzeit von der Metrik *Effektives Ressourcenlimit (Prozent)* gemeldeten Durchsatz hinausgeht, erhöhen Sie entweder `CAP_PERCENTAGE_RESOURCE`, verringern Sie `MIN_PERCENTAGE_RESOURCE` anderer Arbeitsauslastungsgruppen, oder skalieren Sie die Instanz hoch, um weitere Ressourcen hinzuzufügen.  Das Verringern von `REQUEST_MIN_RESOURCE_GRANT_PERCENT` kann zwar die Parallelität erhöhen, erhöht aber nicht unbedingt den Gesamtdurchsatz.| Min, Avg, Max |
|Effektive Mindestanzahl von Ressourcen (Prozent) |*Effektive Mindestanzahl von Ressourcen (Prozent)* ist der Mindestprozentsatz von Ressourcen, die unter Berücksichtigung des Dienstebenenminimums für die Arbeitsauslastungsgruppe reserviert und isoliert sind.  Die Metrik „Effektive Mindestanzahl von Ressourcen (Prozent)“ wird mithilfe des `MIN_PERCENTAGE_RESOURCE`-Parameters in der [CREATE WORKLOAD GROUP](/sql/t-sql/statements/create-workload-group-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)-Syntax konfiguriert.  Der effektive Wert wird [hier](/sql/t-sql/statements/create-workload-group-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest#effective-values) beschrieben.<br><br>Verwenden Sie den Aggregationstyp „Sum“, wenn diese Metrik ungefiltert und nicht aufgeteilt ist, um die gesamte auf dem System konfigurierte Arbeitsauslastungsisolation zu überwachen.<br><br>*Effektive Mindestanzahl von Ressourcen (Prozent)* bestimmt die untere Grenze der garantierten Parallelität (und somit des garantierten Durchsatzes), die eine Arbeitsauslastungsgruppe erreichen kann.  Wenn zusätzliche garantierte Ressourcen benötigt werden, die über den derzeit von der Metrik *Effektive Mindestanzahl von Ressourcen (Prozent)* gemeldeten Wert hinausgehen, erhöhen Sie den für die Arbeitsauslastungsgruppe konfigurierten `MIN_PERCENTAGE_RESOURCE`-Parameter.  Das Verringern von `REQUEST_MIN_RESOURCE_GRANT_PERCENT` kann zwar die Parallelität erhöhen, erhöht aber nicht unbedingt den Gesamtdurchsatz. |Min, Avg, Max|
|Aktive Abfragen von Arbeitsauslastungsgruppen  |Diese Metrik meldet die aktiven Abfragen in der Arbeitsauslastungsgruppe.  Bei Verwenden dieser Metrik ohne Filterung und Aufteilung werden alle aktiven Abfragen angezeigt, die im System ausgeführt werden.|SUM         |
|Zuordnung von Arbeitsauslastungsgruppen nach maximalem Ressourcenprozentsatz |Diese Metrik zeigt die prozentuale Zuordnung von Ressourcen in Relation zu *Effektives Ressourcenlimit (Prozent)* pro Arbeitsauslastungsgruppe an.  Diese Metrik informiert über die effektive Auslastung der Arbeitsauslastungsgruppe.<br><br>Angenommen, für die Arbeitsauslastungsgruppe `DataLoads` ist für *Effektives Ressourcenlimit (Prozent)* 75 % festgelegt und `REQUEST_MIN_RESOURCE_GRANT_PERCENT` mit 25 % konfiguriert.  Der nach `DataLoads` gefilterte Wert von *Zuordnung von Arbeitsauslastungsgruppen nach maximalem Ressourcenprozentsatz* beträgt 33 % (25 %/75 %), wenn eine einzelne Abfrage in dieser Arbeitsauslastungsgruppe ausgeführt wird.<br><br>Verwenden Sie diese Metrik, um die Auslastung einer Arbeitsauslastungsgruppe zu ermitteln.  Ein Wert in der Nähe von 100 % gibt an, dass alle für die Arbeitsauslastungsgruppe verfügbaren Ressourcen verwendet werden.  Außerdem würde ein größerer Wert als 0 (null) der *Metrik für in der Warteschlange befindliche Abfragen der Arbeitsauslastungsgruppe* für die gleiche Arbeitsauslastungsgruppe darauf hinweisen, dass die Arbeitsauslastungsgruppe zusätzliche Ressourcen nutzen würde, wenn sie zugeordnet wären.  Umgekehrt gilt: Wenn diese Metrik konstant niedrig ist und *Aktive Abfragen von Arbeitsauslastungsgruppen* niedrig ist, ist die Arbeitsauslastungsgruppe nicht ausgelastet.  Diese Situation ist besonders problematisch, wenn *Effektives Ressourcenlimit (Prozent)* größer als 0 (null) ist, da dies auf eine [unterausgelastete Arbeitsauslastungsisolation](#underutilized-workload-isolation) hinweisen würde.|Min, Avg, Max |
|Zuordnung von Arbeitsauslastungsgruppen nach Systemprozentsatz | Diese Metrik zeigt die prozentuale Zuordnung von Ressourcen in Relation zum gesamten System an.<br><br>Angenommen, für eine Arbeitsauslastungsgruppe `DataLoads` ist `REQUEST_MIN_RESOURCE_GRANT_PERCENT` mit 25 % konfiguriert.  Der nach `DataLoads` gefilterte Wert von *Zuordnung von Arbeitsauslastungsgruppen nach Systemprozentsatz* beträgt 25 % (25 %/100 %), wenn eine einzelne Abfrage in dieser Arbeitsauslastungsgruppe ausgeführt wird.|Min, Avg, Max |
|Abfragetimeouts für Arbeitsauslastungsgruppen |Abfragen für die Arbeitsauslastungsgruppe, für die ein Timeout aufgetreten ist.  Die von dieser Metrik gemeldeten Abfragetimeouts sind erst nach dem Start der Abfrage aufgetreten (Wartezeiten aufgrund von Sperren oder Ressourcenwartezeiten sind nicht enthalten).<br><br>Das Abfragetimeout wird mit dem Parameter `QUERY_EXECUTION_TIMEOUT_SEC` in der Syntax [CREATE WORKLOAD GROUP](/sql/t-sql/statements/create-workload-group-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) konfiguriert.  Wenn Sie den Wert erhöhen, kann die Anzahl der Abfragetimeouts reduziert werden.<br><br>Erhöhen Sie ggf. den `REQUEST_MIN_RESOURCE_GRANT_PERCENT`-Parameter für die Arbeitsauslastungsgruppe, um den Umfang der Timeouts zu verringern und mehr Ressourcen pro Abfrage zuzuordnen.  Beachten Sie, dass das Erhöhen von `REQUEST_MIN_RESOURCE_GRANT_PERCENT` die Parallelität für die Arbeitsauslastungsgruppe reduziert. |SUM |
|In der Warteschlange befindliche Abfragen der Arbeitsauslastungsgruppe | Abfragen der Arbeitsauslastungsgruppe, die sich derzeit in der Warteschlange befinden und auf die Ausführung warten.  Abfragen können in die Warteschlange eingereiht werden, da sie auf Ressourcen oder Sperren warten.<br><br>Abfragen können aus zahlreichen Gründen warten.  Wenn das System überlastet ist und der Parallelitätsbedarf die Verfügbarkeit überfordert, werden Abfragen in die Warteschlange eingereiht.<br><br>Erwägen Sie, der Arbeitsauslastungsgruppe durch Erhöhen des `CAP_PERCENTAGE_RESOURCE`-Parameters in der [CREATE WORKLOAD GROUP](/sql/t-sql/statements/create-workload-group-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)-Anweisung weitere Ressourcen hinzuzufügen.  Wenn `CAP_PERCENTAGE_RESOURCE` größer ist als die Metrik *Effektives Ressourcenlimit (Prozent)* , wirkt sich die für eine andere Arbeitsauslastungsgruppe konfigurierte Arbeitsauslastungsisolation auf die für diese Arbeitsauslastungsgruppe zugeordneten Ressourcen aus.  Erwägen Sie, `MIN_PERCENTAGE_RESOURCE` anderer Arbeitsauslastungsgruppen zu verringern, oder skalieren Sie die Instanz hoch, um weitere Ressourcen hinzuzufügen. |SUM |

## <a name="monitoring-scenarios-and-actions"></a>Überwachungsszenarien und -aktionen

Im Folgenden finden Sie eine Reihe von Diagrammkonfigurationen, um die Verwendung von Metriken zur Arbeitsauslastungsverwaltung für die Problembehandlung mit zugehörigen Aktionen hervorzuheben.

### <a name="underutilized-workload-isolation"></a>Unterausgelastete Arbeitsauslastungsisolation

Beachten Sie die folgende Arbeitsauslastungsgruppen- und Klassifiziererkonfiguration, bei der eine Arbeitsauslastungsgruppe mit dem Namen `wgPriority` erstellt und ihr `membername` *TheCEO* mithilfe des Arbeitsauslastungsklassifizierers `wcCEOPriority` zugeordnet wird.  Für die Arbeitsauslastungsgruppe `wgPriority` ist eine Arbeitsauslastungsisolation von 25 % konfiguriert (`MIN_PERCENTAGE_RESOURCE` = 25).  Jede Abfrage, die von *TheCEO* übermittelt wird, erhält 5 % der Systemressourcen (`REQUEST_MIN_RESOURCE_GRANT_PERCENT` = 5).

```sql
CREATE WORKLOAD GROUP wgPriority
WITH ( MIN_PERCENTAGE_RESOURCE = 25
      ,CAP_PERCENTAGE_RESOURCE = 50
      ,REQUEST_MIN_RESOURCE_GRANT_PERCENT = 5);

CREATE WORKLOAD CLASSIFIER wcCEOPriority
WITH ( WORKLOAD_GROUP = 'wgPriority'
      ,MEMBERNAME = 'TheCEO');
```

Das folgende Diagramm ist wie folgt konfiguriert:<br>
Metrik 1: *Effektive Mindestanzahl von Ressourcen (Prozent)* (Avg-Aggregation, `blue line`)<br>
Metrik 2: *Zuordnung von Arbeitsauslastungsgruppen nach Systemprozentsatz* (Avg-Aggregation, `purple line`)<br>
Filter: [Arbeitsauslastungsgruppe] = `wgPriority`<br>
![underutilized-wg.png](./media/sql-data-warehouse-workload-management-portal-monitor/underutilized-wg.png) Das Diagramm zeigt, dass bei einer Arbeitsauslastungsisolation von 25 % nur durchschnittlich 10 % verwendet werden.  In diesem Fall könnte der `MIN_PERCENTAGE_RESOURCE`-Parameter auf einen Wert zwischen 10 und 15 verringert werden, sodass andere Arbeitsauslastungen im System die Ressourcen nutzen können.

### <a name="workload-group-bottleneck"></a>Arbeitsauslastungsgruppen-Engpass

Beachten Sie die folgende Arbeitsauslastungsgruppen- und Klassifiziererkonfiguration, bei der eine Arbeitsauslastungsgruppe mit dem Namen `wgDataAnalyst` erstellt und ihr `membername` *DataAnalyst* mithilfe des Arbeitsauslastungsklassifizierers `wcDataAnalyst` zugeordnet wird.  Für die Arbeitsauslastungsgruppe `wgDataAnalyst` ist eine Arbeitsauslastungsisolation von 6 % (`MIN_PERCENTAGE_RESOURCE` = 6) und ein Ressourcenlimit von 9 % (`CAP_PERCENTAGE_RESOURCE` = 9) konfiguriert.  Jede Abfrage, die von *DataAnalyst* übermittelt wird, erhält 3 % der Systemressourcen (`REQUEST_MIN_RESOURCE_GRANT_PERCENT` = 3).

```sql
CREATE WORKLOAD GROUP wgDataAnalyst  
WITH ( MIN_PERCENTAGE_RESOURCE = 6
      ,CAP_PERCENTAGE_RESOURCE = 9
      ,REQUEST_MIN_RESOURCE_GRANT_PERCENT = 3);

CREATE WORKLOAD CLASSIFIER wcDataAnalyst
WITH ( WORKLOAD_GROUP = 'wgDataAnalyst'
      ,MEMBERNAME = 'DataAnalyst');
```

Das folgende Diagramm ist wie folgt konfiguriert:<br>
Metrik 1: *Effektives Ressourcenlimit (Prozent)* (Avg-Aggregation, `blue line`)<br>
Metrik 2: *Zuordnung von Arbeitsauslastungsgruppen nach maximalem Ressourcenprozentsatz* (Avg-Aggregation, `purple line`)<br>
Metrik 3: *In der Warteschlange befindliche Abfragen der Arbeitsauslastungsgruppe* (Sum-Aggregation, `turquoise line`)<br>
Filter: [Arbeitsauslastungsgruppe] = `wgDataAnalyst`<br>
![bottle-necked-wg](./media/sql-data-warehouse-workload-management-portal-monitor/bottle-necked-wg.png) Das Diagramm zeigt, dass die Arbeitsauslastungsgruppe bei einer Begrenzung der Ressourcen auf 9 % zu 90 %+ (der Metrik *Zuordnung von Arbeitsauslastungsgruppen nach maximalem Ressourcenprozentsatz*) ausgelastet ist.  Abfragen werden kontinuierlich in die Warteschlange gestellt, wie die *Metrik für in der Warteschlange befindliche Abfragen der Arbeitsauslastungsgruppe* zeigt.  Wenn Sie in diesem Fall `CAP_PERCENTAGE_RESOURCE` auf einen Wert über 9 % erhöhen, können mehrere Abfragen gleichzeitig ausgeführt werden.  Das Erhöhen von `CAP_PERCENTAGE_RESOURCE` setzt voraus, dass genügend Ressourcen verfügbar sind und nicht von anderen Arbeitsauslastungsgruppen isoliert werden.  Überprüfen Sie das erhöhte Limit anhand der Metrik *Effektives Ressourcenlimit (Prozent)* .  Wenn mehr Durchsatz gewünscht ist, sollten Sie auch `REQUEST_MIN_RESOURCE_GRANT_PERCENT` auf einen Wert über 3 erhöhen.  Durch Erhöhen von `REQUEST_MIN_RESOURCE_GRANT_PERCENT` könnten Abfragen schneller ausgeführt werden.

## <a name="next-steps"></a>Nächste Schritte

- [Schnellstart: Konfigurieren der Arbeitsauslastungsisolation mithilfe von T-SQL](quickstart-configure-workload-isolation-tsql.md)<br>
- [CREATE WORKLOAD GROUP (Transact-SQL)](/sql/t-sql/statements/create-workload-group-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)<br>
- [CREATE WORKLOAD CLASSIFIER (Transact-SQL)](/sql/t-sql/statements/create-workload-classifier-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)<br>
- [Überwachen von Ressourcennutzung und Abfrageaktivität in Azure SQL Data Warehouse](sql-data-warehouse-concept-resource-utilization-query-activity.md)
