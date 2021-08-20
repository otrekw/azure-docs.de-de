---
title: Upsert-Vorgang mit Daten in die Azure Cosmos-DB-Cassandra-API von Spark
description: In diesem Artikel wird erläutert, wie Sie einen Upsert-Vorgang in Tabellen in der Azure Cosmos-DB-Cassandra-API von Spark ausführen.
author: TheovanKraay
ms.author: thvankra
ms.reviewer: sngun
ms.service: cosmos-db
ms.subservice: cosmosdb-cassandra
ms.topic: how-to
ms.date: 09/24/2018
ms.openlocfilehash: ab09908a51004c1f9c2cf202705c47481271427a
ms.sourcegitcommit: 82d82642daa5c452a39c3b3d57cd849c06df21b0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/07/2021
ms.locfileid: "113355337"
---
# <a name="upsert-data-into-azure-cosmos-db-cassandra-api-from-spark"></a>Upsert-Vorgang mit Daten in die Azure Cosmos-DB-Cassandra-API von Spark
[!INCLUDE[appliesto-cassandra-api](includes/appliesto-cassandra-api.md)]

In diesem Artikel wird beschrieben, wie Sie einen Upsert-Vorgang mit Daten in die Azure Cosmos-DB-Cassandra-API von Spark ausführen.

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
> Wenn Sie Spark 3.0 oder höher verwenden, müssen Sie die Cosmos DB-Hilfs- und Verbindungs-Factory nicht installieren. Sie sollten auch `remoteConnectionsPerExecutor` anstelle von `connections_per_executor_max` für den Spark 3-Connector verwenden (siehe oben). Sie sehen, dass verbindungsbezogene Eigenschaften im obigen Notebook definiert sind. Mit der folgenden Syntax können so Verbindungseigenschaften definiert werden, ohne diese auf Clusterebene definierem zu müssen (Spark-Kontextinitialisierung). Bei Verwendung von Vorgängen, die Spark-Kontext erfordern (z. B. `CassandraConnector(sc)` für `update` wie unten gezeigt), müssen Verbindungseigenschaften jedoch auf Clusterebene definiert werden.

## <a name="dataframe-api"></a>Datenrahmen-API

### <a name="create-a-dataframe"></a>Erstellen eines Datenrahmens 

```scala
// (1) Update: Changing author name to include prefix of "Sir"
// (2) Insert: adding a new book

val booksUpsertDF = Seq(
    ("b00001", "Sir Arthur Conan Doyle", "A study in scarlet", 1887),
    ("b00023", "Sir Arthur Conan Doyle", "A sign of four", 1890),
    ("b01001", "Sir Arthur Conan Doyle", "The adventures of Sherlock Holmes", 1892),
    ("b00501", "Sir Arthur Conan Doyle", "The memoirs of Sherlock Holmes", 1893),
    ("b00300", "Sir Arthur Conan Doyle", "The hounds of Baskerville", 1901),
    ("b09999", "Sir Arthur Conan Doyle", "The return of Sherlock Holmes", 1905)
    ).toDF("book_id", "book_author", "book_name", "book_pub_year")
booksUpsertDF.show()
```

### <a name="upsert-data"></a>Durchführen von Upsert für Daten

```scala
// Upsert is no different from create
booksUpsertDF.write
  .mode("append")
  .format("org.apache.spark.sql.cassandra")
  .options(Map( "table" -> "books", "keyspace" -> "books_ks"))
  .save()
```

### <a name="update-data"></a>Aktualisieren von Daten

```scala
//Cassandra connector instance
val cdbConnector = CassandraConnector(sc)

//This runs on the driver, leverage only for one off updates
cdbConnector.withSessionDo(session => session.execute("update books_ks.books set book_price=99.33 where book_id ='b00300' and book_pub_year = 1901;"))
```

## <a name="rdd-api"></a>RDD-API
> [!NOTE]
> Der Upsert-Vorgang aus der RDD-API ist mit dem Create-Vorgang identisch. 

## <a name="next-steps"></a>Nächste Schritte

Lesen Sie die folgenden Artikel, um zu erfahren, wie Sie andere Vorgänge an den Daten ausführen, die in Azure Cosmos-DB-Cassandra-API-Tabellen gespeichert sind:
 
* [Löschvorgänge](cassandra-spark-delete-ops.md)
* [Aggregationsvorgänge](cassandra-spark-aggregation-ops.md)
* [Tabellenkopiervorgänge an der Azure Cosmos DB-Cassandra-API von Spark aus](cassandra-spark-table-copy-ops.md)
