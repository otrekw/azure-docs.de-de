---
title: KQL-Kurzübersicht
description: Eine Liste mit hilfreichen KQL-Funktionen und deren Definitionen mit Syntaxbeispielen.
author: yossi-karp
ms.author: v-yokarp
ms.reviewer: ''
ms.service: data-explorer
ms.topic: conceptual
ms.date: 01/19/2020
ms.openlocfilehash: c5d331b809d3f9f7db8d8c36add8b283bf0b9e39
ms.sourcegitcommit: 747a20b40b12755faa0a69f0c373bd79349f39e3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2020
ms.locfileid: "77664863"
---
# <a name="kql-quick-reference"></a>KQL-Kurzübersicht

Dieser Artikel enthält eine Liste mit Funktionen und Beschreibungen, um Sie bei Ihren ersten Schritten mit der Kusto-Abfragesprache zu unterstützen.

| Operator/Funktion                               | Beschreibung                           | Syntax                                           |
| :---------------------------------------------- | :------------------------------------ |:-------------------------------------------------|
|**Filter/Suche/Bedingung**                      |**_Suche nach relevanten Daten mittels Filter- oder Suchvorgang_** |                      |
| [where](/azure/kusto/query/whereoperator)                      | Filtert nach einem bestimmten Prädikat.           | `T | where Predicate`                         |
| [where contains/has](/azure/kusto/query/whereoperator)        | `Contains`: Sucht nach einer beliebigen Teilzeichenfolgenübereinstimmung. <br> `Has`: Sucht nach einem bestimmten Wort (bessere Leistung).  | `T | where col1 contains/has "[search term]"`|
| [search](/azure/kusto/query/searchoperator)                    | Durchsucht alle Spalten in der Tabelle nach dem Wert. | `[TabularSource |] search [kind=CaseSensitivity] [in (TableSources)] SearchPredicate` |
| [take](/azure/kusto/query/takeoperator)                        | Gibt die angegebene Anzahl von Datensätzen zurück. Verwenden Sie diese Option, um eine Abfrage zu testen.<br>**_Hinweis_** : `_take`_ und `_limit`_ sind Synonyme. | `T | take NumberOfRows` |
| [case](/azure/kusto/query/casefunction)                        | Fügt eine Bedingungsanweisung hinzu (vergleichbar mit „if“/„then“/„elseif“ in anderen Systemen). | `case(predicate_1, then_1, predicate_2, then_2, predicate_3, then_3, else)` |
| [distinct](/azure/kusto/query/distinctoperator)                | Erzeugt eine Tabelle mit der speziellen Kombination der bereitgestellten Spalten der Eingabetabelle. | `distinct [ColumnName], [ColumnName]` |
| **Datum/Uhrzeit**                                   |**_Vorgänge mit Datums- und Uhrzeitfunktionen_**               |                          |
|[ago](/azure/kusto/query/agofunction)                           | Gibt den Zeitversatz relativ zum Zeitpunkt der Abfrageausführung zurück. `ago(1h)` bedeutet beispielsweise „eine Stunde vor dem aktuellen Uhrzeitwert“. | `ago(a_timespan)` |
| [format_datetime](/azure/kusto/query/format-datetimefunction)  | Gibt Daten in [verschiedenen Datumsformaten](/azure/kusto/query/format-datetimefunction#supported-formats) zurück. | `format_datetime(datetime , format)` |
| [bin](/azure/kusto/query/binfunction)                          | Rundet alle Werte in einem Zeitrahmen und gruppiert sie. | `bin(value,roundTo)` |
| **Erstellen/Entfernen von Spalten**                   |**_Hinzufügen oder Entfernen von Spalten in einer Tabelle_** |                                                    |
| [print](/azure/kusto/query/printoperator)                      | Gibt eine einzelne Zeile mit mindestens einem skalaren Ausdruck aus. | `print [ColumnName =] ScalarExpression [',' ...]` |
| [project](/azure/kusto/query/projectoperator)                  | Wählt die einzuschließenden Spalten in der angegebenen Reihenfolge aus. | `T | project ColumnName [= Expression] [, ...]` <br> oder <br> `T | project [ColumnName | (ColumnName[,]) =] Expression [, ...]` |
| [project-away](/azure/kusto/query/projectawayoperator)         | Wählt die Spalten aus, die von der Ausgabe ausgeschlossen werden sollen. | `T | project-away ColumnNameOrPattern [, ...]` |
| [extend](/azure/kusto/query/extendoperator)                    | Erstellt eine berechnete Spalte und fügt sie dem Resultset hinzu. | `T | extend [ColumnName | (ColumnName[, ...]) =] Expression [, ...]` |
| **Sortieren und Aggregieren des Datasets**                 |**_Ändern der Datenstruktur durch sinnvolles Sortieren oder Gruppieren_**|                  |
| [sort](/azure/kusto/query/sortoperator)                        | Sortiert die Zeilen der Eingabetabelle nach mindestens einer Spalte in aufsteigender oder absteigender Reihenfolge. | `T | sort by expression1 [asc|desc], expression2 [asc|desc], …` |
| [top](/azure/kusto/query/topoperator)                          | Gibt die ersten n Zeilen des Datasets zurück, wenn das Dataset mithilfe von `by` sortiert wird. | `T | top numberOfRows by expression [asc|desc] [nulls first|last]` |
| [summarize](/azure/kusto/query/summarizeoperator)              | Gruppiert die Zeilen gemäß den `by`-Gruppenspalten und berechnet Aggregationen für jede Gruppe. | `T | summarize [[Column =] Aggregation [, ...]] [by [Column =] GroupExpression [, ...]]` |
| [count](/azure/kusto/query/countoperator)                       | Zählt Datensätze in der Eingabetabelle (beispielsweise „T“).<br>Dieser Operator ist eine Kurzform von `summarize count() `.| `T | count` |
| [join](/azure/kusto/query/joinoperator)                        | Führt die Zeilen zweier Tabellen zusammen, um eine neue Tabelle zu erzeugen, indem Werte aus den angegebenen Spalten beider Tabellen abgeglichen werden. Unterstützt eine breite Palette von Join-Typen: `flouter`, `inner`, `innerunique`, `leftanti`, `leftantisemi`, `leftouter`, `leftsemi`, `rightanti`, `rightantisemi`, `rightouter`, `rightsemi` | `LeftTable | join [JoinParameters] ( RightTable ) on Attributes` |
| [union](/azure/kusto/query/unionoperator)                      | Akzeptiert zwei oder mehr Tabellen und gibt alle ihre Zeilen zurück. | `[T1] | union [T2], [T3], …` |
| [range](/azure/kusto/query/rangeoperator)                      | Generiert eine Tabelle mit einer arithmetischen Reihe von Werten. | `range columnName from start to stop step step` |
| **Formatieren von Daten**                                 | **_Ändern der Datenstruktur für eine aussagekräftige Ausgabe_** | |
| [lookup](/azure/kusto/query/lookupoperator)                    | Erweitert die Spalten einer Faktentabelle mit nachgeschlagenen Werten aus einer Dimensionstabelle. | `T1 | lookup [kind = (leftouter|inner)] ( T2 ) on Attributes` |
| [mv-expand](/azure/kusto/query/mvexpandoperator)               | Konvertiert dynamische Arrays in Zeilen (mehrwertige Erweiterung). | `T | mv-expand Column` |
| [parse](/azure/kusto/query/parseoperator)                      | Wertet einen Zeichenfolgenausdruck aus und analysiert dessen Wert in eine oder mehrere berechnete Spalten. Ermöglicht die Strukturierung unstrukturierter Daten. | `T | parse [kind=regex  [flags=regex_flags] |simple|relaxed] Expression with * (StringConstant ColumnName [: ColumnType]) *...` |
| [make-series](/azure/kusto/query/make-seriesoperator)          | Erstellt eine Reihe angegebener aggregierter Werte entlang einer angegebenen Achse. | `T | make-series [MakeSeriesParamters] [Column =] Aggregation [default = DefaultValue] [, ...] on AxisColumn from start to end step step [by [Column =] GroupExpression [, ...]]` |
| [let](/azure/kusto/query/letstatement)                         | Bindet einen Namen an Ausdrücke, die auf den gebundenen Wert verweisen können. Bei den Werten kann es sich um Lambdaausdrücke handeln, um Ad-hoc-Funktionen als Teil der Abfrage zu erstellen. Verwenden Sie `let`, um Ausdrücke für Tabellen zu erstellen, deren Ergebnisse wie eine neue Tabelle aussehen. | `let Name = ScalarExpression | TabularExpression | FunctionDefinitionExpression` |
| **Allgemein**                                     | **_Sonstige Vorgänge und Funktionen_** | |
| [invoke](/azure/kusto/query/invokeoperator)                    | Führt die Funktion für die Tabelle aus, die sie als Eingabe erhält. | `T | invoke function([param1, param2])` |
| [evaluate pluginName](/azure/kusto/query/evaluateoperator)     | Überprüft Erweiterungen für die Abfragesprache (Plug-Ins). | `[T |] evaluate [ evaluateParameters ] PluginName ( [PluginArg1 [, PluginArg2]... )` |
| **Visualisierung**                               | **_Vorgänge zum Anzeigen der Daten in einem grafischen Format_** | |
| [render](/azure/kusto/query/renderoperator) | Rendert Ergebnisse als grafische Ausgabe. | `T | render Visualization [with (PropertyName = PropertyValue [, ...] )]` |
