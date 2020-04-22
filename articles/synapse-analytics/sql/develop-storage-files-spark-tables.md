---
title: Abfragen von Spark-Tabellen mithilfe von SQL On-Demand (Vorschau)
description: Übersicht über das Abfragen von Spark-Tabellen mithilfe von SQL On-Demand (Vorschau)
services: synapse-analytics
author: julieMSFT
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: ''
ms.date: 04/15/2020
ms.author: jrasnick
ms.reviewer: jrasnick
ms.openlocfilehash: 41e31a322a3d771557474fdf5c318960822bcfe1
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2020
ms.locfileid: "81420074"
---
# <a name="query-spark-tables-with-azure-synapse-analytics-using-sql-on-demand-preview"></a>Abfragen von Spark-Tabellen mit Azure Synapse Analytics mithilfe von SQL On-Demand (Vorschau)

SQL On-Demand (Vorschau) kann automatisch Metadaten aus Spark-Pools im Synapse-Arbeitsbereich synchronisieren (Vorschau). Für jede in Spark-Pools vorhandene Datenbank wird eine SQL On-Demand-Datenbank erstellt (Vorschau). Für jede auf Parquet oder CSV basierende Spark-Tabelle wird in der SQL On-Demand-Datenbank eine externe Tabelle erstellt. Aus diesem Grund können Sie Spark-Pools herunterfahren und dennoch Spark-Tabellen über SQL On-Demand abfragen.

Wenn Tabelle in Spark partitioniert ist, werden die Dateien im Speicher in Ordner organisiert. SQL On-Demand verwendet Partitionsmetadaten und bezieht nur relevante Ordner und Dateien in Ihre Abfragen ein.

Die Synchronisierung von Metadaten wird für jeden im Azure Synapse-Arbeitsbereich bereitgestellten Spark-Pool automatisch konfiguriert. Sie können sofort mit dem Abfragen von Spark-Tabellen beginnen.

Jede Spark-Tabelle wird durch eine externe Tabelle in einem dbo-Schema repräsentiert, das einer SQL On-Demand-Datenbank entspricht. Führen Sie für Spark-Tabellen eine Abfrage in einer externen [spark_table] aus. Bevor Sie das unten stehende Beispiel ausführen, stellen Sie sicher, dass Sie ordnungsgemäßen [Zugriff auf das Speicherkonto haben](develop-storage-files-storage-access-control.md), in dem sich die Dateien befinden.

```sql
SELECT * FROM [db].dbo.[spark_table]
```

## <a name="spark-data-types-to-sql-data-types-mapping"></a>Zuordnung von Spark-Datentypen zu SQL-Datentypen

| Spark-Datentyp | SQL-Datentyp               |
| --------------- | --------------------------- |
| ByteType        | SMALLINT                    |
| ShortType       | SMALLINT                    |
| IntegerType     | INT                         |
| LongType        | BIGINT                      |
| FloatType       | real                        |
| DoubleType      | float                       |
| DecimalType     | Decimal                     |
| TimestampType   | datetime2                   |
| DateType        | date                        |
| StringType      | varchar(max)*               |
| BinaryType      | varbinary                   |
| BooleanType     | bit                         |
| ArrayType       | varchar(max)* (in JSON)** |
| MapType         | varchar(max)* (in JSON)** |
| StructType      | varchar(max)* (in JSON)** |

\* Die verwendete Sortierung ist Latin1_General_100_BIN2_UTF8.

** ArrayType, MapType und StructType werden als JSONs dargestellt.



## <a name="next-steps"></a>Nächste Schritte

Im Artikel [Steuerung des Speicherzugriffs](develop-storage-files-storage-access-control.md) erfahren Sie mehr über die Steuerung des Speicherzugriffs.
