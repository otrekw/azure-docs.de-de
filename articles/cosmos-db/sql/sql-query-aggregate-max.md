---
title: MAX in der Abfragesprache für Azure Cosmos DB
description: Erfahren Sie mehr über die SQL-Systemfunktion MAX in Azure Cosmos DB.
author: timsander1
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: conceptual
ms.date: 12/02/2020
ms.author: tisande
ms.custom: query-reference
ms.openlocfilehash: b14ca6a61644ab6f668015af62cdea899320c12d
ms.sourcegitcommit: 2d412ea97cad0a2f66c434794429ea80da9d65aa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/14/2021
ms.locfileid: "122350544"
---
# <a name="max-azure-cosmos-db"></a>MAX (Azure Cosmos DB)
[!INCLUDE[appliesto-sql-api](../includes/appliesto-sql-api.md)]

Diese Aggregatfunktion gibt das Maximum der Werte im Ausdruck zurück.
  
## <a name="syntax"></a>Syntax
  
```sql
MAX(<scalar_expr>)  
```  
  
## <a name="arguments"></a>Argumente

*scalar_expr*  
   Ist ein skalarer Ausdruck. 
  
## <a name="return-types"></a>Rückgabetypen
  
Gibt einen skalaren Ausdruck zurück.  
  
## <a name="examples"></a>Beispiele
  
Das folgende Beispiel gibt den Maximalwert von `propertyA` zurück:
  
```sql
SELECT MAX(c.propertyA)
FROM c
```  

## <a name="remarks"></a>Bemerkungen

Diese Systemfunktion profitiert von einem [Bereichsindex](../index-policy.md#includeexclude-strategy). Die Argumente in `MAX` können eine Zahl, eine Zeichenfolge, ein boolescher Wert oder NULL sein. Alle nicht definierten Werte werden ignoriert.

Beim Vergleichen verschiedener Datentypen wird die folgende Prioritätsreihenfolge verwendet (in absteigender Reihenfolge):

- Zeichenfolge
- Zahl
- boolean
- NULL

## <a name="next-steps"></a>Nächste Schritte

- [Mathematische Funktionen in Azure Cosmos DB](sql-query-mathematical-functions.md)
- [Systemfunktionen in Azure Cosmos DB](sql-query-system-functions.md)
- [Aggregatfunktionen in Azure Cosmos DB](sql-query-aggregate-functions.md)