---
title: DDL-Vorgänge in der Azure Cosmos DB-Cassandra-API von Spark
description: In diesem Artikel werden DDL-Vorgänge im Keyspace und in Tabellen für die Azure Cosmos DB-Cassandra-API von Spark beschrieben.
author: TheovanKraay
ms.author: thvankra
ms.reviewer: sngun
ms.service: cosmos-db
ms.subservice: cosmosdb-cassandra
ms.topic: how-to
ms.date: 10/07/2020
ms.openlocfilehash: 54398620775b28f0c497a061f5ea4446a38a5ada
ms.sourcegitcommit: 80d311abffb2d9a457333bcca898dfae830ea1b4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/26/2021
ms.locfileid: "110464905"
---
# <a name="ddl-operations-in-azure-cosmos-db-cassandra-api-from-spark"></a>DDL-Vorgänge in der Azure Cosmos DB-Cassandra-API von Spark
[!INCLUDE[appliesto-cassandra-api](includes/appliesto-cassandra-api.md)]

In diesem Artikel werden DDL-Vorgänge im Keyspace und in Tabellen für die Azure Cosmos DB-Cassandra-API von Spark beschrieben.

## <a name="spark-context"></a>Spark-Kontext

 Für den Connector für die Cassandra-API müssen die Details der Cassandra-Verbindung als Teil des Spark-Kontexts initialisiert werden. Beim Starten eines Notebooks ist der Spark-Kontext bereits initialisiert, und es ist nicht ratsam, den Start zu beenden, und den Kontext erneut zu initialisieren. Eine Lösung ist, die Konfiguration der Cassandra-API-Instanz auf Clusterebene hinzuzufügen, in der Cluster-Spark-Konfiguration. Dies ist eine einmalige Aktivität pro Cluster. Fügen Sie der Spark-Konfiguration den folgenden Code als ein durch Leerzeichen getrenntes Schlüssel-Wert-Paar hinzu:
 
  ```scala
  spark.cassandra.connection.host YOUR_COSMOSDB_ACCOUNT_NAME.cassandra.cosmosdb.azure.com
  spark.cassandra.connection.port 10350
  spark.cassandra.connection.ssl.enabled true
  spark.cassandra.auth.username YOUR_COSMOSDB_ACCOUNT_NAME
  spark.cassandra.auth.password YOUR_COSMOSDB_KEY
  ```

## <a name="cassandra-api-related-configuration"></a>Konfiguration für die Cassandra-API 

```scala
import org.apache.spark.sql.cassandra._
//Spark connector
import com.datastax.spark.connector._
import com.datastax.spark.connector.cql.CassandraConnector

//if using Spark 2.x, CosmosDB library for multiple retry
//import com.microsoft.azure.cosmosdb.cassandra
//spark.conf.set("spark.cassandra.connection.factory", "com.microsoft.azure.cosmosdb.cassandra.CosmosDbConnectionFactory")

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
> Wenn Sie Spark 3.0 oder höher verwenden, müssen Sie die Cosmos DB-Hilfs- und Verbindungs-Factory nicht installieren. Sie sollten auch `remoteConnectionsPerExecutor` anstelle von `connections_per_executor_max` für den Spark 3-Connector verwenden (siehe oben).

## <a name="keyspace-ddl-operations"></a>Keyspace-DDL-Vorgänge

### <a name="create-a-keyspace"></a>Erstellen eines Keyspace

```scala
//Cassandra connector instance
val cdbConnector = CassandraConnector(sc)

// Create keyspace
cdbConnector.withSessionDo(session => session.execute("CREATE KEYSPACE IF NOT EXISTS books_ks WITH REPLICATION = {'class': 'SimpleStrategy', 'replication_factor': 1 } "))
```

#### <a name="validate-in-cqlsh"></a>Überprüfen in cqlsh

Führen Sie den folgenden Befehl in cqlsh aus. Daraufhin sollte der Keyspace, den Sie zuvor erstellt haben, angezeigt werden.

```bash
DESCRIBE keyspaces;
```

### <a name="drop-a-keyspace"></a>Verwerfen eines Keyspace

```scala
val cdbConnector = CassandraConnector(sc)
cdbConnector.withSessionDo(session => session.execute("DROP KEYSPACE books_ks"))
```

#### <a name="validate-in-cqlsh"></a>Überprüfen in cqlsh

```bash
DESCRIBE keyspaces;
```
## <a name="table-ddl-operations"></a>Tabellen-DDL-Vorgänge

**Überlegungen:**  

- Durchsatz kann mithilfe der Anweisung „create table“ auf Tabellenebene zugewiesen werden.  
- In einem Partitionsschlüssel können 20 GB Daten gespeichert werden.  
- In einem Datensatz können bis zu 2 MB Daten gespeichert werden.  
- In einem Partitionsschlüsselbereich können mehrere Partitionsschlüssel gespeichert werden.

### <a name="create-a-table"></a>Erstellen einer Tabelle

```scala
cdbConnector.withSessionDo(session => session.execute("CREATE TABLE IF NOT EXISTS books_ks.books(book_id TEXT,book_author TEXT, book_name TEXT,book_pub_year INT,book_price FLOAT, PRIMARY KEY(book_id,book_pub_year)) WITH cosmosdb_provisioned_throughput=4000 , WITH default_time_to_live=630720000;"))
```

#### <a name="validate-in-cqlsh"></a>Überprüfen in cqlsh

Führen Sie den folgenden Befehl in cqlsh aus. Daraufhin sollte die Tabelle „books“ angezeigt werden: 

```bash
USE books_ks;
DESCRIBE books;
```

Der bereitgestellte Durchsatz und die Standard-TTL-Werte werden in der Ausgabe des vorherigen Befehls nicht angezeigt. Sie können diese Werte im Portal abrufen.

### <a name="alter-table"></a>ALTER TABLE

Sie können die folgenden Werte mit dem Befehl ALTER TABLE ändern:

* Bereitgestellter Durchsatz 
* Wert der Gültigkeitsdauer
<br>Spaltenänderungen werden derzeit nicht unterstützt.

```scala
val cdbConnector = CassandraConnector(sc)
cdbConnector.withSessionDo(session => session.execute("ALTER TABLE books_ks.books WITH cosmosdb_provisioned_throughput=8000, WITH default_time_to_live=0;"))
```

### <a name="drop-table"></a>Löschen einer Tabelle

```scala
val cdbConnector = CassandraConnector(sc)
cdbConnector.withSessionDo(session => session.execute("DROP TABLE IF EXISTS books_ks.books;"))
```

#### <a name="validate-in-cqlsh"></a>Überprüfen in cqlsh

Führen Sie den folgenden Befehl in cqlsh aus. Sie sollten sehen, dass die Tabelle „books“ nicht mehr verfügbar ist:

```bash
USE books_ks;
DESCRIBE tables;
```

## <a name="next-steps"></a>Nächste Schritte

Fahren Sie nach dem Erstellen von Keyspace und Tabelle mit den folgenden Artikeln für CRUD-Vorgänge und Ähnliches fort:
 
* [Erstellungs-/Einfügevorgänge](cassandra-spark-create-ops.md)  
* [Dient zum Lesen von Vorgängen.](cassandra-spark-read-ops.md)  
* [Upsertvorgänge](cassandra-spark-upsert-ops.md)  
* [Löschvorgänge](cassandra-spark-delete-ops.md)  
* [Aggregationsvorgänge](cassandra-spark-aggregation-ops.md)  
* [Tabellenkopiervorgänge an der Azure Cosmos DB-Cassandra-API von Spark aus](cassandra-spark-table-copy-ops.md)  
