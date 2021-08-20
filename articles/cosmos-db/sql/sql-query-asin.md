---
title: ASIN in der Abfragesprache für Azure Cosmos DB
description: Erfahren Sie, wie die SQL-Systemfunktion Arkussinus (ASIN) in Azure Cosmos DB den Winkel, dessen Sinus der angegebene numerische Ausdruck ist, im Bogenmaß zurückgibt.
author: ginamr
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: conceptual
ms.date: 03/04/2020
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: aa2628d83cbd91abe6163a8758349bca47257221
ms.sourcegitcommit: 2d412ea97cad0a2f66c434794429ea80da9d65aa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/14/2021
ms.locfileid: "122342902"
---
# <a name="asin-azure-cosmos-db"></a>ASIN (Azure Cosmos DB)
[!INCLUDE[appliesto-sql-api](../includes/appliesto-sql-api.md)]

 Gibt den Winkel in „rad“ (Bogenmaß) zurück, dessen Sinus der angegebene numerische Ausdruck ist. Wird auch als Arkussinus bezeichnet.  
  
## <a name="syntax"></a>Syntax
  
```sql
ASIN(<numeric_expr>)  
```  
  
## <a name="arguments"></a>Argumente
  
*numeric_expr*  
   Ist ein numerischer Ausdruck.  
  
## <a name="return-types"></a>Rückgabetypen
  
  Gibt einen numerischen Ausdruck zurück.  
  
## <a name="examples"></a>Beispiele
  
  Im folgenden Beispiel wird `ASIN` für -1 zurückgegeben.  
  
```sql
SELECT ASIN(-1) AS asin  
```  
  
 Hier ist das Resultset.  
  
```json
[{"asin": -1.5707963267948966}]  
```  

## <a name="remarks"></a>Bemerkungen

Der Index wird von dieser Systemfunktion nicht verwendet.

## <a name="next-steps"></a>Nächste Schritte

- [Mathematische Funktionen in Azure Cosmos DB](sql-query-mathematical-functions.md)
- [Systemfunktionen in Azure Cosmos DB](sql-query-system-functions.md)
- [Einführung in Azure Cosmos DB](../introduction.md)
