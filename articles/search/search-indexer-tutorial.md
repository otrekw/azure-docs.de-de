---
title: 'Tutorial: Indizieren von Daten aus Azure SQL-Datenbank-Instanzen in C#'
titleSuffix: Azure Cognitive Search
description: In diesem C#-Tutorial erfahren Sie, wie Sie eine Verbindung mit Azure SQL-Datenbank herstellen, durchsuchbare Daten extrahieren und diese in einen Azure Cognitive Search-Index laden.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: tutorial
ms.date: 02/26/2020
ms.openlocfilehash: 978587b68e719b79db31ff25adaf2b38d2235095
ms.sourcegitcommit: 96dc60c7eb4f210cacc78de88c9527f302f141a9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2020
ms.locfileid: "77650061"
---
# <a name="tutorial-index-azure-sql-data-in-c-using-azure-cognitive-search-indexers"></a>Tutorial: Indizieren von Azure SQL-Daten in C# unter Verwendung von Azure Cognitive Search-Indexern

Konfigurieren Sie unter Verwendung von C# einen [Indexer](search-indexer-overview.md), der durchsuchbare Daten aus Azure SQL-Datenbank extrahiert und an einen Suchindex sendet. In diesem Tutorial werden [Azure Cognitive Search-.NET-Clientbibliotheken](https://aka.ms/search-sdk) und eine .NET Core-Konsolenanwendung verwendet, um folgende Aufgaben auszuführen:

> [!div class="checklist"]
> * Erstellen einer mit Azure SQL-Datenbank verbundenen Datenquelle
> * Konfigurieren eines Indexers
> * Ausführen eines Indexers, um Daten in einen Index zu laden
> * Abfragen eines Index als Überprüfungsschritt

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

+ [Azure SQL-Datenbank](https://azure.microsoft.com/services/sql-database/)
+ [Visual Studio](https://visualstudio.microsoft.com/downloads/)
+ [Neuer](search-create-service-portal.md) oder [bereits vorhandener](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) Suchdienst 

> [!Note]
> In diesem Tutorial können Sie den kostenlosen Dienst verwenden. Der kostenlose Suchdienst ist auf drei Indizes, drei Indexer und drei Datenquellen beschränkt. In diesem Tutorial wird davon jeweils eine Instanz erstellt. Vergewissern Sie sich zunächst, dass Ihr Dienst über genügend freie Kapazität für die neuen Ressourcen verfügt.

## <a name="download-source-code"></a>Herunterladen des Quellcodes

Der Quellcode für dieses Tutorial befindet sich im Ordner [DotNetHowToIndexer](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowToIndexers) des GitHub-Repositorys [Azure-Samples/search-dotnet-getting-started](https://github.com/Azure-Samples/search-dotnet-getting-started).

## <a name="get-a-key-and-url"></a>Abrufen eines Schlüssels und einer URL

Für API-Aufrufe benötigen Sie die Dienst-URL und einen Zugriffsschlüssel. Ein Suchdienst wird mit beidem erstellt. Gehen Sie daher wie folgt vor, um die erforderlichen Informationen zu erhalten, falls Sie Azure Cognitive Search Ihrem Abonnement hinzugefügt haben:

1. [Melden Sie sich beim Azure-Portal an](https://portal.azure.com/), und rufen Sie auf der Seite **Übersicht** Ihres Suchdiensts die URL ab. Ein Beispiel für einen Endpunkt ist `https://mydemo.search.windows.net`.

1. Rufen Sie unter **Einstellungen** > **Schlüssel** einen Administratorschlüssel ab, um Vollzugriff auf den Dienst zu erhalten. Es gibt zwei austauschbare Administratorschlüssel – diese wurden zum Zweck der Geschäftskontinuität bereitgestellt, falls Sie einen Rollover für einen Schlüssel durchführen müssen. Für Anforderungen zum Hinzufügen, Ändern und Löschen von Objekten können Sie den primären oder den sekundären Schlüssel verwenden.

   ![Abrufen eines HTTP-Endpunkts und eines Zugriffsschlüssels](media/search-get-started-postman/get-url-key.png "Abrufen eines HTTP-Endpunkts und eines Zugriffsschlüssels")

## <a name="set-up-connections"></a>Einrichten von Verbindungen

1. Starten Sie Visual Studio, und öffnen Sie **DotNetHowToIndexers.sln**.

1. Öffnen Sie im Projektmappen-Explorer die Datei **appsettings.json**, und ersetzen Sie die Platzhalterwerte durch die Verbindungsinformationen für Ihren Suchdienst. Wenn die vollständige URL https://my-demo-service.search.windows.net lautet, muss als Dienstname „my-demo-service“ angegeben werden.

    ```json
    {
      "SearchServiceName": "Put your search service name here",
      "SearchServiceAdminApiKey": "Put your primary or secondary API key here",
      "AzureSqlConnectionString": "Put your Azure SQL database connection string here",
    }
    ```

Für den letzten Eintrag ist eine vorhandene Datenbank erforderlich. Sie erstellen sie im nächsten Schritt.

## <a name="prepare-sample-data"></a>Vorbereiten der Beispieldaten

In diesem Schritt wird eine externe Datenquelle in Azure SQL-Datenbank erstellt, die von einem Indexer durchforstet werden kann. Das Dataset in Azure SQL-Datenbank kann über das Azure-Portal unter Verwendung der Datei *hotels.sql* aus dem Beispiel erstellt werden. Azure Cognitive Search nutzt vereinfachte Rowsets, wie sie beispielsweise auf der Grundlage einer Sicht oder Abfrage generiert werden. Die SQL-Datei in der Beispielprojektmappe erstellt eine einzelne Tabelle und füllt sie auf.

Falls Sie bereits über eine Azure SQL-Datenbank-Ressource verfügen, können Sie ihr die Hoteltabelle hinzufügen (ab Schritt 4).

1. [Melden Sie sich beim Azure-Portal an](https://portal.azure.com/).

1. Suchen Sie nach einer **SQL-Datenbank**, oder erstellen Sie eine. Dabei können Sie die Standardwerte und den günstigsten Tarif verwenden. Die Erstellung eines Servers hat unter anderem den Vorteil, dass Sie einen Administratorbenutzernamen und ein Kennwort angeben können. Diese werden in einem späteren Schritt zum Erstellen und Laden von Tabellen benötigt.

   ![Seite für die neue Datenbank](./media/search-indexer-tutorial/indexer-new-sqldb.png "Seite für die neue Datenbank")

1. Klicken Sie auf **Überprüfen + erstellen**, um den neuen Server und die Datenbank bereitzustellen. Warten Sie, bis Server und Datenbank bereitgestellt wurden.

1. Klicken Sie im Navigationsbereich auf **Abfrage-Editor (Vorschau)** , und geben Sie den Benutzernamen und das Kennwort des Serveradministrators ein. 

   Sollte der Zugriff verweigert werden, kopieren Sie die Client-IP-Adresse aus der Fehlermeldung, und klicken Sie anschließend auf den Link **Serverfirewall festlegen**, um eine Regel hinzuzufügen, die Ihrem Clientcomputer Zugriff gewährt. Geben Sie dabei Ihre Client-IP-Adresse für den Bereich an. Es kann etwas dauern, bis die Regel wirksam wird.

1. Klicken Sie im Abfrage-Editor auf **Abfrage öffnen**, und navigieren Sie zum Speicherort der Datei *hotels.sql* auf Ihrem lokalen Computer. 

1. Wählen Sie die Datei aus, und klicken Sie auf **Öffnen**. Das Skript sollte in etwa wie im folgenden Screenshot aussehen:

   ![SQL-Skript](./media/search-indexer-tutorial/sql-script.png "SQL-Skript")

1. Klicken Sie auf **Ausführen**, um die Abfrage auszuführen. Im Ergebnisbereich sollte eine Erfolgsmeldung für drei Zeilen angezeigt werden.

1. Wenn Sie ein Rowset aus dieser Tabelle zurückgeben möchten, können Sie als Überprüfungsschritt die folgende Abfrage ausführen:

    ```sql
    SELECT * FROM Hotels
    ```

1. Kopieren Sie die ADO.NET-Verbindungszeichenfolge für die Datenbank. Kopieren Sie unter **Einstellungen** > **Verbindungszeichenfolgen** die ADO.NET-Verbindungszeichenfolge (ähnlich wie im folgenden Beispiel).

    ```sql
    Server=tcp:{your_dbname}.database.windows.net,1433;Initial Catalog=hotels-db;Persist Security Info=False;User ID={your_username};Password={your_password};MultipleActiveResultSets=False;Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;
    ```

1. Fügen Sie in Visual Studio in der Datei **appsettings.json** als dritten Eintrag die Verbindungszeichenfolge in „AzureSqlConnectionString“ ein.

    ```json
    {
      "SearchServiceName": "<placeholder-Azure-Search-service-name>",
      "SearchServiceAdminApiKey": "<placeholder-admin-key-for-Azure-Search>",
      "AzureSqlConnectionString": "Server=tcp:{your_dbname}.database.windows.net,1433;Initial Catalog=hotels-db;Persist Security Info=False;User ID={your_username};Password={your_password};MultipleActiveResultSets=False;Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;",
    }
    ```

1. Geben Sie Ihr Kennwort in die Verbindungszeichenfolge in der Datei **appsettings.json** ein. Datenbank- und Benutzernamen werden in Ihrer Verbindungszeichenfolge übernommen, das Kennwort muss jedoch manuell eingegeben werden.

## <a name="build-the-solution"></a>Erstellen der Projektmappe

Drücken Sie F5, um die Projektmappe zu erstellen. Das Programm wird im Debugmodus ausgeführt. Ein Konsolenfenster gibt Aufschluss über den Status der einzelnen Vorgänge.

   ![Konsolenausgabe](./media/search-indexer-tutorial/console-output.png "Konsolenausgabe")

Der Code wird lokal in Visual Studio ausgeführt und stellt eine Verbindung mit Ihrem Suchdienst in Azure her, der wiederum eine Verbindung mit Azure SQL-Datenbank herstellt und das Dataset abruft. Die hohe Anzahl von Vorgängen hat mehrere potenzielle Fehlerquellen zur Folge. Überprüfen Sie im Falle eines Fehlers zunächst Folgendes:

+ Die Angabe der Suchdienst-Verbindungsinformationen beschränkt sich in diesem Tutorial auf den Dienstnamen. Wenn Sie die vollständige URL eingegeben haben, werden die Vorgänge bei der Indexerstellung mit einem Verbindungsfehler beendet.

+ Datenbank-Verbindungsinformationen in **appsettings.json**: Hierbei muss es sich um die ADO.NET-Verbindungszeichenfolge aus dem Portal handeln, die mit einem gültigen Benutzernamen und Kennwort für die Datenbank versehen wurde. Das Benutzerkonto muss über Datenabrufberechtigungen verfügen. Ihre lokale Client-IP-Adresse muss zugriffsberechtigt sein.

+ Ressourceneinschränkungen: Bedenken Sie, dass für den Free-Tarif eine Obergrenze von drei Indizes, Indexern und Datenquellen gilt. Ist die Obergrenze für einen Dienst erreicht, können keine neuen Objekte mehr erstellt werden.

## <a name="check-results"></a>Überprüfen der Ergebnisse

Vergewissern Sie sich im Azure-Portal, dass die Objekterstellung erfolgreich war, und verwenden Sie anschließend den **Suchexplorer**, um den Index abzufragen.

1. [Melden Sie sich beim Azure-Portal an](https://portal.azure.com/), und öffnen Sie auf der Seite **Übersicht** des Suchdiensts nacheinander die einzelnen Listen, um sich zu vergewissern, dass das Objekt erstellt wurde. **Indizes**, **Indexer** und **Datenquellen** enthalten „hotels“, „azure-sql-indexer“ bzw. „azure-sql“.

   ![Kachel „Indexer“ und „Datenquellen“](./media/search-indexer-tutorial/tiles-portal.png)

1. Wählen Sie den Index „hotels“ aus. Auf der Seite „hotels“ ist **Suchexplorer** die erste Registerkarte. 

1. Klicken Sie auf **Suchen**, um eine leere Abfrage auszugeben. 

   Die drei Einträge in Ihrem Index werden als JSON-Dokumente zurückgegeben. Der Suchexplorer gibt Dokumente im JSON-Format zurück, sodass Sie sich die gesamte Struktur ansehen können.

   ![Indexabfragen](./media/search-indexer-tutorial/portal-search.png "Abfragen von Indizes")
   
1. Geben Sie als Nächstes eine Suchzeichenfolge ein: `search=river&$count=true`. 

   Diese Abfrage führt eine Volltextsuche nach dem Begriff `river` durch, und das Ergebnis enthält die Anzahl passender Dokumente. Die Rückgabe der Anzahl passender Dokumente ist hilfreich in Testszenarien mit einem umfangreichen Index mit tausenden oder Millionen von Dokumenten. In diesem Fall entspricht der Abfrage lediglich ein einzelnes Dokument.

1. Geben Sie abschließend eine Suchzeichenfolge ein, die die JSON-Ausgabe auf relevante Felder beschränkt: `search=river&$count=true&$select=hotelId, baseRate, description`. 

   Die Abfrageantwort wird auf die ausgewählten Felder beschränkt, um eine präzisere Ausgabe zu erhalten.

## <a name="explore-the-code"></a>Untersuchen des Codes

Nachdem Sie nun wissen, was durch den Beispielcode erstellt wird, sehen wir uns als Nächstes den Code in der Projektmappe an. Der relevante Code befindet sich in zwei Dateien:

  + **hotel.cs** enthält ein Schema zum Definieren des Index.
  + **Program.cs** enthält Funktionen zum Erstellen und Verwalten von Strukturen in Ihrem Dienst.

### <a name="in-hotelcs"></a>hotel.cs

Das Indexschema definiert die Sammlung „fields“ einschließlich Attributen zur Angabe zulässiger Vorgänge (beispielsweise, ob in einem Feld eine Volltextsuche möglich ist oder ob es gefiltert oder sortiert werden kann), wie in der folgenden Felddefinition für „HotelName“ zu sehen. 

```csharp
. . . 
[IsSearchable, IsFilterable, IsSortable]
public string HotelName { get; set; }
. . .
```

Ein Schema kann auch andere Elemente enthalten. Hierzu zählen beispielsweise Bewertungsprofile zur Erhöhung einer Suchbewertung, benutzerdefinierte Analysen und andere Konstrukte. Für unsere Zwecke reicht jedoch eine einfache Schemadefinition, die lediglich Felder aus den Beispieldatasets enthält.

### <a name="in-programcs"></a>Program.cs

Das Hauptprogramm enthält Logik zum Erstellen eines Clients, eines Index, einer Datenquelle und eines Indexers. Der Code sucht und löscht vorhandene Ressourcen mit dem gleichen Namen (unter der Annahme, dass Sie dieses Programm ggf. mehrmals ausführen).

Das Datenquellenobjekt wird mit Einstellungen konfiguriert, die speziell für Ressourcen von Azure SQL-Datenbank gelten, z. B. [partielle oder inkrementelle Indizierung](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md#capture-new-changed-and-deleted-rows) zur Nutzung der integrierten [Features für die Erkennung von Änderungen](https://docs.microsoft.com/sql/relational-databases/track-changes/about-change-tracking-sql-server) von Azure SQL. Die Demodatenbank „hotels“ in Azure SQL enthält eine Spalte für Vorläufiges Löschen mit dem Namen **IsDeleted**. Wenn diese Spalte in der Datenbank auf „true“ festgelegt ist, entfernt der Indexer das entsprechende Dokument aus dem Azure Cognitive Search-Index.

  ```csharp
  Console.WriteLine("Creating data source...");

  DataSource dataSource = DataSource.AzureSql(
      name: "azure-sql",
      sqlConnectionString: configuration["AzureSQLConnectionString"],
      tableOrViewName: "hotels",
      deletionDetectionPolicy: new SoftDeleteColumnDeletionDetectionPolicy(
          softDeleteColumnName: "IsDeleted",
          softDeleteMarkerValue: "true"));
  dataSource.DataChangeDetectionPolicy = new SqlIntegratedChangeTrackingPolicy();

  searchService.DataSources.CreateOrUpdateAsync(dataSource).Wait();
  ```

Ein Indexerobjekt ist plattformagnostisch. Dies bedeutet, dass die Konfiguration, die Zeitplanung und das Aufrufen unabhängig von der Quelle gleich sind. Dieses Indexerbeispiel enthält einen Zeitplan und eine Zurücksetzungsoption, mit der der Verlauf des Indexers gelöscht wird. Außerdem wird eine Methode zum sofortigen Erstellen und Ausführen des Indexers aufgerufen.

  ```csharp
  Console.WriteLine("Creating Azure SQL indexer...");
  Indexer indexer = new Indexer(
      name: "azure-sql-indexer",
      dataSourceName: dataSource.Name,
      targetIndexName: index.Name,
      schedule: new IndexingSchedule(TimeSpan.FromDays(1)));
  // Indexers contain metadata about how much they have already indexed
  // If we already ran the sample, the indexer will remember that it already
  // indexed the sample data and not run again
  // To avoid this, reset the indexer if it exists
  exists = await searchService.Indexers.ExistsAsync(indexer.Name);
  if (exists)
  {
      await searchService.Indexers.ResetAsync(indexer.Name);
  }

  await searchService.Indexers.CreateOrUpdateAsync(indexer);

  // We created the indexer with a schedule, but we also
  // want to run it immediately
  Console.WriteLine("Running Azure SQL indexer...");

  try
  {
      await searchService.Indexers.RunAsync(indexer.Name);
  }
  catch (CloudException e) when (e.Response.StatusCode == (HttpStatusCode)429)
  {
      Console.WriteLine("Failed to run indexer: {0}", e.Response.Content);
  }
  ```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie in Ihrem eigenen Abonnement arbeiten, ist es ratsam, nach Abschluss eines Projekts die nicht mehr benötigten Ressourcen zu entfernen. Ressourcen, die weiterhin ausgeführt werden, können Sie Geld kosten. Sie können entweder einzelne Ressourcen oder aber die Ressourcengruppe löschen, um den gesamten Ressourcensatz zu entfernen.

Ressourcen können im Portal über den Link „Alle Ressourcen“ oder „Ressourcengruppen“ im linken Navigationsbereich gesucht und verwaltet werden.

Denken Sie bei Verwendung eines kostenlosen Diensts an die Beschränkung auf maximal drei Indizes, Indexer und Datenquellen. Sie können einzelne Elemente über das Portal löschen, um unter dem Limit zu bleiben.

## <a name="next-steps"></a>Nächste Schritte

In Azure Cognitive Search stehen Indexer für verschiedene Azure-Datenquellen zur Verfügung. Machen Sie sich als Nächstes mit dem Indexer für Azure Blob Storage vertraut.

> [!div class="nextstepaction"]
> [Indizieren von Dokumenten in Azure Blob Storage mit Azure Search](search-howto-indexing-azure-blob-storage.md)