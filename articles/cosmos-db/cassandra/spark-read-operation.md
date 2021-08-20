---
title: Lesen von Daten in Cassandra-API-Tabellen mithilfe von Spark
titleSufix: Azure Cosmos DB
description: In diesem Artikel wird beschrieben, wie Sie Daten in Cassandra-API-Tabellen in Azure Cosmos DB lesen.
author: TheovanKraay
ms.author: thvankra
ms.reviewer: sngun
ms.service: cosmos-db
ms.subservice: cosmosdb-cassandra
ms.topic: how-to
ms.date: 06/02/2020
ms.custom: seodec18
ms.openlocfilehash: b43e58acc8b2942d4961f60659993b2d07efadaa
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2021
ms.locfileid: "122339994"
---
# <a name="read-data-from-azure-cosmos-db-cassandra-api-tables-using-spark"></a>Lesen von Daten in Azure Cosmos DB-Cassandra-API-Tabellen mithilfe von Spark
[!INCLUDE[appliesto-cassandra-api](../includes/appliesto-cassandra-api.md)]

 In diesem Artikel wird beschrieben, wie Sie in der Azure Cosmos DB-Cassandra-API von Spark gespeicherte Daten lesen.

## <a name="cassandra-api-configuration"></a>Konfiguration der Cassandra-API
```scala
import org.apache.spark.sql.cassandra._
//Spark connector
import com.datastax.spark.connector._
import com.datastax.spark.connector.cql.CassandraConnector

//if using Spark 2.x, CosmosDB library for multiple retry
//import com.microsoft.azure.cosmosdb.cassandra

//Connection-related
spark.conf.set("spark.cassandra.connection.host","YOUR_ACCOUNT_NAME.cassandra.cosmosdb.azure.com")
spark.conf.set("spark.cassandra.connection.port","10350")
spark.conf.set("spark.cassandra.connection.ssl.enabled","true")
spark.conf.set("spark.cassandra.auth.username","YOUR_ACCOUNT_NAME")
spark.conf.set("spark.cassandra.auth.password","YOUR_ACCOUNT_KEY")
// if using Spark 2.x
// spark.conf.set("spark.cassandra.connection.factory", "com.microsoft.azure.cosmosdb.cassandra.CosmosDbConnectionFactory")

//Throughput-related...adjust as needed
spark.conf.set("spark.cassandra.output.batch.size.rows", "1")
//spark.conf.set("spark.cassandra.connection.connections_per_executor_max", "10") // Spark 2.x
spark.conf.set("spark.cassandra.connection.remoteConnectionsPerExecutor", "10") // Spark 3.x
spark.conf.set("spark.cassandra.output.concurrent.writes", "1000")
spark.conf.set("spark.cassandra.concurrent.reads", "512")
spark.conf.set("spark.cassandra.output.batch.grouping.buffer.size", "1000")
spark.conf.set("spark.cassandra.connection.keep_alive_ms", "600000000")
```

> [!NOTE]
> Wenn Sie Spark 3.0 oder höher verwenden, müssen Sie die Cosmos DB-Hilfs- und Verbindungs-Factory nicht installieren. Sie sollten auch `remoteConnectionsPerExecutor` anstelle von `connections_per_executor_max` für den Spark 3-Connector verwenden (siehe oben). Sie sehen, dass verbindungsbezogene Eigenschaften im obigen Notebook definiert sind. Mit der folgenden Syntax können so Verbindungseigenschaften definiert werden, ohne diese auf Clusterebene definierem zu müssen (Spark-Kontextinitialisierung).

## <a name="dataframe-api"></a>Datenrahmen-API

### <a name="read-table-using-sessionreadformat-command"></a>Lesen einer Tabelle mit dem Befehl „session.read.format“

```scala
val readBooksDF = sqlContext
  .read
  .format("org.apache.spark.sql.cassandra")
  .options(Map( "table" -> "books", "keyspace" -> "books_ks"))
  .load

readBooksDF.explain
readBooksDF.show
```
### <a name="read-table-using-sparkreadcassandraformat"></a>Lesen einer Tabelle mithilfe von spark.read.cassandraFormat 

```scala
val readBooksDF = spark.read.cassandraFormat("books", "books_ks", "").load()
```

### <a name="read-specific-columns-in-table"></a>Lesen von bestimmten Spalten in einer Tabelle

```scala
val readBooksDF = spark
  .read
  .format("org.apache.spark.sql.cassandra")
  .options(Map( "table" -> "books", "keyspace" -> "books_ks"))
  .load
  .select("book_name","book_author", "book_pub_year")

readBooksDF.printSchema
readBooksDF.explain
readBooksDF.show
```

### <a name="apply-filters"></a>Anwenden von Filtern

Sie können Prädikate per Pushdown in die Datenbank verlagern, um Spark-Abfragen mit besserer Optimierung zu ermöglichen. Ein Prädikat ist eine Bedingung für eine Abfrage, für die „true“ oder „false“ zurückgegeben wird. Normalerweise ist sie in der WHERE-Klausel enthalten. Bei einem Pushdown eines Prädikats werden die Daten in der Datenbankabfrage gefiltert, um die Anzahl von aus der Datenbank abgerufenen Einträgen zu reduzieren und die Abfrageleistung zu verbessern. Standardmäßig werden gültige WHERE-Klauseln von der Spark-Dataset-API automatisch per Pushdown in die Datenbank verlagert. 

```scala
val df = spark.read.cassandraFormat("books", "books_ks").load
df.explain
val dfWithPushdown = df.filter(df("book_pub_year") > 1891)
dfWithPushdown.explain

readBooksDF.printSchema
readBooksDF.explain
readBooksDF.show
```

Der Abschnitt `Cassandra Filters` des physischen Plans enthält den Pushdownfilter. 

:::image type="content" source="./media/spark-read-operation/pushdown-predicates-spark-3.png" alt-text="Partitionen":::

## <a name="rdd-api"></a>RDD-API

### <a name="read-table"></a>Lesen einer Tabelle
```scala
val bookRDD = sc.cassandraTable("books_ks", "books")
bookRDD.take(5).foreach(println)
```

### <a name="read-specific-columns-in-table"></a>Lesen von bestimmten Spalten in einer Tabelle

```scala
val booksRDD = sc.cassandraTable("books_ks", "books").select("book_id","book_name").cache
booksRDD.take(5).foreach(println)
```

## <a name="sql-views"></a>SQL-Ansichten 

### <a name="create-a-temporary-view-from-a-dataframe"></a>Erstellen einer temporären Ansicht eines Datenframes

```scala
spark
  .read
  .format("org.apache.spark.sql.cassandra")
  .options(Map( "table" -> "books", "keyspace" -> "books_ks"))
  .load.createOrReplaceTempView("books_vw")
```

### <a name="run-queries-against-the-view"></a>Ausführen von Abfragen mit der Ansicht

```sql
select * from books_vw where book_pub_year > 1891
```

## <a name="next-steps"></a>Nächste Schritte

Im Folgenden finden Sie weitere Artikel zur Arbeit mit der Azure Cosmos DB-Cassandra-API von Spark:
 
 * [Upsertvorgänge](spark-upsert-operations.md)
 * [Löschvorgänge](spark-delete-operation.md)
 * [Aggregationsvorgänge](spark-aggregation-operations.md)
 * [Tabellenkopiervorgänge an der Azure Cosmos DB-Cassandra-API von Spark aus](spark-table-copy-operations.md)

