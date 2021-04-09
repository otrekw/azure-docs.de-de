---
title: C#-Tutorial zur Erstellung Ihrer ersten App
titleSuffix: Azure Cognitive Search
description: Es wird Schritt für Schritt beschrieben, wie Sie Ihre erste C#-Such-App erstellen. Das Tutorial enthält sowohl einen Downloadlink zu einer funktionierenden App auf GitHub als auch den vollständigen Prozess für die Neuerstellung der App.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: tutorial
ms.date: 02/26/2021
ms.custom: devx-track-csharp
ms.openlocfilehash: 0a57e45b264badffd0305eb6ac5b3c8f7c42adf3
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "101695123"
---
# <a name="tutorial-create-your-first-search-app-using-the-net-sdk"></a>Tutorial: Erstellen Ihrer ersten Such-App mit dem .NET SDK

In diesem Tutorial erfahren Sie, wie Sie eine Web-App erstellen, die Ergebnisse aus einem Suchindex mithilfe von Azure Cognitive Search und Visual Studio abfragt und zurückgibt.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Einrichten einer Entwicklungsumgebung
> * Modellieren von Datenstrukturen
> * Erstellen einer Webseite zum Erfassen von Abfrageeingaben und Anzeigen von Ergebnissen
> * Definieren einer Suchmethode
> * Testen der App

Außerdem wird beschrieben, wie einfach die Durchführung eines Suchaufrufs ist. Die zu entwickelnden wichtigen Anweisungen für den Code sind in den folgenden wenigen Zeilen enthalten.

```csharp
var options = new SearchOptions()
{
    // The Select option specifies fields for the result set
    options.Select.Add("HotelName");
    options.Select.Add("Description");
};

var searchResult = await _searchClient.SearchAsync<Hotel>(model.searchText, options).ConfigureAwait(false);
model.resultList = searchResult.Value.GetResults().ToList();
```

Mit einem einzigen Aufruf wird der Index abgefragt, und die Ergebnisse werden zurückgegeben.

:::image type="content" source="media/tutorial-csharp-create-first-app/azure-search-pool.png" alt-text="Suchen nach *pool*" border="true":::

## <a name="overview"></a>Übersicht

In diesem Tutorial wird „hotels-sample-index“ verwendet. Diesen Index können Sie in Ihrem eigenen Suchdienst ganz schnell selbst erstellen. Eine entsprechende Anleitung finden Sie in der [Schnellstartanleitung zum Importieren von Daten](search-get-started-portal.md). Der Index enthält fiktive Hoteldaten, die in jedem Suchdienst als integrierte Datenquelle verfügbar sind.

In der ersten Lektion dieses Tutorials werden eine grundlegende Abfragestruktur und eine Suchseite erstellt, die dann in den weiteren Lektionen mit Paginierung, Facetten und Vorschlagssuche versehen wird.

Eine fertige Version des Codes finden Sie im folgenden Projekt:

* [1-basic-search-page (GitHub)](https://github.com/Azure-Samples/azure-search-dotnet-samples/tree/master/create-first-app/v11/1-basic-search-page)

Dieses Tutorial wurde aktualisiert, sodass es das Paket „Azure.Search.Documents“ (Version 11) verwendet. Eine frühere Version des .NET SDK finden Sie im [Codebeispiel Microsoft.Azure.Search (Version 10)](https://github.com/Azure-Samples/azure-search-dotnet-samples/tree/master/create-first-app/v10).

## <a name="prerequisites"></a>Voraussetzungen

* [Neuer](search-create-service-portal.md) oder [bereits vorhandener](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) Suchdienst

* Erstellen Sie den Index „hotels-sample-index“ gemäß der Anleitung unter [Schnellstart: Erstellen eines Index für Azure Cognitive Search im Azure-Portal](search-get-started-portal.md).

* [Visual Studio](https://visualstudio.microsoft.com/)

* [Azure Cognitive Search-Clientbibliothek (Version 11)](https://www.nuget.org/packages/Azure.Search.Documents/)

### <a name="install-and-run-the-project-from-github"></a>Installieren und Ausführen des Projekts von GitHub

Wenn Sie gleich mit einer funktionierenden App arbeiten möchten, führen Sie die Schritte unten und dann den vollständigen Code aus.

1. Beispiel auf GitHub: [Erstellen der ersten App](https://github.com/Azure-Samples/azure-search-dotnet-samples/tree/master/create-first-app/v11).

1. Wählen Sie im [Stammordner](https://github.com/Azure-Samples/azure-search-dotnet-samples) die Option **Code** und dann **Clone** (Klonen) oder **Download ZIP** (ZIP herunterladen) aus, um eine private lokale Kopie des Projekts zu erstellen.

1. Navigieren Sie in Visual Studio zur Projektmappe mit der einfachen Suchseite („1-basic-search-page“), und öffnen Sie sie. Wählen Sie anschließend die Option **Ohne Debuggen starten** (oder drücken Sie F5), um das Programm zu erstellen und auszuführen.

1. Dabei handelt es sich um einen Hotelindex. Geben Sie also einige Wörter ein, die Sie bei einer Suche nach Hotels verwenden würden (z. B. „WiFi“, „View“, „Bar“, „Parking“), und überprüfen Sie die Ergebnisse.

    :::image type="content" source="media/tutorial-csharp-create-first-app/azure-search-wifi.png" alt-text="Suchen nach *wifi*" border="true":::

Dieses Projekt und Ihre Web-App werden hoffentlich reibungslos ausgeführt. Viele der grundlegenden Komponenten für komplexere Suchen sind in dieser einen App enthalten. Es ist also hilfreich, die App durchzugehen und Schritt für Schritt neu zu erstellen. Diese Schritte werden in den folgenden Abschnitten beschrieben.

## <a name="set-up-a-development-environment"></a>Einrichten einer Entwicklungsumgebung

Beginnen Sie mit einem Visual Studio-Projekt, um dieses Projekt von Grund auf neu zu erstellen und so die Komponenten von Azure Cognitive Search noch besser kennenzulernen.

1. Wählen Sie in Visual Studio **Neu** > **Projekt** und dann **ASP.NET Core-Webanwendung**.

    :::image type="content" source="media/tutorial-csharp-create-first-app/azure-search-project1.png" alt-text="Erstellen eines Cloudprojekts" border="true":::

1. Benennen Sie das Projekt, z. B. „FirstSearchApp“, und legen Sie den Speicherort fest. Klicken Sie auf **Erstellen**.

1. Wählen Sie die Projektvorlage **Webanwendung (Model-View-Controller)** aus.

    :::image type="content" source="media/tutorial-csharp-create-first-app/azure-search-project2.png" alt-text="Erstellen eines MVC-Projekts" border="true":::

1. Installieren der Clientbibliothek Wählen Sie **Tools** > **NuGet-Paket-Manager** > **NuGet-Pakete für Lösung verwalten...** und dann **Durchsuchen** aus. Suchen Sie anschließend nach „azure.search.documents“. Installieren Sie **Azure.Search.Documents** (Version 11 oder höher), und akzeptieren Sie die Lizenzverträge und die Abhängigkeiten.

    :::image type="content" source="media/tutorial-csharp-create-first-app/azure-search-nuget-azure.png" alt-text="Hinzufügen von Azure-Bibliotheken per NuGet" border="true":::

### <a name="initialize-azure-cognitive-search"></a>Initialisieren von Azure Cognitive Search

Für dieses Beispiel verwenden Sie öffentlich verfügbare Hoteldaten. Diese Daten umfassen eine willkürliche Sammlung mit 50 fiktiven Hotelnamen und -beschreibungen, die nur den Zweck der Bereitstellung von Demodaten erfüllen. Geben Sie einen Namen und einen API-Schlüssel an, um auf diese Daten zuzugreifen.

1. Öffnen Sie **appsettings.json**, und ersetzen Sie die Standardzeilen durch die Suchdienst-URL (im Format `https://<service-name>.search.windows.net`) sowie durch einen [Administrator- oder Abfrage-API-Schlüssel](search-security-api-keys.md) Ihres Suchdiensts. Da Sie keinen Index erstellen oder aktualisieren müssen, können Sie für dieses Tutorial den Abfrageschlüssel verwenden.

    ```csharp
    {
        "SearchServiceName": "<YOUR-SEARCH-SERVICE-URI>",
        "SearchServiceQueryApiKey": "<YOUR-SEARCH-SERVICE-API-KEY>"
    }
    ```

1. Wählen Sie im Projektmappen-Explorer die Datei aus, und ändern Sie in „Eigenschaften“ die Einstellung **In Ausgabeverzeichnis kopieren** in **Kopieren, wenn neuer**.

    :::image type="content" source="media/tutorial-csharp-create-first-app/azure-search-copy-if-newer.png" alt-text="Kopieren der App-Einstellungen in die Ausgabe" border="true":::

## <a name="model-data-structures"></a>Modellieren von Datenstrukturen

Modelle (C#-Klassen) werden verwendet, um für Daten die Kommunikation zwischen dem Client (View), dem Server (Controller) und auch der Azure-Cloud über die MVC-Architektur (Model, View, Controller) zu ermöglichen. Normalerweise spiegeln diese Modelle die Struktur der Daten wider, auf die zugegriffen wird.

In diesem Schritt erstellen Sie ein Modell der Datenstrukturen des Suchindexes und die Suchzeichenfolgen, die in der View-Controller-Kommunikation verwendet wird. Im Index „Hotels“ hat jedes Hotel viele Zimmer, und jedes Hotel verfügt über eine mehrteilige Adresse. Insgesamt wird ein Hotel durch eine hierarchische und verschachtelte Datenstruktur dargestellt. Sie benötigen drei Klassen, um die einzelnen Komponenten zu erstellen.

Die Klassen **Hotel**, **Address**, und **Room** werden als [*komplexe Typen*](search-howto-complex-data-types.md) bezeichnet. Dies ist ein wichtiges Feature von Azure Cognitive Search. Komplexe Typen können über viele Ebenen mit Klassen und Unterklassen verfügen und ermöglichen die Darstellung von deutlich komplexeren Datenstrukturen, als wenn *einfache Typen* (Klasse, die nur primitive Member enthält) verwendet werden.

1. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf **Modelle** > **Hinzufügen** > **Neues Element**.

1. Wählen Sie **Klasse**, und benennen Sie das Element „Hotel.cs“. Ersetzen Sie den gesamten Inhalt von „Hotel.cs“ durch den folgenden Code. Beachten Sie die Member **Address** und **Room** der Klasse. Da es sich bei diesen Feldern selbst um Klassen handelt, benötigen Sie auch hierfür Modelle.

    ```csharp
    using Azure.Search.Documents.Indexes;
    using Azure.Search.Documents.Indexes.Models;
    using Microsoft.Spatial;
    using System;
    using System.Text.Json.Serialization;

    namespace FirstAzureSearchApp.Models
    {
        public partial class Hotel
        {
            [SimpleField(IsFilterable = true, IsKey = true)]
            public string HotelId { get; set; }

            [SearchableField(IsSortable = true)]
            public string HotelName { get; set; }

            [SearchableField(AnalyzerName = LexicalAnalyzerName.Values.EnLucene)]
            public string Description { get; set; }

            [SearchableField(AnalyzerName = LexicalAnalyzerName.Values.FrLucene)]
            [JsonPropertyName("Description_fr")]
            public string DescriptionFr { get; set; }

            [SearchableField(IsFilterable = true, IsSortable = true, IsFacetable = true)]
            public string Category { get; set; }

            [SearchableField(IsFilterable = true, IsFacetable = true)]
            public string[] Tags { get; set; }

            [SimpleField(IsFilterable = true, IsSortable = true, IsFacetable = true)]
            public bool? ParkingIncluded { get; set; }

            [SimpleField(IsFilterable = true, IsSortable = true, IsFacetable = true)]
            public DateTimeOffset? LastRenovationDate { get; set; }

            [SimpleField(IsFilterable = true, IsSortable = true, IsFacetable = true)]
            public double? Rating { get; set; }

            public Address Address { get; set; }

            [SimpleField(IsFilterable = true, IsSortable = true)]
            public GeographyPoint Location { get; set; }

            public Room[] Rooms { get; set; }
        }
    }
    ```

1. Wiederholen Sie die gleichen Schritte zum Erstellen eines Modells für die **Address**-Klasse, und geben Sie der Datei den Namen „Address.cs“. Ersetzen Sie den Inhalt durch den folgenden Code.

    ```csharp
    using Azure.Search.Documents.Indexes;

    namespace FirstAzureSearchApp.Models
    {
        public partial class Address
        {
            [SearchableField]
            public string StreetAddress { get; set; }

            [SearchableField(IsFilterable = true, IsSortable = true, IsFacetable = true)]
            public string City { get; set; }

            [SearchableField(IsFilterable = true, IsSortable = true, IsFacetable = true)]
            public string StateProvince { get; set; }

            [SearchableField(IsFilterable = true, IsSortable = true, IsFacetable = true)]
            public string PostalCode { get; set; }

            [SearchableField(IsFilterable = true, IsSortable = true, IsFacetable = true)]
            public string Country { get; set; }
        }
    }
    ```

1. Führen Sie den Prozess noch einmal durch, um die **Room**-Klasse zu erstellen, und geben Sie der Datei den Namen „Room.cs“.

    ```csharp
    using Azure.Search.Documents.Indexes;
    using Azure.Search.Documents.Indexes.Models;
    using System.Text.Json.Serialization;

    namespace FirstAzureSearchApp.Models
    {
        public partial class Room
        {
            [SearchableField(AnalyzerName = LexicalAnalyzerName.Values.EnMicrosoft)]
            public string Description { get; set; }

            [SearchableField(AnalyzerName = LexicalAnalyzerName.Values.FrMicrosoft)]
            [JsonPropertyName("Description_fr")]
            public string DescriptionFr { get; set; }

            [SearchableField(IsFilterable = true, IsFacetable = true)]
            public string Type { get; set; }

            [SimpleField(IsFilterable = true, IsFacetable = true)]
            public double? BaseRate { get; set; }

            [SearchableField(IsFilterable = true, IsFacetable = true)]
            public string BedOptions { get; set; }

            [SimpleField(IsFilterable = true, IsFacetable = true)]
            public int SleepsCount { get; set; }

            [SimpleField(IsFilterable = true, IsFacetable = true)]
            public bool? SmokingAllowed { get; set; }

            [SearchableField(IsFilterable = true, IsFacetable = true)]
            public string[] Tags { get; set; }
        }
    }
    ```

1. Das letzte Modell, das Sie in diesem Tutorial erstellen, ist die Klasse mit dem Namen **SearchData**. Diese stellt die Benutzereingabe (**searchText**) und die Ausgabe der Suche (**resultList**) dar. Der Typ der Ausgabe **SearchResults&lt;Hotel&gt;** ist entscheidend. Dieser Typ stimmt nämlich genau mit den Ergebnissen der Suche überein, und diesen Verweis müssen Sie an die Ansicht (View) übergeben. Ersetzen Sie die Standardvorlage durch den folgenden Code.

    ```csharp
    using Azure.Search.Documents.Models;

    namespace FirstAzureSearchApp.Models
    {
        public class SearchData
        {
            // The text to search for.
            public string searchText { get; set; }

            // The list of results.
            public SearchResults<Hotel> resultList;
        }
    }
    ```

## <a name="create-a-web-page"></a>Erstellen einer Webseite

Projektvorlagen verfügen über eine Reihe von Clientansichten, die sich im Ordner **Ansichten** befinden. Die Ansichten richten sich jeweils nach der Version von Core .NET, die Sie verwenden (in diesem Beispiel 3.1). In diesem Tutorial ändern Sie **Index.cshtml** so, dass die Elemente einer Suchseite enthalten sind.

Löschen Sie den gesamten Inhalt von „Index.cshtml“, und erstellen Sie die Datei in den folgenden Schritten neu.

1. Das Tutorial verwendet zwei kleine Bilder in der Ansicht: ein Azure-Logo und ein Lupensymbol („azure-logo.png“ und „search.png“). Kopieren Sie die Bilder aus dem GitHub-Projekt in den Ordner **wwwroot/images** in Ihrem Projekt.

1. In der ersten Zeile der Datei „Index.cshtml“ sollte auf das Modell verwiesen werden, das für die Kommunikation der Daten zwischen dem Client (View) und dem Server (Controller) verwendet wird. Dies ist das zuvor erstellte Modell **SearchData**. Fügen Sie diese Zeile der Datei „Index.cshtml“ hinzu.

    ```csharp
    @model FirstAzureSearchApp.Models.SearchData
    ```

1. Die Standardmethode besteht darin, einen Titel für die Ansicht einzugeben. Die nächsten Zeilen sollten also wie folgt lauten:

    ```csharp
    @{
        ViewData["Title"] = "Home Page";
    }
    ```

1. Geben Sie nach dem Titel einen Verweis auf ein HTML-Stylesheet ein, das Sie in Kürze erstellen.

    ```csharp
    <head>
        <link rel="stylesheet" href="~/css/hotels.css" />
    </head>
    ```

1. Der Hauptteil der Ansicht behandelt zwei Anwendungsfälle. Zuerst muss bei der ersten Verwendung eine leere Seite bereitgestellt werden, bevor Suchtext eingegeben wird. Zweitens müssen zusätzlich zum Textfeld für die Suche die Ergebnisse verarbeitet werden, damit diese vom Benutzer mehrfach genutzt werden können.

   Zum Behandeln dieser beiden Fälle müssen Sie überprüfen, ob das für die Ansicht bereitgestellte Modell NULL ist. Ein NULL-Modell gibt den ersten Anwendungsfall (die erste Ausführung der App) an. Fügen Sie der Datei „Index.cshtml“ Folgendes hinzu, und lesen Sie sich die Kommentare durch.

    ```csharp
    <body>
    <h1 class="sampleTitle">
        <img src="~/images/azure-logo.png" width="80" />
        Hotels Search
    </h1>

    @using (Html.BeginForm("Index", "Home", FormMethod.Post))
    {
        // Display the search text box, with the search icon to the right of it.
        <div class="searchBoxForm">
            @Html.TextBoxFor(m => m.searchText, new { @class = "searchBox" }) <input class="searchBoxSubmit" type="submit" value="">
        </div>

        @if (Model != null)
        {
            // Show the result count.
            <p class="sampleText">
                @Model.resultList.Count Results
            </p>

            @for (var i = 0; i < Model.resultList.Count; i++)
            {
                // Display the hotel name and description.
                @Html.TextAreaFor(m => m.resultList[i].Document.HotelName, new { @class = "box1" })
                @Html.TextArea($"desc{i}", Model.resultList[i].Document.Description, new { @class = "box2" })
            }
        }
    }
    </body>
    ```

1. Fügen Sie das Stylesheet hinzu. Wählen Sie in Visual Studio **Datei**> **Neu** > **Datei** und dann **Stylesheet** aus (wobei **Allgemein** ausgewählt ist).

   Ersetzen Sie den Standardcode durch Folgendes. Wir gehen auf diese Datei nicht näher ein. Bei den Formaten handelt es sich um HTML-Standardformate.

    ```html
    textarea.box1 {
        width: 648px;
        height: 30px;
        border: none;
        background-color: azure;
        font-size: 14pt;
        color: blue;
        padding-left: 5px;
    }

    textarea.box2 {
        width: 648px;
        height: 100px;
        border: none;
        background-color: azure;
        font-size: 12pt;
        padding-left: 5px;
        margin-bottom: 24px;
    }

    .sampleTitle {
        font: 32px/normal 'Segoe UI Light',Arial,Helvetica,Sans-Serif;
        margin: 20px 0;
        font-size: 32px;
        text-align: left;
    }

    .sampleText {
        font: 16px/bold 'Segoe UI Light',Arial,Helvetica,Sans-Serif;
        margin: 20px 0;
        font-size: 14px;
        text-align: left;
        height: 30px;
    }

    .searchBoxForm {
        width: 648px;
        box-shadow: 0 0 0 1px rgba(0,0,0,.1), 0 2px 4px 0 rgba(0,0,0,.16);
        background-color: #fff;
        display: inline-block;
        border-collapse: collapse;
        border-spacing: 0;
        list-style: none;
        color: #666;
    }

    .searchBox {
        width: 568px;
        font-size: 16px;
        margin: 5px 0 1px 20px;
        padding: 0 10px 0 0;
        border: 0;
        max-height: 30px;
        outline: none;
        box-sizing: content-box;
        height: 35px;
        vertical-align: top;
    }

    .searchBoxSubmit {
        background-color: #fff;
        border-color: #fff;
        background-image: url(/images/search.png);
        background-repeat: no-repeat;
        height: 20px;
        width: 20px;
        text-indent: -99em;
        border-width: 0;
        border-style: solid;
        margin: 10px;
        outline: 0;
    }
    ```

1. Speichern Sie die Stylesheetdatei als „hotels.css“ im Ordner **wwwroot/css** neben der Standarddatei „site.css“.

Unsere Ansicht ist nun fertig. An diesem Punkt sind die Modelle und die Ansichten vollständig. Jetzt muss nur noch der Controller erstellt werden, um alles zu verbinden.

## <a name="define-methods"></a>Methoden definieren

In diesem Schritt ändern Sie den Inhalt von **Home Controller**.

1. Öffnen Sie die Datei „HomeController.cs“, und ersetzen Sie die **using**-Anweisungen durch Folgendes.

    ```csharp
    using Azure;
    using Azure.Search.Documents;
    using Azure.Search.Documents.Indexes;
    using FirstAzureSearchApp.Models;
    using Microsoft.AspNetCore.Mvc;
    using Microsoft.Extensions.Configuration;
    using System;
    using System.Diagnostics;
    using System.Linq;
    using System.Threading.Tasks;
    ```

### <a name="add-index-methods"></a>Hinzufügen von Index-Methoden

In einer MVC-App ist die **Index()** -Methode eine Methode für die Standardaktion eines Controllers. Sie öffnet die HTML-Indexseite. Die Standardmethode, die keine Parameter akzeptiert, wird in diesem Tutorial für den Anwendungsfall des Anwendungsstarts verwendet, d. h. dem Rendern einer leeren Suchseite.

In diesem Abschnitt erweitern wir die-Methode, um einen zweiten Anwendungsfall zu unterstützen: das Rendern der Seite, wenn ein Benutzer den Suchtext eingegeben hat. Um diesen Fall zu unterstützen, wird die index-Methode so erweitert, dass sie ein Modell als Parameter akzeptiert.

1. Fügen Sie die folgende Methode nach der **Index()** -Standardmethode hinzu.

    ```csharp
        [HttpPost]
        public async Task<ActionResult> Index(SearchData model)
        {
            try
            {
                // Ensure the search string is valid.
                if (model.searchText == null)
                {
                    model.searchText = "";
                }

                // Make the Azure Cognitive Search call.
                await RunQueryAsync(model);
            }

            catch
            {
                return View("Error", new ErrorViewModel { RequestId = "1" });
            }
            return View(model);
        }
    ```

    Achten Sie auf die **async**-Deklaration der Methode und den **await**-Aufruf von **RunQueryAsync**. Diese Schlüsselwörter sorgen dafür, dass Aufrufe asynchron erfolgen, sodass Threads auf dem Server nicht blockieren.

    Für den **catch**-Block wird das Fehlermodell verwendet, das standardmäßig erstellt wurde.

### <a name="note-the-error-handling-and-other-default-views-and-methods"></a>Fehlerbehandlung und andere Standardansichten und -methoden

Je nach verwendeter .NET Core-Version wird standardmäßig jeweils eine andere Gruppe von Standardansichten erstellt. Bei .NET Core 3.1 lauten die Standardansichten beispielsweise „Index“, „Privacy“ und „Error“. Sie können diese Standardseiten beim Ausführen der App anzeigen und dann untersuchen, wie sie im Controller verarbeitet werden.

Sie testen die Ansicht „Error“ später in diesem Tutorial.

Im GitHub-Beispiel werden die nicht verwendeten Ansichten und die zugehörigen Aktionen gelöscht.

### <a name="add-the-runqueryasync-method"></a>Hinzufügen der RunQueryAsync-Methode

Der Azure Cognitive Search-Aufruf ist in unserer **RunQueryAsync**-Methode gekapselt.

1. Fügen Sie zuerst einige statische Variablen zum Einrichten des Azure-Diensts und einen Aufruf für deren Initialisierung hinzu.

    ```csharp
        private static SearchClient _searchClient;
        private static SearchIndexClient _indexClient;
        private static IConfigurationBuilder _builder;
        private static IConfigurationRoot _configuration;

        private void InitSearch()
        {
            // Create a configuration using appsettings.json
            _builder = new ConfigurationBuilder().AddJsonFile("appsettings.json");
            _configuration = _builder.Build();

            // Read the values from appsettings.json
            string searchServiceUri = _configuration["SearchServiceUri"];
            string queryApiKey = _configuration["SearchServiceQueryApiKey"];

            // Create a service and index client.
            _indexClient = new SearchIndexClient(new Uri(searchServiceUri), new AzureKeyCredential(queryApiKey));
            _searchClient = _indexClient.GetSearchClient("hotels");
        }
    ```

2. Fügen Sie nun die eigentliche **RunQueryAsync**-Methode hinzu.

    ```csharp
    private async Task<ActionResult> RunQueryAsync(SearchData model)
    {
        InitSearch();

        var options = new SearchOptions() { };

        // Enter Hotel property names into this list so only these values will be returned.
        // If Select is empty, all values will be returned, which can be inefficient.
        options.Select.Add("HotelName");
        options.Select.Add("Description");

        // For efficiency, the search call should be asynchronous, so use SearchAsync rather than Search.
        var searchResult = await _searchClient.SearchAsync<Hotel>(model.searchText, options).ConfigureAwait(false);
        model.resultList = searchResult.Value.GetResults().ToList();

        // Display the results.
        return View("Index", model);
    }
    ```

    Bei dieser Methode stellen Sie zuerst sicher, dass unsere Azure-Konfiguration initiiert wurde, und legen Sie dann einige Suchoptionen fest. Die Option **Select** gibt an, welche Felder in den Ergebnissen zurückgegeben werden, und entspricht daher den Eigenschaftsnamen in der **hotel**-Klasse. Wenn Sie **Select** weglassen, werden alle nicht ausgeblendeten Felder zurückgegeben. Dies ist nicht effizient, wenn nur ein Teil der verfügbaren Felder relevant ist.

    Der asynchrone Suchvorgang formuliert die Anforderung (modelliert als **searchText**) und die Antwort (modelliert als **searchResult**). Wenn Sie diesen Code debuggen, ist die **SearchResult**-Klasse eine gute Wahl für das Festlegen eines Breakpoints, um den Inhalt von **model.resultList** zu untersuchen. Dies ist ein intuitiver Vorgang, bei dem Sie außer den gewünschten Daten nicht noch viele weitere Informationen zusätzlich erhalten.

### <a name="test-the-app"></a>Testen der App

Überprüfen wir nun, ob die App richtig ausgeführt wird.

1. Wählen Sie **Debuggen** > **Ohne Debuggen starten** aus, oder drücken Sie **F5**. Wenn die App erwartungsgemäß ausgeführt wird, sollten Sie die Ausgangsindexansicht erhalten.

     :::image type="content" source="media/tutorial-csharp-create-first-app/azure-search-index.png" alt-text="Öffnen der App" border="true":::

1. Geben Sie eine Abfragezeichenfolge ein, z. B. „beach“ (oder anderen Text, der Ihnen gerade einfällt), und klicken Sie auf das Suchsymbol, um die Anforderung zu senden.

     :::image type="content" source="media/tutorial-csharp-create-first-app/azure-search-beach.png" alt-text="Suchen nach *beach*" border="true":::

1. Versuchen Sie, „five star“ einzugeben. Beachten Sie, dass diese Abfrage keine Ergebnisse zurückgibt. Bei einer anspruchsvolleren Suche würde „five star“ als Synonym für „luxury“ erkannt werden, und die entsprechenden Ergebnisse würden angezeigt werden. Unterstützung für [Synonyme](search-synonyms.md) ist in Azure Cognitive Search verfügbar, wird in dieser Tutorialreihe jedoch nicht behandelt.

1. Versuchen Sie, „hot“ als Suchtext einzugeben. Es werden _keine_ Einträge zurückgegeben, die das Wort „hotel“ enthalten. Bei unserer Suche wird nur nach ganzen Wörtern gesucht, aber es werden trotzdem einige Ergebnisse zurückgegeben.

1. Versuchen Sie es mit anderen Wörtern: „pool“, „sunshine“, „view“ usw. Dies ist eine sehr einfache Funktion von Azure Cognitive Search, die aber trotzdem in überzeugender Manier ausgeführt wird.

## <a name="test-edge-conditions-and-errors"></a>Testen von Edgebedingungen und -fehlern

Es ist wichtig zu überprüfen, ob unsere Features für die Fehlerbehandlung wie gewünscht funktionieren, auch wenn gerade keine Fehler zu beobachten sind. 

1. Geben Sie in der **Index**-Methode nach dem **try {** -Aufruf die Zeile **Throw new Exception()** ein. Diese Ausnahme erzwingt einen Fehler, wenn Sie nach Text suchen.

2. Führen Sie die App aus, geben Sie „bar“ als Suchtext ein, und klicken Sie auf das Suchsymbol. Die Ausnahme sollte in der Fehleransicht angezeigt werden.

     :::image type="content" source="media/tutorial-csharp-create-first-app/azure-search-error.png" alt-text="Erzwingen eines Fehlers" border="true":::

    > [!Important]
    > Es wird als Sicherheitsrisiko angesehen, interne Fehlernummern auf Fehlerseiten zurückzugeben. Falls Ihre App für die allgemeine Nutzung bestimmt ist, sollten Sie sich über die sicheren und bewährten Methoden informieren, die beim Auftreten eines Fehlers befolgt werden sollten.

3. Entfernen Sie **Throw new Exception()** , wenn Sie der Meinung sind, dass die Fehlerbehandlung wie gewünscht durchgeführt wird.

## <a name="takeaways"></a>Wesentliche Punkte

Beachten Sie die folgenden Erkenntnisse aus diesem Projekt:

* Ein Azure Cognitive Search-Aufruf ist kompakt, und es ist einfach, die Ergebnisse zu interpretieren.
* Mit asynchronen Aufrufen wird dem Controller ein geringer Grad an Komplexität hinzugefügt, aber dies ist die bewährte Methode, wenn Sie qualitativ gute Apps entwickeln möchten.
* Mit dieser App wurde eine einfache Textsuche durchgeführt, die gemäß der Einrichtung von **searchOptions** definiert wurde. Diese eine Klasse kann aber mit vielen Membern aufgefüllt werden, die der Erweiterung der Suche dienen. Es ist kein großer Zusatzaufwand erforderlich, um diese App erheblich leistungsstärker zu gestalten.

## <a name="next-steps"></a>Nächste Schritte

Um die Benutzerfreundlichkeit zu erhöhen, fügen Sie weitere Features hinzu, insbesondere eine Paginierung (entweder Seitenzahlen oder unendliches Scrollen) und AutoVervollständigen/Vorschläge. Außerdem sollten Sie die Verwendung von ausgefeilteren Suchoptionen erwägen (z. B. geografische Suchen nach Hotels innerhalb eines bestimmten Radius um einen bestimmten Punkt und Sortierung von Suchergebnissen).

Diese nächsten Schritte werden in den folgenden Tutorials behandelt. Wir beginnen mit der Paginierung.

> [!div class="nextstepaction"]
> [C#-Tutorial: Suchergebnispaginierung – Azure Cognitive Search](tutorial-csharp-paging.md)