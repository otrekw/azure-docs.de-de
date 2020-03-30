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
ms.openlocfilehash: f2092753ab054a639e208aab4a6b7317c1bd5edb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "74795739"
---
| Begrenzung | BESCHREIBUNG |
|:---|:---|
| Abfragesprache | Azure Monitor verwendet dieselbe [Kusto-Abfragesprache](/azure/kusto/query/) wie Azure Data Explorer. Weitere Informationen finden Sie unter [Azure Monitor – Unterschiede in der Protokollabfragesprache](../articles/azure-monitor/log-query/data-explorer-difference.md) für KQL-Sprachelemente, die in Azure Monitor nicht unterstützt werden. |
| Azure-Regionen | Protokollabfragen können zu einem übermäßigen Aufwand führen, wenn sich die Daten über Log Analytics-Arbeitsbereiche in mehreren Azure-Regionen erstrecken. Weitere Informationen finden Sie unter [Abfragegrenzwerte](../articles/azure-monitor/log-query/scope.md#query-limits). |
| Ressourcenübergreifende Abfragen | Die maximale Anzahl von Application Insights-Ressourcen und Log Analytics-Arbeitsbereichen in einer einzelnen Abfrage ist auf 100 beschränkt.<br>Ressourcenübergreifende Abfrage wird im View Designer nicht unterstützt.<br>Eine ressourcenübergreifende Abfrage in Protokollwarnungen wird in der neuen scheduledQueryRules-API unterstützt.<br>Ausführliche Informationen finden Sie unter [Ressourcenübergreifende Abfragelimits](../articles/azure-monitor/log-query/cross-workspace-query.md#cross-resource-query-limits). |
| Abfragedrosselung | Ein Benutzer darf maximal 200 Abfragen pro 30 Sekunden in einer beliebigen Anzahl von Arbeitsbereichen ausführen. Dieser Grenzwert gilt für programmgesteuerte Abfragen und für Abfragen, die von Visualisierungskomponenten wie Azure-Dashboards und der Zusammenfassungsseite für den Log Analytics-Arbeitsbereich initiiert wurden. |
