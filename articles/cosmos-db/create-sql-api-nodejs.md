---
title: 'Schnellstart: Abfragen von Daten aus einem Azure Cosmos DB-SQL-API-Konto mithilfe von Node.js'
description: Hier erfahren Sie, wie Sie mithilfe von Node.js eine App erstellen, die eine Verbindung mit einem Azure Cosmos DB-SQL-API-Konto herstellt und Daten abfragt.
author: deborahc
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: nodejs
ms.topic: quickstart
ms.date: 11/19/2019
ms.author: dech
ms.openlocfilehash: 8df78df27ffb7e8bb8fc88567bd0b3d37be20488
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2020
ms.locfileid: "76719499"
---
# <a name="quickstart-use-nodejs-to-connect-and-query-data-from-azure-cosmos-db-sql-api-account"></a>Schnellstart: Verwenden von Node.js zum Herstellen einer Verbindung mit einem und Abfragen von Daten aus einem Azure Cosmos DB-SQL-API-Konto

> [!div class="op_single_selector"]
> * [.NET V3](create-sql-api-dotnet.md)
> * [.NET V4](create-sql-api-dotnet-V4.md)
> * [Java](create-sql-api-java.md)
> * [Node.js](create-sql-api-nodejs.md)
> * [Python](create-sql-api-python.md)
> * [Xamarin](create-sql-api-xamarin-dotnet.md)

In dieser Schnellstartanleitung wird gezeigt, wie Sie eine Node.js-App verwenden, um eine Verbindung mit dem [SQL-API](sql-api-introduction.md)-Konto in Azure Cosmos DB herzustellen. Anschließend können Sie Azure Cosmos DB-SQL-Abfragen verwenden, um Daten abzufragen und zu verwalten. Die Node.js-App, die Sie in diesem Artikel erstellen, verwendet das [SQL JavaScript SDK](sql-api-sdk-node.md). In diesem Schnellstart wird Version 2.0 des [JavaScript SDK](https://www.npmjs.com/package/@azure/cosmos) verwendet.

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)] 
[!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

* Berücksichtigen Sie zudem Folgendes:
    * [Node.js](https://nodejs.org/en/) Version v6.0.0 oder höher
    * [Git-Client](https://git-scm.com/)

## <a name="create-a-database"></a>Erstellen einer Datenbank 

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a name="add-a-container"></a>Hinzufügen eines Containers

[!INCLUDE [cosmos-db-create-collection](../../includes/cosmos-db-create-collection.md)]

## <a name="add-sample-data"></a>Hinzufügen von Beispieldaten

[!INCLUDE [cosmos-db-create-sql-api-add-sample-data](../../includes/cosmos-db-create-sql-api-add-sample-data.md)]

## <a name="query-your-data"></a>Abfragen Ihrer Daten

[!INCLUDE [cosmos-db-create-sql-api-query-data](../../includes/cosmos-db-create-sql-api-query-data.md)]

## <a name="clone-the-sample-application"></a>Klonen der Beispielanwendung

Klonen Sie zunächst eine Node.js-App aus GitHub, legen Sie die Verbindungszeichenfolge fest, und führen Sie sie aus.

1. Öffnen Sie eine Eingabeaufforderung, erstellen Sie einen neuen Ordner namens „git-samples“, und schließen Sie die Eingabeaufforderung.

    ```bash
    md "C:\git-samples"
    ```

2. Öffnen Sie ein Git-Terminalfenster (z.B. git bash), und verwenden Sie den Befehl `cd`, um in den neuen Ordner zu gelangen und dort die Beispiel-App zu installieren.

    ```bash
    cd "C:\git-samples"
    ```

3. Führen Sie den folgenden Befehl aus, um das Beispielrepository zu klonen. Dieser Befehl erstellt eine Kopie der Beispiel-App auf Ihrem Computer.

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-sql-api-nodejs-getting-started.git
    ```

## <a name="review-the-code"></a>Überprüfen des Codes

Dieser Schritt ist optional. Wenn Sie erfahren möchten, wie die Azure Cosmos-Datenbankressourcen im Code erstellt werden, können Sie sich die folgenden Codeausschnitte ansehen. Andernfalls können Sie mit [Aktualisieren der Verbindungszeichenfolge](#update-your-connection-string) fortfahren. 

Hinweis: Wenn Sie mit der vorherigen Version des JavaScript SDK vertraut sind, kennen Sie unter Umständen bereits die Begriffe „Sammlung“ und „Dokument“. Azure Cosmos DB unterstützt [mehrere API-Modelle](https://docs.microsoft.com/azure/cosmos-db/introduction). Daher werden in Version 2.0 und höheren Versionen des JavaScript SDK die generischen Begriffe „Container“ (für eine Sammlung, einen Graph oder eine Tabelle) und „Element“ zum Beschreiben des Containerinhalts verwendet.

Die folgenden Codeausschnitte stammen alle aus der Datei **app.js**.

* Das Objekt `CosmosClient` ist initialisiert.

    ```javascript
    const client = new CosmosClient({ endpoint, key });
    ```

* Erstellen Sie eine neue Azure Cosmos-Datenbank.

    ```javascript
    const { database } = await client.databases.createIfNotExists({ id: databaseId });
    ```

* Ein neuer Container (Sammlung) wird in der Datenbank erstellt.

    ```javascript
    const { container } = await client.database(databaseId).containers.createIfNotExists({ id: containerId });
    ```

* Ein Element (Dokument) wird erstellt.

    ```javascript
    const { item } = await client.database(databaseId).container(containerId).items.create(itemBody);
    ```

* In der Familiendatenbank wird eine SQL-Abfrage über JSON ausgeführt. Die Abfrage gibt alle Kinder der Familie Anderson zurück. 

    ```javascript
      const querySpec = {
        query: 'SELECT VALUE r.children FROM root r WHERE r.lastName = @lastName',
        parameters: [
          {
            name: '@lastName',
            value: 'Andersen'
          }
        ]
      }

      const { resources: results } = await client
        .database(databaseId)
        .container(containerId)
        .items.query(querySpec)
        .fetchAll()
      for (var queryResult of results) {
        let resultString = JSON.stringify(queryResult)
        console.log(`\tQuery returned ${resultString}\n`)
      }
    ```    

## <a name="update-your-connection-string"></a>Aktualisieren der Verbindungszeichenfolge

Wechseln Sie nun zurück zum Azure-Portal, um die Details der Verbindungszeichenfolge Ihres Azure Cosmos-Kontos abzurufen. Kopieren Sie die Verbindungszeichenfolge in die App, damit sie eine Verbindung mit der Datenbank herstellen kann.

1. Klicken Sie im [Azure-Portal](https://portal.azure.com/) in Ihrem Azure Cosmos-Konto im linken Navigationsbereich auf **Schlüssel**, und klicken Sie anschließend auf **Lese-/Schreibschlüssel**. Mithilfe der Schaltflächen zum Kopieren auf der rechten Seite des Bildschirms kopieren Sie im nächsten Schritt den URI und den Primärschlüssel in die Datei `config.js`.

    ![Anzeigen und Kopieren eines Zugriffsschlüssels im Azure-Portal auf dem Blatt „Schlüssel“](./media/create-sql-api-dotnet/keys.png)

2. Öffnen Sie die Datei `config.js`. 

3. Kopieren Sie den URI-Wert aus dem Portal (mithilfe der Schaltfläche zum Kopieren), und legen Sie ihn in `config.js` als Wert des Endpunktschlüssels fest. 

    `config.endpoint = "<Your Azure Cosmos account URI>"`

4. Kopieren Sie anschließend den Wert für PRIMARY KEY aus dem Portal, und legen Sie ihn als Wert von `config.key` in `config.js` fest. Sie haben die App nun mit allen erforderlichen Informationen für die Kommunikation mit Azure Cosmos DB aktualisiert. 

    `config.key = "<Your Azure Cosmos account key>"`
    
## <a name="run-the-app"></a>Ausführen der App

1. Führen Sie `npm install` in einem Terminal aus, um erforderliche NPM-Module zu installieren

2. Führen Sie `node app.js` in einem Terminal aus, um Ihre Node-Anwendung zu starten.

Jetzt können Sie zum Daten-Explorer zurückkehren, um diese neuen Daten anzupassen und mit ihnen zu arbeiten.

## <a name="review-slas-in-the-azure-portal"></a>Überprüfen von SLAs im Azure-Portal

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

[!INCLUDE [cosmosdb-delete-resource-group](../../includes/cosmos-db-delete-resource-group.md)]

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie gelernt, wie Sie ein Azure Cosmos-Konto erstellen, einen Container mit dem Daten-Explorer erstellen und eine App ausführen. Jetzt können Sie zusätzliche Daten in Ihre Azure Cosmos-Datenbank importieren. 

> [!div class="nextstepaction"]
> [Import data into Azure Cosmos DB (Importieren von Daten in Azure Cosmos DB)](import-data.md)


