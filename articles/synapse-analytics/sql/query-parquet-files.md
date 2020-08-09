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
ms.openlocfilehash: 8083edaf647f52a07d55dddf21fe5751340783be
ms.sourcegitcommit: 11e2521679415f05d3d2c4c49858940677c57900
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/31/2020
ms.locfileid: "87496235"
---
# <a name="query-parquet-files-using-sql-on-demand-preview-in-azure-synapse-analytics"></a>Abfragen von Parquet-Dateien mit SQL On-Demand (Vorschauversion) in Azure Synapse Analytics

In diesem Artikel erfahren Sie, wie Sie eine Abfrage mit SQL On-Demand (Vorschauversion) schreiben können, die Parquet-Dateien lesen kann.

## <a name="quickstart-example"></a>Schnellstartbeispiel

Mit der Funktion `OPENROWSET` können Sie den Inhalt einer Parquet-Datei lesen, indem Sie die URL zur Datei bereitstellen.

### <a name="read-parquet-file"></a>Lesen einer Parquet-Datei

Am einfachsten können Sie den Inhalt Ihrer `PARQUET`-Datei anzeigen, indem Sie der Funktion `OPENROWSET` die Datei-URL bereitstellen und Parquet als `FORMAT` angeben. Wenn die Datei öffentlich verfügbar ist oder Ihre Azure AD-Identität auf diese Datei zugreifen kann, sollten Sie den Inhalt der Datei mithilfe einer Abfrage wie im folgenden Beispiel anzeigen können:

```sql
select top 10 *
from openrowset(
    bulk 'https://pandemicdatalake.blob.core.windows.net/public/curated/covid-19/ecdc_cases/latest/ecdc_cases.parquet',
    format = 'parquet') as rows
```

Stellen Sie sicher, dass Sie auf diese Datei zugreifen können. Wenn Ihre Datei mit einem SAS-Schlüssel oder einer benutzerdefinierten Azure-Identität geschützt ist, müssen Sie [Anmeldeinformationen auf Serverebene für die SQL-Anmeldung](develop-storage-files-storage-access-control.md?tabs=shared-access-signature#server-scoped-credential) einrichten.

### <a name="data-source-usage"></a>Datenquellennutzung

Im vorherigen Beispiel wird der vollständige Pfad zur Datei verwendet. Alternativ können Sie eine externe Datenquelle mit dem Speicherort erstellen, der auf den Stammordner des Speichers verweist, und diese Datenquelle sowie den relativen Pfad zur Datei in der Funktion `OPENROWSET` verwenden:

```sql
create external data source covid
with ( location = 'https://pandemicdatalake.blob.core.windows.net/public/curated/covid-19/ecdc_cases' );
go
select top 10 *
from openrowset(
        bulk 'latest/ecdc_cases.parquet',
        data_source = 'covid',
        format = 'parquet'
    ) as rows
```

Wenn eine Datenquelle mit einem SAS-Schlüssel oder einer benutzerdefinierten Identität geschützt ist, können Sie die [Datenquelle mit datenbankweit gültigen Anmeldeinformationen](develop-storage-files-storage-access-control.md?tabs=shared-access-signature#database-scoped-credential) konfigurieren.

### <a name="explicitly-specify-schema"></a>Explizites Angeben des Schemas

Bei `OPENROWSET` können Sie mit der `WITH`-Klausel explizit angeben, welche Spalten aus der Datei gelesen werden sollen:

```sql
select top 10 *
from openrowset(
        bulk 'latest/ecdc_cases.parquet',
        data_source = 'covid',
        format = 'parquet'
    ) with ( date_rep date, cases int, geo_id varchar(6) ) as rows
```

In den folgenden Abschnitten erfahren Sie, wie Sie verschiedene Typen von Parquet-Dateien abfragen.

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
