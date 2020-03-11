---
title: CONCAT in der Abfragesprache für Azure Cosmos DB
description: Erfahren Sie, wie die SQL-Systemfunktion CONCAT in Azure Cosmos DB verwendet wird, um eine Zeichenfolge zurückzugeben, die das Ergebnis der Verkettung von mindestens zwei Zeichenfolgenwerten ist.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/03/2020
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: c8a0941376ed74d7f8cb819d78df43eb9f0b7bd1
ms.sourcegitcommit: f915d8b43a3cefe532062ca7d7dbbf569d2583d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/05/2020
ms.locfileid: "78302609"
---
# <a name="concat-azure-cosmos-db"></a>CONCAT (Azure Cosmos DB)
 Gibt eine Zeichenfolge zurück, die das Ergebnis der Verkettung von zwei oder mehr Zeichenfolgenwerten darstellt.  
  
## <a name="syntax"></a>Syntax
  
```sql
CONCAT(<str_expr1>, <str_expr2> [, <str_exprN>])  
```  
  
## <a name="arguments"></a>Argumente
  
*str_expr*  
   Ein Zeichenfolgenausdruck, der mit den anderen Werten verkettet werden soll. Die `CONCAT`-Funktion erfordert mindestens zwei *str_expr*-Argumente.  
  
## <a name="return-types"></a>Rückgabetypen
  
  Gibt einen Zeichenfolgenausdruck zurück.  
  
## <a name="examples"></a>Beispiele
  
  Das folgende Beispiel gibt die verkettete Zeichenfolge der angegebenen Werte zurück.  
  
```sql
SELECT CONCAT("abc", "def") AS concat  
```  
  
 Hier ist das Resultset.  
  
```json
[{"concat": "abcdef"}]  
```  
  
## <a name="remarks"></a>Bemerkungen

Der Index wird von dieser Systemfunktion nicht verwendet.

## <a name="next-steps"></a>Nächste Schritte

- [Zeichenfolgenfunktionen in Azure Cosmos DB](sql-query-string-functions.md)
- [Systemfunktionen in Azure Cosmos DB](sql-query-system-functions.md)
- [Einführung in Azure Cosmos DB](introduction.md)
