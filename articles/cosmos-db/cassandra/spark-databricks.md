---
title: Zugriff auf die Azure Cosmos DB-Cassandra-API von Azure Databricks
description: Dieser Artikel behandelt die Arbeit mit der Azure Cosmos DB-Cassandra-API von Azure Databricks aus.
author: TheovanKraay
ms.author: thvankra
ms.reviewer: sngun
ms.service: cosmos-db
ms.subservice: cosmosdb-cassandra
ms.topic: how-to
ms.date: 09/24/2018
ms.openlocfilehash: 89986ca27b67f4bd420121c3113324e6e7397219
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2021
ms.locfileid: "122340090"
---
# <a name="access-azure-cosmos-db-cassandra-api-data-from-azure-databricks"></a>Zugriff auf die Azure Cosmos DB-Cassandra-API-Daten von Azure Databricks aus
[!INCLUDE[appliesto-cassandra-api](../includes/appliesto-cassandra-api.md)]

Dieser Artikel behandelt die Arbeit mit der Azure Cosmos DB-Cassandra-API von Spark aus mit [Azure Databricks](/azure/databricks/scenarios/what-is-azure-databricks).

## <a name="prerequisites"></a>Voraussetzungen

* [Bereitstellen eines Azure Cosmos DB-Cassandra-API-Kontos](manage-data-dotnet.md#create-a-database-account)

* [Überprüfen der Grundlagen des Herstellens der Verbindung mit der Azure Cosmos DB-Cassandra-API](connect-spark-configuration.md)

* [Bereitstellen eines Azure Databricks-Clusters](/azure/databricks/scenarios/quickstart-create-databricks-workspace-portal)

* [Überprüfen der Codebeispiele für die Arbeit mit der Cassandra-API](connect-spark-configuration.md#next-steps)

* [Verwenden von cqlsh für die Validierung, falls bevorzugt](connect-spark-configuration.md#connecting-to-azure-cosmos-db-cassandra-api-from-spark)

* **Konfiguration der Cassandra-API-Instanz für den Cassandra-Connector**:

  Für den Connector für die Cassandra-API müssen die Details der Cassandra-Verbindung als Teil des Spark-Kontexts initialisiert werden. Beim Starten eines Databricks-Notebooks ist der Spark-Kontext bereits initialisiert, und es ist nicht ratsam, den Start zu beenden, und den Kontext erneut zu initialisieren. Eine Lösung ist, die Konfiguration der Cassandra-API-Instanz auf Clusterebene hinzuzufügen, in der Cluster-Spark-Konfiguration. Dies ist eine einmalige Aktivität pro Cluster. Fügen Sie der Spark-Konfiguration den folgenden Code als ein durch Leerzeichen getrenntes Schlüssel-Wert-Paar hinzu:
 
  ```scala
  spark.cassandra.connection.host YOUR_COSMOSDB_ACCOUNT_NAME.cassandra.cosmosdb.azure.com
  spark.cassandra.connection.port 10350
  spark.cassandra.connection.ssl.enabled true
  spark.cassandra.auth.username YOUR_COSMOSDB_ACCOUNT_NAME
  spark.cassandra.auth.password YOUR_COSMOSDB_KEY
  ```

## <a name="add-the-required-dependencies"></a>Hinzufügen der erforderlichen Abhängigkeiten

* **Cassandra-Spark-Connector**: Zur Integration der Azure Cosmos DB-Cassandra-API in Spark sollte der Cassandra-Connector dem Azure Databricks-Cluster angefügt werden. So fügen Sie das Cluster an:

  * Überprüfen Sie die Version der Databricks Runtime, die Spark-Version. Suchen Sie dann die [Maven-Koordinaten](https://mvnrepository.com/artifact/com.datastax.spark/spark-cassandra-connector), die mit dem Cassandra-Spark-Connector kompatibel sind, und fügen Sie sie dem Cluster hinzu. Informationen zum Anfügen der Connector-Bibliothek an den Cluster finden Sie im Artikel [Upload a Maven package or Spark package (Hochladen eines Maven- oder Spark-Pakets)](https://docs.databricks.com/user-guide/libraries.html). Wir empfehlen die Auswahl der Version 7.5 der Databricks-Runtime, die Spark 3.0 unterstützt. Wählen Sie **Bibliotheken** > **Neu installieren** > **Maven** aus, und fügen Sie dann `com.datastax.spark:spark-cassandra-connector-assembly_2.12:3.0.0` in den Maven-Koordinaten hinzu, um den Apache Spark Cassandra-Connector ihrem Cluster hinzuzufügen. Bei Verwendung von Spark 2.x wird eine Umgebung mit Spark-Version 2.4.5 empfohlen, in der der Spark-Connector bei den maven-Koordinaten `com.datastax.spark:spark-cassandra-connector_2.11:2.4.3` verwendet wird.

* **Azure Cosmos DB Cassandra-API-spezifische Bibliothek**: Wenn Sie Spark 2.x verwenden, ist eine benutzerdefinierte Verbindungsfactory ist erforderlich, um die Wiederholungsrichtlinie des Cassandra-Spark-Connectors für die Azure Cosmos DB-Cassandra-API zu konfigurieren. Fügen Sie die `com.microsoft.azure.cosmosdb:azure-cosmos-cassandra-spark-helper:1.2.0`[Maven-Koordinaten](https://search.maven.org/artifact/com.microsoft.azure.cosmosdb/azure-cosmos-cassandra-spark-helper/1.2.0/jar) hinzu, um die Bibliothek dem Cluster anzufügen.

> [!NOTE]
> Wenn Sie Spark 3.0 oder höher verwenden, müssen Sie die oben genannte Cosmos DB Cassandra-API-spezifische Bibliothek nicht installieren.

## <a name="sample-notebooks"></a>Beispiel-Notebooks

Eine Liste mit Azure Databricks-[Beispiel-Notebooks](https://github.com/Azure-Samples/azure-cosmos-db-cassandra-api-spark-notebooks-databricks/tree/main/notebooks/scala) steht im GitHub-Repository zum Download zur Verfügung. Diese Beispiele zeigen Ihnen, wie Sie eine Verbindung mit der Cassandra-API von Azure Cosmos DB von Spark aus herstellen und verschiedene CRUD-Vorgänge an den Daten ausführen. Sie können auch in Ihren Databricks-Clusterarbeitsbereich [alle Notebooks importieren](https://github.com/Azure-Samples/azure-cosmos-db-cassandra-api-spark-notebooks-databricks/tree/main/dbc) und dort ausführen. 

## <a name="accessing-azure-cosmos-db-cassandra-api-from-spark-scala-programs"></a>Zugriff auf die Azure Cosmos DB-Cassandra-API von Spark Scala-Programmen aus

Als automatisierte Prozesse in Azure Databricks auszuführende Spark-Programme werden mit [spark-submit](https://spark.apache.org/docs/latest/submitting-applications.html) an den Cluster gesendet und zum Ausführen über die Azure Databricks-Aufträge eingeplant.

Die folgenden Links sollen Ihnen Hilfe zum Einstieg in das Erstellen von Spark Scala-Programmen für die Interaktion mit der Azure Cosmos DB-Cassandra-API bieten.
* [Herstellen einer Verbindung mit der Azure Cosmos DB-Cassandra-API von Spark Scala-Programmen aus](https://github.com/Azure-Samples/azure-cosmos-db-cassandra-api-spark-connector-sample/blob/main/src/main/scala/com/microsoft/azure/cosmosdb/cassandra/SampleCosmosDBApp.scala)
* [Ausführen eines Spark Scala-Programms als automatisierter Auftrag in Azure Databricks](/azure/databricks/jobs)
* [Vollständige Liste der Codebeispiele für die Arbeit mit der Cassandra-API](connect-spark-configuration.md#next-steps)

## <a name="next-steps"></a>Nächste Schritte

Erste Schritte mit dem [Erstellen eines Cassandra-API-Kontos, einer Datenbank und einer Tabelle](create-account-java.md) mithilfe einer Java-Anwendung.
