---
title: ATAN in der Abfragesprache für Azure Cosmos DB
description: Erfahren Sie, wie die SQL-Systemfunktion Arkustangens (ATAN) in Azure Cosmos DB den Winkel, dessen Tangens der angegebene numerische Ausdruck ist, im Bogenmaß zurückgibt.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/04/2020
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 899c94a939be7825dca82522eab235bde9252896
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "78302677"
---
# <a name="atan-azure-cosmos-db"></a>ATAN (Azure Cosmos DB)
 Gibt den Winkel in „rad“ (Bogenmaß) zurück, dessen Tangens der angegebene numerische Ausdruck ist. Dies wird auch als Arkustangens bezeichnet.  
  
## <a name="syntax"></a>Syntax
  
```sql
ATAN(<numeric_expr>)  
```  
  
## <a name="arguments"></a>Argumente
  
*numeric_expr*  
   Ist ein numerischer Ausdruck.  
  
## <a name="return-types"></a>Rückgabetypen
  
  Gibt einen numerischen Ausdruck zurück.  
  
## <a name="examples"></a>Beispiele
  
  Im folgenden Beispiel wird `ATAN` für den angegebenen Wert zurückgegeben.  
  
```sql
SELECT ATAN(-45.01) AS atan  
```  
  
 Hier ist das Resultset.  
  
```json
[{"atan": -1.5485826962062663}]  
```  
  
## <a name="remarks"></a>Bemerkungen

Der Index wird von dieser Systemfunktion nicht verwendet.

## <a name="next-steps"></a>Nächste Schritte

- [Mathematische Funktionen in Azure Cosmos DB](sql-query-mathematical-functions.md)
- [Systemfunktionen in Azure Cosmos DB](sql-query-system-functions.md)
- [Einführung in Azure Cosmos DB](introduction.md)
