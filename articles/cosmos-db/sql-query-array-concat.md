---
title: ARRAY_CONCAT in der Abfragesprache für Azure Cosmos DB
description: Erfahren Sie, wie die SQL-Systemfunktion Array Concat in Azure Cosmos DB verwendet wird, um ein Array zurückzugeben, das das Ergebnis der Verkettung von mehreren Arraywerten ist.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 10370e16c95f4fc747dd3a66a56794da38562972
ms.sourcegitcommit: 9405aad7e39efbd8fef6d0a3c8988c6bf8de94eb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/05/2019
ms.locfileid: "74871821"
---
# <a name="array_concat-azure-cosmos-db"></a>ARRAY_CONCAT (Azure Cosmos DB)
 Gibt ein Array zurück, das das Ergebnis der Verkettung von zwei oder mehr Arraywerten darstellt.  
  
## <a name="syntax"></a>Syntax
  
```sql
ARRAY_CONCAT (<arr_expr1>, <arr_expr2> [, <arr_exprN>])  
```  
  
## <a name="arguments"></a>Argumente
  
*arr_expr*  
   Ist ein Arrayausdruck, der mit den anderen Werten verkettet werden soll. Die `ARRAY_CONCAT`-Funktion erfordert mindestens zwei *arr_expr*-Argumente.  
  
## <a name="return-types"></a>Rückgabetypen
  
  Gibt einen Arrayausdruck zurück.  
  
## <a name="examples"></a>Beispiele
  
  Im folgenden Beispiel wird veranschaulicht, wie zwei Arrays verkettet werden.  
  
```sql
SELECT ARRAY_CONCAT(["apples", "strawberries"], ["bananas"]) AS arrayConcat 
```  
  
 Hier ist das Resultset.  
  
```json
[{"arrayConcat": ["apples", "strawberries", "bananas"]}]  
```  
  

## <a name="next-steps"></a>Nächste Schritte

- [Arrayfunktionen in Azure Cosmos DB](sql-query-array-functions.md)
- [Systemfunktionen in Azure Cosmos DB](sql-query-system-functions.md)
- [Einführung in Azure Cosmos DB](introduction.md)
