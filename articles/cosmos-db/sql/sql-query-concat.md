---
title: CONCAT in der Abfragesprache für Azure Cosmos DB
description: Erfahren Sie, wie die SQL-Systemfunktion CONCAT in Azure Cosmos DB verwendet wird, um eine Zeichenfolge zurückzugeben, die das Ergebnis der Verkettung von mindestens zwei Zeichenfolgenwerten ist.
author: ginamr
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: conceptual
ms.date: 03/03/2020
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: fbedaf0c1b01b4c3ce8ad08f7fec51fe12bf8593
ms.sourcegitcommit: 2d412ea97cad0a2f66c434794429ea80da9d65aa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/14/2021
ms.locfileid: "122342899"
---
# <a name="concat-azure-cosmos-db"></a>CONCAT (Azure Cosmos DB)
[!INCLUDE[appliesto-sql-api](../includes/appliesto-sql-api.md)]

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
- [Einführung in Azure Cosmos DB](../introduction.md)
