---
title: Überwachen der Nutzung und Leistung bei Windows-Desktop-Apps
description: Analysieren Sie die Nutzung und Leistung Ihrer Windows-Desktop-App mit Application Insights.
ms.topic: conceptual
ms.date: 06/11/2020
ms.openlocfilehash: 1b8909c47594ebd752035ca88b23d4b836345f88
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "84718783"
---
# <a name="monitoring-usage-and-performance-in-classic-windows-desktop-apps"></a>Überwachen der Nutzung und Leistung von klassischen Windows-Desktop-Apps

Lokal, in Azure oder in anderen Clouds gehostete Anwendungen können Application Insights nutzen. Die einzige Einschränkung besteht darin, dass die [Kommunikation](../../azure-monitor/app/ip-addresses.md) mit dem Application Insights-Dienst zugelassen werden muss. Für die Überwachung von UWP-Anwendungen (universelle Windows-Plattform) wird [Visual Studio App Center](../../azure-monitor/learn/mobile-center-quickstart.md) empfohlen.

## <a name="to-send-telemetry-to-application-insights-from-a-classic-windows-application"></a>So senden Sie Telemetriedaten aus einer klassischen Windows-Anwendung an Application Insights
1. Erstellen Sie im [Azure-Portal](https://portal.azure.com)[eine Application Insights-Ressource](../../azure-monitor/app/create-new-resource.md ). 
2. Erstellen Sie eine Kopie des Instrumentierungsschlüssels.
3. Bearbeiten Sie die NuGet-Pakete Ihres App-Projekts in Visual Studio, und fügen Sie ihnen „Microsoft.ApplicationInsights.WindowsServer“ hinzu. (Oder wählen Sie „Microsoft.ApplicationInsights“ aus, wenn Sie nur die Basis-API ohne die Standardmodule zur Telemetriesammlung verwenden möchten.)
4. Legen Sie den Instrumentierungsschlüssel im Code fest:
   
    `TelemetryConfiguration.Active.InstrumentationKey = "` *Ihr Schlüssel* `";`
   
    Oder legen Sie ihn in der Datei „ApplicationInsights.config“ fest (wenn Sie eines der Standardtelemetriepakete installiert haben):
   
    `<InstrumentationKey>`*Ihr Schlüssel*`</InstrumentationKey>` 
   
    Wenn Sie die Datei „ApplicationInsights.config“ verwenden, stellen Sie sicher, dass ihre Eigenschaften im Projektmappen-Explorer auf **Buildvorgang = Inhalt, In Ausgabeverzeichnis kopieren = Kopieren**festgelegt sind.
5. [Verwenden Sie die API](../../azure-monitor/app/api-custom-events-metrics.md) , um Telemetriedaten zu senden.
6. Führen Sie die App aus, und sehen Sie sich die Telemetriedaten in der Ressource an, die Sie im Azure-Portal erstellt haben.

## <a name="example-code"></a><a name="telemetry"></a>Beispielcode

```csharp
using Microsoft.ApplicationInsights;

    public partial class Form1 : Form
    {
        private TelemetryClient tc = new TelemetryClient();
        ...
        private void Form1_Load(object sender, EventArgs e)
        {
            // Alternative to setting ikey in config file:
            tc.InstrumentationKey = "key copied from portal";

            // Set session data:
            tc.Context.Session.Id = Guid.NewGuid().ToString();
            tc.Context.Device.OperatingSystem = Environment.OSVersion.ToString();

            // Log a page view:
            tc.TrackPageView("Form1");
            ...
        }

        protected override void OnClosing(System.ComponentModel.CancelEventArgs e)
        {
            e.Cancel = true;

            if (tc != null)
            {
                tc.Flush(); // only for desktop apps

                // Allow time for flushing:
                System.Threading.Thread.Sleep(1000);
            }
            base.OnClosing(e);
        }

```

## <a name="override-storage-of-computer-name"></a>Außerkraftsetzung des gespeicherten Computernamens

Standardmäßig erfasst und speichert dieses SDK den Computernamen des Systems, das Telemetriedaten ausgibt.

Der Computername wird vom [Legacy-Enterprise-Tarif (pro Knoten)](https://docs.microsoft.com/azure/azure-monitor/app/pricing#legacy-enterprise-per-node-pricing-tier) von Application Insights zu internen Abrechnungszwecken verwendet. Wenn Sie `telemetry.Context.Cloud.RoleInstance` mithilfe eines Telemetrie-Initialisierers überschreiben, wird standardmäßig die separate `ai.internal.nodeName`-Eigenschaft gesendet, die weiterhin den Wert des Computernamens enthält. Dieser Wert wird nicht mit Ihrer Application Insights-Telemetrie gespeichert, sondern intern bei der Erfassung verwendet, um Abwärtskompatibilität mit dem knotenbasierten Legacyabrechnungsmodell zuzulassen.

Wenn Sie den [Legacy-Enterprise-Tarif (pro Knoten)](https://docs.microsoft.com/azure/azure-monitor/app/pricing#legacy-enterprise-per-node-pricing-tier) verwenden und lediglich die Speicherung des Computernamens außer Kraft setzen müssen, verwenden Sie einen Telemetrie-Initialisierer:

**Schreiben Sie einen benutzerdefinierten Telemetrie-Initialisierer wie nachfolgend gezeigt.**

```csharp
using Microsoft.ApplicationInsights.Channel;
using Microsoft.ApplicationInsights.Extensibility;

namespace CustomInitializer.Telemetry
{
    public class MyTelemetryInitializer : ITelemetryInitializer
    {
        public void Initialize(ITelemetry telemetry)
        {
            if (string.IsNullOrEmpty(telemetry.Context.Cloud.RoleInstance))
            {
                // Set custom role name here. Providing an empty string will result
                // in the computer name still be sent via this property.
                  telemetry.Context.Cloud.RoleInstance = "Custom RoleInstance";
            }
        }
    }
}
```

Instanziieren Sie den Initialisierer in der `Main()`-Methode `Program.cs` unten, indem Sie den Instrumentierungsschlüssel festlegen:

```csharp
 using Microsoft.ApplicationInsights.Extensibility;
 using CustomInitializer.Telemetry;

   static void Main()
        {
            TelemetryConfiguration.Active.InstrumentationKey = "{Instrumentation-key-here}";
            TelemetryConfiguration.Active.TelemetryInitializers.Add(new MyTelemetryInitializer());
            //...
        }
```

## <a name="override-transmission-of-computer-name"></a>Außerkraftsetzung der Übertragung des Computernamens

Wenn Sie nicht den [Legacy-Enterprise-Tarif (pro Knoten)](https://docs.microsoft.com/azure/azure-monitor/app/pricing#legacy-enterprise-per-node-pricing-tier) verwenden und alle Telemetriedaten, die den Computernamen enthalten, vollständig verhindern möchten, müssen Sie einen Telemetrieprozessor verwenden.

### <a name="telemetry-processor"></a>Telemetrieprozessor

```csharp
using Microsoft.ApplicationInsights.Channel;
using Microsoft.ApplicationInsights.Extensibility;


namespace WindowsFormsApp2
{
    public class CustomTelemetryProcessor : ITelemetryProcessor
    {
        private readonly ITelemetryProcessor _next;

        public CustomTelemetryProcessor(ITelemetryProcessor next)
        {
            _next = next;
        }

        public void Process(ITelemetry item)
        {
            if (item != null)
            {
                item.Context.Cloud.RoleInstance = string.Empty;
            }

            _next.Process(item);
        }
    }
}
```

Instanziieren Sie den Telemetrieprozessor in der `Main()`-Methode von `Program.cs` unter der Einstellung des Instrumentierungsschlüssels:

```csharp
using Microsoft.ApplicationInsights.Extensibility;

namespace WindowsFormsApp2
{
    static class Program
    {
        static void Main()
        {
            TelemetryConfiguration.Active.InstrumentationKey = "{Instrumentation-key-here}";
            var builder = TelemetryConfiguration.Active.DefaultTelemetrySink.TelemetryProcessorChainBuilder;
            builder.Use((next) => new CustomTelemetryProcessor(next));
            builder.Build();
            //...
        }
    }
}

```

> [!NOTE]
> Obwohl Sie selbst im [Legacy-Enterprise-Tarif (pro Knoten)](https://docs.microsoft.com/azure/azure-monitor/app/pricing#legacy-enterprise-per-node-pricing-tier) grundsätzlich auch wie oben beschrieben einen Telemetrieprozessor verwenden können, führt dies zu einer möglichen erhöhten Abrechnung, da Knoten für die Abrechnung pro Knoten nicht ordnungsgemäß voneinander unterschieden werden können.

## <a name="next-steps"></a>Nächste Schritte
* [Erstellen eines Dashboards](../../azure-monitor/app/overview-dashboard.md)
* [Diagnosesuche](../../azure-monitor/app/diagnostic-search.md)
* [Untersuchen von Metriken](../../azure-monitor/platform/metrics-charts.md)
* [Schreiben von Analytics-Abfragen](../../azure-monitor/app/analytics.md)

