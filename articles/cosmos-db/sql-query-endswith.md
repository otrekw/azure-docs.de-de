---
title: EndsWith in der Abfragesprache für Azure Cosmos DB
description: Hier erfahren Sie mehr über die SQL-Systemfunktion ENDSWITH in Azure Cosmos DB, die einen booleschen Wert zurückgibt, der angibt, ob der erste Zeichenfolgenausdruck mit dem zweiten endet.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/20/2020
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 0cd927af50eca04aa8162d9d8f292077d9e4165c
ms.sourcegitcommit: 1f25aa993c38b37472cf8a0359bc6f0bf97b6784
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/26/2020
ms.locfileid: "83844963"
---
# <a name="endswith-azure-cosmos-db"></a>ENDSWITH (Azure Cosmos DB)

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

Diese Systemfunktion profitiert von einem [Bereichsindex](index-policy.md#includeexclude-strategy).

Der RU-Verbrauch von EndsWith erhöht sich, wenn die Kardinalität der Eigenschaft in der Systemfunktion zunimmt. Anders ausgedrückt: Wenn Sie überprüfen, ob ein Eigenschaftswert mit einer bestimmten Zeichenfolge endet, hängt die RU-Gebühr der Anforderung von der Anzahl möglicher Werte für diese Eigenschaft ab.

Sehen Sie sich beispielsweise die beiden Eigenschaften „town“ und „country“ an. Die Kardinalität von „town“ ist 5.000, die Kardinalität von „country“ ist 200. Hier sind zwei Beispielabfragen:

```sql
    SELECT * FROM c WHERE ENDSWITH(c.town, "York", false)
```

```sql
    SELECT * FROM c WHERE ENDSWITH(c.country, "States", false)
```

Die erste Abfrage verbraucht wahrscheinlich mehr RUs als die zweite Abfrage, da die Kardinalität von „town“ höher als die von „country“ ist.

## <a name="next-steps"></a>Nächste Schritte

- [Zeichenfolgenfunktionen in Azure Cosmos DB](sql-query-string-functions.md)
- [Systemfunktionen in Azure Cosmos DB](sql-query-system-functions.md)
- [Einführung in Azure Cosmos DB](introduction.md)
