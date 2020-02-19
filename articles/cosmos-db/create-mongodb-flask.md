---
title: Erstellen einer Python Flask-Web-App mit der API für MongoDB von Azure Cosmos DB
description: Stellt ein Python Flask-Codebeispiel vor, mit dem Sie eine Verbindung herstellen und Abfragen über die API für MongoDB von Azure Cosmos DB durchführen können.
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.devlang: python
ms.topic: quickstart
ms.date: 12/26/2018
ms.openlocfilehash: 43f4cf7e4008aa01a26c48a8e99f7465eeeb234b
ms.sourcegitcommit: db2d402883035150f4f89d94ef79219b1604c5ba
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/07/2020
ms.locfileid: "77061742"
---
# <a name="quickstart-build-a-python-app-using-azure-cosmos-dbs-api-for-mongodb"></a>Schnellstart: Erstellen einer Python-App mit der API für MongoDB von Azure Cosmos DB

> [!div class="op_single_selector"]
> * [.NET](create-mongodb-dotnet.md)
> * [Java](create-mongodb-java.md)
> * [Node.js](create-mongodb-nodejs.md)
> * [Python](create-mongodb-flask.md)
> * [Xamarin](create-mongodb-xamarin.md)
> * [Golang](create-mongodb-golang.md)
>  

In dieser Schnellstartanleitung verwenden Sie ein Azure Cosmos DB-Konto für die Mongo DB-API oder den Azure Cosmos DB-Emulator, um eine über GitHub geklonte Python Flask-To-Do-Web-App auszuführen. Azure Cosmos DB ist ein Multimodell-Datenbankdienst, mit dem Sie mithilfe der Funktionen für globale Verteilung und horizontale Skalierung schnell Dokument-, Tabellen-, Schlüssel-Wert- und Graph-Datenbanken erstellen und abfragen können.

## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure-Konto mit einem aktiven Abonnement. [Erstellen Sie ein kostenloses Konto.](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) Oder [testen Sie Azure Cosmos DB kostenlos](https://azure.microsoft.com/try/cosmosdb/) ohne ein Azure-Abonnement. Alternativ dazu können Sie den [Azure Cosmos DB-Emulator](local-emulator.md) verwenden. 
- [Python 3.6+](https://www.python.org/downloads/)
- [Visual Studio Code](https://code.visualstudio.com/Download) mit der [Python-Erweiterung](https://marketplace.visualstudio.com/items?itemName=donjayamanne.python)

## <a name="clone-the-sample-application"></a>Klonen der Beispielanwendung

Klonen Sie als Nächstes eine Flask-MongoDB-App aus GitHub, legen Sie die Verbindungszeichenfolge fest, und führen Sie die App aus. Sie werden feststellen, wie einfach Sie programmgesteuert mit Daten arbeiten können.

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
    git clone https://github.com/Azure-Samples/CosmosDB-Flask-Mongo-Sample.git
    ```
3. Führen Sie den folgenden Befehl aus, um die Python-Module zu installieren.

    ```bash 
    pip install -r .\requirements.txt
    ```
4. Öffnen Sie den Ordner in Visual Studio Code.

## <a name="review-the-code"></a>Überprüfen des Codes

Dieser Schritt ist optional. Wenn Sie erfahren möchten, wie die Datenbankressourcen im Code erstellt werden, können Sie sich die folgenden Codeausschnitte ansehen. Andernfalls können Sie mit [Ausführen der Web-App](#run-the-web-app) fortfahren. 

Die folgenden Codeausschnitte stammen alle aus der Datei *app.py* und verwenden die Verbindungszeichenfolge für den lokalen Azure Cosmos DB-Emulator. Das Kennwort muss wie weiter unten dargestellt aufgeteilt werden, da die Schrägstriche ansonsten nicht analysiert werden können.

* Initialisieren Sie den MongoDB-Client, rufen Sie die Datenbank ab, und führen Sie die Authentifizierung durch.

    ```python
    client = MongoClient("mongodb://127.0.0.1:10250/?ssl=true") #host uri
    db = client.test    #Select the database
    db.authenticate(name="localhost",password='C2y6yDjf5' + r'/R' + '+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw' + r'/Jw==')
    ```

* Rufen Sie die Sammlung ab, oder erstellen Sie sie, falls sie noch nicht vorhanden ist.

    ```python
    todos = db.todo #Select the collection
    ```

* Erstellen der App

    ```Python
    app = Flask(__name__)
    title = "TODO with Flask"
    heading = "ToDo Reminder"
    ```
    
## <a name="run-the-web-app"></a>Ausführen der Web-App

1. Vergewissern Sie sich, dass der Azure Cosmos DB-Emulator ausgeführt wird.

2. Öffnen Sie ein Terminalfenster, und wechseln Sie mithilfe von `cd` zu dem Verzeichnis, in dem die App gespeichert ist.

3. Legen Sie die Umgebungsvariable für die Flask-App mit `set FLASK_APP=app.py` bzw. mit `$env:FLASK_APP = app.py` für PowerShell-Editoren oder mit `export FLASK_APP=app.py` fest, wenn Sie einen Mac verwenden. 

4. Führen Sie die App mit `flask run` aus, und navigieren Sie zu *http:\//127.0.0.1:5000/* .

5. Fügen Sie Aufgaben hinzu, entfernen Sie Aufgaben, und verfolgen Sie, wie sie der Sammlung hinzugefügt bzw. darin geändert werden.

## <a name="create-a-database-account"></a>Erstellen eines Datenbankkontos

Wenn Sie den Code mit einem aktiven Azure Cosmos DB-Konto testen möchten, erstellen Sie über das Azure-Portal ein Konto.

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount-mongodb.md)]

## <a name="update-your-connection-string"></a>Aktualisieren der Verbindungszeichenfolge

Wenn Sie den Code mit einem aktiven Azure Cosmos DB-Konto testen möchten, rufen Sie die Informationen zur Verbindungszeichenfolge ab. Kopieren Sie sie anschließend in die App.

1. Wählen Sie in Ihrem Azure Cosmos DB-Konto im Azure-Portal im linken Navigationsbereich die Option **Verbindungszeichenfolge** und anschließend **Lese-/Schreibschlüssel** aus. Sie verwenden die Schaltflächen zum Kopieren auf der rechten Seite des Bildschirms, um den Benutzernamen, die Verbindungszeichenfolge und das Kennwort zu kopieren. 

2. Öffnen Sie die Datei *app.py* im Stammverzeichnis.

3. Kopieren Sie den Wert für **Benutzername** aus dem Portal (mithilfe der Schaltfläche zum Kopieren), und legen Sie ihn in der Datei *app.py* als Wert für **Name** fest.

4. Kopieren Sie dann den Wert Ihrer **Verbindungszeichenfolge** aus dem Portal, und legen Sie ihn in der Datei *app.py* als **MongoClient**-Wert fest.

5. Kopieren Sie schließlich das **Kennwort** aus dem Portal, und legen Sie es in der Datei *app.py* als **Kennwort** fest.

Sie haben die App nun mit allen erforderlichen Informationen für die Kommunikation mit Azure Cosmos DB aktualisiert. Sie können sie nun auf die gleiche Weise ausführen wie zuvor.

## <a name="deploy-to-azure"></a>Bereitstellen in Azure

Zum Bereitstellen dieser App können Sie in Azure eine neue Web-App erstellen und Continuous Deployment mit einer Verzweigung dieses GitHub-Repositorys aktivieren. Eine Anleitung zum Einrichten von Continuous Deployment mit GitHub in Azure finden Sie in [diesem Tutorial](https://docs.microsoft.com/azure/app-service/deploy-continuous-deployment).

Entfernen Sie beim Bereitstellen in Azure Ihre Anwendungsschlüssel, und stellen Sie sicher, dass der folgende Abschnitt auskommentiert ist:

```python
    client = MongoClient(os.getenv("MONGOURL"))
    db = client.test    #Select the database
    db.authenticate(name=os.getenv("MONGO_USERNAME"),password=os.getenv("MONGO_PASSWORD"))
```

Fügen Sie anschließend „MONGOURL“, „MONGO_PASSWORD“ und „MONGO_USERNAME“ den Anwendungseinstellungen hinzu. Weitere Informationen zu Anwendungseinstellungen in Azure-Web-Apps finden Sie in [diesem Tutorial](https://docs.microsoft.com/azure/app-service/configure-common#configure-app-settings).

Wenn Sie keinen Fork dieses Repositorys erstellen möchten, können Sie weiter unten auch die Schaltfläche **Bereitstellung in Azure** auswählen. Richten Sie anschließend in Azure die Anwendungseinstellungen mit Ihren Azure Cosmos DB-Kontoinformationen ein.

<a href="https://deploy.azure.com/?repository=https://github.com/heatherbshapiro/To-Do-List---Flask-MongoDB-Example" target="_blank">
<img src="https://azuredeploy.net/deploybutton.png" alt="Click to Deploy to Azure">
</a>

> [!NOTE]
> Wenn Sie Ihren Code in GitHub speichern oder andere Quellcodeverwaltungsoptionen verwenden möchten, entfernen Sie vorher unbedingt Ihre Verbindungszeichenfolgen aus dem Code. Sie können stattdessen mit Anwendungseinstellungen für die Web-App festgelegt werden.

## <a name="review-slas-in-the-azure-portal"></a>Überprüfen von SLAs im Azure-Portal

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

[!INCLUDE [cosmosdb-delete-resource-group](../../includes/cosmos-db-delete-resource-group.md)]

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie erfahren, wie Sie ein Azure Cosmos DB-Konto für die Mongo DB-API erstellen und den Azure Cosmos DB-Emulator zum Ausführen einer über GitHub geklonten Python Flask-To-Do-Web-App verwenden. Jetzt können Sie zusätzliche Daten in Ihr Azure Cosmos DB-Konto importieren. 

> [!div class="nextstepaction"]
> [Import MongoDB data into Azure Cosmos DB (Importieren von Daten aus MongoDB in Azure Cosmos DB)](mongodb-migrate.md)
