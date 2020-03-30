---
title: STARTSWITH in der Abfragesprache für Azure Cosmos DB
description: Erfahren Sie mehr über die SQL-Systemfunktion STARTSWITH in Azure Cosmos DB.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 9ed49c067946186f8b79f67bad0a460113eacb73
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "78295707"
---
# <a name="startswith-azure-cosmos-db"></a>STARTSWITH (Azure Cosmos DB)
 Gibt einen booleschen Wert zurück, um anzugeben, ob der erste Zeichenfolgenausdruck mit dem zweiten beginnt.  
  
## <a name="syntax"></a>Syntax
  
```sql
STARTSWITH(<str_expr1>, <str_expr2>)  
```  
  
## <a name="arguments"></a>Argumente
  
*str_expr1*  
   Ist ein Zeichenfolgenausdruck.
  
*str_expr2*  
   Ist ein Zeichenfolgenausdruck, der mit dem Anfang von *str_expr1* verglichen werden soll.

## <a name="return-types"></a>Rückgabetypen
  
  Gibt einen booleschen Ausdruck zurück.  
  
## <a name="examples"></a>Beispiele
  
  Das folgende Beispiel überprüft, ob die Zeichenfolge „abc“ mit „b“ und „a“ beginnt.  
  
```sql
SELECT STARTSWITH("abc", "b") AS s1, STARTSWITH("abc", "a") AS s2  
```  
  
 Hier ist das Resultset.  
  
```json
[{"s1": false, "s2": true}]  
```  

## <a name="remarks"></a>Bemerkungen

Diese Systemfunktion profitiert von einem [Bereichsindex](index-policy.md#includeexclude-strategy).

## <a name="next-steps"></a>Nächste Schritte

- [Zeichenfolgenfunktionen in Azure Cosmos DB](sql-query-string-functions.md)
- [Systemfunktionen in Azure Cosmos DB](sql-query-system-functions.md)
- [Einführung in Azure Cosmos DB](introduction.md)
