---
title: Optimale Nutzung von Azure Application Insights | Microsoft-Dokumentation
description: Nachdem Sie die ersten Schritte mit Application Insights gemacht haben, finden Sie hier eine Zusammenfassung der Funktionen, mit denen Sie sich beschäftigen können.
ms.topic: conceptual
ms.date: 02/03/2017
ms.openlocfilehash: 6d30833840b62412ae03c761baacf54c939c9d33
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/20/2020
ms.locfileid: "86540025"
---
# <a name="more-telemetry-from-application-insights"></a>Weitere Telemetriedaten aus Application Insights
Nachdem Sie [Application Insights zu Ihrem .ASP NET-Code hinzugefügt haben](../../azure-monitor/app/asp-net.md), können Sie einige weitere Einstellungen vornehmen, um noch mehr Telemetriedaten zu erhalten. 

| Action | Ergebnis|
|---|---|
|(IIS-Server) [Installieren Sie den Statusmonitor](https://go.microsoft.com/fwlink/?LinkId=506648) auf jedem Servercomputer.<br/>(Azure-Web-Apps) Öffnen Sie in der Azure-Systemsteuerung für die Web-App das Blatt „Application Insights“.| [**Leistungsindikatoren**](../../azure-monitor/app/performance-counters.md)<br/>[**Ausnahmen**](asp-net-exceptions.md) – ausführliche Stapelüberwachungen<br/>[**Abhängigkeiten**](../../azure-monitor/app/asp-net-dependencies.md)|
|[Fügen Sie den JavaScript-Codeausschnitt zu Webseiten hinzu](../../azure-monitor/app/javascript.md).|[Seitenleistung](../../azure-monitor/app/usage-overview.md), Browserausnahmen, AJAX-Leistung. Benutzerdefinierte clientseitige Telemetriedaten.|
|[Erstellen Sie Verfügbarkeitswebtests](../../azure-monitor/app/monitor-web-app-availability.md).|Sie erhalten Benachrichtigungen, wenn Ihre Website nicht mehr verfügbar ist.|
|[Stellen Sie sicher, dass „buildinfo.config“](/visualstudio/debugger/diagnose-problems-after-deployment?view=vs-2015) von MSBuild generiert wird.|[Anmerkungen zu Builds in Metrikdiagrammen](./annotations.md)
|[Schreiben Sie benutzerdefinierte Ereignisse und Metriken](../../azure-monitor/app/api-custom-events-metrics.md).|Anzahl der Geschäftsereignisse und Metriken, Nachverfolgung ausführlicher Informationen zur Nutzung und vieles mehr.|
|[Erstellen Sie ein Profil Ihrer Livewebsite](https://aka.ms/AIProfilerPreview).|Detaillierte Funktionszeitangaben aus Ihrer Live-Web-App|
