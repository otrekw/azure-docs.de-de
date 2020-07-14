---
title: Transformationsfunktionen im Wranglingdatenfluss in Azure Data Factory
description: Übersicht über die verfügbaren Funktionen im Wranglingdatenfluss in Azure Data Factory
author: djpmsft
ms.author: daperlov
ms.reviewer: gamal
ms.service: data-factory
ms.topic: conceptual
ms.date: 11/01/2019
ms.openlocfilehash: e63c3f329cb9c1fd5ca91274540f5145c3ad098a
ms.sourcegitcommit: dee7b84104741ddf74b660c3c0a291adf11ed349
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85921553"
---
# <a name="transformation-functions-in-wrangling-data-flow"></a>Transformationsfunktionen im Wranglingdatenfluss

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

Der Wranglingdatenfluss in Azure Data Factory ermöglicht die codefreie agile Datenaufbereitung und Data Wrangling für die Cloud. Der Wranglingdatenfluss kann in [Power Query Online](https://docs.microsoft.com/powerquery-m/power-query-m-reference) integriert werden und stellt Power Query M-Funktionen für Data Wrangling über Spark-Ausführung bereit. 

> [!NOTE]
> Wranglingdatenflüsse sind derzeit in der öffentlichen Vorschau verfügbar

Derzeit werden nicht alle Power Query M-Funktionen für Data Wrangling unterstützt, obwohl sie während der Erstellung verfügbar sind. Beim Erstellen von Wranglingdatenflüssen wird die folgende Fehlermeldung angezeigt, wenn eine Funktion nicht unterstützt wird:

`The wrangling data flow is invalid. Expression.Error: The transformation logic isn't supported. Please try a simpler expression`

Die unterstützten Power Query M-Funktionen sind im Folgenden aufgeführt.

## <a name="column-management"></a>Spaltenverwaltung

* Auswahl: [Table.SelectColumns](https://docs.microsoft.com/powerquery-m/table-selectcolumns)
* Entfernung: [Table.RemoveColumns](https://docs.microsoft.com/powerquery-m/table-removecolumns)
* Umbenennung: [Table.RenameColumns](https://docs.microsoft.com/powerquery-m/table-renamecolumns), [Table.PrefixColumns](https://docs.microsoft.com/powerquery-m/table-prefixcolumns), [Table.TransformColumnNames](https://docs.microsoft.com/powerquery-m/table-transformcolumnnames)
* Neuanordnung: [Table.ReorderColumns](https://docs.microsoft.com/powerquery-m/table-reordercolumns)

## <a name="row-filtering"></a>Zeilenfilterung

Verwenden Sie die M-Funktion [Table.SelectRows](https://docs.microsoft.com/powerquery-m/table-selectrows), um Zeilen nach den folgenden Bedingungen zu filtern:

* Gleichheit und Ungleichheit
* Vergleiche von Zahlen, Text und Datumsangaben (jedoch nicht von Datum/Uhrzeit)
* Zahlenangaben, z. B. [Number.IsEven](https://docs.microsoft.com/powerquery-m/number-iseven)/[Odd](https://docs.microsoft.com/powerquery-m/number-iseven)
* Texteingrenzung unter Verwendung von [Text.Contains](https://docs.microsoft.com/powerquery-m/text-contains), [Text.StartsWith](https://docs.microsoft.com/powerquery-m/text-startswith) oder [Text.EndsWith](https://docs.microsoft.com/powerquery-m/text-endswith)
* Datumsbereiche einschließlich aller [Datumsfunktionen](https://docs.microsoft.com/powerquery-m/date-functions) mit „IsIn“) 
* Kombinationen dieser Bedingungen unter Verwendung der Bedingungen „and“, „or“ oder „not“

## <a name="adding-and-transforming-columns"></a>Hinzufügen und Transformieren von Spalten

Spalten lassen sich mit den folgenden M-Funktionen hinzufügen oder transformieren: [Table.AddColumn](https://docs.microsoft.com/powerquery-m/table-addcolumn), [Table.TransformColumns](https://docs.microsoft.com/powerquery-m/table-transformcolumns), [Table.ReplaceValue](https://docs.microsoft.com/powerquery-m/table-replacevalue), [Table.DuplicateColumn](https://docs.microsoft.com/powerquery-m/table-duplicatecolumn). Nachfolgend sind die unterstützten Transformationsfunktionen aufgeführt.

* Numerische Arithmetik
* Textverkettung
* Arithmetik für Datum und Uhrzeit (arithmetische Operatoren [Date.AddDays](https://docs.microsoft.com/powerquery-m/date-adddays), [Date.AddMonths](https://docs.microsoft.com/powerquery-m/date-addmonths), [Date.AddQuarters](https://docs.microsoft.com/powerquery-m/date-addquarters), [Date.AddWeeks](https://docs.microsoft.com/powerquery-m/date-addweeks), [Date.AddYears](https://docs.microsoft.com/powerquery-m/date-addyears))
* Dauern können für die Arithmetik für Datum und Uhrzeit verwendet werden, müssen jedoch vor dem Schreiben in eine Senke in einen anderen Typ transformiert werden (arithmetische Operatoren [#duration](https://docs.microsoft.com/powerquery-m/sharpduration), [Duration.Days](https://docs.microsoft.com/powerquery-m/duration-days), [Duration.Hours](https://docs.microsoft.com/powerquery-m/duration-hours), [Duration.Minutes](https://docs.microsoft.com/powerquery-m/duration-minutes), [Duration.Seconds](https://docs.microsoft.com/powerquery-m/duration-seconds), [Duration.TotalDays](https://docs.microsoft.com/powerquery-m/duration-totaldays), [Duration.TotalHours](https://docs.microsoft.com/powerquery-m/duration-totalhours), [Duration.TotalMinutes](https://docs.microsoft.com/powerquery-m/duration-totalminutes), [Duration.TotalSeconds](https://docs.microsoft.com/powerquery-m/duration-totalseconds))    
* Großteil der standardmäßigen, wissenschaftlichen und trigonometrischen numerischen Funktionen (alle Funktionen unter [Vorgänge](https://docs.microsoft.com/powerquery-m/number-functions#operations), [Rundung](https://docs.microsoft.com/powerquery-m/number-functions#rounding) und [Trigonometrie](https://docs.microsoft.com/powerquery-m/number-functions#trigonometry), *mit Ausnahme von* Number.Factorial, Number.Permutations und Number.Combinations)
* Ersetzung ([Replacer.ReplaceText](https://docs.microsoft.com/powerquery-m/replacer-replacetext), [Replacer.ReplaceValue](https://docs.microsoft.com/powerquery-m/replacer-replacevalue), [Text.Replace](https://docs.microsoft.com/powerquery-m/text-replace), [Text.Remove](https://docs.microsoft.com/powerquery-m/text-remove))
* Textextraktion in Bezug auf die Position ([Text.PositionOf](https://docs.microsoft.com/powerquery-m/text-positionof), [Text.Length](https://docs.microsoft.com/powerquery-m/text-length), [Text.Start](https://docs.microsoft.com/powerquery-m/text-start), [Text.End](https://docs.microsoft.com/powerquery-m/text-end), [Text.Middle](https://docs.microsoft.com/powerquery-m/text-middle), [Text.ReplaceRange](https://docs.microsoft.com/powerquery-m/text-replacerange), [Text.RemoveRange](https://docs.microsoft.com/powerquery-m/text-removerange))
* Grundlegende Textformatierung ([Text.Lower](https://docs.microsoft.com/powerquery-m/text-lower), [Text.Upper](https://docs.microsoft.com/powerquery-m/text-upper), [Text.Trim](https://docs.microsoft.com/powerquery-m/text-trim)/[Start](https://docs.microsoft.com/powerquery-m/text-trimstart)/[End](https://docs.microsoft.com/powerquery-m/text-trimend), [Text.PadStart](https://docs.microsoft.com/powerquery-m/text-padstart)/[End](https://docs.microsoft.com/powerquery-m/text-padend), [Text.Reverse](https://docs.microsoft.com/powerquery-m/text-reverse))
* Datum/Uhrzeit-Funktionen ([Date.Day](https://docs.microsoft.com/powerquery-m/date-day), [Date.Month](https://docs.microsoft.com/powerquery-m/date-month), [Date.Year](https://docs.microsoft.com/powerquery-m/date-year) [Time.Hour](https://docs.microsoft.com/powerquery-m/time-hour), [Time.Minute](https://docs.microsoft.com/powerquery-m/time-minute), [Time.Second](https://docs.microsoft.com/powerquery-m/time-second), [Date.DayOfWeek](https://docs.microsoft.com/powerquery-m/date-dayofweek), [Date.DayOfYear](https://docs.microsoft.com/powerquery-m/date-dayofyear), [Date.DaysInMonth](https://docs.microsoft.com/powerquery-m/date-daysinmonth))
* If-Ausdrücke (Verzweigungen müssen jedoch übereinstimmende Typen aufweisen)
* Zeilenfilter als logische Spalte
* number-, text-, logical-, date- und datetime-Konstanten

<a name="mergingjoining-tables"></a>Zusammenführen und Verknüpfen von Tabellen
----------------------
* Power Query generiert eine geschachtelte Verknüpfung (Table.NestedJoin; Benutzer können auch manuell [Table.AddJoinColumn](https://docs.microsoft.com/powerquery-m/table-addjoincolumn) schreiben).
    Benutzer müssen dann die geschachtelte Verknüpfungsspalte in eine nicht geschachtelten Verknüpfung erweitern (Table.ExpandTableColumn, wird in keinem anderen Kontext unterstützt).
* Die M-Funktion [Table.Join](https://docs.microsoft.com/powerquery-m/table-join) kann direkt geschrieben werden, um einen zusätzlichen Erweiterungsschritt zu vermeiden, der Benutzer muss jedoch sicherstellen, dass in den verknüpften Tabellen keine doppelten Spaltennamen vorhanden sind.
* Unterstützte Verknüpfungsarten:   [Inner](https://docs.microsoft.com/powerquery-m/joinkind-inner), [LeftOuter](https://docs.microsoft.com/powerquery-m/joinkind-leftouter), [RightOuter](https://docs.microsoft.com/powerquery-m/joinkind-rightouter), [FullOuter](https://docs.microsoft.com/powerquery-m/joinkind-fullouter)
* [Value.Equals](https://docs.microsoft.com/powerquery-m/value-equals) sowie [Value.NullableEquals](https://docs.microsoft.com/powerquery-m/value-nullableequals) werden als Vergleichsfunktionen für Schlüsselgleichheit unterstützt.

## <a name="group-by"></a>Gruppieren nach

Verwenden Sie [Table.Group](https://docs.microsoft.com/powerquery-m/table-group), um Werte zu aggregieren.
* Muss mit einer Aggregationsfunktion verwendet werden.
* Unterstützte Aggregationsfunktionen:   [Table.RowCount](https://docs.microsoft.com/powerquery-m/table-rowcount), [List.Sum](https://docs.microsoft.com/powerquery-m/list-sum), [List.Count](https://docs.microsoft.com/powerquery-m/list-count), [List.Average](https://docs.microsoft.com/powerquery-m/list-average), [List.Min](https://docs.microsoft.com/powerquery-m/list-min), [List.Max](https://docs.microsoft.com/powerquery-m/list-max), [List.StandardDeviation](https://docs.microsoft.com/powerquery-m/list-standarddeviation), [List.First](https://docs.microsoft.com/powerquery-m/list-first), [List.Last](https://docs.microsoft.com/powerquery-m/list-last)

## <a name="sorting"></a>Sortierung

Verwenden Sie [Table.Sort](https://docs.microsoft.com/powerquery-m/table-sort), um Werte zu sortieren.

## <a name="reducing-rows"></a>Verringern von Zeilen

„Erste Zeilen beibehalten“ und „Erste Zeilen entfernen“, „Zeilenbereich beibehalten“ (entsprechende M-Funktionen unterstützen nur die Anzahl und keine Bedingungen: [Table.FirstN](https://docs.microsoft.com/powerquery-m/table-firstn), [Table.Skip](https://docs.microsoft.com/powerquery-m/table-skip), [Table.RemoveFirstN](https://docs.microsoft.com/powerquery-m/table-removefirstn), [Table.Range](https://docs.microsoft.com/powerquery-m/table-range), [Table.MinN](https://docs.microsoft.com/powerquery-m/table-minn), [Table.MaxN](https://docs.microsoft.com/powerquery-m/table-maxn))

## <a name="known-unsupported-functions"></a>Bekannte nicht unterstützte Funktionen

| Funktion | Status |
| -- | -- |
| Table.PromoteHeaders | Wird nicht unterstützt. Dasselbe Ergebnis kann erzielt werden, indem „Erste Zeile als Header verwenden“ im Dataset festgelegt wird. |
| Table.CombineColumns | Dies ist ein gängiges Szenario, das nicht direkt unterstützt wird. Es kann jedoch durch Hinzufügen einer neuen Spalte umgesetzt werden, in der zwei angegebene Spalten verkettet sind.  Beispiel: Table.AddColumn(RemoveEmailColumn, "Name", each [FirstName] & " " & [LastName]) |
| Table.TransformColumnTypes | Diese Funktion wird in den meisten Fällen unterstützt. Die folgenden Szenarien werden nicht unterstützt: Transformieren des Typs „string“ in „currency“, Transformieren des Typs „string“ in „time“, Transformieren des Typs „string“ in „Percentage“. |
| Table.NestedJoin | Wenn Sie nur einen Join durchführen, führt dies zu einem Überprüfungsfehler. Die Spalten müssen erweitert werden, damit der Vorgang funktioniert. |
| Table.Distinct | Das Entfernen doppelter Zeilen wird nicht unterstützt. |
| Table.RemoveLastN | Das Entfernen der unteren Zeilen wird nicht unterstützt. |
| Table.RowCount | Nicht unterstützt. Sie können jedoch eine Spalte mit durchgehend leeren Zellen hinzufügen (Bedingungsspalte kann verwendet werden) und dann „Gruppieren nach“ für diese Spalte ausführen. Table.Group wird unterstützt. | 
| Fehlerbehandlung auf Zeilenebene | Die Fehlerbehandlung auf Zeilenebene wird derzeit nicht unterstützt. Wenn Sie z. B. nicht numerische Werte aus einer Spalte herausfiltern möchten, können Sie u. a. die Textspalte in eine Zahl transformieren. Jede Zelle, die nicht transformiert werden kann, weist einen Fehlerstatus auf und muss gefiltert werden. Dieses Szenario ist in einem Wranglingdatenfluss nicht möglich. |
| Table.Transpose | Nicht unterstützt |
| Table.Pivot | Nicht unterstützt |

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie Sie [einen Wranglingdatenfluss erstellen](wrangling-data-flow-tutorial.md).