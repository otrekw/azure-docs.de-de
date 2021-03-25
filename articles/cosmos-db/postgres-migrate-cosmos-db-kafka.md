---
title: Migrieren von Daten aus PostgreSQL zum Azure Cosmos DB-Cassandra-API-Konto mithilfe von Apache Kafka
description: Erfahren Sie, wie Sie mit Kafka Connect Daten aus PostgreSQL in Echtzeit mit der Azure Cosmos DB-Cassandra-API synchronisieren.
author: abhirockzz
ms.service: cosmos-db
ms.subservice: cosmosdb-cassandra
ms.topic: how-to
ms.date: 01/05/2021
ms.author: abhishgu
ms.reviewer: abhishgu
ms.openlocfilehash: 0038219ee8c1721ff5ab2be76231d33d2bd9064d
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "98203064"
---
# <a name="migrate-data-from-postgresql-to-azure-cosmos-db-cassandra-api-account-using-apache-kafka"></a>Migrieren von Daten aus PostgreSQL zum Azure Cosmos DB-Cassandra-API-Konto mithilfe von Apache Kafka
[!INCLUDE[appliesto-cassandra-api](includes/appliesto-cassandra-api.md)]

Die Cassandra-API in Azure Cosmos DB hat sich aus einer Reihe unterschiedlicher Gründe zu einer guten Wahl für Unternehmensworkloads entwickelt, die unter Apache Cassandra ausgeführt werden:

* **Erhebliche Kosteneinsparungen:** Sie können Kosten sparen mit Azure Cosmos DB, einschließlich der Kosten für virtuelle Computer, Bandbreite und alle anwendbaren Oracle-Lizenzen. Außerdem entfällt die Verwaltung der Kosten für Rechenzentren, Server, SSD-Speicher, Netzwerk und Strom.

* **Bessere Skalierbarkeit und Verfügbarkeit:** Beseitigt einzelne Fehlerquellen, verbessert die Skalierbarkeit und Verfügbarkeit für Ihre Anwendungen.

* **Kein Mehraufwand für Verwaltung und Überwachung:** Als vollständig verwalteter Clouddienst entfernt Azure Cosmos DB den Mehraufwand der Verwaltung und Überwachung von unzähligen Einstellungen.

[Kafka Connect](https://kafka.apache.org/documentation/#connect) ist eine Plattform zum Streamen von Daten zwischen [Apache Kafka](https://kafka.apache.org/) und anderen Systemen auf skalierbare und zuverlässige Weise. Sie unterstützt mehrere einsatzbereite Connectors, sodass Sie keinen benutzerdefinierten Code benötigen, um externe Systeme in Apache Kafka zu integrieren.

In diesem Artikel wird veranschaulicht, wie Sie eine Kombination aus Kafka-Connectors verwenden, um eine Datenpipeline zum fortlaufenden Synchronisieren von Datensätzen aus einer relationalen Datenbank wie [PostgreSQL](https://www.postgresql.org/) mit der [Azure Cosmos DB-Cassandra-API](cassandra-introduction.md) zu verwenden.

## <a name="overview"></a>Übersicht

Hier finden Sie eine allgemeine Übersicht über den gesamten Flow, der in diesem Artikel vorgestellt wird.

Die Daten in der PostgreSQL-Tabelle werden mithilfe des [Debezium PostgreSQL-Connectors](https://debezium.io/documentation/reference/1.2/connectors/postgresql.html) an Apache Kafka gepusht. Hierbei handelt es sich um einen **Quellconnector** für Kafka Connect. Einfüge-, Aktualisierungs- oder Löschvorgänge mit Datensätzen in der PostgreSQL-Tabelle werden als `change data`-Ereignisse erfasst und an Kafka-Themen gesendet. Der [DataStax Apache Kafka-Connector](https://docs.datastax.com/en/kafka/doc/kafka/kafkaIntro.html) (in Kafka Connect ein **Senkenconnector**) bildet den zweiten Teil der Pipeline. Er synchronisiert die Änderungsereignisse von Daten aus Kafka-Themen mit Tabellen der Azure Cosmos DB-Cassandra-API.

> [!NOTE]
> Durch die Verwendung spezifischer Features des DataStax Apache Kafka-Connectors können Sie Daten in mehrere Tabellen pushen. In diesem Beispiel hilft der Connector dabei, Datensätze zu Datenänderungen in zwei Cassandra-Tabellen zu speichern, die unterschiedliche Anforderungen an Abfragen unterstützen.

## <a name="prerequisites"></a>Voraussetzungen

* [Bereitstellen eines Azure Cosmos DB-Cassandra-API-Kontos](create-cassandra-dotnet.md#create-a-database-account)
* [Verwenden von cqlsh oder der gehosteten Shell für die Prüfung](cassandra-support.md#hosted-cql-shell-preview)
* JDK 8 oder höher
* [Docker](https://www.docker.com/) (optional)

## <a name="base-setup"></a>Basiseinrichtung

### <a name="set-up-postgresql-database-if-you-havent-already"></a>Richten Sie die PostgreSQL-Datenbank ein, falls dies noch nicht geschehen ist. 

Sie können eine lokale Datenbank verwenden, oder Sie können eine auf Ihren lokalen Computer [herunterladen und installieren](https://www.postgresql.org/download/). Es ist auch möglich, einen [Docker-Container](https://hub.docker.com/_/postgres) zu verwenden.

So starten Sie einen Container

```bash
docker run --rm -p 5432:5432 -e POSTGRES_PASSWORD=<enter password> postgres
```

Stellen Sie mithilfe des [`psql`](https://www.postgresql.org/docs/current/app-psql.html)-Clients eine Verbindung mit Ihrer PostgreSQL-Instanz her:

```bash
psql -h localhost -p 5432 -U postgres -W -d postgres
```

Erstellen Sie eine Tabelle zum Speichern von Daten zu Beispielbestellungen:

```sql
CREATE SCHEMA retail;

CREATE TABLE retail.orders_info (
    orderid SERIAL NOT NULL PRIMARY KEY,
    custid INTEGER NOT NULL,
    amount INTEGER NOT NULL,
    city VARCHAR(255) NOT NULL,
    purchase_time VARCHAR(40) NOT NULL
);
```

### <a name="using-the-azure-portal-create-the-cassandra-keyspace-and-the-tables-required-for-the-demo-application"></a>Erstellen Sie mithilfe des Azure-Portals den Cassandra-Keyspace und die Tabellen, die für die Demoanwendung erforderlich sind.

> [!NOTE]
> Verwenden Sie die gleichen Keyspace- und Tabellennamen wie nachfolgend angegeben.

```sql
CREATE KEYSPACE retail WITH REPLICATION = {'class' : 'NetworkTopologyStrategy', 'datacenter1' : 1};

CREATE TABLE retail.orders_by_customer (order_id int, customer_id int, purchase_amount int, city text, purchase_time timestamp, PRIMARY KEY (customer_id, purchase_time)) WITH CLUSTERING ORDER BY (purchase_time DESC) AND cosmosdb_cell_level_timestamp=true AND cosmosdb_cell_level_timestamp_tombstones=true AND cosmosdb_cell_level_timetolive=true;

CREATE TABLE retail.orders_by_city (order_id int, customer_id int, purchase_amount int, city text, purchase_time timestamp, PRIMARY KEY (city,order_id)) WITH cosmosdb_cell_level_timestamp=true AND cosmosdb_cell_level_timestamp_tombstones=true AND cosmosdb_cell_level_timetolive=true;
```

### <a name="setup-apache-kafka"></a>Einrichten von Apache Kafka 

In diesem Artikel wird ein lokaler Cluster verwendet, Sie können aber auch eine beliebige andere Option verwenden. [Laden Sie Kafka herunter](https://kafka.apache.org/downloads), entzippen Sie das Paket, und starten Sie ZooKeeper und den Kafka-Cluster.

```bash
cd <KAFKA_HOME>/bin

#start zookeeper
bin/zookeeper-server-start.sh config/zookeeper.properties

#start kafka (in another terminal)
bin/kafka-server-start.sh config/server.properties
```

### <a name="setup-connectors"></a>Einrichten der Connectors

Installieren Sie den Debezium PostgreSQL- und den DataStax Apache Kafka-Connector. Laden Sie das Plug-In-Archiv für den Debezium PostgreSQL-Connector herunter. Wenn Sie z. B. Version 1.3.0 des Connectors herunterladen möchten (zum Zeitpunkt des Verfassens die neueste Version), verwenden Sie [diesen Link](https://repo1.maven.org/maven2/io/debezium/debezium-connector-postgres/1.3.0.Final/debezium-connector-postgres-1.3.0.Final-plugin.tar.gz). Laden Sie den DataStax Apache Kafka-Connector unter [diesem Link](https://downloads.datastax.com/#akc) herunter.

Entzippen Sie beide Connectorarchive, und kopieren Sie die JAR-Dateien in den [Plug-In-Pfad von Kafka Connect](https://kafka.apache.org/documentation/#connectconfigs).


```bash
cp <path_to_debezium_connector>/*.jar <KAFKA_HOME>/libs
cp <path_to_cassandra_connector>/*.jar <KAFKA_HOME>/libs
```

> Weitere Informationen finden Sie in der Dokumentation zu [Debezium](https://debezium.io/documentation/reference/1.2/connectors/postgresql.html#postgresql-deploying-a-connector) und [DataStax](https://docs.datastax.com/en/kafka/doc/).

## <a name="configure-kafka-connect-and-start-data-pipeline"></a>Konfigurieren von Kafka Connect und Starten der Datenpipeline

### <a name="start-kafka-connect-cluster"></a>Starten des Kafka Connect-Clusters

```bash
cd <KAFKA_HOME>/bin
./connect-distributed.sh ../config/connect-distributed.properties
```

### <a name="start-postgresql-connector-instance"></a>Starten der PostgreSQL-Connectorinstanz

Speichern der Connectorkonfiguration (JSON) in einer Datei, z. B. `pg-source-config.json`

```json
{
    "name": "pg-orders-source",
    "config": {
        "connector.class": "io.debezium.connector.postgresql.PostgresConnector",
        "database.hostname": "localhost",
        "database.port": "5432",
        "database.user": "postgres",
        "database.password": "password",
        "database.dbname": "postgres",
        "database.server.name": "myserver",
        "plugin.name": "wal2json",
        "table.include.list": "retail.orders_info",
        "value.converter": "org.apache.kafka.connect.json.JsonConverter"
    }
}
```

So starten Sie die PostgreSQL-Connectorinstanz

```bash
curl -X POST -H "Content-Type: application/json" --data @pg-source-config.json http://localhost:8083/connectors
```

> [!NOTE]
> Für das Löschen verwenden Sie: `curl -X DELETE http://localhost:8083/connectors/pg-orders-source`


### <a name="insert-data"></a>Einfügen von Daten

Die Tabelle `orders_info` enthält Details zur Bestellung, z. B. Bestell-ID, Kunden-ID, Ort usw. Füllen Sie die Tabelle mit zufälligen Daten auf, indem Sie folgenden SQL-Code verwenden.

```sql
insert into retail.orders_info (
    custid, amount, city, purchase_time
)
select
    random() * 10000 + 1,
    random() * 200,
    ('{New Delhi,Seattle,New York,Austin,Chicago,Cleveland}'::text[])[ceil(random()*3)],
    NOW() + (random() * (interval '1 min'))
from generate_series(1, 10) s(i);
```

Damit sollten 10 Datensätze in die Tabelle eingefügt werden. Achten Sie darauf, dass die Anzahl der Datensätze in `generate_series(1, 10)` unten Ihrem Anforderungsbeispiel entspricht. Verwenden Sie zum Einfügen von `100` Datensätzen `generate_series(1, 100)`

So bestätigen Sie den Vorgang

```bash
select * from retail.orders_info;
```

Überprüfen der Change Data Capture-Ereignisse im Kafka-Thema

> [!NOTE]
> Beachten Sie, dass der Name des Themas gemäß der [Connectorkonvention](https://debezium.io/documentation/reference/1.3/connectors/postgresql.html#postgresql-topic-names) `myserver.retail.orders_info` lautet.

```bash
cd <KAFKA_HOME>/bin

./kafka-console-consumer.sh --topic myserver.retail.orders_info --bootstrap-server localhost:9092 --from-beginning
```

Die Änderungsereignisse zu den Daten sollten im JSON-Format angezeigt werden.

### <a name="start-datastax-apache-kafka-connector-instance"></a>Starten der DataStax Apache Kafka-Connectorinstanz

Speichern Sie die Connectorkonfiguration (JSON) in einer Datei (z. B. `cassandra-sink-config.json`), und passen Sie die Eigenschaften an Ihre Umgebung an.

```json
{
    "name": "kafka-cosmosdb-sink",
    "config": {
        "connector.class": "com.datastax.oss.kafka.sink.CassandraSinkConnector",
        "tasks.max": "1",
        "topics": "myserver.retail.orders_info",
        "contactPoints": "<Azure Cosmos DB account name>.cassandra.cosmos.azure.com",
        "loadBalancing.localDc": "<Azure Cosmos DB region e.g. Southeast Asia>",
        "datastax-java-driver.advanced.connection.init-query-timeout": 5000,
        "ssl.hostnameValidation": true,
        "ssl.provider": "JDK",
        "ssl.keystore.path": "<path to JDK keystore path e.g. <JAVA_HOME>/jre/lib/security/cacerts>",
        "ssl.keystore.password": "<keystore password: it is 'changeit' by default>",
        "port": 10350,
        "maxConcurrentRequests": 500,
        "maxNumberOfRecordsInBatch": 32,
        "queryExecutionTimeout": 30,
        "connectionPoolLocalSize": 4,
        "auth.username": "<Azure Cosmos DB user name (same as account name)>",
        "auth.password": "<Azure Cosmos DB password>",
        "topic.myserver.retail.orders_info.retail.orders_by_customer.mapping": "order_id=value.orderid, customer_id=value.custid, purchase_amount=value.amount, city=value.city, purchase_time=value.purchase_time",
        "topic.myserver.retail.orders_info.retail.orders_by_city.mapping": "order_id=value.orderid, customer_id=value.custid, purchase_amount=value.amount, city=value.city, purchase_time=value.purchase_time",
        "key.converter": "org.apache.kafka.connect.storage.StringConverter",
        "transforms": "unwrap",
        "transforms.unwrap.type": "io.debezium.transforms.ExtractNewRecordState",
        "offset.flush.interval.ms": 10000
    }
}
```

So starten Sie die Connectorinstanz

```bash
curl -X POST -H "Content-Type: application/json" --data @cassandra-sink-config.json http://localhost:8083/connectors
```

Der Connector sollte aktiv werden. Damit sollte auch die gesamte Pipeline von PostgreSQL zu Azure Cosmos DB betriebsbereit sein.

### <a name="query-azure-cosmos-db"></a>Abfragen einer Azure Cosmos DB

Überprüfen Sie die Cassandra-Tabellen in Azure Cosmos DB. Hier sind einige Abfragen, die Sie ausprobieren können:

```sql
select count(*) from retail.orders_by_customer;
select count(*) from retail.orders_by_city;

select * from retail.orders_by_customer;
select * from retail.orders_by_city;

select * from retail.orders_by_city where city='Seattle';
select * from retail.orders_by_customer where customer_id = 10;
```

Sie können noch weitere Daten in PostgreSQL einfügen und überprüfen, ob die Datensätze mit Azure Cosmos DB synchronisiert werden.

## <a name="next-steps"></a>Nächste Schritte

* [Integrieren von Apache Kafka und der Cassandra-API für Azure Cosmos DB mithilfe von Kafka Connect](cassandra-kafka-connect.md)
* [Integrieren von Apache Kafka Connect in Azure Event Hubs (Vorschau) mit Debezium für Change Data Capture](../event-hubs/event-hubs-kafka-connect-debezium.md)
* [Migrieren von Daten zur Azure Cosmos DB-Cassandra-API mithilfe von Blitzz](oracle-migrate-cosmos-db-blitzz.md)
* [Bereitstellen des Durchsatzes für Container und Datenbanken](set-throughput.md) 
* [Bewährte Methoden für Partitionsschlüssel](partitioning-overview.md#choose-partitionkey)
* Artikel zum [Schätzen des Durchsatzes (RU/s) mit dem Azure Cosmos DB Capacity Planner](estimate-ru-with-capacity-planner.md)

