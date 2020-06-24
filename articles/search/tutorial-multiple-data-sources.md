---
title: C#-Tutorial zum Indizieren mehrerer Azure-Datenquellen
titleSuffix: Azure Cognitive Search
description: Hier erfahren Sie, wie Sie mithilfe von Indexern Daten aus mehreren Datenquellen in einen einzelnen Azure Cognitive Search-Index importieren. Dieses Tutorial und der Beispielcode gelten für C#.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: tutorial
ms.date: 06/20/2020
ms.openlocfilehash: d63e437090b2875c7e6a8273fdf22d49597d408f
ms.sourcegitcommit: 635114a0f07a2de310b34720856dd074aaf4f9cd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/23/2020
ms.locfileid: "85262207"
---
# <a name="tutorial-index-from-multiple-data-sources-using-the-net-sdk"></a>Tutorial: Indizieren von mehreren Datenquellen mithilfe des .NET SDK

Azure Cognitive Search kann Daten aus mehreren Datenquellen importieren, analysieren und in einem einzelnen konsolidierten Suchindex indizieren. Dies ist hilfreich, wenn strukturierte Daten mit weniger strukturierten Daten oder sogar Nur-Text-Daten aus anderen Quellen wie etwa Text-, HTML- oder JSON-Dokumenten aggregiert werden.

In diesem Tutorial erfahren Sie, wie Sie Hoteldaten aus einer Azure Cosmos DB-Datenquelle indizieren und mit Hotelzimmerdetails aus Azure Blob Storage-Dokumenten zusammenführen. Das Ergebnis ist ein kombinierter Hotelsuchindex mit komplexen Datentypen.

In diesem Tutorial werden C# und das [.NET SDK](https://docs.microsoft.com/dotnet/api/overview/azure/search) verwendet. Sie führen in diesem Tutorial die folgenden Aufgaben durch:

> [!div class="checklist"]
> * Hochladen von Beispieldaten und Erstellen von Datenquellen
> * Ermitteln des Dokumentschlüssels
> * Definieren und Erstellen des Index
> * Indizieren von Hoteldaten aus Azure Cosmos DB
> * Zusammenführen von Hotelzimmerdaten aus Blob Storage

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

+ [Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/create-cosmosdb-resources-portal)
+ [Azure Storage (in englischer Sprache)](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account)
+ [Visual Studio 2019](https://visualstudio.microsoft.com/)
+ [Neuer](search-create-service-portal.md) oder [bereits vorhandener](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) Suchdienst 

> [!Note]
> In diesem Tutorial können Sie den kostenlosen Dienst verwenden. Der kostenlose Suchdienst ist auf drei Indizes, drei Indexer und drei Datenquellen beschränkt. In diesem Tutorial wird davon jeweils eine Instanz erstellt. Vergewissern Sie sich zunächst, dass Ihr Dienst über genügend freie Kapazität für die neuen Ressourcen verfügt.

## <a name="download-files"></a>Herunterladen von Dateien

Den Quellcode für dieses Tutorial finden Sie im GitHub-Repository [azure-search-dotnet-samples](https://github.com/Azure-Samples/azure-search-dotnet-samples) im Ordner [multiple-data-sources](https://github.com/Azure-Samples/azure-search-dotnet-samples/tree/master/multiple-data-sources).

## <a name="1---create-services"></a>1\. Erstellen der Dienste

In diesem Tutorial wird Azure Cognitive Search für die Indizierung und für Abfragen verwendet, und Azure Cosmos DB wird für das eine Dataset und Azure-Blobspeicher für das andere Dataset genutzt. 

Alle Dienste sollten nach Möglichkeit in derselben Region und Ressourcengruppe erstellt werden, um eine möglichst große Nähe zu erreichen und die Verwaltung zu vereinfachen. In der Praxis können sich Ihre Dienste in einer beliebigen Region befinden.

In diesem Beispiel werden zwei kleine Datensätze verwendet, die sieben fiktive Hotels beschreiben. Ein Datensatz beschreibt die Hotels an sich und wird in eine Azure Cosmos DB-Datenbank geladen. Der andere Datensatz enthält Hotelzimmerdetails und wird in Form von sieben separaten JSON-Dateien bereitgestellt, die in Azure Blob Storage hochgeladen werden.

### <a name="start-with-cosmos-db"></a>Beginnen mit Cosmos DB

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an, und navigieren Sie zur Übersichtsseite Ihres Azure Cosmos DB-Kontos.

1. Wählen Sie **Daten-Explorer** und danach **Neue Datenbank** aus.

   ![Erstellen einer neuen Datenbank](media/tutorial-multiple-data-sources/cosmos-newdb.png "Erstellen einer neuen Datenbank")

1. Geben Sie den Namen **hotel-rooms-db** ein. Übernehmen Sie für die übrigen Einstellungen die Standardwerte.

   ![Konfigurieren der Datenbank](media/tutorial-multiple-data-sources/cosmos-dbname.png "Konfigurieren der Datenbank")

1. Erstellen Sie einen neuen Container. Verwenden Sie die soeben erstellte Datenbank. Geben Sie **hotels** als Containername ein, und verwenden Sie **/HotelId** als Partitionsschlüssel.

   ![Hinzufügen eines Containers](media/tutorial-multiple-data-sources/cosmos-add-container.png "Hinzufügen eines Containers")

1. Wählen Sie unterhalb von **hotels** den Eintrag **Elemente** aus, und klicken Sie auf der Befehlsleiste auf **Element hochladen**. Navigieren Sie im Projektordner zur Datei **cosmosdb/HotelsDataSubset_CosmosDb.json**, und wählen Sie die Datei aus.

   ![Hochladen in die Azure Cosmos DB-Sammlung](media/tutorial-multiple-data-sources/cosmos-upload.png "Hochladen in die Cosmos DB-Sammlung")

1. Aktualisieren Sie mithilfe der Schaltfläche „Aktualisieren“ die Elementansicht in der Hotelsammlung. Daraufhin sollten sieben neue Datenbankdokumente aufgeführt werden.

### <a name="azure-blob-storage"></a>Azure Blob Storage

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an, navigieren Sie zu Ihrem Azure-Speicherkonto, und klicken Sie nacheinander auf **Blobs** und **+ Container**.

1. [Erstellen Sie einen Blobcontainer](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-portal) mit dem Namen **hotel-rooms** zum Speichern der exemplarischen JSON-Hotelzimmerdateien. Sie können die öffentliche Zugriffsebene auf beliebige gültige Werte festlegen.

   ![Erstellen eines Blobcontainers](media/tutorial-multiple-data-sources/blob-add-container.png "Erstellen eines Blobcontainers")

1. Nachdem der Container erstellt wurde, öffnen Sie ihn, und wählen Sie auf der Befehlsleiste die Option **Hochladen** aus. Navigieren Sie zu dem Ordner, der die Beispieldateien enthält. Wählen Sie alle Dateien aus, und klicken Sie dann auf **Hochladen**.

   ![Hochladen von Dateien](media/tutorial-multiple-data-sources/blob-upload.png "Hochladen von Dateien")

Nach Abschluss des Uploadvorgangs sollten die Dateien in der Liste für den Datencontainer angezeigt werden.

### <a name="azure-cognitive-search"></a>Azure Cognitive Search

Die dritte Komponente, Azure Cognitive Search, können Sie [im Portal erstellen](search-create-service-portal.md). Im Rahmen dieser exemplarischen Vorgehensweise können Sie den Free-Tarif verwenden. 

### <a name="get-an-admin-api-key-and-url-for-azure-cognitive-search"></a>Abrufen eines Administrator-API-Schlüssels und einer URL für Azure Cognitive Search

Für die Interaktion mit dem Azure Cognitive Search-Dienst benötigen Sie die Dienst-URL und einen Zugriffsschlüssel. Ein Suchdienst wird mit beidem erstellt. Gehen Sie daher wie folgt vor, um die erforderlichen Informationen zu erhalten, falls Sie Azure Cognitive Search Ihrem Abonnement hinzugefügt haben:

1. [Melden Sie sich beim Azure-Portal an](https://portal.azure.com/), und rufen Sie auf der Seite **Übersicht** Ihres Suchdiensts die URL ab. Ein Beispiel für einen Endpunkt ist `https://mydemo.search.windows.net`.

1. Rufen Sie unter **Einstellungen** > **Schlüssel** einen Administratorschlüssel ab, um Vollzugriff auf den Dienst zu erhalten. Es gibt zwei austauschbare Administratorschlüssel – diese wurden zum Zweck der Geschäftskontinuität bereitgestellt, falls Sie einen Rollover für einen Schlüssel durchführen müssen. Für Anforderungen zum Hinzufügen, Ändern und Löschen von Objekten können Sie den primären oder den sekundären Schlüssel verwenden.

   Rufen Sie auch den Abfrageschlüssel ab. Es empfiehlt sich, Abfrageanforderungen mit schreibgeschütztem Zugriff auszugeben.

   ![Abrufen des Dienstnamens sowie der Administrator- und Abfrageschlüssel](media/search-get-started-nodejs/service-name-and-keys.png)

Ein gültiger Schlüssel stellt anforderungsbasiert eine Vertrauensstellung her zwischen der Anwendung, die die Anforderung versendet, und dem Dienst, der sie verarbeitet.

## <a name="2---set-up-your-environment"></a>2\. Einrichten Ihrer Umgebung

1. Starten Sie Visual Studio 2019, und wählen Sie als Nächstes im Menü **Extras** die Option **NuGet-Paket-Manager** und dann **NuGet-Pakete für Projektmappe verwalten...** aus. 

1. Suchen Sie auf der Registerkarte **Durchsuchen** nach **Microsoft.Azure.Search** (Version 9.0.1 oder höher), und führen Sie die Installation durch. Klicken Sie sich durch die zusätzlichen Dialogfelder, um die Installation abzuschließen.

    ![Hinzufügen von Azure-Bibliotheken per NuGet](./media/tutorial-csharp-create-first-app/azure-search-nuget-azure.png)

1. Suchen Sie nach dem NuGet-Paket **Microsoft.Extensions.Configuration.Json**, und installieren Sie es ebenfalls.

1. Öffnen Sie die Projektmappendatei **AzureSearchMultipleDataSources.sln**.

1. Bearbeiten Sie im Projektmappen-Explorer die Datei **appsettings.json**, um Verbindungsinformationen hinzuzufügen.  

    ```json
    {
      "SearchServiceName": "Put your search service name here",
      "SearchServiceAdminApiKey": "Put your primary or secondary API key here",
      "BlobStorageAccountName": "Put your Azure Storage account name here",
      "BlobStorageConnectionString": "Put your Azure Blob Storage connection string here",
      "CosmosDBConnectionString": "Put your Cosmos DB connection string here",
      "CosmosDBDatabaseName": "hotel-rooms-db"
    }
    ```

Für die ersten beiden Einträge werden die URL und die Administratorschlüssel für Ihren Azure Cognitive Search-Dienst verwendet. Heißt der Endpunkt beispielsweise `https://mydemo.search.windows.net`, muss als Dienstname `mydemo` angegeben werden.

Die nächsten Einträge dienen zum Angeben der Kontonamen und Verbindungszeichenfolgeninformationen für die Azure Blob Storage- und die Azure Cosmos DB-Datenquelle.

## <a name="3---map-key-fields"></a>3\. Zuordnen von Schlüsselfeldern

Zum Zusammenführen von Inhalten ist es erforderlich, dass beide Datenströme im Suchindex auf dieselben Dokumente ausgerichtet sind. 

In Azure Cognitive Search werden die einzelnen Dokumente durch das Schlüsselfeld eindeutig identifiziert. Jeder Suchindex muss über genau ein Schlüsselfeld vom Typ `Edm.String`verfügen. Dieses Schlüsselfeld muss für jedes Dokument in einer Datenquelle vorhanden sein, die dem Index hinzugefügt wird. (Es ist gleichzeitig das einzige erforderliche Feld.)

Stellen Sie beim Indizieren von Daten aus mehreren Datenquellen sicher, dass jede eingehende Zeile bzw. jedes Dokument einen gemeinsamen Dokumentschlüssel enthält, um Daten aus zwei physisch getrennten Quelldokumenten im kombinierten Index in einem neuen Suchdokument zusammenzuführen. 

Häufig ist ein gewisses Maß an Vorausplanung erforderlich, um einen sinnvollen Dokumentschlüssel für Ihren Index zu bestimmen und sicherzustellen, dass er in beiden Datenquellen vorhanden ist. In dieser Demo ist der Schlüssel `HotelId` für jedes Hotel in Cosmos DB auch in den JSON-Blobs für die Zimmer im Blobspeicher vorhanden.

Azure Cognitive Search-Indexer können Feldzuordnungen verwenden, um im Zuge der Indizierung den Namen und sogar das Format von Datenfeldern zu ändern, damit Quelldaten dem korrekten Indexfeld zugeführt werden können. In Cosmos DB hat die Hotel-ID den Namen **`HotelId`** . In den JSON-Blobdateien für die Hotelzimmer lautet die Hotel-ID dagegen **`Id`** . Das Programm behandelt diese Diskrepanz, indem es das Feld **`Id`** aus den Blobs dem Schlüsselfeld **`HotelId`** im Index zuordnet.

> [!NOTE]
> Automatisch generierte Dokumentschlüssel (wie sie etwa standardmäßig von einigen Indexern erstellt werden) eignen sich häufig nicht besonders gut für kombinierte Indizes. Grundsätzlich empfiehlt es sich, einen aussagekräftigen, eindeutigen Schlüsselwert zu verwenden, der bereits in Ihren Datenquellen vorhanden ist oder diesen problemlos hinzugefügt werden kann.

## <a name="4---explore-the-code"></a>4\. Untersuchen des Codes

Sobald die Daten und Konfigurationseinstellungen vorhanden sind, kann das Beispielprogramm in **AzureSearchMultipleDataSources.sln** erstellt und ausgeführt werden.

Diese einfache C#/.NET-Konsolen-App führt folgende Aufgaben aus:

* Sie erstellt auf Grundlage der Datenstruktur der C#-Klasse „Hotel“ (die auch auf die Klassen „Address“ und „Room“ verweist) einen neuen Index.
* Sie erstellt eine neue Datenquelle und einen Indexer, der Azure Cosmos DB-Daten Indexfeldern zuordnet. Beides sind Objekte in Azure Cognitive Search.
* Sie führt den Indexer aus, um Hoteldaten aus Cosmos DB zu laden.
* Sie erstellt eine zweite Datenquelle und einen Indexer, der JSON-Blobdaten Indexfeldern zuordnet.
* Sie führt den zweiten Indexer aus, um Zimmerdaten aus dem Blobspeicher zu laden.

 Nehmen Sie sich vor dem Ausführen des Programms etwas Zeit, um sich den Code sowie die Index- und Indexerdefinitionen für dieses Beispiel etwas genauer anzusehen. Der relevante Code befindet sich in zwei Dateien:

  + **Hotel.cs** enthält das Schema, das den Index definiert.
  + **Program.cs** enthält Funktionen zum Erstellen des Azure Cognitive Search-Index, der Datenquellen und der Indexer sowie zum Laden der kombinierten Ergebnisse in den Index.

### <a name="create-an-index"></a>Erstellen eines Index

In diesem Beispielprogramm wird das .NET SDK verwendet, um einen Azure Cognitive Search-Index zu definieren und zu erstellen. Das SDK nutzt die Klasse [FieldBuilder](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.fieldbuilder), um eine Indexstruktur auf der Grundlage einer C#-Datenmodellklasse zu generieren.

Das Datenmodell wird durch die Klasse „Hotel“ definiert, die auch Verweise auf die Klassen „Address“ und „Room“ enthält. „FieldBuilder“ führt ein Drilldown durch die verschiedenen Klassendefinitionen aus, um eine komplexe Datenstruktur für den Index zu generieren. Mithilfe von Metadatentags werden die Attribute der einzelnen Felder definiert, um beispielsweise anzugeben, ob das Feld durchsuchbar oder sortierbar ist.

Die folgenden Codeausschnitte aus der Datei **Hotel.cs** zeigen, wie ein einzelnes Feld und ein Verweis auf eine andere Datenmodellklasse angegeben werden können.

```csharp
. . . 
[IsSearchable, IsFilterable, IsSortable]
public string HotelName { get; set; }
. . .
public Room[] Rooms { get; set; }
. . .
```

In der Datei **Program.cs** wird der Index mit einem Namen und einer Feldsammlung definiert, die durch die Methode `FieldBuilder.BuildForType<Hotel>()` generiert wird, und anschließend erstellt:

```csharp
private static async Task CreateIndex(string indexName, SearchServiceClient searchService)
{
    // Create a new search index structure that matches the properties of the Hotel class.
    // The Address and Room classes are referenced from the Hotel class. The FieldBuilder
    // will enumerate these to create a complex data structure for the index.
    var definition = new Index()
    {
        Name = indexName,
        Fields = FieldBuilder.BuildForType<Hotel>()
    };
    await searchService.Indexes.CreateAsync(definition);
}
```

### <a name="create-azure-cosmos-db-data-source-and-indexer"></a>Erstellen der Azure Cosmos DB-Datenquelle und des zugehörigen Indexers

Das Hauptprogramm enthält auch Logik zum Erstellen der Azure Cosmos DB-Datenquelle für die Hoteldaten.

Dazu wird zunächst der Name der Azure Cosmos DB-Datenbank mit der Verbindungszeichenfolge verkettet. Anschließend wird das Datenquellenobjekt definiert – einschließlich spezifischer Einstellungen für Azure Cosmos DB-Quellen (wie etwa die Eigenschaft „useChangeDetection“).

  ```csharp
private static async Task CreateAndRunCosmosDbIndexer(string indexName, SearchServiceClient searchService)
{
    // Append the database name to the connection string
    string cosmosConnectString = 
        configuration["CosmosDBConnectionString"]
        + ";Database=" 
        + configuration["CosmosDBDatabaseName"];

    DataSource cosmosDbDataSource = DataSource.CosmosDb(
        name: configuration["CosmosDBDatabaseName"], 
        cosmosDbConnectionString: cosmosConnectString,
        collectionName: "hotels",
        useChangeDetection: true);

    // The Azure Cosmos DB data source does not need to be deleted if it already exists,
    // but the connection string might need to be updated if it has changed.
    await searchService.DataSources.CreateOrUpdateAsync(cosmosDbDataSource);
  ```

Nach Erstellung der Datenquelle richtet das Programm einen Azure Cosmos DB-Indexer namens **hotel-rooms-cosmos-indexer** ein.

```csharp
    Indexer cosmosDbIndexer = new Indexer(
        name: "hotel-rooms-cosmos-indexer",
        dataSourceName: cosmosDbDataSource.Name,
        targetIndexName: indexName,
        schedule: new IndexingSchedule(TimeSpan.FromDays(1)));
    
    // Indexers keep metadata about how much they have already indexed.
    // If we already ran this sample, the indexer will remember that it already
    // indexed the sample data and not run again.
    // To avoid this, reset the indexer if it exists.
    bool exists = await searchService.Indexers.ExistsAsync(cosmosDbIndexer.Name);
    if (exists)
    {
        await searchService.Indexers.ResetAsync(cosmosDbIndexer.Name);
    }
    await searchService.Indexers.CreateOrUpdateAsync(cosmosDbIndexer);
```
Bereits vorhandene Indexer mit dem gleichen Namen werden vor der Erstellung des neuen Indexers gelöscht (für den Fall, dass Sie dieses Beispiel mehrmals ausführen möchten).

In diesem Beispiel wird ein Zeitplan für den Indexer definiert, sodass er einmal täglich ausgeführt wird. Sie können die Zeitplaneigenschaft aus diesem Aufruf entfernen, wenn der Indexer in Zukunft nicht mehr automatisch ausgeführt werden soll.

### <a name="index-azure-cosmos-db-data"></a>Indizieren der Azure Cosmos DB-Daten

Nach Erstellung der Datenquelle und des zugehörigen Indexers ist der Code zum Ausführen des Indexers nicht sonderlich lang:

```csharp
    try
    {
        await searchService.Indexers.RunAsync(cosmosDbIndexer.Name);
    }
    catch (CloudException e) when (e.Response.StatusCode == (HttpStatusCode)429)
    {
        Console.WriteLine("Failed to run indexer: {0}", e.Response.Content);
    }
```

Dieses Beispiel enthält einen einfachen try-catch-Block, um Fehler zu melden, die ggf. bei der Ausführung auftreten.

Nach Ausführung des Azure Cosmos DB-Indexers enthält der Suchindex einen vollständigen Satz exemplarischer Hoteldokumente. Die Zimmerfelder für die einzelnen Hotels sind jedoch ein leeres Array, da die Azure Cosmos DB-Datenquelle keine Zimmerdetails enthält. Als Nächstes führt das Programm einen Pullvorgang für Blob Storage aus, um die Zimmerdaten zu laden und zusammenzuführen.

### <a name="create-blob-storage-data-source-and-indexer"></a>Erstellen der Blob Storage-Datenquelle und des zugehörigen Indexers

Zum Abrufen der Zimmerdetails richtet das Programm zunächst eine Blob Storage-Datenquelle ein, um auf eine Gruppe individueller JSON-Blobdateien zu verweisen.

```csharp
private static async Task CreateAndRunBlobIndexer(string indexName, SearchServiceClient searchService)
{
    DataSource blobDataSource = DataSource.AzureBlobStorage(
        name: configuration["BlobStorageAccountName"],
        storageConnectionString: configuration["BlobStorageConnectionString"],
        containerName: "hotel-rooms");

    // The blob data source does not need to be deleted if it already exists,
    // but the connection string might need to be updated if it has changed.
    await searchService.DataSources.CreateOrUpdateAsync(blobDataSource);
```

Nach Erstellung der Datenquelle richtet das Programm einen Blobindexer namens **hotel-rooms-blob-indexer** ein.

```csharp
    // Add a field mapping to match the Id field in the documents to 
    // the HotelId key field in the index
    List<FieldMapping> map = new List<FieldMapping> {
        new FieldMapping("Id", "HotelId")
    };

    Indexer blobIndexer = new Indexer(
        name: "hotel-rooms-blob-indexer",
        dataSourceName: blobDataSource.Name,
        targetIndexName: indexName,
        fieldMappings: map,
        parameters: new IndexingParameters().ParseJson(),
        schedule: new IndexingSchedule(TimeSpan.FromDays(1)));

    // Reset the indexer if it already exists
    bool exists = await searchService.Indexers.ExistsAsync(blobIndexer.Name);
    if (exists)
    {
        await searchService.Indexers.ResetAsync(blobIndexer.Name);
    }
    await searchService.Indexers.CreateOrUpdateAsync(blobIndexer);
```

Die JSON-Blobs enthalten anstelle von **`HotelId`** ein Schlüsselfeld namens **`Id`** . Der Code weist den Indexer mithilfe der `FieldMapping`-Klasse an, den Wert des Felds **`Id`** an den Dokumentschlüssel **`HotelId`** im Index weiterzuleiten.

Blob Storage-Indexer können Parameter nutzen, die den zu verwendenden Analysemodus angeben. Der Analysemodus unterscheidet sich abhängig davon, ob Blobs ein einzelnes Dokument oder mehrere Dokumente innerhalb des gleichen Blobs darstellen. In diesem Beispiel stellt jedes Blob ein einzelnes Indexdokument dar, weshalb im Code der Parameter `IndexingParameters.ParseJson()` verwendet wird.

Weitere Informationen zu Indexer-Analyseparametern für JSON-Blobs finden Sie unter [Indizieren von JSON-Blobs mit dem Azure Search-Blobindexer](search-howto-index-json-blobs.md). Weitere Informationen zum Angeben dieser Parameter unter Verwendung des .NET SDK finden Sie unter [IndexingParametersExtensions Class](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.indexingparametersextensions) (Klasse „IndexingParametersExtensions“).

Bereits vorhandene Indexer mit dem gleichen Namen werden vor der Erstellung des neuen Indexers gelöscht (für den Fall, dass Sie dieses Beispiel mehrmals ausführen möchten).

In diesem Beispiel wird ein Zeitplan für den Indexer definiert, sodass er einmal täglich ausgeführt wird. Sie können die Zeitplaneigenschaft aus diesem Aufruf entfernen, wenn der Indexer in Zukunft nicht mehr automatisch ausgeführt werden soll.

### <a name="index-blob-data"></a>Indizieren der Blobdaten

Nach Erstellung der Blob Storage-Datenquelle und des zugehörigen Indexers ist der Code zum Ausführen des Indexers nicht sonderlich komplex:

```csharp
    try
    {
        await searchService.Indexers.RunAsync(cosmosDbIndexer.Name);
    }
    catch (CloudException e) when (e.Response.StatusCode == (HttpStatusCode)429)
    {
        Console.WriteLine("Failed to run indexer: {0}", e.Response.Content);
    }
```

Da der Index bereits mit Hoteldaten aus der Azure Cosmos DB-Datenbank aufgefüllt wurde, aktualisiert der Blobindexer die im Index vorhandenen Dokumente und fügt die Zimmerdetails hinzu.

> [!NOTE]
> Wenn beide Datenquellen die gleichen schlüsselfremden Felder enthalten und die Daten in diesen Feldern nicht übereinstimmen, enthält der Index die Werte des zuletzt ausgeführten Indexers. In unserem Beispiel enthalten beide Datenquellen ein Feld namens **HotelName**. Sollten sich die Daten in diesem Feld bei Dokumenten mit dem gleichen Schlüsselwert aus irgendeinem Grund unterscheiden, werden im Index die Daten vom Typ **HotelName** aus der zuletzt indizierten Datenquelle gespeichert.

## <a name="5---search"></a>5\. Suchen

Nach Ausführung des Programms können Sie den aufgefüllten Suchindex über den [**Suchexplorer**](search-explorer.md) des Portals erkunden.

Öffnen Sie im Azure-Portal die Seite **Übersicht** für den Suchdienst, und suchen Sie in der Liste **Indexes** nach dem Index **hotel-rooms-sample**.

  ![Liste mit den Azure Cognitive Search-Indizes](media/tutorial-multiple-data-sources/index-list.png "Liste mit den Azure Cognitive Search-Indizes")

Klicken Sie in der Liste auf den Index „hotel-rooms-sample“. Daraufhin wird eine Suchexploreroberfläche für den Index angezeigt. Geben Sie eine Abfrage für einen Begriff wie „Luxury“ ein. Daraufhin sollte in den Ergebnissen mindestens ein Dokument angezeigt werden, und dieses Dokument sollte eine Liste mit Zimmerobjekten aus dem zugehörigen Zimmerarray enthalten.

## <a name="reset-and-rerun"></a>Zurücksetzen und erneut ausführen

In den frühen experimentellen Phasen der Entwicklung besteht der praktikabelste Ansatz für den Übergang von einer Entwurfsphase zur nächsten darin, die Objekte aus Azure Cognitive Search zu löschen und Ihrem Code zu erlauben, sie neu zu erstellen. Ressourcennamen sind eindeutig. Wenn Sie ein Objekt löschen, können Sie es unter dem gleichen Namen neu erstellen.

Im Beispielcode dieses Tutorials wird eine Überprüfung auf vorhandene Objekte durchgeführt. Diese werden dann gelöscht, damit Sie Ihren Code erneut ausführen können.

Sie können auch das Portal verwenden, um Indizes, Indexer und Datenquellen zu löschen.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie in Ihrem eigenen Abonnement arbeiten, ist es ratsam, nach Abschluss eines Projekts die nicht mehr benötigten Ressourcen zu entfernen. Ressourcen, die weiterhin ausgeführt werden, können Sie Geld kosten. Sie können entweder einzelne Ressourcen oder aber die Ressourcengruppe löschen, um den gesamten Ressourcensatz zu entfernen.

Ressourcen können im Portal über den Link „Alle Ressourcen“ oder „Ressourcengruppen“ im linken Navigationsbereich gesucht und verwaltet werden.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie sich nun mit der Erfassung von Daten aus mehreren Quellen vertraut gemacht haben, können Sie sich die Indexerkonfiguration näher ansehen, indem Sie mit Cosmos DB beginnen.

> [!div class="nextstepaction"]
> [Indizieren von Cosmos DB-Daten mithilfe eines Indexers in der kognitiven Azure-Suche](search-howto-index-cosmosdb.md)