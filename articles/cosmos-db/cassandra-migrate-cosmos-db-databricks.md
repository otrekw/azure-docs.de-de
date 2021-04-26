---
title: Migrieren von Daten aus Apache Cassandra zur Azure Cosmos DB-Cassandra-API mithilfe von Databricks (Spark)
description: Erfahren Sie, wie Sie Daten mithilfe von Azure Databricks und Spark aus einer Apache Cassandra-Datenbank zur Azure Cosmos DB-Cassandra-API migrieren.
author: TheovanKraay
ms.service: cosmos-db
ms.subservice: cosmosdb-cassandra
ms.topic: how-to
ms.date: 03/10/2021
ms.author: thvankra
ms.reviewer: thvankra
ms.openlocfilehash: caedefbf3887205b68bcd5de5e7cd5f1f7d7f53c
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "104801008"
---
# <a name="migrate-data-from-cassandra-to-an-azure-cosmos-db-cassandra-api-account-by-using-azure-databricks"></a>Migrieren von Daten aus Cassandra zu einem Azure Cosmos DB-Cassandra-API-Konto mithilfe von Azure Databricks
[!INCLUDE[appliesto-cassandra-api](includes/appliesto-cassandra-api.md)]

Die Cassandra-API in Azure Cosmos DB hat sich aus mehreren Gründen zu einer guten Wahl für Unternehmensworkloads entwickelt, die unter Apache Cassandra ausgeführt werden:

* **Kein Mehraufwand für Verwaltung und Überwachung:** Beseitigt den Mehraufwand der Verwaltung und Überwachung von Einstellungen für Betriebssysteme, JVM und YAML-Dateien und deren Interaktionen.

* **Erhebliche Kosteneinsparungen:** Sie können Kosten sparen mit Azure Cosmos DB, einschließlich der Kosten für virtuelle Computer, Bandbreite und alle anwendbaren Lizenzen. Die Verwaltung von Kosten für Rechenzentren, Server, SSD-Speicher, Netzwerk und Strom entfallen.

* **Möglichkeit der Verwendung von vorhandenem Code und Tools**: Azure Cosmos DB bietet Kompatibilität auf Verbindungsprotokollebene mit vorhandenen SDKs und Tools. Durch diese Kompatibilität ist sichergestellt, dass Sie die vorhandene Codebasis mit der Cassandra-API für Azure Cosmos DB mit nur geringfügigen Änderungen verwenden können.

Es gibt viele Möglichkeiten, Datenbankworkloads von einer Plattform zu einer anderen zu migrieren. [Azure Databricks](https://azure.microsoft.com/services/databricks/) ist ein Platform as a Service-Angebot (PaaS) für [Apache Spark](https://spark.apache.org/), das Offlinemigrationsvorgänge in großem Umfang ermöglicht. In diesem Artikel werden die erforderlichen Schritte zum Migrieren von Daten aus nativen Apache Cassandra-Keyspaces und -Tabellen zur Azure Cosmos DB-Cassandra-API mithilfe von Azure Databricks beschrieben.

## <a name="prerequisites"></a>Voraussetzungen

* [Ein Azure Cosmos DB-Cassandra-API-Konto muss vorhanden sein.](create-cassandra-dotnet.md#create-a-database-account)

* [Überprüfen der Grundlagen des Herstellens der Verbindung mit einer Azure Cosmos DB-Cassandra-API](cassandra-spark-generic.md)

* Überprüfen Sie die [in der Azure Cosmos DB-Cassandra-API unterstützten Features](cassandra-support.md), um die Kompatibilität sicherzustellen.

* Vergewissern Sie sich, dass im Zielkonto der Azure Cosmos DB-Cassandra-API bereits leere Keyspaces und Tabellen erstellt wurden.

* [Verwenden von cqlsh oder der gehosteten Shell für die Prüfung](cassandra-support.md#hosted-cql-shell-preview)

## <a name="provision-an-azure-databricks-cluster"></a>Bereitstellen eines Azure Databricks-Clusters

Nutzen Sie die Anweisungen zum [Bereitstellen eines Azure Databricks-Clusters](/azure/databricks/scenarios/quickstart-create-databricks-workspace-portal). Wir empfehlen die Auswahl der Version 7.5 der Databricks-Runtime, die Spark 3.0 unterstützt.

:::image type="content" source="./media/cassandra-migrate-cosmos-db-databricks/databricks-runtime.png" alt-text="Screenshot, der die Suche nach der Databricks-Laufzeitversion zeigt.":::

## <a name="add-dependencies"></a>Hinzufügen von Abhängigkeiten

Sie müssen dem Cluster die Apache Spark-Cassandra-Connectorbibliothek hinzufügen, um eine Verbindung mit nativen und Azure Cosmos DB-Cassandra-Endpunkten herzustellen. Wählen Sie in Ihrem Cluster **Bibliotheken**  >  **Neue**  >  **Maven** installieren und fügen Sie dann `com.datastax.spark:spark-cassandra-connector-assembly_2.12:3.0.0` in Maven-Koordinaten hinzu.

:::image type="content" source="./media/cassandra-migrate-cosmos-db-databricks/databricks-search-packages.png" alt-text="Screenshot, der zeigt, wie Maven-Pakete in Databricks gesucht werden.":::

Wählen Sie **Installieren** aus, und starten Sie den Cluster nach Abschluss der Installation neu.

> [!NOTE]
> Stellen Sie sicher, dass Sie den Databricks-Cluster neu starten, nachdem die Cassandra-Connectorbibliothek installiert wurde.

## <a name="create-scala-notebook-for-migration"></a>Erstellen eines Scala-Notebooks für die Migration

Erstellen eines Scala Notebooks in Databricks. Ersetzen Sie Ihre Quell- und Zielkonfigurationen für Cassandra durch die entsprechenden Anmeldeinformationen und die Quell-und Zielkeyspaces und -tabellen. Führen Sie dann den folgenden Code aus:

```scala
import com.datastax.spark.connector._
import com.datastax.spark.connector.cql._
import org.apache.spark.SparkContext

// source cassandra configs
val nativeCassandra = Map( 
    "spark.cassandra.connection.host" -> "<Source Cassandra Host>",
    "spark.cassandra.connection.port" -> "9042",
    "spark.cassandra.auth.username" -> "<USERNAME>",
    "spark.cassandra.auth.password" -> "<PASSWORD>",
    "spark.cassandra.connection.ssl.enabled" -> "false",
    "keyspace" -> "<KEYSPACE>",
    "table" -> "<TABLE>"
)

//target cassandra configs
val cosmosCassandra = Map( 
    "spark.cassandra.connection.host" -> "<USERNAME>.cassandra.cosmos.azure.com",
    "spark.cassandra.connection.port" -> "10350",
    "spark.cassandra.auth.username" -> "<USERNAME>",
    "spark.cassandra.auth.password" -> "<PASSWORD>",
    "spark.cassandra.connection.ssl.enabled" -> "true",
    "keyspace" -> "<KEYSPACE>",
    "table" -> "<TABLE>",
    //throughput related settings below - tweak these depending on data volumes. 
    "spark.cassandra.output.batch.size.rows"-> "1",
    "spark.cassandra.output.concurrent.writes" -> "1000",
    "spark.cassandra.concurrent.reads" -> "512",
    "spark.cassandra.output.batch.grouping.buffer.size" -> "1000",
    "spark.cassandra.connection.keep_alive_ms" -> "600000000"
)

//Read from native Cassandra
val DFfromNativeCassandra = sqlContext
  .read
  .format("org.apache.spark.sql.cassandra")
  .options(nativeCassandra)
  .load
  
//Write to CosmosCassandra
DFfromNativeCassandra
  .write
  .format("org.apache.spark.sql.cassandra")
  .options(cosmosCassandra)
  .mode(SaveMode.Append)
  .save
```

> [!NOTE]
> Die `spark.cassandra.output.batch.size.rows` und `spark.cassandra.output.concurrent.writes` Werte und die Anzahl der Worker in Ihrem Spark-Cluster sind wichtige Konfigurationen, die optimiert werden müssen, um die [Ratenbegrenzung](/samples/azure-samples/azure-cosmos-cassandra-java-retry-sample/azure-cosmos-db-cassandra-java-retry-sample/)zu vermeiden. Die Ratenbegrenzung erfolgt, wenn die Anforderungen an Azure Cosmos DB den bereitgestellten Durchsatz oder die [Anforderungseinheiten](./request-units.md) (Request Units, RUS) überschreiten. Sie müssen diese Einstellungen möglicherweise abhängig von der Anzahl der Executors im Spark-Cluster und eventuell der Größe (und somit der RU-Kosten) jedes in die Zieltabellen geschriebenen Datensatzes anpassen.

## <a name="troubleshoot"></a>Problembehandlung

### <a name="rate-limiting-429-error"></a>Ratenbegrenzung (Fehler 429)

Möglicherweise wird der Fehlercode 429 oder "Anforderungsrate ist groß" angezeigt, auch wenn Sie die Einstellungen auf die Mindestwerte reduziert haben. Die folgenden Szenarien können die Ratenbegrenzung verursachen:

* **Der der Tabelle zugewiesene Durchsatz ist kleiner als 6,000 [Anforderungseinheiten](./request-units.md)** . Selbst bei minimalen Einstellungen kann Spark Schreibvorgänge mit einer Rate ab 6,000 Anforderungseinheiten ausführen. Wenn Sie eine Tabelle in einem Keyspace mit gemeinsamem Durchsatz bereitgestellt haben, ist es möglich, dass diese Tabelle zur Laufzeit weniger als 6,000 Anforderungseinheiten zur Verfügung hat.

    Stellen Sie sicher, dass die Tabelle, zu der migriert wird, mindestens 6.000 RUS verfügbar hat, wenn Sie die Migration ausführen. Weisen Sie der Tabelle ggf. dedizierte Anforderungseinheiten zu.

* **Übermäßige Datenschiefe bei großen Datenmengen**. Wenn Sie sehr viele Daten in eine bestimmte Tabelle migrieren müssen, aber eine erhebliche Schiefe in den Daten vorliegt (d. h. eine große Anzahl von Datensätzen, die für denselben Partitionsschlüsselwert geschrieben werden), kann es trotzdem zu einer Ratenbegrenzung kommen, selbst wenn Sie mehrere [Anforderungseinheiten](./request-units.md) in Ihrer Tabelle bereitgestellt haben. Anforderungseinheiten werden gleichmäßig auf die physischen Partitionen verteilt werden und eine starke Datenschiefe kann einen Engpass bei Anforderungen an eine einzelne Partition verursachen.

    In diesem Szenario reduzieren Sie die Durchsatzeinstellungen in Spark auf ein Minimum und erzwingen Sie eine langsame Ausführung der Migration. Dieses Szenario kann häufiger vorkommen, wenn Referenz- oder Steuertabellen migriert werden, bei denen der Zugriff weniger häufig erfolgt und die Datenschiefe hoch sein kann. Wenn jedoch eine erhebliche Schiefe in einem anderen Tabellentyp vorhanden ist, sollten Sie möglicherweise Ihr Datenmodell überprüfen, um Probleme mit heißen Partitionen für die Workload bei stabilen Vorgängen zu vermeiden.

## <a name="next-steps"></a>Nächste Schritte

* [Bereitstellen des Durchsatzes für Container und Datenbanken](set-throughput.md)
* [Bewährte Methoden für Partitionsschlüssel](partitioning-overview.md#choose-partitionkey)
* [Schätzen des Durchsatzes (RU/s) mit dem Azure Cosmos DB Capacity Planner](estimate-ru-with-capacity-planner.md)
* [Elastische Skalierung in der Cassandra-API von Azure Cosmos DB](manage-scale-cassandra.md)
