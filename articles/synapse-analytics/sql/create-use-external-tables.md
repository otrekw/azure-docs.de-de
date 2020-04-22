---
title: Erstellen und Verwenden externer Tabellen in SQL On-Demand (Vorschauversion)
description: In diesem Abschnitt erfahren Sie, wie Sie externe Tabellen in SQL On-Demand (Vorschauversion) erstellen und verwenden. Externe Tabellen sind nützlich, wenn Sie den Zugriff auf externe Daten in SQL On-Demand steuern und Tools wie Power BI in Verbindung mit SQL On-Demand verwenden möchten.
services: synapse-analytics
author: vvasic-msft
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: ''
ms.date: 04/15/2020
ms.author: vvasic
ms.reviewer: jrasnick, carlrab
ms.openlocfilehash: a708e5e713e62ab5b7ca70b61235a84830b051cd
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2020
ms.locfileid: "81420794"
---
# <a name="create-and-use-external-tables-in-sql-on-demand-preview-using-azure-synapse-analytics"></a>Erstellen und Verwenden externer Tabellen in SQL On-Demand (Vorschauversion) mithilfe von Azure Synapse Analytics

In diesem Abschnitt erfahren Sie, wie Sie externe Tabellen in SQL On-Demand (Vorschauversion) erstellen und verwenden. Externe Tabellen sind nützlich, wenn Sie den Zugriff auf externe Daten in SQL On-Demand steuern und Tools wie Power BI in Verbindung mit SQL On-Demand verwenden möchten.

## <a name="prerequisites"></a>Voraussetzungen

Lesen Sie zunächst die unten angegebenen Artikel, und stellen Sie sicher, dass Sie die Voraussetzungen zum Erstellen und Verwenden von externen Tabellen in SQL On-Demand erfüllen:

- [Erstmalige Einrichtung](query-data-storage.md#first-time-setup)
- [Voraussetzungen](query-data-storage.md#prerequisites)

## <a name="create-an-external-table"></a>Erstellen einer externen Tabelle

Externe Tabellen können auf die gleiche Weise wie reguläre externe SQL Server-Tabellen erstellt werden. Mit der folgenden Abfrage wird eine externe Tabelle erstellt, die die Datei *population.csv* liest.

> [!NOTE]
> Ändern Sie die erste Zeile in der Abfrage (d. h. [mydbname]), sodass die von Ihnen erstellte Datenbank verwendet wird. Haben Sie noch keine Datenbank erstellt, lesen Sie die Informationen unter [Erstmalige Einrichtung](query-data-storage.md#first-time-setup).

```sql
USE [mydbname];
GO

CREATE EXTERNAL DATA SOURCE [CsvDataSource] WITH (
    LOCATION = 'https://sqlondemandstorage.blob.core.windows.net/csv'
);
GO

CREATE EXTERNAL FILE FORMAT CSVFileFormat
WITH (  
    FORMAT_TYPE = DELIMITEDTEXT,
    FORMAT_OPTIONS (
        FIELD_TERMINATOR = ',',
        STRING_DELIMITER = '"',
        FIRST_ROW = 2
    )
);
GO

CREATE EXTERNAL TABLE populationExternalTable
(
    [country_code] VARCHAR (5) COLLATE Latin1_General_BIN2,
    [country_name] VARCHAR (100) COLLATE Latin1_General_BIN2,
    [year] smallint,
    [population] bigint
);
WITH (
    LOCATION = 'population/population.csv',
    DATA_SOURCE = CsvDataSource,
    FILE_FORMAT = CSVFileFormat
);
GO
```

## <a name="use-a-external-table"></a>Verwenden einer externen Tabelle

Externe Tabellen können in Ihren Abfragen auf die gleiche Weise verwendet werden wie in SQL Server-Abfragen.

Die folgende Abfrage veranschaulicht die Verwendung der externen Tabelle *population*, die Sie im Abschnitt [Erstellen einer externen Tabelle](#create-an-external-table) erstellt haben. Sie gibt Ländernamen mit der jeweiligen Bevölkerung im Jahr 2019 in absteigender Reihenfolge zurück.

> [!NOTE]
> Ändern Sie die erste Zeile in der Abfrage (d. h. [mydbname]), sodass die von Ihnen erstellte Datenbank verwendet wird. Haben Sie noch keine Datenbank erstellt, lesen Sie die Informationen unter [Erstmalige Einrichtung](query-data-storage.md#first-time-setup).

```sql
USE [mydbname];
GO

SELECT
    country_name, population
FROM populationExternalTable
WHERE
    [year] = 2019
ORDER BY
    [population] DESC;
```

## <a name="next-steps"></a>Nächste Schritte

Informationen zum Speichern der Ergebnisse einer Abfrage im Speicher finden Sie unter [Speichern von Abfrageergebnissen im Speicher mithilfe von SQL On-Demand (Vorschauversion) und Azure Synapse Analytics](../sql/create-external-table-as-select.md).
