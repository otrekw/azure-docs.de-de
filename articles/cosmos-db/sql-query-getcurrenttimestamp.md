---
title: GetCurrentTimestamp in der Abfragesprache für Azure Cosmos DB
description: Erfahren Sie mehr über die SQL-Systemfunktion GetCurrentTimestamp in Azure Cosmos DB.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 08/19/2020
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: c1e0a8d69edab0c01005268ee49c23625236f03a
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "88606929"
---
# <a name="getcurrenttimestamp-azure-cosmos-db"></a>GetCurrentTimestamp (Azure Cosmos DB)

 Diese Funktion gibt die Anzahl der Millisekunden zurück, die seit dem Donnerstag am 1. Januar 1970 um 00:00:00 Uhr vergangen sind.
  
## <a name="syntax"></a>Syntax
  
```sql
GetCurrentTimestamp ()  
```  
  
## <a name="return-types"></a>Rückgabetypen
  
Gibt einen numerischen Wert mit Vorzeichen zurück, der der aktuellen Anzahl der Millisekunden entspricht, die seit der Unix-Epoche verstrichen sind, d. h. die Anzahl der Millisekunden, die seit Donnerstag, 1. Januar 1970, 00:00:00 Uhr verstrichen sind.

## <a name="remarks"></a>Bemerkungen

GetCurrentTimestamp() ist eine nichtdeterministische Funktion. Das zurückgegebene Ergebnis entspricht der koordinierten Weltzeit (UTC).

Der Index wird von dieser Systemfunktion nicht verwendet.

## <a name="examples"></a>Beispiele
  
  Im folgenden Beispiel wird gezeigt, wie der aktuelle Zeitstempel mithilfe der integrierten GetCurrentTimestamp()-Funktion abgerufen wird.
  
```sql
SELECT GetCurrentTimestamp() AS currentUtcTimestamp
```  
  
 Hier ist ein Beispielresultset:
  
```json
[{
  "currentUtcTimestamp": 1556916469065
}]  
```

## <a name="next-steps"></a>Nächste Schritte

- [Datums- und Uhrzeitfunktionen in Azure Cosmos DB](sql-query-date-time-functions.md)
- [Systemfunktionen in Azure Cosmos DB](sql-query-system-functions.md)
- [Einführung in Azure Cosmos DB](introduction.md)
