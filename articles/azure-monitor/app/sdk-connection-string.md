---
title: Verbindungszeichenfolgen in Azure Application Insights | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Verbindungszeichenfolgen verwenden.
ms.topic: conceptual
author: timothymothra
ms.author: tilee
ms.date: 01/17/2020
ms.custom: devx-track-javascript, devx-track-csharp
ms.reviewer: mbullwin
ms.openlocfilehash: 944257c93e00dca77507f26db15f7bf45fbb387e
ms.sourcegitcommit: 62e1884457b64fd798da8ada59dbf623ef27fe97
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/26/2020
ms.locfileid: "88936417"
---
# <a name="connection-strings"></a>Verbindungszeichenfolgen

## <a name="overview"></a>Übersicht

Verbindungszeichenfolgen bieten Benutzern von Application Insight eine einzelne Konfigurationseinstellung, sodass nicht mehrere Proxyeinstellungen benötigt werden. Dies ist sehr hilfreich für Intranetwebserver sowie Sovereign Cloud- oder Hybrid Cloud-Umgebungen, aus denen Daten an den Überwachungsdienst gesendet werden sollen.

Die Schlüssel-Wert-Paare stellen eine einfache Möglichkeit dar, mit der Benutzer eine Präfix-Suffix-Kombination für jeden Application Insights-Dienst und jedes Application Insights-Produkt definieren können.

> [!IMPORTANT]
> Es wird nicht empfohlen, sowohl die Verbindungszeichenfolge als auch den Instrumentierungsschlüssel festzulegen. Wenn ein Benutzer beide festgelegt hat, hat die zuletzt festgelegte Einstellung Vorrang. 


## <a name="scenario-overview"></a>Übersicht über das Szenario 

Kundenszenarien, die die wichtigsten Auswirkungen veranschaulichen:

- Firewallausnahmen oder Proxyumleitungen 

    Wenn eine Überwachung für den Intranetwebserver erforderlich ist, mussten Kunden bei unseren früheren Lösungen ihrer Konfiguration einzelne Dienstendpunkte hinzufügen. Weitere Informationen finden Sie [hier](../faq.md#can-i-monitor-an-intranet-web-server). 
    Verbindungszeichenfolgen stellen eine bessere Alternative dar, da der gesamte Aufwand auf eine einzige Einstellung reduziert wird. Ein einfacher Präfix-Suffix-Zusatz ermöglicht das automatische Auffüllen und Umleiten aller Endpunkte an die richtigen Dienste. 

- Sovereign Cloud- oder Hybrid Cloud-Umgebungen

    Benutzer können Daten an eine definierte [Azure Government-Region](../../azure-government/compare-azure-government-global-azure.md#application-insights) senden.
    Mit Verbindungszeichenfolgen können Sie Endpunkteinstellungen für Ihre Intranetserver oder Hybrid Cloud-Einstellungen definieren. 

## <a name="getting-started"></a>Erste Schritte

### <a name="finding-my-connection-string"></a>Wie finde ich meine Verbindungszeichenfolge heraus?

Ihre Verbindungszeichenfolge wird auf dem Übersichtsblatt Ihrer Application Insights-Ressource angezeigt.

![Verbindungszeichenfolge auf Übersichtsblatt](media/overview-dashboard/overview-connection-string.png)

### <a name="schema"></a>Schema

#### <a name="max-length"></a>Max. Länge

Die Verbindung hat eine maximale unterstützte Länge von 4.096 Zeichen.

#### <a name="key-value-pairs"></a>Schlüssel-Wert-Paare

Die Verbindungszeichenfolge besteht aus einer Liste von Einstellungen, die als Schlüssel-Wert-Paare dargestellt werden, die durch Semikolons voneinander getrennt sind: `key1=value1;key2=value2;key3=value3`

#### <a name="syntax"></a>Syntax

- `InstrumentationKey` (Beispiel: 00000000-0000-0000-0000-000000000000) Die Verbindungszeichenfolge ist ein **erforderliches** Feld.
- `Authorization` (Beispiel: iKey) (Diese Einstellung ist optional, da derzeit nur die iKey-Autorisierung unterstützt wird.)
- `EndpointSuffix` (Beispiel: applicationinsights.azure.cn) Das Festlegen des Endpunktsuffixes weist das SDK an, mit welcher Azure-Cloud eine Verbindung hergestellt werden soll. Das SDK stellt den Rest des Endpunkts für die einzelnen Dienste zusammen.
- Explizite Endpunkte.
  Jeder Dienst kann in der Verbindungszeichenfolge explizit überschrieben werden.
   - `IngestionEndpoint` (Beispiel: `https://dc.applicationinsights.azure.com`)
   - `LiveEndpoint` (Beispiel: `https://live.applicationinsights.azure.com`)
   - `ProfilerEndpoint` (Beispiel: `https://profiler.applicationinsights.azure.com`)
   - `SnapshotEndpoint` (Beispiel: `https://snapshot.applicationinsights.azure.com`)

#### <a name="endpoint-schema"></a>Endpunktschema

`<prefix>.<suffix>`
- Präfix: Definiert einen Dienst. 
- Suffix: Definiert den allgemeinen Domänennamen.

##### <a name="valid-suffixes"></a>Gültige Suffixe

Im Folgenden finden Sie eine Liste gültiger Suffixe. 
- applicationinsights.azure.cn
- applicationinsights.us


Weitere Informationen: https://docs.microsoft.com/azure/azure-monitor/app/custom-endpoints#regions-that-require-endpoint-modification


##### <a name="valid-prefixes"></a>Gültige Präfixe

- [Telemetriedatenerfassung:](./app-insights-overview.md) `dc`
- [Livemetriken:](./live-stream.md) `live`
- [Profiler:](./profiler-overview.md) `profiler`
- [Momentaufnahme:](./snapshot-debugger.md) `snapshot`



## <a name="connection-string-examples"></a>Beispiele für Verbindungszeichenfolgen


### <a name="minimal-valid-connection-string"></a>Minimal gültige Verbindungszeichenfolge

`InstrumentationKey=00000000-0000-0000-0000-000000000000;`

In diesem Beispiel wurde nur der Instrumentierungsschlüssel festgelegt.

- Das Autorisierungsschema ist standardmäßig „ikey“. 
- Instrumentierungsschlüssel: 00000000-0000-0000-0000-000000000000
- Die regionalen Dienst-URIs basieren auf den [SDK-Standardwerten](https://github.com/microsoft/ApplicationInsights-dotnet/blob/e50d569cebf485e72e98f4a08a0bc0e30cdf42bc/BASE/src/Microsoft.ApplicationInsights/Extensibility/Implementation/Endpoints/Constants.cs#L6) und bieten eine Verbindung mit der öffentlichen globalen Azure-Cloud:
   - Erfassung: `https://dc.services.visualstudio.com/`
   - Livemetriken: `https://rt.services.visualstudio.com/`
   - Profiler: `https://agent.azureserviceprofiler.net/`
   - Debugger: `https://agent.azureserviceprofiler.net/`



### <a name="connection-string-with-endpoint-suffix"></a>Verbindungszeichenfolge mit Endpunktsuffix

`InstrumentationKey=00000000-0000-0000-0000-000000000000;EndpointSuffix=ai.contoso.com;`

In diesem Beispiel gibt diese Verbindungszeichenfolge das Endpunktsuffix an, und das SDK erstellt die Dienstendpunkte.

- Das Autorisierungsschema ist standardmäßig „ikey“. 
- Instrumentierungsschlüssel: 00000000-0000-0000-0000-000000000000
- Die regionalen Dienst-URIs basieren auf dem bereitgestellten Endpunktsuffix: 
   - Erfassung: `https://dc.ai.contoso.com`
   - Livemetriken: `https://live.ai.contoso.com`
   - Profiler: `https://profiler.ai.contoso.com`
   - Debugger: `https://snapshot.ai.contoso.com`  



### <a name="connection-string-with-explicit-endpoint-overrides"></a>Verbindungszeichenfolge mit expliziten Endpunktüberschreibungen 

`InstrumentationKey=00000000-0000-0000-0000-000000000000;IngestionEndpoint=https://custom.com:111/;LiveEndpoint=https://custom.com:222/;ProfilerEndpoint=https://custom.com:333/;SnapshotEndpoint=https://custom.com:444/;`

In diesem Beispiel gibt diese Verbindungszeichenfolge explizite Überschreibungen für jeden Dienst an. Das SDK verwendet die genauen Endpunkte, die ohne Änderung bereitgestellt werden.

- Das Autorisierungsschema ist standardmäßig „ikey“. 
- Instrumentierungsschlüssel: 00000000-0000-0000-0000-000000000000
- Die regionalen Dienst-URIs basieren auf den expliziten Überschreibungswerten: 
   - Erfassung: `https://custom.com:111/`
   - Livemetriken: `https://custom.com:222/`
   - Profiler: `https://custom.com:333/`
   - Debugger: `https://custom.com:444/`  


## <a name="how-to-set-a-connection-string"></a>Festlegen einer Verbindungszeichenfolge

Verbindungszeichenfolgen werden in den folgenden SDK-Versionen unterstützt:
- .NET und .NET Core v2.12.0
- Java v2.5.1 und Java 3.0
- JavaScript v2.3.0
- NodeJS v1.5.0
- Python v1.0.0

Eine Verbindungszeichenfolge kann im Code, in einer Umgebungsvariable oder in einer Konfigurationsdatei festgelegt werden.



### <a name="environment-variable"></a>Umgebungsvariable

- Verbindungszeichenfolge: `APPLICATIONINSIGHTS_CONNECTION_STRING`

# <a name="netnetcore"></a>[.NET/.NetCore](#tab/net)

TelemetryConfiguration.ConnectionString: https://github.com/microsoft/ApplicationInsights-dotnet/blob/add45ceed35a817dc7202ec07d3df1672d1f610d/BASE/src/Microsoft.ApplicationInsights/Extensibility/TelemetryConfiguration.cs#L271-L274

.NET (explizit festgelegt):
```csharp
var configuration = new TelemetryConfiguration
{
    ConnectionString = "InstrumentationKey=00000000-0000-0000-0000-000000000000;"
};
```

.NET (Konfigurationsdatei):

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationInsights xmlns="http://schemas.microsoft.com/ApplicationInsights/2013/Settings">
    <ConnectionString>InstrumentationKey=00000000-0000-0000-0000-000000000000</ConnectionString>
</ApplicationInsights>
```

.NET Core (explizit festgelegt):
```csharp
public void ConfigureServices(IServiceCollection services)
{
    var options = new ApplicationInsightsServiceOptions { ConnectionString = "InstrumentationKey=00000000-0000-0000-0000-000000000000;" };
    services.AddApplicationInsightsTelemetry(options: options);
}
```

NetCore (config.json): 

```json
{
  "ApplicationInsights": {
    "ConnectionString" : "InstrumentationKey=00000000-0000-0000-0000-000000000000;"
    }
  }
```


# <a name="java"></a>[Java](#tab/java)


Java (v2.5.x) (explizit festgelegt):
```java
TelemetryConfiguration.getActive().setConnectionString("InstrumentationKey=00000000-0000-0000-0000-000000000000");
```

ApplicationInsights.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationInsights xmlns="http://schemas.microsoft.com/ApplicationInsights/2013/Settings">
    <ConnectionString>InstrumentationKey=00000000-0000-0000-0000-000000000000;</ConnectionString>
</ApplicationInsights>
```

# <a name="javascript"></a>[JavaScript](#tab/js)

Wichtig: JavaScript unterstützt die Verwendung von Umgebungsvariablen nicht.

Verwenden des Codeausschnitts:

```javascript
<script type="text/javascript">
var sdkInstance="appInsightsSDK";window[sdkInstance]="appInsights";var aiName=window[sdkInstance],aisdk=window[aiName]||function(e){function n(e){t[e]=function(){var n=arguments;t.queue.push(function(){t[e].apply(t,n)})}}var t={config:e};t.initialize=!0;var i=document,a=window;setTimeout(function(){var n=i.createElement("script");n.src=e.url||"https://az416426.vo.msecnd.net/scripts/b/ai.2.min.js",i.getElementsByTagName("script")[0].parentNode.appendChild(n)});try{t.cookie=i.cookie}catch(e){}t.queue=[],t.version=2;for(var r=["Event","PageView","Exception","Trace","DependencyData","Metric","PageViewPerformance"];r.length;)n("track"+r.pop());n("startTrackPage"),n("stopTrackPage");var s="Track"+r[0];if(n("start"+s),n("stop"+s),n("addTelemetryInitializer"),n("setAuthenticatedUserContext"),n("clearAuthenticatedUserContext"),n("flush"),t.SeverityLevel={Verbose:0,Information:1,Warning:2,Error:3,Critical:4},!(!0===e.disableExceptionTracking||e.extensionConfig&&e.extensionConfig.ApplicationInsightsAnalytics&&!0===e.extensionConfig.ApplicationInsightsAnalytics.disableExceptionTracking)){n("_"+(r="onerror"));var o=a[r];a[r]=function(e,n,i,a,s){var c=o&&o(e,n,i,a,s);return!0!==c&&t["_"+r]({message:e,url:n,lineNumber:i,columnNumber:a,error:s}),c},e.autoExceptionInstrumented=!0}return t}(
{
  connectionString:"InstrumentationKey=00000000-0000-0000-0000-000000000000;"
}
);window[aiName]=aisdk,aisdk.queue&&0===aisdk.queue.length&&aisdk.trackPageView({});
</script>
```


Manuelle Einrichtung:
```javascript
import { ApplicationInsights } from '@microsoft/applicationinsights-web'

const appInsights = new ApplicationInsights({ config: {
  connectionString: 'InstrumentationKey=00000000-0000-0000-0000-000000000000;'
  /* ...Other Configuration Options... */
} });
appInsights.loadAppInsights();
appInsights.trackPageView();
```

# <a name="nodejs"></a>[Node.js](#tab/nodejs)

```javascript
const appInsights = require("applicationinsights");
appInsights.setup("InstrumentationKey=00000000-0000-0000-0000-000000000000;");
appInsights.start();
```

# <a name="python"></a>[Python](#tab/python)

Es wird Benutzern empfohlen, die Umgebungsvariable festzulegen.

So legen Sie die Verbindungszeichenfolge explizit fest

```python
from opencensus.ext.azure.trace_exporter import AzureExporter
from opencensus.trace.samplers import ProbabilitySampler
from opencensus.trace.tracer import Tracer

tracer = Tracer(exporter=AzureExporter(connection_string='InstrumentationKey=00000000-0000-0000-0000-000000000000'), sampler=ProbabilitySampler(1.0))
```


## <a name="next-steps"></a>Nächste Schritte

Beginnen mit der Laufzeitmethode mit:

* [Virtueller Azure-Computer und Azure-VM-Skalierungsgruppe – Auf IIS gehostete Apps](./azure-vm-vmss-apps.md)
* [IIS-Server](./monitor-performance-live-website-now.md)
* [Azure-Web-Apps](./azure-web-apps.md)

Beginnen mit der Entwicklungszeitmethode mit:

* [ASP.NET](./asp-net.md)
* [ASP.NET Core](./asp-net-core.md)
* [Java](./java-get-started.md)
* [Node.js](./nodejs.md)
* [Python](./opencensus-python.md)

