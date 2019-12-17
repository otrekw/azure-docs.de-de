---
title: Ausdrucks-Generator im Zuordnungsdatenfluss
description: Erstellen von Ausdrücken mithilfe des Ausdrucks-Generators in Zuordnungsdatenflüssen in Azure Data Factory
author: kromerm
ms.author: makromer
ms.reviewer: daperlov
ms.service: data-factory
ms.topic: conceptual
ms.date: 12/9/2019
ms.openlocfilehash: 01aa2574ac6edd1ce5e1b209eac3e43bbed82fce
ms.sourcegitcommit: 5ab4f7a81d04a58f235071240718dfae3f1b370b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/10/2019
ms.locfileid: "74969355"
---
# <a name="building-expressions-in-mapping-data-flow"></a>Erstellen von Ausdrücken im Zuordnungsdatenfluss

Im Zuordnungsdatenfluss werden viele Transformationseigenschaften als Ausdrücke eingegeben. Diese Ausdrücke bestehen aus Spaltenwerten, Parametern, Funktionen, Operatoren und Literalen, die zur Laufzeit zu einem Spark-Datentyp ausgewertet werden.

## <a name="opening-the-expression-builder"></a>Öffnen des Ausdrucks-Generators

Die Schnittstelle zur Ausdrucksbearbeitung in der Data Factory-Benutzeroberfläche wird als **Ausdrucks-Generator** bezeichnet. Während Sie Ihre Ausdruckslogik eingeben, verwendet Data Factory die [IntelliSense](https://docs.microsoft.com/visualstudio/ide/using-intellisense?view=vs-2019)-Codevervollständigung zum Hervorheben, für die Syntaxprüfung und zur automatischen Vervollständigung.

![Ausdrucks-Generator](media/data-flow/xpb1.png "Ausdrucks-Generator")

Öffnen Sie in Transformationen wie der abgeleiteten Spalte und dem abgeleiteten Filter, bei denen Ausdrücke obligatorisch sind, den Ausdrucks-Generator durch Klicken auf das blaue Ausdrucksfeld.

![Ausdrucks-Generator](media/data-flow/expressionbox.png "Ausdrucks-Generator")

Wenn auf Spalten in einer übereinstimmenden oder „Gruppieren nach“-Bedingung verwiesen wird, kann ein Ausdruck Werte aus Spalten extrahieren. Wählen Sie zum Erstellen eines Ausdrucks die Option „berechnete Spalte“ aus.

![Ausdrucks-Generator](media/data-flow/computedcolumn.png "Ausdrucks-Generator")

In Fällen, in denen ein Ausdruck oder ein Literalwert gültige Eingaben sind, können Sie mit „Dynamischen Inhalt hinzufügen“ einen Ausdruck erstellen, der zu einem Literal ausgewertet wird.

![Ausdrucks-Generator](media/data-flow/add-dynamic-content.png "Ausdrucks-Generator")

## <a name="expression-language-reference"></a>Ausdruckssprache – Referenz

Bei Zuordnungsdatenflüssen gibt es integrierte Funktionen und Operatoren, die in Ausdrücken verwendet werden können. Eine Liste der verfügbaren Funktionen finden Sie auf der Referenzseite [Mapping Data Flow Expression Language ](data-flow-expression-functions.md) (Ausdruckssprache für Zuordnungsdatenflüsse).

## <a name="column-names-with-special-characters"></a>Spaltennamen mit Sonderzeichen

Wenn Sie Spaltennamen mit Sonder- oder Leerzeichen haben, setzen Sie den Namen in geschweifte Klammern, um darauf in einem Ausdruck zu verweisen.

```{[dbo].this_is my complex name$$$}```

## <a name="previewing-expression-results"></a>Anzeigen einer Vorschau von Ausdrucksergebnissen

Wenn [debug-mode](concepts-data-flow-debug-mode.md) aktiviert ist, können Sie mithilfe des Live-Spark-Clusters in einer Vorschau zum Fortschritt der Bearbeitung anzeigen, zu was Ihr Ausdruck ausgewertet wird. Während Sie Ihre Logik erstellen, können Sie Ihren Ausdruck in Echtzeit debuggen. 

![Ausdrucks-Generator](media/data-flow/exp4b.png "Ausdrucksdatenvorschau")

Klicken Sie auf die Schaltfläche „Aktualisieren“, um die Ergebnisse Ihres Ausdrucks anhand eines Livebeispiels Ihrer Quelle zu aktualisieren.

![Ausdrucks-Generator](media/data-flow/exp5.png "Ausdrucksdatenvorschau")

## <a name="string-interpolation"></a>Zeichenfolgeninterpolation

Verwenden Sie doppelte Anführungszeichen, um literalen Zeichenfolgentext zusammen mit Ausdrücken einzuschließen. Sie können Ausdrucksfunktionen, Spalten und Parameter einbeziehen. Die Zeichenfolgeninterpolation ist nützlich zur Vermeidung eines intensiven Einsatzes von Zeichenfolgenverkettung, wenn Parameter in Abfragezeichenfolgen einbezogen werden. Wenn Sie eine Ausdruckssyntax verwenden möchten, schließen Sie sie in geschweifte Klammern ein.

Einige Beispiele für Zeichenfolgeninterpolation:

* ```"My favorite movie is {iif(instr(title,', The')>0,"The {split(title,', The')[1]}",title)}"```

* ```"select * from {$tablename} where orderyear > {$year}"```

* ```"Total cost with sales tax is {round(totalcost * 1.08,2)}"```

## <a name="commenting-expressions"></a>Kommentieren von Ausdrücken

Fügen Sie Ihren Ausdrücken unter Verwendung der einzeiligen und mehrzeiligen Kommentarsyntax Kommentare hinzu:

![Kommentare](media/data-flow/comments.png "Kommentare")

Nachstehend finden Sie Beispiele für gültige Kommentare:

* ```/* This is my comment */```

* ```/* This is a```
*   ```multi-line comment */```
   
* ```// This is a single line comment```

Wenn Sie einen Kommentar am Anfang des Ausdrucks einfügen, wird er im Transformationstextfeld angezeigt, sodass die Transformationsausdrücke dokumentiert werden:

![Kommentare](media/data-flow/comments2.png "Kommentare")

## <a name="regular-expressions"></a>Reguläre Ausdrücke

Viele Ausdruckssprachfunktionen verwenden die reguläre Ausdruckssyntax. Wenn Sie Funktionen mit regulären Ausdrücken verwenden, versucht der Ausdrucks-Generator, den umgekehrten Schrägstrich (\\) als Escapezeichensequenz zu interpretieren. Wenn Sie in Ihrem regulären Ausdruck umgekehrte Schrägstriche verwenden, schließen Sie entweder den gesamten regulären Ausdruck (RegEx) in Ticks (\`) ein, oder verwenden Sie einen doppelten umgekehrten Schrägstrich.

Beispiel für die Verwendung von Ticks

```
regex_replace('100 and 200', `(\d+)`, 'digits')
```

Beispiel für die Verwendung eines doppelten umgekehrten Schrägstrichs

```
regex_replace('100 and 200', '(\\d+)', 'digits')
```

## <a name="addressing-array-indexes"></a>Angeben von Arrayindizes

Verwenden Sie bei Ausdrucksfunktionen, die Arrays zurückgeben, eckige Klammern [], um bestimmte Indizes innerhalb dieses Rückgabearrayobjekts anzugeben. Das Array ist einzelbasiert.

![Ausdrucks-Generator: Array](media/data-flow/expb2.png "Ausdrucksdatenvorschau")

## <a name="keyboard-shortcuts"></a>Tastenkombinationen

* ```Ctrl-K Ctrl-C```: Gesamte Zeile auskommentieren
* ```Ctrl-K Ctrl-U```: Auskommentierung aufheben
* ```F1```: Befehle für Editor-Hilfe anzeigen
* ```Alt-Down Arrow```: Aktuelle Zeile nach unten verschieben
* ```Alt-Up Arrow```: Aktuelle Zeile nach oben verschieben
* ```Cntrl-Space```: Kontexthilfe anzeigen

## <a name="convert-to-dates-or-timestamps"></a>Konvertieren in Datumsangaben oder Zeitstempel

Wenn Sie Zeichenfolgenliterale in Ihre Zeitstempelausgabe einbeziehen möchten, müssen Sie Ihre Konvertierung in ```toString()``` einschließen.

```toString(toTimestamp('12/31/2016T00:12:00', 'MM/dd/yyyy\'T\'HH:mm:ss'), 'MM/dd /yyyy\'T\'HH:mm:ss')```

Wenn Sie Millisekunden von einer Epoche in ein Datum oder einen Zeitstempel konvertieren möchten, verwenden Sie `toTimestamp(<number of milliseconds>)`. Wenn die Zeit in Sekunden angezeigt wird, multiplizieren Sie sie mit 1.000.

```toTimestamp(1574127407*1000l)```

Das nachgestellte „l“ am Ende des vorstehenden Ausdrucks gibt eine Konvertierung in einen „Long“-Datentyp als Inline-Syntax an.

## <a name="next-steps"></a>Nächste Schritte

[Mit dem Erstellen von Datentransformationsausdrücken beginnen](data-flow-expression-functions.md)
