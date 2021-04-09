---
title: TimestampToDateTime in der Azure Cosmos DB-Abfragesprache
description: Erfahren Sie mehr über die SQL-Systemfunktion TimestampToDateTime in Azure Cosmos DB.
author: timsander1
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: conceptual
ms.date: 08/18/2020
ms.author: tisande
ms.custom: query-reference
ms.openlocfilehash: e9a77ab0ac32d627d59e2cb0fa4a680f174a6833
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "104587235"
---
# <a name="timestamptodatetime-azure-cosmos-db"></a>TimestampToDateTime (Azure Cosmos DB)
[!INCLUDE[appliesto-sql-api](includes/appliesto-sql-api.md)]

Konvertiert den angegebenen Zeitstempelwert in einen DateTime-Wert.
  
## <a name="syntax"></a>Syntax
  
```sql
TimestampToDateTime (<Timestamp>)
```

## <a name="arguments"></a>Argumente

*Timestamp*  

Ein numerischer Wert mit Vorzeichen, die aktuelle Anzahl der Millisekunden, die seit der Unix-Epoche verstrichen sind. Anders ausgedrückt: die Anzahl der Millisekunden, die seit Donnerstag, 1. Januar 1970, 00:00:00 verstrichen sind.

## <a name="return-types"></a>Rückgabetypen

Gibt einen Zeichenfolgenwert für das UTC-Datum und die UTC-Uhrzeit gemäß ISO 8601 im Format `YYYY-MM-DDThh:mm:ss.fffffffZ` zurück. Hierbei gilt:
  
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

TimestampToDateTime gibt `undefined` zurück, wenn der angegebene Zeitstempelwert ungültig ist.

## <a name="examples"></a>Beispiele
  
Im folgenden Beispiel wird der Zeitstempel in ein DateTime-Element konvertiert.

```sql
SELECT TimestampToDateTime(1594227912345) AS DateTime
```

```json
[
    {
        "DateTime": "2020-07-08T17:05:12.3450000Z"
    }
]
```  

## <a name="next-steps"></a>Nächste Schritte

- [Datums- und Uhrzeitfunktionen in Azure Cosmos DB](sql-query-date-time-functions.md)
- [Systemfunktionen in Azure Cosmos DB](sql-query-system-functions.md)
- [Einführung in Azure Cosmos DB](introduction.md)
