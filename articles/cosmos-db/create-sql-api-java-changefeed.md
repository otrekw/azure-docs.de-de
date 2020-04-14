---
title: 'Tutorial: End-to-End-Anwendungsbeispiel für Async Java-SQL-API mit Änderungsfeed'
description: In diesem Tutorial wird Schritt für Schritt eine einfache Java-SQL-API-Anwendung beschrieben, bei der Dokumente in einen Azure Cosmos DB-Container eingefügt werden, während per Änderungsfeed eine materialisierte Sicht des Containers angezeigt wird.
author: anfeldma
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: java
ms.topic: tutorial
ms.date: 04/01/2020
ms.author: anfeldma
ms.openlocfilehash: 5eab523dde2a13a85b0c8ff5bcbb3ecb5912e78e
ms.sourcegitcommit: 3c318f6c2a46e0d062a725d88cc8eb2d3fa2f96a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/02/2020
ms.locfileid: "80587217"
---
# <a name="tutorial---an-end-to-end-async-java-sql-api-application-sample-with-change-feed"></a>Tutorial: End-to-End-Anwendungsbeispiel für Async Java-SQL-API mit Änderungsfeed

In diesem Tutorial wird Schritt für Schritt eine einfache Java-SQL-API-Anwendung beschrieben, bei der Dokumente in einen Azure Cosmos DB-Container eingefügt werden, während per Änderungsfeed eine materialisierte Sicht des Containers angezeigt wird.

## <a name="prerequisites"></a>Voraussetzungen

* PC

* URI und Schlüssel für Ihr Azure Cosmos DB-Konto

* Maven

* Java 8

## <a name="background"></a>Hintergrund

Über den Azure Cosmos DB-Änderungsfeed wird eine ereignisgesteuerte Schnittstelle zum Auslösen von Aktionen als Reaktion auf Dokumenteinfügungen bereitgestellt. Hierfür gibt es viele Anwendungsfälle. Bei Anwendungen mit hoher Lese- und Schreibintensität wird der Änderungsfeed beispielsweise vor allem genutzt, um eine **materialisierte Echtzeitsicht** eines Containers zu erstellen, während dafür Dokumente erfasst werden. Die materialisierte Sicht des Containers enthält dieselben Daten, aber sie ist partitioniert, um effiziente Lesevorgänge zu ermöglichen. So werden für die Anwendung sowohl effiziente Lese- als auch Schreibvorgänge erzielt.

Die Verwaltung der Ereignisse von Änderungsfeeds wird größtenteils von der Änderungsfeedprozessor-Bibliothek durchgeführt, die in das SDK integriert ist. Diese Bibliothek ist leistungsfähig genug, um die Ereignisse von Änderungsfeeds bei Bedarf auf mehrere Worker zu verteilen. Hierfür müssen Sie die Änderungsfeedbibliothek lediglich mit einem Rückruf versehen.

In diesem einfachen Beispiel wird die Änderungsfeedprozessor-Bibliothek mit nur einem Worker veranschaulicht, über den Dokumente für eine materialisierte Sicht erstellt und gelöscht werden.

## <a name="setup"></a>Einrichten

Klonen Sie das Repository mit dem App-Beispiel, falls Sie dies noch nicht getan haben:

```bash
git clone https://github.com/Azure-Samples/azure-cosmos-java-sql-app-example.git
```

> Sie haben die Wahl, ob Sie diese Schnellstartanleitung mit Version 4.0 oder 3.7.0 des Java SDK durcharbeiten. **Geben Sie im Terminal ```git checkout SDK3.7.0```** ein, wenn Sie Java SDK 3.7.0 nutzen möchten. Verwenden Sie andernfalls weiter den ```master```-Branch, da hierfür standardmäßig Java SDK 4.0 genutzt wird.

Öffnen Sie im Repositoryverzeichnis ein Terminal. Führen Sie den Buildvorgang für die App durch, indem Sie Folgendes ausführen:

```bash
mvn clean package
```

## <a name="walkthrough"></a>Exemplarische Vorgehensweise

1. Prüfen Sie als Erstes, ob Sie über ein Azure Cosmos DB-Konto verfügen. Öffnen Sie in Ihrem Browser das **Azure-Portal**, und navigieren Sie zu Ihrem Azure Cosmos DB-Konto und dann im linken Bereich zu **Daten-Explorer**.

    ![Azure Cosmos DB-Konto](media/create-sql-api-java-changefeed/cosmos_account_empty.JPG)

1. Führen Sie die App im Terminal mit dem folgenden Befehl aus:

    ```bash
    mvn exec:java -Dexec.mainClass="com.azure.cosmos.workedappexample.SampleGroceryStore" -DACCOUNT_HOST="your-account-uri" -DACCOUNT_KEY="your-account-key" -Dexec.cleanupDaemonThreads=false
    ```

1. Drücken Sie die EINGABETASTE, wenn Folgendes angezeigt wird:

    ```bash
    Press enter to create the grocery store inventory system...
    ```

    Wechseln Sie anschließend in Ihrem Browser zurück zum Daten-Explorer im Azure-Portal. Sie sehen, dass die Datenbank **GroceryStoreDatabase** mit drei leeren Containern hinzugefügt wurde: 

    * **InventoryContainer**: Der Bestandsdatensatz für unser Lebensmittelgeschäft-Beispiel partitioniert nach ```id``` (eine UUID).
    * **InventoryContainer-pktype**: Eine materialisierte Sicht des Bestandsdatensatzes, die für Abfragen nach ```type``` optimiert ist.
    * **InventoryContainer-leases**: Ein Leasecontainer wird für Änderungsfeeds immer benötigt. Mit Leases wird der Status der App beim Lesen des Änderungsfeeds nachverfolgt.


    ![Leere Container](media/create-sql-api-java-changefeed/cosmos_account_resources_lease_empty.JPG)


1. Im Terminal wird eine Eingabeaufforderung angezeigt:

    ```bash
    Press enter to start creating the materialized view...
    ```

    Drücken Sie die EINGABETASTE. Der unten angegebene Codeblock wird ausgeführt, und der Änderungsfeedprozessor wird in einem anderen Thread initialisiert: 


    **Java SDK 4.0**
    ```java
    changeFeedProcessorInstance = getChangeFeedProcessor("SampleHost_1", feedContainer, leaseContainer);
    changeFeedProcessorInstance.start()
        .subscribeOn(Schedulers.elastic())
        .doOnSuccess(aVoid -> {
            isProcessorRunning.set(true);
        })
        .subscribe();

    while (!isProcessorRunning.get()); //Wait for Change Feed processor start
    ```

    **Java SDK 3.7.0**
    ```java
    changeFeedProcessorInstance = getChangeFeedProcessor("SampleHost_1", feedContainer, leaseContainer);
    changeFeedProcessorInstance.start()
        .subscribeOn(Schedulers.elastic())
        .doOnSuccess(aVoid -> {
            isProcessorRunning.set(true);
        })
        .subscribe();

    while (!isProcessorRunning.get()); //Wait for Change Feed processor start    
    ```

    ```"SampleHost_1"``` ist der Name des Workers für den Änderungsfeedprozessor. Mit ```changeFeedProcessorInstance.start()``` wird der Änderungsfeedprozessor gestartet.

    Wechseln Sie in Ihrem Browser zurück zum Daten-Explorer im Azure-Portal. Klicken Sie unter dem Container **InventoryContainer-leases** auf **items**, um den Inhalt anzuzeigen. Sie sehen, dass über den Änderungsfeedprozessor Daten in den Leasecontainer eingefügt wurden. Der Prozessor hat dem Worker ```SampleHost_1``` eine Lease für einige Partitionen von **InventoryContainer** zugewiesen.

    ![Leases](media/create-sql-api-java-changefeed/cosmos_leases.JPG)

1. Drücken Sie im Terminal erneut die EINGABETASTE. In **InventoryContainer** werden zehn Dokumente eingefügt. Jede Dokumenteinfügung wird im Änderungsfeed als JSON-Code angezeigt. Mit dem folgenden Rückrufcode werden diese Ereignisse verarbeitet, indem die JSON-Dokumente in einer materialisierten Sicht gespiegelt werden:

    **Java SDK 4.0**
    ```java
    public static ChangeFeedProcessor getChangeFeedProcessor(String hostName, CosmosAsyncContainer feedContainer, CosmosAsyncContainer leaseContainer) {
        ChangeFeedProcessorOptions cfOptions = new ChangeFeedProcessorOptions();
        cfOptions.setFeedPollDelay(Duration.ofMillis(100));
        cfOptions.setStartFromBeginning(true);
        return ChangeFeedProcessor.changeFeedProcessorBuilder()
            .setOptions(cfOptions)
            .setHostName(hostName)
            .setFeedContainer(feedContainer)
            .setLeaseContainer(leaseContainer)
            .setHandleChanges((List<JsonNode> docs) -> {
                for (JsonNode document : docs) {
                        //Duplicate each document update from the feed container into the materialized view container
                        updateInventoryTypeMaterializedView(document);
                }

            })
            .build();
    }

    private static void updateInventoryTypeMaterializedView(JsonNode document) {
        typeContainer.upsertItem(document).subscribe();
    }
    ```

    **Java SDK 3.7.0**
    ```java
    public static ChangeFeedProcessor getChangeFeedProcessor(String hostName, CosmosContainer feedContainer, CosmosContainer leaseContainer) {
        ChangeFeedProcessorOptions cfOptions = new ChangeFeedProcessorOptions();
        cfOptions.feedPollDelay(Duration.ofMillis(100));
        cfOptions.startFromBeginning(true);
        return ChangeFeedProcessor.Builder()
            .options(cfOptions)
            .hostName(hostName)
            .feedContainer(feedContainer)
            .leaseContainer(leaseContainer)
            .handleChanges((List<CosmosItemProperties> docs) -> {
                for (CosmosItemProperties document : docs) {
                        //Duplicate each document update from the feed container into the materialized view container
                        updateInventoryTypeMaterializedView(document);
                }

            })
            .build();
    }

    private static void updateInventoryTypeMaterializedView(CosmosItemProperties document) {
        typeContainer.upsertItem(document).subscribe();
    }    
    ```

1. Warten Sie kurz ab, bis der Code fünf bis zehn Sekunden lang ausgeführt wurde. Wechseln Sie dann zurück zum Daten-Explorer im Azure-Portal, und navigieren Sie zu **InventoryContainer > items**. Sie sehen, dass Elemente (items) in den Bestandscontainer eingefügt werden. Achten Sie auf den Partitionsschlüssel (```id```).

    ![Feedcontainer](media/create-sql-api-java-changefeed/cosmos_items.JPG)

1. Navigieren Sie im Daten-Explorer zu **InventoryContainer-pktype > items**. Dies ist die materialisierte Sicht. Die Elemente in diesem Container spiegeln **InventoryContainer** wider, weil sie vom Änderungsfeed programmgesteuert eingefügt wurden. Notieren Sie sich den Partitionsschlüssel (```type```). Diese materialisierte Sicht ist für Abfragen optimiert, die nach ```type``` gefiltert werden. Für **InventoryContainer** wäre dies nicht effizient, weil der Container basierend auf ```id``` partitioniert ist.

    ![Materialisierte Sicht](media/create-sql-api-java-changefeed/cosmos_materializedview2.JPG)

1. Wir löschen ein Dokument gleichzeitig aus **InventoryContainer** und aus **InventoryContainer-pktype**, indem wir nur einmal den Aufruf ```upsertItem()``` verwenden. Sehen Sie sich zunächst den Daten-Explorer im Azure-Portal an. Wir löschen das Dokument, für das ```/type == "plums"``` gilt. Es ist unten rot markiert.

    ![Materialisierte Sicht](media/create-sql-api-java-changefeed/cosmos_materializedview-emph-todelete.JPG)

    Drücken Sie erneut die EINGABETASTE, um die Funktion ```deleteDocument()``` im Beispielcode aufzurufen. Mit dieser Funktion (siehe unten) wird ein Upsert-Vorgang auf eine neue Version des Dokuments mit ```/ttl == 5``` durchgeführt, um die Gültigkeitsdauer (Time-To-Live, TTL) für das Dokument auf fünf Sekunden festzulegen. 
    
    **Java SDK 4.0**
    ```java
    public static void deleteDocument() {

        String jsonString =    "{\"id\" : \"" + idToDelete + "\""
                + ","
                + "\"brand\" : \"Jerry's\""
                + ","
                + "\"type\" : \"plums\""
                + ","
                + "\"quantity\" : \"50\""
                + ","
                + "\"ttl\" : 5"
                + "}";

        ObjectMapper mapper = new ObjectMapper();
        JsonNode document = null;

        try {
            document = mapper.readTree(jsonString);
        } catch (Exception e) {
            e.printStackTrace();
        }

        feedContainer.upsertItem(document,new CosmosItemRequestOptions()).block();
    }    
    ```

    **Java SDK 3.7.0**
    ```java
    public static void deleteDocument() {

        String jsonString =    "{\"id\" : \"" + idToDelete + "\""
                + ","
                + "\"brand\" : \"Jerry's\""
                + ","
                + "\"type\" : \"plums\""
                + ","
                + "\"quantity\" : \"50\""
                + ","
                + "\"ttl\" : 5"
                + "}";

        ObjectMapper mapper = new ObjectMapper();
        JsonNode document = null;

        try {
            document = mapper.readTree(jsonString);
        } catch (Exception e) {
            e.printStackTrace();
        }

        feedContainer.upsertItem(document,new CosmosItemRequestOptions()).block();
    }    
    ```

    Der Änderungsfeed ```feedPollDelay``` ist auf 100 ms festgelegt. Aus diesem Grund reagiert der Änderungsfeed auf dieses Update nahezu sofort und ruft wie oben gezeigt ```updateInventoryTypeMaterializedView()``` auf. Mit diesem letzten Funktionsaufruf wird für das neue Dokument mit der Gültigkeitsdauer von fünf Sekunden ein Upsert-Vorgang in **InventoryContainer-pktype** durchgeführt.

    Das Ergebnis ist, dass das Dokument nach fünf Sekunden abläuft und aus beiden Containern gelöscht wird.

    Diese Vorgehensweise ist erforderlich, da vom Änderungsfeed nur beim Einfügen oder Aktualisieren von Elementen Ereignisse ausgegeben werden (und beim Löschen nicht).

1. Drücken Sie noch einmal die EINGABETASTE, um das Programm zu schließen und die Ressourcen zu bereinigen.
