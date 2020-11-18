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
ms.date: 10/05/2020
ms.custom: devx-track-csharp
ms.openlocfilehash: f3e43a6b72d8de25de3220a9a6ac4e0b3986a467
ms.sourcegitcommit: e2dc549424fb2c10fcbb92b499b960677d67a8dd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/17/2020
ms.locfileid: "94701805"
---
# <a name="quickstart-create-a-search-index-using-the-azuresearchdocuments-client-library"></a>Schnellstart: Erstellen eines Suchindexes mithilfe Clientbibliothek „Azure.Search.Documents“

Verwenden Sie die neue [Clientbibliothek „Azure.Search.Documents“ (Version 11)](/dotnet/api/overview/azure/search.documents-readme), um eine .NET Core-Konsolenanwendung in C# zu erstellen, die einen Suchindex erstellt, lädt und abfragt.

[Laden Sie den Quellcode herunter](https://github.com/Azure-Samples/azure-search-dotnet-samples/tree/master/quickstart/v11), um mit einem fertigen Projekt zu beginnen, oder führen Sie die Schritte in diesem Artikel aus, um ein eigenes zu erstellen.

> [!NOTE]
> Suchen Sie nach einer früheren Version? Dann finden Sie Informationen unter [Erstellen eines Suchindexes mithilfe von Microsoft.Azure.Search V10](search-get-started-dotnet-v10.md).

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie beginnen, benötigen Sie die folgenden Tools und Dienste:

+ Ein Azure-Konto mit einem aktiven Abonnement. Sie können [kostenlos ein Konto erstellen](https://azure.microsoft.com/free/).

+ Ein Azure Cognitive Search-Dienst [Erstellen Sie einen Dienst](search-create-service-portal.md), oder [suchen Sie nach einem vorhandenen Dienst](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices). Für diesen Schnellstart können Sie einen kostenlosen Dienst verwenden. 

+ [Visual Studio](https://visualstudio.microsoft.com/downloads/) (beliebige Edition). Der Beispielcode wurde in der kostenlosen Community-Edition von Visual Studio 2019 getestet.

+ [NuGet-Paket „Azure.Search.Documents“](https://www.nuget.org/packages/Azure.Search.Documents/)

## <a name="set-up-your-project"></a>Einrichten des Projekts

Stellen Sie Dienstverbindungsinformationen zusammen, und starten Sie dann Visual Studio, um ein neues Konsolen-App-Projekt zu erstellen, das unter .NET Core ausgeführt werden kann.

<a name="get-service-info"></a>

### <a name="copy-a-key-and-endpoint"></a>Kopieren eines Schlüssels und Endpunkts

Aufrufe, die an den Dienst gerichtet werden, erfordern jeweils einen URL-Endpunkt und einen Zugriffsschlüssel. Suchen Sie in einem ersten Schritt den API-Schlüssel und die URL, die Sie zu Ihrem Projekt hinzufügen möchten. Sie werden beide Werte bei der Erstellung des Clients in einem späteren Schritt angeben.

1. [Melden Sie sich beim Azure-Portal an](https://portal.azure.com/), und rufen Sie auf der Seite **Übersicht** Ihres Suchdiensts die URL ab. Ein Beispiel für einen Endpunkt ist `https://mydemo.search.windows.net`.

2. Rufen Sie in **Einstellungen** > **Schlüssel** einen Administratorschlüssel für vollständige Rechte für den Dienst ab. Dieser ist erforderlich, wenn Sie Objekte erstellen oder löschen. Es gibt einen Primär- und einen Sekundärschlüssel, die austauschbar sind. Sie können beide verwenden.

   ![Abrufen eines HTTP-Endpunkts und eines Zugriffsschlüssels](media/search-get-started-rest/get-url-key.png "Abrufen eines HTTP-Endpunkts und eines Zugriffsschlüssels")

Für alle an Ihren Dienst gesendeten Anforderungen ist ein API-Schlüssel erforderlich. Ein gültiger Schlüssel stellt anforderungsbasiert eine Vertrauensstellung her zwischen der Anwendung, die die Anforderung versendet, und dem Dienst, der sie verarbeitet.

### <a name="install-the-nuget-package"></a>Installieren des NuGet-Pakets

Fügen Sie nach dem Erstellen des Projekts die Clientbibliothek hinzu. Das [Azure.Search.Documents-Paket](https://www.nuget.org/packages/Azure.Search.Documents/) besteht aus einer Clientbibliothek, die alle APIs bereitstellt, die für die Arbeit mit einem Suchdienst in .NET verwendet werden.

1. Wählen Sie unter **Tools** > **NuGet-Paket-Manager** die Option **NuGet-Pakete für Projektmappe verwalten...** aus. 

1. Klicken Sie auf **Durchsuchen**.

1. Suchen Sie nach `Azure.Search.Documents`, und wählen Sie Version 11.0 oder höher aus.

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

1. Erstellen Sie zwei Clients: [SearchIndexClient](/dotnet/api/azure.search.documents.indexes.searchindexclient) erstellt den Index und [SearchClient](/dotnet/api/azure.search.documents.searchclient) lädt und fragt einen vorhandenen Index ab. Beide benötigen den Dienstendpunkt und einen Administrator-API-Schlüssel für die Authentifizierung mit Berechtigungen zum Erstellen/Löschen.

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
       SearchClient srchclient = new SearchClient(serviceEndpoint, indexName, credential);
    ```

## <a name="1---create-an-index"></a>1\. Erstellen eines Index

In dieser Schnellstartanleitung wird ein Hotelindex erstellt, in den Sie Hoteldaten laden und Abfragen dafür ausführen. In diesem Schritt definieren Sie die Felder im Index. Jede Felddefinition enthält einen Namen, einen Datentyp und Attribute, die bestimmen, wie das Feld verwendet wird.

In diesem Beispiel werden aus Gründen der Einfachheit und Lesbarkeit synchrone Methoden der Bibliothek „Azure.Search.Documents“ verwendet. In Produktionsszenarien sollten Sie jedoch asynchrone Methoden verwenden, um Ihre App skalierbar und reaktionsfähig zu halten. So würden Sie beispielsweise [CreateIndexAsync](/dotnet/api/azure.search.documents.indexes.searchindexclient.createindexasync) anstatt [CreateIndex](/dotnet/api/azure.search.documents.indexes.searchindexclient.createindex) verwenden.

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

1. Erstellen Sie in **Program.cs** ein [SearchIndex](/dotnet/api/azure.search.documents.indexes.models.searchindex)-Objekt, und rufen Sie dann die [CreateIndex](/dotnet/api/azure.search.documents.indexes.searchindexclient.createindex)-Methode auf, um den Index in Ihrem Suchdienst auszudrücken.

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

In der Azure.Search.Documents-Clientbibliothek können Sie [SearchableField](/dotnet/api/azure.search.documents.indexes.models.searchablefield) und [SimpleField](/dotnet/api/azure.search.documents.indexes.models.simplefield) verwenden, um Felddefinitionen zu optimieren. Beide sind Ableitungen von [SearchField](/dotnet/api/azure.search.documents.indexes.models.searchfield) und können Ihren Code möglicherweise vereinfachen:

+ `SimpleField` kann ein beliebiger Datentyp sein, ist immer „nicht durchsuchbar“ (wird bei Volltextsuchabfragen ignoriert) und ist abrufbar (nicht ausgeblendet). Andere Attribute sind standardmäßig deaktiviert, können jedoch aktiviert werden. Sie können ein `SimpleField` für Dokument-IDs oder Felder verwenden, die nur in Filtern, Facets oder Bewertungsprofilen verwendet werden. Sofern dies der Fall ist, stellen Sie sicher, dass Sie alle Attribute anwenden, die für das Szenario erforderlich sind, z. B. `IsKey = true` für eine Dokument-ID. Weitere Informationen finden Sie unter [SimpleFieldAttribute.cs](https://github.com/Azure/azure-sdk-for-net/blob/master/sdk/search/Azure.Search.Documents/src/Indexes/SimpleFieldAttribute.cs) im Quellcode.

+ `SearchableField` muss eine Zeichenfolge sein und ist immer „durchsuchbar“ und „abrufbar“. Andere Attribute sind standardmäßig deaktiviert, können jedoch aktiviert werden. Da dieser Feldtyp durchsuchbar ist, unterstützt er Synonyme und die gesamte Palette der Eigenschaften des Analysetools. Weitere Informationen finden Sie unter [SearchableFieldAttribute.cs](https://github.com/Azure/azure-sdk-for-net/blob/master/sdk/search/Azure.Search.Documents/src/Indexes/SearchableFieldAttribute.cs) im Quellcode.

Ob Sie die grundlegende `SearchField`-API oder eines der Hilfsmodelle verwenden, müssen Sie Filter-, Facet- und Sortierattribute explizit aktivieren. Beispielsweise müssen [IsFilterable](/dotnet/api/azure.search.documents.indexes.models.searchfield.isfilterable), [IsSortable](/dotnet/api/azure.search.documents.indexes.models.searchfield.issortable) und [IsFacetable](/dotnet/api/azure.search.documents.indexes.models.searchfield.isfacetable) explizit mit Attributen versehen werden, wie im obigen Beispiel gezeigt. 

<a name="load-documents"></a>

## <a name="2---load-documents"></a>2\. Laden von Dokumenten

Azure Cognitive Search durchsucht im Dienst gespeicherte Inhalte. In diesem Schritt laden Sie JSON-Dokumente, die dem soeben erstellten Hotelindex entsprechen.

Bei Azure Cognitive Search sind Suchdokumente Datenstrukturen, die sowohl Eingaben für die Indizierung als auch Ausgaben von Abfragen sind. Beispiele für Dokumenteingaben aus einer externen Datenquelle wären etwa Zeilen in einer Datenbank, Blobs in Blob Storage oder JSON-Dokumente auf dem Datenträger. In diesem Beispiel nehmen wir eine Abkürzung und betten JSON-Dokumente für fünf Hotels direkt in den Code ein. 

Beim Hochladen von Dokumenten muss ein [IndexDocumentsBatch](/dotnet/api/azure.search.documents.models.indexdocumentsbatch-1)-Objekt verwendet werden. Ein `IndexDocumentsBatch`-Objekt enthält eine Sammlung von [Aktionen](/dotnet/api/azure.search.documents.models.indexdocumentsbatch-1.actions), und jede dieser Aktionen enthält wiederum ein Dokument und eine Eigenschaft, die Azure Cognitive Search mitteilt, welche Aktion ausgeführt werden soll ([„upload“, „merge“, „delete“ oder „mergeOrUpload“](search-what-is-data-import.md#indexing-actions)).

1. Erstellen Sie in **Program.cs** ein Array mit Dokumenten und Indexaktionen, und übergeben Sie es anschließend an `IndexDocumentsBatch`. Die folgenden Dokumente entsprechen dem Index „hotels-quickstart-v11“ (gemäß Definition durch die Klasse „hotel“).

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
    srchclient.IndexDocuments(batch, idxoptions);
    ```

    Nachdem Sie das [IndexDocumentsBatch](/dotnet/api/azure.search.documents.models.indexdocumentsbatch-1)-Objekt initialisiert haben, können Sie es an den Index senden, indem Sie [IndexDocuments](/dotnet/api/azure.search.documents.searchclient.indexdocuments) für Ihr [SearchClient](/dotnet/api/azure.search.documents.searchclient)-Objekt aufrufen.

1. Da es sich hierbei um eine Konsolen-App handelt, die alle Befehle sequenziell ausführt, fügen Sie eine Wartezeit von 2 Sekunden zwischen der Indizierung und Abfragen hinzu.

    ```csharp
    // Wait 2 seconds for indexing to complete before starting queries (for demo and console-app purposes only)
    Console.WriteLine("Waiting for indexing...\n");
    System.Threading.Thread.Sleep(2000);
    ```

    Die Verzögerung von zwei Sekunden dient zur Kompensierung der Indizierung. Diese erfolgt asynchron, sodass alle Dokumente vor der Ausführung der Abfragen indiziert werden können. Die Programmierung einer solchen Verzögerung ist in der Regel nur in Demos, bei Tests und in Beispielanwendungen erforderlich.

## <a name="3---search-an-index"></a>3\. Durchsuchen eines Index

Abfrageergebnisse können abgerufen werden, sobald das erste Dokument indiziert wurde. Mit dem Testen des Index sollte aber gewartet werden, bis alle Dokumente indiziert wurden.

In diesem Abschnitt werden zwei Funktionen hinzugefügt: Abfragelogik und Ergebnisse. Verwenden Sie für Abfragen die [Search](/dotnet/api/azure.search.documents.searchclient.search)-Methode. Diese Methode akzeptiert sowohl Suchtext (die Abfragezeichenfolge) als auch andere [Optionen](/dotnet/api/azure.search.documents.searchoptions).

Die [SearchResults](/dotnet/api/azure.search.documents.models.searchresults-1)-Klasse stellt die Ergebnisse dar.

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
    private static void RunQueries(SearchClient srchclient)
    {
        SearchOptions options;
        SearchResults<Hotel> response;

        Console.WriteLine("Query #1: Search on the term 'motel' and list the relevance score for each match...\n");

        options = new SearchOptions()
        {
            Filter = "",
            OrderBy = { "" }
        };

        response = srchclient.Search<Hotel>("motel", options);
        WriteDocuments(response);

        Console.WriteLine("Query #2: Find hotels where 'type' equals hotel...\n");

        options = new SearchOptions()
        {
            Filter = "hotelCategory eq 'hotel'",
        };

        response = srchclient.Search<Hotel>("*", options);
        WriteDocuments(response);

        Console.WriteLine("Query #3: Filter on rates less than $200 and sort by when the hotel was last updated...\n");

        options = new SearchOptions()
        {
            Filter = "baseRate lt 200",
            OrderBy = { "lastRenovationDate desc" }
        };

        response = srchclient.Search<Hotel>("*", options);
        WriteDocuments(response);
    }
    ```

1. Fügen Sie RunQueries zu `Main()` hinzu.

    ```csharp
    Console.WriteLine("Starting queries...\n");
    RunQueries(srchclient);
    ```

Dieses Beispiel zeigt die zwei [Abgleichsmethoden für Begriffe in einer Abfrage](search-query-overview.md#types-of-queries): Volltextsuche und Filter.

+ Volltextsuchabfragen für einen oder mehrere Begriffe in durchsuchbaren Feldern in Ihrem Index. Die erste Abfrage ist eine Volltextsuche. Die Volltextsuche erzeugt relevante Bewertungen, die dazu dienen, den Ergebnissen einen Rang zuzuweisen.

+ Ein Filter ist ein boolescher Ausdruck, der für Felder vom Typ [IsFilterable](/dotnet/api/azure.search.documents.indexes.models.searchfield.isfilterable) in einem Index ausgewertet wird. Filterabfragen schließen Werte ein oder aus. Daher ist einer Filterabfrage keine Relevanzbewertung zugeordnet. Die letzten zwei Abfragen veranschaulichen die Filtersuche.

Volltextsuche und Filter können separat oder zusammen verwendet werden.

Suchen und Filtern erfolgt mithilfe der [SearchClient.Search](/dotnet/api/azure.search.documents.searchclient.search)-Methode. Eine Suchabfrage kann in der `searchText`-Zeichenfolge und ein Filterausdruck kann in der [Filter](/dotnet/api/azure.search.documents.searchoptions.filter)-Eigenschaft der [SearchOptions](/dotnet/api/azure.search.documents.searchoptions)-Klasse übergeben werden. Um ohne Suche zu filtern, übergeben Sie einfach `"*"` für den Parameter `searchText` der [Search](/dotnet/api/azure.search.documents.searchclient.search)-Methode. Wenn Sie ohne Filter suchen möchten, legen Sie die Eigenschaft `Filter` nicht fest, oder übergeben Sie einfach keine `SearchOptions`-Instanz.

## <a name="run-the-program"></a>Ausführen des Programms

Drücken Sie F5, um die App neu zu erstellen und das fertige Programm auszuführen. 

Die Ausgabe umfasst Nachrichten aus [Console.WriteLine](/dotnet/api/system.console.writeline), wobei Abfrageinformationen und -ergebnisse hinzugefügt werden.

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
> [Beginnen mit der Kostenanalyse mit Cost Management](../cost-management-billing/costs/quick-acm-cost-analysis.md?WT.mc_id=costmanagementcontent_docsacmhorizontal_-inproduct-learn)