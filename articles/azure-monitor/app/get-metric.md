---
title: Get-Metric in Azure Monitor Application Insights
description: Erfahren Sie, wie Sie den Aufruf von GetMetric() effektiv zum Erfassen lokal vorab aggregierter Metriken für .NET- und .NET Core-Anwendungen mit Azure Monitor Application Insights einsetzen.
ms.service: azure-monitor
ms.subservice: application-insights
ms.topic: conceptual
ms.date: 04/28/2020
ms.openlocfilehash: 0ce2651d5cfcb1578d78982af109a004aaac11f4
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/03/2021
ms.locfileid: "101719779"
---
# <a name="custom-metric-collection-in-net-and-net-core"></a>Benutzerdefinierte Sammlung von Metriken in .NET und .NET Core

Die .NET und .NET Core SDKs für Azure Monitor Application Insights bieten zum Sammeln benutzerdefinierter Metriken die beiden Methoden `TrackMetric()` und `GetMetric()`. Der Hauptunterschied dieser beiden Methoden ist die lokale Aggregation. `TrackMetric()` fehlt eine Vorabaggregation, die dagegen von `GetMetric()` geboten wird. Der empfohlene Ansatz ist die Aggregation. Daher ist `TrackMetric()` nicht mehr die bevorzugte Methode zum Sammeln benutzerdefinierter Metriken. In diesem Artikel erfahren Sie mehr über die Verwendung der GetMetric()-Methode und ihre Funktionsweise.

## <a name="trackmetric-versus-getmetric"></a>Vergleich von TrackMetric und GetMetric

`TrackMetric()` sendet rohe Telemetriedaten, die eine Metrik angeben. Es ist ineffizient, für jeden Wert ein einzelnes Telemetrieelement zu senden. `TrackMetric()` ist auch, was die Leistung betrifft, nicht effizient, da jedes `TrackMetric(item)`-Element die vollständige SDK-Pipeline von Telemetrieinitialisierern und -prozessoren durchläuft. Im Gegensatz zu `TrackMetric()` übernimmt `GetMetric()` für Sie die lokale Vorabaggregation und übermittelt dann nur in einem festen Intervall von einer Minute eine aggregierte zusammenfassende Metrik. Wenn Sie also eine bestimmte benutzerdefinierte Metrik auf Sekunden- oder sogar Millisekundenebene genau überwachen müssen, können Sie dies tun, während Ihnen nur die Kosten für Speicher und Netzwerkdatenverkehr für eine minütliche Überwachung entstehen. Dadurch wird auch das Risiko einer Drosselung stark verringert, da die Gesamtanzahl der Telemetrieelemente, die für eine aggregierte Metrik gesendet werden müssen, erheblich reduziert wird.

In Application Insights unterliegen benutzerdefinierte Metriken, die über `TrackMetric()` und `GetMetric()` gesammelt wurden, nicht der [Stichprobenentnahme](./sampling.md). Die Stichprobenentnahme wichtiger Metriken kann zu Szenarien führen, in denen die Warnungen, die Sie möglicherweise für diese Metriken eingerichtet haben, unzuverlässig werden könnten. Dadurch, dass Sie keine Stichproben Ihrer benutzerdefinierten Metriken entnehmen, können Sie sich im Allgemeinen darauf verlassen, dass bei Überschreitung Ihrer Warnschwellenwerte eine Warnung ausgelöst wird.  Da jedoch für benutzerdefinierte Metriken keine Stichproben entnommen werden, gibt es möglicherweise einige Bedenken.

Wenn Sie Trends in einer Metrik im Sekundentakt oder in einem noch detaillierteren Intervall verfolgen müssen, kann dies zu folgendem Ergebnis führen:

- Höhere Datenspeicherkosten. Mit der Menge der Daten, die Sie an Azure Monitor senden, sind Kosten verbunden. (Je mehr Daten Sie senden, desto höher sind die gesamten Überwachungskosten.)
- Höherer Aufwand für Netzwerkdatenverkehr/Leistung. (In einigen Szenarien kann sich dies sowohl monetär als auch auf die Anwendungsleistung auswirken.)
- Risiko der Erfassungsdrosselung. Der Azure Monitor-Dienst entfernt („drosselt“) Datenpunkte, wenn Ihre App in kurzen Abständen eine große Menge an Telemetriedaten sendet.

Eine Drosselung ist insofern besonders problematisch, weil eine Drosselung ebenso wie eine Stichprobenentnahme zu verpassten Warnungen führen kann, da die Bedingung zum Auslösen einer Warnung lokal auftreten und dann am Erfassungsendpunkt aufgrund von zu vielen gesendeten Daten verloren gehen könnte. Aus diesem Grund empfehlen wir für .NET und .NET Core, `TrackMetric()` nur dann zu verwenden, wenn Sie Ihre eigene lokale Aggregationslogik implementiert haben. Wenn Sie versuchen, alle Fälle zu verfolgen, in denen ein Ereignis innerhalb einer bestimmten Zeitspanne auftritt, ist [`TrackEvent()`](./api-custom-events-metrics.md#trackevent) womöglich besser geeignet. Denken Sie jedoch daran, dass im Gegensatz zu benutzerdefinierten Metriken für benutzerdefinierte Ereignisse Stichproben entnommen werden müssen. Sie können zwar `TrackMetric()` weiterhin verwenden, auch ohne Ihre eigene lokale Vorabaggregation zu schreiben. Wenn Sie dies jedoch tun, sollten Sie sich der Tücken bewusst sein.

Zusammenfassend ist `GetMetric()` der empfohlene Ansatz, da dabei eine Vorabaggregation erfolgt, die Werte aus allen Aufrufen von Track() kumuliert und einmal pro Minute eine Zusammenfassung/Aggregation sendet. Dies kann den Kosten- und Leistungsaufwand erheblich reduzieren, da weniger Datenpunkte gesendet, aber dennoch alle relevanten Informationen erfasst werden.

> [!NOTE]
> Nur die .NET und .NET-Core SDKs bieten die GetMetric()-Methode. Wenn Sie mit Java arbeiten, können Sie [Micrometer-Metriken](./micrometer-java.md) oder `TrackMetric()` verwenden. Bei JavaScript und Node.js können Sie weiterhin `TrackMetric()` einsetzen. Beachten Sie jedoch die im vorherigen Abschnitt dargelegten Nachteile. Für Python können Sie [OpenCensus.stats](./opencensus-python.md#metrics) verwenden, um benutzerdefinierte Metriken zu senden. Die Metrikimplementierung unterscheidet sich aber.

## <a name="getting-started-with-getmetric"></a>Erste Schritte mit GetMetric

Für unsere Beispiele verwenden wir eine einfache .NET Core 3.1-Workerdienstanwendung. Wenn Sie die Testumgebung, die mit diesen Beispielen verwendet wurde, exakt nachbilden möchten, befolgen Sie die Schritte 1-6 des [Artikels zum Überwachen des Workerdiensts](./worker-service.md#net-core-30-worker-service-application), um Application Insights einer einfachen Projektvorlage für den Workerdienst hinzuzufügen. Diese Konzepte gelten für alle allgemeinen Anwendungen, für die das SDK verwendet werden kann, einschließlich Web- und Konsolen-Apps.

### <a name="sending-metrics"></a>Senden von Metriken

Ersetzen Sie den Inhalt Ihrer Datei `worker.cs` durch Folgendes:

```csharp
using System;
using System.Threading;
using System.Threading.Tasks;
using Microsoft.Extensions.Hosting;
using Microsoft.Extensions.Logging;
using Microsoft.ApplicationInsights;

namespace WorkerService3
{
    public class Worker : BackgroundService
    {
        private readonly ILogger<Worker> _logger;
        private TelemetryClient _telemetryClient;

        public Worker(ILogger<Worker> logger, TelemetryClient tc)
        {
            _logger = logger;
            _telemetryClient = tc;
        }

        protected override async Task ExecuteAsync(CancellationToken stoppingToken)
        {   // The following line demonstrates usages of GetMetric API.
            // Here "computersSold", a custom metric name, is being tracked with a value of 42 every second.
            while (!stoppingToken.IsCancellationRequested)
            {
                _telemetryClient.GetMetric("ComputersSold").TrackValue(42);

                _logger.LogInformation("Worker running at: {time}", DateTimeOffset.Now);
                await Task.Delay(1000, stoppingToken);
            }
        }
    }
}
```

Wenn Sie den obigen Code ausführen und beobachten, wie die Telemetriedaten über das Ausgabefenster von Visual Studio oder ein Tool wie Fiddler von Telerik gesendet werden, sehen Sie, wie die While-Schleife wiederholt ausgeführt wird, ohne dass Telemetriedaten gesendet werden. Dann wird ein einzelnes Telemetrieelement ungefähr bei der 60-Sekunden-Marke gesendet, die bei unserem Test wie folgt aussieht:

```json
Application Insights Telemetry: {"name":"Microsoft.ApplicationInsights.Dev.00000000-0000-0000-0000-000000000000.Metric", "time":"2019-12-28T00:54:19.0000000Z",
"ikey":"00000000-0000-0000-0000-000000000000",
"tags":{"ai.application.ver":"1.0.0.0",
"ai.cloud.roleInstance":"Test-Computer-Name",
"ai.internal.sdkVersion":"m-agg2c:2.12.0-21496",
"ai.internal.nodeName":"Test-Computer-Name"},
"data":{"baseType":"MetricData",
"baseData":{"ver":2,"metrics":[{"name":"ComputersSold",
"kind":"Aggregation",
"value":1722,
"count":41,
"min":42,
"max":42,
"stdDev":0}],
"properties":{"_MS.AggregationIntervalMs":"42000",
"DeveloperMode":"true"}}}}
```

Dieses einzelne Telemetrieelement stellt ein Aggregat von Messungen 41 verschiedener Metriken dar. Da wir immer wieder denselben Wert senden, haben wir die *Standardabweichung (stDev)* 0 mit identischen *maximalen (max)* und *minimalen (min)* Werten. Die Eigenschaft *value* stellt die Summe aller aggregierten Einzelwerte dar.

> [!NOTE]
> GetMetric unterstützt die Nachverfolgung des letzten Werts (d. h. das „Messgerät“) oder das Nachverfolgen von Histogrammen/Verteilungen nicht.

Wenn wir unsere Application Insights-Ressource auf der Oberfläche „Protokolle (Analytics)“ untersuchen, sieht dieses einzelne Telemetrieelement wie folgt aus:

![Log Analytics: Abfrageansicht](./media/get-metric/log-analytics.png)

> [!NOTE]
> Da das rohe Telemetrieelement nach der Erfassung weder eine explizite Summeneigenschaft noch ein Summenfeld enthielt, erstellen wir eine(s) für Sie. In diesem Fall stellen die Eigenschaften `value` und `valueSum` dasselbe dar.

Sie können auf Ihre benutzerdefinierte Metriktelemetrie auch im Abschnitt [_Metriken_](../essentials/metrics-charts.md) im Portal zugreifen: sowohl als [protokollbasierte als auch als benutzerdefinierte Metrik](pre-aggregated-metrics-log-metrics.md). (Der nachfolgende Screenshot ist ein Beispiel für protokollbasiert.) ![Ansicht im Metrik-Explorer](./media/get-metric/metrics-explorer.png)

### <a name="caching-metric-reference-for-high-throughput-usage"></a>Referenz zur Zwischenspeicherung von Metriken für Nutzung mit hohem Durchsatz

In einigen Fällen werden Metrikwerte sehr häufig beobachtet. Beispielsweise kann für einen Dienst mit hohem Durchsatz, der 500 Anforderungen pro Sekunde verarbeitet, der Wunsch bestehen, für jede Anforderung 20 Telemetriemetriken zu senden. Dies bedeutet, dass 10.000 Werte pro Sekunde nachverfolgt werden. In solchen Szenarien mit hohem Durchsatz müssen Benutzer dem SDK möglicherweise helfen, indem bestimmte Nachschlagevorgänge vermieden werden.

In diesem Fall wurde beispielsweise im obigen Beispiel eine Suche nach einem Handle für die Metrik ComputersSold durchgeführt und dann der beobachtete Wert 42 nachverfolgt. Stattdessen kann der Handle für Aufrufe mehrerer Nachverfolgungen zwischengespeichert werden:

```csharp
//...

        protected override async Task ExecuteAsync(CancellationToken stoppingToken)
        {
            // This is where the cache is stored to handle faster lookup
            Metric computersSold = _telemetryClient.GetMetric("ComputersSold");
            while (!stoppingToken.IsCancellationRequested)
            {

                computersSold.TrackValue(42);

                computersSold.TrackValue(142);

                _logger.LogInformation("Worker running at: {time}", DateTimeOffset.Now);
                await Task.Delay(50, stoppingToken);
            }
        }

```

Zusätzlich zur Zwischenspeicherung des Metrikhandles reduzierte das obige Beispiel auch `Task.Delay` auf 50 Millisekunden, sodass die Schleife häufiger ausgeführt wurde, was zu 772 Aufrufen von `TrackValue()` führte.

## <a name="multi-dimensional-metrics"></a>Mehrdimensionale Metriken

Die Beispiele im vorherigen Abschnitt zeigen Metriken ohne Dimensionen. Metriken können auch mehrdimensional sein. Wir unterstützen derzeit bis zu 10 Dimensionen.

 Es folgt ein Beispiel für das Erstellen einer eindimensionalen Metrik:

```csharp
//...

        protected override async Task ExecuteAsync(CancellationToken stoppingToken)
        {
            // This is an example of a metric with a single dimension.
            // FormFactor is the name of the dimension.
            Metric computersSold= _telemetryClient.GetMetric("ComputersSold", "FormFactor");

            while (!stoppingToken.IsCancellationRequested)
            {
                // The number of arguments (dimension values)
                // must match the number of dimensions specified while GetMetric.
                // Laptop, Tablet, etc are values for the dimension "FormFactor"
                computersSold.TrackValue(42, "Laptop");
                computersSold.TrackValue(20, "Tablet");
                computersSold.TrackValue(126, "Desktop");


                _logger.LogInformation("Worker running at: {time}", DateTimeOffset.Now);
                await Task.Delay(50, stoppingToken);
            }
        }

```

Wenn dieser Code mindestens 60 Sekunden ausgeführt wird, werden drei verschiedene Telemetrieelemente an Azure gesendet, die jeweils die Aggregation eines der drei Formfaktoren darstellen. Wie zuvor können Sie diese in der Ansicht „Protokolle (Analytics)“ überprüfen:

![Log Analytics-Ansicht der mehrdimensionalen Metrik](./media/get-metric/log-analytics-multi-dimensional.png)

Auch auf der Oberfläche von Metrik-Explorer:

![Benutzerdefinierte Metriken](./media/get-metric/custom-metrics.png)

Sie werden jedoch feststellen, dass Sie nicht in der Lage sind, die Metrik anhand Ihrer neuen benutzerdefinierten Dimension aufzuteilen oder Ihre benutzerdefinierte Dimension in der Metrikansicht anzuzeigen:

![Unterstützung der Aufteilung](./media/get-metric/splitting-support.png)

Standardmäßig sind mehrdimensionale Metriken auf der Metrik-Explorer-Oberfläche für Application Insights-Ressourcen nicht aktiviert.

### <a name="enable-multi-dimensional-metrics"></a>Aktivieren mehrdimensionaler Metriken

Um mehrdimensionale Metriken für eine Application Insights-Ressource zu aktivieren, wählen Sie **Nutzung und geschätzte Kosten** > **Benutzerdefinierte Metriken** > **Dimensionswarnungen für benutzerdefinierte Metriken aktivieren** > **OK** aus. Weitere Informationen dazu finden Sie [hier](pre-aggregated-metrics-log-metrics.md#custom-metrics-dimensions-and-pre-aggregation).

Nachdem Sie diese Änderung vorgenommen und neue mehrdimensionale Telemetriedaten gesendet haben, können Sie die **Aufteilung anwenden**.

> [!NOTE]
> Nur für neu gesendete Metriken, die nach dem Aktivieren der Funktion im Portal gesendet wurden, werden Dimensionen gespeichert.

![Anwenden des Teilens](./media/get-metric/apply-splitting.png)

Sie können Ihre Metrikaggregationen für jede _FormFactor_-Dimension anzeigen:

![Formfaktoren](./media/get-metric/formfactor.png)

### <a name="how-to-use-metricidentifier-when-there-are-more-than-three-dimensions"></a>Verwenden von MetricIdentifier, wenn mehr als drei Dimensionen vorhanden sind

Derzeit werden 10 Dimensionen unterstützt. Bei mehr als drei Dimensionen muss jedoch `MetricIdentifier` verwendet werden:

```csharp
// Add "using Microsoft.ApplicationInsights.Metrics;" to use MetricIdentifier
// MetricIdentifier id = new MetricIdentifier("[metricNamespace]","[metricId],"[dim1]","[dim2]","[dim3]","[dim4]","[dim5]");
MetricIdentifier id = new MetricIdentifier("CustomMetricNamespace","ComputerSold", "FormFactor", "GraphicsCard", "MemorySpeed", "BatteryCapacity", "StorageCapacity");
Metric computersSold  = _telemetryClient.GetMetric(id);
computersSold.TrackValue(110,"Laptop", "Nvidia", "DDR4", "39Wh", "1TB");
```

## <a name="custom-metric-configuration"></a>Konfiguration benutzerdefinierter Metriken

Wenn Sie die Metrikkonfiguration ändern möchten, müssen Sie dies an der Stelle tun, an der die Metrik initialisiert wird.

### <a name="special-dimension-names"></a>Besondere Dimensionsnamen

Metriken verwenden nicht den Telemetriekontext von `TelemetryClient`, der für den Zugriff auf sie verwendet wird. Spezielle Dimensionsnamen, die als Konstanten in der Klasse `MetricDimensionNames` verfügbar sind, sind die beste Umgehung dieser Einschränkung.

Für Metrikaggregate, die von der nachstehenden Metrik „Special Operation Request Size“ gesendet werden, wird `Context.Operation.Name` **nicht** auf „Special Operation“ festgelegt. Wohingegen bei `TrackMetric()` oder jedem anderen TrackXXX() `OperationName` ordnungsgemäß auf „Special Operation“ festgelegt wird.

``` csharp
        //...
        TelemetryClient specialClient;
        private static int GetCurrentRequestSize()
        {
            // Do stuff
            return 1100;
        }
        int requestSize = GetCurrentRequestSize()

        protected override async Task ExecuteAsync(CancellationToken stoppingToken)
        {
            while (!stoppingToken.IsCancellationRequested)
            {
                //...
                specialClient.Context.Operation.Name = "Special Operation";
                specialClient.GetMetric("Special Operation Request Size").TrackValue(requestSize);
                //...
            }
                   
        }
```

Verwenden Sie in diesem Fall die speziellen Dimensionsnamen, die in der `MetricDimensionNames`-Klasse aufgeführt sind, um `TelemetryContext`-Werte anzugeben.

Wenn z. B. das Metrikaggregat, das sich aus der nächsten Anweisung ergibt, an den Cloudendpunkt von Application Insights gesendet wird, wird sein Datenfeld `Context.Operation.Name` auf „Special Operation“ festgelegt:

```csharp
_telemetryClient.GetMetric("Request Size", MetricDimensionNames.TelemetryContext.Operation.Name).TrackValue(requestSize, "Special Operation");
```

Die Werte dieser speziellen Dimension werden in `TelemetryContext` kopiert und nicht als „normale“ Dimension verwendet. Wenn Sie eine Vorgangsdimension auch für die normale Erkundung von Metriken beibehalten möchten, müssen Sie für diesen Zweck eine separate Dimension erstellen:

```csharp
_telemetryClient.GetMetric("Request Size", "Operation Name", MetricDimensionNames.TelemetryContext.Operation.Name).TrackValue(requestSize, "Special Operation", "Special Operation");
```

### <a name="dimension-and-time-series-capping"></a>Obergrenzen von Dimensionen und Zeitreihen

 Um zu verhindern, dass das Telemetriesubsystem versehentlich Ihre Ressourcen ausschöpft, können Sie die maximale Anzahl von Datenreihen pro Metrik steuern. Die Standardgrenzwerte sind höchstens insgesamt 1.000 Datenreihen pro Metrik und höchstens 100 verschiedene Werte pro Dimension.

 Im Zusammenhang mit der Obergrenze für Dimensionen und Zeitreihen verwenden wir `Metric.TrackValue(..)`, um sicherzustellen, dass die Grenzwerte eingehalten werden. Wenn die Grenzwerte bereits erreicht sind, gibt `Metric.TrackValue(..)` FALSE zurück, sodass der Wert nicht nachverfolgt wird. Anderenfalls wird TRUE zurückgegeben. Dies ist nützlich, wenn die Daten für eine Metrik aus Benutzereingaben stammen.

Der Konstruktor `MetricConfiguration` übernimmt einige Optionen, wie verschiedene Reihen innerhalb der jeweiligen Metrik und ein Objekt einer Klasse zu verwalten sind, die `IMetricSeriesConfiguration` implementiert, die das Aggregationsverhalten für jede einzelne Reihe der Metrik angibt:

``` csharp
var metConfig = new MetricConfiguration(seriesCountLimit: 100, valuesPerDimensionLimit:2,
                new MetricSeriesConfigurationForMeasurement(restrictToUInt32Values: false));

Metric computersSold = _telemetryClient.GetMetric("ComputersSold", "Dimension1", "Dimension2", metConfig);

// Start tracking.
computersSold.TrackValue(100, "Dim1Value1", "Dim2Value1");
computersSold.TrackValue(100, "Dim1Value1", "Dim2Value2");

// The following call gives 3rd unique value for dimension2, which is above the limit of 2.
computersSold.TrackValue(100, "Dim1Value1", "Dim2Value3");
// The above call does not track the metric, and returns false.
```

* `seriesCountLimit` ist die maximale Anzahl von Datenzeitreihen, die eine Metrik enthalten kann. Sobald dieser Grenzwert erreicht wird, werden Aufrufe für `TrackValue()` nicht nachverfolgt.
* `valuesPerDimensionLimit` begrenzt auf ähnliche Weise die Anzahl der verschiedenen Werte pro Dimension.
* `restrictToUInt32Values` bestimmt, ob nur nicht negative ganzzahlige Werte nachverfolgt werden sollen oder nicht.

Es folgt ein Beispiel, wie Sie eine Nachricht senden, um zu erfahren, ob Obergrenzen überschritten werden:

```csharp
if (! computersSold.TrackValue(100, "Dim1Value1", "Dim2Value3"))
{
// Add "using Microsoft.ApplicationInsights.DataContract;" to use SeverityLevel.Error
_telemetryClient.TrackTrace("Metric value not tracked as value of one of the dimension exceeded the cap. Revisit the dimensions to ensure they are within the limits",
SeverityLevel.Error);
}
```

## <a name="next-steps"></a>Nächste Schritte

* [Erfahren Sie mehr](./worker-service.md) zum Überwachen von Workerdienstanwendungen.
* Weitere Informationen finden Sie unter [Protokollbasierte und vorab aggregierte Metriken](./pre-aggregated-metrics-log-metrics.md).
* [Metrik-Explorer](../essentials/metrics-getting-started.md)
* Aktivieren von Application Insights für [ASP.NET Core-Anwendungen](asp-net-core.md)
* Aktivieren von Application Insights für [ASP.NET-Anwendungen](asp-net.md)
