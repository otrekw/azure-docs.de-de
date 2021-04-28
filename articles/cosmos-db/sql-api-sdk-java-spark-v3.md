---
title: 'Apache Spark 3-OLTP-Connector von Azure Cosmos DB für die SQL-API (Vorschauversion): Versionshinweise und Ressourcen'
description: Hier finden Sie Informationen zum Apache Spark 3-OLTP-Connector von Azure Cosmos DB für die SQL-API (Vorschauversion) – einschließlich Veröffentlichungsterminen, Deaktivierungsterminen und Änderungen an den einzelnen Versionen des Azure Cosmos DB SQL Java SDK.
author: anfeldma-ms
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: java
ms.topic: reference
ms.date: 04/06/2021
ms.author: anfeldma
ms.custom: devx-track-java
ms.openlocfilehash: e7d206b63d6e1bf741a5dc298dd5bbc2d48ab11b
ms.sourcegitcommit: dddd1596fa368f68861856849fbbbb9ea55cb4c7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2021
ms.locfileid: "107368471"
---
# <a name="azure-cosmos-db-apache-spark-3-oltp-connector-for-core-sql-api-preview-release-notes-and-resources"></a>Apache Spark 3-OLTP-Connector von Azure Cosmos DB für die Core-API (SQL) (Vorschauversion): Versionshinweise und Ressourcen
[!INCLUDE[appliesto-sql-api](includes/appliesto-sql-api.md)]

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
> * [Spark 3-OLTP-Connector](sql-api-sdk-java-spark-v3.md)
> * [Spark 2-OLTP-Connector](sql-api-sdk-java-spark.md)
> * [Python](sql-api-sdk-python.md)
> * [REST](/rest/api/cosmos-db/)
> * [REST-Ressourcenanbieter](/rest/api/cosmos-db-resource-provider/)
> * [SQL](./sql-query-getting-started.md)
> * [Bulk Executor – .NET v2](sql-api-sdk-bulk-executor-dot-net.md)
> * [Bulk Executor – Java](sql-api-sdk-bulk-executor-java.md)

**Apache Spark 3-OLTP-Connector von Azure Cosmos DB (Vorschauversion)** bietet Apache Spark v3-Unterstützung für Azure Cosmos DB mithilfe der SQL-API.
[Azure Cosmos DB](introduction.md) ist ein global verteilter Datenbankdienst, der Entwicklern durch eine Vielzahl von Standard-APIs wie SQL, MongoDB, Cassandra, Graph und Tabellen die Arbeit mit Daten ermöglicht.

> [!Note]
> Diese Version des Spark 3-OLTP-Connectors von Azure Cosmos DB ist ein Vorschau-Build.
> Dieser Build wurde nicht geladen und es wurde dafür kein Leistungstest durchgeführt.
> Dieser Build wird nicht für die Verwendung in Produktionsszenarios empfohlen.
>

## <a name="documentation"></a>Dokumentation

- [Erste Schritte](https://github.com/Azure/azure-sdk-for-java/blob/feature/cosmos/spark30/sdk/cosmos/azure-cosmos-spark_3-1_2-12/docs/quick-start.md)
- [Katalog-API](https://github.com/Azure/azure-sdk-for-java/blob/feature/cosmos/spark30/sdk/cosmos/azure-cosmos-spark_3-1_2-12/docs/catalog-api.md)
- [Konfigurationsparameterreferenz](https://github.com/Azure/azure-sdk-for-java/blob/feature/cosmos/spark30/sdk/cosmos/azure-cosmos-spark_3-1_2-12/docs/configuration-reference.md)


## <a name="version-compatibility"></a>Versionskompatibilität

| Connector     | Spark         | Java-Mindestversion | Unterstützte Scala-Versionen |
| ------------- | ------------- | -------------------- | -----------------------  |
| 4.0.0-beta.1  | 3.1.1         |        8             | 2,12                     |

## <a name="download"></a>Herunterladen

Sie können die Maven-Koordinate der JAR-Datei verwenden, um den Spark-Connector automatisch über Databricks Runtime 8 von Maven zu installieren: `com.azure.cosmos.spark:azure-cosmos-spark_3-1_2-12:4.0.0-beta.1`

Sie können die Integration auch im Cosmos DB-Spark-Connector in Ihr SBT-Projekt integrieren:
```scala
libraryDependencies += "com.azure.cosmos.spark" % "azure-cosmos-spark_3-1_2-12" % "4.0.0-beta.1"
```

Der Cosmos DB-Spark-Connector ist im [zentralen Maven-Repository verfügbar](https://search.maven.org/artifact/com.azure.cosmos.spark/azure-cosmos-spark_3-1_2-12/4.0.0-beta.1/jar).

### <a name="general"></a>Allgemein

Wenn ein Fehler auftreten sollte, melden Sie [hier](https://github.com/Azure/azure-sdk-for-java/issues/new) ein Problem.

Wenn Sie ein neues Feature oder Änderungen vorschlagen möchten, die vorgenommen werden könnten, melden Sie ein Problem auf die gleiche Weise wie bei einem Fehler.

[!INCLUDE[Changelog](~/azure-sdk-for-java-cosmos-db/sdk/cosmos/azure-cosmos-spark_3-1_2-12/CHANGELOG.md)]

## <a name="next-steps"></a>Nächste Schritte

Lesen Sie unsere [Schnellstartanleitung für die Arbeit mit dem 3-OLTP-Connector von Azure Cosmos DB Spark](create-sql-api-spark.md).
