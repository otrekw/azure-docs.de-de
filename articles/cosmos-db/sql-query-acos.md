---
title: ACOS in der Abfragesprache für Azure Cosmos DB
description: Erfahren Sie, wie die SQL-Systemfunktion ACOS (Arkuskosinus) in Azure Cosmos DB den Winkel, dessen Kosinus der angegebene numerische Ausdruck ist, im Bogenmaß zurückgibt.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: fd5baa3ffefb7039c40706253e633661a51150f2
ms.sourcegitcommit: 9405aad7e39efbd8fef6d0a3c8988c6bf8de94eb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/05/2019
ms.locfileid: "74873521"
---
# <a name="acos-azure-cosmos-db"></a>ACOS (Azure Cosmos DB)
 Gibt den Winkel in „rad“ (Bogenmaß) zurück, dessen Kosinus der angegebene numerische Ausdruck ist. Wird auch als Arkuskosinus bezeichnet.  
  
## <a name="syntax"></a>Syntax
  
```sql
ACOS(<numeric_expr>)  
```  
  
## <a name="arguments"></a>Argumente
  
*numeric_expr*  
   Ist ein numerischer Ausdruck.  
  
## <a name="return-types"></a>Rückgabetypen
  
  Gibt einen numerischen Ausdruck zurück.  
  
## <a name="examples"></a>Beispiele
  
  Im folgenden Beispiel wird `ACOS` für -1 zurückgegeben.  
  
```sql
SELECT ACOS(-1) AS acos 
```  
  
 Hier ist das Resultset.  
  
```json
[{"acos": 3.1415926535897931}]  
```  

## <a name="next-steps"></a>Nächste Schritte

- [Mathematische Funktionen in Azure Cosmos DB](sql-query-mathematical-functions.md)
- [Systemfunktionen in Azure Cosmos DB](sql-query-system-functions.md)
- [Einführung in Azure Cosmos DB](introduction.md)
