---
title: 'Schnellstart: Erstellen eines Suchindex in .NET'
titleSuffix: Azure Cognitive Search
description: In diesem C#-Schnellstart erfahren Sie, wie Sie mit der Clientbibliothek „Azure.Search.Documents“ einen Index erstellen, Daten laden und Abfragen ausführen.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.devlang: dotnet
ms.topic: quickstart
ms.date: 08/05/2020
ms.openlocfilehash: a2a860a2ff96c74f9d19fe7abfd845bbae8023cd
ms.sourcegitcommit: 4f1c7df04a03856a756856a75e033d90757bb635
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/07/2020
ms.locfileid: "87922267"
---
# <a name="quickstart-create-a-search-index-using-the-azuresearchdocuments-client-library"></a>Schnellstart: Erstellen eines Suchindexes mithilfe Clientbibliothek „Azure.Search.Documents“

Verwenden Sie die neue [Clientbibliothek „Azure.Search.Documents“ (Version 11)](https://docs.microsoft.com/dotnet/api/overview/azure/search.documents-readme?view=azure-dotnet), um eine .NET Core-Konsolenanwendung in C# zu erstellen, die einen Suchindex erstellt, lädt und abfragt.

[Laden Sie den Quellcode herunter](https://github.com/Azure-Samples/azure-search-dotnet-samples/tree/master/quickstart-v11), um mit einem fertigen Projekt zu beginnen, oder führen Sie die Schritte in diesem Artikel aus, um ein eigenes zu erstellen.

> [!NOTE]
> Suchen Sie nach einer früheren Version? Dann finden Sie Informationen unter [Erstellen eines Suchindexes mithilfe von Microsoft.Azure.Search V10](search-get-started-dotnet-v10.md).

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie beginnen, benötigen Sie die folgenden Tools und Dienste:

+ Ein Azure-Konto mit einem aktiven Abonnement. Sie können [kostenlos ein Konto erstellen](https://azure.microsoft.com/free/).

+ Ein Azure Cognitive Search-Dienst [Erstellen Sie einen Dienst](search-create-service-portal.md), oder [suchen Sie nach einem vorhandenen Dienst](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices). Für diesen Schnellstart können Sie einen kostenlosen Dienst verwenden. 

+ [Visual Studio](https://visualstudio.microsoft.com/downloads/) (beliebige Edition). Der Beispielcode wurde in der kostenlosen Community-Edition von Visual Studio 2019 getestet.

<a name="get-service-info"></a>

## <a name="get-a-key-and-endpoint"></a>Einen Schlüssel und Endpunkt abrufen

Aufrufe, die an den Dienst gerichtet werden, erfordern jeweils einen URL-Endpunkt und einen Zugriffsschlüssel. Ein Suchdienst wird mit beidem erstellt. Gehen Sie daher wie folgt vor, um die erforderlichen Informationen zu erhalten, falls Sie Azure Cognitive Search Ihrem Abonnement hinzugefügt haben:

1. [Melden Sie sich beim Azure-Portal an](https://portal.azure.com/), und rufen Sie auf der Seite **Übersicht** Ihres Suchdiensts die URL ab. Ein Beispiel für einen Endpunkt ist `https://mydemo.search.windows.net`.

2. Rufen Sie in **Einstellungen** > **Schlüssel** einen Administratorschlüssel für vollständige Rechte für den Dienst ab. Dieser ist erforderlich, wenn Sie Objekte erstellen oder löschen. Es gibt einen Primär- und einen Sekundärschlüssel, die austauschbar sind. Sie können beide verwenden.

   ![Abrufen eines HTTP-Endpunkts und eines Zugriffsschlüssels](media/search-get-started-postman/get-url-key.png "Abrufen eines HTTP-Endpunkts und eines Zugriffsschlüssels")

Für alle an Ihren Dienst gesendeten Anforderungen ist ein API-Schlüssel erforderlich. Ein gültiger Schlüssel stellt anforderungsbasiert eine Vertrauensstellung her zwischen der Anwendung, die die Anforderung versendet, und dem Dienst, der sie verarbeitet.

## <a name="set-up-your-project"></a>Einrichten des Projekts

Starten Sie Visual Studio, und erstellen Sie ein neues Konsolen-App-Projekt, das mit .NET Core ausgeführt werden kann. 

### <a name="install-the-nuget-package"></a>Installieren des NuGet-Pakets

Fügen Sie nach dem Erstellen des Projekts die Clientbibliothek hinzu. Das [Azure.Search.Documents-Paket](https://www.nuget.org/packages/Azure.Search.Documents/) besteht aus einer Clientbibliothek, die alle APIs bereitstellt, die für die Arbeit mit einem Suchdienst in .NET verwendet werden.

1. Wählen Sie unter **Tools** > **NuGet-Paket-Manager** die Option **NuGet-Pakete für Projektmappe verwalten...** aus. 

1. Klicken Sie auf **Durchsuchen**.

1. Suchen Sie nach `Azure.Search.Documents`, und wählen Sie Version 11.0.0 aus.

1. Klicken Sie rechts auf **Installieren**, um die Assembly Ihrem Projekt und Ihrer Projektmappe hinzuzufügen.

### <a name="create-a-search-client"></a>Erstellen eines Suchclients

1. Ändern Sie in **Program.cs** den Namespace in `AzureSearch.SDK.Quickstart.v11`, und fügen Sie dann die folgenden `using`-Direktiven hinzu.

   ```csharp
   using Azure;
   using Azure.Search.Documents;
   using Azure.Search.Documents.Indexes;
   using Azure.Search.Documents.Indexes.Models;
   using Azure.Search.Documents.Models;
   ```

1. Erstellen Sie zwei Clients: [SearchIndexClient](https://docs.microsoft.com/dotnet/api/azure.search.documents.indexes.searchindexclient) erstellt den Index, und [SearchClient](https://docs.microsoft.com/dotnet/api/azure.search.documents.searchclient) arbeitet mit einem vorhandenen Index. Beide benötigen den Dienstendpunkt und einen Administrator-API-Schlüssel für die Authentifizierung mit Berechtigungen zum Erstellen/Löschen.

   ```csharp
   static void Main(string[] args)
   {
       string serviceName = "<YOUR-SERVICE-NAME>";
       string indexName = "hotels-quickstart-v11";
       string apiKey = "<YOUR-ADMIN-API-KEY>";

       // Create a SearchIndexClient to send create/delete index commands
       Uri serviceEndpoint = new Uri($"https://{serviceName}.search.windows.net/");
       AzureKeyCredential credential = new AzureKeyCredential(apiKey);
       SearchIndexClient idxclient = new SearchIndexClient(serviceEndpoint, credential);

       // Create a SearchClient to load and query documents
       SearchClient qryclient = new SearchClient(serviceEndpoint, indexName, credential);
    ```

## <a name="1---create-an-index"></a>1\. Erstellen eines Index

In dieser Schnellstartanleitung wird ein Hotelindex erstellt, in den Sie Hoteldaten laden und Abfragen dafür ausführen. In diesem Schritt definieren Sie die Felder im Index. Jede Felddefinition enthält einen Namen, einen Datentyp und Attribute, die bestimmen, wie das Feld verwendet wird.

In diesem Beispiel werden aus Gründen der Einfachheit und Lesbarkeit synchrone Methoden der Bibliothek „Azure.Search.Documents“ verwendet. In Produktionsszenarien sollten Sie jedoch asynchrone Methoden verwenden, um Ihre App skalierbar und reaktionsfähig zu halten. So würden Sie beispielsweise [CreateIndexAsync](https://docs.microsoft.com/dotnet/api/azure.search.documents.indexes.searchindexclient.createindexasync) anstatt [CreateIndex](https://docs.microsoft.com/dotnet/api/azure.search.documents.indexes.searchindexclient.createindex) verwenden.

1. Fügen Sie Ihrem Projekt eine leere Klassendefinition hinzu: **Hotel.cs**

1. Definieren Sie in **Hotel.cs** die Struktur eines Hoteldokuments.

    ```csharp
    using System;
    using System.Text.Json.Serialization;

    namespace AzureSearch.SDK.Quickstart.v11
    {
        public class Hotel
        {
            [JsonPropertyName("hotelId")]
            public string Id { get; set; }

            [JsonPropertyName("hotelName")]
            public string Name { get; set; }

            [JsonPropertyName("hotelCategory")]
            public string Category { get; set; }

            [JsonPropertyName("baseRate")]
            public Int32 Rate { get; set; }

            [JsonPropertyName("lastRenovationDate")]
            public DateTime Updated { get; set; }
        }
    }
    ```

1. Geben Sie in **Program.cs** die Felder und Attribute an. [SearchIndex](https://docs.microsoft.com/dotnet/api/azure.search.documents.indexes.models.searchindex) und [CreateIndex](https://docs.microsoft.com/dotnet/api/azure.search.documents.indexes.searchindexclient.createindex) werden zum Erstellen eines Index verwendet.

   ```csharp
    // Define an index schema using SearchIndex
    // Create the index using SearchIndexClient
    SearchIndex index = new SearchIndex(indexName)
    {
        Fields =
            {
                new SimpleField("hotelId", SearchFieldDataType.String) { IsKey = true, IsFilterable = true, IsSortable = true },
                new SearchableField("hotelName") { IsFilterable = true, IsSortable = true },
                new SearchableField("hotelCategory") { IsFilterable = true, IsSortable = true },
                new SimpleField("baseRate", SearchFieldDataType.Int32) { IsFilterable = true, IsSortable = true },
                new SimpleField("lastRenovationDate", SearchFieldDataType.DateTimeOffset) { IsFilterable = true, IsSortable = true }
            }
    };

    Console.WriteLine("{0}", "Creating index...\n");
    idxclient.CreateIndex(index);
   ```

Attribute für das Feld bestimmen die Verwendung in einer Anwendung. So muss beispielsweise jedem Feld, das einen Filterausdruck unterstützt, das Attribut `IsFilterable` zugewiesen werden.

Anders als bei früheren Versionen des .NET SDK, die [IsSearchable](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.field.issearchable) für durchsuchbare Zeichenfolgenfeldern benötigen, können Sie [SearchableField](https://docs.microsoft.com/dotnet/api/azure.search.documents.indexes.models.searchablefield) und [SimpleField](https://docs.microsoft.com/dotnet/api/azure.search.documents.indexes.models.simplefield) verwenden, um Felddefinitionen zu optimieren.

Ähnlich wie bei den vorherigen Versionen sind auch andere Attribute für die Definition selbst erforderlich. Beispielsweise müssen [IsFilterable](https://docs.microsoft.com/dotnet/api/azure.search.documents.indexes.models.searchfield.isfilterable), [IsSortable](https://docs.microsoft.com/dotnet/api/azure.search.documents.indexes.models.searchfield.issortable) und [IsFacetable](https://docs.microsoft.com/dotnet/api/azure.search.documents.indexes.models.searchfield.isfacetable) explizit mit Attributen versehen werden, wie im obigen Beispiel gezeigt. 

<a name="load-documents"></a>

## <a name="2---load-documents"></a>2\. Laden von Dokumenten

Azure Cognitive Search durchsucht im Dienst gespeicherte Inhalte. In diesem Schritt laden Sie JSON-Dokumente, die dem soeben erstellten Hotelindex entsprechen.

In Azure Cognitive Search sind Dokumente Datenstrukturen, bei denen es sich sowohl um Eingaben für die Indizierung als auch um Ausgaben von Abfragen handeln kann. Beispiele für Dokumenteingaben aus einer externen Datenquelle wären etwa Zeilen in einer Datenbank, Blobs in Blob Storage oder JSON-Dokumente auf dem Datenträger. In diesem Beispiel nehmen wir eine Abkürzung und betten JSON-Dokumente für fünf Hotels direkt in den Code ein. 

Beim Hochladen von Dokumenten muss ein [IndexDocumentsBatch](https://docs.microsoft.com/dotnet/api/azure.search.documents.models.indexdocumentsbatch-1)-Objekt verwendet werden. Ein IndexDocumentsBatch-Objekt enthält eine Sammlung von [Aktionen](https://docs.microsoft.com/dotnet/api/azure.search.documents.models.indexdocumentsbatch-1.actions), und jede dieser Aktionen enthält wiederum ein Dokument und eine Eigenschaft, die Azure Cognitive Search mitteilt, welche Aktion ausgeführt werden soll ([upload, merge, delete oder mergeOrUpload](search-what-is-data-import.md#indexing-actions)).

1. Erstellen Sie in **Program.cs** ein Array von Dokumenten und Indexaktionen, und übergeben Sie dann das Array an `ndexDocumentsBatch`. Die unten aufgeführten Dokumente entsprechen den Index „hotels-quickstart-v11“, wie von der Hotelklasse definiert.

    ```csharp
    // Load documents (using a subset of fields for brevity)
    IndexDocumentsBatch<Hotel> batch = IndexDocumentsBatch.Create(
        IndexDocumentsAction.Upload(new Hotel { Id = "78", Name = "Upload Inn", Category = "hotel", Rate = 279, Updated = new DateTime(2018, 3, 1, 7, 0, 0) }),
        IndexDocumentsAction.Upload(new Hotel { Id = "54", Name = "Breakpoint by the Sea", Category = "motel", Rate = 162, Updated = new DateTime(2015, 9, 12, 7, 0, 0) }),
        IndexDocumentsAction.Upload(new Hotel { Id = "39", Name = "Debug Motel", Category = "motel", Rate = 159, Updated = new DateTime(2016, 11, 11, 7, 0, 0) }),
        IndexDocumentsAction.Upload(new Hotel { Id = "48", Name = "NuGet Hotel", Category = "hotel", Rate = 238, Updated = new DateTime(2016, 5, 30, 7, 0, 0) }),
        IndexDocumentsAction.Upload(new Hotel { Id = "12", Name = "Renovated Ranch", Category = "motel", Rate = 149, Updated = new DateTime(2020, 1, 24, 7, 0, 0) }));

    IndexDocumentsOptions idxoptions = new IndexDocumentsOptions { ThrowOnAnyError = true };

    Console.WriteLine("{0}", "Loading index...\n");
    qryclient.IndexDocuments(batch, idxoptions);
    ```

    Nachdem Sie das [IndexDocumentsBatch](https://docs.microsoft.com/dotnet/api/azure.search.documents.models.indexdocumentsbatch-1)-Objekt initialisiert haben, können Sie es an den Index senden, indem Sie [IndexDocuments](https://docs.microsoft.com/dotnet/api/azure.search.documents.searchclient.indexdocuments) für Ihr [SearchClient](https://docs.microsoft.com/dotnet/api/azure.search.documents.searchclient)-Objekt aufrufen.

1. Da es sich hierbei um eine Konsolen-App handelt, die alle Befehle sequenziell ausführt, fügen Sie eine Wartezeit von 2 Sekunden zwischen der Indizierung und Abfragen hinzu.

    ```csharp
    // Wait 2 seconds for indexing to complete before starting queries (for demo and console-app purposes only)
    Console.WriteLine("Waiting for indexing...\n");
    System.Threading.Thread.Sleep(2000);
    ```

    Die Verzögerung von zwei Sekunden dient zur Kompensierung der Indizierung. Diese erfolgt asynchron, sodass alle Dokumente vor der Ausführung der Abfragen indiziert werden können. Die Programmierung einer solchen Verzögerung ist in der Regel nur in Demos, bei Tests und in Beispielanwendungen erforderlich.

## <a name="3---search-an-index"></a>3\. Durchsuchen eines Index

Abfrageergebnisse können abgerufen werden, sobald das erste Dokument indiziert wurde. Mit dem Testen des Index sollte aber gewartet werden, bis alle Dokumente indiziert wurden.

In diesem Abschnitt werden zwei Funktionen hinzugefügt: Abfragelogik und Ergebnisse. Verwenden Sie für Abfragen die [Search](https://docs.microsoft.com/dotnet/api/azure.search.documents.searchclient.search)-Methode. Diese Methode akzeptiert sowohl Suchtext (die Abfragezeichenfolge) als auch andere [Optionen](https://docs.microsoft.com/dotnet/api/azure.search.documents.searchoptions).

Die [SearchResults](https://docs.microsoft.com/dotnet/api/azure.search.documents.models.searchresults-1)-Klasse stellt die Ergebnisse dar.

1. Erstellen Sie in **Program.cs** eine WriteDocuments-Methode, die Suchergebnisse in der Konsole ausgibt.

    ```csharp
    private static void WriteDocuments(SearchResults<Hotel> searchResults)
    {
        foreach (SearchResult<Hotel> response in searchResults.GetResults())
        {
            Hotel doc = response.Document;
            var score = response.Score;
            Console.WriteLine($"Name: {doc.Name}, Type: {doc.Category}, Rate: {doc.Rate}, Last-update: {doc.Updated}, Score: {score}");
        }

        Console.WriteLine();
    }
    ```

1. Erstellen Sie eine RunQueries-Methode, um Abfragen auszuführen und Ergebnisse zurückzugeben. Bei den Ergebnissen handelt es sich um Objekte vom Typ „Hotel“.

    ```csharp
    private static void RunQueries(SearchClient qryclient)
    {
        SearchOptions options;
        SearchResults<Hotel> response;

        Console.WriteLine("Query #1: Search on the term 'motel' and list the relevance score for each match...\n");

        options = new SearchOptions()
        {
            Filter = "",
            OrderBy = { "" }
        };

        response = qryclient.Search<Hotel>("motel", options);
        WriteDocuments(response);

        Console.WriteLine("Query #2: Find hotels where 'type' equals hotel...\n");

        options = new SearchOptions()
        {
            Filter = "hotelCategory eq 'hotel'",
        };

        response = qryclient.Search<Hotel>("*", options);
        WriteDocuments(response);

        Console.WriteLine("Query #3: Filter on rates less than $200 and sort by when the hotel was last updated...\n");

        options = new SearchOptions()
        {
            Filter = "baseRate lt 200",
            OrderBy = { "lastRenovationDate desc" }
        };

        response = qryclient.Search<Hotel>("*", options);
        WriteDocuments(response);
    }
    ```

Dieses Beispiel zeigt die zwei [Abgleichsmethoden für Begriffe in einer Abfrage](search-query-overview.md#types-of-queries): Volltextsuche und Filter.

+ Volltextsuchabfragen für einen oder mehrere Begriffe in durchsuchbaren Feldern in Ihrem Index. Die erste Abfrage ist eine Volltextsuche. Die Volltextsuche erzeugt relevante Bewertungen, die dazu dienen, den Ergebnissen einen Rang zuzuweisen.

+ Ein Filter ist ein boolescher Ausdruck, der für Felder vom Typ [IsFilterable](https://docs.microsoft.com/dotnet/api/azure.search.documents.indexes.models.searchfield.isfilterable) in einem Index ausgewertet wird. Filterabfragen schließen Werte ein oder aus. Daher ist einer Filterabfrage keine Relevanzbewertung zugeordnet. Die letzten zwei Abfragen veranschaulichen die Filtersuche.

Volltextsuche und Filter können separat oder zusammen verwendet werden.

Suchen und Filtern erfolgt mithilfe der [SearchClient.Search](https://docs.microsoft.com/dotnet/api/azure.search.documents.searchclient.search)-Methode. Eine Suchabfrage kann in der `searchText`-Zeichenfolge und ein Filterausdruck kann in der [Filter](https://docs.microsoft.com/dotnet/api/azure.search.documents.searchoptions.filter)-Eigenschaft der [SearchOptions](https://docs.microsoft.com/dotnet/api/azure.search.documents.searchoptions)-Klasse übergeben werden. Um ohne Suche zu filtern, übergeben Sie einfach `"*"` für den Parameter `searchText` der [Search](https://docs.microsoft.com/dotnet/api/azure.search.documents.searchclient.search)-Methode. Wenn Sie ohne Filter suchen möchten, legen Sie die Eigenschaft `Filter` nicht fest, oder übergeben Sie einfach keine `SearchOptions`-Instanz.

## <a name="run-the-program"></a>Ausführen des Programms

Drücken Sie F5, um die App neu zu erstellen und das fertige Programm auszuführen. 

Die Ausgabe umfasst Nachrichten aus [Console.WriteLIne](https://docs.microsoft.com/dotnet/api/system.console.writeline), wobei Abfrageinformationen und -ergebnisse hinzugefügt werden.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie in Ihrem eigenen Abonnement arbeiten, sollten Sie sich am Ende eines Projekts überlegen, ob Sie die erstellten Ressourcen noch benötigen. Ressourcen, die weiterhin ausgeführt werden, können Sie Geld kosten. Sie können entweder einzelne Ressourcen oder aber die Ressourcengruppe löschen, um den gesamten Ressourcensatz zu entfernen.

Ressourcen können im Portal über den Link **Alle Ressourcen** oder **Ressourcengruppen** im linken Navigationsbereich gesucht und verwaltet werden.

Denken Sie bei Verwendung eines kostenlosen Diensts an die Beschränkung auf maximal drei Indizes, Indexer und Datenquellen. Sie können einzelne Elemente über das Portal löschen, um unter dem Limit zu bleiben. 

## <a name="next-steps"></a>Nächste Schritte

In dieser C#-Schnellstartanleitung haben Sie eine Reihe von Aufgaben ausgeführt, um einen Index zu erstellen, Dokumente in den Index zu laden und Abfragen auszuführen. In einzelnen Phasen haben wir den Code zur besseren Lesbarkeit und zum besseren Verständnis ein wenig vereinfacht. Nachdem Sie sich mit den grundlegenden Konzepten vertraut gemacht haben, können Sie Ihr Wissen im nächsten Artikel mit alternativen Ansätzen und Konzepten vertiefen. 

> [!div class="nextstepaction"]
> [Verwenden von Azure Search aus einer .NET-Anwendung](search-howto-dotnet-sdk.md)

Möchten Sie Ihre Cloudausgaben optimieren und dabei sparen?

> [!div class="nextstepaction"]
> [Beginnen mit der Kostenanalyse mit Cost Management](https://docs.microsoft.com/azure/cost-management-billing/costs/quick-acm-cost-analysis?WT.mc_id=costmanagementcontent_docsacmhorizontal_-inproduct-learn)