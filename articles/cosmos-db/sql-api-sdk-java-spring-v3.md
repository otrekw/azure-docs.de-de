---
title: Versionshinweise und Ressourcen zu Spring Data Azure Cosmos DB v3 für die SQL-API
description: Hier finden Sie wichtige Informationen zu Spring Data Azure Cosmos DB v3 für die SQL-API, z. B. Veröffentlichungstermine, Deaktivierungstermine und Änderungen an den einzelnen Versionen des Azure Cosmos DB SQL Async Java SDK.
author: anfeldma-ms
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: java
ms.topic: reference
ms.date: 08/18/2020
ms.author: anfeldma
ms.custom: devx-track-java
ms.openlocfilehash: e67e6911eeac29036dee2b68c19395b34e1d11da
ms.sourcegitcommit: d68c72e120bdd610bb6304dad503d3ea89a1f0f7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/01/2020
ms.locfileid: "89228034"
---
# <a name="spring-data-azure-cosmos-db-v3-for-core-sql-api-release-notes-and-resources"></a>Spring Data Azure Cosmos DB v3 für Core-API (SQL): Versionshinweise und Ressourcen
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

Mit Spring Data Azure Cosmos DB v3 für Core (SQL) können Entwickler Azure Cosmos DB in Spring-Anwendungen nutzen. Bei Spring Data Azure Cosmos DB wird die Spring Data-Schnittstelle für die Bearbeitung von Datenbanken und Sammlungen, die Verwendung von Dokumenten und die Ausgabe von Abfragen verfügbar gemacht. Für dasselbe Maven-Artefakt werden sowohl synchrone als auch asynchrone (reaktive) APIs unterstützt. 

Spring Data Azure Cosmos DB weist eine Abhängigkeit vom Spring Data-Framework auf. Das Azure Cosmos DB SDK-Team veröffentlicht Maven-Artefakte für Spring Data v 2.2 und v 2.3.

Das [Spring-Framework](https://spring.io/projects/spring-framework) ist ein Programmier- und Konfigurationsmodell, mit dem die Java-Anwendungsentwicklung optimiert wird. Die Website der Organisation enthält die Information, dass mit Spring die „Verknüpfung“ (Englisch: „plumbing“) von Anwendungen durch Abhängigkeitsinjektion optimiert wird. Spring ist bei vielen Entwicklern beliebt, weil das Erstellen und Testen von Anwendungen damit vereinfacht wird. Mit [Spring Boot](https://spring.io/projects/spring-boot) wird diese „Verknüpfung“ auf die Entwicklung von Webanwendungen und Microservices ausgedehnt. [Spring Data](https://spring.io/projects/spring-data) ist ein Programmiermodell und -framework für den Zugriff auf Datenspeicher, z. B. Azure Cosmos DB, im Kontext einer Spring- bzw. Spring Boot-Anwendung. 

Sie können Spring Data Azure Cosmos DB in Ihren [Azure Spring Cloud](https://azure.microsoft.com/services/spring-cloud/)-Anwendungen verwenden.

> [!IMPORTANT]  
> Diese Versionshinweise gelten für v3 von Spring Data Azure Cosmos DB. Die Versionshinweise zu v2 finden Sie [hier](sql-api-sdk-java-spring-v2.md). 
>
> Spring Data Azure Cosmos DB unterstützt nur die SQL-API.
>
> Die folgenden Leitfäden enthalten Informationen zur Unterstützung von Spring Data für andere Azure Cosmos DB-APIs:
> * [Spring Data für Apache Cassandra mit Azure Cosmos DB](https://docs.microsoft.com/azure/developer/java/spring-framework/configure-spring-data-apache-cassandra-with-cosmos-db)
> * [Spring Data MongoDB mit Azure Cosmos DB](https://docs.microsoft.com/azure/developer/java/spring-framework/configure-spring-data-mongodb-with-cosmos-db)
> * [Spring Data Gremlin Starter mit Azure Cosmos DB](https://docs.microsoft.com/azure/developer/java/spring-framework/configure-spring-data-gremlin-java-app-with-cosmos-db)
>

## <a name="start-here"></a>Beginnen Sie hier

# <a name="explore"></a>[Erkunden](#tab/explore)

<img src="media/sql-api-sdk-java-spring-v3/up-arrow.png" alt="explore the tabs above" width="80"/>

### <a name="navigate-the-tabs-above-for-basic-spring-data-azure-cosmos-db-samples"></a>Einfache Spring Data Azure Cosmos DB-Beispiele finden Sie über die Registerkarten oben.

# <a name="pomxml"></a>[pom.xml](#tab/pom)

### <a name="configure-dependencies"></a>Konfigurieren von Abhängigkeiten

Es sind zwei Spring Data Azure Cosmos DB v3-Maven-Artefakte verfügbar.

Artefakt, das von Spring Data Framework v2.2 abhängig ist:
```xml
<dependency>
    <groupId>com.azure</groupId>
    <artifactId>azure-spring-data-2-2-cosmos</artifactId>
    <version>latest</version>
</dependency>
```

Artefakt, das von Spring Data Framework v2.3 abhängig ist:
```xml
<dependency>
    <groupId>com.azure</groupId>
    <artifactId>azure-spring-data-2-3-cosmos</artifactId>
    <version>latest</version>
</dependency>
```

# <a name="connect"></a>[Herstellen einer Verbindung](#tab/connect)

### <a name="connect"></a>Verbinden

Geben Sie Details zum Azure Cosmos DB-Konto und -Container an. Spring Data Azure Cosmos DB erstellt den Client automatisch und stellt eine Verbindung mit dem Container her.

[application.properties](https://github.com/Azure-Samples/azure-spring-data-cosmos-java-sql-api-getting-started/blob/main/azure-spring-data-2-2-cosmos-java-getting-started/src/main/resources/application.properties):
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

[Erstellen](https://github.com/Azure-Samples/azure-spring-data-cosmos-java-sql-api-getting-started/blob/main/azure-spring-data-2-3-cosmos-java-getting-started/src/main/java/com/azure/spring/data/cosmostutorial/SampleApplication.java): [!code-java[](~/spring-data-azure-cosmos-db-sql-tutorial/azure-spring-data-2-3-cosmos-java-getting-started/src/main/java/com/azure/spring/data/cosmostutorial/SampleApplication.java?name=Create)]

[Löschen](https://github.com/Azure-Samples/azure-spring-data-cosmos-java-sql-api-getting-started/blob/main/azure-spring-data-2-3-cosmos-java-getting-started/src/main/java/com/azure/spring/data/cosmostutorial/SampleApplication.java): [!code-java[](~/spring-data-azure-cosmos-db-sql-tutorial/azure-spring-data-2-3-cosmos-java-getting-started/src/main/java/com/azure/spring/data/cosmostutorial/SampleApplication.java?name=Delete)]

# <a name="query"></a>[Abfrage](#tab/queries)

### <a name="query"></a>Abfrage

[Abfragen](https://github.com/Azure-Samples/azure-spring-data-cosmos-java-sql-api-getting-started/blob/main/azure-spring-data-2-3-cosmos-java-getting-started/src/main/java/com/azure/spring/data/cosmostutorial/SampleApplication.java): [!code-java[](~/spring-data-azure-cosmos-db-sql-tutorial/azure-spring-data-2-3-cosmos-java-getting-started/src/main/java/com/azure/spring/data/cosmostutorial/SampleApplication.java?name=Query)]

---

## <a name="helpful-content"></a>Nützliche Inhalte

| Inhalt | Spring Data Framework v2.2 | Spring Data Framework v2.3 |
|---|---|
| **SDK-Download** | [Maven](https://mvnrepository.com/artifact/com.azure/azure-spring-data-2-2-cosmos) | [Maven](https://mvnrepository.com/artifact/com.azure/azure-spring-data-2-3-cosmos) |
|**Am SDK mitwirken** | [Spring Data Azure Cosmos DB-Repository auf GitHub](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/cosmos/azure-spring-data-2-2-cosmos) | [Spring Data Azure Cosmos DB-Repository auf GitHub](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/cosmos/azure-spring-data-2-3-cosmos) | 
|**Tutorial**| [Spring Data Azure Cosmos DB-Tutorial auf GitHub](https://github.com/Azure-Samples/azure-spring-data-cosmos-java-sql-api-getting-started/tree/main/azure-spring-data-2-2-cosmos-java-getting-started) | [Spring Data Azure Cosmos DB-Tutorial auf GitHub](https://github.com/Azure-Samples/azure-spring-data-cosmos-java-sql-api-getting-started/tree/main/azure-spring-data-2-3-cosmos-java-getting-started) |

## <a name="release-history"></a>Releaseverlauf

### <a name="300-beta2-unreleased"></a>3.0.0-beta.2 (nicht freigegeben)

### <a name="300-beta1-2020-08-17"></a>3.0.0-beta.1 (2020-08-17)
#### <a name="new-features"></a>Neue Funktionen
* Die Gruppen-ID wurde auf `com.azure` aktualisiert.
* Die Artefakt-ID wurde auf `azure-spring-data-2-3-cosmos` aktualisiert.
* Die azure-cosmos-SDK-Abhängigkeit wurde auf `4.3.2-beta.2` aktualisiert.
* Unterstützung für die Überwachung von Entitäten: automatische Verwaltung der Felder „createdBy“, „createdDate“, „lastModifiedBy“ und „lastModifiedDate“ mit Anmerkungen.
* `@GeneratedValue`-Anmerkungsunterstützung für die automatische ID-Generierung für ID-Felder vom Typ `String`.
* Unterstützung für mehrere Datenbankkonfigurationen für ein einzelnes Cosmos-Konto mit mehreren Datenbanken und für mehrere Cosmos-Konten mit mehreren Datenbanken.
* Unterstützung für die `@Version`-Anmerkung für ein beliebiges Zeichenfolgenfeld.
* Aktualisierte Synchronisierungs-APIs geben Typen als `Iterable`-Typen statt als `List` zurück.
* `CosmosClientBuilder` aus dem Cosmos-SDK wurde als Spring Bean für die `@Configuration`-Klasse verfügbar gemacht.
* `CosmosConfig` wurde aktualisiert, sodass Abfragemetriken und die Prozessorimplementierung für die Abfragediagnose enthalten sind.
* Unterstützung für die Rückgabe des `Optional`-Datentyps für einzelne Ergebnisabfragen.
#### <a name="renames"></a>Umbenennungen
* `CosmosDbFactory` in `CosmosFactory`.
* `CosmosDBConfig` in `CosmosConfig`.
* `CosmosDBAccessException` in `CosmosAccessException`.
* `Document`-Anmerkung in die `Container`-Anmerkung.
* `DocumentIndexingPolicy`-Anmerkung in die `CosmosIndexingPolicy`-Anmerkung.
* `DocumentQuery` in `CosmosQuery`.
* application.properties-Flag `populateQueryMetrics` in `queryMetricsEnabled`.
#### <a name="key-bug-fixes"></a>Wichtige Fehlerbehebungen
* Diagnoseprotokollierungstasks werden als `Parallel`-Threads geplant, um das Blockieren von Netty-E/A-Threads zu vermeiden.
* Optimistische Sperre beim Löschvorgang behoben.
* Problem beim Versehen von Abfragen mit Escapezeichen für die `IN`-Klausel behoben.
* Ein Problem wurde behoben, indem der `long`-Datentyp für `@Id` zugelassen wurde.
* Ein Problem wurde behoben, indem `boolean`, `long` `int`, `double` als Datentypen für die `@PartitionKey`-Anmerkung zugelassen werden.
* Schlüsselwörter `IgnoreCase` & `AllIgnoreCase` für Abfragen ohne Berücksichtigung von Groß-/Kleinschreibung korrigiert.
* Der Standardwert 4000 für die Anforderungseinheit beim automatischen Erstellen von Containern wurde entfernt.

## <a name="faq"></a>Häufig gestellte Fragen
[!INCLUDE [cosmos-db-sdk-faq](../../includes/cosmos-db-sdk-faq.md)]

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu Cosmos DB finden Sie auf der Seite zum Dienst [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/).

Weitere Informationen zum Spring-Framework finden Sie auf der [Homepage des Projekts](https://spring.io/projects/spring-framework).

Weitere Informationen zu Spring Boot finden Sie auf der [Homepage des Projekts](https://spring.io/projects/spring-boot).

Weitere Informationen zu Spring Data finden Sie auf der [Homepage des Projekts](https://spring.io/projects/spring-data).