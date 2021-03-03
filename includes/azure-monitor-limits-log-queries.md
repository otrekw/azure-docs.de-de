---
title: include file
description: include file
services: azure-monitor
author: rboucher
tags: azure-service-management
ms.topic: include
ms.date: 07/22/2019
ms.author: bwren
ms.custom: include file
ms.openlocfilehash: 5f2b77c7d8e1a2da9517183043231b717b6cceab
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/03/2021
ms.locfileid: "101734041"
---
### <a name="general-query-limits"></a>Allgemeine Abfragegrenzwerte

| Begrenzung | BESCHREIBUNG |
|:---|:---|
| Abfragesprache | Azure Monitor verwendet dieselbe [Kusto-Abfragesprache](/azure/kusto/query/) wie Azure Data Explorer. Weitere Informationen finden Sie unter [Azure Monitor – Unterschiede in der Protokollabfragesprache](/azure/data-explorer/kusto/query/) für KQL-Sprachelemente, die in Azure Monitor nicht unterstützt werden. |
| Azure-Regionen | Protokollabfragen können zu einem übermäßigen Aufwand führen, wenn sich die Daten über Log Analytics-Arbeitsbereiche in mehreren Azure-Regionen erstrecken. Weitere Informationen finden Sie unter [Abfragegrenzwerte](../articles/azure-monitor/logs/scope.md#query-scope-limits). |
| Ressourcenübergreifende Abfragen | Die maximale Anzahl von Application Insights-Ressourcen und Log Analytics-Arbeitsbereichen in einer einzelnen Abfrage ist auf 100 beschränkt.<br>Ressourcenübergreifende Abfrage wird im View Designer nicht unterstützt.<br>Eine ressourcenübergreifende Abfrage in Protokollwarnungen wird in der neuen scheduledQueryRules-API unterstützt.<br>Ausführliche Informationen finden Sie unter [Ressourcenübergreifende Abfragelimits](../articles/azure-monitor/logs/cross-workspace-query.md#cross-resource-query-limits). |

### <a name="user-query-throttling"></a>Drosselung von Benutzerabfragen
In Azure Monitor gelten mehrere Drosselungsgrenzwerte zum Schutz vor einer übermäßige Anzahl von Abfragen. Dieses Verhalten kann die System-Back-End-Ressourcen überlasten und die Reaktionsfähigkeit des Diensts gefährden. Die folgenden Grenzwerte sind darauf ausgelegt, Kunden vor Unterbrechungen zu schützen und eine konsistente Dienstebene zu gewährleisten. Die Drosselung und die Grenzwerte für Benutzer betreffen nur Szenarien mit übermäßigem Verbrauch und sind bei normaler Verwendung in der Regel nicht relevant.


| "Measure" | Grenzwert pro Benutzer | BESCHREIBUNG |
|:---|:---|:---|
| Gleichzeitige Abfragen | 5 | Wenn für den Benutzer bereits fünf Abfragen ausgeführt werden, werden alle neuen Abfragen in eine Parallelitätswarteschlange pro Benutzer eingereiht. Wenn eine der ausgeführten Abfragen beendet wird, wird die nächste Abfrage aus der Warteschlange abgerufen und gestartet. Das bezieht sich nicht auf Abfragen aus Warnungsregeln.
| Zeit in der Parallelitätswarteschlange | 3 Minuten | Wenn sich eine Abfrage länger als drei Minuten in der Warteschlange befindet, ohne zu starten, wird sie mit einer HTTP-Fehlerantwort mit dem Code 429 beendet. |
| Gesamtanzahl von Abfragen in der Parallelitätswarteschlange | 200 | Sobald die Anzahl von Abfragen in der Warteschlange 200 erreicht, werden alle weiteren Abfragen mit dem HTTP-Fehlercode 429 abgelehnt. Diese Zahl gilt zusätzlich zu den fünf Abfragen, die gleichzeitig ausgeführt werden können. |
| Abfragerate | 200 Abfragen pro 30 Sekunden | Dies ist die Gesamtrate, mit der Abfragen von einem einzelnen Benutzer an alle Arbeitsbereiche übermittelt werden können.  Dieser Grenzwert gilt für programmgesteuerte Abfragen und für Abfragen, die von Visualisierungskomponenten wie Azure-Dashboards und der Zusammenfassungsseite für den Log Analytics-Arbeitsbereich initiiert wurden. |

- Optimieren Sie Ihre Abfragen wie unter [Optimieren von Protokollabfragen in Azure Monitor](../articles/azure-monitor/logs/query-optimization.md) beschrieben.
- Dashboards und Arbeitsmappen können mehrere Abfragen in einer einzelnen Ansicht enthalten, die bei jedem Laden oder Aktualisieren einen Burst von Abfragen generieren. Zerlegen Sie sie ggf. in mehrere Ansichten, die bedarfsgesteuert geladen werden. 
- Erwägen Sie, in Power BI anstelle von unformatierten Protokollen nur aggregierte Ergebnisse zu extrahieren.