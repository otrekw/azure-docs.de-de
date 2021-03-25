---
title: DateTimeFromParts in der Abfragesprache für Azure Cosmos DB
description: Erfahren Sie mehr über die SQL-Systemfunktion DateTimeFromParts in Azure Cosmos DB.
author: timsander1
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: conceptual
ms.date: 07/09/2020
ms.author: tisande
ms.custom: query-reference
ms.openlocfilehash: b0e7996b71e68db371201da1f0f5c93486ae4e29
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "104592091"
---
# <a name="datetimefromparts-azure-cosmos-db"></a>DateTimeFromParts (Azure Cosmos DB)
[!INCLUDE[appliesto-sql-api](includes/appliesto-sql-api.md)]

Gibt einen aus Eingabewerten erstellten DateTime-Wert als Zeichenfolge zurück.
  
## <a name="syntax"></a>Syntax
  
```sql
DateTimeFromParts(<numberYear>, <numberMonth>, <numberDay> [, numberHour]  [, numberMinute]  [, numberSecond] [, numberOfFractionsOfSecond])
```

## <a name="arguments"></a>Argumente
  
*numberYear* Ganzzahliger Wert für das Jahr im Format `YYYY`

*numberMonth*  
   Ganzzahliger Wert für den Monat im Format `MM`

*numberDay*  
   Ganzzahliger Wert für den Tag im Format `DD`

*numberHour* (optional) Ganzzahliger Wert für die Stunde im Format `hh`

*numberMinute* (optional) Ganzzahliger Wert für die Minute im Format `mm`

*numberSecond* (optional) Ganzzahliger Wert für die Sekunde im Format `ss`

*numberOfFractionsOfSecond* (optional) Ganzzahliger Wert für einen Sekundenbruchteil im Format `.fffffff`

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
|Z|UTC-Kennzeichner|
  
 Weitere Informationen zum ISO 8601-Format finden Sie unter [ISO_8601](https://en.wikipedia.org/wiki/ISO_8601).

## <a name="remarks"></a>Bemerkungen

Wenn die angegebenen ganzen Zahlen einen ungültigen DateTime-Wert ergeben, gibt DateTimeFromParts `undefined` zurück.

Wenn ein optionales Argument nicht angegeben wird, lautet sein Wert 0.

## <a name="examples"></a>Beispiele

Im folgenden Beispiel sind nur erforderliche Argumente zum Erstellen eines DateTime-Werts enthalten:

```sql
SELECT DateTimeFromParts(2020, 9, 4) AS DateTime
```

```json
[
    {
        "DateTime": "2020-09-04T00:00:00.0000000Z"
    }
]
```

Im folgenden Beispiel werden auch einige optionale Argumente zum Erstellen eines DateTime-Werts verwendet:

```sql
SELECT DateTimeFromParts(2020, 9, 4, 10, 52) AS DateTime
```

```json
[
    {
        "DateTime": "2020-09-04T10:52:00.0000000Z"
    }
]
```

Im folgenden Beispiel werden alle optionalen Argumente zum Erstellen eines DateTime-Werts verwendet:

```sql
SELECT DateTimeFromParts(2020, 9, 4, 10, 52, 12, 3456789) AS DateTime
```

```json
[
    {
        "DateTime": "2020-09-04T10:52:12.3456789Z"
    }
]
```

## <a name="next-steps"></a>Nächste Schritte

- [Datums- und Uhrzeitfunktionen in Azure Cosmos DB](sql-query-date-time-functions.md)
- [Systemfunktionen in Azure Cosmos DB](sql-query-system-functions.md)
- [Einführung in Azure Cosmos DB](introduction.md)
