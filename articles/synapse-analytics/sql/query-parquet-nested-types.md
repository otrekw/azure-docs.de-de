---
title: Abfragen von geschachtelten Parquet-Typen mit SQL On-Demand (Vorschauversion)
description: In diesem Artikel erfahren Sie, wie Sie geschachtelte Parquet-Typen abfragen können.
services: synapse-analytics
author: azaricstefan
ms.service: synapse-analytics
ms.topic: how-to
ms.subservice: ''
ms.date: 05/20/2020
ms.author: v-stazar
ms.reviewer: jrasnick, carlrab
ms.openlocfilehash: 82edee84317b5d542bf65e29514286f96c18bbcc
ms.sourcegitcommit: 493b27fbfd7917c3823a1e4c313d07331d1b732f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/21/2020
ms.locfileid: "83744235"
---
# <a name="query-parquet-nested-types-using-sql-on-demand-preview-in-azure-synapse-analytics"></a>Abfragen von geschachtelten Parquet-Typen mit SQL On-Demand (Vorschauversion) in Azure Synapse Analytics

In diesem Artikel erfahren Sie, wie Sie eine Abfrage mit SQL On-Demand (Vorschauversion) in Azure Synapse Analytics schreiben können.  Diese Abfrage liest geschachtelte Parquet-Typen.

## <a name="prerequisites"></a>Voraussetzungen

Der erste Schritt besteht in der **Erstellung einer Datenbank** mit einer Datenquelle, die einen Verweis enthält. Initialisieren Sie dann die Objekte, indem Sie das [Setupskript](https://github.com/Azure-Samples/Synapse/blob/master/SQL/Samples/LdwSample/SampleDB.sql) für diese Datenbank ausführen. Mit diesem Setupskript werden die Datenquellen, die für die gesamte Datenbank gültigen Anmeldeinformationen und externe Dateiformate erstellt, die in diesen Beispielen verwendet werden.

## <a name="project-nested-or-repeated-data"></a>Projizieren von geschachtelten oder wiederholten Daten

Die folgende Abfrage liest die Datei *justSimpleArray.parquet*. Sie projiziert alle Spalten aus der Parquet-Datei einschließlich geschachtelter oder wiederholter Daten.

```sql
SELECT
    *
FROM
    OPENROWSET(
        BULK 'parquet/nested/justSimpleArray.parquet',
        DATA_SOURCE = 'SqlOnDemandDemo',
        FORMAT='PARQUET'
    ) AS [r];
```

## <a name="access-elements-from-nested-columns"></a>Zugreifen auf Elemente aus geschachtelten Spalten

Die folgende Abfrage liest die Datei *structBeispiel.parquet* und zeigt, wie Elemente einer geschachtelten Spalte an die Oberfläche gebracht werden:

```sql
SELECT
    *
FROM
    OPENROWSET(
        BULK 'parquet/nested/structExample.parquet',
        DATA_SOURCE = 'SqlOnDemandDemo',
        FORMAT='PARQUET'
    )
    WITH (
        -- you can see original n"sted columns values by uncommenting lines below
        --DateStruct VARCHAR(8000),
        [DateStruct.Date] DATE,
        --TimeStruct VARCHAR(8000),
        [TimeStruct.Time] TIME,
        --TimestampStruct VARCHAR(8000),
        [TimestampStruct.Timestamp] DATETIME2,
        --DecimalStruct VARCHAR(8000),
        [DecimalStruct.Decimal] DECIMAL(18, 5),
        --FloatStruct VARCHAR(8000),
        [FloatStruct.Float] FLOAT
    ) AS [r];
```

## <a name="access-elements-from-repeated-columns"></a>Zugreifen auf Elemente aus wiederholten Spalten

Die folgende Abfrage liest die Datei *justSimpleArray.parquet* und verwendet [JSON_VALUE](/sql/t-sql/functions/json-value-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), um ein **skalares** Element aus einer sich wiederholenden Spalte, z. B. einem Array oder einer Zuordnung, abzurufen:

```sql
SELECT
    *,
    JSON_VALUE(SimpleArray, '$[0]') AS FirstElement,
    JSON_VALUE(SimpleArray, '$[1]') AS SecondElement,
    JSON_VALUE(SimpleArray, '$[2]') AS ThirdElement
FROM
    OPENROWSET(
        BULK 'parquet/nested/justSimpleArray.parquet',
        DATA_SOURCE = 'SqlOnDemandDemo',
        FORMAT='PARQUET'
    ) AS [r];
```

Die folgende Abfrage liest die Datei *mapExample.parquet* und verwendet [JSON_QUERY](/sql/t-sql/functions/json-query-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), um ein **nicht skalares** Element aus einer sich wiederholenden Spalte, z. B. einem Array oder einer Zuordnung, abzurufen:

```sql
SELECT
    MapOfPersons,
    JSON_QUERY(MapOfPersons, '$."John Doe"') AS [John]
FROM
    OPENROWSET(
        BULK 'parquet/nested/mapExample.parquet',
        DATA_SOURCE = 'SqlOnDemandDemo',
        FORMAT='PARQUET'
    ) AS [r];
```

## <a name="next-steps"></a>Nächste Schritte

Der nächste Artikel zeigt Ihnen, wie Sie [JSON-Dateien abfragen](query-json-files.md) können.
