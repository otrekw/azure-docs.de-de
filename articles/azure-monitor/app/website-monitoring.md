---
title: 'Schnellstart: Überwachen von Websites mit Azure Monitor Application Insights'
description: In dieser Schnellstartanleitung erfahren Sie, wie Sie mit Azure Monitor Application Insights eine client-/browserseitige Websiteüberwachung einrichten.
ms.topic: quickstart
ms.date: 03/19/2021
ms.custom: mvc
ms.openlocfilehash: 0e10db39c8dbbf81087d696cfbb5b2ded1ae79ac
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "104654941"
---
# <a name="quickstart-start-monitoring-your-website-with-azure-monitor-application-insights"></a>Schnellstart: Beginnen der Überwachung Ihrer Website mit Azure Monitor Application Insights

In dieser Schnellstartanleitung erfahren Sie, wie Sie das Application Insights JavaScript SDK (Open Source) zu Ihrer Website hinzufügen. Darüber hinaus wird gezeigt, wie Sie Erkenntnisse hinsichtlich der client-/browserseitigen Erfahrung Ihrer Websitebesucher gewinnen.

Mit Azure Monitor Application Insights können Sie ganz einfach die Verfügbarkeit, Leistung und Nutzung Ihrer Website überwachen. Sie können auch Fehler in Ihrer Anwendung schnell erkennen und diagnostizieren, ohne darauf warten zu müssen, dass diese Fehler von Benutzern gemeldet werden. Application Insights bietet Funktionen für die serverseitige Überwachung und Funktionen für die client-/browserseitige Überwachung.

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Konto mit einem aktiven Abonnement. Sie können [kostenlos ein Konto erstellen](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).
* Eine Website, der Sie das Application Insights JavaScript SDK hinzufügen können

## <a name="enable-application-insights"></a>Aktivieren von Application Insights

Application Insights kann Telemetriedaten aus jeder mit dem Internet verbundenen Anwendung erfassen, die lokal oder in der Cloud ausgeführt wird. Führen Sie die folgenden Schritte aus, um diese Daten anzuzeigen:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.
1. Klicken Sie auf **Ressource erstellen** > **Verwaltungstools** > **Application Insights**.

   > [!NOTE]
   >Wenn Sie zum ersten Mal eine Application Insights-Ressource erstellen, finden Sie unter [Erstellen einer Application Insights-Ressource](./create-new-resource.md) weitere Informationen dazu.
1. Wenn das Dialogfeld für die Konfiguration angezeigt wird, füllen Sie die Eingabefelder anhand der Informationen in der folgenden Tabelle aus:

    | Einstellungen        | Wert           | Beschreibung  |
   | ------------- |:-------------|:-----|
   | **Name**      | Global eindeutiger Wert | Der Name, der die zu überwachende App identifiziert |
   | **Ressourcengruppe**     | myResourceGroup      | Der Name der neuen Ressourcengruppe, die Application Insights-Daten hosten soll. Sie können eine neue Ressourcengruppe erstellen oder eine bereits vorhandene Ressourcengruppe verwenden. |
   | **Location** | East US | Wählen Sie einen Standort in Ihrer Nähe oder in der Nähe des Standorts aus, an dem Ihre App gehostet wird. |
1. Klicken Sie auf **Erstellen**.

## <a name="create-an-html-file"></a>Erstellen einer HTML-Datei

1. Erstellen Sie auf Ihrem lokalen Computer eine Datei namens ``hello_world.html``. Erstellen Sie in diesem Beispiel die Datei im Stammverzeichnis von Laufwerk C, sodass das Ergebnis etwa wie folgt aussieht: ``C:\hello_world.html``.
1. Kopieren Sie das folgende Skript, und fügen Sie es in ``hello_world.html`` ein:

    ```html
    <!DOCTYPE html>
    <html>
    <head>
    <title>Azure Monitor Application Insights</title>
    </head>
    <body>
    <h1>Azure Monitor Application Insights Hello World!</h1>
    <p>You can use the Application Insights JavaScript SDK to perform client/browser-side monitoring of your website. To learn about more advanced JavaScript SDK configurations, visit the <a href="https://github.com/Microsoft/ApplicationInsights-JS/blob/master/API-reference.md" title="API Reference">API reference</a>.</p>
    </body>
    </html>
    ```

## <a name="configure-application-insights-sdk"></a>Konfigurieren des Application Insights SDK

1. Wählen Sie **Übersicht** aus, und kopieren Sie die **Verbindungszeichenfolge** Ihrer Anwendung. In diesem Beispiel benötigen wir nur den Instrumentationsschlüsselteil der Verbindungszeichenfolge `InstrumentationKey=00000000-0000-0000-0000-000000000000;`.

    :::image type="content" source="media/website-monitoring/keys.png" alt-text="Screenshot: Übersichtsseite mit dem Instrumentierungsschlüssel und der Verbindungszeichenfolge":::

1. Fügen Sie der Datei ``hello_world.html`` das folgende Skript vor dem schließenden Tag ``</head>`` hinzu:

    ```html
    <script type="text/javascript">
    !function(T,l,y){var S=T.location,k="script",D="instrumentationKey",C="ingestionendpoint",I="disableExceptionTracking",E="ai.device.",b="toLowerCase",w="crossOrigin",N="POST",e="appInsightsSDK",t=y.name||"appInsights";(y.name||T[e])&&(T[e]=t);var n=T[t]||function(d){var g=!1,f=!1,m={initialize:!0,queue:[],sv:"5",version:2,config:d};function v(e,t){var n={},a="Browser";return n[E+"id"]=a[b](),n[E+"type"]=a,n["ai.operation.name"]=S&&S.pathname||"_unknown_",n["ai.internal.sdkVersion"]="javascript:snippet_"+(m.sv||m.version),{time:function(){var e=new Date;function t(e){var t=""+e;return 1===t.length&&(t="0"+t),t}return e.getUTCFullYear()+"-"+t(1+e.getUTCMonth())+"-"+t(e.getUTCDate())+"T"+t(e.getUTCHours())+":"+t(e.getUTCMinutes())+":"+t(e.getUTCSeconds())+"."+((e.getUTCMilliseconds()/1e3).toFixed(3)+"").slice(2,5)+"Z"}(),iKey:e,name:"Microsoft.ApplicationInsights."+e.replace(/-/g,"")+"."+t,sampleRate:100,tags:n,data:{baseData:{ver:2}}}}var h=d.url||y.src;if(h){function a(e){var t,n,a,i,r,o,s,c,u,p,l;g=!0,m.queue=[],f||(f=!0,t=h,s=function(){var e={},t=d.connectionString;if(t)for(var n=t.split(";"),a=0;a<n.length;a++){var i=n[a].split("=");2===i.length&&(e[i[0][b]()]=i[1])}if(!e[C]){var r=e.endpointsuffix,o=r?e.location:null;e[C]="https://"+(o?o+".":"")+"dc."+(r||"services.visualstudio.com")}return e}(),c=s[D]||d[D]||"",u=s[C],p=u?u+"/v2/track":d.endpointUrl,(l=[]).push((n="SDK LOAD Failure: Failed to load Application Insights SDK script (See stack for details)",a=t,i=p,(o=(r=v(c,"Exception")).data).baseType="ExceptionData",o.baseData.exceptions=[{typeName:"SDKLoadFailed",message:n.replace(/\./g,"-"),hasFullStack:!1,stack:n+"\nSnippet failed to load ["+a+"] -- Telemetry is disabled\nHelp Link: https://go.microsoft.com/fwlink/?linkid=2128109\nHost: "+(S&&S.pathname||"_unknown_")+"\nEndpoint: "+i,parsedStack:[]}],r)),l.push(function(e,t,n,a){var i=v(c,"Message"),r=i.data;r.baseType="MessageData";var o=r.baseData;return o.message='AI (Internal): 99 message:"'+("SDK LOAD Failure: Failed to load Application Insights SDK script (See stack for details) ("+n+")").replace(/\"/g,"")+'"',o.properties={endpoint:a},i}(0,0,t,p)),function(e,t){if(JSON){var n=T.fetch;if(n&&!y.useXhr)n(t,{method:N,body:JSON.stringify(e),mode:"cors"});else if(XMLHttpRequest){var a=new XMLHttpRequest;a.open(N,t),a.setRequestHeader("Content-type","application/json"),a.send(JSON.stringify(e))}}}(l,p))}function i(e,t){f||setTimeout(function(){!t&&m.core||a()},500)}var e=function(){var n=l.createElement(k);n.src=h;var e=y[w];return!e&&""!==e||"undefined"==n[w]||(n[w]=e),n.onload=i,n.onerror=a,n.onreadystatechange=function(e,t){"loaded"!==n.readyState&&"complete"!==n.readyState||i(0,t)},n}();y.ld<0?l.getElementsByTagName("head")[0].appendChild(e):setTimeout(function(){l.getElementsByTagName(k)[0].parentNode.appendChild(e)},y.ld||0)}try{m.cookie=l.cookie}catch(p){}function t(e){for(;e.length;)!function(t){m[t]=function(){var e=arguments;g||m.queue.push(function(){m[t].apply(m,e)})}}(e.pop())}var n="track",r="TrackPage",o="TrackEvent";t([n+"Event",n+"PageView",n+"Exception",n+"Trace",n+"DependencyData",n+"Metric",n+"PageViewPerformance","start"+r,"stop"+r,"start"+o,"stop"+o,"addTelemetryInitializer","setAuthenticatedUserContext","clearAuthenticatedUserContext","flush"]),m.SeverityLevel={Verbose:0,Information:1,Warning:2,Error:3,Critical:4};var s=(d.extensionConfig||{}).ApplicationInsightsAnalytics||{};if(!0!==d[I]&&!0!==s[I]){var c="onerror";t(["_"+c]);var u=T[c];T[c]=function(e,t,n,a,i){var r=u&&u(e,t,n,a,i);return!0!==r&&m["_"+c]({message:e,url:t,lineNumber:n,columnNumber:a,error:i}),r},d.autoExceptionInstrumented=!0}return m}(y.cfg);function a(){y.onInit&&y.onInit(n)}(T[t]=n).queue&&0===n.queue.length?(n.queue.push(a),n.trackPageView({})):a()}(window,document,{
    src: "https://js.monitor.azure.com/scripts/b/ai.2.min.js", // The SDK URL Source
    // name: "appInsights", // Global SDK Instance name defaults to "appInsights" when not supplied
    // ld: 0, // Defines the load delay (in ms) before attempting to load the sdk. -1 = block page load and add to head. (default) = 0ms load after timeout,
    // useXhr: 1, // Use XHR instead of fetch to report failures (if available),
    crossOrigin: "anonymous", // When supplied this will add the provided value as the cross origin attribute on the script tag
    // onInit: null, // Once the application insights instance has loaded and initialized this callback function will be called with 1 argument -- the sdk instance (DO NOT ADD anything to the sdk.queue -- As they won't get called)
    cfg: { // Application Insights Configuration
        connectionString:"InstrumentationKey=YOUR_INSTRUMENTATION_KEY_GOES_HERE;" 
        /* ...Other Configuration Options... */
    }});
    </script>
    ```

    > [!NOTE]
    > Der aktuelle Codeausschnitt (oben aufgeführt) ist Version „5“. Die Version ist im Ausschnitt als „sv:"#"“ codiert, und die [aktuelle Version und Konfigurationsdetails sind auf GitHub verfügbar](https://go.microsoft.com/fwlink/?linkid=2156318).

1. Bearbeiten Sie ``hello_world.html``, und fügen Sie Ihren Instrumentierungsschlüssel hinzu.

1. Öffnen Sie ``hello_world.html`` in einer lokalen Browsersitzung. Mit dieser Aktion wird ein einzelner Seitenaufruf generiert. Sie können Ihren Browser aktualisieren, um mehrere Testseitenaufrufe zu generieren.

## <a name="monitor-your-website-in-the-azure-portal"></a>Überwachen Ihrer Website im Azure-Portal

1. Öffnen Sie jetzt im Azure-Portal erneut die Seite **Übersicht** für Application Insights, um Details zu Ihrer aktuell ausgeführten Anwendung anzuzeigen. Auf der Seite **Übersicht** haben Sie den Instrumentierungsschlüssel abgerufen.

   Die vier Standarddiagramme auf der Übersichtsseite sind auf serverseitige Anwendungsdaten ausgelegt. Da Sie die client-/browserseitigen Interaktionen mit dem JavaScript SDK instrumentieren, ist diese spezielle Ansicht nicht relevant (es sei denn, es ist auch ein serverseitiges SDK installiert).

1. Wählen Sie **Protokolle** aus.  Mit dieser Aktion wird **Protokolle** geöffnet. Hier steht eine erweiterte Abfragesprache zum Analysieren aller Daten zur Verfügung, die von Application Insights gesammelt werden. Führen Sie die folgende Abfrage aus, um Daten im Zusammenhang mit den clientseitigen Browseranforderungen anzuzeigen:

    ```kusto
    // average pageView duration by name
    let timeGrain=1s;
    let dataset=pageViews
    // additional filters can be applied here
    | where timestamp > ago(15m)
    | where client_Type == "Browser" ;
    // calculate average pageView duration for all pageViews
    dataset
    | summarize avg(duration) by bin(timestamp, timeGrain)
    | extend pageView='Overall'
    // render result in a chart
    | render timechart
    ```

   :::image type="content" source="media/website-monitoring/log-query.png" alt-text="Screenshot: Protokollanalysediagramm der Benutzeranforderungen in einem bestimmten Zeitraum":::

1. Kehren Sie zur Seite **Übersicht** zurück. Wählen Sie unter der Überschrift **Untersuchen** die Option **Leistung** und anschließend die Registerkarte **Browser** aus. Metriken im Zusammenhang mit der Leistung Ihrer Website werden angezeigt. Es gibt eine Ansicht zum Analysieren von Fehlern und Ausnahmen für Ihre Website. Sie können **Beispiele** auswählen, um auf die [End-to-End-Transaktionsdetails](./transaction-diagnostics.md) zuzugreifen.

     :::image type="content" source="media/website-monitoring/performance.png" alt-text="Screenshot: Registerkarte „Leistung“ mit dem Browsermetrikdiagramm":::

1. Wählen Sie im Hauptmenü von Application Insights unter der Überschrift **Nutzung** die Option [**Benutzer**](./usage-segmentation.md) aus, um die [Analysetools für Benutzerverhalten](./usage-overview.md) zu erkunden. Da wir zum Testen einen einzelnen Computer verwenden, werden nur Daten für einen einzelnen Benutzer angezeigt.

1. Für eine komplexere Website mit mehreren Seiten können Sie das Tool [**Benutzerabläufe**](./usage-flows.md) verwenden, um die Navigation der Besucher auf Ihrer Website nachzuvollziehen.

Informationen zu komplexeren Konfigurationen für die Websiteüberwachung finden Sie in der [API-Referenz für das JavaScript SDK](./javascript.md).

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie planen, mit zusätzlichen Schnellstarts oder Tutorials fortzufahren, sollten Sie die in diesem Schnellstart erstellten Ressourcen nicht bereinigen. Führen Sie andernfalls die folgenden Schritte aus, um alle erstellten Ressourcen dieses Schnellstarts im Azure-Portal zu löschen.

> [!NOTE]
> Wenn Sie eine vorhandene Ressourcengruppe verwendet haben, funktionieren die folgenden Anweisungen nicht. Stattdessen können Sie einfach die einzelne Application Insights-Ressource löschen. Denken Sie daran: Wenn Sie eine Ressourcengruppe löschen, werden auch alle zugrunde liegenden Ressourcen gelöscht, die Mitglieder dieser Gruppe sind.

1. Wählen Sie im Azure-Portal im Menü auf der linken Seite **Ressourcengruppen** und dann **myResourceGroup** oder den Namen Ihrer temporären Ressourcengruppe aus.
1. Wählen Sie auf der Seite mit Ihrer Ressourcengruppe die Option **Löschen** aus, geben Sie in das Textfeld **myResourceGroup** ein, und wählen Sie anschließend **Löschen** aus.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Suchen nach und Diagnostizieren von Leistungsproblemen](../logs/log-query-overview.md)

