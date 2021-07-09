---
title: Erstellen einer Web-App mit der API für MongoDB von Azure Cosmos DB und dem .NET SDK
description: Stellt ein .NET-Codebeispiel vor, mit dem Sie eine Verbindung herstellen und Abfragen über die API für MongoDB von Azure Cosmos DB durchführen können.
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.devlang: dotnet
ms.topic: quickstart
ms.date: 10/15/2020
ms.custom: devx-track-csharp
ms.openlocfilehash: 387455f170d64c78ba4e1aab700a91a81bef2ed0
ms.sourcegitcommit: f3b930eeacdaebe5a5f25471bc10014a36e52e5e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/16/2021
ms.locfileid: "112235579"
---
# <a name="quickstart-build-a-net-web-app-using-azure-cosmos-dbs-api-for-mongodb"></a>Schnellstart: Erstellen einer .NET-Web-App mit der API für MongoDB von Azure Cosmos DB 
[!INCLUDE[appliesto-mongodb-api](includes/appliesto-mongodb-api.md)]

> [!div class="op_single_selector"]
> * [.NET](create-mongodb-dotnet.md)
> * [Java](create-mongodb-java.md)
> * [Node.js](create-mongodb-nodejs.md)
> * [Xamarin](create-mongodb-xamarin.md)
> * [Golang](create-mongodb-go.md)
>  

Azure Cosmos DB ist die schnelle NoSQL-Datenbank von Microsoft mit offenen APIs für jede Größenordnung. Sie können schnell Dokument-, Schlüssel-Wert- und Graph-Datenbanken erstellen und abfragen und dabei stets die Vorteile der globalen Verteilung und der horizontalen Skalierung nutzen, die Cosmos DB zugrunde liegen. 

In diesem Schnellstart wird veranschaulicht, wie ein Cosmos-Konto mit der [API für MongoDB von Azure Cosmos DB](mongodb-introduction.md) erstellt wird. Anschließend erstellen Sie eine Web-App für Aufgabenlisten mit dem [MongoDB .NET-Treiber](https://docs.mongodb.com/ecosystem/drivers/csharp/) und stellen diese bereit.

## <a name="prerequisites-to-run-the-sample-app"></a>Voraussetzungen für das Ausführen der Beispiel-App

* [Visual Studio](https://www.visualstudio.com/downloads/)
* Ein Azure Cosmos DB-Konto.

Falls Sie noch nicht über Visual Studio verfügen, laden Sie [Visual Studio 2019 Community Edition](https://www.visualstudio.com/downloads/) mit der beim Setup installierten Workload **ASP.NET und Webentwicklung** herunter.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)] 

<a id="create-account"></a>
## <a name="create-a-database-account"></a>Erstellen eines Datenbankkontos

[!INCLUDE [cosmos-db-create-dbaccount](includes/cosmos-db-create-dbaccount-mongodb.md)]

Das in diesem Artikel beschriebene Beispiel ist mit MongoDB.Driver-Version 2.6.1 kompatibel.

## <a name="clone-the-sample-app"></a>Klonen der Beispiel-App

Führen Sie die folgenden Befehle in einem GitHub-fähigen Befehlsfenster wie [Git bash](https://git-scm.com/downloads) aus:

```bash
mkdir "C:\git-samples"
cd "C:\git-samples"
git clone https://github.com/Azure-Samples/azure-cosmos-db-mongodb-dotnet-getting-started.git
```

Die obenstehenden Befehle haben folgende Konsequenzen:

1. Erstellen Sie für das Beispiel das Verzeichnis *C:\git-samples*. Wählen Sie einen für Ihr Betriebssystem geeigneten Ordner aus.
1. Ändern Sie Ihr aktuelles Verzeichnis in den Ordner *C:\git-samples*.
1. Klonen Sie das Beispiel in den Ordner *C:\git-samples*.

Alternativ zur Verwendung von Git können Sie auch [das Projekt als ZIP-Datei herunterladen](https://github.com/Azure-Samples/azure-cosmos-db-mongodb-dotnet-getting-started/archive/master.zip).

## <a name="review-the-code"></a>Überprüfen des Codes

1. Klicken Sie in Visual Studio mit der rechten Maustaste auf das Projekt im **Projektmappen-Explorer**, und klicken Sie anschließend auf **NuGet-Pakete verwalten**.
1. Geben Sie im NuGet-Feld **Durchsuchen** den Suchbegriff *MongoDB.Driver* ein.
1. Installieren Sie die Bibliothek **MongoDB.Driver** mithilfe der Ergebnisse. Dadurch wird das Paket „MongoDB.Driver“ mit all seinen Abhängigkeiten installiert.

Die folgenden Schritte sind optional. Wenn Sie erfahren möchten, wie die Datenbankressourcen im Code erstellt werden, sehen Sie sich die folgenden Codeausschnitte an. Fahren Sie andernfalls mit dem [Aktualisieren der Verbindungszeichenfolge](#update-the-connection-string) fort.

Die folgenden Codeausschnitte stammen aus der Datei *DAL/Dal.cs*.

* Der folgende Code dient zum Initialisieren des Clients:

    ```cs
        MongoClientSettings settings = new MongoClientSettings();
        settings.Server = new MongoServerAddress(host, 10255);
        settings.UseSsl = true;
        settings.SslSettings = new SslSettings();
        settings.SslSettings.EnabledSslProtocols = SslProtocols.Tls12;

        MongoIdentity identity = new MongoInternalIdentity(dbName, userName);
        MongoIdentityEvidence evidence = new PasswordEvidence(password);

        settings.Credential = new MongoCredential("SCRAM-SHA-1", identity, evidence);

        MongoClient client = new MongoClient(settings);
    ```

* Der folgende Code ruft die Datenbank und Sammlung ab:

    ```cs
    private string dbName = "Tasks";
    private string collectionName = "TasksList";

    var database = client.GetDatabase(dbName);
    var todoTaskCollection = database.GetCollection<MyTask>(collectionName);
    ```

* Der folgende Code ruft alle Dokumente ab:

    ```cs
    collection.Find(new BsonDocument()).ToList();
    ```

Der folgende Code erstellt eine Aufgabe und fügt sie in die Sammlung ein:

   ```csharp
    public void CreateTask(MyTask task)
    {
        var collection = GetTasksCollectionForEdit();
        try
        {
            collection.InsertOne(task);
        }
        catch (MongoCommandException ex)
        {
            string msg = ex.Message;
        }
    }
   ```
   Ebenso können Sie Dokumente mit den Methoden [collection.UpdateOne()](https://docs.mongodb.com/stitch/mongodb/actions/collection.updateOne/index.html) und [collection.DeleteOne()](https://docs.mongodb.com/stitch/mongodb/actions/collection.deleteOne/index.html) aktualisieren und löschen.

## <a name="update-the-connection-string"></a>Aktualisieren der Verbindungszeichenfolge

Kopieren Sie im Azure-Portal die Informationen der Verbindungszeichenfolge:

1. Wählen Sie im [Azure-Portal](https://portal.azure.com/) in Ihrem Cosmos-Konto im linken Navigationsbereich **Verbindungszeichenfolge** aus, und klicken Sie anschließend auf **Lese-/Schreibschlüssel**. Kopieren Sie im nächsten Schritt mithilfe der Schaltflächen zum Kopieren auf der rechten Seite des Bildschirms den Benutzernamen, das Kennwort sowie den Host in die Datei „dal.cs“.

2. Öffnen Sie die Datei *DAL/Dal.cs*.

3. Kopieren Sie den Wert des **Benutzernamens** aus dem Portal (mithilfe der Schaltfläche zum Kopieren), und legen Sie ihn in der Datei **Dal.cs** als Wert des **Benutzernamens** fest.

4. Kopieren Sie den Wert des **Hosts** aus dem Portal, und legen Sie ihn in der Datei **Dal.cs** als Wert des **Hosts** fest.

5. Kopieren Sie den Wert des **Kennworts** aus dem Portal, und legen Sie ihn in der Datei **Dal.cs** als Wert des **Kennworts** fest.

<!-- TODO Store PW correctly-->
> [!WARNING]
> Checken Sie Kennwörter oder andere sensible Daten niemals in den Quellcode ein.

Sie haben die App nun mit allen erforderlichen Informationen für die Kommunikation mit Cosmos DB aktualisiert.

## <a name="run-the-web-app"></a>Ausführen der Web-App

1. Drücken Sie STRG+F5, um die Anwendung auszuführen. Der Standardbrowser wird mit der App gestartet. 
1. Klicken Sie im Browser auf **Erstellen**, und erstellen Sie einige Aufgaben in Ihrer Aufgabenlisten-App.

<!-- 
## Deploy the app to Azure 
1. In VS, right click .. publish
2. This is so easy, why is this critical step missed?
-->
## <a name="review-slas-in-the-azure-portal"></a>Überprüfen von SLAs im Azure-Portal

[!INCLUDE [cosmosdb-tutorial-review-slas](includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

[!INCLUDE [cosmosdb-delete-resource-group](includes/cosmos-db-delete-resource-group.md)]

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie gelernt, wie Sie ein Cosmos-Konto erstellen, eine Sammlung erstellen und eine Konsolen-App ausführen. Jetzt können Sie zusätzliche Daten in Ihre Cosmos-Datenbank importieren. 

> [!div class="nextstepaction"]
> [Import MongoDB data into Azure Cosmos DB (Importieren von Daten aus MongoDB in Azure Cosmos DB)](../dms/tutorial-mongodb-cosmos-db.md?toc=%2fazure%2fcosmos-db%2ftoc.json%253ftoc%253d%2fazure%2fcosmos-db%2ftoc.json)
