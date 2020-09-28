---
title: Versionshinweise und Ressourcen zu Spring Data Azure Cosmos DB v3 für die SQL-API
description: Hier finden Sie Informationen zu Spring Data Azure Cosmos DB v3 für die SQL-API, z. B. Veröffentlichungstermine, Deaktivierungstermine und Änderungen an den einzelnen Versionen des Azure Cosmos DB SQL Async Java SDK.
author: anfeldma-ms
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: java
ms.topic: reference
ms.date: 08/18/2020
ms.author: anfeldma
ms.custom: devx-track-java
ms.openlocfilehash: 9e5b92918d93109183740be555bb805877862407
ms.sourcegitcommit: 0fd1f3fe7817ad44d878d580ec167e1508051795
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/19/2020
ms.locfileid: "90817869"
---
# <a name="spring-data-azure-cosmos-db-v3-for-core-sql-api-release-notes-and-resources"></a>Spring Data Azure Cosmos DB v3 für die Core (SQL)-API: Versionshinweise und Ressourcen
> [!div class="op_single_selector"]
> * [.NET SDK v3](sql-api-sdk-dotnet-standard.md)
> * [.NET SDK v2](sql-api-sdk-dotnet.md)
> * [.NET Core SDK v2](sql-api-sdk-dotnet-core.md)
> * [.NET Change Feed SDK v2](sql-api-sdk-dotnet-changefeed.md)
> * [Node.js](sql-api-sdk-node.md)
> * [Java SDK v4](sql-api-sdk-java-v4.md)
> * [Async Java SDK v2](sql-api-sdk-async-java.md)
> * [Sync Java SDK v2](sql-api-sdk-java.md)
> * [Spring Data v2](sql-api-sdk-java-spring-v2.md)
> * [Spring Data v3](sql-api-sdk-java-spring-v3.md)
> * [Spark-Connector](sql-api-sdk-java-spark.md)
> * [Python](sql-api-sdk-python.md)
> * [REST](/rest/api/cosmos-db/)
> * [REST-Ressourcenanbieter](/rest/api/cosmos-db-resource-provider/)
> * [SQL](sql-api-query-reference.md)
> * [Bulk Executor – .NET v2](sql-api-sdk-bulk-executor-dot-net.md)
> * [Bulk Executor – Java](sql-api-sdk-bulk-executor-java.md)

Mit Spring Data Azure Cosmos DB Version 3 für Core (SQL) können Entwickler Azure Cosmos DB in Spring-Anwendungen nutzen. Bei Spring Data Azure Cosmos DB wird die Spring Data-Schnittstelle für die Bearbeitung von Datenbanken und Sammlungen, die Verwendung von Dokumenten und die Ausgabe von Abfragen verfügbar gemacht. Für dasselbe Maven-Artefakt werden sowohl synchrone als auch asynchrone (reaktive) APIs unterstützt. 

Spring Data Azure Cosmos DB ist abhängig vom Spring Data-Framework. Das Azure Cosmos DB SDK-Team veröffentlicht Maven-Artefakte für die Spring Data-Versionen 2.2 und 2.3.

Das [Spring-Framework](https://spring.io/projects/spring-framework) ist ein Programmier- und Konfigurationsmodell, mit dem die Java-Anwendungsentwicklung optimiert wird. Spring optimiert die „Verknüpfung“ von Anwendungen mithilfe von Abhängigkeitsinjektion. Spring ist bei vielen Entwicklern beliebt, weil dadurch das Erstellen und Testen von Anwendungen vereinfacht wird. Mit [Spring Boot](https://spring.io/projects/spring-boot) wird diese „Verknüpfung“ auf die Entwicklung von Webanwendungen und Microservices ausgedehnt. [Spring Data](https://spring.io/projects/spring-data) ist ein Programmiermodell und -framework für den Zugriff auf Datenspeicher wie Azure Cosmos DB, im Kontext einer Spring- bzw. Spring Boot-Anwendung. 

Sie können Spring Data Azure Cosmos DB in Ihren [Azure Spring Cloud](https://azure.microsoft.com/services/spring-cloud/)-Anwendungen verwenden.

> [!IMPORTANT]  
> Diese Versionshinweise gelten für Version 3 von Spring Data Azure Cosmos DB. Sie können [Versionshinweise zu Version 2 hier](sql-api-sdk-java-spring-v2.md) finden. 
>
> Spring Data Azure Cosmos DB unterstützt nur die SQL-API.
>
> In diesen Artikeln finden Sie Informationen zu Spring Data für andere Azure Cosmos DB-APIs:
> * [Spring Data für Apache Cassandra mit Azure Cosmos DB](https://docs.microsoft.com/azure/developer/java/spring-framework/configure-spring-data-apache-cassandra-with-cosmos-db)
> * [Spring Data MongoDB mit Azure Cosmos DB](https://docs.microsoft.com/azure/developer/java/spring-framework/configure-spring-data-mongodb-with-cosmos-db)
> * [Spring Data Gremlin Starter mit Azure Cosmos DB](https://docs.microsoft.com/azure/developer/java/spring-framework/configure-spring-data-gremlin-java-app-with-cosmos-db)
>

## <a name="start-here"></a>Beginnen Sie hier

# <a name="explore"></a>[Erkunden](#tab/explore)

<img src="media/sql-api-sdk-java-spring-v3/up-arrow.png" alt="explore the tabs above" width="80"/>

#### <a name="these-tabs-contain-basic-spring-data-azure-cosmos-db-samples"></a>Diese Registerkarten enthalten grundlegende Beispiele zu Spring Data Azure Cosmos DB.

# <a name="pomxml"></a>[pom.xml](#tab/pom)

### <a name="configure-dependencies"></a>Konfigurieren von Abhängigkeiten

  ```xml
  <dependency>
      <groupId>com.azure</groupId>
      <artifactId>azure-spring-data-cosmos</artifactId>
      <version>latest</version>
  </dependency>
  ```

# <a name="connect"></a>[Herstellen einer Verbindung](#tab/connect)

### <a name="connect"></a>Verbinden

Geben Sie Details zum Azure Cosmos DB-Konto und -Container an. Spring Data Azure Cosmos DB erstellt den Client automatisch und stellt eine Verbindung mit dem Container her.

[application.properties](https://github.com/Azure-Samples/azure-spring-data-cosmos-java-sql-api-getting-started/blob/main/azure-spring-data-cosmos-java-getting-started/src/main/resources/application.properties):
```
cosmos.uri=${ACCOUNT_HOST}
cosmos.key=${ACCOUNT_KEY}
cosmos.secondaryKey=${SECONDARY_ACCOUNT_KEY}

dynamic.collection.name=spel-property-collection
# Populate query metrics
cosmos.queryMetricsEnabled=true
```

# <a name="doc-ops"></a>[Doc-Ops](#tab/docs)

### <a name="document-operations"></a>Dokumentenvorgänge

Erstellen:
:::code language="java" source="~/spring-data-azure-cosmos-db-sql-tutorial/azure-spring-data-cosmos-java-getting-started/src/main/java/com/azure/spring/data/cosmostutorial/SampleApplication.java" ID="Create":::

Delete (Löschen):
:::code language="java" source="~/spring-data-azure-cosmos-db-sql-tutorial/azure-spring-data-cosmos-java-getting-started/src/main/java/com/azure/spring/data/cosmostutorial/SampleApplication.java" ID="Delete":::

# <a name="query"></a>[Abfrage](#tab/queries)

Abfrage:
:::code language="java" source="~/spring-data-azure-cosmos-db-sql-tutorial/azure-spring-data-cosmos-java-getting-started/src/main/java/com/azure/spring/data/cosmostutorial/SampleApplication.java" ID="Query":::

---

## <a name="resources"></a>Ressourcen

* **Tragen zum SDK bei**: [Spring Data Azure Cosmos DB-Repository auf GitHub](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/cosmos/azure-spring-data-cosmos)

* **Tutorial**: [Spring Data Azure Cosmos DB-Tutorial auf GitHub](https://github.com/Azure-Samples/azure-spring-data-cosmos-java-sql-api-getting-started) 

## <a name="release-history"></a>Releaseverlauf

### <a name="300-beta2-september-17-2020"></a>3.0.0-Beta.2 (17. September 2020)

#### <a name="new-features"></a>Neue Funktionen

* Die Artefakt-ID wurde auf `azure-spring-data-cosmos` aktualisiert.
* Die azure-cosmos-Abhängigkeit wurde auf `4.5.0` aktualisiert.
* `Query Annotation`-Unterstützung für native Abfragen.
* Unterstützung für Java 11.
* Unterstützung für geschachtelten Partitionsschlüssel durch Verfügbarmachen des Felds `partitionKeyPath` in der Anmerkung `@Container` wurde hinzugefügt.
* Unterstützung für den Abfragetyp `limit` wurde hinzugefügt, sodass `top` und `first` beim Definieren von Repository-APIs verwendet werden kann.

#### <a name="key-bug-fixes"></a>Wichtige Fehlerbehebungen

* Ein Fehler im Zusammenhang mit geschachteltem Partitionsschlüssel bei Verwendung mit der Anmerkung `@GeneratedValue` wurde behoben.

### <a name="300-beta1-august-17-2020"></a>3.0.0-Beta.1 (17. August 2020)

#### <a name="new-features"></a>Neue Funktionen

* Aktualisiert die Gruppen-ID auf `com.azure`.
* Aktualisiert die Artefakt-ID auf `azure-spring-data-2-3-cosmos`.
* Aktualisiert die azure-cosmos-SDK-Abhängigkeit auf `4.3.2-beta.2`.
* Fügt Unterstützung für das Überwachen von Entitäten hinzu: automatische Verwaltung der mit Anmerkungen versehenen Felder `createdBy`, `createdDate`, `lastModifiedBy` und `lastModifiedDate`.
* Fügt `@GeneratedValue`-Anmerkungsunterstützung für die automatische ID-Generierung bei ID-Feldern vom Typ `String` hinzu.
* Fügt Unterstützung für mehrere Datenbankkonfigurationen bei einem einzelnen Azure Cosmos DB-Konto mit mehreren Datenbanken und bei mehreren Azure Cosmos DB-Konten mit mehreren Datenbanken hinzu.
* Fügt Unterstützung für die Anmerkung `@Version` für ein beliebiges Zeichenfolgenfeld hinzu.
* Aktualisiert Synchronisierungs-API-Rückgabetypen in `Iterable`-Typen statt `List`.
* Macht `CosmosClientBuilder` aus dem Azure Cosmos DB SDK als Spring Bean für die Klasse `@Configuration` verfügbar.
* Aktualisiert `CosmosConfig`, sodass Abfragemetriken und die Prozessorimplementierung für die Abfragediagnose darin enthalten sind.
* Fügt Unterstützung für die Rückgabe des Datentyps `Optional` für einzelne Ergebnisabfragen hinzu.

#### <a name="renames"></a>Umbenennungen

* `CosmosDbFactory` in `CosmosFactory`.
* `CosmosDBConfig` in `CosmosConfig`.
* `CosmosDBAccessException` in `CosmosAccessException`.
* `Document`-Anmerkung in die `Container`-Anmerkung.
* `DocumentIndexingPolicy`-Anmerkung in die `CosmosIndexingPolicy`-Anmerkung.
* `DocumentQuery` in `CosmosQuery`.
* application.properties-Flag `populateQueryMetrics` in `queryMetricsEnabled`.

#### <a name="key-bug-fixes"></a>Wichtige Fehlerbehebungen

* Tasks zur Diagnoseprotokollierung werden als `Parallel`-Threads geplant, um das Blockieren von Netty-E/A-Threads zu vermeiden.
* Korrigiert die optimistische Sperre beim Löschvorgang.
* Behebt ein Problem beim Versehen von Abfragen mit Escapezeichen für die `IN`-Klausel.
* Behebt ein Problem, indem der Datentyp `long` für `@Id` zugelassen wird.
* Behebt ein Problem, indem `boolean`, `long`, `int` und `double` als Datentypen für die Anmerkung `@PartitionKey` zugelassen werden.
* Korrigiert die Schlüsselwörter `IgnoreCase` und `AllIgnoreCase` für Abfragen ohne Berücksichtigung von Groß-/Kleinschreibung.
* Entfernt den Standardwert für Anforderungseinheiten (4.000), wenn Container automatisch erstellt werden.

## <a name="faq"></a>Häufig gestellte Fragen

[!INCLUDE [cosmos-db-sdk-faq](../../includes/cosmos-db-sdk-faq.md)]

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/).

Informieren Sie sich genauer über das [Spring-Framework](https://spring.io/projects/spring-framework).

Informieren Sie sich genauer über [Spring Boot](https://spring.io/projects/spring-boot).

Informieren Sie sich genauer über [Spring Data](https://spring.io/projects/spring-data).