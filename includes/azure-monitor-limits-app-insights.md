---
title: include file
description: include file
services: azure-monitor
author: rboucher
tags: azure-service-management
ms.topic: include
ms.date: 02/07/2019
ms.author: robb
ms.custom: include file
ms.openlocfilehash: e224faca3c384ee5b3df02b015199ac5eab403bc
ms.sourcegitcommit: d4734bc680ea221ea80fdea67859d6d32241aefc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/14/2021
ms.locfileid: "100370099"
---
Es gibt einige Grenzwerte hinsichtlich der Anzahl von Metriken und Ereignissen pro Anwendung (d. h. pro Instrumentationsschlüssel). Die Beschränkungen hängen von dem von Ihnen ausgewählten [Tarif](https://azure.microsoft.com/pricing/details/application-insights/) ab.

| Resource | Begrenzung | Hinweis
| --- | --- | --- |
| Gesamtdaten pro Tag | 100 GB | Die Datenmenge kann durch Festlegen einer Obergrenze reduziert werden. Wird eine höhere Datenmenge benötigt, können Sie den Grenzwert im Portal auf bis zu 1.000 GB erhöhen. Bei Kapazitäten über 1.000 GB senden Sie eine E-Mail an AIDataCap@microsoft.com.
| Drosselung | 32.000 Ereignisse/s | Das Limit wird eine Minute lang gemessen.
| Beibehaltung von Daten | [30-730 Tage](../articles/azure-monitor/app/pricing.md#change-the-data-retention-period) | Hierbei handelt es sich um eine Ressource für [Suche](../articles/azure-monitor/app/diagnostic-search.md), [Analyse](../articles/azure-monitor/log-query/log-query-overview.md) und [Metrik-Explorer](../articles/azure-monitor/platform/metrics-charts.md).
| [Webtests in mehreren Schritten](../articles/azure-monitor/app/availability-multistep.md) mit detaillierter Ergebnisaufbewahrung | 90 Tage | Diese Ressource liefert detaillierte Ergebnisse der einzelnen Schritte.
| Maximale Ereignisgröße | 64.000 Bytes |
| Eingenschaft und Länge der Namen von Metriken | 150 | Siehe [Typschemas](https://github.com/MohanGsk/ApplicationInsights-Home/tree/master/EndpointSpecs/Schemas/Bond/).
| Zeichenfolgenlänge des Eigenschaftswerts | 8\.192 | Siehe [Typschemas](https://github.com/MohanGsk/ApplicationInsights-Home/tree/master/EndpointSpecs/Schemas/Bond/).
| Länge von Ablaufverfolgungs- und Ausnahmebenachrichtigungen | 32,768  | Siehe [Typschemas](https://github.com/MohanGsk/ApplicationInsights-Home/tree/master/EndpointSpecs/Schemas/Bond/).
| [Verfügbarkeitstests](../articles/azure-monitor/app/monitor-web-app-availability.md) Anzahl pro App | 100 |
| Vermerkdauer von [Profiler](../articles/azure-monitor/app/profiler.md)-Daten | 5 Tage |
| Pro Tag gesendete [Profiler](../articles/azure-monitor/app/profiler.md)-Daten | 10 GB |

Weitere Informationen zu [Preisen und Kontingenten für Application Insights ](../articles/azure-monitor/app/pricing.md)finden Sie hier.
