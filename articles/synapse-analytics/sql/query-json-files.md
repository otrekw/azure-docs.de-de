---
title: Abfragen von JSON-Dateien mit SQL On-Demand (Vorschauversion)
description: In diesem Abschnitt wird erläutert, wie JSON-Dateien mithilfe von SQL On-Demand in Azure Synapse Analytics gelesen werden.
services: synapse-analytics
author: azaricstefan
ms.service: synapse-analytics
ms.topic: how-to
ms.subservice: sql
ms.date: 05/20/2020
ms.author: v-stazar
ms.reviewer: jrasnick, carlrab
ms.openlocfilehash: 04b2d7842222426010b76a1a7ed4c72ee74e3d87
ms.sourcegitcommit: 11e2521679415f05d3d2c4c49858940677c57900
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/31/2020
ms.locfileid: "87489723"
---
# <a name="query-json-files-using-sql-on-demand-preview-in-azure-synapse-analytics"></a>Abfragen von JSON-Dateien mit SQL On-Demand (Vorschauversion) in Azure Synapse Analytics

In diesem Artikel erfahren Sie, wie Sie eine Abfrage mit SQL On-Demand (Vorschauversion) in Azure Synapse Analytics schreiben können. Das Ziel der Abfrage ist das Lesen von JSON-Dateien mithilfe von [OPENROWSET](develop-openrowset.md). 
- JSON-Standarddateien, in denen mehrere JSON-Dokumente als JSON-Array gespeichert sind.
- Durch Zeilen getrennte JSON-Dateien, in denen JSON-Dokumente durch Zeilenumbruchzeichen getrennt sind. Gängige Erweiterungen für diese Dateitypen sind `jsonl`, `ldjson`und `ndjson`.

## <a name="read-json-documents"></a>Lesen von JSON-Dokumenten

Am einfachsten können Sie den Inhalt Ihrer JSON-Datei anzeigen, indem Sie der Funktion `OPENROWSET` die Datei-URL bereitstellen, CSV als `FORMAT` angeben und den Wert `0x0b` für `fieldterminator` und `fieldquote` festlegen. Wenn Sie durch Zeilen getrennte JSON-Dateien lesen müssen, ist dies ausreichend. Wenn Sie über eine klassische JSON-Datei verfügen, müssen Sie für `rowterminator` den Wert `0x0b` festlegen. Die Funktion `OPENROWSET` analysiert die JSON-Datei und gibt jedes Dokument im folgenden Format zurück:

| DOC |
| --- |
|{"date_rep":"2020-07-24","day":24,"month":7,"year":2020,"cases":3,"deaths":0,"geo_id":"AF"}|
|{"date_rep":"2020-07-25","day":25,"month":7,"year":2020,"cases":7,"deaths":0,"geo_id":"AF"}|
|{"date_rep":"2020-07-26","day":26,"month":7,"year":2020,"cases":4,"deaths":0,"geo_id":"AF"}|
|{"date_rep":"2020-07-27","day":27,"month":7,"year":2020,"cases":8,"deaths":0,"geo_id":"AF"}|

Wenn die Datei öffentlich verfügbar ist oder Ihre Azure AD-Identität auf diese Datei zugreifen kann, sollten Sie den Inhalt der Datei mithilfe einer Abfrage wie in den folgenden Beispielen anzeigen können.

### <a name="read-json-files"></a>Lesen von JSON-Dateien

Die folgende Beispielabfrage liest JSON- und durch Zeilen getrennte JSON-Dateien und gibt jedes Dokument als separate Zeile zurück.

```sql
select top 10 *
from openrowset(
        bulk 'https://pandemicdatalake.blob.core.windows.net/public/curated/covid-19/ecdc_cases/latest/ecdc_cases.jsonl',
        format = 'csv',
        fieldterminator ='0x0b',
        fieldquote = '0x0b'
    ) with (doc nvarchar(max)) as rows
go
select top 10 *
from openrowset(
        bulk 'https://pandemicdatalake.blob.core.windows.net/public/curated/covid-19/ecdc_cases/latest/ecdc_cases.json',
        format = 'csv',
        fieldterminator ='0x0b',
        fieldquote = '0x0b',
        rowterminator = '0x0b' --> You need to override rowterminator to read classic JSON
    ) with (doc nvarchar(max)) as rows
```

Diese Abfrage gibt jedes JSON-Dokument als separate Zeile des Resultsets zurück. Stellen Sie sicher, dass Sie auf diese Datei zugreifen können. Wenn Ihre Datei mit einem SAS-Schlüssel oder einer benutzerdefinierten Identität geschützt ist, müssen Sie [Anmeldeinformationen auf Serverebene für die SQL-Anmeldung](develop-storage-files-storage-access-control.md?tabs=shared-access-signature#server-scoped-credential) einrichten. 

### <a name="data-source-usage"></a>Datenquellennutzung

Im vorherigen Beispiel wird der vollständige Pfad zur Datei verwendet. Alternativ können Sie eine externe Datenquelle mit dem Speicherort erstellen, der auf den Stammordner des Speichers verweist, und diese Datenquelle sowie den relativen Pfad zur Datei in der Funktion `OPENROWSET` verwenden:

```sql
create external data source covid
with ( location = 'https://pandemicdatalake.blob.core.windows.net/public/curated/covid-19/ecdc_cases' );
go
select top 10 *
from openrowset(
        bulk 'latest/ecdc_cases.jsonl',
        data_source = 'covid',
        format = 'csv',
        fieldterminator ='0x0b',
        fieldquote = '0x0b'
    ) with (doc nvarchar(max)) as rows
go
select top 10 *
from openrowset(
        bulk 'latest/ecdc_cases.json',
        data_source = 'covid',
        format = 'csv',
        fieldterminator ='0x0b',
        fieldquote = '0x0b',
        rowterminator = '0x0b' --> You need to override rowterminator to read classic JSON
    ) with (doc nvarchar(max)) as rows
```

Wenn eine Datenquelle mit einem SAS-Schlüssel oder einer benutzerdefinierten Identität geschützt ist, können Sie die [Datenquelle mit datenbankweit gültigen Anmeldeinformationen](develop-storage-files-storage-access-control.md?tabs=shared-access-signature#database-scoped-credential) konfigurieren.

In den folgenden Abschnitten erfahren Sie, wie Sie verschiedene Typen von JSON-Dateien abfragen.

## <a name="parse-json-documents"></a>Analysieren von JSON-Dokumenten

Die Abfragen in den vorherigen Beispielen geben jedes JSON-Dokument als einzelne Zeichenfolge in einer separaten Zeile des Resultsets zurück. Mit den Funktionen `JSON_VALUE` und `OPENJSON` können Sie die Werte in JSON-Dokumenten analysieren und als relationale Werte zurückgeben, wie es im folgenden Beispiel gezeigt wird:

| date\_rep | cases | geo\_id |
| --- | --- | --- |
| 2020-07-24 | 3 | AF |
| 2020-07-25 | 7 | AF |
| 2020-07-26 | 4 | AF |
| 2020-07-27 | 8| AF |

### <a name="sample-json-document"></a>JSON-Beispieldokument

Die Abfragebeispiele lesen *JSON*-Dateien, die Dokumente mit der folgenden Struktur enthalten:

```json
{
    "date_rep":"2020-07-24",
    "day":24,"month":7,"year":2020,
    "cases":13,"deaths":0,
    "countries_and_territories":"Afghanistan",
    "geo_id":"AF",
    "country_territory_code":"AFG",
    "continent_exp":"Asia",
    "load_date":"2020-07-25 00:05:14",
    "iso_country":"AF"
}
```

> [!NOTE]
> Wenn diese Dokumente als eine durch Trennzeichen getrennte JSON-Datei gespeichert werden, müssen Sie `FIELDTERMINATOR` und `FIELDQUOTE` auf „0x0b“ festlegen. Wenn Sie über ein JSON-Standardformat verfügen, müssen Sie `ROWTERMINATOR` auf „0x0b“ festlegen.

### <a name="query-json-files-using-json_value"></a>Abfragen von JSON-Dateien mit JSON_VALUE

Die folgende Abfrage zeigt, wie Sie mithilfe von [JSON_VALUE](/sql/t-sql/functions/json-value-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) skalare Werte (Titel, Herausgeber) aus einem JSON-Dokument abrufen:

```sql
select
    JSON_VALUE(doc, '$.date_rep') AS date_reported,
    JSON_VALUE(doc, '$.countries_and_territories') AS country,
    JSON_VALUE(doc, '$.cases') as cases,
    doc
from openrowset(
        bulk 'latest/ecdc_cases.jsonl',
        data_source = 'covid',
        format = 'csv',
        fieldterminator ='0x0b',
        fieldquote = '0x0b'
    ) with (doc nvarchar(max)) as rows
order by JSON_VALUE(doc, '$.geo_id') desc
```

### <a name="query-json-files-using-openjson"></a>Abfragen von JSON-Dateien mit OPENJSON

Die folgende Abfrage verwendet [OPENJSON](/sql/t-sql/functions/openjson-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest). Die in Serbien gemeldeten COVID-Statistiken werden abgerufen:

```sql
select
    *
from openrowset(
        bulk 'latest/ecdc_cases.jsonl',
        data_source = 'covid',
        format = 'csv',
        fieldterminator ='0x0b',
        fieldquote = '0x0b'
    ) with (doc nvarchar(max)) as rows
    cross apply openjson (doc)
        with (  date_rep datetime2,
                cases int,
                fatal int '$.deaths',
                country varchar(100) '$.countries_and_territories')
where country = 'Serbia'
order by country, date_rep desc;
```

## <a name="next-steps"></a>Nächste Schritte

Die nächsten Artikel in dieser Reihe veranschaulichen Folgendes:

- [Abfragen von Ordnern und mehreren Dateien](query-folders-multiple-csv-files.md)
- [Erstellen und Verwenden von Sichten](create-use-views.md)
