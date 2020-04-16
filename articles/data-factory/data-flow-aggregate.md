---
title: Aggregattransformation in Mapping Data Flow
description: Informationen zum bedarfsorientierten Aggregieren von Daten in Azure Data Factory mithilfe der Mapping Data Flow-Aggregattransformation
author: kromerm
ms.author: makromer
ms.reviewer: daperlov
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 03/24/2020
ms.openlocfilehash: 050fcc33f0e3d65ca8e94b3be143c5b1ae77608d
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2020
ms.locfileid: "81416555"
---
# <a name="aggregate-transformation-in-mapping-data-flow"></a>Aggregattransformation in Mapping Data Flow

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

Die Aggregattransformation definiert Aggregationen von Spalten in Ihren Datenströmen. Mithilfe des Ausdrucks-Generators können Sie verschiedene Arten von Aggregationen wie SUM, MIN, MAX und COUNT gruppiert nach vorhandenen oder berechneten Spalten definieren.

## <a name="group-by"></a>Gruppieren nach

Wählen Sie eine bereits vorhandene Spalte aus, oder erstellen Sie eine neue berechnete Spalte, um diese als GROUP BY-Klausel für Ihre Aggregation zu verwenden. Wenn Sie eine vorhandene Spalte verwenden möchten, wählen Sie sie aus der Dropdownliste aus. Zum Erstellen einer neuen berechneten Spalte zeigen Sie mit dem Mauszeiger auf die Klausel, und klicken Sie auf **Berechnete Spalte**. Dadurch öffnet sich der [Datenfluss-Ausdrucks-Generator](concepts-data-flow-expression-builder.md). Sobald Sie die berechnete Spalte erstellt haben, geben Sie den Namen der Ausgabespalte in das Feld **Benennen als** ein. Wenn Sie eine zusätzliche GROUP BY-Klausel hinzufügen möchten, zeigen Sie mit dem Mauszeiger auf eine bereits vorhandene Klausel, und klicken Sie auf das Pluszeichen.

![Einstellungen für „Gruppieren nach“ in der Aggregattransformation](media/data-flow/agg.png "Einstellungen für „Gruppieren nach“ in der Aggregattransformation")

Die Verwendung einer GROUP BY-Klausel in einer Aggregattransformation ist optional.

## <a name="aggregate-column"></a>Aggregatspalte 

Wechseln Sie zur Registerkarte **Aggregate**, um Aggregationsausdrücke zu erstellen. Sie können entweder eine bereits vorhandene Spalte mit einer Aggregation überschreiben oder ein neues Feld mit einem neuen Namen erstellen. Der Aggregationsausdruck wird in das Feld auf der rechten Seite neben dem Selektor für den Spaltennamen eingegeben. Sie können den Ausdruck bearbeiten, indem Sie auf das Textfeld klicken, um den Ausdrucks-Generator zu öffnen. Wenn Sie zusätzliche Aggregationen hinzufügen möchten, zeigen Sie mit dem Mauszeiger auf einen bereits vorhandenen Ausdruck, und klicken Sie auf das Pluszeichen, um eine neue Aggregationsspalte oder ein [Spaltenmuster](concepts-data-flow-column-pattern.md) zu erstellen.

Jeder Aggregationsausdruck muss mindestens eine Aggregatfunktion enthalten.

![Aggregateinstellungen der Aggregattransformation](media/data-flow/agg2.png "Aggregateinstellungen der Aggregattransformation")


> [!NOTE]
> Im Debugmodus kann der Ausdrucks-Generator keine Datenvorschau mit Aggregatfunktionen generieren. Wenn Sie die Datenvorschau für eine Aggregattransformation abrufen möchten, schließen Sie den Ausdrucks-Generator, und rufen Sie die Daten über die Registerkarte „Datenvorschau“ ab.

## <a name="reconnect-rows-and-columns"></a>Neuverbinden von Zeilen und Spalten

Aggregattransformationen ähneln SQL-Abfragen für die Aggregatauswahl. Spalten, die nicht in der GROUP BY-Klausel oder in den Aggregatfunktionen enthalten sind, fließen nicht in die Ausgabe der Aggregattransformation. Wenn Sie andere Spalten in die aggregierte Ausgabe einschließen möchten, führen Sie eine der folgenden Methoden aus:

* Verwenden Sie eine Aggregatfunktion, z.B. `last()` oder `first()`, um die zusätzliche Spalte einzuschließen.
* Verknüpfen Sie die Spalten mithilfe des [Self-Join-Musters](https://mssqldude.wordpress.com/2018/12/20/adf-data-flows-self-join/) erneut mit dem Ausgabedatenstrom.

## <a name="removing-duplicate-rows"></a>Entfernen doppelter Zeilen

Aggregattransformationen werden häufig für das Entfernen oder Identifizieren doppelter Einträge in Quelldaten verwendet. Dieser Prozess wird als Deduplizierung bezeichnet. Verwenden Sie basierend auf einem Satz von Group by-Schlüsseln eine Heuristik Ihrer Wahl, um zu ermitteln, welche der doppelten Zeilen beibehalten werden sollen. Häufige Heuristiken sind `first()`, `last()`, `max()` und `min()`. Verwenden Sie [Spaltenmuster](concepts-data-flow-column-pattern.md), um die Regel auf alle Spalten mit Ausnahme der Group by-Spalten anzuwenden.

![Deduplizierung](media/data-flow/agg-dedupe.png "Deduplizierung")

Im obigen Beispiel werden die Spalten `ProductID` und `Name` für die Gruppierung verwendet. Wenn zwei Zeilen identische Werte für diese beiden Spalten aufweisen, werden sie als Duplikate angesehen. In dieser Aggregattransformation werden die Werte der ersten übereinstimmenden Zeile beibehalten, und alle anderen werden entfernt. Mithilfe der Syntax von Spaltenmustern werden alle Spalten, deren Namen nicht `ProductID` und `Name` sind, dem vorhandenen Spaltennamen zugeordnet. Außerdem erhalten sie den Wert der ersten übereinstimmenden Zeile. Das Ausgabeschema ist mit dem Eingabeschema identisch.

Bei Datenüberprüfungsszenarien können Sie mithilfe der `count()`-Funktion zählen, wie viele Duplikate vorhanden sind.

## <a name="data-flow-script"></a>Datenflussskript

### <a name="syntax"></a>Syntax

```
<incomingStream>
    aggregate(
           groupBy(
                <groupByColumnName> = <groupByExpression1>,
                <groupByExpression2>
               ),
           <aggregateColumn1> = <aggregateExpression1>,
           <aggregateColumn2> = <aggregateExpression2>,
           each(
                match(matchExpression),
                <metadataColumn1> = <metadataExpression1>,
                <metadataColumn2> = <metadataExpression2>
               )
          ) ~> <aggregateTransformationName>
```

### <a name="example"></a>Beispiel

Im folgenden Beispiel werden bei einem eingehenden Datenstrom `MoviesYear` die Zeilen nach der Spalte `year` gruppiert. Die Transformation erstellt eine Aggregatspalte `avgrating`, die den Durchschnitt der Spalte `Rating` ergibt. Diese Aggregattransformation hat den Namen `AvgComedyRatingsByYear`.

Auf der Data Factory-Benutzeroberfläche sieht diese Transformation wie folgt aus:

![Beispiel für „Gruppieren nach“](media/data-flow/agg-script1.png "Beispiel für „Gruppieren nach“")

![Aggregatbeispiel](media/data-flow/agg-script2.png "Aggregatbeispiel")

Das Datenflussskript für diese Transformation befindet sich im folgenden Codeausschnitt.

```
MoviesYear aggregate(
                groupBy(year),
                avgrating = avg(toInteger(Rating))
            ) ~> AvgComedyRatingByYear
```

![Skript zum Aggregieren von Datenflüssen](media/data-flow/aggdfs1.png "Skript zum Aggregieren von Datenflüssen")

```MoviesYear```: Abgeleitete Spalte, die das Jahr und die Titelspalten definiert: ```AvgComedyRatingByYear``` Aggregattransformation für die durchschnittliche Bewertung von Komödien gruppiert nach Jahr: ```avgrating``` Name der neuen Spalte, die erstellt wird, um den aggregierten Wert aufzunehmen

```
MoviesYear aggregate(groupBy(year),
    avgrating = avg(toInteger(Rating))) ~> AvgComedyRatingByYear
```

## <a name="next-steps"></a>Nächste Schritte

* Definieren Sie eine fensterbasierte Aggregation mithilfe der [Fenstertransformation](data-flow-window.md).
