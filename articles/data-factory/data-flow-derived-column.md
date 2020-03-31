---
title: Transformation für abgeleitete Spalten in Mapping Data Flow
description: Informationen zum bedarfsorientierten Transformieren von Daten in Azure Data Factory mithilfe der Mapping Data Flow-Transformation für abgeleitete Spalten.
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 10/15/2019
ms.openlocfilehash: 66396de52b3709c1d9357f32a375a29a8dcdbd1d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "77048751"
---
# <a name="derived-column-transformation-in-mapping-data-flow"></a>Transformation für abgeleitete Spalten in Mapping Data Flow

Verwenden Sie die Transformation für abgeleitete Spalten, um in Ihrem Datenfluss neue Spalten zu generieren oder vorhandene Felder zu ändern.

## <a name="derived-column-settings"></a>Einstellungen für abgeleitete Spalten

Sie können eine bereits vorhandene Spalte löschen, indem Sie diese über die Dropdownliste mit den Spalten auswählen. Alternativ können Sie auch das Feld „Spaltenauswahl“ als Textfeld verwenden und den Namen der neuen Spalte eingeben. Den Ausdruck der abgeleiteten Spalte können Sie erstellen, indem Sie auf das Feld „Ausdruck eingeben“ klicken, um den [Data Flow-Ausdrucks-Generator](concepts-data-flow-expression-builder.md) zu öffnen.

![Einstellungen für abgeleitete Spalten](media/data-flow/dc1.png "Einstellungen für abgeleitete Spalten")

Sie können weitere abgeleitete Spalten hinzufügen, indem Sie mit dem Mauszeiger auf eine vorhandene Spalte zeigen und auf das Pluszeichen klicken. Klicken Sie entweder auf **Spalte hinzufügen** oder auf **Spaltenmuster hinzufügen**. Spaltenmuster können nützlich sein, wenn sich Ihre Spaltennamen von den Quellen unterscheiden. Weitere Informationen finden Sie unter [Column Patterns (Spaltenmuster)](concepts-data-flow-column-pattern.md).

![Auswahl für neue abgeleitete Spalte](media/data-flow/columnpattern.png "Auswahl für neue abgeleitete Spalte")

## <a name="build-schemas-in-output-schema-pane"></a>Erstellen von Schemas im Ausgabeschemabereich

Die Spalten, die Sie ändern und dem Schema hinzufügen, werden im Ausgabeschemabereich aufgeführt. Hier können Sie auf interaktive Weise einfache und komplexe Datenstrukturen erstellen. Wenn Sie weitere Felder hinzufügen möchten, wählen Sie **Spalte hinzufügen** aus. Wenn Sie Hierarchien erstellen möchten, wählen Sie **Unterspalte hinzufügen** aus.

![Hinzufügen einer Unterspalte](media/data-flow/addsubcolumn.png "Hinzufügen einer Unterspalte")

Weitere Informationen zum Umgang mit komplexen Typen im Datenfluss finden Sie unter [JSON-Verarbeitung mit Mapping Data Flow](format-json.md#mapping-data-flow-properties).

![Hinzufügen einer komplexen Spalte](media/data-flow/complexcolumn.png "Hinzufügen von Spalten")

## <a name="data-flow-script"></a>Datenflussskript

### <a name="syntax"></a>Syntax

```
<incomingStream>
    derive(
           <columnName1> = <expression1>,
           <columnName2> = <expression2>,
           each(
                match(matchExpression),
                <metadataColumn1> = <metadataExpression1>,
                <metadataColumn2> = <metadataExpression2>
               )
          ) ~> <deriveTransformationName>
```

### <a name="example"></a>Beispiel

Das folgende Beispiel ist eine abgeleitete Spalte mit dem Namen `CleanData`, die aus einem eingehenden Datenstrom `MoviesYear` zwei abgeleitete Spalten erstellt. Die erste abgeleitete Spalte ersetzt die Spalte `Rating` durch den Wert für „Rating“ als ganzzahliger Typ. Die zweite abgeleitete Spalte ist ein Muster, das jede Spalte abgleicht, deren Name mit „movies“ beginnt. Für jede übereinstimmende Spalte wird eine Spalte `movie` erstellt, die dem Wert der übereinstimmenden Spalte entspricht und das Präfix „movie_“ erhält. 

Auf der Data Factory-Benutzeroberfläche sieht diese Transformation wie folgt aus:

![Ableitungsbeispiel](media/data-flow/derive-script1.png "Ableitungsbeispiel")

Das Datenflussskript für diese Transformation befindet sich im folgenden Codeausschnitt:

```
MoviesYear derive(
                Rating = toInteger(Rating),
                each(
                    match(startsWith(name,'movies')),
                    'movie' = 'movie_' + toString($$)
                )
            ) ~> CleanData
```

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen finden Sie unter [Mapping Data Flow expression language (Mapping Data Flow-Ausdruckssprache)](data-flow-expression-functions.md).
