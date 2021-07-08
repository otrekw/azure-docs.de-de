---
title: Livemigration zu Azure Managed Instance for Apache Cassandra mithilfe von Apache Spark und eines Proxys für duales Schreiben
description: Hier erhalten Sie Informationen zur Migration zu Azure Managed Instance for Apache Cassandra mithilfe von Apache Spark und eines Proxys für duales Schreiben.
author: TheovanKraay
ms.author: thvankra
ms.service: managed-instance-apache-cassandra
ms.topic: overview
ms.date: 06/02/2021
ms.openlocfilehash: 2d82e4fa00c387ca2b7de58b223a6abf11ad1491
ms.sourcegitcommit: c072eefdba1fc1f582005cdd549218863d1e149e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/10/2021
ms.locfileid: "111956030"
---
# <a name="live-migration-to-azure-managed-instance-for-apache-cassandra-using-dual-write-proxy"></a>Livemigration zu Azure Managed Instance for Apache Cassandra mithilfe eines Proxys für duales Schreiben

> [!IMPORTANT]
> Azure Managed Instance for Apache Cassandra befindet sich derzeit in der Public Preview-Phase.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar.
> Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Es wird empfohlen, nach Möglichkeit die native Apache Cassandra-Funktion zur Migration von Daten aus Ihrem vorhandenen Cluster zu Azure Managed Instance for Apache Cassandra zu verwenden, indem Sie einen [Hybridcluster](configure-hybrid-cluster.md) konfigurieren. Dabei wird das Gossipprotokoll von Apache Cassandra verwendet, um Daten nahtlos aus Ihrem Quellrechenzentrum in Ihr neues Rechenzentrum der verwalteten Instanz zu replizieren. In manchen Szenarios kann es jedoch möglich sein, dass die Version Ihrer Quelldatenbank nicht kompatibel ist oder ein anderer Grund das Einrichten eines Hybridclusters verhindert. In diesem Artikel wird beschrieben, wie Sie Daten live mithilfe eines [Proxys für duales Schreiben](https://github.com/Azure-Samples/cassandra-proxy) und Apache Spark zu Azure Managed Instance for Apache Cassandra migrieren. Diese Methode bietet folgende Vorteile:

- **Minimale Anwendungsänderungen**: Der Proxy kann Verbindungen von Ihrem Anwendungscode mit wenigen oder gar keinen Konfigurationsänderungen akzeptieren, alle Anforderungen an Ihre Quelldatenbank weiterleiten und Schreibvorgänge asynchron an ein sekundäres Ziel weiterleiten. 
- **Abhängigkeit vom Wire Protocol des Clients**: Da diese Methode nicht von Back-End-Ressourcen oder internen Protokollen abhängig ist, kann sie für jedes Cassandra-Quell- oder -Zielsystem verwendet werden, das das Apache Cassandra-Wire Protocol implementiert.

In der folgenden Abbildung wird diese Methode veranschaulicht:


:::image type="content" source="./media/migration/live-migration.gif" alt-text="Azure Managed Instance for Apache Cassandra ist ein verwalteter Dienst für Apache Cassandra." border="false":::

## <a name="prerequisites"></a>Voraussetzungen

* Stellen Sie über das [Azure-Portal](create-cluster-portal.md) oder die [Azure CLI](create-cluster-cli.md) einen Cluster für Azure Managed Instance for Apache Cassandra bereit, und stellen Sie sicher, dass Sie mit [CQLSH eine Verbindung mit Ihrem Cluster herstellen können](./create-cluster-portal.md#connecting-to-your-cluster).

* [Stellen Sie in Ihrem verwalteten Cassandra-VNET ein Azure Databricks-Konto bereit](deploy-cluster-databricks.md). Stellen Sie sicher, dass es auch Netzwerkzugriff auf Ihren Cassandra-Quellcluster besitzt. In diesem Konto erstellen Sie einen Spark-Cluster zum Laden der Verlaufsdaten.

* Vergewissern Sie sich, dass Sie das Keyspace-/Tabellenschema aus Ihrer Cassandra-Quelldatenbank in die Zieldatenbank Ihrer verwalteten Cassandra-Instanz migriert haben.


## <a name="provision-a-spark-cluster"></a>Bereitstellen eines Spark-Clusters

Es wird empfohlen, Version 7.5 der Azure Databricks-Runtime auszuwählen, die Spark 3.0 unterstützt.

:::image type="content" source="../cosmos-db/media/cassandra-migrate-cosmos-db-databricks/databricks-runtime.png" alt-text="Screenshot, der die Suche nach der Databricks-Laufzeitversion zeigt.":::

## <a name="add-spark-dependencies"></a>Hinzufügen von Spark-Abhängigkeiten

Sie müssen dem Cluster die Apache Spark-Cassandra-Connectorbibliothek hinzufügen, um eine Verbindung mit nativen und Azure Cosmos DB-Cassandra-Endpunkten herzustellen. Wählen Sie in Ihrem Cluster **Bibliotheken**  >  **Neue**  >  **Maven** installieren und fügen Sie dann `com.datastax.spark:spark-cassandra-connector-assembly_2.12:3.0.0` in Maven-Koordinaten hinzu.

:::image type="content" source="../cosmos-db/media/cassandra-migrate-cosmos-db-databricks/databricks-search-packages.png" alt-text="Screenshot, der zeigt, wie Maven-Pakete in Databricks gesucht werden.":::

Wählen Sie **Installieren** aus, und starten Sie den Cluster nach Abschluss der Installation neu.

> [!NOTE]
> Stellen Sie sicher, dass Sie den Databricks-Cluster neu starten, nachdem die Cassandra-Connectorbibliothek installiert wurde.

## <a name="install-dual-write-proxy"></a>Installieren eines Proxys für duales Schreiben

Für eine optimale Leistung bei dualen Schreibvorgängen wird empfohlen, den Proxy auf allen Knoten in Ihrem Cassandra-Quellcluster zu installieren.

```bash
#assuming you do not have git already installed
sudo apt-get install git 

#assuming you do not have maven already installed
sudo apt install maven

#clone repo for dual-write proxy
git clone https://github.com/Azure-Samples/cassandra-proxy.git

#change directory
cd cassandra-proxy

#compile the proxy
mvn package
```

## <a name="start-dual-write-proxy"></a>Starten des Proxys für duales Schreiben

Es wird empfohlen, den Proxy auf allen Knoten in Ihrem Cassandra-Quellcluster zu installieren. Sie müssen mindestens den folgenden Befehl ausführen, um den Proxy auf allen Knoten zu starten. Ersetzen Sie `<target-server>` durch eine IP- oder Serveradresse eines der Knoten im Zielcluster. Ersetzen Sie `<path to JKS file>` durch den Pfad zu einer lokalen JKS-Datei und `<keystore password>` durch das entsprechende Kennwort:  

```bash
java -jar target/cassandra-proxy-1.0-SNAPSHOT-fat.jar localhost <target-server> --proxy-jks-file <path to JKS file> --proxy-jks-password <keystore password>
```
Für SSL können Sie entweder einen vorhandenen Keystore implementieren (z. B. den von Ihrem Quellcluster verwendeten Keystore), oder Sie können mit keytool ein selbstsigniertes Zertifikat erstellen:

```bash
keytool -genkey -keyalg RSA -alias selfsigned -keystore keystore.jks -storepass password -validity 360 -keysize 2048
```

> [!NOTE]
> Stellen Sie sicher, dass Ihre Clientanwendung beim Erstellen von SSL-Verbindungen mit der Datenbank über den Proxy denselben Keystore und dasselbe Kennwort wie für den Proxy für duales Schreiben verwendet.

Wenn Sie den Proxy auf diese Weise starten, muss Folgendes zutreffen:

- Quell- und Zielendpunkte besitzen denselben Benutzernamen und dasselbe Kennwort.
- Quell- und Zielendpunkte implementieren SSL.

Standardmäßig werden die Anmeldeinformationen der Quelle von Ihrer Client-App übergeben und vom Proxy zum Herstellen einer Verbindung mit den Quell- und Zielclustern verwendet. Bei Bedarf können Sie den Benutzernamen und das Kennwort des Cassandra-Zielendpunkts beim Starten des Proxys separat angeben:

```bash
java -jar target/cassandra-proxy-1.0-SNAPSHOT-fat.jar localhost <target-server> --proxy-jks-file <path to JKS file> --proxy-jks-password <keystore password> --target-username <username> --target-password <password>
```

Werden die Quell- und Zielports nicht angegeben, lautet der Standardwert `9042`. Werden der Cassandra-Quell- oder Zielendpunkt an einem anderen Port ausgeführt, können Sie mit `--source-port` oder `--target-port` eine andere Portnummer angeben. 

```bash
java -jar target/cassandra-proxy-1.0-SNAPSHOT-fat.jar localhost <target-server> --source-port 9042 --target-port 10350 --proxy-jks-file <path to JKS file> --proxy-jks-password <keystore password> --target-username <username> --target-password <password>
```

Wenn die Quell- oder Zielendpunkte SSL nicht implementieren, können Sie dieses auch deaktivieren. Verwenden Sie hierzu die Flags `--disable-source-tls` oder `--disable-target-tls`:

```bash
java -jar target/cassandra-proxy-1.0-SNAPSHOT-fat.jar localhost <target-server> --source-port 9042 --target-port 10350 --proxy-jks-file <path to JKS file> --proxy-jks-password <keystore password> --target-username <username> --target-password <password> --disable-source-tls true  --disable-target-tls true 
```

Möglicherweise möchten Sie den Proxy nicht auf den Clusterknoten selbst, sondern auf einem separaten Computer installieren. Geben Sie in diesem Fall die IP von `<source-server>` an:

```bash
java -jar target/cassandra-proxy-1.0-SNAPSHOT-fat.jar <source-server> <destination-server>
```

> [!NOTE]
> Wenn Sie den Proxy in einem nativen Apache Cassandra-Cluster nicht auf allen Knoten installieren und ausführen, wirkt sich dies auf die Leistung Ihrer Anwendung aus, da der Clienttreiber nicht für alle Knoten innerhalb des Clusters eine Verbindung öffnen kann. 

Der Proxy lauscht standardmäßig an Port 29042. Sie können den Port, an dem der Proxy lauscht, jedoch auch ändern. Dies kann etwa der Fall sein, wenn Sie Codeänderungen auf Anwendungsebene vermeiden möchten. Führen Sie hierzu den Cassandra-Quellserver an einem anderen Port und den Proxy am Cassandra-Standardport aus:

```bash
java -jar target/cassandra-proxy-1.0-SNAPSHOT-fat.jar source-server destination-server --proxy-port 9042
```

> [!NOTE]
> Für die Installation des Proxys auf den Clusterknoten ist kein Neustart der Knoten erforderlich. Wenn Sie jedoch über viele Anwendungsclients verfügen und den Proxy auf dem Cassandra-Standardport 9042 ausführen, um Codeänderungen auf Anwendungsebene zu vermeiden, müssen Sie den Cluster neu starten. 

Der Proxy verfügt über Funktionen zum Erzwingen von Protokollen, die erforderlich sein können, wenn der Quellendpunkt fortgeschrittener ist als das Ziel. Geben Sie in diesem Fall `--protocol-version` und `--cql-version` an:

```bash
java -jar target/cassandra-proxy-1.0-SNAPSHOT-fat.jar source-server destination-server --protocol-version 4 --cql-version 3.11
```

Sobald der Proxy für duales Schreiben ausgeführt wird, müssen Sie den Port auf Ihrem Anwendungsclient ändern und neu starten (bzw. den Cassandra-Port ändern und den Cluster neu starten, wenn Sie sich für diese Methode entschieden haben). Der Proxy beginnt nun mit der Weiterleitung von Schreibvorgängen an den Zielendpunkt. Weitere Informationen zur [Überwachung und zu Metriken](https://github.com/Azure-Samples/cassandra-proxy#monitoring) im Proxytool finden Sie hier. 


## <a name="run-the-historic-data-load"></a>Laden der Verlaufsdaten

Erstellen Sie zum Laden der Daten ein Scala-Notebook in Ihrem Databricks-Konto. Ersetzen Sie Ihre Quell- und Zielkonfigurationen für Cassandra durch die entsprechenden Anmeldeinformationen und die Quell-und Zielkeyspaces und -tabellen. Fügen Sie im folgenden Beispiel nach Bedarf für jede Tabelle weitere Variablen hinzu, und führen Sie den Code anschließend aus. Nachdem Ihre Anwendung mit dem Senden von Anforderungen an den Proxy für duales Schreiben begonnen hat, können Sie die Verlaufsdaten migrieren. 

```scala
import com.datastax.spark.connector._
import com.datastax.spark.connector.cql._
import org.apache.spark.SparkContext

// source cassandra configs
val sourceCassandra = Map( 
    "spark.cassandra.connection.host" -> "<Source Cassandra Host>",
    "spark.cassandra.connection.port" -> "9042",
    "spark.cassandra.auth.username" -> "<USERNAME>",
    "spark.cassandra.auth.password" -> "<PASSWORD>",
    "spark.cassandra.connection.ssl.enabled" -> "true",
    "keyspace" -> "<KEYSPACE>",
    "table" -> "<TABLE>"
)

//target cassandra configs
val targetCassandra = Map( 
    "spark.cassandra.connection.host" -> "<Source Cassandra Host>",
    "spark.cassandra.connection.port" -> "9042",
    "spark.cassandra.auth.username" -> "<USERNAME>",
    "spark.cassandra.auth.password" -> "<PASSWORD>",
    "spark.cassandra.connection.ssl.enabled" -> "true",
    "keyspace" -> "<KEYSPACE>",
    "table" -> "<TABLE>",
    //throughput related settings below - tweak these depending on data volumes. 
    "spark.cassandra.output.batch.size.rows"-> "1",
    "spark.cassandra.output.concurrent.writes" -> "1000",
    "spark.cassandra.connection.remoteConnectionsPerExecutor" -> "1",
    "spark.cassandra.concurrent.reads" -> "512",
    "spark.cassandra.output.batch.grouping.buffer.size" -> "1000",
    "spark.cassandra.connection.keep_alive_ms" -> "600000000"
)

//set timestamp to ensure it is before read job starts
val timestamp: Long = System.currentTimeMillis / 1000

//Read from source Cassandra
val DFfromSourceCassandra = sqlContext
  .read
  .format("org.apache.spark.sql.cassandra")
  .options(sourceCassandra)
  .load
  
//Write to target Cassandra
DFfromSourceCassandra
  .write
  .format("org.apache.spark.sql.cassandra")
  .options(targetCassandra)
  .option("writetime", timestamp)
  .mode(SaveMode.Append)
  .save
```

> [!NOTE]
> Wie Sie im obigen Scala-Beispiel sehen, wird `timestamp` vor dem Lesen aller Daten in der Quelltabelle auf die aktuelle Uhrzeit und `writetime` anschließend auf diesen rückdatierten Zeitstempel festgelegt. Dadurch soll sichergestellt werden, dass Datensätze, die aus den Verlaufsdaten auf den Zielendpunkt geschrieben werden, keine Updates mit einem späteren Zeitstempel aus dem Proxy für duales Schreiben überschreiben können, während die Verlaufsdaten gelesen werden. Falls Sie jedoch die *genauen* Zeitstempel beibehalten müssen, sollten Sie die Verlaufsdaten auf eine Weise migrieren, bei der die Zeitstempel beibehalten werden, wie etwa in [diesem Beispiel](https://github.com/scylladb/scylla-migrator). 

## <a name="validation"></a>Überprüfen

Sobald das Laden der Verlaufsdaten abgeschlossen ist, sollten Ihre Datenbanken synchron und zur Übernahme bereit sein. Vor der endgültigen Übernahme wird jedoch eine Überprüfung der Quelle und des Ziels empfohlen, um sicherzustellen, dass die Anforderungsergebnisse übereinstimmen.


## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Verwalten von Azure Managed Instance for Apache Cassandra-Ressourcen mit der Azure CLI](manage-resources-cli.md)