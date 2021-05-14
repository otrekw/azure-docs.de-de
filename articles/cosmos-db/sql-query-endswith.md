---
title: EndsWith in der Abfragesprache für Azure Cosmos DB
description: Hier erfahren Sie mehr über die SQL-Systemfunktion ENDSWITH in Azure Cosmos DB, die einen booleschen Wert zurückgibt, der angibt, ob der erste Zeichenfolgenausdruck mit dem zweiten endet.
author: ginamr
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: conceptual
ms.date: 06/02/2020
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 64514e69b41dda26fc39e747d7fef88706a64c64
ms.sourcegitcommit: 62e800ec1306c45e2d8310c40da5873f7945c657
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2021
ms.locfileid: "108163875"
---
# <a name="endswith-azure-cosmos-db"></a>ENDSWITH (Azure Cosmos DB)
[!INCLUDE[appliesto-sql-api](includes/appliesto-sql-api.md)]

Gibt einen booleschen Wert zurück, um anzugeben, ob der erste Zeichenfolgenausdruck mit dem zweiten endet.  
  
## <a name="syntax"></a>Syntax
  
```sql
ENDSWITH(<str_expr1>, <str_expr2> [, <bool_expr>])
```  
  
## <a name="arguments"></a>Argumente
  
*str_expr1*  
   Ist ein Zeichenfolgenausdruck.  
  
*str_expr2*  
   Ist ein Zeichenfolgenausdruck, der mit dem Ende von *str_expr1* verglichen werden soll.

*bool_expr* ist ein optionaler Wert zum Ignorieren der Groß-/Kleinschreibung. Wenn diese Einstellung auf TRUE festgelegt ist, wird bei ENDSWITH eine Suche ohne Beachtung der Groß-/Kleinschreibung durchgeführt. Wenn keine Angabe erfolgt, ist dieser Wert FALSE.
  
## <a name="return-types"></a>Rückgabetypen
  
  Gibt einen booleschen Ausdruck zurück.  
  
## <a name="examples"></a>Beispiele
  
Das folgende Beispiel überprüft, ob die Zeichenfolge „abc“ mit „b“ und „bC“ endet.  
  
```sql
SELECT ENDSWITH("abc", "b", false) AS e1, ENDSWITH("abc", "bC", false) AS e2, ENDSWITH("abc", "bC", true) AS e3
```  
  
 Hier ist das Resultset.  
  
```json
[
    {
        "e1": false,
        "e2": false,
        "e3": true
    }
]
```  

## <a name="remarks"></a>Bemerkungen

Erfahren Sie, [wie diese Systemfunktion mit Zeichenfolgen den Index verwendet](sql-query-string-functions.md).

## <a name="next-steps"></a>Nächste Schritte

- [Zeichenfolgenfunktionen in Azure Cosmos DB](sql-query-string-functions.md)
- [Systemfunktionen in Azure Cosmos DB](sql-query-system-functions.md)
- [Einführung in Azure Cosmos DB](introduction.md)
