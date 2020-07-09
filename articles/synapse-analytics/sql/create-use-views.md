---
title: Erstellen und Verwenden von Ansichten in SQL On-Demand (Vorschauversion)
description: In diesem Abschnitt erfahren Sie, wie Sie Ansichten zum Umschließen von Abfragen in SQL On-Demand (Vorschauversion) erstellen und verwenden. Ansichten ermöglichen Ihnen die erneute Verwendung dieser Abfragen. Ansichten sind ebenfalls erforderlich, wenn Sie Tools wie Power BI zusammen mit SQL On-Demand verwenden möchten.
services: synapse-analytics
author: azaricstefan
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: sql
ms.date: 05/20/2020
ms.author: v-stazar
ms.reviewer: jrasnick, carlrab
ms.openlocfilehash: 79206ffb51b41c3d7e671bb37353548b47190f6b
ms.sourcegitcommit: 6fd28c1e5cf6872fb28691c7dd307a5e4bc71228
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/23/2020
ms.locfileid: "85206461"
---
# <a name="create-and-use-views-in-sql-on-demand-preview-using-azure-synapse-analytics"></a>Erstellen und Verwenden von Ansichten in SQL On-Demand (Vorschauversion) mithilfe von Azure Synapse Analytics

In diesem Abschnitt erfahren Sie, wie Sie Ansichten zum Umschließen von Abfragen in SQL On-Demand (Vorschauversion) erstellen und verwenden. Ansichten ermöglichen Ihnen die erneute Verwendung dieser Abfragen. Ansichten sind ebenfalls erforderlich, wenn Sie Tools wie Power BI zusammen mit SQL On-Demand verwenden möchten.

## <a name="prerequisites"></a>Voraussetzungen

Im ersten Schritt erstellen Sie eine Datenbank, in der die Ansicht erstellt werden soll, und Sie initialisieren die zum Authentifizieren beim Azure-Speicher erforderlichen Objekte, indem Sie das [Setupskript](https://github.com/Azure-Samples/Synapse/blob/master/SQL/Samples/LdwSample/SampleDB.sql) für diese Datenbank ausführen. Alle Abfragen in diesem Artikel werden in Ihrer Beispieldatenbank ausgeführt.

## <a name="create-a-view"></a>Erstellen einer Ansicht

Ansichten können auf die gleiche Weise wie reguläre SQL Server-Ansichten erstellt werden. Mit der folgenden Abfrage wird eine Ansicht erstellt, die die Datei *population.csv* liest.

> [!NOTE]
> Ändern Sie die erste Zeile in der Abfrage (d. h. [mydbname]), sodass die von Ihnen erstellte Datenbank verwendet wird.

```sql
USE [mydbname];
GO

DROP VIEW IF EXISTS populationView;
GO

CREATE VIEW populationView AS
SELECT * 
FROM OPENROWSET(
        BULK 'csv/population/population.csv',
        DATA_SOURCE = 'SqlOnDemandDemo',
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

Bei der Ansicht in diesem Beispiel wird die Funktion `OPENROWSET` mit dem absoluten Pfad zu den zugrunde liegenden Dateien verwendet. Wenn Sie über `EXTERNAL DATA SOURCE` mit einer Stamm-URL Ihres Speichers verfügen, können Sie `OPENROWSET` mit `DATA_SOURCE` und dem relativen Dateipfad verwenden:

```
CREATE VIEW TaxiView
AS SELECT *, nyc.filepath(1) AS [year], nyc.filepath(2) AS [month]
FROM
    OPENROWSET(
        BULK 'parquet/taxi/year=*/month=*/*.parquet',
        DATA_SOURCE = 'sqlondemanddemo',
        FORMAT='PARQUET'
    ) AS nyc
```

## <a name="use-a-view"></a>Verwenden einer Ansicht

Ansichten können in Ihren Abfragen auf die gleiche Weise verwendet werden wie in SQL Server-Abfragen.

Die folgende Abfrage veranschaulicht die Verwendung der Ansicht *population_csv*, die Sie unter [Erstellen einer Ansicht](#create-a-view) erstellt haben. Sie gibt Länder-/Regionsnamen mit der entsprechenden Bevölkerung im Jahr 2019 in absteigender Reihenfolge zurück.

> [!NOTE]
> Ändern Sie die erste Zeile in der Abfrage (d. h. [mydbname]), sodass die von Ihnen erstellte Datenbank verwendet wird.

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
