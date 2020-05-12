---
title: Speichern von Abfrageergebnissen im Speicher
description: In diesem Artikel erfahren Sie, wie Abfrageergebnisse mithilfe von SQL On-Demand (Vorschauversion) im Speicher gespeichert werden.
services: synapse-analytics
author: vvasic-msft
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: ''
ms.date: 04/15/2020
ms.author: vvasic
ms.reviewer: jrasnick, carlrab
ms.openlocfilehash: dd7666bb9f22214fb4701e6be9edc171912d9bf9
ms.sourcegitcommit: 366e95d58d5311ca4b62e6d0b2b47549e06a0d6d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/01/2020
ms.locfileid: "82691864"
---
# <a name="store-query-results-to-storage-using-sql-on-demand-preview-using-azure-synapse-analytics"></a>Speichern von Abfrageergebnissen im Speicher mithilfe von SQL On-Demand (Vorschauversion) und Azure Synapse Analytics

In diesem Artikel erfahren Sie, wie Abfrageergebnisse mithilfe von SQL On-Demand (Vorschauversion) im Speicher gespeichert werden.

## <a name="prerequisites"></a>Voraussetzungen

Lesen Sie zunächst die unten angegebenen Artikel, und stellen Sie sicher, dass die Voraussetzungen erfüllt sind:

- [Erstmalige Einrichtung](query-data-storage.md#first-time-setup)
- [Voraussetzungen](query-data-storage.md#prerequisites)

## <a name="create-external-table-as-select"></a>CREATE EXTERNAL TABLE AS SELECT

Sie können die CETAS-Anweisung (CREATE EXTERNAL TABLE AS SELECT) verwenden, um die Abfrageergebnisse im Speicher zu speichern.

> [!NOTE]
> Ändern Sie die erste Zeile in der Abfrage (d. h. [mydbname]), sodass die von Ihnen erstellte Datenbank verwendet wird. Haben Sie noch keine Datenbank erstellt, lesen Sie die Informationen unter [Erstmalige Einrichtung](query-data-storage.md#first-time-setup). Sie müssen „LOCATION“ für die externe MyDataSource-Datenquelle so ändern, dass auf einen Speicherort verwiesen wird, für den Sie über Schreibberechtigungen verfügen. 

```sql
USE [mydbname];
GO

CREATE EXTERNAL DATA SOURCE [MyDataSource] WITH (
    LOCATION = 'https://sqlondemandstorage.blob.core.windows.net/csv'
);
GO

CREATE EXTERNAL FILE FORMAT [ParquetFF] WITH (
    FORMAT_TYPE = PARQUET,
    DATA_COMPRESSION = 'org.apache.hadoop.io.compress.SnappyCodec'
);
GO

CREATE EXTERNAL TABLE [dbo].[PopulationCETAS] WITH (
        LOCATION = 'populationParquet/',
        DATA_SOURCE = [MyDataSource],
        FILE_FORMAT = [ParquetFF]
) AS
SELECT
    *
FROM
    OPENROWSET(
        BULK 'https://sqlondemandstorage.blob.core.windows.net/csv/population-unix/population.csv',
        FORMAT='CSV'
    ) WITH (
        CountryCode varchar(4),
        CountryName varchar(64),
        Year int,
        PopulationCount int
    ) AS r;

```

## <a name="use-a-external-table-created"></a>Verwenden einer erstellten externen Tabelle

Eine externe Tabelle, die mithilfe von CETAS erstellt wurde, kann wie eine reguläre externe Tabelle verwendet werden.

> [!NOTE]
> Ändern Sie die erste Zeile in der Abfrage (d. h. [mydbname]), sodass die von Ihnen erstellte Datenbank verwendet wird. Haben Sie noch keine Datenbank erstellt, lesen Sie die Informationen unter [Erstmalige Einrichtung](query-data-storage.md#first-time-setup).

```sql
USE [mydbname];
GO

SELECT
    country_name, population
FROM PopulationCETAS
WHERE
    [year] = 2019
ORDER BY
    [population] DESC;
```

## <a name="next-steps"></a>Nächste Schritte

Informationen zum Abfragen verschiedener Dateitypen finden Sie in den Artikeln zum [Abfragen einer einzelnen CSV-Datei](query-single-csv-file.md), [Abfragen von Parquet-Dateien](query-parquet-files.md) und [Abfragen von JSON-Dateien](query-json-files.md).
