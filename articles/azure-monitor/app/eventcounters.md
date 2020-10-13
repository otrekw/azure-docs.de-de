---
title: EventCounters in Application Insights | Microsoft-Dokumentation
description: Überwachen Sie systemeigene und benutzerdefinierte .NET/.NET Core-EventCounters in Application Insights.
ms.topic: conceptual
ms.date: 09/20/2019
ms.custom: devx-track-csharp
ms.openlocfilehash: a9af36f3c81ee52b41a8eed875c1a286b95bf838
ms.sourcegitcommit: 23aa0cf152b8f04a294c3fca56f7ae3ba562d272
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/07/2020
ms.locfileid: "91803642"
---
# <a name="eventcounters-introduction"></a>Einführung in EventCounters

[`EventCounter`](/dotnet/core/diagnostics/event-counters) ist ein .NET/.NET Core-Mechanismus zum Veröffentlichen und Verwenden von Indikatoren oder Statistiken. EventCounters werden auf allen Betriebssystemplattformen unterstützt: Windows, Linux und macOS. Sie können als plattformübergreifende Entsprechung für [PerformanceCounters](/dotnet/api/system.diagnostics.performancecounter) angesehen werden, die nur auf Windows-Systemen unterstützt werden.

Während Benutzer beliebige benutzerdefinierte `EventCounters` veröffentlichen können, um ihre Anforderungen zu erfüllen, veröffentlicht die .NET Core-Runtime ab Version 3.0 standardmäßig eine Gruppe dieser Zähler. In diesem Dokument werden Sie durch die erforderlichen Schritte zum Erfassen und Anzeigen von `EventCounters` (systemdefiniert oder benutzerdefiniert) in Azure Application Insights geführt.

## <a name="using-application-insights-to-collect-eventcounters"></a>Verwenden von Application Insights für das Erfassen von EventCounters

Application Insights unterstützt das Erfassen von `EventCounters` mit `EventCounterCollectionModule`, einem Bestandteil des neu veröffentlichten NuGet-Pakets [Microsoft.ApplicationInsights.EventCounterCollector](https://www.nuget.org/packages/Microsoft.ApplicationInsights.EventCounterCollector). `EventCounterCollectionModule` wird bei Verwendung von [AspNetCore](asp-net-core.md) oder [WorkerService](worker-service.md) automatisch aktiviert. `EventCounterCollectionModule` erfasst Indikatoren mit einer nicht konfigurierbaren Erfassungshäufigkeit von 60 Sekunden. Zum Erfassen von EventCounters sind keine speziellen Berechtigungen erforderlich.

## <a name="default-counters-collected"></a>Erfasste Standardindikatoren

Ab Version 2.15.0 von entweder [AspNetCore SDK](asp-net-core.md) oder [WorkerService SDK](worker-service.md) werden standardmäßig keine Indikatoren erfasst. Das Modul selbst wird aktiviert, damit Benutzer einfach die gewünschten Zähler hinzufügen können, um sie zu erfassen.

Eine Liste bekannter Indikatoren, die von der .NET-Runtime veröffentlicht werden, finden Sie im Dokument [Verfügbare Leistungsindikatoren](/dotnet/core/diagnostics/event-counters#available-counters).

## <a name="customizing-counters-to-be-collected"></a>Anpassen zu erfassender Indikatoren

Das folgende Beispiel veranschaulicht das Hinzufügen und Entfernen von Indikatoren. Diese Anpassung erfolgt in der `ConfigureServices`-Methode Ihrer Anwendung, nachdem die Erfassung von Telemetriedaten durch Application Insights entweder mit `AddApplicationInsightsTelemetry()` oder `AddApplicationInsightsWorkerService()` aktiviert wurde. Nachfolgend sehen Sie einen Beispielcode aus einer ASP.NET Core-Anwendung. Informationen zu anderen Anwendungstypen finden Sie in [diesem](worker-service.md#configuring-or-removing-default-telemetrymodules) Dokument.

```csharp
    using Microsoft.ApplicationInsights.Extensibility.EventCounterCollector;
    using Microsoft.Extensions.DependencyInjection;

    public void ConfigureServices(IServiceCollection services)
    {
        //... other code...

        // The following code shows how to configure the module to collect
        // additional counters.
        services.ConfigureTelemetryModule<EventCounterCollectionModule>(
            (module, o) =>
            {
                // This removes all default counters, if any.
                module.Counters.Clear();

                // This adds a user defined counter "MyCounter" from EventSource named "MyEventSource"
                module.Counters.Add(new EventCounterCollectionRequest("MyEventSource", "MyCounter"));

                // This adds the system counter "gen-0-size" from "System.Runtime"
                module.Counters.Add(new EventCounterCollectionRequest("System.Runtime", "gen-0-size"));
            }
        );
    }
```

## <a name="disabling-eventcounter-collection-module"></a>Deaktivieren des Sammlungsmoduls EventCounter

`EventCounterCollectionModule` kann mithilfe von `ApplicationInsightsServiceOptions` deaktiviert werden. Ein Beispiel der Verwendung des ASP.NET Core SDK finden Sie nachstehend.

```csharp
    using Microsoft.ApplicationInsights.AspNetCore.Extensions;
    using Microsoft.Extensions.DependencyInjection;

    public void ConfigureServices(IServiceCollection services)
    {
        //... other code...

        var applicationInsightsServiceOptions = new ApplicationInsightsServiceOptions();
        applicationInsightsServiceOptions.EnableEventCounterCollectionModule = false;
        services.AddApplicationInsightsTelemetry(applicationInsightsServiceOptions);
    }
```

Ein ähnlicher Ansatz kann auch für das WorkerService SDK befolgt werden, wobei jedoch der Namespace wie im nachstehenden Beispiel gezeigt geändert werden muss.

```csharp
    using Microsoft.ApplicationInsights.WorkerService;
    using Microsoft.Extensions.DependencyInjection;

    var applicationInsightsServiceOptions = new ApplicationInsightsServiceOptions();
    applicationInsightsServiceOptions.EnableEventCounterCollectionModule = false;
    services.AddApplicationInsightsTelemetryWorkerService(applicationInsightsServiceOptions);
```

## <a name="event-counters-in-metric-explorer"></a>EventCounters im Metrik-Explorer

Zum Anzeigen von EventCounter-Metriken im [Metrik-Explorer](../platform/metrics-charts.md) wählen Sie die Application Insights-Ressource aus, und wählen Sie dann protokollbasierte Metriken als Metriknamespace aus. EventCounter-Metriken werden dann unter der Kategorie „Benutzerdefiniert“ angezeigt.

> [!div class="mx-imgBorder"]
> ![Im Metrik-Explorer von Application Insights gemeldete EventCounters](./media/event-counters/metrics-explorer-counter-list.png)

## <a name="event-counters-in-analytics"></a>EventCounters in Analytics

Sie können Berichte zu EventCounters auch in [Analytics](../log-query/log-query-overview.md) in der Tabelle **customMetrics** suchen und anzeigen.

Führen Sie z.B. die folgende Abfrage aus, um zu sehen, welche Indikatoren erfasst werden und für die Abfrage verfügbar sind:

```Kusto
customMetrics | summarize avg(value) by name
```

> [!div class="mx-imgBorder"]
> ![In Application Insights Analytics gemeldete EventCounters](./media/event-counters/analytics-event-counters.png)

Um ein Diagramm eines bestimmten Indikators (z.B. `ThreadPool Completed Work Item Count`) im aktuellen Zeitraum zu erhalten, führen Sie die folgende Abfrage aus.

```Kusto
customMetrics 
| where name contains "System.Runtime|ThreadPool Completed Work Item Count"
| where timestamp >= ago(1h)
| summarize  avg(value) by cloud_RoleInstance, bin(timestamp, 1m)
| render timechart
```
> [!div class="mx-imgBorder"]
> ![Diagramm eines einzelnen Indikators in Application Insights](./media/event-counters/analytics-completeditems-counters.png)

Wie für andere Telemetriedaten ist auch in der Tabelle **customMetrics** eine `cloud_RoleInstance`-Spalte enthalten, die die Identität der Hostserverinstanz angibt, auf dem Ihre Anwendung ausgeführt wird. Die obige Abfrage zeigt den Indikatorwert pro Instanz und kann zum Vergleichen der Leistung verschiedener Serverinstanzen verwendet werden.

## <a name="alerts"></a>Alerts
Wie bei anderen Metriken können Sie [eine Warnung einrichten](../platform/alerts-log.md), damit Sie gewarnt werden, wenn ein EventCounter einen von Ihnen festgelegten Grenzwert überschreitet. Öffnen Sie den Bereich „Warnungen“, und klicken Sie auf „Warnung hinzufügen“.

## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

### <a name="can-i-see-eventcounters-in-live-metrics"></a>Werden EventCounters in Livemetriken angezeigt?

Livemetriken zeigen derzeit keine EventCounters an. Verwenden Sie den Metrik-Explorer oder Analytics, um die Telemetrie anzuzeigen.

### <a name="i-have-enabled-application-insights-from-azure-web-app-portal-but-i-cant-see-eventcounters"></a>Ich habe Application Insights über das Azure-Web-App-Portal aktiviert. Es werden aber keine EventCounters angezeigt.

 Die [Application Insights-Erweiterung](./azure-web-apps.md) für ASP.NET Core unterstützt dieses Feature noch nicht. Dieses Dokument wird aktualisiert, sobald dieses Feature unterstützt wird.

## <a name="next-steps"></a><a name="next"></a>Nächste Schritte

* [Abhängigkeitsüberwachung](./asp-net-dependencies.md)

