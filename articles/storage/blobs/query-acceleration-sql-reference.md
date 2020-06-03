---
title: Abfragebeschleunigung – SQL-Sprachreferenz (Vorschau)
titleSuffix: Azure Storage
description: Erfahren Sie, wie Sie die SQL-Syntax für die Abfragebeschleunigung verwenden.
services: storage
author: normesta
ms.service: storage
ms.topic: conceptual
ms.date: 04/21/2020
ms.author: normesta
ms.subservice: data-lake-storage-gen2
ms.reviewer: ereilebr
ms.openlocfilehash: 3408970bcf5e34ce9f0f0afe9e723b4877dcd694
ms.sourcegitcommit: 1f48ad3c83467a6ffac4e23093ef288fea592eb5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2020
ms.locfileid: "84193403"
---
# <a name="query-acceleration-sql-language-reference-preview"></a>Abfragebeschleunigung – SQL-Sprachreferenz (Vorschau)

Die Abfragebeschleunigung unterstützt eine ANSI SQL-ähnliche Sprache, um Abfragen für Blobinhalte zu schreiben.  Der SQL-Dialekt für die Abfragebeschleunigung stellt eine Teilmenge von ANSI SQL mit einer begrenzten Anzahl unterstützter Datentypen, Operatoren usw. dar, bietet aber gleichzeitig auch eine Erweiterung von ANSI SQL, um Abfragen für hierarchische teilstrukturierte Datenformate wie JSON zu unterstützen. 

> [!NOTE]
> Die Abfragebeschleunigung ist als öffentliche Vorschau in den Regionen „Kanada, Mitte“ und „Frankreich, Mitte“ verfügbar. Informationen zu den Einschränkungen finden Sie im Artikel [Bekannte Probleme](data-lake-storage-known-issues.md). Wie Sie sich für die Vorschau registrieren, erfahren Sie in [diesem Formular](https://aka.ms/adls/qa-preview-signup). 

## <a name="select-syntax"></a>SELECT-Syntax

Die einzige von der Abfragebeschleunigung unterstützte SQL-Anweisung ist die SELECT-Anweisung. In diesem Beispiel wird jede Zeile zurückgegeben, für die der Ausdruck „true“ zurückgibt.

```sql
SELECT * FROM table [WHERE expression] [LIMIT limit]
```

Bei CSV-formatierten Daten muss *table* `BlobStorage` lauten.  Dies bedeutet, dass die Abfrage für jedes Blob ausgeführt wird, das im REST-Aufruf angegeben wurde.
Bei JSON-formatierten Daten ist *table* ein „Tabellendeskriptor“.   Weitere Informationen finden Sie im Abschnitt [Tabellendeskriptoren](#table-descriptors) in diesem Artikel.

Im folgenden Beispiel gibt die Anweisung für jede Zeile, für die der WHERE-Ausdruck (*expression*) „true“ zurückgibt, eine neue Zeile zurück, die aus der Auswertung der einzelnen Projektionsausdrücke resultiert.


```sql
SELECT expression [, expression …] FROM table [WHERE expression] [LIMIT limit]
```

Im folgenden Beispiel wird eine aggregierte Berechnung (z. B. der Durchschnittswert einer bestimmten Spalte) für jede Zeile zurückgegeben, für die *expression* „true“ zurückgibt. 

```sql
SELECT aggregate_expression FROM table [WHERE expression] [LIMIT limit]
```

Im folgenden Beispiel werden geeignete Offsets zum Aufteilen eines CSV-formatierten Blobs zurückgegeben.  Weitere Informationen finden Sie im Abschnitt [Sys.Split](#sys-split) dieses Artikels.

```sql
SELECT sys.split(split_size)FROM BlobStorage
```

<a id="data-types"></a>

## <a name="data-types"></a>Datentypen

|Datentyp|BESCHREIBUNG|
|---------|-------------------------------------------|
|INT      |Ganze 64-Bit-Zahl mit Vorzeichen:                     |
|GLEITKOMMAZAHL    |64-Bit-Gleitkommazahl (mit doppelter Genauigkeit).|
|STRING   |Unicode-Zeichenfolge variabler Länge.            |
|timestamp|Ein Zeitpunkt.                           |
|BOOLEAN  |„true“ oder „false“.                             |

Beim Lesen von Werten aus CSV-formatierten Daten werden alle Werte als Zeichenfolgen gelesen.  Zeichenfolgenwerte können mithilfe von CAST-Ausdrücken in andere Typen konvertiert werden.  Werte können abhängig vom Kontext implizit in andere Typen umgewandelt werden. Weitere Informationen finden Sie unter [Rangfolge der Datentypen (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/data-types/data-type-precedence-transact-sql?view=sql-server-2017).

## <a name="expressions"></a>Ausdrücke

### <a name="referencing-fields"></a>Verweisen auf Felder

Bei JSON-formatierten oder CSV-formatierten Daten mit einer Kopfzeile kann anhand des Namens auf Felder verwiesen werden.  Feldnamen können mit oder ohne Anführungszeichen angegeben werden. Feldnamen in Anführungszeichen werden in doppelte Anführungszeichen (") eingeschlossen und dürfen Leerzeichen enthalten. Außerdem wird die Groß-/Kleinschreibung beachtet.  Bei Feldnamen ohne Anführungszeichen wird die Groß-/Kleinschreibung nicht beachtet, und Sie dürfen keine Sonderzeichen enthalten.

In CSV-formatierten Daten kann auch durch die Ordnungszahl auf Felder verwiesen werden, der ein Unterstrich (_) vorangestellt ist.  Beispielsweise kann auf das erste Feld mit „_1“ oder auf das elfte Feld mit „_11“ verwiesen werden.  Anhand der Ordnungszahl auf Felder zu verweisen, ist bei CSV-formatierten Daten hilfreich, die keine Kopfzeile enthalten. In diesem Fall bietet die Ordnungszahl die einzige Möglichkeit, auf ein bestimmtes Feld zu verweisen.

### <a name="operators"></a>Operatoren

Folgende SQL-Standardoperatoren werden unterstützt:

``=``, ``!=``, ``<>``, ``<``, ``<=``, ``>``, ``>=``, ``+``, ``-``, ``/``, ``*``, ``%``, ``AND``, ``OR``, ``NOT``, ``CAST``, ``BETWEEN``, ``IN``, ``NULLIF``, ``COALESCE``

Wenn sich die Datentypen auf der linken und rechten Seite eines Operators unterscheiden, wird eine automatische Konvertierung gemäß den hier angegebenen Regeln durchgeführt: [Rangfolge der Datentypen (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/data-types/data-type-precedence-transact-sql?view=sql-server-2017).

Die SQL-Sprache für die Abfragebeschleunigung unterstützt nur eine sehr kleine Teilmenge der Datentypen, die in diesem Artikel erläutert werden.  Weitere Informationen finden Sie im Abschnitt [Datentypen](#data-types) dieses Artikels.

### <a name="casts"></a>Umwandlungen

Die SQL-Sprache für die Abfragebeschleunigung unterstützt den CAST-Operator gemäß den folgenden Regeln: [Datentypkonvertierung (Datenbank-Engine)](https://docs.microsoft.com/sql/t-sql/data-types/data-type-conversion-database-engine?view=sql-server-2017).  

Die SQL-Sprache für die Abfragebeschleunigung unterstützt nur eine kleine Teilmenge der Datentypen, die in diesem Artikel erläutert werden.  Weitere Informationen finden Sie im Abschnitt [Datentypen](#data-types) dieses Artikels.

### <a name="string-functions"></a>Zeichenfolgenfunktionen

Die SQL-Sprache für die Abfragebeschleunigung unterstützt die folgenden standardmäßigen SQL-Zeichenfolgenfunktionen:

``LIKE``, ``CHAR_LENGTH``, ``CHARACTER_LENGTH``, ``LOWER``, ``UPPER``, ``SUBSTRING``, ``TRIM``, ``LEADING``, ``TRAILING``.

Hier einige Beispiele:

|Funktion|Beispiel|Ergebnis|
|---|---|---|
|CHARACTER_LENGTH|``SELECT CHARACTER_LENGTH('abcdefg') from BlobStorage`` |``7``|
|CHAR_LENGTH|``SELECT CHAR_LENGTH(_1) from BlobStorage``|``1``|
|LOWER|``SELECT LOWER('AbCdEfG') from BlobStorage``|``abcdefg``|
|UPPER|``SELECT UPPER('AbCdEfG') from BlobStorage``|``ABCDEFG``|
|SUBSTRING|``SUBSTRING('123456789', 1, 5)``|``23456``|
|TRIM|``TRIM(BOTH '123' FROM '1112211Microsoft22211122')``|``Microsoft``|

Mit der [LIKE](https://docs.microsoft.com/sql/t-sql/language-elements/like-transact-sql?view=sql-server-ver15)-Funktion können Sie nach einem Muster suchen. Hier einige Beispiele, in denen die [LIKE](https://docs.microsoft.com/sql/t-sql/language-elements/like-transact-sql?view=sql-server-ver15)-Funktion verwendet wird, um die Datenzeichenfolge ``abc,abd,cd\ntest,test2,test3\na_bc,xc%d^e,gh[i `` zu durchsuchen.

|Abfrage|Beispiel|
|--|--|
|``SELECT _1, _2, _3 from BlobStorage where _2 LIKE 'a%'``|``abc,abd,cd\n``|
|``SELECT * from BlobStorage where _1 LIKE 'a[bcd]c``|``abc,abd,cd\n``|
|``SELECT _1 from BlobStorage where _2 LIKE '[^xyz]%'``|``abc\ntest\n``|
|``SELECT * from BlobStorage where _1 LIKE 'a_``|``abc,abd,cd\n``|
|``SELECT _2,_3 from BlobStorage where _3 LIKE '[g-h]_![[a-j]' Escape '!'``|``xc%d^e,gh[i\n``|

### <a name="date-functions"></a>Datumsfunktionen

Die folgenden standardmäßigen SQL-Datumsfunktionen werden unterstützt:

``DATE_ADD``, ``DATE_DIFF``, ``EXTRACT``, ``TO_STRING``, ``TO_TIMESTAMP``.

Derzeit werden alle [Datumsformate gemäß IS0-8601-Standard](https://www.w3.org/TR/NOTE-datetime) konvertiert. 

#### <a name="date_add-function"></a>DATE_ADD-Funktion

In der SQL-Sprache für die Abfragebeschleunigung können für die ``DATE_ADD``-Funktion „year“, „month“, „day“, „hour“, „minute“ und „second“ angegeben werden.

Beispiele:

```sql
DATE_ADD(datepart, quantity, timestamp)
DATE_ADD('minute', 1, CAST('2017-01-02T03:04:05.006Z' AS TIMESTAMP)
```

#### <a name="date_diff-function"></a>DATE_DIFF-Funktion

In der SQL-Sprache für die Abfragebeschleunigung können für die ``DATE_DIFF``-Funktion „year“, „month“, „day“, „hour“, „minute“ und „second“ angegeben werden.

```sql
DATE_DIFF(datepart, timestamp, timestamp)
DATE_DIFF('hour','2018-11-09T00:00+05:30','2018-11-09T01:00:23-08:00') 
```

#### <a name="extract-function"></a>EXTRACT-Funktion

Bei EXTRACT unterstützt die SQL-Sprache für die Abfragebeschleunigung im Gegensatz zum Datumsteil für die ``DATE_ADD``-Funktion „timezone_hour“ und „timezone_minute“ als Datumsteil.

Beispiele:

```sql
EXTRACT(datepart FROM timestampstring)
EXTRACT(YEAR FROM '2010-01-01T')
```

#### <a name="to_string-function"></a>TO_STRING-Funktion

Beispiele:

```sql
TO_STRING(TimeStamp , format)
TO_STRING(CAST('1969-07-20T20:18Z' AS TIMESTAMP),  'MMMM d, y')
```

In dieser Tabelle werden Zeichenfolgen beschrieben, mit denen Sie das Ausgabeformat der ``TO_STRING``-Funktion angeben können.

|Formatzeichenfolge    |Output                               |
|-----------------|-------------------------------------|
|yy               |Jahr im zweistelligen Format – 1999 als 99|
|y                |Jahr im vierstelligen Format               |
|yyyy             |Jahr im vierstelligen Format               |
|M                |Monat des Jahres – 1                    |
|MM               |Monat mit Nullen aufgefüllt – 01               |
|MMM              |Abkürzung für den Monat des Jahres – JAN            |
|MMMM             |Vollständiger Monatsname – Mai                      |
|d                |Tag des Monats (1–31)                  |
|dd               |Tag des Monats mit Nullen aufgefüllt (01–31)     |
|a                |AM oder PM                             |
|h.                |Stunde des Tages (1–12)                   |
|hh               |Stunden des Tages mit Nullen aufgefüllt (01–12)     |
|H                |Stunde des Tages (0–23)                   |
|HH               |Stunde des Tages mit Nullen aufgefüllt (00–23)      |
|m                |Minute der Stunde (0–59)                |
|MM               |Minute mit Nullen aufgefüllt (00–59)           |
|s                |Sekunde der Minuten (0–59)             |
|ss               |Sekunden mit Nullen aufgefüllt (00–59)          |
|E                |Sekundenbruchteil (0,1–0,9)        |
|SS               |Sekundenbruchteil (0,01–0,99)      |
|SSS              |Sekundenbruchteil (0,001–0,999)    |
|X                |Abweichung in Stunden                      |
|XX oder XXXX       |Abweichung in Stunden und Minuten (+0430)  |
|XXX oder XXXXX     |Abweichung in Stunden und Minuten (-07:00) |
|x                |Abweichung in Stunden (7)                  |
|xx oder xxxx       |Abweichung in Stunden und Minuten (+0530)    |
|Xxx oder xxxxx     |Abweichung in Stunden und Minuten (+05:30)   |

#### <a name="to_timestamp-function"></a>TO_TIMESTAMP-Funktion

Es werden nur IS0-8601-Formate unterstützt.

Beispiele:

```sql
TO_TIMESTAMP(string)
TO_TIMESTAMP('2007T')
```

> [!NOTE]
> Sie können auch die ``UTCNOW``-Funktion verwenden, um die Systemzeit abzurufen.


## <a name="aggregate-expressions"></a>Aggregatausdrücke

Eine SELECT-Anweisung kann entweder einen oder mehrere Projektionsausdrücke oder einen einzelnen Aggregatausdruck enthalten.  Die folgenden Aggregatausdrücke werden unterstützt:

|Ausdruck|BESCHREIBUNG|
|--|--|
|[COUNT(\*)](https://docs.microsoft.com/sql/t-sql/functions/count-transact-sql?view=sql-server-ver15)    |Gibt die Anzahl der Datensätze zurück, die mit dem Prädikatausdruck übereinstimmen.|
|[COUNT(Ausdruck)](https://docs.microsoft.com/sql/t-sql/functions/count-transact-sql?view=sql-server-ver15)    |Gibt die Anzahl der Datensätze zurück, für die der Ausdruck ungleich NULL ist.|
|[AVERAGE(Ausdruck)](https://docs.microsoft.com/sql/t-sql/functions/avg-transact-sql?view=sql-server-ver15)    |Gibt den Durchschnitt der Werte des Ausdrucks zurück, die ungleich NULL sind.|
|[MIN(Ausdruck)](https://docs.microsoft.com/sql/t-sql/functions/min-transact-sql?view=sql-server-ver15)    |Gibt den kleinsten Wert des Ausdrucks zurück, der ungleich NULL ist.|
|[MAX(Ausdruck)](https://docs.microsoft.com/sql/t-sql/functions/max-transact-sql?view=sql-server-ver15)    |Gibt den größten Wert des Ausdrucks zurück, der ungleich NULL ist.|
|[SUM(Ausdruck)](https://docs.microsoft.com/sql/t-sql/functions/sum-transact-sql?view=sql-server-ver15)    |Gibt die Summe aller Werte des Ausdrucks zurück, die ungleich NULL sind.|

### <a name="missing"></a>MISSING

``IS MISSING`` ist der einzige nicht standardmäßige Operator, der von der SQL-Sprache für die Abfragebeschleunigung unterstützt wird.  Wenn bei JSON-Daten in einem bestimmten Eingabedatensatz ein Feld fehlt, ergibt das Ausdrucksfeld ``IS MISSING`` den booleschen Wert „true“.

<a id="table-descriptors"></a>

## <a name="table-descriptors"></a>Tabellendeskriptoren

Bei CSV-Daten lautet der Tabellenname immer `BlobStorage`.  Beispiel:

```sql
SELECT * FROM BlobStorage
```

Für JSON-Daten stehen zusätzliche Optionen zur Verfügung:

```sql
SELECT * FROM BlobStorage[*].path
```

Dadurch können Abfragen für Teilmengen von JSON-Daten ausgeführt werden.

Bei JSON-Abfragen können Sie den Pfad in einem Teil der FROM-Klausel angeben. Diese Pfade helfen, die Teilmenge von JSON-Daten zu analysieren. Die Pfade können auf JSON-Array- und -Objektwerte verweisen.

Wir erläutern dies ausführlicher anhand eines Beispiels.

Hier unsere Beispieldaten:

```json
{
  "id": 1,
  "name": "mouse",
  "price": 12.5,
  "tags": [
    "wireless",
    "accessory"
  ],
  "dimensions": {
    "length": 3,
    "width": 2,
    "height": 2
  },
  "weight": 0.2,
  "warehouses": [
    {
      "latitude": 41.8,
      "longitude": -87.6
    }
  ]
}
```

Möglicherweise interessieren Sie sich nur für das JSON-Objekt `warehouses` in den oben aufgeführten Daten. Das Objekt `warehouses` ist ein JSON-Arraytyp. Sie können es in der FROM-Klausel angeben. Die Beispielabfrage sieht in etwa wie folgt aus.

```sql
SELECT latitude FROM BlobStorage[*].warehouses[*]
```

Mit der Abfrage werden alle Felder abgerufen, jedoch nur „latitude“ ausgewählt.

Wenn Sie nur auf den JSON-Objektwert `dimensions` zugreifen möchten, können Sie in der Abfrage auf dieses Objekt verweisen. Beispiel:

```sql
SELECT length FROM BlobStorage[*].dimensions
```

Dadurch wird auch der Zugriff auf Elemente des Objekts `dimensions` eingeschränkt. Wenn Sie auf andere Elemente von JSON-Feldern und auf innere Werte von JSON-Objekten zugreifen möchten, können Sie eine Abfrage wie im folgenden Beispiel verwenden:

```sql
SELECT weight,warehouses[0].longitude,id,tags[1] FROM BlobStorage[*]
```

> [!NOTE]
> BlobStorage und BlobStorage[\*] verweisen beide auf das gesamte Objekt. Wenn Sie jedoch einen Pfad in der FROM-Klausel angegeben haben, müssen Sie BlobStorage[\*].path verwenden.

<a id="sys-split"></a>

## <a name="syssplit"></a>Sys.Split

Dies ist eine besondere Form der SELECT-Anweisung, die nur für CSV-formatierte Daten verfügbar ist.

```sql
SELECT sys.split(split_size)FROM BlobStorage
```

Verwenden Sie diese Anweisung, wenn Sie CSV-Datensätze in Batches herunterladen und verarbeiten möchten. Auf diese Weise können Datensätze parallel verarbeitet werden, anstatt alle Datensätze einzeln herunterladen zu müssen. Diese Anweisung gibt keine Datensätze aus der CSV-Datei zurück. Stattdessen wird eine Auflistung von Batchgrößen zurückgegeben. Anschließend können Sie jede Batchgröße verwenden, um einen Batch von Datensätzen abzurufen. 

Verwenden Sie den *split_size*-Parameter, um die Anzahl von Bytes anzugeben, die jeder Batch enthalten soll. Wenn Sie z. B. jeweils nur 10 MB an Daten verarbeiten möchten, sieht die Anweisung wie folgt aus: `SELECT sys.split(10485760)FROM BlobStorage`. Dabei entsprechen 10 MB 10.485.760 Bytes. Jeder Batch enthält so viele Datensätze, wie in diese 10 MB passen. 

In den meisten Fällen ist die Größe jedes Batches etwas höher als der von Ihnen angegebene Wert. Dies liegt daran, dass ein Batch keinen Teildatensatz enthalten darf. Wenn der letzte Datensatz in einem Batch vor dem Ende Ihres Schwellenwerts beginnt, wird der Batch vergrößert, so dass er den vollständigen Datensatz aufnehmen kann. Die Größe des letzten Batches wird wahrscheinlich etwas unter der von Ihnen angegebenen Größe liegen.

>[!NOTE]
> Die Größe für „split_size“ muss mindestens 10 MB (10485760) betragen.

## <a name="see-also"></a>Weitere Informationen

- [Abfragebeschleunigung für Azure Data Lake Storage (Vorschau)](data-lake-storage-query-acceleration.md)
- [Filtern von Daten mithilfe der Abfragebeschleunigung für Azure Data Lake Storage (Vorschau)](data-lake-storage-query-acceleration-how-to.md)

