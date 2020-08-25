---
title: DateTimePart in der Azure Cosmos DB-Abfragesprache
description: Erfahren Sie mehr über die SQL-Systemfunktion DateTimePart in Azure Cosmos DB.
author: timsander1
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 08/14/2020
ms.author: tisande
ms.custom: query-reference
ms.openlocfilehash: 87663c18ddaa5da6740a0f54aa5f2812cbb06af8
ms.sourcegitcommit: 152c522bb5ad64e5c020b466b239cdac040b9377
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/14/2020
ms.locfileid: "88226906"
---
# <a name="datetimepart-azure-cosmos-db"></a>DateTimePart (Azure Cosmos DB)

Gibt den Wert des angegebenen DateTimePart zwischen der angegebenen DateTime zurück.
  
## <a name="syntax"></a>Syntax
  
```sql
DateTimePart (<DateTimePart> , <DateTime>)
```

## <a name="arguments"></a>Argumente
  
*DateTimePart*  
   Der Teil der Datumsangabe, für den DateTimePart den Wert zurückgibt. In der folgenden Tabelle sind alle gültigen DateTimePart-Argumente aufgeführt:

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

*DateTime*  
   UTC-Datum und -Uhrzeit als Zeichenfolgenwert gemäß ISO 8601 im Format `YYYY-MM-DDThh:mm:ss.fffffffZ`

## <a name="return-types"></a>Rückgabetypen

Gibt einen positiven Integerwert zurück.

## <a name="remarks"></a>Hinweise

DateTimePart gibt aus den folgenden Gründen `undefined` zurück:

- Der angegebene Wert für DateTimePart ist ungültig.
- DateTime ist keine gültige DateTime-Angabe nach ISO 8601.

Der Index wird von dieser Systemfunktion nicht verwendet.

## <a name="examples"></a>Beispiele

Das folgende Beispiel gibt den ganzzahligen Wert des Monats zurück:

```sql
SELECT DateTimePart("m", "2020-01-02T03:04:05.6789123Z") AS MonthValue
```

```json
[
    {
        "MonthValue": 1
    }
]
```

Dieses Beispiel gibt die Anzahl der Mikrosekunden zurück:

```sql
SELECT DateTimePart("mcs", "2020-01-02T03:04:05.6789123Z") AS MicrosecondsValue
```

```json
[
    {
        "MicrosecondsValue": 678912
    }
]
```

## <a name="next-steps"></a>Nächste Schritte

- [Datums- und Uhrzeitfunktionen in Azure Cosmos DB](sql-query-date-time-functions.md)
- [Systemfunktionen in Azure Cosmos DB](sql-query-system-functions.md)
- [Einführung in Azure Cosmos DB](introduction.md)
