---
title: SUM in der Abfragesprache für Azure Cosmos DB
description: Erfahren Sie mehr über die SQL-Systemfunktion SUM in Azure Cosmos DB.
author: timsander1
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: conceptual
ms.date: 12/02/2020
ms.author: tisande
ms.custom: query-reference
ms.openlocfilehash: 77f00474a53b3fa068989a111517dc05806a1ed4
ms.sourcegitcommit: 2d412ea97cad0a2f66c434794429ea80da9d65aa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/14/2021
ms.locfileid: "122342962"
---
# <a name="sum-azure-cosmos-db"></a>SUM (Azure Cosmos DB)
[!INCLUDE[appliesto-sql-api](../includes/appliesto-sql-api.md)]

Diese Aggregatfunktion gibt die Summe der Werte im Ausdruck zurück.
  
## <a name="syntax"></a>Syntax
  
```sql
SUM(<numeric_expr>)  
```  
  
## <a name="arguments"></a>Argumente
  
*numeric_expr*  
   Ist ein numerischer Ausdruck.  
  
## <a name="return-types"></a>Rückgabetypen
  
Gibt einen numerischen Ausdruck zurück.  
  
## <a name="examples"></a>Beispiele
  
Das folgende Beispiel gibt die Summe von `propertyA` zurück:
  
```sql
SELECT SUM(c.propertyA)
FROM c
```  

## <a name="remarks"></a>Bemerkungen

Diese Systemfunktion profitiert von einem [Bereichsindex](../index-policy.md#includeexclude-strategy). Wenn ein Argument in `SUM` eine Zeichenfolge, ein boolescher Wert oder NULL ist, gibt die gesamte Aggregatsystemfunktion `undefined` zurück. Wenn ein Argument einen Wert vom Typ `undefined` aufweist, wirkt sich dies nicht auf die Berechnung von `SUM` aus.

## <a name="next-steps"></a>Nächste Schritte

- [Mathematische Funktionen in Azure Cosmos DB](sql-query-mathematical-functions.md)
- [Systemfunktionen in Azure Cosmos DB](sql-query-system-functions.md)
- [Aggregatfunktionen in Azure Cosmos DB](sql-query-aggregate-functions.md)