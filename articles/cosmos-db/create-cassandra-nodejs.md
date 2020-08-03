---
title: 'Schnellstart: Cassandra-API mit Node.js – Azure Cosmos DB'
description: In dieser Schnellstartanleitung erfahren Sie, wie Sie mithilfe der Cassandra-API von Azure Cosmos DB eine Profilanwendung mit Node.js erstellen.
author: TheovanKraay
ms.author: thvankra
ms.service: cosmos-db
ms.subservice: cosmosdb-cassandra
ms.devlang: nodejs
ms.topic: quickstart
ms.date: 05/18/2020
ms.openlocfilehash: 7575ad7f6444162c7e8ce2a781beeef9555c6240
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2020
ms.locfileid: "87062666"
---
# <a name="quickstart-build-a-cassandra-app-with-nodejs-sdk-and-azure-cosmos-db"></a>Schnellstart: Erstellen einer Cassandra-App mit dem Node.js-SDK und Azure Cosmos DB

> [!div class="op_single_selector"]
> * [.NET](create-cassandra-dotnet.md)
> * [.NET Core](create-cassandra-dotnet-core.md)
> * [Java v3](create-cassandra-java.md)
> * [Java v4](create-cassandra-java-v4.md)
> * [Node.js](create-cassandra-nodejs.md)
> * [Python](create-cassandra-python.md)
>  

In dieser Schnellstartanleitung erstellen Sie ein Azure Cosmos DB-Cassandra-API-Konto und verwenden eine über GitHub geklonte Cassandra-Node.js-App, um eine Cassandra-Datenbank und einen Cassandra-Container zu erstellen. Azure Cosmos DB ist ein Multimodell-Datenbankdienst, mit dem Sie mithilfe der Funktionen für globale Verteilung und horizontale Skalierung schnell Dokument-, Tabellen-, Schlüssel-Wert- und Graph-Datenbanken erstellen und abfragen können.

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)] Alternativ können Sie [Azure Cosmos DB ohne Azure-Abonnement testen](https://azure.microsoft.com/try/cosmosdb/) – kostenlos und ohne Verpflichtung.

Zudem benötigen Sie:
* [Node.js](https://nodejs.org/dist/v0.10.29/x64/node-v0.10.29-x64.msi) Version v0.10.29 oder höher
* [Git-Client](https://git-scm.com/)

## <a name="create-a-database-account"></a>Erstellen eines Datenbankkontos

Vor dem Erstellen einer Dokumentdatenbank müssen Sie mit Azure Cosmos DB ein Cassandra-Konto erstellen.

[!INCLUDE [cosmos-db-create-dbaccount-cassandra](../../includes/cosmos-db-create-dbaccount-cassandra.md)]

## <a name="clone-the-sample-application"></a>Klonen der Beispielanwendung

In diesem Schritt klonen wir eine Cassandra-API-App aus GitHub, legen die Verbindungszeichenfolge fest und führen die App aus. Sie werden feststellen, wie einfach Sie programmgesteuert mit Daten arbeiten können. 

1. Öffnen Sie eine Eingabeaufforderung. Erstellen Sie einen neuen Ordner namens `git-samples`. Schließen Sie dann die Eingabeaufforderung.

    ```bash
    md "C:\git-samples"
    ```

2. Öffnen Sie ein Git-Terminalfenster, z.B. git bash. Wechseln Sie mit dem Befehl `cd` in den neuen Ordner, um die Beispiel-App zu installieren.

    ```bash
    cd "C:\git-samples"
    ```

3. Führen Sie den folgenden Befehl aus, um das Beispielrepository zu klonen. Dieser Befehl erstellt eine Kopie der Beispiel-App auf Ihrem Computer.

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-cassandra-nodejs-getting-started.git
    ```

## <a name="review-the-code"></a>Überprüfen des Codes

Dieser Schritt ist optional. Wenn Sie erfahren möchten, wie der Code die Datenbankressourcen erstellt, können Sie sich die folgenden Codeausschnitte ansehen. Die Codeausschnitte stammen alle aus der Datei `uprofile.js` im Ordner `C:\git-samples\azure-cosmos-db-cassandra-nodejs-getting-started`. Andernfalls können Sie mit [Aktualisieren der Verbindungszeichenfolge](#update-your-connection-string) fortfahren. 

* Die Werte für Benutzername und Kennwort wurden auf der Seite „Verbindungszeichenfolge“ des Azure-Portals festgelegt. `path\to\cert` gibt den Pfad für das X.509-Zertifikat an. 

   ```javascript
   var ssl_option = {
        cert : fs.readFileSync("path\to\cert"),
        rejectUnauthorized : true,
        secureProtocol: 'TLSv1_2_method'
        };
   const authProviderLocalCassandra = new cassandra.auth.PlainTextAuthProvider(config.username, config.password);
   ```

* `client` wird mit contactPoint-Informationen initialisiert. Der Kontaktpunkt (contactPoint) wird aus dem Azure-Portal abgerufen.

    ```javascript
    const client = new cassandra.Client({contactPoints: [config.contactPoint], authProvider: authProviderLocalCassandra, sslOptions:ssl_option});
    ```

* `client` stellt eine Verbindung mit der Cassandra-API von Azure Cosmos DB her.

    ```javascript
    client.connect(next);
    ```

* Ein neuer Keyspace wird erstellt.

    ```javascript
    function createKeyspace(next) {
        var query = "CREATE KEYSPACE IF NOT EXISTS uprofile WITH replication = {\'class\': \'NetworkTopologyStrategy\', \'datacenter1\' : \'1\' }";
        client.execute(query, next);
        console.log("created keyspace");    
  }
    ```

* Eine neue Tabelle wird erstellt.

   ```javascript
   function createTable(next) {
    var query = "CREATE TABLE IF NOT EXISTS uprofile.user (user_id int PRIMARY KEY, user_name text, user_bcity text)";
        client.execute(query, next);
        console.log("created table");
   },
   ```

* Schlüssel-Wert-Entitäten werden eingefügt.

    ```javascript
        function insert(next) {
            console.log("\insert");
            const arr = ['INSERT INTO  uprofile.user (user_id, user_name , user_bcity) VALUES (1, \'AdrianaS\', \'Seattle\')',
                        'INSERT INTO  uprofile.user (user_id, user_name , user_bcity) VALUES (2, \'JiriK\', \'Toronto\')',
                        'INSERT INTO  uprofile.user (user_id, user_name , user_bcity) VALUES (3, \'IvanH\', \'Mumbai\')',
                        'INSERT INTO  uprofile.user (user_id, user_name , user_bcity) VALUES (4, \'IvanH\', \'Seattle\')',
                        'INSERT INTO  uprofile.user (user_id, user_name , user_bcity) VALUES (5, \'IvanaV\', \'Belgaum\')',
                        'INSERT INTO  uprofile.user (user_id, user_name , user_bcity) VALUES (6, \'LiliyaB\', \'Seattle\')',
                        'INSERT INTO  uprofile.user (user_id, user_name , user_bcity) VALUES (7, \'JindrichH\', \'Buenos Aires\')',
                        'INSERT INTO  uprofile.user (user_id, user_name , user_bcity) VALUES (8, \'AdrianaS\', \'Seattle\')',
                        'INSERT INTO  uprofile.user (user_id, user_name , user_bcity) VALUES (9, \'JozefM\', \'Seattle\')'];
            arr.forEach(element => {
            client.execute(element);
            });
            next();
        },
    ```

* Abfrage zum Abrufen aller Schlüsselwerte.

    ```javascript
        function selectAll(next) {
            console.log("\Select ALL");
            var query = 'SELECT * FROM uprofile.user';
            client.execute(query, function (err, result) {
            if (err) return next(err);
            result.rows.forEach(function(row) {
                console.log('Obtained row: %d | %s | %s ',row.user_id, row.user_name, row.user_bcity);
            }, this);
            next();
            });
        },
    ```  
    
* Abfrage zum Abrufen eines Schlüsselwerts.

    ```javascript
        function selectById(next) {
            console.log("\Getting by id");
            var query = 'SELECT * FROM uprofile.user where user_id=1';
            client.execute(query, function (err, result) {
            if (err) return next(err);
            result.rows.forEach(function(row) {
                console.log('Obtained row: %d | %s | %s ',row.user_id, row.user_name, row.user_bcity);
            }, this);
            next();
            });
        }
    ```  

## <a name="update-your-connection-string"></a>Aktualisieren der Verbindungszeichenfolge

Wechseln Sie nun zurück zum Azure-Portal, um die Informationen der Verbindungszeichenfolge abzurufen und in die App zu kopieren. Die Angabe der Verbindungszeichenfolge ermöglicht Ihrer App die Kommunikation mit Ihrer gehosteten Datenbank.

1. Wählen Sie im [Azure-Portal](https://portal.azure.com/) in Ihrem Azure Cosmos DB-Konto die Option **Verbindungszeichenfolge** aus. 

1. Verwenden Sie die Schaltfläche :::image type="icon" source="./media/create-cassandra-nodejs/copy.png"::: auf der rechten Seite des Bildschirms, um den obersten Wert (den Kontaktpunkt) zu kopieren.

    :::image type="content" source="./media/create-cassandra-nodejs/keys.png" alt-text="Anzeigen und Kopieren von Kontaktpunkt, Benutzername und Kennwort aus dem Azure-Portal auf der Seite „Verbindungszeichenfolge“":::

1. Öffnen Sie die Datei `config.js` . 

1. Ersetzen Sie `<FillMEIN>` in Zeile 4 durch den Kontaktpunktwert aus dem Portal.

    Zeile 4 sollte nun in etwa wie folgt aussehen: 

    `config.contactPoint = "cosmos-db-quickstarts.cassandra.cosmosdb.azure.com:10350"`

1. Kopieren Sie den Wert für den Benutzernamen aus dem Portal, und überschreiben Sie damit `<FillMEIN>` in Zeile 2.

    Zeile 2 sollte nun in etwa wie folgt aussehen: 

    `config.username = 'cosmos-db-quickstart';`
    
1. Kopieren Sie den Wert für das Kennwort aus dem Portal, und überschreiben Sie damit `<FillMEIN>` in Zeile 3.

    Zeile 3 sollte nun in etwa wie folgt aussehen:

    `config.password = '2Ggkr662ifxz2Mg==';`

1. Speichern Sie die Datei `config.js`.
    
## <a name="use-the-x509-certificate"></a>Verwenden des X.509-Zertifikats

1. Laden Sie das Zertifikat von „Baltimore CyberTrust Root“ unter [https://cacert.omniroot.com/bc2025.crt](https://cacert.omniroot.com/bc2025.crt) lokal herunter. Benennen Sie die Datei mit der Dateierweiterung `.cer` um.

   Das Zertifikat weist die Seriennummer `02:00:00:b9` und den SHA1-Fingerabdruck `d4🇩🇪20:d0:5e:66:fc:53:fe:1a:50:88:2c:78:db:28:52:ca:e4:74` auf.

2. Öffnen Sie `uprofile.js`, und ändern Sie `path\to\cert` in einen Verweis auf das neue Zertifikat.

3. Speichern Sie `uprofile.js`.

> [!NOTE]
> Wenn in den Schritten weiter unten ein Zertifikatfehler auftritt und Sie einen Windows-Computer verwenden, vergewissern Sie sich, dass Sie eine CRT-Datei gemäß des folgenden Verfahrens ordnungsgemäß in das CER-Format von Microsoft konvertiert haben.
> 
> Doppelklicken Sie auf die CRT-Datei, um sie in der Zertifikatanzeige zu öffnen. 
>
> :::image type="content" source="./media/create-cassandra-nodejs/crtcer1.gif" alt-text="Anzeigen und Überprüfen der Ausgabe":::
>
> Klicken Sie im Zertifikat-Assistenten auf „Weiter“. Wählen Sie „Base-64-codiert X.509 (.CER)“ und dann „Weiter“ aus.
>
> :::image type="content" source="./media/create-cassandra-nodejs/crtcer2.gif" alt-text="Anzeigen und Überprüfen der Ausgabe":::
>
> Wählen Sie „Durchsuchen“ aus, um ein Ziel zu suchen, und geben Sie einen Dateinamen ein.
> Wählen Sie „Weiter“ und dann „Fertig“ aus.
>
> Sie sollten nun über eine ordnungsgemäß formatierte CER-Datei verfügen. Stellen Sie sicher, dass der Pfad in `uprofile.js` auf diese Datei verweist.

## <a name="run-the-nodejs-app"></a>Ausführen der Node.js-App

1. Vergewissern Sie sich im Git-Terminalfenster, dass Sie sich in dem zuvor geklonten Beispielverzeichnis befinden:

    ```bash
    cd azure-cosmos-db-cassandra-nodejs-getting-started
    ```

2. Führen Sie `npm install` aus, um die erforderlichen npm-Module zu installieren.

3. Führen Sie `node uprofile.js` aus, um Ihre Node-Anwendung zu starten.

4. Überprüfen Sie die Ergebnisse über die Befehlszeile.

    :::image type="content" source="./media/create-cassandra-nodejs/output.png" alt-text="Anzeigen und Überprüfen der Ausgabe":::

    Drücken Sie STRG+C, um die Programmausführung zu beenden und das Konsolenfenster zu schließen. 

5. Öffnen Sie im Azure-Portal den **Daten-Explorer**, um diese neuen Daten abzufragen, zu ändern und zu verwenden. 

    :::image type="content" source="./media/create-cassandra-nodejs/data-explorer.png" alt-text="Anzeigen der Daten im Daten-Explorer"::: 

## <a name="review-slas-in-the-azure-portal"></a>Überprüfen von SLAs im Azure-Portal

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

[!INCLUDE [cosmosdb-delete-resource-group](../../includes/cosmos-db-delete-resource-group.md)]

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie erfahren, wie Sie ein Azure Cosmos DB-Konto mit Cassandra-API erstellen und eine Cassandra-Node.js-App ausführen, die eine Cassandra-Datenbank und einen Cassandra-Container erstellt. Jetzt können Sie zusätzliche Daten in Ihr Azure Cosmos DB-Konto importieren. 

> [!div class="nextstepaction"]
> [Azure Cosmos DB: Import Cassandra data](cassandra-import-data.md) (Azure Cosmos DB: Importieren von Cassandra-Daten)