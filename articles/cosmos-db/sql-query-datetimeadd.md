---
title: DateTimeAdd in der Abfragesprache für Azure Cosmos DB
description: Erfahren Sie mehr über die SQL-Systemfunktion DateTimeAdd in Azure Cosmos DB.
author: timsander1
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 07/09/2020
ms.author: tisande
ms.custom: query-reference
ms.openlocfilehash: 0b2741a3d2b013ba7bd97038eb4ba4512f36af11
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "86261505"
---
# <a name="datetimeadd-azure-cosmos-db"></a>DateTimeAdd (Azure Cosmos DB)

Gibt einen DateTime-Zeichenfolgenwert zurück, der sich aus dem Hinzufügen eines angegebenen Zahlenwerts (als ganze Zahl mit Vorzeichen) zu einer angegebenen DateTime-Zeichenfolge ergibt  
  
## <a name="syntax"></a>Syntax
  
```sql
DateTimeAdd (<DateTimePart> , <numeric_expr> ,<DateTime>)
```

## <a name="arguments"></a>Argumente
  
*DateTimePart*  
   Hierbei handelt es sich um den Teil des Datums, zu dem DateTimeAdd eine ganze Zahl hinzufügt. In der folgenden Tabelle sind alle gültigen DateTimePart-Argumente aufgeführt:

| DateTimePart | Abkürzungen        |
| ------------ | -------------------- |
| Jahr         | "year", "yyyy", "yy" |
| Month (Monat)        | "month", "mm", "m"   |
| Day (Tag)          | "day", "dd", "d"     |
| Hour         | "hour", "hh"         |
| Minute       | "minute", "mi", "n"  |
| Sekunde       | "second", "ss", "s"  |
| Millisekunde  | "millisecond", "ms"  |
| Mikrosekunde  | "microsecond", "mcs" |
| Nanosekunde   | "nanosecond", "ns"   |

*numeric_expr*  
   Ein ganzzahliger Wert mit Vorzeichen, der zum DateTimePart-Wert des angegebenen DateTime-Werts hinzugefügt wird

*DateTime*  
   UTC-Datum und -Uhrzeit als ISO-8601-Zeichenfolgenwert im Format `YYYY-MM-DDThh:mm:ss.fffffffZ` mit der folgenden Bedeutung:
  
  |Format|BESCHREIBUNG|
  |-|-|
  |YYYY|vierstellige Jahreszahl|
  |MM|zweistellige Monatszahl (01 = Januar usw.)|
  |DD|zweistellige Zahl für den Tag des Monats (01 bis 31)|
  |T|Trennzeichen, das den Anfang der Uhrzeitelemente markiert|
  |hh|zweistellige Stundenzahl (00 bis 23)|
  |MM|zweistellige Minutenzahl (00 bis 59)|
  |ss|zweistellige Sekundenzahl (00 bis 59)|
  |.fffffff|siebenstellige Sekundenbruchteile|
  |Z|UTC-Kennzeichner||
  
  Weitere Informationen zum ISO 8601-Format finden Sie unter [ISO_8601](https://en.wikipedia.org/wiki/ISO_8601).

## <a name="return-types"></a>Rückgabetypen

Gibt UTC-Datum und -Uhrzeit als ISO-8601-Zeichenfolgenwert im Format `YYYY-MM-DDThh:mm:ss.fffffffZ` mit der folgenden Bedeutung zurück:
  
  |Format|BESCHREIBUNG|
  |-|-|
  |YYYY|vierstellige Jahreszahl|
  |MM|zweistellige Monatszahl (01 = Januar usw.)|
  |DD|zweistellige Zahl für den Tag des Monats (01 bis 31)|
  |T|Trennzeichen, das den Anfang der Uhrzeitelemente markiert|
  |hh|zweistellige Stundenzahl (00 bis 23)|
  |MM|zweistellige Minutenzahl (00 bis 59)|
  |ss|zweistellige Sekundenzahl (00 bis 59)|
  |.fffffff|siebenstellige Sekundenbruchteile|
  |Z|UTC-Kennzeichner||

## <a name="remarks"></a>Bemerkungen

DateTimeAdd gibt aus den folgenden Gründen `undefined` zurück:

- Der angegebene Wert für DateTimePart ist ungültig.
- Der angegebene Wert für numeric_expr ist keine gültige ganze Zahl.
- Der DateTime-Wert im Argument oder Ergebnis ist kein gültiger ISO-8601-DateTime-Wert.

## <a name="examples"></a>Beispiele
  
Im folgenden Beispiel wird ein Monat zum DateTime-Wert `2020-07-09T23:20:13.4575530Z` hinzugefügt.

```sql
SELECT DateTimeAdd("mm", 1, "2020-07-09T23:20:13.4575530Z") AS OneMonthLater
```

```json
[
    {
        "OneMonthLater": "2020-08-09T23:20:13.4575530Z"
    }
]
```  

Im folgenden Beispiel werden zwei Stunden vom DateTime-Wert `2020-07-09T23:20:13.4575530Z` abgezogen:

```sql
SELECT DateTimeAdd("hh", -2, "2020-07-09T23:20:13.4575530Z") AS TwoHoursEarlier
```

```json
[
    {
        "TwoHoursEarlier": "2020-07-09T21:20:13.4575530Z"
    }
]
```  

## <a name="next-steps"></a>Nächste Schritte

- [Datums- und Uhrzeitfunktionen in Azure Cosmos DB](sql-query-date-time-functions.md)
- [Systemfunktionen in Azure Cosmos DB](sql-query-system-functions.md)
- [Einführung in Azure Cosmos DB](introduction.md)
