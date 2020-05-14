---
title: 'Schnellstart: Überwachen von Websites mit Azure Monitor Application Insights'
description: In dieser Schnellstartanleitung erfahren Sie, wie Sie mit Azure Monitor Application Insights eine client-/browserseitige Websiteüberwachung einrichten.
ms.topic: quickstart
ms.date: 07/15/2019
ms.custom: mvc
ms.openlocfilehash: b47f3ce1ebed12d14dffd68e87dd013bb86218ea
ms.sourcegitcommit: c8a0fbfa74ef7d1fd4d5b2f88521c5b619eb25f8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/05/2020
ms.locfileid: "82801636"
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
   >Wenn Sie zum ersten Mal eine Application Insights-Ressource erstellen, finden Sie unter [Erstellen einer Application Insights-Ressource](https://docs.microsoft.com/azure/azure-monitor/app/create-new-resource) weitere Informationen dazu.
1. Wenn das Dialogfeld für die Konfiguration angezeigt wird, füllen Sie die Eingabefelder anhand der Informationen in der folgenden Tabelle aus:

    | Einstellungen        | Wert           | BESCHREIBUNG  |
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

1. Wählen Sie **Übersicht** > **Grundlagen** aus, und kopieren Sie den **Instrumentierungsschlüssel** Ihrer Anwendung.

   ![Formular „Neue Application Insights-Ressource“](media/website-monitoring/instrumentation-key-001.png)

1. Fügen Sie der Datei ``hello_world.html`` das folgende Skript vor dem schließenden Tag ``</head>`` hinzu:

   ```javascript
   <script type="text/javascript">
      var sdkInstance="appInsightsSDK";window[sdkInstance]="appInsights";var aiName=window[sdkInstance],aisdk=window[aiName]||function(e){function n(e){t[e]=function(){var n=arguments;t.queue.push(function(){t[e].apply(t,n)})}}var t={config:e};t.initialize=!0;var i=document,a=window;setTimeout(function(){var n=i.createElement("script");n.src=e.url||"https://az416426.vo.msecnd.net/scripts/b/ai.2.min.js",i.getElementsByTagName("script")[0].parentNode.appendChild(n)});try{t.cookie=i.cookie}catch(e){}t.queue=[],t.version=2;for(var r=["Event","PageView","Exception","Trace","DependencyData","Metric","PageViewPerformance"];r.length;)n("track"+r.pop());n("startTrackPage"),n("stopTrackPage");var s="Track"+r[0];if(n("start"+s),n("stop"+s),n("setAuthenticatedUserContext"),n("clearAuthenticatedUserContext"),n("flush"),!(!0===e.disableExceptionTracking||e.extensionConfig&&e.extensionConfig.ApplicationInsightsAnalytics&&!0===e.extensionConfig.ApplicationInsightsAnalytics.disableExceptionTracking)){n("_"+(r="onerror"));var o=a[r];a[r]=function(e,n,i,a,s){var c=o&&o(e,n,i,a,s);return!0!==c&&t["_"+r]({message:e,url:n,lineNumber:i,columnNumber:a,error:s}),c},e.autoExceptionInstrumented=!0}return t}(
      {
         instrumentationKey:"INSTRUMENTATION_KEY"
      }
      );window[aiName]=aisdk,aisdk.queue&&0===aisdk.queue.length&&aisdk.trackPageView({});
   </script>
   ```

1. Bearbeiten Sie ``hello_world.html``, und fügen Sie Ihren Instrumentierungsschlüssel hinzu.

1. Öffnen Sie ``hello_world.html`` in einer lokalen Browsersitzung. Mit dieser Aktion wird ein einzelner Seitenaufruf generiert. Sie können Ihren Browser aktualisieren, um mehrere Testseitenaufrufe zu generieren.

## <a name="monitor-your-website-in-the-azure-portal"></a>Überwachen Ihrer Website im Azure-Portal

1. Öffnen Sie jetzt im Azure-Portal erneut die Seite **Übersicht** für Application Insights, um Details zu Ihrer aktuell ausgeführten Anwendung anzuzeigen. Auf der Seite **Übersicht** haben Sie den Instrumentierungsschlüssel abgerufen.

   Die vier Standarddiagramme auf der Übersichtsseite sind auf serverseitige Anwendungsdaten ausgelegt. Da Sie die client-/browserseitigen Interaktionen mit dem JavaScript SDK instrumentieren, ist diese spezielle Ansicht nicht relevant (es sei denn, es ist auch ein serverseitiges SDK installiert).

1. Wählen Sie **Analytics** ![Symbol Anwendungsübersicht“](media/website-monitoring/006.png) aus.  Mit dieser Aktion wird **Analytics** geöffnet. Hier steht eine erweiterte Abfragesprache zum Analysieren aller Daten zur Verfügung, die von Application Insights gesammelt werden. Führen Sie die folgende Abfrage aus, um Daten im Zusammenhang mit den clientseitigen Browseranforderungen anzuzeigen:

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

   ![Analysediagramm der Benutzeranforderungen in einem bestimmten Zeitraum](./media/website-monitoring/analytics-query.png)

1. Kehren Sie zur Seite **Übersicht** zurück. Wählen Sie unter der Überschrift **Untersuchen** die Option **Browser** und anschließend **Leistung** aus.  Metriken im Zusammenhang mit der Leistung Ihrer Website werden angezeigt. Es gibt eine Ansicht zum Analysieren von Fehlern und Ausnahmen für Ihre Website. Sie können **Beispiele** auswählen, um auf die [End-to-End-Transaktionsdetails](../../azure-monitor/app/transaction-diagnostics.md) zuzugreifen.

   ![Servermetriken – Diagramm](./media/website-monitoring/browser-performance.png)

1. Wählen Sie im Hauptmenü von Application Insights unter der Überschrift **Nutzung** die Option [**Benutzer**](../../azure-monitor/app/usage-segmentation.md) aus, um die [Analysetools für Benutzerverhalten](../../azure-monitor/app/usage-overview.md) zu erkunden. Da wir zum Testen einen einzelnen Computer verwenden, werden nur Daten für einen einzelnen Benutzer angezeigt. Bei einer Livewebsite kann die Benutzerverteilung beispielsweise wie folgt aussehen:

     ![Benutzerdiagramm](./media/website-monitoring/usage-users.png)

1. Für eine komplexere Website mit mehreren Seiten können Sie das Tool [**Benutzerabläufe**](../../azure-monitor/app/usage-flows.md) verwenden, um die Navigation der Besucher auf Ihrer Website nachzuvollziehen.

   ![Visualisierung von Benutzerabläufen](./media/website-monitoring/user-flows.png)

Informationen zu komplexeren Konfigurationen für die Websiteüberwachung finden Sie in der [API-Referenz für das JavaScript SDK](https://github.com/Microsoft/ApplicationInsights-JS/blob/master/API-reference.md).

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie planen, mit zusätzlichen Schnellstarts oder Tutorials fortzufahren, sollten Sie die in diesem Schnellstart erstellten Ressourcen nicht bereinigen. Führen Sie andernfalls die folgenden Schritte aus, um alle erstellten Ressourcen dieses Schnellstarts im Azure-Portal zu löschen.

> [!NOTE]
> Wenn Sie eine vorhandene Ressourcengruppe verwendet haben, funktionieren die folgenden Anweisungen nicht. Stattdessen können Sie einfach die einzelne Application Insights-Ressource löschen. Denken Sie daran: Wenn Sie eine Ressourcengruppe löschen, werden auch alle zugrunde liegenden Ressourcen gelöscht, die Mitglieder dieser Gruppe sind.

1. Wählen Sie im Azure-Portal im Menü auf der linken Seite **Ressourcengruppen** und dann **myResourceGroup** oder den Namen Ihrer temporären Ressourcengruppe aus.
1. Wählen Sie auf der Seite mit Ihrer Ressourcengruppe die Option **Löschen** aus, geben Sie in das Textfeld **myResourceGroup** ein, und wählen Sie anschließend **Löschen** aus.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Suchen nach und Diagnostizieren von Leistungsproblemen](https://docs.microsoft.com/azure/application-insights/app-insights-analytics)
