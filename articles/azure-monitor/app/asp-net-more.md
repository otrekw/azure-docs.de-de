---
title: Optimale Nutzung von Azure Application Insights | Microsoft-Dokumentation
description: Nachdem Sie die ersten Schritte mit Application Insights gemacht haben, finden Sie hier eine Zusammenfassung der Funktionen, mit denen Sie sich beschäftigen können.
ms.topic: conceptual
ms.date: 02/03/2017
ms.openlocfilehash: c0be377636159dbb2bdb628af6a0bc9077942397
ms.sourcegitcommit: a76ff927bd57d2fcc122fa36f7cb21eb22154cfa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/28/2020
ms.locfileid: "87321342"
---
# <a name="more-telemetry-from-application-insights"></a>Weitere Telemetriedaten aus Application Insights
Nachdem Sie [Application Insights zu Ihrem .ASP NET-Code hinzugefügt haben](./asp-net.md), können Sie einige weitere Einstellungen vornehmen, um noch mehr Telemetriedaten zu erhalten. 

| Action | Ergebnis|
|---|---|
|(IIS-Server) [Installieren Sie den Statusmonitor](https://go.microsoft.com/fwlink/?LinkId=506648) auf jedem Servercomputer.<br/>(Azure-Web-Apps) Öffnen Sie in der Azure-Systemsteuerung für die Web-App das Blatt „Application Insights“.| [**Leistungsindikatoren**](./performance-counters.md)<br/>[**Ausnahmen**](asp-net-exceptions.md) – ausführliche Stapelüberwachungen<br/>[**Abhängigkeiten**](./asp-net-dependencies.md)|
|[Fügen Sie den JavaScript-Codeausschnitt zu Webseiten hinzu](./javascript.md).|[Seitenleistung](./usage-overview.md), Browserausnahmen, AJAX-Leistung. Benutzerdefinierte clientseitige Telemetriedaten.|
|[Erstellen Sie Verfügbarkeitswebtests](./monitor-web-app-availability.md).|Sie erhalten Benachrichtigungen, wenn Ihre Website nicht mehr verfügbar ist.|
|[Stellen Sie sicher, dass „buildinfo.config“](/visualstudio/debugger/diagnose-problems-after-deployment?view=vs-2015) von MSBuild generiert wird.|[Anmerkungen zu Builds in Metrikdiagrammen](./annotations.md)
|[Schreiben Sie benutzerdefinierte Ereignisse und Metriken](./api-custom-events-metrics.md).|Anzahl der Geschäftsereignisse und Metriken, Nachverfolgung ausführlicher Informationen zur Nutzung und vieles mehr.|
|[Erstellen Sie ein Profil Ihrer Livewebsite](https://aka.ms/AIProfilerPreview).|Detaillierte Funktionszeitangaben aus Ihrer Live-Web-App|

