---
title: LOG10 in der Abfragesprache für Azure Cosmos DB
description: Hier erfahren Sie mehr über die SQL-Systemfunktion LOG10 in Azure Cosmos DB, die den Logarithmus zur Basis 10 des angegebenen numerischen Ausdrucks zurückgibt.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 6f47200f6978d91f46c010640bb9c2bb26e9b7d5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "78302490"
---
# <a name="log10-azure-cosmos-db"></a>LOG10 (Azure Cosmos DB)
 Gibt den dekadischen Logarithmus des angegebenen numerischen Ausdrucks zurück.  
  
## <a name="syntax"></a>Syntax
  
```sql
LOG10 (<numeric_expr>)  
```  
  
## <a name="arguments"></a>Argumente
  
*numeric_expression*  
   Ist ein numerischer Ausdruck.  
  
## <a name="return-types"></a>Rückgabetypen
  
  Gibt einen numerischen Ausdruck zurück.  
  
## <a name="remarks"></a>Bemerkungen
  
  Die Funktionen LOG10 und POWER sind ihre gegenseitigen Umkehrfunktionen. Beispiel: 10 ^ LOG10(n) = n.  
  
## <a name="examples"></a>Beispiele
  
  Im folgenden Beispiel wird eine Variable deklariert und der LOG10-Wert der angegebenen Variablen zurückgegeben (100).  
  
```sql
SELECT LOG10(100) AS log10 
```  
  
 Hier ist das Resultset.  
  
```json
[{log10: 2}]  
```  

## <a name="remarks"></a>Bemerkungen

Der Index wird von dieser Systemfunktion nicht verwendet.

## <a name="next-steps"></a>Nächste Schritte

- [Mathematische Funktionen in Azure Cosmos DB](sql-query-mathematical-functions.md)
- [Systemfunktionen in Azure Cosmos DB](sql-query-system-functions.md)
- [Einführung in Azure Cosmos DB](introduction.md)
