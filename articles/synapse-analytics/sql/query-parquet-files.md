---
title: Abfragen von Parquet-Dateien mit SQL On-Demand (Vorschauversion)
description: In diesem Artikel erfahren Sie, wie Sie Parquet-Dateien mit SQL On-Demand abfragen können (Vorschauversion).
services: synapse analytics
author: azaricstefan
ms.service: synapse-analytics
ms.topic: how-to
ms.subservice: sql
ms.date: 05/20/2020
ms.author: v-stazar
ms.reviewer: jrasnick, carlrab
ms.openlocfilehash: 4bab1ef4588a705f0dd6cdb34be8272868f826e9
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85207565"
---
# <a name="query-parquet-files-using-sql-on-demand-preview-in-azure-synapse-analytics"></a>Abfragen von Parquet-Dateien mit SQL On-Demand (Vorschauversion) in Azure Synapse Analytics

In diesem Artikel erfahren Sie, wie Sie eine Abfrage mit SQL On-Demand (Vorschauversion) schreiben können, die Parquet-Dateien lesen kann.

## <a name="prerequisites"></a>Voraussetzungen

Der erste Schritt besteht in der **Erstellung einer Datenbank** mit einer Datenquelle, die auf das Speicherkonto [NYC Yellow Taxi](https://azure.microsoft.com/services/open-datasets/catalog/nyc-taxi-limousine-commission-yellow-taxi-trip-records/) verweist. Initialisieren Sie dann die Objekte, indem Sie das [Setupskript](https://github.com/Azure-Samples/Synapse/blob/master/SQL/Samples/LdwSample/SampleDB.sql) für diese Datenbank ausführen. Mit diesem Setupskript werden die Datenquellen, die für die gesamte Datenbank gültigen Anmeldeinformationen und externe Dateiformate erstellt, die in diesen Beispielen verwendet werden.

## <a name="dataset"></a>Dataset

In diesem Beispiel wird das Dataset [NYC Yellow Taxi](https://azure.microsoft.com/services/open-datasets/catalog/nyc-taxi-limousine-commission-yellow-taxi-trip-records/) verwendet. Sie können Parquet-Dateien auf dieselbe Weise abfragen wie Sie [CSV-Dateien lesen](query-parquet-files.md). Der einzige Unterschied besteht darin, dass der Parameter `FILEFORMAT` auf `PARQUET` festgelegt werden muss. In den Beispielen in diesem Artikel werden die Besonderheiten beim Lesen von Parquet-Dateien erläutert.

## <a name="query-set-of-parquet-files"></a>Abfragesatz von Parquet-Dateien

Bei der Abfrage von Parquet-Dateien können Sie nur die relevanten Spalten angeben.

```sql
SELECT
        YEAR(tpepPickupDateTime),
        passengerCount,
        COUNT(*) AS cnt
FROM  
    OPENROWSET(
        BULK 'puYear=2018/puMonth=*/*.snappy.parquet',
        DATA_SOURCE = 'YellowTaxi',
        FORMAT='PARQUET'
    ) WITH (
        tpepPickupDateTime DATETIME2,
        passengerCount INT
    ) AS nyc
GROUP BY
    passengerCount,
    YEAR(tpepPickupDateTime)
ORDER BY
    YEAR(tpepPickupDateTime),
    passengerCount;
```

## <a name="automatic-schema-inference"></a>Automatischer Schemarückschluss

Sie brauchen die OPENROWSET WITH-Klausel nicht zu verwenden, wenn Sie Parquet-Dateien lesen. Spaltennamen und Datentypen werden automatisch aus Parquet-Dateien gelesen.

Das folgende Beispiel zeigt die automatischen Schemarückschlussfunktionen für Parquet-Dateien. Hierzu wird die Anzahl von Zeilen im September 2017 ohne Angabe eines Schemas zurückgegeben.

> [!NOTE]
> Beim Lesen von Parquet-Dateien müssen Sie in der OPENROWSET WITH-Klausel keine Spalten angeben. In diesem Fall verwendet der SQL On-Demand-Abfragedienst die Metadaten in der Parquet-Datei und bindet die Spalten nach Namen.

```sql
SELECT TOP 10 *
FROM  
    OPENROWSET(
        BULK 'puYear=2018/puMonth=*/*.snappy.parquet',
        DATA_SOURCE = 'YellowTaxi',
        FORMAT='PARQUET'
    ) AS nyc
```

### <a name="query-partitioned-data"></a>Abfragen von partitionierten Daten

Das in diesem Beispiel bereitgestellte Dataset ist in separate Unterordner aufgeteilt (partitioniert). Mithilfe der filepath-Funktion können Sie bestimmte Partitionen als Ziel verwenden. Dieses Beispiel zeigt die Fahrpreisbeträge nach Jahr, Monat und Zahlungsart für die ersten drei Monate des Jahres 2017.

> [!NOTE]
> Die SQL On-Demand-Abfrage ist mit dem Hive/Hadoop-Partitionierungsschema kompatibel.

```sql
SELECT
        YEAR(tpepPickupDateTime),
        passengerCount,
        COUNT(*) AS cnt
FROM  
    OPENROWSET(
        BULK 'puYear=*/puMonth=*/*.snappy.parquet',
        DATA_SOURCE = 'YellowTaxi',
        FORMAT='PARQUET'
    ) nyc
WHERE
    nyc.filepath(1) = 2017
    AND nyc.filepath(2) IN (1, 2, 3)
    AND tpepPickupDateTime BETWEEN CAST('1/1/2017' AS datetime) AND CAST('3/31/2017' AS datetime)
GROUP BY
    passengerCount,
    YEAR(tpepPickupDateTime)
ORDER BY
    YEAR(tpepPickupDateTime),
    passengerCount;
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
