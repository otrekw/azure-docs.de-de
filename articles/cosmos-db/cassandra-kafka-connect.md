---
title: Integrieren von Apache Kafka und der Cassandra-API für Azure Cosmos DB mithilfe von Kafka Connect
description: Erfahren Sie, wie mithilfe von DataStax Apache Kafka Connector Daten aus Kafka in der Cassandra-API für Azure Cosmos DB erfasst werden.
author: abhirockzz
ms.service: cosmos-db
ms.subservice: cosmosdb-cassandra
ms.topic: how-to
ms.date: 12/14/2020
ms.author: abhishgu
ms.reviewer: abhishgu
ms.openlocfilehash: 25972ba2bb30c39838c4822a42af292e8d8b1dba
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "97803628"
---
# <a name="ingest-data-from-apache-kafka-into-azure-cosmos-db-cassandra-api-using-kafka-connect"></a>Erfassen von Daten aus Apache Kafka in der Cassandra-API für Azure Cosmos DB mithilfe von Kafka Connect
[!INCLUDE[appliesto-cassandra-api](includes/appliesto-cassandra-api.md)]

Vorhandene Cassandra-Anwendungen können aufgrund der [Kompatibilität der CQLv4-Treiber](https://cassandra.apache.org/doc/latest/getting_started/drivers.html?highlight=driver) problemlos mit der [Cassandra-API für Azure Cosmos DB](cassandra-introduction.md) zusammenarbeiten. Sie nutzen diese Fähigkeit, um Streamingplattformen wie [Apache Kafka](https://kafka.apache.org/) zu integrieren und Daten in Azure Cosmos DB einzubinden.

Daten in Apache Kafka (Themen) sind nur nützlich, wenn Sie von anderen Anwendungen verwendet oder in anderen Systemen erfasst werden. Sie können eine Lösung über die [Producer- und Consumer-APIs von Kafka](https://kafka.apache.org/documentation/#api) mit [einer Sprache und einem Client-SDK Ihrer Wahl](https://cwiki.apache.org/confluence/display/KAFKA/Clients) erstellen. Kafka Connect bietet eine alternative Lösung. Es ist eine Plattform zum Streamen von Daten zwischen Apache Kafka und anderen Systemen auf skalierbare und zuverlässige Weise. Da Kafka Connect Standardconnectors unterstützt, zu denen auch Cassandra zählt, müssen Sie keinen benutzerdefinierten Code schreiben, um Kafka in die Cassandra-API für Azure Cosmos DB zu integrieren. 

In diesem Artikel wird [DataStax Apache Kafka Connector](https://docs.datastax.com/en/kafka/doc/kafka/kafkaIntro.html) verwendet, eine Open-Source-Software, die auf dem Kafka Connect-Framework basiert und Datensätze aus einem Kafka-Thema in den Zeilen einer oder mehrerer Cassandra-Tabellen erfasst. Das Beispiel enthält eine wiederverwendbare Einrichtung mithilfe von Docker Compose. Dies ist recht praktisch, da es Ihnen ermöglicht, alle erforderlichen Komponenten lokal mit einem einzigen Befehl per Bootstrap zu starten. Zu diese Komponenten gehören Kafka, Zookeeper, Kafka Connect-Worker und die Datengenerator-Beispielanwendung.

Es folgt eine Auflistung der Komponenten und ihrer Dienstdefinitionen. Die vollständige `docker-compose`-Datei finden Sie [im GitHub-Repository](https://github.com/Azure-Samples/cosmosdb-cassandra-kafka/blob/main/docker-compose.yaml).

- Kafka und Zookeeper verwenden [Debezium](https://hub.docker.com/r/debezium/kafka/)-Images.
- Zur Ausführung als Docker-Container wird DataStax Apache Kafka Connector auf einem vorhandenen Docker-Image aufgesetzt ([Debezium/Connect-Basis](https://github.com/debezium/docker-images/tree/master/connect-base/1.2)). Dieses Image umfasst eine Installation von Kafka und die zugehörigen Kafka Connect-Bibliotheken, sodass benutzerdefinierte Connectors auf einfache Weise hinzugefügt werden können. Informationen finden Sie in der [Dockerfile](https://github.com/Azure-Samples/cosmosdb-cassandra-kafka/blob/main/connector/Dockerfile).
- Der `data-generator`-Dienst fügt zufällig generierte (JSON-) Daten in das Kafka-Thema `weather-data` ein. Den Code und die `Dockerfile` finden Sie im [GitHub-Repository](https://github.com/Azure-Samples/cosmosdb-cassandra-kafka/blob/main/data-generator/).

## <a name="prerequisites"></a>Voraussetzungen

* [Bereitstellen eines Azure Cosmos DB-Cassandra-API-Kontos](create-cassandra-dotnet.md#create-a-database-account)

* [Verwenden von cqlsh oder der gehosteten Shell für die Prüfung](cassandra-support.md#hosted-cql-shell-preview)

* Installieren von [Docker](https://docs.docker.com/get-docker/) und [Docker Compose](https://docs.docker.com/compose/install)

## <a name="create-keyspace-tables-and-start-the-integration-pipeline"></a>Erstellen von Keyspace und Tabellen und Starten der Integrationspipeline

Erstellen Sie mithilfe des Azure-Portals den Cassandra-Keyspace und die Tabellen, die für die Demoanwendung erforderlich sind.

> [!NOTE]
> Verwenden Sie die gleichen Keyspace- und Tabellennamen wie nachfolgend angegeben.

```sql
CREATE KEYSPACE weather WITH REPLICATION = {'class' : 'NetworkTopologyStrategy', 'datacenter1' : 1};

CREATE TABLE weather.data_by_state (station_id text, temp int, state text, ts timestamp, PRIMARY KEY (state, ts)) WITH CLUSTERING ORDER BY (ts DESC) AND cosmosdb_cell_level_timestamp=true AND cosmosdb_cell_level_timestamp_tombstones=true AND cosmosdb_cell_level_timetolive=true;

CREATE TABLE weather.data_by_station (station_id text, temp int, state text, ts timestamp, PRIMARY KEY (station_id, ts)) WITH CLUSTERING ORDER BY (ts DESC) AND cosmosdb_cell_level_timestamp=true AND cosmosdb_cell_level_timestamp_tombstones=true AND cosmosdb_cell_level_timetolive=true;
```

Klonen Sie das GitHub-Repository:

```bash
git clone https://github.com/Azure-Samples/cosmosdb-cassandra-kafka
cd cosmosdb-cassandra-kafka
```

Starten Sie alle Dienste:

```shell
docker-compose --project-name kafka-cosmos-cassandra up --build
```

> [!NOTE]
> Das Herunterladen und Starten der Container kann einige Zeit in Anspruch nehmen. Dies ist jedoch ein einmaliger Vorgang.

Überprüfen Sie wie folgt, ob alle Container gestartet wurden:

```shell
docker-compose -p kafka-cosmos-cassandra ps
```

Die Datengeneratoranwendung beginnt, das Thema `weather-data` in Kafka mit Daten zu füllen. Sie können auch eine schnelle Integritätsprüfung durchführen, um dies zu bestätigen. Sehen Sie sich den Docker-Container an, der den Kafka Connect-Worker ausführt:


```bash
docker exec -it kafka-cosmos-cassandra_cassandra-connector_1 bash
```

Sobald Sie die Containershell aufgerufen haben, starten Sie einfach den üblichen Consumerprozess auf der Kafka-Konsole. Daraufhin sollte das Einfließen von Wetterdaten (im JSON-Format) zu sehen sein.

```bash
cd ../bin
./kafka-console-consumer.sh --bootstrap-server kafka:9092 --topic weather-data
```

## <a name="cassandra-sink-connector-setup"></a>Einrichtung des Cassandra Sink-Connectors

Kopieren Sie den folgenden JSON-Inhalt in eine Datei (dieser können Sie den Namen `cassandra-sink-config.json` geben). Sie müssen diese Datei entsprechend Ihrer Einrichtung aktualisieren. Der restliche Teil dieses Abschnitts enthält Anleitungen dazu.

```json
{
    "name": "kafka-cosmosdb-sink",
    "config": {
        "connector.class": "com.datastax.oss.kafka.sink.CassandraSinkConnector",
        "tasks.max": "1",
        "topics": "weather-data",
        "contactPoints": "<cosmos db account name>.cassandra.cosmos.azure.com",
        "port": 10350,
        "loadBalancing.localDc": "<cosmos db region e.g. Southeast Asia>",
        "auth.username": "<enter username for cosmosdb account>",
        "auth.password": "<enter password for cosmosdb account>",
        "ssl.hostnameValidation": true,
        "ssl.provider": "JDK",
        "ssl.keystore.path": "/etc/alternatives/jre/lib/security/cacerts/",
        "ssl.keystore.password": "changeit",
        "datastax-java-driver.advanced.connection.init-query-timeout": 5000,
        "maxConcurrentRequests": 500,
        "maxNumberOfRecordsInBatch": 32,
        "queryExecutionTimeout": 30,
        "connectionPoolLocalSize": 4,
        "topic.weather-data.weather.data_by_state.mapping": "station_id=value.stationid, temp=value.temp, state=value.state, ts=value.created",
        "topic.weather-data.weather.data_by_station.mapping": "station_id=value.stationid, temp=value.temp, state=value.state, ts=value.created",
        "key.converter": "org.apache.kafka.connect.storage.StringConverter",
        "value.converter": "org.apache.kafka.connect.json.JsonConverter",
        "value.converter.schemas.enable": false,
        "offset.flush.interval.ms": 10000
    }
}
```

Im Folgenden finden Sie eine Übersicht über die Attribute:

**Grundlegende Konnektivität**

- `contactPoints`: Geben Sie den Kontaktpunkt für Cosmos DB Cassandra ein.
- `loadBalancing.localDc`: Geben Sie die Region für das Cosmos DB-Konto ein, z. B. „Asien, Südosten“.
- `auth.username`: Geben Sie den Benutzernamen ein.
- `auth.password`: Geben Sie das Kennwort ein.
- `port`: Geben Sie den Portwert ein. (Dies ist `10350` und nicht `9042`. Behalten Sie den Wert unverändert bei.)

**SSL-Konfiguration**

Azure Cosmos DB erzwingt [sichere Konnektivität über SSL](database-security.md) und der Kafka Connect-Connector unterstützt ebenfalls SSL.

- `ssl.keystore.path`: Pfad zum JDK-Keystore im Container: `/etc/alternatives/jre/lib/security/cacerts/`
- `ssl.keystore.password`: Kennwort (Standard) für den JDK-Keystore
- `ssl.hostnameValidation`: Die Überprüfung des Knotenhostnamens wird aktiviert.
- `ssl.provider`: `JDK` wird als SSL-Anbieter verwendet.

**Generische Parameter**

- `key.converter`: Der Zeichenfolgenkonverter `org.apache.kafka.connect.storage.StringConverter` wird verwendet.
- `value.converter`: Da es sich bei den Daten in Kafka-Themen um JSON-Daten handelt, wird `org.apache.kafka.connect.json.JsonConverter` verwendet.
- `value.converter.schemas.enable`: Da der JSON-Nutzlast kein Schema zugeordnet ist (für die Demo-App), muss Kafka Connect angewiesen werden, nicht nach einem Schema zu suchen. Dazu wird dieses Attribut auf `false` festgelegt. Andernfalls treten Fehler auf.

### <a name="install-the-connector"></a>Installieren des Connectors

Installieren Sie den Connector mithilfe des Kafka Connect-REST-Endpunkts:

```shell
curl -X POST -H "Content-Type: application/json" --data @cassandra-sink-config.json http://localhost:8083/connectors
```

Überprüfen Sie den Status wie folgt:

```
curl http://localhost:8080/connectors/kafka-cosmosdb-sink/status
```

Wenn alles gut läuft, sollte der Connector mit seinem Wunderwerk beginnen. Er sollte sich bei Azure Cosmos DB authentifizieren und mit dem Erfassen von Daten aus dem Kafka-Thema (`weather-data`) in die Cassandra-Tabellen `weather.data_by_state` und `weather.data_by_station` beginnen.

Sie können nun Daten in den Tabellen abfragen. Wechseln Sie zum Azure-Portal, und rufen Sie die gehostete CQL-Shell für Ihr Azure Cosmos DB-Konto auf.

:::image type="content" source="./media/cassandra-kafka-connect/cqlsh.png" alt-text="Öffnen von CQLSH":::

## <a name="query-data-from-azure-cosmos-db"></a>Abfragen von Daten aus Azure Cosmos DB

Überprüfen Sie die Tabellen `data_by_state` und `data_by_station`. Es folgen einige Beispielabfragen für den Einstieg:

```sql
select * from weather.data_by_state where state = 'state-1';
select * from weather.data_by_state where state IN ('state-1', 'state-2');
select * from weather.data_by_state where state = 'state-3' and ts > toTimeStamp('2020-11-26');

select * from weather.data_by_station where station_id = 'station-1';
select * from weather.data_by_station where station_id IN ('station-1', 'station-2');
select * from weather.data_by_station where station_id IN ('station-2', 'station-3') and ts > toTimeStamp('2020-11-26');
```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

[!INCLUDE [cosmosdb-delete-resource-group](../../includes/cosmos-db-delete-resource-group.md)]

## <a name="next-steps"></a>Nächste Schritte

* [Bereitstellen des Durchsatzes für Container und Datenbanken](set-throughput.md) 
* [Bewährte Methoden für Partitionsschlüssel](partitioning-overview.md#choose-partitionkey)
* Artikel zum [Schätzen des Durchsatzes (RU/s) mit dem Azure Cosmos DB Capacity Planner](estimate-ru-with-capacity-planner.md)
