---
title: 'Schnellstart: Verwenden von Java zum Erstellen einer Dokumentdatenbank mithilfe von Azure Cosmos DB'
description: Diese Schnellstartanleitung enthält ein Java-Codebeispiel, mit dem Sie eine Verbindung mit der SQL-API von Azure Cosmos DB herstellen und diese API abfragen können.
author: SnehaGunda
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: java
ms.topic: quickstart
ms.date: 10/31/2019
ms.author: sngun
ms.custom: seo-java-august2019, seo-java-september2019
ms.openlocfilehash: 8c2ae82bae8457a1c715f160994c7a0da94193ff
ms.sourcegitcommit: f718b98dfe37fc6599d3a2de3d70c168e29d5156
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/11/2020
ms.locfileid: "77134494"
---
# <a name="quickstart-build-a-java-app-to-manage-azure-cosmos-db-sql-api-data"></a>Schnellstart: Erstellen einer Java-App zum Verwalten von Azure Cosmos DB-SQL-API-Daten


> [!div class="op_single_selector"]
> * [.NET V3](create-sql-api-dotnet.md)
> * [.NET V4](create-sql-api-dotnet-V4.md)
> * [Java](create-sql-api-java.md)
> * [Node.js](create-sql-api-nodejs.md)
> * [Python](create-sql-api-python.md)
> * [Xamarin](create-sql-api-xamarin-dotnet.md)

In dieser Schnellstartanleitung erstellen und verwalten Sie ein Azure Cosmos DB-SQL-API-Konto im Azure-Portal mithilfe einer über GitHub geklonten Java-App. Sie erstellen zunächst über das Azure-Portal ein Azure Cosmos DB-SQL-API-Konto und anschließend mithilfe des SQL Java SDK eine Java-App. Dann fügen Sie mithilfe der Java-Anwendung dem Cosmos DB-Konto Ressourcen hinzu. Azure Cosmos DB ist ein Multimodell-Datenbankdienst, mit dem Sie mithilfe der Funktionen für globale Verteilung und horizontale Skalierung schnell Dokument-, Tabellen-, Schlüssel-Wert- und Graph-Datenbanken erstellen und abfragen können.

## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure-Konto mit einem aktiven Abonnement. [Erstellen Sie ein kostenloses Konto.](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) Oder [testen Sie Azure Cosmos DB kostenlos](https://azure.microsoft.com/try/cosmosdb/) ohne ein Azure-Abonnement. Sie können auch den [Azure Cosmos DB-Emulator](https://aka.ms/cosmosdb-emulator) mit dem URI `https://localhost:8081` und dem Schlüssel `C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==` verwenden.
- [Java Development Kit (JDK) 8](https://www.azul.com/downloads/azure-only/zulu/?&version=java-8-lts&architecture=x86-64-bit&package=jdk) Die Umgebungsvariable `JAVA_HOME` muss auf den Ordner verweisen, in dem das JDK installiert ist.
- Ein [binäres Maven-Archiv](https://maven.apache.org/download.cgi). Führen Sie unter Ubuntu `apt-get install maven` aus, um Maven zu installieren.
- [Git](https://www.git-scm.com/downloads). Führen Sie unter Ubuntu `sudo apt-get install git` aus, um Git zu installieren.

## <a name="create-a-database-account"></a>Erstellen eines Datenbankkontos

Vor dem Erstellen einer Dokumentdatenbank müssen Sie ein SQL-API-Konto mit Azure Cosmos DB erstellen.

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a name="add-a-container"></a>Hinzufügen eines Containers

[!INCLUDE [cosmos-db-create-collection](../../includes/cosmos-db-create-collection.md)]

<a id="add-sample-data"></a>
## <a name="add-sample-data"></a>Hinzufügen von Beispieldaten

[!INCLUDE [cosmos-db-create-sql-api-add-sample-data](../../includes/cosmos-db-create-sql-api-add-sample-data.md)]

## <a name="query-your-data"></a>Abfragen Ihrer Daten

[!INCLUDE [cosmos-db-create-sql-api-query-data](../../includes/cosmos-db-create-sql-api-query-data.md)]

## <a name="clone-the-sample-application"></a>Klonen der Beispielanwendung

Beginnen wir nun mit der Verwendung von Code. Klonen Sie zunächst eine SQL-API-App aus GitHub, legen Sie die Verbindungszeichenfolge fest, und führen Sie sie aus. Sie werden feststellen, wie einfach Sie programmgesteuert mit Daten arbeiten können. 

Führen Sie den folgenden Befehl aus, um das Beispielrepository zu klonen. Dieser Befehl erstellt eine Kopie der Beispiel-App auf Ihrem Computer.

```bash
git clone https://github.com/Azure-Samples/azure-cosmos-java-getting-started.git
```

## <a name="review-the-code"></a>Überprüfen des Codes

Dieser Schritt ist optional. Wenn Sie erfahren möchten, wie die Datenbankressourcen im Code erstellt werden, können Sie sich die folgenden Codeausschnitte ansehen. Andernfalls können Sie mit [Ausführen der App](#run-the-app) fortfahren. 

* `CosmosClient`-Initialisierung `CosmosClient` bietet die clientseitige logische Darstellung für den Azure Cosmos-Datenbankdienst. Mit diesem Client werden Anforderungen für den Dienst konfiguriert und ausgeführt.
    
    [!code-java[](~/azure-cosmosdb-java-v4-getting-started/src/main/java/com/azure/cosmos/sample/sync/SyncMain.java?name=CreateSyncClient)]

* Erstellung einer Cosmos-Datenbank:

    [!code-java[](~/azure-cosmosdb-java-v4-getting-started/src/main/java/com/azure/cosmos/sample/sync/SyncMain.java?name=CreateDatabaseIfNotExists)]

* Erstellung eines Cosmos-Containers:

    [!code-java[](~/azure-cosmosdb-java-v4-getting-started/src/main/java/com/azure/cosmos/sample/sync/SyncMain.java?name=CreateContainerIfNotExists)]

* Erstellung eines Elements mithilfe der Methode `createItem`:

    [!code-java[](~/azure-cosmosdb-java-v4-getting-started/src/main/java/com/azure/cosmos/sample/sync/SyncMain.java?name=CreateItem)]
   
* Punktlesevorgänge werden mithilfe der Methoden `getItem` und `read` ausgeführt:

    [!code-java[](~/azure-cosmosdb-java-v4-getting-started/src/main/java/com/azure/cosmos/sample/sync/SyncMain.java?name=ReadItem)]

* SQL-Abfragen über JSON erfolgen mithilfe der `queryItems`-Methode:

    [!code-java[](~/azure-cosmosdb-java-v4-getting-started/src/main/java/com/azure/cosmos/sample/sync/SyncMain.java?name=QueryItems)]

## <a name="run-the-app"></a>Ausführen der App

Wechseln Sie nun zurück zum Azure-Portal, um die Informationen zur Verbindungszeichenfolge abzurufen und die App mit den Endpunktinformationen zu starten. Dadurch kann Ihre App mit Ihrer gehosteten Datenbank kommunizieren.

1. Wechseln Sie im Git-Terminalfenster mithilfe von `cd` in den Ordner mit dem Beispielcode.

    ```bash
    cd azure-cosmos-java-getting-started
    ```

2. Verwenden Sie im Git-Terminalfenster den folgenden Befehl, um die erforderlichen Java-Pakete zu installieren.

    ```bash
    mvn package
    ```

3. Starten Sie im Git-Terminalfenster die Java-Anwendung mithilfe des folgenden Befehls. (Ersetzen Sie YOUR_COSMOS_DB_HOSTNAME durch den in Anführungszeichen gesetzten URI-Wert aus dem Portal und YOUR_COSMOS_DB_MASTER_KEY durch den in Anführungszeichen gesetzten Primärschlüssel aus dem Portal.)

    ```bash
    mvn exec:java -DACCOUNT_HOST=YOUR_COSMOS_DB_HOSTNAME -DACCOUNT_KEY=YOUR_COSMOS_DB_MASTER_KEY

    ```

    Im Terminalfenster wird eine Benachrichtigung angezeigt, dass die FamilyDB-Datenbank erstellt wurde. 
    
4. Die App erstellt eine Datenbank mit dem Namen `AzureSampleFamilyDB`.
5. Die App erstellt einen Container mit dem Namen `FamilyContainer`.
6. Die App führt Punktlesevorgänge mithilfe der Objekt-IDs und des Partitionsschlüsselwerts aus (in unserem Beispiel „lastName“). 
7. Die App fragt Elemente ab, um alle Familien mit dem entsprechenden Nachnamen („Andersen“, „Wakefield“, „Johnson“) abzurufen.

7. Die App löscht die erstellten Ressourcen nicht. Gehen Sie zurück zum Portal, um in Ihrem Konto [die Ressourcen zu löschen](#clean-up-resources),  damit keine Gebühren anfallen.

## <a name="review-slas-in-the-azure-portal"></a>Überprüfen von SLAs im Azure-Portal

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

[!INCLUDE [cosmosdb-delete-resource-group](../../includes/cosmos-db-delete-resource-group.md)]

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie gelernt, wie Sie ein Azure Cosmos DB-SQL-API-Konto erstellen, eine Dokumentdatenbank und einen Container mit dem Daten-Explorer erstellen und eine Java-App ausführen, um das gleiche Ergebnis programmgesteuert zu erreichen. Jetzt können Sie zusätzliche Daten in Ihr Azure Cosmos DB-Konto importieren. 

> [!div class="nextstepaction"]
> [Import data into Azure Cosmos DB (Importieren von Daten in Azure Cosmos DB)](import-data.md)
