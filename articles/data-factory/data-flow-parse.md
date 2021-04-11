---
title: Analysedatentransformation in einem Zuordnungsdatenfluss
description: Eingebettete Spaltendokumente analysieren
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/08/2021
ms.openlocfilehash: 4db9503ea84ae13148a89a03048c73399413e5cc
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "101710191"
---
# <a name="parse-transformation-in-mapping-data-flow"></a>Analysetransformation in einem Zuordnungsdatenfluss

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Verwenden Sie die Analysetransformation zum Analysieren von Spalten in Ihren Daten, die in Dokumentform sind. Die zurzeit unterstützten Typen von eingebetteten Dokumenten, die analysiert werden können, sind JSON und Text mit Trennzeichen.

> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RWykdO]

## <a name="configuration"></a>Konfiguration

Im Konfigurationspanel für Analysetransformation wählen Sie zuerst den Datentyp aus, der in den Spalten enthalten ist, die Sie inline analysieren möchten. Die Analysetransformation enthält auch die folgenden Konfigurationseinstellungen.

![Analyseeinstellungen](media/data-flow/data-flow-parse-1.png "Analysieren")

### <a name="column"></a>Column

Ähnlich wie bei abgeleiteten Spalten und Aggregaten ändern Sie an dieser Stelle eine vorhandene Spalte, indem Sie sie in der Dropdownauswahl auswählen. Sie können hier aber auch den Namen einer neuen Spalte eingeben. ADF speichert die analysierten Quelldaten in dieser Spalte.

### <a name="expression"></a>Ausdruck

Verwenden Sie den Ausdrucks-Generator zum Festlegen der Quelle für Ihre Analyse. Dazu können Sie einfach die Quellspalte mit den eigenständigen Daten, die Sie analysieren möchten, auswählen oder aber komplexe Ausdrücke zum Analysieren erstellen.

### <a name="output-column-type"></a>Typ der Ausgabespalte

Hier konfigurieren Sie das Zielausgabeschema aus der Analyse, das in eine einzelne Spalte geschrieben wird.

![Analysebeispiel](media/data-flow/data-flow-parse-2.png "Analysebeispiel")

In diesem Beispiel haben wir die Analyse des eingehenden Felds "jsonString" definiert, das Nur-Text ist, aber als JSON-Struktur formatiert wurde. Wir werden die analysierten Ergebnisse als JSON in der neuen Spalte „json“ mit folgendem Schema speichern:

```(trade as boolean, customers as string[])```

Überprüfen Sie auf der Registerkarte „Überprüfen“ und in der Datenvorschau, ob Ihre Ausgabe richtig zugeordnet wird.

## <a name="examples"></a>Beispiele

```
source(output(
        name as string,
        location as string,
        satellites as string[],
        goods as (trade as boolean, customers as string[], orders as (orderId as string, orderTotal as double, shipped as (orderItems as (itemName as string, itemQty as string)[]))[])
    ),
    allowSchemaDrift: true,
    validateSchema: false,
    ignoreNoFilesFound: false,
    documentForm: 'documentPerLine') ~> JsonSource
source(output(
        movieId as string,
        title as string,
        genres as string
    ),
    allowSchemaDrift: true,
    validateSchema: false,
    ignoreNoFilesFound: false) ~> CsvSource
JsonSource derive(jsonString = toString(goods)) ~> StringifyJson
StringifyJson parse(json = jsonString ? (trade as boolean,
        customers as string[]),
    format: 'json',
    documentForm: 'arrayOfDocuments') ~> ParseJson
CsvSource derive(csvString = 'Id|name|year\n\'1\'|\'test1\'|\'1999\'') ~> CsvString
CsvString parse(csv = csvString ? (id as integer,
        name as string,
        year as string),
    format: 'delimited',
    columnNamesAsHeader: true,
    columnDelimiter: '|',
    nullValue: '',
    documentForm: 'documentPerLine') ~> ParseCsv
ParseJson select(mapColumn(
        jsonString,
        json
    ),
    skipDuplicateMapInputs: true,
    skipDuplicateMapOutputs: true) ~> KeepStringAndParsedJson
ParseCsv select(mapColumn(
        csvString,
        csv
    ),
    skipDuplicateMapInputs: true,
    skipDuplicateMapOutputs: true) ~> KeepStringAndParsedCsv
```

## <a name="data-flow-script"></a>Datenflussskript

### <a name="syntax"></a>Syntax

### <a name="examples"></a>Beispiele

```
parse(json = jsonString ? (trade as boolean,
                                customers as string[]),
                format: 'json',
                documentForm: 'singleDocument') ~> ParseJson

parse(csv = csvString ? (id as integer,
                                name as string,
                                year as string),
                format: 'delimited',
                columnNamesAsHeader: true,
                columnDelimiter: '|',
                nullValue: '',
                documentForm: 'documentPerLine') ~> ParseCsv
```    

## <a name="next-steps"></a>Nächste Schritte

* Verwenden Sie die [Vereinfachungstransformation](data-flow-flatten.md) zum Pivotieren von Zeilen zu Spalten.
* Verwenden Sie die [Transformation für abgeleitete Spalten](data-flow-derived-column.md) zum Pivotieren von Spalten zu Zeilen.
