---
title: Abfragen des Azure Cosmos DB-Analysespeichers mit Apache Spark für Azure Synapse Analytics
description: Hier erfahren Sie, wie Sie den Azure Cosmos DB-Analysespeicher mit Apache Spark für Azure Synapse Analytics abfragen.
services: synapse-analytics
author: ArnoMicrosoft
ms.service: synapse-analytics
ms.topic: quickstart
ms.subservice: ''
ms.date: 05/06/2020
ms.author: acomet
ms.reviewer: jrasnick
ms.openlocfilehash: 02d4b6a636bff5ef11686abba6efb52f45f04779
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2020
ms.locfileid: "83599252"
---
# <a name="query-azure-cosmos-db-analytical-store-with-apache-spark-for-azure-synapse-analytics"></a>Abfragen des Azure Cosmos DB-Analysespeichers mit Apache Spark für Azure Synapse Analytics

Dieser Artikel enthält einige Beispiele dafür, wie Sie mithilfe von Synapse-Gesten mit dem Analysespeicher interagieren können. Gesten werden angezeigt, wenn Sie mit der rechten Maustaste auf einen Container klicken. Mit Gesten können Sie schnell Code generieren und an Ihre Anforderungen anpassen. Gesten eignen sich auch ideal zum Ermitteln von Daten mit nur einem Mausklick.

## <a name="load-to-dataframe"></a>In Datenrahmen laden

In diesem Schritt lesen Sie Daten aus einem Azure Cosmos DB-Analysespeicher in einen Spark-Datenrahmen. Es werden zehn Zeilen aus dem Datenrahmen namens ***df*** angezeigt. Wurden Ihre Daten in den Datenrahmen gelesen, können Sie weitere Analysen ausführen.

Dieser Vorgang wirkt sich nicht auf den Transaktionsspeicher aus.

```python
# To select a preferred list of regions in a multi-region Azure Cosmos DB account, add .option("spark.cosmos.preferredRegions", "<Region1>,<Region2>")

df = spark.read.format("cosmos.olap")\
    .option("spark.synapse.linkedService", "INFERRED")\
    .option("spark.cosmos.container", "INFERRED")\
    .load()

df.show(10)
```

Die entsprechende Codegeste in **Scala** entspräche dem folgenden Code:
```java
// To select a preferred list of regions in a multi-region Azure Cosmos DB account, add option("spark.cosmos.preferredRegions", "<Region1>,<Region2>")

val df_olap = spark.read.format("cosmos.olap").
    option("spark.synapse.linkedService", "pySparkSamplesDb").
    option("spark.cosmos.container", "trafficSourceColl").
    load()
```

## <a name="create-spark-table"></a>Spark-Tabelle erstellen

Mit dieser Geste erstellen Sie eine Spark-Tabelle, die auf den ausgewählten Container verweist. Dieser Vorgang verursacht keinerlei Datenverschiebungen. Wenn Sie diese Tabelle löschen möchten, wirkt sich dies nicht auf den zugrunde liegenden Container (und den entsprechenden Analysespeicher) aus. 

Dieses Szenario ist praktisch, wenn Sie Tabellen über Drittanbietertools wiederverwenden und den Zugriff auf die Daten für die Laufzeit bereitstellen möchten.

```sql
%%sql
-- To select a preferred list of regions in a multi-region Azure Cosmos DB account, add spark.cosmos.preferredRegions '<Region1>,<Region2>' in the config options

create table call_center using cosmos.olap options (
    spark.synapse.linkedService 'INFERRED',
    spark.cosmos.container 'INFERRED'
)
```

## <a name="write-dataframe-to-container"></a>Datenrahmen in Container schreiben

Mit dieser Geste schreiben Sie einen Datenrahmen in einen Container. Dieser Vorgang wirkt sich auf die Transaktionsleistung und die Nutzung von Anforderungseinheiten aus. Azure Cosmos DB-Transaktionsleistung eignet sich ideal für Schreibtransaktionen. Ersetzen Sie **YOURDATAFRAME** unbedingt durch den Datenrahmen, in den Sie zurückschreiben möchten.

```python
# Write a Spark DataFrame into an Azure Cosmos DB container
# To select a preferred list of regions in a multi-region Azure Cosmos DB account, add .option("spark.cosmos.preferredRegions", "<Region1>,<Region2>")


YOURDATAFRAME.write.format("cosmos.oltp")\
    .option("spark.synapse.linkedService", "INFERRED")\
    .option("spark.cosmos.container", "INFERRED")\
    .option("spark.cosmos.write.upsertEnabled", "true")\
    .mode('append')\
    .save()
```

Die entsprechende Codegeste in **Scala** entspräche dem folgenden Code:
```java
// To select a preferred list of regions in a multi-region Azure Cosmos DB account, add option("spark.cosmos.preferredRegions", "<Region1>,<Region2>")

import org.apache.spark.sql.SaveMode

df.write.format("cosmos.oltp").
    option("spark.synapse.linkedService", "pySparkSamplesDb").
    option("spark.cosmos.container", "trafficSourceColl"). 
    option("spark.cosmos.write.upsertEnabled", "true").
    mode(SaveMode.Overwrite).
    save()
```

## <a name="load-streaming-dataframe-from-container"></a>Streamingdatenrahmen aus Container laden
Bei dieser Geste verwenden Sie die Spark-Streamingfunktion, um Daten aus einem Container in einen Datenrahmen zu laden. Die Daten werden im primären Data Lake-Konto (und Dateisystem) gespeichert, das Sie mit dem Arbeitsbereich verbunden haben. 

Wurde der Ordner */localReadCheckpointFolder* nicht erstellt, wird er automatisch erstellt. Dieser Vorgang wirkt sich auf die Transaktionsleistung von Azure Cosmos DB aus.

```python
# To select a preferred list of regions in a multi-region Azure Cosmos DB account, add .option("spark.cosmos.preferredRegions", "<Region1>,<Region2>")

dfStream = spark.readStream\
    .format("cosmos.oltp")\
    .option("spark.synapse.linkedService", "INFERRED")\
    .option("spark.cosmos.container", "INFERRED")\
    .option("spark.cosmos.changeFeed.readEnabled", "true")\
    .option("spark.cosmos.changeFeed.startFromTheBeginning", "true")\
    .option("spark.cosmos.changeFeed.checkpointLocation", "/localReadCheckpointFolder")\
    .option("spark.cosmos.changeFeed.queryName", "streamQuery")\
    .load()
```

Die entsprechende Codegeste in **Scala** entspräche dem folgenden Code:
```java
// To select a preferred list of regions in a multi-region Azure Cosmos DB account, add .option("spark.cosmos.preferredRegions", "<Region1>,<Region2>")

val dfStream = spark.readStream.
    format("cosmos.oltp").
    option("spark.synapse.linkedService", "pySparkSamplesDb").
    option("spark.cosmos.container", "trafficSourceColl").
    option("spark.cosmos.changeFeed.readEnabled", "true").
    option("spark.cosmos.changeFeed.startFromTheBeginning", "true").
    option("spark.cosmos.changeFeed.checkpointLocation", "/localReadCheckpointFolder").
    option("spark.cosmos.changeFeed.queryName", "streamTestRevin2").
    load()
```

## <a name="write-streaming-dataframe-to-container"></a>Streamingdatenrahmen in Container schreiben
Bei dieser Geste schreiben Sie einen Streamingdatenrahmen in den von Ihnen ausgewählten Azure Cosmos DB-Container. Wurde der Ordner */localReadCheckpointFolder* nicht erstellt, wird er automatisch erstellt. Dieser Vorgang wirkt sich auf die Transaktionsleistung von Azure Cosmos DB aus.

```python
# To select a preferred list of regions in a multi-region Azure Cosmos DB account, add .option("spark.cosmos.preferredRegions", "<Region1>,<Region2>")

streamQuery = dfStream\
        .writeStream\
        .format("cosmos.oltp")\
        .outputMode("append")\
        .option("checkpointLocation", "/localWriteCheckpointFolder")\
        .option("spark.synapse.linkedService", "INFERRED")\
        .option("spark.cosmos.container", "trafficSourceColl_sink")\
        .option("spark.cosmos.connection.mode", "gateway")\
        .start()

streamQuery.awaitTermination()
```

Die entsprechende Codegeste in **Scala** entspräche dem folgenden Code:
```java
// To select a preferred list of regions in a multi-region Azure Cosmos DB account, add .option("spark.cosmos.preferredRegions", "<Region1>,<Region2>")

val query = dfStream.
            writeStream.
            format("cosmos.oltp").
            outputMode("append").
            option("checkpointLocation", "/localWriteCheckpointFolder").
            option("spark.synapse.linkedService", "pySparkSamplesDb").
            option("spark.cosmos.container", "test2").
            option("spark.cosmos.connection.mode", "gateway").
            start()

query.awaitTermination()
```
## <a name="next-steps"></a>Nächste Schritte

* [Informationen zu den unterstützten Features zwischen Synapse und Azure Cosmos DB](./concept-synapse-link-cosmos-db-support.md)
* [Herstellen einer Verbindung mit Synapse Link für Azure Cosmos DB](../quickstart-connect-synapse-link-cosmos-db.md)
