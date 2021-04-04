---
title: Speichern der Abfrageergebnisse aus einem serverlosen SQL-Pool
description: In diesem Artikel erfahren Sie, wie Abfrageergebnisse mithilfe eines serverlosen SQL-Pools im Speicher gespeichert werden.
services: synapse-analytics
author: vvasic-msft
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: sql
ms.date: 04/15/2020
ms.author: vvasic
ms.reviewer: jrasnick
ms.openlocfilehash: 12841c747116cc9e14f348dfcf81acaa5da5e8c9
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "98165364"
---
# <a name="store-query-results-to-storage-using-serverless-sql-pool-in-azure-synapse-analytics"></a>Speichern von Abfrageergebnissen im Speicher mithilfe eines serverlosen SQL-Pools in Azure Synapse Analytics

In diesem Artikel erfahren Sie, wie Abfrageergebnisse mithilfe eines serverlosen SQL-Pools im Speicher gespeichert werden.

## <a name="prerequisites"></a>Voraussetzungen

Im ersten Schritt **erstellen Sie eine Datenbank**, in der Sie die Abfragen ausführen. Initialisieren Sie dann die Objekte, indem Sie das [Setupskript](https://github.com/Azure-Samples/Synapse/blob/master/SQL/Samples/LdwSample/SampleDB.sql) für diese Datenbank ausführen. Mit diesem Setupskript werden die Datenquellen, die für die gesamte Datenbank gültigen Anmeldeinformationen und externe Dateiformate erstellt, die in diesen Beispielen zum Lesen von Daten verwendet werden.

Befolgen Sie die Anweisungen in diesem Artikel, um Datenquellen, datenbankgestützte Anmeldeinformationen und externe Dateiformate zu erstellen, die zum Schreiben von Daten in den Ausgabespeicher verwendet werden.

## <a name="create-external-table-as-select"></a>CREATE EXTERNAL TABLE AS SELECT

Sie können mithilfe der CETAS-Anweisung (CREATE EXTERNAL TABLE AS SELECT) die Abfrageergebnisse im Speicher speichern.

> [!NOTE]
> Ändern Sie die erste Zeile in der Abfrage (d. h. [mydbname]), sodass die von Ihnen erstellte Datenbank verwendet wird.

```sql
USE [mydbname];
GO

CREATE DATABASE SCOPED CREDENTIAL [SasTokenWrite]
WITH IDENTITY = 'SHARED ACCESS SIGNATURE',
     SECRET = 'sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-04-18T20:42:12Z&st=2019-04-18T12:42:12Z&spr=https&sig=lQHczNvrk1KoYLCpFdSsMANd0ef9BrIPBNJ3VYEIq78%3D';
GO

CREATE EXTERNAL DATA SOURCE [MyDataSource] WITH (
    LOCATION = 'https://<storage account name>.blob.core.windows.net/csv', CREDENTIAL = [SasTokenWrite]
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
        BULK 'csv/population-unix/population.csv',
        DATA_SOURCE = 'sqlondemanddemo',
        FORMAT = 'CSV', PARSER_VERSION = '2.0'
    ) WITH (
        CountryCode varchar(4),
        CountryName varchar(64),
        Year int,
        PopulationCount int
    ) AS r;

```

> [!NOTE]
> Für die erneute Ausführung müssen Sie dieses Skript modifizieren und den Zielspeicherort ändern. Externe Tabellen können nicht an dem Speicherort erstellt werden, an dem bei Ihnen bereits Daten vorhanden sind.

## <a name="use-the-external-table"></a>Verwenden der externen Tabelle

Sie können die mithilfe von CETAS erstellte externe Tabelle wie eine reguläre externe Tabelle verwendet werden.

> [!NOTE]
> Ändern Sie die erste Zeile in der Abfrage (d. h. [mydbname]), sodass die von Ihnen erstellte Datenbank verwendet wird.

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

## <a name="remarks"></a>Bemerkungen

Nachdem Sie Ihre Ergebnisse gespeichert haben, können die Daten in der externen Tabelle nicht mehr geändert werden. Eine Wiederholung ist für dieses Skript nicht möglich, da von CETAS die zugrunde liegenden Daten, die bei der vorherigen Ausführung erstellt wurden, nicht überschrieben werden. Stimmen Sie für die folgenden Feedbackvorschläge, falls Sie einige davon für Ihre Szenarien benötigen, oder machen Sie auf der Azure-Feedbackseite neue Vorschläge:
- [Ermöglichen des Einfügens von neuen Daten in eine externe Tabelle](https://feedback.azure.com/forums/307516-azure-synapse-analytics/suggestions/32981347-polybase-allow-insert-new-data-to-existing-exteran)
- [Ermöglichen des Löschens von Daten aus einer externen Tabelle](https://feedback.azure.com/forums/307516-azure-synapse-analytics/suggestions/15158034-polybase-delete-from-external-tables)
- [Angeben von Partitionen in CETAS](https://feedback.azure.com/forums/307516-azure-synapse-analytics/suggestions/19520860-polybase-partitioned-by-functionality-when-creati)
- [Angeben von Dateigrößen und Zählern](https://feedback.azure.com/forums/307516-azure-synapse-analytics/suggestions/42263617-cetas-specify-number-of-parquet-files-file-size)

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Abfragen verschiedener Dateitypen finden Sie in den Artikeln zum [Abfragen einer einzelnen CSV-Datei](query-single-csv-file.md), [Abfragen von Parquet-Dateien](query-parquet-files.md) und [Abfragen von JSON-Dateien](query-json-files.md).
