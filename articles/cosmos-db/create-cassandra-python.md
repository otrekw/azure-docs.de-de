---
title: 'Schnellstart: Cassandra-API mit Python – Azure Cosmos DB'
description: In diesem Schnellstart erfahren Sie, wie Sie mithilfe der Apache Cassandra-API von Azure Cosmos DB eine Profilanwendung mit Python erstellen.
author: TheovanKraay
ms.author: thvankra
ms.service: cosmos-db
ms.subservice: cosmosdb-cassandra
ms.devlang: python
ms.topic: quickstart
ms.date: 08/13/2020
ms.custom: devx-track-python
ms.openlocfilehash: f376a1f3601c976ff1efdaee1da6181510a9cf64
ms.sourcegitcommit: 3bf69c5a5be48c2c7a979373895b4fae3f746757
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/14/2020
ms.locfileid: "88234940"
---
# <a name="quickstart-build-a-cassandra-app-with-python-sdk-and-azure-cosmos-db"></a>Schnellstart: Erstellen einer Cassandra-App mit dem Python-SDK und Azure Cosmos DB

> [!div class="op_single_selector"]
> * [.NET](create-cassandra-dotnet.md)
> * [.NET Core](create-cassandra-dotnet-core.md)
> * [Java v3](create-cassandra-java.md)
> * [Java v4](create-cassandra-java-v4.md)
> * [Node.js](create-cassandra-nodejs.md)
> * [Python](create-cassandra-python.md)
>  

In dieser Schnellstartanleitung erstellen Sie ein Azure Cosmos DB-Cassandra-API-Konto und verwenden eine über GitHub geklonte Cassandra-Python-App, um eine Cassandra-Datenbank und einen Cassandra-Container zu erstellen. Azure Cosmos DB ist ein Multimodell-Datenbankdienst, mit dem Sie mithilfe der Funktionen für globale Verteilung und horizontale Skalierung schnell Dokument-, Tabellen-, Schlüssel-Wert- und Graph-Datenbanken erstellen und abfragen können.

## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure-Konto mit einem aktiven Abonnement. [Erstellen Sie ein kostenloses Konto.](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) Oder [testen Sie Azure Cosmos DB kostenlos](https://azure.microsoft.com/try/cosmosdb/) ohne ein Azure-Abonnement.
- [Python 2.7.14 oder höher bzw. 3.4 oder höher](https://www.python.org/downloads/)
- [Git](https://git-scm.com/downloads).
- [Python-Treiber für Apache Cassandra](https://github.com/datastax/python-driver)

## <a name="create-a-database-account"></a>Erstellen eines Datenbankkontos

Vor dem Erstellen einer Dokumentdatenbank müssen Sie mit Azure Cosmos DB ein Cassandra-Konto erstellen.

[!INCLUDE [cosmos-db-create-dbaccount-cassandra](../../includes/cosmos-db-create-dbaccount-cassandra.md)]

## <a name="clone-the-sample-application"></a>Klonen der Beispielanwendung

In diesem Schritt klonen wir eine Cassandra-API-App aus GitHub, legen die Verbindungszeichenfolge fest und führen die App aus. Sie werden feststellen, wie einfach Sie programmgesteuert mit Daten arbeiten können. 

1. Öffnen Sie eine Eingabeaufforderung. Erstellen Sie einen neuen Ordner namens `git-samples`. Schließen Sie dann die Eingabeaufforderung.

    ```bash
    md "C:\git-samples"
    ```

2. Öffnen Sie ein Git-Terminalfenster (z.B. git bash), und verwenden Sie den Befehl `cd`, um in den neuen Ordner zu gelangen und dort die Beispiel-App zu installieren.

    ```bash
    cd "C:\git-samples"
    ```

3. Führen Sie den folgenden Befehl aus, um das Beispielrepository zu klonen. Dieser Befehl erstellt eine Kopie der Beispiel-App auf Ihrem Computer.

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-cassandra-python-getting-started.git
    ```

## <a name="review-the-code"></a>Überprüfen des Codes

Dieser Schritt ist optional. Wenn Sie erfahren möchten, wie der Code die Datenbankressourcen erstellt, können Sie sich die folgenden Codeausschnitte ansehen. Die Codeausschnitte stammen alle aus der Datei *pyquickstart.py*. Andernfalls können Sie mit [Aktualisieren der Verbindungszeichenfolge](#update-your-connection-string) fortfahren. 

* `cluster` wird mit Informationen zu `contactPoint` und `port` initialisiert, die aus dem Azure-Portal abgerufen werden. `cluster` stellt dann über die Methode `connect()` eine Verbindung mit der Cassandra-API von Azure Cosmos DB her. Mithilfe des Benutzernamens, des Kennworts und des Standardzertifikats oder eines expliziten Zertifikats (sofern Sie eins mit der Konfigurationsdatei angeben) wird eine autorisierte Verbindung hergestellt.

  :::code language="python" source="~/cosmosdb-cassandra-python-sample/pyquickstart.py" id="authenticateAndConnect":::

* Ein neuer Keyspace wird erstellt.

  :::code language="python" source="~/cosmosdb-cassandra-python-sample/pyquickstart.py" id="createKeyspace":::

* Eine neue Tabelle wird erstellt.

  :::code language="python" source="~/cosmosdb-cassandra-python-sample/pyquickstart.py" id="createTable":::

* Schlüssel-Wert-Entitäten werden eingefügt.

  :::code language="python" source="~/cosmosdb-cassandra-python-sample/pyquickstart.py" id="insertData":::

* Abfrage zum Abrufen aller Schlüsselwerte.

  :::code language="python" source="~/cosmosdb-cassandra-python-sample/pyquickstart.py" id="queryAllItems":::
    
* Abfrage zum Abrufen eines Schlüsselwerts.

  :::code language="python" source="~/cosmosdb-cassandra-python-sample/pyquickstart.py" id="queryByID":::

## <a name="update-your-connection-string"></a>Aktualisieren der Verbindungszeichenfolge

Wechseln Sie nun zurück zum Azure-Portal, um die Informationen der Verbindungszeichenfolge abzurufen und in die App zu kopieren. Die Angabe der Verbindungszeichenfolge ermöglicht Ihrer App die Kommunikation mit Ihrer gehosteten Datenbank.

1. Wählen Sie im [Azure-Portal](https://portal.azure.com/) in Ihrem Azure Cosmos DB-Konto die Option **Verbindungszeichenfolge** aus. 

1. Verwenden Sie die Schaltfläche :::image type="icon" source="./media/create-cassandra-python/copy.png"::: auf der rechten Seite des Bildschirms, um den obersten Wert (den Kontaktpunkt) zu kopieren. 

   :::image type="content" source="./media/create-cassandra-python/keys.png" alt-text="Anzeigen und Kopieren eines Benutzernamens, Kennworts und Kontaktpunkts für den Zugriff im Azure-Portal auf dem Blatt „Verbindungszeichenfolge“":::

1. Speichern Sie die Datei *config.py*. 

1. Ersetzen Sie `<FILLME>` in Zeile 10 durch den Kontaktpunktwert aus dem Portal.

    Zeile 10 sollte nun in etwa wie folgt aussehen: 

    `'contactPoint': 'cosmos-db-quickstarts.cassandra.cosmosdb.azure.com:10350'`

1. Kopieren Sie den Wert für den Benutzernamen aus dem Portal, und überschreiben Sie damit `<FILLME>` in Zeile 6.

    Zeile 6 sollte nun in etwa wie folgt aussehen: 

    `'username': 'cosmos-db-quickstart',`
    
1. Kopieren Sie den Wert für das Kennwort aus dem Portal, und überschreiben Sie damit `<FILLME>` in Zeile 8.

    Zeile 8 sollte nun in etwa wie folgt aussehen:

    `'password' = '2Ggkr662ifxz2Mg==`';`

1. Speichern Sie die Datei *config.py*.
    
## <a name="use-the-x509-certificate"></a>Verwenden des X.509-Zertifikats

1. Laden Sie das Zertifikat von „Baltimore CyberTrust Root“ unter [https://cacert.omniroot.com/bc2025.crt](https://cacert.omniroot.com/bc2025.crt) lokal herunter. Benennen Sie die Datei mit der Dateierweiterung *.cer* um.

   Das Zertifikat weist die Seriennummer `02:00:00:b9` und den SHA1-Fingerabdruck `d4🇩🇪20:d0:5e:66:fc:53:fe:1a:50:88:2c:78:db:28:52:ca:e4:74` auf.

2. Öffnen Sie *pyquickstart.py*, und ändern Sie `path\to\cert`, um auf das neue Zertifikat zu verweisen.

3. Speichern Sie *pyquickstart.py*.

## <a name="run-the-python-app"></a>Ausführen der Python-App

1. Verwenden Sie den Befehl cd im Git-Terminal, um in den Ordner `azure-cosmos-db-cassandra-python-getting-started` zu wechseln. 

2. Führen Sie die folgenden Befehle aus, um die erforderlichen Module zu installieren:

    ```python
    python -m pip install cassandra-driver
    python -m pip install prettytable
    python -m pip install requests
    python -m pip install pyopenssl
    ```

2. Führen Sie den folgenden Befehl aus, um Ihre Python-Anwendung zu starten:

    ```
    python pyquickstart.py
    ```

3. Überprüfen Sie die Ergebnisse über die Befehlszeile.

    Drücken Sie STRG+C, um die Programmausführung zu beenden und das Konsolenfenster zu schließen. 

    :::image type="content" source="./media/create-cassandra-python/output.png" alt-text="Anzeigen und Überprüfen der Ausgabe":::
    
4. Öffnen Sie im Azure-Portal den **Daten-Explorer**, um diese neuen Daten abzufragen, zu ändern und zu verwenden. 

    :::image type="content" source="./media/create-cassandra-python/data-explorer.png" alt-text="Anzeigen der Daten im Daten-Explorer":::

## <a name="review-slas-in-the-azure-portal"></a>Überprüfen von SLAs im Azure-Portal

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

[!INCLUDE [cosmosdb-delete-resource-group](../../includes/cosmos-db-delete-resource-group.md)]

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie erfahren, wie Sie ein Azure Cosmos DB-Konto mit Cassandra-API erstellen und eine Cassandra-Python-App ausführen, die eine Cassandra-Datenbank und einen Cassandra-Container erstellt. Jetzt können Sie zusätzliche Daten in Ihr Azure Cosmos DB-Konto importieren. 

> [!div class="nextstepaction"]
> [Azure Cosmos DB: Import Cassandra data](cassandra-import-data.md) (Azure Cosmos DB: Importieren von Cassandra-Daten)

