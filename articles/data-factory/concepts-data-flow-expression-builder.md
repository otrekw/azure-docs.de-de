---
title: Ausdrucks-Generator im Zuordnungsdatenfluss
description: Erstellen von Ausdrücken mithilfe des Ausdrucks-Generators in Zuordnungsdatenflüssen in Azure Data Factory
author: kromerm
ms.author: makromer
ms.reviewer: daperlov
ms.service: data-factory
ms.topic: conceptual
ms.date: 06/20/2020
ms.openlocfilehash: 7e2b655b344af90c4555beb0af85fa11cbc6d1c8
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85126160"
---
# <a name="build-expressions-in-mapping-data-flow"></a>Erstellen von Ausdrücken im Zuordnungsdatenfluss

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Im Zuordnungsdatenfluss werden viele Transformationseigenschaften als Ausdrücke eingegeben. Diese Ausdrücke bestehen aus Spaltenwerten, Parametern, Funktionen, Operatoren und Literalen, die zur Laufzeit zu einem Spark-Datentyp ausgewertet werden.

> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE4tkur]

## <a name="open-expression-builder"></a>Öffnen des Ausdrucks-Generators

Die Schnittstelle zur Ausdrucksbearbeitung in der Azure Data Factory-Benutzeroberfläche wird als Ausdrucks-Generator bezeichnet. Während Sie Ihre Ausdruckslogik eingeben, verwendet Data Factory die [IntelliSense](https://docs.microsoft.com/visualstudio/ide/using-intellisense?view=vs-2019)-Codevervollständigung zum Hervorheben, für die Syntaxprüfung und zur automatischen Vervollständigung.

![Ausdrucks-Generator](media/data-flow/xpb1.png "Ausdrucks-Generator")

Öffnen Sie in Transformationen wie der abgeleiteten Spalte und dem abgeleiteten Filter, bei denen Ausdrücke obligatorisch sind, den Ausdrucks-Generator, indem Sie das blaue Ausdrucksfeld auswählen.

![Blaues Ausdrucksfeld](media/data-flow/expressionbox.png "Ausdrucks-Generator")

Wenn Sie in einer Übereinstimmungs- oder Gruppieren nach-Bedingung auf Spalten verweisen, kann ein Ausdruck Werte aus Spalten extrahieren. Wählen Sie zum Erstellen eines Ausdrucks die Option **Berechnete Spalte** aus.

![Option „Berechnete Spalte“](media/data-flow/computedcolumn.png "Ausdrucks-Generator")

In Fällen, in denen ein Ausdruck oder ein Literalwert gültige Eingaben sind, können Sie mit **Dynamischen Inhalt hinzufügen** einen Ausdruck erstellen, der zu einem Literalwert ausgewertet wird.

![Option „Dynamischen Inhalt hinzufügen“](media/data-flow/add-dynamic-content.png "Ausdrucks-Generator")

## <a name="expression-language-reference"></a>Ausdruckssprache – Referenz

Zuordnungsdatenflüsse verfügen über integrierte Funktionen und Operatoren, die in Ausdrücken verwendet werden können. Eine Liste der verfügbaren Funktionen finden Sie unter [Ausdrucksfunktionen im Zuordnungsdatenfluss](data-flow-expression-functions.md).

## <a name="column-names-with-special-characters"></a>Spaltennamen mit Sonderzeichen

Wenn Sie Spaltennamen mit Sonder- oder Leerzeichen haben, setzen Sie den Namen in geschweifte Klammern, um darauf in einem Ausdruck zu verweisen.

```{[dbo].this_is my complex name$$$}```

## <a name="preview-expression-results"></a>Vorschau von Ausdrucksergebnissen

Wenn [debug mode](concepts-data-flow-debug-mode.md) aktiviert ist, können Sie mithilfe des Live-Spark-Clusters während der Bearbeitung eine Vorschau der Elemente anzeigen, zu denen Ihr Ausdruck ausgewertet wird. Während Sie Ihre Logik erstellen, können Sie den Ausdruck in Echtzeit debuggen. 

![Vorschau während der Bearbeitung](media/data-flow/exp4b.png "Ausdrucksdatenvorschau")

Klicken Sie auf **Aktualisieren**, um die Ergebnisse Ihres Ausdrucks anhand eines Livebeispiels Ihrer Quelle zu aktualisieren.

![Schaltfläche „Aktualisieren“](media/data-flow/exp5.png "Ausdrucksdatenvorschau")

## <a name="string-interpolation"></a>Zeichenfolgeninterpolation

Verwenden Sie Anführungszeichen, um Literalzeichenfolgen zusammen mit Ausdrücken einzubeziehen. Sie können Ausdrucksfunktionen, Spalten und Parameter einbeziehen. Die Zeichenfolgeninterpolation ist hilfreich zum Vermeiden von übermäßiger Zeichenfolgenverkettung, wenn Parameter in Abfragezeichenfolgen einbezogen werden. Wenn Sie eine Ausdruckssyntax verwenden möchten, schließen Sie sie in geschweifte Klammern ein.

Einige Beispiele für Zeichenfolgeninterpolation:

* ```"My favorite movie is {iif(instr(title,', The')>0,"The {split(title,', The')[1]}",title)}"```

* ```"select * from {$tablename} where orderyear > {$year}"```

* ```"Total cost with sales tax is {round(totalcost * 1.08,2)}"```

## <a name="comment-expressions"></a>Kommentarausdrücke

Fügen Sie Ihren Ausdrücken Kommentare hinzu. Verwenden Sie dabei eine einzeilige oder eine mehrzeilige Kommentarsyntax.

Die folgenden Beispiele stellen gültige Kommentare dar:

* ```/* This is my comment */```

* ```/* This is a```
*   ```multi-line comment */```

Wenn Sie einen Kommentar am Anfang des Ausdrucks einfügen, wird er im Transformationstextfeld angezeigt und dokumentiert Ihre Transformationsausdrücke.

![Kommentar im Transformationstextfeld](media/data-flow/comments2.png "Kommentare")

## <a name="regular-expressions"></a>Reguläre Ausdrücke

Viele Ausdruckssprachfunktionen verwenden die reguläre Ausdruckssyntax. Wenn Sie Funktionen mit regulären Ausdrücken verwenden, versucht der Ausdrucks-Generator, den umgekehrten Schrägstrich (\\) als Escapezeichensequenz zu interpretieren. Wenn Sie in Ihrem regulären Ausdruck umgekehrte Schrägstriche verwenden, schließen Sie entweder den gesamten regulären Ausdruck in Backticks (\`) ein, oder verwenden Sie einen doppelten umgekehrten Schrägstrich.

Ein Beispiel mit Backticks:

```
regex_replace('100 and 200', `(\d+)`, 'digits')
```

Ein Beispiel mit doppelten umgekehrten Schrägstrichen:

```
regex_replace('100 and 200', '(\\d+)', 'digits')
```

## <a name="address-array-indexes"></a>Angeben von Arrayindizes

Verwenden Sie bei Ausdrucksfunktionen, die Arrays zurückgeben, eckige Klammern ([]), um bestimmte Indizes innerhalb dieser Rückgabearrayobjekte anzugeben. Das Array basiert auf Einsen.

![Ausdrucks-Generator: Array](media/data-flow/expb2.png "Ausdrucksdatenvorschau")

## <a name="keyboard-shortcuts"></a>Tastenkombinationen

* STRG+K, STRG+C: Gesamte Zeile auskommentieren
* STRG+K, STRG+U: Auskommentierung aufheben
* F1: Befehle der Editor-Hilfe anzeigen
* ALT+NACH-UNTEN-TASTE: Aktuelle Zeile nach unten verschieben
* ALT+NACH-OBEN-TASTE: Aktuelle Zeile nach oben verschieben
* STRG+LEERTASTE: Kontexthilfe anzeigen

## <a name="convert-to-dates-or-timestamps"></a>Konvertieren in Datumsangaben oder Zeitstempel

Wenn Sie Zeichenfolgenliterale in Ihre Zeitstempelausgabe einbeziehen möchten, schließen Sie die Konvertierung in ```toString()``` ein.

```toString(toTimestamp('12/31/2016T00:12:00', 'MM/dd/yyyy\'T\'HH:mm:ss'), 'MM/dd /yyyy\'T\'HH:mm:ss')```

Wenn Sie Millisekunden von einer Epoche in ein Datum oder einen Zeitstempel konvertieren möchten, verwenden Sie `toTimestamp(<number of milliseconds>)`. Wenn die Zeit in Sekunden angezeigt wird, multiplizieren Sie den Wert mit 1.000.

```toTimestamp(1574127407*1000l)```

Das nachgestellte „l“ am Ende des vorstehenden Ausdrucks gibt eine Konvertierung in einen long-Datentyp als inline-Syntax an.

## <a name="find-time-from-epoch-or-unix-time"></a>Zeit aus Epochen- oder Unix-Zeit finden

toLong( currentTimestamp() - toTimestamp('1970-01-01 00:00:00.000', 'yyyy-MM-dd HH:mm:ss.SSS') ) * 1000l

## <a name="next-steps"></a>Nächste Schritte

[Mit dem Erstellen von Datentransformationsausdrücken beginnen](data-flow-expression-functions.md)
