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
ms.openlocfilehash: 5d02736e9cb0a612e434dc5a79a73d7a62785728
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85207650"
---
# <a name="query-json-files-using-sql-on-demand-preview-in-azure-synapse-analytics"></a>Abfragen von JSON-Dateien mit SQL On-Demand (Vorschauversion) in Azure Synapse Analytics

In diesem Artikel erfahren Sie, wie Sie eine Abfrage mit SQL On-Demand (Vorschauversion) in Azure Synapse Analytics schreiben können. Das Ziel der Abfrage ist das Lesen von JSON-Dateien. Unterstützte Formate sind in [OPENROWSET](develop-openrowset.md) aufgeführt.

## <a name="prerequisites"></a>Voraussetzungen

Im ersten Schritt **erstellen Sie eine Datenbank**, in der Sie die Abfragen ausführen. Initialisieren Sie dann die Objekte, indem Sie das [Setupskript](https://github.com/Azure-Samples/Synapse/blob/master/SQL/Samples/LdwSample/SampleDB.sql) für diese Datenbank ausführen. Mit diesem Setupskript werden die Datenquellen, die für die gesamte Datenbank gültigen Anmeldeinformationen und externe Dateiformate erstellt, die in diesen Beispielen verwendet werden.

## <a name="sample-json-files"></a>JSON-Beispieldateien

Der folgende Abschnitt enthält Beispielskripts zum Lesen von JSON-Dateien. Die Dateien sind in einem *JSON*-Container im Ordner *books* gespeichert und enthalten einen einzelnen Bucheintrag mit folgender Struktur:

```json
{
   "_id":"ahokw88",
   "type":"Book",
   "title":"The AWK Programming Language",
   "year":"1988",
   "publisher":"Addison-Wesley",
   "authors":[
      "Alfred V. Aho",
      "Brian W. Kernighan",
      "Peter J. Weinberger"
   ],
   "source":"DBLP"
}
```

## <a name="read-json-files"></a>Lesen von JSON-Dateien

Um JSON-Dateien mit JSON_VALUE und [JSON_QUERY](/sql/t-sql/functions/json-query-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) zu verarbeiten, müssen Sie die JSON-Datei als eine einzelne Spalte aus dem Speicher lesen. Das folgende Skript liest die Datei *book1.json* als einzelne Spalte:

```sql
SELECT
    *
FROM
    OPENROWSET(
        BULK 'json/books/book1.json',
        DATA_SOURCE = 'SqlOnDemandDemo',
        FORMAT='CSV',
        FIELDTERMINATOR ='0x0b',
        FIELDQUOTE = '0x0b',
        ROWTERMINATOR = '0x0b'
    )
    WITH (
        jsonContent varchar(8000)
    ) AS [r];
```

> [!NOTE]
> Sie lesen die gesamte JSON-Datei als einzelne Zeile oder Spalte. Daher sind FIELDTERMINATOR, FIELDQUOTE und ROWTERMINATOR auf 0x0b festgelegt.

## <a name="query-json-files-using-json_value"></a>Abfragen von JSON-Dateien mit JSON_VALUE

Die folgende Abfrage zeigt, wie Sie mithilfe von [JSON_VALUE](/sql/t-sql/functions/json-value-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) skalare Werte (Titel, Verleger) aus einem Buch mit dem Titel *Probabilistic and Statistical Methods in Cryptology, An Introduction by Selected Topics* abrufen:

```sql
SELECT
    JSON_VALUE(jsonContent, '$.title') AS title,
    JSON_VALUE(jsonContent, '$.publisher') as publisher,
    jsonContent
FROM
    OPENROWSET(
        BULK 'json/books/*.json',
        DATA_SOURCE = 'SqlOnDemandDemo',
        FORMAT='CSV',
        FIELDTERMINATOR ='0x0b',
        FIELDQUOTE = '0x0b',
        ROWTERMINATOR = '0x0b'
    )
    WITH (
        jsonContent varchar(8000)
    ) AS [r]
WHERE
    JSON_VALUE(jsonContent, '$.title') = 'Probabilistic and Statistical Methods in Cryptology, An Introduction by Selected Topics';
```

## <a name="query-json-files-using-json_query"></a>Abfragen von JSON-Dateien mit JSON_QUERY

Die folgende Abfrage zeigt Ihnen, wie Sie mit [JSON_QUERY](/sql/t-sql/functions/json-query-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) Objekte und Arrays (Autoren) aus einem Buch mit dem Titel *Probabilistic and Statistical Methods in Cryptology, An Introduction by Selected Topics* abrufen:

```sql
SELECT
    JSON_QUERY(jsonContent, '$.authors') AS authors,
    jsonContent
FROM
    OPENROWSET(
        BULK 'json/books/*.json',
        DATA_SOURCE = 'SqlOnDemandDemo',
        FORMAT='CSV',
        FIELDTERMINATOR ='0x0b',
        FIELDQUOTE = '0x0b',
        ROWTERMINATOR = '0x0b'
    )
    WITH (
        jsonContent varchar(8000)
    ) AS [r]
WHERE
    JSON_VALUE(jsonContent, '$.title') = 'Probabilistic and Statistical Methods in Cryptology, An Introduction by Selected Topics';
```

## <a name="query-json-files-using-openjson"></a>Abfragen von JSON-Dateien mit OPENJSON

Die folgende Abfrage verwendet [OPENJSON](/sql/t-sql/functions/openjson-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest). Sie ruft Objekte und Eigenschaften in einem Buch mit dem Titel *Probabilistic and Statistical Methods in Cryptology, An Introduction by Selected Articles* ab:

```sql
SELECT
    j.*
FROM
    OPENROWSET(
        BULK 'json/books/*.json',
        DATA_SOURCE = 'SqlOnDemandDemo',
        FORMAT='CSV',
        FIELDTERMINATOR ='0x0b',
        FIELDQUOTE = '0x0b',
        ROWTERMINATOR = '0x0b'
    )
    WITH (
        jsonContent NVARCHAR(max) -- Use appropriate length. Make sure JSON fits. 
    ) AS [r]
CROSS APPLY OPENJSON(jsonContent) AS j
WHERE
    JSON_VALUE(jsonContent, '$.title') = 'Probabilistic and Statistical Methods in Cryptology, An Introduction by Selected Topics';
```

## <a name="next-steps"></a>Nächste Schritte

Die nächsten Artikel in dieser Reihe veranschaulichen Folgendes:

- [Abfragen von Ordnern und mehreren Dateien](query-folders-multiple-csv-files.md)
- [Erstellen und Verwenden von Sichten](create-use-views.md)
