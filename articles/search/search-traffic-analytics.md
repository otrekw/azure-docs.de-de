---
title: Bericht zu „Datenverkehrsanalyse durchsuchen“
titleSuffix: Azure Cognitive Search
description: Aktivieren Sie „Datenverkehrsanalyse durchsuchen“ für Azure Cognitive Search, sammeln Sie Telemetriedaten und vom Benutzer initiierte Ereignisse mithilfe von Application Insights, und analysieren Sie die Ergebnisse anschließend in einem Power BI-Bericht.
author: HeidiSteen
manager: nitinme
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 12/11/2019
ms.openlocfilehash: 53fd02856a805f8bb5d7261cc9e6e32861b2b4fd
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2019
ms.locfileid: "75426986"
---
# <a name="implement-search-traffic-analytics-in-azure-cognitive-search"></a>Implementieren von „Datenverkehrsanalyse durchsuchen“ in Azure Cognitive Search

„Datenverkehrsanalyse durchsuchen“ ist ein Muster für das Implementieren einer Feedbackschleife für Ihren Suchdienst. Ziel ist es, Telemetriedaten für vom Benutzer initiierte Klickereignisse und Tastatureingaben zu sammeln. Mithilfe dieser Informationen können Sie die Effektivität ihrer Suchlösung ermitteln, einschließlich beliebter Suchbegriffe, der Klickraten (CTR) und der Abfrageeingaben, die keine Ergebnisse zurückgeben.

Dieses Muster ist abhängig von [Application Insights](https://docs.microsoft.com/azure/azure-monitor/app/app-insights-overview) (einem Feature von [Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/)) zur Sammlung von Benutzerdaten. Sie müssen dem Clientcode auch Instrumentation hinzufügen, wie in diesem Artikel beschrieben. Abschließend benötigen Sie einen Berichtserstellungsmechanismus, um die Daten zu analysieren. Wir empfehlen Power BI, aber Sie können ein beliebiges Tool verwenden, das eine Verbindung mit Application Insights herstellt.

> [!NOTE]
> Das in diesem Artikel beschriebene Muster gilt für erweiterte Szenarien und Clickstreamdaten, die von Ihrem Client generiert werden. Alternativ können Sie einen Bericht der von Ihrem Suchdienst generierten Protokollinformationen erstellen. Weitere Informationen zu Dienstprotokollberichten finden Sie unter [Überwachen von Ressourcenverbrauch und Abfrageaktivität](search-monitor-usage.md).

## <a name="identify-relevant-search-data"></a>Identifizieren von relevanten Suchdaten

Um nützliche Suchmetriken zu erhalten, müssen einige Signale von den Benutzern der Suchanwendung protokolliert werden. Diese Signale kennzeichnen Inhalte, die Benutzer interessieren, und die sie als relevant für ihre Bedürfnisse einschätzen.

„Datenverkehrsanalyse durchsuchen“ benötigt zwei Signale:

+ Vom Benutzer generierte Ereignisse: Nur von einem Benutzer initiierte Suchabfragen sind interessant. Suchabfragen, die dem Auffüllen von Facets, zusätzlichem Inhalt oder internen Informationen dienen, sind nicht wichtig und verzerren und beeinflussen Ihre Ergebnisse.

+ Vom Benutzer generierte Klickereignisse: Anhand der Klicks in diesem Dokument nehmen wir Bezug auf einen Benutzer, der ein bestimmtes Suchergebnis einer Suchabfrage auswählt. Ein Klick bedeutet im Allgemeinen, dass ein Dokument ein relevantes Ergebnis für eine bestimmte Suchabfrage ist.

Durch das Verknüpfen von Suche und Klickereignissen mit einer Korrelations-ID können Sie das Verhalten von Benutzern in Ihrer Anwendung analysieren. Diese Einblicke in die Suche können Sie nicht erhalten, wenn Sie ausschließlich Suchdatenverkehrsprotokolle verwenden.

## <a name="add-search-traffic-analytics"></a>Hinzufügen von „Datenverkehrsanalyse durchsuchen“

Die im vorherigen Abschnitt erwähnten Signale müssen von der Suchanwendung erfasst werden, während der Benutzer mit ihr interagiert. Application Insights ist eine für mehrere Plattformen verfügbare erweiterbare Überwachungslösung mit flexiblen Instrumentierungsoptionen. Mit der Verwendung von Application Insights nutzen Sie die von Azure Cognitive Search erstellten Power BI-Suchberichte zur Vereinfachung der Datenanalyse.

Auf der Seite des [Portals](https://portal.azure.com) für Ihren Dienst von Azure Cognitive Search enthält die Seite „Datenverkehrsanalyse durchsuchen“ einen Spickzettel, anhand dessen Sie diesem Telemetriemuster folgen können. Sie können auch eine Application Insights-Ressource auswählen oder erstellen und alle erforderlichen Daten an einem Ort anzeigen.

![Anweisungen zu „Datenverkehrsanalyse durchsuchen“][1]

## <a name="1---select-a-resource"></a>1 – Auswählen einer Ressource

Sie müssen eine zu verwendende Application Insights-Ressource auswählen oder sie erstellen, wenn Sie noch keine besitzen. Sie können eine Ressource verwenden, die bereits verwendet wird, um die erforderlichen benutzerdefinierten Ereignisse zu protokollieren.

Wenn Sie eine neue Application Insights-Ressource erstellen, sind alle Anwendungstypen für dieses Szenario gültig. Wählen Sie dasjenige, das am besten für die Plattform geeignet ist, die Sie verwenden.

Sie benötigen den Instrumentierungsschlüssel, um den Telemetrieclient für Ihre Anwendung zu erstellen. Sie erhalten ihn über das Dashboard des Application Insights-Portals, oder rufen Sie ihn auf der Seite „Datenverkehrsanalyse durchsuchen“ ab, wozu Sie die Instanz auswählen, die Sie verwenden möchten.

## <a name="2---add-instrumentation"></a>2 – Hinzufügen der Instrumentierung

In diesem Schritt instrumentieren Sie Ihre eigene Suchanwendung mit der Application Insights-Ressource, die Sie im vorherigen Schritt erstellt haben. Dieser Prozess umfasst vier Schritte:

**Schritt 1: Erstellen eines Telemetrieclients**

Dies ist das Objekt, das Ereignisse an die Application Insights-Ressource sendet.

*C#*

    private TelemetryClient telemetryClient = new TelemetryClient();
    telemetryClient.InstrumentationKey = "<YOUR INSTRUMENTATION KEY>";

*JavaScript*

    <script type="text/javascript">var appInsights=window.appInsights||function(config){function r(config){t[config]=function(){var i=arguments;t.queue.push(function(){t[config].apply(t,i)})}}var t={config:config},u=document,e=window,o="script",s=u.createElement(o),i,f;s.src=config.url||"//az416426.vo.msecnd.net/scripts/a/ai.0.js";u.getElementsByTagName(o)[0].parentNode.appendChild(s);try{t.cookie=u.cookie}catch(h){}for(t.queue=[],i=["Event","Exception","Metric","PageView","Trace","Dependency"];i.length;)r("track"+i.pop());return r("setAuthenticatedUserContext"),r("clearAuthenticatedUserContext"),config.disableExceptionTracking||(i="onerror",r("_"+i),f=e[i],e[i]=function(config,r,u,e,o){var s=f&&f(config,r,u,e,o);return s!==!0&&t["_"+i](config,r,u,e,o),s}),t}
    ({
    instrumentationKey: "<YOUR INSTRUMENTATION KEY>"
    });
    window.appInsights=appInsights;
    </script>

Entnehmen Sie andere Sprachen und Plattformen bitte der vollständigen [Liste](https://docs.microsoft.com/azure/application-insights/app-insights-platforms).

**Schritt 2: Anfordern einer Such-ID für die Korrelation**

Um Suchanforderungen mit Klicks zu korrelieren, ist eine Korrelations-ID erforderlich, die diese zwei unterschiedlichen Ereignisse miteinander in Beziehung setzt. Azure Cognitive Search bietet Ihnen eine Such-ID, wenn Sie sie mit einem Header anfordern:

*C#*

    // This sample uses the .NET SDK https://www.nuget.org/packages/Microsoft.Azure.Search

    var client = new SearchIndexClient(<SearchServiceName>, <IndexName>, new SearchCredentials(<QueryKey>)
    var headers = new Dictionary<string, List<string>>() { { "x-ms-azs-return-searchid", new List<string>() { "true" } } };
    var response = await client.Documents.SearchWithHttpMessagesAsync(searchText: searchText, searchParameters: parameters, customHeaders: headers);
    IEnumerable<string> headerValues;
    string searchId = string.Empty;
    if (response.Response.Headers.TryGetValues("x-ms-azs-searchid", out headerValues)){
     searchId = headerValues.FirstOrDefault();
    }

*JavaScript*

    request.setRequestHeader("x-ms-azs-return-searchid", "true");
    request.setRequestHeader("Access-Control-Expose-Headers", "x-ms-azs-searchid");
    var searchId = request.getResponseHeader('x-ms-azs-searchid');

**Schritt 3: Protokollsuchereignisse**

Bei jeder Suchanforderung eines Benutzers sollten Sie dies mit folgendem Schema als Suchereignis in einem benutzerdefinierten Application Insights-Ereignis protokollieren:

**SearchServiceName**: (String) Suchdienstname **SearchId**: (GUID) eindeutiger Bezeichner der Suchabfrage (in der Suchantwort enthalten) **IndexName**: (String) abzufragender Suchdienstindex **QueryTerms**: (String) vom Benutzer eingegebene Suchbegriffe **ResultCount**: (Int) Anzahl der zurückgegebenen Dokumente (in der Suchantwort enthalten) **ScoringProfile**: (String) Name des verwendeten Bewertungsprofils, falls vorhanden

> [!NOTE]
> Fordern Sie die Anzahl vom Benutzer generierter Abfragen an, indem Sie „$count = true“ Ihrer Suchabfrage hinzufügen. Weitere Informationen finden Sie [hier](https://docs.microsoft.com/rest/api/searchservice/search-documents#request).
>

> [!NOTE]
> Denken Sie daran, nur Suchabfragen zu protokollieren, die von Benutzern generiert werden.
>

*C#*

    var properties = new Dictionary <string, string> {
    {"SearchServiceName", <service name>},
    {"SearchId", <search Id>},
    {"IndexName", <index name>},
    {"QueryTerms", <search terms>},
    {"ResultCount", <results count>},
    {"ScoringProfile", <scoring profile used>}
    };
    telemetryClient.TrackEvent("Search", properties);

*JavaScript*

    appInsights.trackEvent("Search", {
    SearchServiceName: <service name>,
    SearchId: <search id>,
    IndexName: <index name>,
    QueryTerms: <search terms>,
    ResultCount: <results count>,
    ScoringProfile: <scoring profile used>
    });

**Schritt 4: Protokollieren von Klickereignissen**

Jedes Mal, wenn ein Benutzer auf ein Dokument klickt, ist dies ein Signal, dass für Suchanalysezwecke protokolliert werden muss. Verwenden Sie benutzerdefinierte Application Insights-Ereignisse, um diese Ereignisse mit dem folgenden Schema zu protokollieren:

**ServiceName**: (String) Suchdienstname **SearchId**: (Guid) eindeutiger Bezeichner der zugehörigen Suchabfrage **DocId**: (String) Dokumentbezeichner **Position**: (Int) Rang des Dokuments auf der Seite der Suchergebnisse

> [!NOTE]
> Die Position bezieht sich auf die Hauptreihenfolge in der Anwendung. Sie können diese Zahl frei festlegen, solange sie immer identisch ist, um den Vergleich zu ermöglichen.
>

*C#*

    var properties = new Dictionary <string, string> {
    {"SearchServiceName", <service name>},
    {"SearchId", <search id>},
    {"ClickedDocId", <clicked document id>},
    {"Rank", <clicked document position>}
    };
    telemetryClient.TrackEvent("Click", properties);

*JavaScript*

    appInsights.trackEvent("Click", {
        SearchServiceName: <service name>,
        SearchId: <search id>,
        ClickedDocId: <clicked document id>,
        Rank: <clicked document position>
    });

## <a name="3---analyze-in-power-bi"></a>3 – Analysieren in Power BI

Nachdem Sie Ihre App instrumentiert und sichergestellt haben, dass die Anwendung ordnungsgemäß mit Application Insights verbunden ist, laden Sie eine vordefinierte Berichtsvorlage herunter, um Daten in Power BI Desktop zu analysieren. Der Bericht enthält vordefinierte Diagramme und Tabellen, die nützlich für die Analyse der zusätzlichen Daten sind, die für die Analyse des Suchdatenverkehrs erfasst wurden. 

1. Klicken Sie im linken Navigationsbereich des Azure Cognitive Search-Dashboards unter **Einstellungen** auf **Datenverkehrsanalyse durchsuchen**.

2. Klicken Sie auf der Seite **Datenverkehrsanalyse durchsuchen** in Schritt 3 auf **Power BI Desktop abrufen**, um Power BI zu installieren.

   ![Abrufen von Power BI-Berichten](./media/search-traffic-analytics/get-use-power-bi.png "Abrufen von Power BI-Berichten")

2. Klicken Sie auf derselben Seite auf **Power BI-Bericht herunterladen**.

3. Der Bericht wird in Power BI Desktop geöffnet, und Sie werden aufgefordert, eine Verbindung mit Application Insights herzustellen und Anmeldeinformationen anzugeben. Sie finden Verbindungsinformationen auf den Seiten des Microsoft Azure-Portals für Ihre Application Insights-Ressource. Geben Sie für Anmeldeinformationen denselben Benutzernamen und dasselbe Kennwort wie für die Anmeldung beim Portal an.

   ![Herstellen einer Verbindung zu Application Insights](./media/search-traffic-analytics/connect-to-app-insights.png "Herstellen einer Verbindung zu Application Insights")

4. Klicken Sie auf **Laden**.

Der Bericht enthält Diagramme und Tabellen, mit denen Sie fundiertere Entscheidungen treffen können, um Leistung und Relevanz der Suche zu verbessern.

Die Metriken umfassen die folgenden Elemente:

* Das Suchvolumen und die beliebtesten Begriff-Dokument-Paare: Begriffe, die dazu führen, dass auf das gleiche Dokument geklickt wird, geordnet nach Klicks.
* Suchvorgänge ohne Klicks: Begriffe für Spitzenabfragen, die keine Klicks registrieren.

Im folgenden Screenshot sind die integrierten Berichte und Diagramme für die Analyse von „Datenverkehrsanalyse durchsuchen“ abgebildet.

![Power BI-Dashboard für Azure Cognitive Search](./media/search-traffic-analytics/azuresearch-powerbi-dashboard.png "Power BI-Dashboard für Azure Cognitive Search")

## <a name="next-steps"></a>Nächste Schritte
Instrumentieren Sie Ihre Suchanwendung, um aussagekräftige Daten über Ihren Suchdienst zu erhalten.

Weitere Informationen finden Sie unter [Application Insights](https://docs.microsoft.com/azure/azure-monitor/app/app-insights-overview). Besuchen Sie auch die [Seite mit der Preisübersicht](https://azure.microsoft.com/pricing/details/application-insights/), um mehr über die verschiedenen Dienstebenen zu erfahren.

Erfahren Sie hier mehr über das Erstellen erstaunlicher Berichte. Weitere Informationen finden Sie unter [Erste Schritte mit Power BI Desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-getting-started/).

<!--Image references-->
[1]: ./media/search-traffic-analytics/azuresearch-trafficanalytics.png
[2]: ./media/search-traffic-analytics/azuresearch-appinsightsdata.png
[3]: ./media/search-traffic-analytics/azuresearch-pbitemplate.png
