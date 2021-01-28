---
title: C#-Tutorial zur Verwendung von Facetten zur Vereinfachung der Navigation
titleSuffix: Azure Cognitive Search
description: Fahren Sie nach der Suchergebnispaginierung mit diesem Tutorial fort, um Facettennavigation hinzuzufügen. Hier erfahren Sie, wie Facetten verwendet werden können, um ganz einfach eine Suche einzugrenzen.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: tutorial
ms.date: 01/26/2021
ms.custom: devx-track-csharp
ms.openlocfilehash: 44a05ed5b4386f2787a1c84dfeb61a5d539c7cb5
ms.sourcegitcommit: a055089dd6195fde2555b27a84ae052b668a18c7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/26/2021
ms.locfileid: "98789802"
---
# <a name="tutorial-add-faceted-navigation-using-the-net-sdk"></a>Tutorial: Hinzufügen der Facettennavigation mithilfe des .NET SDK

Facetten ermöglichen eine selbstgesteuerte Navigation durch Bereitstellung eines Satzes von Links zum Filtern von Ergebnissen. In diesem Tutorial wird eine Facettennavigationsstruktur auf der linken Seite der Seite mit Bezeichnungen und klickbarem Text eingefügt, um die Ergebnisse einzugrenzen.

In diesem Tutorial lernen Sie Folgendes:
> [!div class="checklist"]
> * Festlegen von Modelleigenschaften als _IsFacetable_
> * Hinzufügen der Facettennavigation zu Ihrer App

## <a name="overview"></a>Übersicht

Facetten beruhen auf Feldern in Ihrem Suchindex. Eine Abfrageanforderung, die facet=[Zeichenfolge] enthält, stellt das Feld für die Facette bereit. Eine Abfrage kann mehrere Facetten umfassen (z. B. `&facet=category&facet=amenities`). Diese müssen jeweils durch ein kaufmännisches Und-Zeichen (&) voneinander getrennt werden. Zum Implementieren einer Facettennavigationsstruktur müssen Sie sowohl Facetten als auch Filter angeben. Der Filter wird für ein Click-Ereignis verwendet, um die Ergebnisse einzugrenzen. Wenn Sie z. B. auf „budget“ klicken, werden die Ergebnisse anhand dieser Kriterien gefiltert.

Dieses Tutorial erweitert das im Tutorial [Hinzufügen von Paginierung zu Suchergebnissen](tutorial-csharp-paging.md) erstellte Projekt um unendliches Scrollen.

Eine fertige Version des Codes in diesem Tutorial finden Sie im folgenden Projekt:

* [4-add-facet-navigation (GitHub)](https://github.com/Azure-Samples/azure-search-dotnet-samples/tree/master/create-first-app/v11/4-add-facet-navigation)

## <a name="prerequisites"></a>Voraussetzungen

* Projektmappe [2a-add-paging (GitHub)](https://github.com/Azure-Samples/azure-search-dotnet-samples/tree/master/create-first-app/v11/2a-add-paging). Dieses Projekt kann entweder Ihre eigene im vorherigen Tutorial erstellte Version oder eine Kopie von GitHub sein.

Dieses Tutorial wurde aktualisiert, um das Paket [Azure.Search.Documents (Version 11)](https://www.nuget.org/packages/Azure.Search.Documents/) verwenden zu können. Eine frühere Version des .NET SDK finden Sie im [Codebeispiel Microsoft.Azure.Search (Version 10)](https://github.com/Azure-Samples/azure-search-dotnet-samples/tree/master/create-first-app/v10).

## <a name="set-model-properties-as-isfacetable"></a>Festlegen von Modelleigenschaften als „IsFacetable“

Damit eine Modelleigenschaft in einer Facettensuche gefunden werden kann, muss sie mit **IsFacetable** gekennzeichnet sein.

1. Untersuchen Sie die Klasse **Hotel**. **Category** und **Tags** sind beispielsweise als **IsFacetable** gekennzeichnet, **HotelName** und **Description** dagegen nicht. 

    ```cs
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
    ```

1. Wir ändern in diesem Tutorial keine Tags, schließen Sie die Datei „hotel.cs“ also unverändert.

    > [!Note]
    > Eine Facettensuche führt zu einem Fehler, wenn ein bei der Suche angefordertes Feld nicht ordnungsgemäß gekennzeichnet ist.

## <a name="add-facet-navigation-to-your-app"></a>Hinzufügen der Facettennavigation zu Ihrer App

Für dieses Beispiel ermöglichen wir es dem Benutzer, eine Hotelkategorie oder eine Ausstattung aus einer Liste von Links auszuwählen, die links neben den Ergebnissen angezeigt werden. Der Benutzer beginnt mit der Eingabe von Suchtext und grenzt dann die Ergebnisse der Suche zunehmend ein, indem er eine Kategorie oder ein Ausstattungsmerkmal auswählt.

Der Controller muss die Listen der Facetten an die Ansicht übergeben. Um die Benutzerauswahl während der Suche beizubehalten, verwenden wir den Zwischenspeicher als Mechanismus für die Zustandsspeicherung.

![Verwenden der Facettennavigation zum Eingrenzen einer Suche nach „Pool“](./media/tutorial-csharp-create-first-app/azure-search-facet-nav.png)

### <a name="add-filter-strings-to-the-searchdata-model"></a>Hinzufügen von Filterzeichenfolgen zum SearchData-Modell

1. Öffnen Sie die Datei „SearchData.cs“, und fügen Sie der Klasse **SearchData** Zeichenfolgeneigenschaften hinzu, sodass sie die Facettenfilter-Zeichenfolgen enthält.

    ```cs
    public string categoryFilter { get; set; }
    public string amenityFilter { get; set; }
    ```

### <a name="add-the-facet-action-method"></a>Hinzufügen der Aktionsmethode „Facette“

Der Home-Controller benötigt die neue Aktion **Facet** sowie Aktualisierungen der vorhandenen Aktionen **Index** und **Page** sowie der Methode **RunQueryAsync**.

<!-- 1. Open the home controller file, and add the **using** statement, to enable the **List&lt;string&gt;** construct.

    ```cs
    using System.Collections.Generic; 
    ```-->

1. Ersetzen Sie die Aktionsmethode **Index(SearchData model)** .

    ```cs
    public async Task<ActionResult> Index(SearchData model)
    {
        try
        {
            // Ensure the search string is valid.
            if (model.searchText == null)
            {
                model.searchText = "";
            }

            // Make the search call for the first page.
            await RunQueryAsync(model, 0, 0, "", "").ConfigureAwait(false);
        }
        catch
        {
            return View("Error", new ErrorViewModel { RequestId = "1" });
        }

        return View(model);
    }
    ```

1. Ersetzen Sie die Aktionsmethode **PageAsync(SearchData model)** .

    ```cs
    public async Task<ActionResult> PageAsync(SearchData model)
    {
        try
        {
            int page;

            // Calculate the page that should be displayed.
            switch (model.paging)
            {
                case "prev":
                    page = (int)TempData["page"] - 1;
                    break;

                case "next":
                    page = (int)TempData["page"] + 1;
                    break;

                default:
                    page = int.Parse(model.paging);
                    break;
            }

            // Recover the leftMostPage.
            int leftMostPage = (int)TempData["leftMostPage"];

            // Recover the filters.
            string catFilter = TempData["categoryFilter"].ToString();
            string ameFilter = TempData["amenityFilter"].ToString();

            // Recover the search text.
            model.searchText = TempData["searchfor"].ToString();

            // Search for the new page.
            await RunQueryAsync(model, page, leftMostPage, catFilter, ameFilter);
        }

        catch
        {
            return View("Error", new ErrorViewModel { RequestId = "2" });
        }
        return View("Index", model);
    }
    ```

1. Fügen Sie die Aktionsmethode **FacetAsync(SearchData model)** hinzu, die aktiviert wird, wenn der Benutzer auf einen Facettenlink klickt. Das Modell enthält einen Suchfilter entweder für die Kategorie oder eine Ausstattung. Fügen Sie es nach der **PageAsync**-Aktion hinzu.

    ```cs
    public async Task<ActionResult> FacetAsync(SearchData model)
    {
        try
        {
            // Filters set by the model override those stored in temporary data.
            string catFilter;
            string ameFilter;
            if (model.categoryFilter != null)
            {
                catFilter = model.categoryFilter;
            } else
            {
                catFilter = TempData["categoryFilter"].ToString();
            }

            if (model.amenityFilter != null)
            {
                ameFilter = model.amenityFilter;
            } else
            {
                ameFilter = TempData["amenityFilter"].ToString();
            }

            // Recover the search text.
            model.searchText = TempData["searchfor"].ToString();

            // Initiate a new search.
            await RunQueryAsync(model, 0, 0, catFilter, ameFilter).ConfigureAwait(false);
        }
        catch
        {
            return View("Error", new ErrorViewModel { RequestId = "2" });
        }

        return View("Index", model);
    }
    ```

### <a name="set-up-the-search-filter"></a>Einrichten des Suchfilters

Wenn ein Benutzer eine bestimmte Facette auswählt, z. B. auf die Kategorie **Resort und Spa** klickt, dann sollten in den Ergebnissen nur Hotels angezeigt werden, die als diese Kategorie angegeben sind. Zum Einschränken einer Suche auf diese Weise müssen wir einen _Filter_ einrichten.

1. Ersetzen Sie die Methode **RunQueryAsync** durch den folgenden Code: In erster Linie nimmt er eine Kategoriefilter-Zeichenfolge und eine Ausstattungs-Filterzeichenfolge und legt den Parameter **Filter** von **SearchOptions** fest.

    ```cs
    private async Task<ActionResult> RunQueryAsync(SearchData model, int page, int leftMostPage, string catFilter, string ameFilter)
    {
        InitSearch();

        string facetFilter = "";

        if (catFilter.Length > 0 && ameFilter.Length > 0)
        {
            // Both facets apply.
            facetFilter = $"{catFilter} and {ameFilter}"; 
        } else
        {
            // One, or zero, facets apply.
            facetFilter = $"{catFilter}{ameFilter}";
        }

        var options = new SearchOptions
        {
            Filter = facetFilter,

            SearchMode = SearchMode.All,

            // Skip past results that have already been returned.
            Skip = page * GlobalVariables.ResultsPerPage,

            // Take only the next page worth of results.
            Size = GlobalVariables.ResultsPerPage,

            // Include the total number of results.
            IncludeTotalCount = true,
        };

        // Return information on the text, and number, of facets in the data.
        options.Facets.Add("Category,count:20");
        options.Facets.Add("Tags,count:20");

        // Enter Hotel property names into this list, so only these values will be returned.
        options.Select.Add("HotelName");
        options.Select.Add("Description");
        options.Select.Add("Category");
        options.Select.Add("Tags");

        // For efficiency, the search call should be asynchronous, so use SearchAsync rather than Search.
        model.resultList = await _searchClient.SearchAsync<Hotel>(model.searchText, options).ConfigureAwait(false);

        // This variable communicates the total number of pages to the view.
        model.pageCount = ((int)model.resultList.TotalCount + GlobalVariables.ResultsPerPage - 1) / GlobalVariables.ResultsPerPage;

        // This variable communicates the page number being displayed to the view.
        model.currentPage = page;

        // Calculate the range of page numbers to display.
        if (page == 0)
        {
            leftMostPage = 0;
        }
        else if (page <= leftMostPage)
        {
            // Trigger a switch to a lower page range.
            leftMostPage = Math.Max(page - GlobalVariables.PageRangeDelta, 0);
        }
        else if (page >= leftMostPage + GlobalVariables.MaxPageRange - 1)
        {
            // Trigger a switch to a higher page range.
            leftMostPage = Math.Min(page - GlobalVariables.PageRangeDelta, model.pageCount - GlobalVariables.MaxPageRange);
        }
        model.leftMostPage = leftMostPage;

        // Calculate the number of page numbers to display.
        model.pageRange = Math.Min(model.pageCount - leftMostPage, GlobalVariables.MaxPageRange);

        // Ensure Temp data is stored for the next call.
        TempData["page"] = page;
        TempData["leftMostPage"] = model.leftMostPage;
        TempData["searchfor"] = model.searchText;
        TempData["categoryFilter"] = catFilter;
        TempData["amenityFilter"] = ameFilter;

        // Return the new view.
        return View("Index", model);
    }
    ```

    Der Liste der zurückzugebenden **Select**-Elemente wurden die Eigenschaften **Category** und **Tags** hinzugefügt. Diese Hinzufügung ist keine Voraussetzung dafür, dass die Facettennavigation funktioniert, aber wir verwenden diese Informationen, um sicherzustellen, dass die Filter korrekt funktionieren.

### <a name="add-lists-of-facet-links-to-the-view"></a>Hinzufügen von Listen mit Facettenlinks zur Ansicht

Die Ansicht erfordert einige wesentliche Änderungen. 

1. Öffnen Sie zunächst die Datei „hotels.css“ (im Ordner „wwwroot/css“), und fügen Sie die folgenden Klassen hinzu:

    ```html
    .facetlist {
        list-style: none;
    }

    .facetchecks {
        width: 250px;
        display: normal;
        color: #666;
        margin: 10px;
        padding: 5px;
    }

    .facetheader {
        font-size: 10pt;
        font-weight: bold;
        color: darkgreen;
    }
    ```

1. Für die Ansicht organisieren Sie die Ausgabe in einer Tabelle, um die Facettenlisten auf der linken Seite und die Ergebnisse auf der rechten Seite sauber auszurichten. Öffnen Sie die Datei „index.cshtml“. Ersetzen Sie den gesamten Inhalt der HTML-&lt;Textkörper&gt;-Tags durch folgenden Code:

    ```html
    <body>
        @using (Html.BeginForm("Index", "Home", FormMethod.Post))
        {
            <table>
                <tr>
                    <td></td>
                    <td>
                        <h1 class="sampleTitle">
                            <img src="~/images/azure-logo.png" width="80" />
                            Hotels Search - Facet Navigation
                        </h1>
                    </td>
                </tr>

                <tr>
                    <td></td>
                    <td>
                        <!-- Display the search text box, with the search icon to the right of it.-->
                        <div class="searchBoxForm">
                            @Html.TextBoxFor(m => m.searchText, new { @class = "searchBox" }) <input value="" class="searchBoxSubmit" type="submit">
                        </div>
                    </td>
                </tr>

                <tr>
                    <td valign="top">
                        <div id="facetplace" class="facetchecks">

                            @if (Model != null && Model.resultList != null)
                            {
                                List<string> categories = Model.resultList.Facets["Category"].Select(x => x.Value.ToString()).ToList();

                                if (categories.Count > 0)
                                {
                                    <h5 class="facetheader">Category:</h5>
                                    <ul class="facetlist">
                                        @for (var c = 0; c < categories.Count; c++)
                                        {
                                            var facetLink = $"{categories[c]} ({Model.resultList.Facets["Category"][c].Count})";
                                            <li>
                                                @Html.ActionLink(facetLink, "FacetAsync", "Home", new { categoryFilter = $"Category eq '{categories[c]}'" }, null)
                                            </li>
                                        }
                                    </ul>
                                }

                                List<string> tags = Model.resultList.Facets["Tags"].Select(x => x.Value.ToString()).ToList();

                                if (tags.Count > 0)
                                {
                                    <h5 class="facetheader">Amenities:</h5>
                                    <ul class="facetlist">
                                        @for (var c = 0; c < tags.Count; c++)
                                        {
                                            var facetLink = $"{tags[c]} ({Model.resultList.Facets["Tags"][c].Count})";
                                            <li>
                                                @Html.ActionLink(facetLink, "FacetAsync", "Home", new { amenityFilter = $"Tags/any(t: t eq '{tags[c]}')" }, null)
                                            </li>
                                        }
                                    </ul>
                                }
                            }
                        </div>
                    </td>
                    <td valign="top">
                        <div id="resultsplace">
                            @if (Model != null && Model.resultList != null)
                            {
                                // Show the result count.
                                <p class="sampleText">
                                    @Model.resultList.TotalCount Results
                                </p>

                                var results = Model.resultList.GetResults().ToList();

                                @for (var i = 0; i < results.Count; i++)
                                {
                                    string amenities = string.Join(", ", results[i].Document.Tags);

                                    string fullDescription = results[i].Document.Description;
                                    fullDescription += $"\nCategory: {results[i].Document.Category}";
                                    fullDescription += $"\nAmenities: {amenities}";


                                    // Display the hotel name and description.
                                    @Html.TextAreaFor(m => results[i].Document.HotelName, new { @class = "box1" })
                                    @Html.TextArea($"desc{i}", fullDescription, new { @class = "box2" })
                                }
                            }
                        </div>
                    </td>
                </tr>

                <tr>
                    <td></td>
                    <td valign="top">
                        @if (Model != null && Model.pageCount > 1)
                        {
                            // If there is more than one page of results, show the paging buttons.
                            <table>
                                <tr>
                                    <td class="tdPage">
                                        @if (Model.currentPage > 0)
                                        {
                                            <p class="pageButton">
                                                @Html.ActionLink("|<", "PageAsync", "Home", new { paging = "0" }, null)
                                            </p>
                                        }
                                        else
                                        {
                                            <p class="pageButtonDisabled">|&lt;</p>
                                        }
                                    </td>

                                    <td class="tdPage">
                                        @if (Model.currentPage > 0)
                                        {
                                            <p class="pageButton">
                                                @Html.ActionLink("<", "PageAsync", "Home", new { paging = "prev" }, null)
                                            </p>
                                        }
                                        else
                                        {
                                            <p class="pageButtonDisabled">&lt;</p>
                                        }
                                    </td>

                                    @for (var pn = Model.leftMostPage; pn < Model.leftMostPage + Model.pageRange; pn++)
                                    {
                                        <td class="tdPage">
                                            @if (Model.currentPage == pn)
                                            {
                                                // Convert displayed page numbers to 1-based and not 0-based.
                                                <p class="pageSelected">@(pn + 1)</p>
                                            }
                                            else
                                            {
                                                <p class="pageButton">
                                                    @Html.ActionLink((pn + 1).ToString(), "PageAsync", "Home", new { paging = @pn }, null)
                                                </p>
                                            }
                                        </td>
                                    }

                                    <td class="tdPage">
                                        @if (Model.currentPage < Model.pageCount - 1)
                                        {
                                            <p class="pageButton">
                                                @Html.ActionLink(">", "PageAsync", "Home", new { paging = "next" }, null)
                                            </p>
                                        }
                                        else
                                        {
                                            <p class="pageButtonDisabled">&gt;</p>
                                        }
                                    </td>

                                    <td class="tdPage">
                                        @if (Model.currentPage < Model.pageCount - 1)
                                        {
                                            <p class="pageButton">
                                                @Html.ActionLink(">|", "PageAsync", "Home", new { paging = Model.pageCount - 1 }, null)
                                            </p>
                                        }
                                        else
                                        {
                                            <p class="pageButtonDisabled">&gt;|</p>
                                        }
                                    </td>
                                </tr>
                            </table>
                        }
                    </td>
                </tr>
            </table>
        }
    </body>
    ```

    Beachten Sie die Verwendung des Aufrufs **Html.ActionLink**. Dieser Aufruf kommuniziert gültige Filterzeichenfolgen an den Controller, wenn der Benutzer auf einen Facettenlink klickt. 

### <a name="run-and-test-the-app"></a>Ausführen und Testen der App

Der Vorteil der Facettennavigation für den Benutzer besteht darin, dass er die Suche mit einem einzigen Klick eingrenzen kann. Dies können wir in der folgenden Sequenz zeigen.

1. Starten Sie die App, und geben Sie als Suchtext „Flughafen“ ein. Stellen Sie sicher, dass die Liste der Facetten ordentlich auf der linken Seite angezeigt wird. Diese Facetten sind alle, die für Hotels mit „Flughafen“ ihren Textdaten gelten, mit der Angabe, wie oft sie vorkommen.

    ![Verwenden der Facettennavigation zum Eingrenzen einer Suche nach „Flughafen“](./media/tutorial-csharp-create-first-app/azure-search-facet-airport.png)

1. Klicken Sie auf die Kategorie **Resort und Spa**. Stellen Sie sicher, dass sich alle Ergebnisse in dieser Kategorie befinden.

    ![Eingrenzen der Suche auf „Resort und Spa“](./media/tutorial-csharp-create-first-app/azure-search-facet-airport-ras.png)

1. Klicken Sie auf die Ausstattung **Kontinentales Frühstück**. Stellen Sie sicher, dass sich alle Ergebnisse weiterhin in der Kategorie „Resort und Spa“ befinden und die ausgewählte Ausstattung aufweisen.

    ![Eingrenzen der Suche auf „Kontinentales Frühstück“](./media/tutorial-csharp-create-first-app/azure-search-facet-airport-ras-cb.png)

1. Versuchen Sie, eine andere Kategorie auszuwählen, dann eine Ausstattung, und sehen Sie sich die Ergebnisse der Einschränkung an. Dann versuchen Sie es andersherum, eine Ausstattung, dann eine Kategorie. Senden Sie eine leere Suche, um die Seite zurückzusetzen.

    >[!Note]
    > Wenn eine Auswahl in einer Facettenliste (z. B. Kategorie) getroffen wird, überschreibt sie jede vorherige Auswahl innerhalb der Kategorienliste.

## <a name="takeaways"></a>Wesentliche Punkte

Beachten Sie die folgenden Erkenntnisse aus diesem Projekt:

* Es ist zwingend erforderlich, jedes Facettenfeld mit der **IsFacetable**-Eigenschaft markiert wird, damit es in die Facettennavigation eingeschlossen wird.
* Facetten werden mit Filtern kombiniert, um die Ergebnisse einzugrenzen.
* Facetten sind kumulativ, d. h., jede Auswahl baut auf der vorherigen Auswahl auf, um die Ergebnisse weiter einzugrenzen.

## <a name="next-steps"></a>Nächste Schritte

Im nächsten Tutorial betrachten wir die Anordnung der Ergebnisse. Bis zu diesem Zeitpunkt werden Ergebnisse einfach in der Reihenfolge angeordnet, in der sie sich in der Datenbank befinden.

> [!div class="nextstepaction"]
> [C#-Tutorial: Reihenfolge der Ergebnisse – Azure Cognitive Search](tutorial-csharp-orders.md)
