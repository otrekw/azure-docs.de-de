---
title: 'Azure Application Insights: Außerkraftsetzen der SDK-Standardendpunkte'
description: Ändern der Standardendpunkte des Azure Monitor Application Insights SDK für Regionen wie Azure Government
ms.topic: conceptual
ms.date: 07/26/2019
ms.custom: references_regions, devx-track-javascript
ms.openlocfilehash: 22f81b327c161df4f6605cfcb64b48b8d75beed3
ms.sourcegitcommit: f353fe5acd9698aa31631f38dd32790d889b4dbb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/29/2020
ms.locfileid: "87373129"
---
# <a name="application-insights-overriding-default-endpoints"></a>Überschreiben der Standardendpunkte von Application Insights

Zum Senden von Daten aus Application Insights in bestimmten Regionen müssen Sie die standardmäßigen Endpunktadressen überschreiben. Jedes SDK erfordert geringfügig andere Änderungen, die in diesem Artikel beschrieben werden. Diese Änderungen erfordern eine Anpassung des Beispielcodes und das Ersetzen der Platzhalterwerte für `QuickPulse_Endpoint_Address`, `TelemetryChannel_Endpoint_Address` und `Profile_Query_Endpoint_address` durch die tatsächlichen Endpunktadressen der jeweiligen Region. Am Ende dieses Artikels finden Sie Links zu den Endpunktadressen für Regionen, in denen diese Konfiguration erforderlich ist.

> [!NOTE]
> [Verbindungszeichenfolgen](./sdk-connection-string.md?tabs=net) sind die neue bevorzugte Methode zum Festlegen von benutzerdefinierten Endpunkten in Application Insights.

---

## <a name="sdk-code-changes"></a>Änderungen am SDK-Code

# <a name="net"></a>[.NET](#tab/net)

> [!NOTE]
> Die Datei „applicationinsights.config“ wird automatisch überschrieben, wenn ein SDK-Upgrade erfolgt. Nachdem ein SDK-Upgrade erfolgt ist, müssen Sie die regionsspezifischen Endpunktwerte erneut eingeben.

```xml
<ApplicationInsights>
  ...
  <TelemetryModules>
    <Add Type="Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector.QuickPulse.QuickPulseTelemetryModule, Microsoft.AI.PerfCounterCollector">
      <QuickPulseServiceEndpoint>Custom_QuickPulse_Endpoint_Address</QuickPulseServiceEndpoint>
    </Add>
  </TelemetryModules>
    ...
  <TelemetryChannel>
     <EndpointAddress>TelemetryChannel_Endpoint_Address</EndpointAddress>
  </TelemetryChannel>
  ...
  <ApplicationIdProvider Type="Microsoft.ApplicationInsights.Extensibility.Implementation.ApplicationId.ApplicationInsightsApplicationIdProvider, Microsoft.ApplicationInsights">
    <ProfileQueryEndpoint>Profile_Query_Endpoint_address</ProfileQueryEndpoint>
  </ApplicationIdProvider>
  ...
</ApplicationInsights>
```

# <a name="net-core"></a>[.NET Core](#tab/netcore)

Ändern Sie die Datei „appsettings.json“ in Ihrem Projekt wie folgt, um den Hauptendpunkt anzupassen:

```json
"ApplicationInsights": {
    "InstrumentationKey": "instrumentationkey",
    "TelemetryChannel": {
      "EndpointAddress": "TelemetryChannel_Endpoint_Address"
    }
  }
```

Die Werte für „Live Metrics“ und „Profile Query Endpoint“ können nur mittels Code festgelegt werden. Um die Standardwerte für alle Endpunktwerte mittels Code zu überschreiben, nehmen Sie in der Datei `Startup.cs` die folgenden Änderungen an der `ConfigureServices`-Methode vor:

```csharp
using Microsoft.ApplicationInsights.Extensibility.Implementation.ApplicationId;
using Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector.QuickPulse; //Place at top of Startup.cs file

   services.ConfigureTelemetryModule<QuickPulseTelemetryModule>((module, o) => module.QuickPulseServiceEndpoint="QuickPulse_Endpoint_Address");

   services.AddSingleton<IApplicationIdProvider, ApplicationInsightsApplicationIdProvider>(_ => new ApplicationInsightsApplicationIdProvider() { ProfileQueryEndpoint = "Profile_Query_Endpoint_address" });

   services.AddSingleton<ITelemetryChannel>(_ => new ServerTelemetryChannel() { EndpointAddress = "TelemetryChannel_Endpoint_Address" });

    //Place in the ConfigureServices method. Place this before services.AddApplicationInsightsTelemetry("instrumentation key"); if it's present
```

# <a name="azure-functions"></a>[Azure-Funktionen](#tab/functions)

Für Azure Functions wird nun empfohlen, [Verbindungszeichenfolgen](./sdk-connection-string.md?tabs=net) zu verwenden, die in den Anwendungseinstellungen der Funktion festgelegt werden. Um im Funktionsbereich auf die Anwendungseinstellungen für Ihre Funktion zuzugreifen, wählen Sie **Einstellungen** > **Konfiguration** > **Anwendungseinstellungen** aus. 

Name: `APPLICATIONINSIGHTS_CONNECTION_STRING` Wert: `Connection String Value`

# <a name="java"></a>[Java](#tab/java)

Ändern Sie in der Datei „applicationinsights.xml“ die Standardendpunktadresse.

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationInsights xmlns="http://schemas.microsoft.com/ApplicationInsights/2013/Settings">
  <InstrumentationKey>ffffeeee-dddd-cccc-bbbb-aaaa99998888</InstrumentationKey>
  <TelemetryModules>
    <Add type="com.microsoft.applicationinsights.web.extensibility.modules.WebRequestTrackingTelemetryModule"/>
    <Add type="com.microsoft.applicationinsights.web.extensibility.modules.WebSessionTrackingTelemetryModule"/>
    <Add type="com.microsoft.applicationinsights.web.extensibility.modules.WebUserTrackingTelemetryModule"/>
  </TelemetryModules>
  <TelemetryInitializers>
    <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebOperationIdTelemetryInitializer"/>
    <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebOperationNameTelemetryInitializer"/>
    <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebSessionTelemetryInitializer"/>
    <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebUserTelemetryInitializer"/>
    <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebUserAgentTelemetryInitializer"/>
  </TelemetryInitializers>
  <!--Add the following Channel value to modify the Endpoint address-->
  <Channel type="com.microsoft.applicationinsights.channel.concrete.inprocess.InProcessTelemetryChannel">
  <EndpointAddress>TelemetryChannel_Endpoint_Address</EndpointAddress>
  </Channel>
</ApplicationInsights>
```

### <a name="spring-boot"></a>Spring Boot

Ändern Sie die Datei `application.properties`, und fügen Sie Folgendes hinzu:

```yaml
azure.application-insights.channel.in-process.endpoint-address= TelemetryChannel_Endpoint_Address
```

# <a name="nodejs"></a>[Node.js](#tab/nodejs)

```javascript
var appInsights = require("applicationinsights");
appInsights.setup('INSTRUMENTATION_KEY');
appInsights.defaultClient.config.endpointUrl = "TelemetryChannel_Endpoint_Address"; // ingestion
appInsights.defaultClient.config.profileQueryEndpoint = "Profile_Query_Endpoint_address"; // appid/profile lookup
appInsights.defaultClient.config.quickPulseHost = "QuickPulse_Endpoint_Address"; //live metrics
appInsights.Configuration.start();
```

Die Endpunkte können auch über Umgebungsvariablen konfiguriert werden:

```
Instrumentation Key: "APPINSIGHTS_INSTRUMENTATIONKEY"
Profile Endpoint: "Profile_Query_Endpoint_address"
Live Metrics Endpoint: "QuickPulse_Endpoint_Address"
```

# <a name="javascript"></a>[JavaScript](#tab/js)

```javascript
<script type="text/javascript">
    var sdkInstance="appInsightsSDK";window[sdkInstance]="appInsights";var aiName=window[sdkInstance],aisdk=window[aiName]||function(e){function n(e){t[e]=function(){var n=arguments;t.queue.push(function(){t[e].apply(t,n)})}}var t={config:e};t.initialize=!0;var i=document,a=window;setTimeout(function(){var n=i.createElement("script");n.src=e.url||"https://az416426.vo.msecnd.net/scripts/b/ai.2.min.js",i.getElementsByTagName("script")[0].parentNode.appendChild(n)});try{t.cookie=i.cookie}catch(e){}t.queue=[],t.version=2;for(var r=["Event","PageView","Exception","Trace","DependencyData","Metric","PageViewPerformance"];r.length;)n("track"+r.pop());n("startTrackPage"),n("stopTrackPage");var s="Track"+r[0];if(n("start"+s),n("stop"+s),n("setAuthenticatedUserContext"),n("clearAuthenticatedUserContext"),n("flush"),!(!0===e.disableExceptionTracking||e.extensionConfig&&e.extensionConfig.ApplicationInsightsAnalytics&&!0===e.extensionConfig.ApplicationInsightsAnalytics.disableExceptionTracking)){n("_"+(r="onerror"));var o=a[r];a[r]=function(e,n,i,a,s){var c=o&&o(e,n,i,a,s);return!0!==c&&t["_"+r]({message:e,url:n,lineNumber:i,columnNumber:a,error:s}),c},e.autoExceptionInstrumented=!0}return t}(
    {
      instrumentationKey:"INSTRUMENTATION_KEY",
      endpointUrl: "TelemetryChannel_Endpoint_Address"
    }
    );window[aiName]=aisdk,aisdk.queue&&0===aisdk.queue.length&&aisdk.trackPageView({});
</script>
```

# <a name="python"></a>[Python](#tab/python)

Anleitungen zum Ändern des Erfassungsendpunkts für das opencensus-python-SDK finden Sie im [opencensus-python-Repository](https://github.com/census-instrumentation/opencensus-python/blob/af284a92b80bcbaf5db53e7e0813f96691b4c696/contrib/opencensus-ext-azure/opencensus/ext/azure/common/__init__.py).

---

## <a name="regions-that-require-endpoint-modification"></a>Regionen, für die Endpunktänderungen erforderlich sind

Derzeit sind [Azure Government](../../azure-government/compare-azure-government-global-azure.md#application-insights) und [Azure China](/azure/china/resources-developer-guide) die einzigen Regionen, für die Endpunktänderungen erforderlich sind.

|Region |  Endpoint Name (Endpunktname) | Wert |
|-----------------|:------------|:-------------|
| Azure China | Telemetriekanal | `https://dc.applicationinsights.azure.cn/v2/track` |
| Azure China | QuickPulse (Live Metrics) |`https://live.applicationinsights.azure.cn/QuickPulseService.svc` |
| Azure China | Profile Query |`https://dc.applicationinsights.azure.cn/api/profiles/{0}/appId`  |
| Azure Government | Telemetriekanal |`https://dc.applicationinsights.us/v2/track` |
| Azure Government | QuickPulse (Live Metrics) |`https://quickpulse.applicationinsights.us/QuickPulseService.svc` |
| Azure Government | Profile Query |`https://dc.applicationinsights.us/api/profiles/{0}/appId` |

Wenn Sie derzeit die [Application Insights-REST-API](https://dev.applicationinsights.io/
) nutzen, auf die normalerweise über „api.applicationinsights.io“ zugegriffen wird, müssen Sie einen lokalen Endpunkt in Ihrer Region verwenden:

|Region |  Endpoint Name (Endpunktname) | Wert |
|-----------------|:------------|:-------------|
| Azure China | REST-API | `api.applicationinsights.azure.cn` |
| Azure Government | REST-API | `api.applicationinsights.us`|

> [!NOTE]
> Eine auf Agents bzw. Erweiterungen basierende Überwachung ohne Code für Azure App Services wird in diesen Regionen **derzeit nicht unterstützt**. Sobald diese Funktionalität verfügbar ist, wird dieser Artikel aktualisiert.

## <a name="next-steps"></a>Nächste Schritte

- Um mehr über die benutzerdefinierten Änderungen für Azure Government zu erfahren, lesen Sie die detaillierten Anleitungen für die [Überwachung und Verwaltung von Azure](../../azure-government/compare-azure-government-global-azure.md#application-insights).
- Weitere Informationen zu Azure China finden Sie im [Playbook zu Azure China](/azure/china/).
