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
ms.openlocfilehash: 627b020ce618a2a1f2646a95e143947876bd6a15
ms.sourcegitcommit: af1cbaaa4f0faa53f91fbde4d6009ffb7662f7eb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/22/2020
ms.locfileid: "82072636"
---
### <a name="general-query-limits"></a>Allgemeine Abfragegrenzwerte

| Begrenzung | BESCHREIBUNG |
|:---|:---|
| Abfragesprache | Azure Monitor verwendet dieselbe [Kusto-Abfragesprache](/azure/kusto/query/) wie Azure Data Explorer. Weitere Informationen finden Sie unter [Azure Monitor – Unterschiede in der Protokollabfragesprache](../articles/azure-monitor/log-query/data-explorer-difference.md) für KQL-Sprachelemente, die in Azure Monitor nicht unterstützt werden. |
| Azure-Regionen | Protokollabfragen können zu einem übermäßigen Aufwand führen, wenn sich die Daten über Log Analytics-Arbeitsbereiche in mehreren Azure-Regionen erstrecken. Weitere Informationen finden Sie unter [Abfragegrenzwerte](../articles/azure-monitor/log-query/scope.md#query-limits). |
| Ressourcenübergreifende Abfragen | Die maximale Anzahl von Application Insights-Ressourcen und Log Analytics-Arbeitsbereichen in einer einzelnen Abfrage ist auf 100 beschränkt.<br>Ressourcenübergreifende Abfrage wird im View Designer nicht unterstützt.<br>Eine ressourcenübergreifende Abfrage in Protokollwarnungen wird in der neuen scheduledQueryRules-API unterstützt.<br>Ausführliche Informationen finden Sie unter [Ressourcenübergreifende Abfragelimits](../articles/azure-monitor/log-query/cross-workspace-query.md#cross-resource-query-limits). |

### <a name="user-query-throttling"></a>Drosselung von Benutzerabfragen
In Azure Monitor gelten mehrere Drosselungsgrenzwerte zum Schutz vor einer übermäßige Anzahl von Abfragen. Dieses Verhalten kann die System-Back-End-Ressourcen überlasten und die Reaktionsfähigkeit des Diensts gefährden. Die folgenden Grenzwerte sind darauf ausgelegt, Kunden vor Unterbrechungen zu schützen und eine konsistente Dienstebene zu gewährleisten. Die Drosselung und die Grenzwerte für Benutzer betreffen nur Szenarien mit übermäßigem Verbrauch und sind bei normaler Verwendung in der Regel nicht relevant.


| "Measure" | Grenzwert pro Benutzer | BESCHREIBUNG |
|:---|:---|:---|
| Gleichzeitige Abfragen | 5 | Wenn für den Benutzer bereits fünf Abfragen ausgeführt werden, werden alle neuen Abfragen in eine Parallelitätswarteschlange pro Benutzer eingereiht. Wenn eine der ausgeführten Abfragen beendet wird, wird die nächste Abfrage aus der Warteschlange abgerufen und gestartet. Das bezieht sich nicht auf Abfragen aus Warnungsregeln.
| Zeit in der Parallelitätswarteschlange | Zweieinhalb Minuten | Wenn sich eine Abfrage länger als zweieinhalb Minuten in der Warteschlange befindet, ohne zu starten, wird sie mit einer HTTP-Fehlerantwort mit dem Code 429 beendet. |
| Gesamtanzahl von Abfragen in der Parallelitätswarteschlange | 40 | Sobald die Anzahl von Abfragen in der Warteschlange 40 erreicht, werden alle weiteren Abfragen mit dem HTTP-Fehlercode 429 abgelehnt. Diese Zahl gilt zusätzlich zu den fünf Abfragen, die gleichzeitig ausgeführt werden können. |
| Abfragerate | 200 Abfragen pro 30 Sekunden | Dies ist die Gesamtrate, mit der Abfragen von einem einzelnen Benutzer an alle Arbeitsbereiche übermittelt werden können.  Dieser Grenzwert gilt für programmgesteuerte Abfragen und für Abfragen, die von Visualisierungskomponenten wie Azure-Dashboards und der Zusammenfassungsseite für den Log Analytics-Arbeitsbereich initiiert wurden. |

- Optimieren Sie Ihre Abfragen wie unter [Optimieren von Protokollabfragen in Azure Monitor](../articles/azure-monitor/log-query/query-optimization.md) beschrieben.
- Dashboards und Arbeitsmappen können mehrere Abfragen in einer einzelnen Ansicht enthalten, die bei jedem Laden oder Aktualisieren einen Burst von Abfragen generieren. Zerlegen Sie sie ggf. in mehrere Ansichten, die bedarfsgesteuert geladen werden. 
- Erwägen Sie, in Power BI anstelle von unformatierten Protokollen nur aggregierte Ergebnisse zu extrahieren.