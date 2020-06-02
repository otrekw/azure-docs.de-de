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
ms.openlocfilehash: cbf6d42f3b1d130a6bf89f07bd3a7009ff0e8fa8
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2020
ms.locfileid: "83647521"
---
# <a name="store-query-results-to-storage-using-sql-on-demand-preview-using-azure-synapse-analytics"></a>Speichern von Abfrageergebnissen im Speicher mithilfe von SQL On-Demand (Vorschauversion) und Azure Synapse Analytics

In diesem Artikel erfahren Sie, wie Abfrageergebnisse mithilfe von SQL On-Demand (Vorschauversion) im Speicher gespeichert werden.

## <a name="prerequisites"></a>Voraussetzungen

Lesen Sie zunächst die unten angegebenen Artikel, und stellen Sie sicher, dass die Voraussetzungen erfüllt sind:

- [Erstmalige Einrichtung](query-data-storage.md#first-time-setup)
- [Voraussetzungen](query-data-storage.md#prerequisites)

## <a name="create-external-table-as-select"></a>CREATE EXTERNAL TABLE AS SELECT

Sie können mithilfe der CETAS-Anweisung (CREATE EXTERNAL TABLE AS SELECT) die Abfrageergebnisse im Speicher speichern.

> [!NOTE]
> Ändern Sie die erste Zeile in der Abfrage (d. h. [mydbname]), sodass die von Ihnen erstellte Datenbank verwendet wird. Haben Sie noch keine Datenbank erstellt, lesen Sie die Informationen unter [Erstmalige Einrichtung](query-data-storage.md#first-time-setup). Sie müssen „LOCATION“ für die externe MyDataSource-Datenquelle so ändern, dass auf einen Speicherort verwiesen wird, für den Sie über Schreibberechtigungen verfügen. 

```sql
USE [mydbname];
GO

CREATE DATABASE SCOPED CREDENTIAL [SasTokenWrite]
WITH IDENTITY = 'SHARED ACCESS SIGNATURE',
     SECRET = 'sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-04-18T20:42:12Z&st=2019-04-18T12:42:12Z&spr=https&sig=lQHczNvrk1KoYLCpFdSsMANd0ef9BrIPBNJ3VYEIq78%3D';
GO

CREATE EXTERNAL DATA SOURCE [MyDataSource] WITH (
    LOCATION = 'https://<storage account name>.blob.core.windows.net/csv', CREDENTIAL [SasTokenWrite]
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

## <a name="use-the-external-table"></a>Verwenden der externen Tabelle

Sie können die mithilfe von CETAS erstellte externe Tabelle wie eine reguläre externe Tabelle verwendet werden.

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

Weitere Informationen zum Abfragen verschiedener Dateitypen finden Sie in den Artikeln zum [Abfragen einer einzelnen CSV-Datei](query-single-csv-file.md), [Abfragen von Parquet-Dateien](query-parquet-files.md) und [Abfragen von JSON-Dateien](query-json-files.md).
