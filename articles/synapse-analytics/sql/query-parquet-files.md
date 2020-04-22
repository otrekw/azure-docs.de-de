---
title: Abfragen von Parquet-Dateien mit SQL On-Demand (Vorschauversion)
description: In diesem Artikel erfahren Sie, wie Sie Parquet-Dateien mit SQL On-Demand abfragen können (Vorschauversion).
services: synapse analytics
author: azaricstefan
ms.service: synapse-analytics
ms.topic: how-to
ms.subservice: ''
ms.date: 04/15/2020
ms.author: v-stazar
ms.reviewer: jrasnick, carlrab
ms.openlocfilehash: 0b272a8c8ce81fc40585014e5930f5d7b1b5f2c0
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2020
ms.locfileid: "81427590"
---
# <a name="query-parquet-files-using-sql-on-demand-preview-in-azure-synapse-analytics"></a>Abfragen von Parquet-Dateien mit SQL On-Demand (Vorschauversion) in Azure Synapse Analytics

In diesem Artikel erfahren Sie, wie Sie eine Abfrage mit SQL On-Demand (Vorschauversion) schreiben können, die Parquet-Dateien lesen kann.

## <a name="prerequisites"></a>Voraussetzungen

Lesen Sie die folgenden Artikel, bevor Sie den Rest dieses Artikels lesen:

- [Erstmalige Einrichtung](query-data-storage.md#first-time-setup)
- [Voraussetzungen](query-data-storage.md#prerequisites)

## <a name="dataset"></a>Dataset

Sie können Parquet-Dateien auf dieselbe Weise abfragen wie CSV-Dateien. Der einzige Unterschied besteht darin, dass der FILEFORMAT-Parameter auf PARQUET festgelegt werden muss. In den Beispielen in diesem Artikel werden die Besonderheiten beim Lesen von Parquet-Dateien erläutert.

> [!NOTE]
> Beim Lesen von Parquet-Dateien müssen Sie in der OPENROWSET WITH-Klausel keine Spalten angeben. SQL On-Demand nutzt die Metadaten in der Parquet-Datei und bindet die Spalten nach Namen.

Sie werden den Ordner *parquet/taxi* für die Beispielabfragen verwenden. Er enthält Daten der Fahrtenaufzeichnungen für „NYC Taxi – Yellow Taxi“ von Juli 2016 bis Juni 2018.

Die Daten sind nach Jahr und Monat partitioniert und die Ordnerstruktur ist wie folgt:

- year=2016
  - month=6
  - ...
  - month=12
- year=2017
  - month=1
  - ...
  - month=12
- year=2018
  - month=1
  - ...
  - month=6

## <a name="query-set-of-parquet-files"></a>Abfragesatz von Parquet-Dateien

Bei der Abfrage von Parquet-Dateien können Sie nur die relevanten Spalten angeben.

```sql
SELECT
        YEAR(pickup_datetime),
        passenger_count,
        COUNT(*) AS cnt
FROM  
    OPENROWSET(
        BULK 'https://sqlondemandstorage.blob.core.windows.net/parquet/taxi/*/*/*',
        FORMAT='PARQUET'
    ) WITH (
        pickup_datetime DATETIME2,
        passenger_count INT
    ) AS nyc
GROUP BY
    passenger_count,
    YEAR(pickup_datetime)
ORDER BY
    YEAR(pickup_datetime),
    passenger_count;
```

## <a name="automatic-schema-inference"></a>Automatischer Schemarückschluss

Sie brauchen die OPENROWSET WITH-Klausel nicht zu verwenden, wenn Sie Parquet-Dateien lesen. Spaltennamen und Datentypen werden automatisch aus Parquet-Dateien gelesen.

Das folgende Beispiel zeigt die automatischen Schemarückschlussfunktionen für Parquet-Dateien. Hierzu wird die Anzahl von Zeilen im September 2017 ohne Angabe eines Schemas zurückgegeben.

> [!NOTE]
> Beim Lesen von Parquet-Dateien müssen Sie in der OPENROWSET WITH-Klausel keine Spalten angeben. In diesem Fall verwendet der SQL On-Demand-Abfragedienst die Metadaten in der Parquet-Datei und bindet die Spalten nach Namen.

```sql
SELECT
    COUNT_BIG(*)
FROM
    OPENROWSET(
        BULK 'https://sqlondemandstorage.blob.core.windows.net/parquet/taxi/year=2017/month=9/*.parquet',
        FORMAT='PARQUET'
    ) AS nyc;
```

### <a name="query-partitioned-data"></a>Abfragen von partitionierten Daten

Das in diesem Beispiel bereitgestellte Dataset ist in separate Unterordner aufgeteilt (partitioniert). Mithilfe der filepath-Funktion können Sie bestimmte Partitionen als Ziel verwenden. Dieses Beispiel zeigt die Fahrpreisbeträge nach Jahr, Monat und Zahlungsart für die ersten drei Monate des Jahres 2017.

> [!NOTE]
> Die SQL On-Demand-Abfrage ist mit dem Hive/Hadoop-Partitionierungsschema kompatibel.

```sql
SELECT
    nyc.filepath(1) AS [year],
    nyc.filepath(2) AS [month],
    payment_type,
    SUM(fare_amount) AS fare_total
FROM
    OPENROWSET(
        BULK 'https://sqlondemandstorage.blob.core.windows.net/parquet/taxi/year=*/month=*/*.parquet',
        FORMAT='PARQUET'
    ) AS nyc
WHERE
    nyc.filepath(1) = 2017
    AND nyc.filepath(2) IN (1, 2, 3)
    AND pickup_datetime BETWEEN CAST('1/1/2017' AS datetime) AND CAST('3/31/2017' AS datetime)
GROUP BY
    nyc.filepath(1),
    nyc.filepath(2),
    payment_type
ORDER BY
    nyc.filepath(1),
    nyc.filepath(2),
    payment_type;
```

## <a name="type-mapping"></a>Typzuordnung

Parquet-Dateien enthalten Typbeschreibungen für die einzelnen Spalten. In der folgenden Tabelle wird beschrieben, wie Parquet-Typen den nativen SQL-Typen zugeordnet werden.

| Parquet-Typ | Logischer Parquet-Typ (Anmerkung) | SQL-Datentyp |
| --- | --- | --- |
| BOOLEAN | | bit |
| BINARY/BYTE_ARRAY | | varbinary |
| Double | | float |
| GLEITKOMMAZAHL | | real |
| INT32 | | INT |
| INT64 | | BIGINT |
| INT96 | |datetime2 |
| FIXED_LEN_BYTE_ARRAY | |BINARY |
| BINARY |UTF8 |varchar \*(UTF8-Sortierung) |
| BINARY |STRING |varchar \*(UTF8-Sortierung) |
| BINARY |ENUM|varchar \*(UTF8-Sortierung) |
| BINARY |UUID |UNIQUEIDENTIFIER |
| BINARY |DECIMAL |Decimal |
| BINARY |JSON |varchar(max) \*(UTF8-Sortierung) |
| BINARY |BSON |varbinary(max) |
| FIXED_LEN_BYTE_ARRAY |DECIMAL |Decimal |
| BYTE_ARRAY |INTERVAL |varchar(max), in ein standardisiertes Format serialisiert |
| INT32 |INT(8, true) |SMALLINT |
| INT32 |INT(16, true) |SMALLINT |
| INT32 |INT(32, true) |INT |
| INT32 |INT(8, false) |TINYINT |
| INT32 |INT(16, false) |INT |
| INT32 |INT(32, false) |BIGINT |
| INT32 |DATE |date |
| INT32 |DECIMAL |Decimal |
| INT32 |TIME (MILLIS)|time |
| INT64 |INT(64, true) |BIGINT |
| INT64 |INT(64, false) |decimal(20,0) |
| INT64 |DECIMAL |Decimal |
| INT64 |TIME (MICROS/NANOS) |time |
|INT64 |TIMESTAMP (MILLIS/MICROS/NANOS) |datetime2 |
|[Komplexer Typ](https://github.com/apache/parquet-format/blob/master/LogicalTypes.md#lists) |AUFLISTEN |varchar(max), serialisiert in JSON |
|[Komplexer Typ](https://github.com/apache/parquet-format/blob/master/LogicalTypes.md#maps)|MAP|varchar(max), serialisiert in JSON |

## <a name="next-steps"></a>Nächste Schritte

Wechseln Sie zum nächsten Artikel, um Informationen zum [Abfragen geschachtelter Parquet-Typen](query-parquet-nested-types.md) zu erhalten.
