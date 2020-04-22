---
title: Erstellen und Verwenden von Ansichten in SQL On-Demand (Vorschauversion)
description: In diesem Abschnitt erfahren Sie, wie Sie Ansichten zum Umschließen von Abfragen in SQL On-Demand (Vorschauversion) erstellen und verwenden. Ansichten ermöglichen Ihnen die erneute Verwendung dieser Abfragen. Ansichten sind ebenfalls erforderlich, wenn Sie Tools wie Power BI zusammen mit SQL On-Demand verwenden möchten.
services: synapse-analytics
author: azaricstefan
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: ''
ms.date: 04/15/2020
ms.author: v-stazar
ms.reviewer: jrasnick, carlrab
ms.openlocfilehash: 0f5323193706fdd00739be6c71a4fe12cfedf21b
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2020
ms.locfileid: "81420774"
---
# <a name="create-and-use-views-in-sql-on-demand-preview-using-azure-synapse-analytics"></a>Erstellen und Verwenden von Ansichten in SQL On-Demand (Vorschauversion) mithilfe von Azure Synapse Analytics

In diesem Abschnitt erfahren Sie, wie Sie Ansichten zum Umschließen von Abfragen in SQL On-Demand (Vorschauversion) erstellen und verwenden. Ansichten ermöglichen Ihnen die erneute Verwendung dieser Abfragen. Ansichten sind ebenfalls erforderlich, wenn Sie Tools wie Power BI zusammen mit SQL On-Demand verwenden möchten.

## <a name="prerequisites"></a>Voraussetzungen

Lesen Sie zunächst die unten angegebenen Artikel, und stellen Sie sicher, dass Sie die Voraussetzungen zum Erstellen und Verwenden von SQL On-Demand-Ansichten erfüllen:

- [Erstmalige Einrichtung](query-data-storage.md#first-time-setup)
- [Voraussetzungen](query-data-storage.md#prerequisites)

## <a name="create-a-view"></a>Erstellen einer Ansicht

Ansichten können auf die gleiche Weise wie reguläre SQL Server-Ansichten erstellt werden. Mit der folgenden Abfrage wird eine Ansicht erstellt, die die Datei *population.csv* liest.

> [!NOTE]
> Ändern Sie die erste Zeile in der Abfrage (d. h. [mydbname]), sodass die von Ihnen erstellte Datenbank verwendet wird. Haben Sie noch keine Datenbank erstellt, lesen Sie die Informationen unter [Erstmalige Einrichtung](query-data-storage.md#first-time-setup).

```sql
USE [mydbname];
GO

DROP VIEW IF EXISTS populationView;
GO

CREATE VIEW populationView AS
SELECT * 
FROM OPENROWSET(
        BULK 'https://sqlondemandstorage.blob.core.windows.net/csv/population/population.csv',
         FORMAT = 'CSV', 
        FIELDTERMINATOR =',', 
        ROWTERMINATOR = '\n'
    )
WITH (
    [country_code] VARCHAR (5) COLLATE Latin1_General_BIN2,
    [country_name] VARCHAR (100) COLLATE Latin1_General_BIN2,
    [year] smallint,
    [population] bigint
) AS [r];
```

## <a name="use-a-view"></a>Verwenden einer Ansicht

Ansichten können in Ihren Abfragen auf die gleiche Weise verwendet werden wie in SQL Server-Abfragen.

Die folgende Abfrage veranschaulicht die Verwendung der Ansicht *population_csv*, die Sie unter [Erstellen einer Ansicht](#create-a-view) erstellt haben. Sie gibt Ländernamen mit der entsprechenden Bevölkerung im Jahr 2019 in absteigender Reihenfolge zurück.

> [!NOTE]
> Ändern Sie die erste Zeile in der Abfrage (d. h. [mydbname]), sodass die von Ihnen erstellte Datenbank verwendet wird. Haben Sie noch keine Datenbank erstellt, lesen Sie die Informationen unter [Erstmalige Einrichtung](query-data-storage.md#first-time-setup).

```sql
USE [mydbname];
GO

SELECT
    country_name, population
FROM populationView
WHERE
    [year] = 2019
ORDER BY
    [population] DESC;
```

## <a name="next-steps"></a>Nächste Schritte

Informationen zum Abfragen verschiedener Dateitypen finden Sie in den Artikeln zum [Abfragen einer einzelnen CSV-Datei](query-single-csv-file.md), [Abfragen von Parquet-Dateien](query-parquet-files.md) und [Abfragen von JSON-Dateien](query-json-files.md).
