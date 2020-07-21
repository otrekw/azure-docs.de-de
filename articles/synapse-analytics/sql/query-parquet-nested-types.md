---
title: Abfragen von geschachtelten Parquet-Typen mit SQL On-Demand (Vorschauversion)
description: In diesem Artikel erfahren Sie, wie Sie geschachtelte Parquet-Typen abfragen können.
services: synapse-analytics
author: azaricstefan
ms.service: synapse-analytics
ms.topic: how-to
ms.subservice: sql
ms.date: 05/20/2020
ms.author: v-stazar
ms.reviewer: jrasnick, carlrab
ms.openlocfilehash: bf2dbf501b5cd3b6cd0ab6b0e9bbbc2208c98a58
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85478449"
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

Die folgende Abfrage liest die Datei *structBeispiel.parquet* und zeigt, wie Elemente einer geschachtelten Spalte an die Oberfläche gebracht werden: Es gibt zwei Möglichkeiten, auf einen geschachtelten Wert zu verweisen:
- Durch Angeben des Pfadausdrucks des geschachtelten Werts nach der Typspezifikation
- Durch Formatieren des Spaltennamens als geschachtelten Wert mithilfe von „.“ zum Verweisen auf die Felder

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
        -- you can see original nested columns values by uncommenting lines below
        --DateStruct VARCHAR(8000),
        [DateValue] DATE '$.DateStruct.Date',
        --TimeStruct VARCHAR(8000),
        [TimeStruct.Time] TIME,
        --TimestampStruct VARCHAR(8000),
        [TimestampStruct.Timestamp] DATETIME2,
        --DecimalStruct VARCHAR(8000),
        DecimalValue DECIMAL(18, 5) '$.DecimalStruct.Decimal',
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

Sie können auch explizit auf die Spalten verweisen, die in der `WITH`-Klausel zurückgegeben werden sollen:

```sql
SELECT DocId,
    MapOfPersons,
    JSON_QUERY(MapOfPersons, '$."John Doe"') AS [John]
FROM
    OPENROWSET(
        BULK 'parquet/nested/mapExample.parquet',
        DATA_SOURCE = 'SqlOnDemandDemo',
        FORMAT='PARQUET'
    ) 
    WITH (DocId bigint, MapOfPersons VARCHAR(max)) AS [r];
```

Die Struktur `MakOfPersons` wird als `VARCHAR`-Spalte zurückgegeben und als JSON-Zeichenfolge formatiert.

## <a name="projecting-values-from-repeated-columns"></a>Projizieren von Werten aus wiederholten Spalten

Wenn Sie in manchen Spalten über ein Array aus Skalarwerten (z. B. `[1,2,3]`) verfügen, können Sie sie einfach erweitern und mithilfe des folgenden Skripts mit der Hauptzeile verknüpfen:

```sql
SELECT
    SimpleArray, Element
FROM
    OPENROWSET(
        BULK 'parquet/nested/justSimpleArray.parquet',
        DATA_SOURCE = 'SqlOnDemandDemo',
        FORMAT='PARQUET'
    ) AS arrays
    CROSS APPLY OPENJSON (SimpleArray) WITH (Element int '$') as array_values
```

## <a name="next-steps"></a>Nächste Schritte

Der nächste Artikel zeigt Ihnen, wie Sie [JSON-Dateien abfragen](query-json-files.md) können.
