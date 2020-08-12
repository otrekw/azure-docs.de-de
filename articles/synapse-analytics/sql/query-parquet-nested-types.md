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
ms.openlocfilehash: fb56c4da77ddeb87ebc3724a3b138994e4da98e7
ms.sourcegitcommit: 11e2521679415f05d3d2c4c49858940677c57900
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/31/2020
ms.locfileid: "87489689"
---
# <a name="query-nested-types-in-parquet-and-json-files-using-sql-on-demand-preview-in-azure-synapse-analytics"></a>Abfragen von geschachtelten Typen in Parquet und JSON-Dateien mit SQL On-Demand (Vorschauversion) in Azure Synapse Analytics

In diesem Artikel erfahren Sie, wie Sie eine Abfrage mit SQL On-Demand (Vorschauversion) in Azure Synapse Analytics schreiben können. Diese Abfrage liest geschachtelte Parquet-Typen.
Bei geschachtelten Typen handelt es sich um komplexe Strukturen zur Darstellung von Objekten oder Arrays. Geschachtelte Typen können in folgenden Formaten gespeichert werden: 
- In [PARQUET](query-parquet-files.md). Dabei können mehrere komplexe Spalten mit Arrays und Objekten vorhanden sein.
- In hierarchischen [JSON-Dateien](query-json-files.md). Dabei können komplexe JSON-Dokumente als einzelne Spalte gelesen werden.
- In einer CosmosDB-Sammlung. Dabei kann jedes Dokument komplexe geschachtelte Eigenschaften enthalten (aktuell in der geschlossenen öffentlichen Vorschau verfügbar).

Synapse SQL On-Demand formatiert alle geschachtelten Typen als JSON-Objekte und -Arrays. Folglich können Sie [komplexe Objekte mit JSON-Funktionen extrahieren oder ändern](https://docs.microsoft.com/sql/relational-databases/json/validate-query-and-change-json-data-with-built-in-functions-sql-server) oder [JSON-Daten mit der OPENJSON-Funktion](https://docs.microsoft.com/sql/relational-databases/json/convert-json-data-to-rows-and-columns-with-openjson-sql-server) analysieren. 

Nachfolgend ist eine Beispielabfrage gezeigt, mit der Skalar- und Objektwerte aus der JSON-Datei [COVID-19 Open Research Dataset](https://azure.microsoft.com/services/open-datasets/catalog/covid-19-open-research/) mit geschachtelten Objekten extrahiert werden. 

```sql
SELECT
    title = JSON_VALUE(doc, '$.metadata.title'),
    first_author = JSON_QUERY(doc, '$.metadata.authors[0]'),
    first_author_name = JSON_VALUE(doc, '$.metadata.authors[0].first'),
    complex_object = doc
FROM
    OPENROWSET(
        BULK 'https://azureopendatastorage.blob.core.windows.net/covid19temp/comm_use_subset/pdf_json/000b7d1517ceebb34e1e3e817695b6de03e2fa78.json',
        FORMAT='CSV', FIELDTERMINATOR ='0x0b', FIELDQUOTE = '0x0b', ROWTERMINATOR = '0x0b'
    )
    WITH ( doc varchar(MAX) ) AS docs;
```

Die Funktion `JSON_VALUE` gibt einen Skalarwert aus dem Feld unter dem angegebenen Pfad zurück. Die Funktion `JSON_QUERY` gibt ein als JSON formatiertes Objekt aus dem Feld unter dem angegebenen Pfad zurück.

> [!IMPORTANT]
> In diesem Beispiel wird eine Datei aus [COVID-19 Open Research Dataset](https://azure.microsoft.com/services/open-datasets/catalog/covid-19-open-research/) verwendet. Weitere Informationen finden Sie in der Lizenz und Datenstruktur auf dieser Seite.

## <a name="prerequisites"></a>Voraussetzungen

Der erste Schritt besteht in der **Erstellung einer Datenbank** mit einer Datenquelle, die einen Verweis enthält. Initialisieren Sie dann die Objekte, indem Sie das [Setupskript](https://github.com/Azure-Samples/Synapse/blob/master/SQL/Samples/LdwSample/SampleDB.sql) für diese Datenbank ausführen. Mit diesem Setupskript werden die Datenquellen, die für die gesamte Datenbank gültigen Anmeldeinformationen und externe Dateiformate erstellt, die in diesen Beispielen verwendet werden.

## <a name="project-nested-or-repeated-data"></a>Projizieren von geschachtelten oder wiederholten Daten

Eine PARQUET-Datei kann über mehrere Spalten mit komplexen Typen verfügen. Die Werte aus diesen Spalten werden als JSON-Text formatiert und als VARCHAR-Spalte zurückgegeben. Mit der folgenden Abfrage wird die Datei *structExample.parquet* gelesen und gezeigt, wie die Werte der geschachtelten Spalten gelesen werden sollen: 

```sql
SELECT
    DateStruct, TimeStruct, TimestampStruct, DecimalStruct, FloatStruct
FROM
    OPENROWSET(
        BULK 'parquet/nested/structExample.parquet',
        DATA_SOURCE = 'SqlOnDemandDemo',
        FORMAT='PARQUET'
    )
    WITH (
        DateStruct VARCHAR(8000),
        TimeStruct VARCHAR(8000),
        TimestampStruct VARCHAR(8000),
        DecimalStruct VARCHAR(8000),
        FloatStruct VARCHAR(8000)
    ) AS [r];
```

Diese Abfrage gibt das folgende Ergebnis zurück, wobei der Inhalt der einzelnen geschachtelten Objekte als JSON-Text zurückgegeben wird:

| DateStruct    | TimeStruct    | TimestampStruct   | DecimalStruct | FloatStruct |
| --- | --- | --- | --- | --- |
|{"Date":"2009-04-25"}| {"Time":"20:51:54.3598000"}|    {"Timestamp":"5501-04-08 12:13:57.4821000"}|    {"Decimal":11143412.25350}| {"Float":0.5}|
|{"Date":"1916-04-29"}| {"Time":"00:16:04.6778000"}|    {"Timestamp":"1990-06-30 20:50:52.6828000"}|    {"Decimal":1963545.62800}|  {"Float":-2.125}|

Die folgende Abfrage liest die Datei *justSimpleArray.parquet*. Sie projiziert alle Spalten aus der Parquet-Datei einschließlich geschachtelter oder wiederholter Daten.

```sql
SELECT
    SimpleArray
FROM
    OPENROWSET(
        BULK 'parquet/nested/justSimpleArray.parquet',
        DATA_SOURCE = 'SqlOnDemandDemo',
        FORMAT='PARQUET'
    ) AS [r];
```

Bei dieser Abfrage wird das folgende Ergebnis zurückgegeben:

|SimpleArray|
| --- |
|[11,12,13]|
|[21,22,23]|

## <a name="read-properties-from-nested-object-columns"></a>Lesen von Eigenschaften aus Spalten mit geschachtelten Objekten

Mit der Funktion `JSON_VALUE` können Sie Werte aus einer Spalte als JSON-formatierten Text zurückgeben:

```sql
SELECT
    title = JSON_VALUE(complex_column, '$.metadata.title'),
    first_author_name = JSON_VALUE(complex_column, '$.metadata.authors[0].first'),
    body_text = JSON_VALUE(complex_column, '$.body_text.text'),
    complex_column
FROM
    OPENROWSET( BULK 'https://azureopendatastorage.blob.core.windows.net/covid19temp/comm_use_subset/pdf_json/000b7d1517ceebb34e1e3e817695b6de03e2fa78.json',
                FORMAT='CSV', FIELDTERMINATOR ='0x0b', FIELDQUOTE = '0x0b', ROWTERMINATOR = '0x0b' ) WITH ( complex_column varchar(MAX) ) AS docs;
```

Das Ergebnis ist in der folgenden Tabelle gezeigt:

|title  | first_author_name | body_text | complex_column |
| --- | --- | --- | --- |
| Supplementary Information An eco-epidemiolo... | Julien   | - Figure S1 : Phylogeny of... | `{    "paper_id": "000b7d1517ceebb34e1e3e817695b6de03e2fa78",    "metadata": {        "title": "Supplementary Information An eco-epidemiological study of Morbilli-related paramyxovirus infection in Madagascar bats reveals host-switching as the dominant macro-evolutionary mechanism",        "authors": [            {                "first": "Julien"` |

Im Gegensatz zu JSON-Dateien, die in den meisten Fällen eine einzelne Spalte mit komplexen JSON-Objekten zurückgeben, können PARQUET-Dateien mehrere komplexe Spalten enthalten. Sie können die Eigenschaften der geschachtelten Spalte mit der Funktion `JSON_VALUE` für jede Spalte lesen. Mit `OPENROWSET` können Sie die Pfade der geschachtelten Eigenschaften direkt in der `WITH`-Klausel angeben. Pfade können als Name der Spalte angegeben werden. Alternativ können Sie nach dem Spaltentyp einen [JSON-Pfadausdruck](https://docs.microsoft.com/sql/relational-databases/json/json-path-expressions-sql-server) hinzufügen.

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
        [DateValue] DATE '$.DateStruct.Date',
        [TimeStruct.Time] TIME,
        [TimestampStruct.Timestamp] DATETIME2,
        DecimalValue DECIMAL(18, 5) '$.DecimalStruct.Decimal',
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

Das Ergebnis ist in der folgenden Tabelle gezeigt:

|SimpleArray    | FirstElement  | SecondElement | ThirdElement |
| --- | --- | --- | --- |
| [11,12,13] | 11   | 12 | 13 |
| [21,22,23] | 21   | 22 | 23 |

## <a name="access-sub-objects-from-complex-columns"></a>Zugreifen auf Teilobjekte aus komplexen Spalten

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

Die Struktur `MapOfPersons` wird als `VARCHAR`-Spalte zurückgegeben und als JSON-Zeichenfolge formatiert.

## <a name="project-values-from-repeated-columns"></a>Projizieren von Werten aus wiederholten Spalten

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
