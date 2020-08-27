---
title: REPLICATE in der Abfragesprache für Azure Cosmos DB
description: Erfahren Sie mehr über die SQL-Systemfunktion REPLICATE in Azure Cosmos DB.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/03/2020
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: aea29cfff6b3827cfb9169722e48120e3a5a3709
ms.sourcegitcommit: c5021f2095e25750eb34fd0b866adf5d81d56c3a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/25/2020
ms.locfileid: "88794319"
---
# <a name="replicate-azure-cosmos-db"></a>REPLICATE (Azure Cosmos DB)
 Wiederholt einen Zeichenfolgenwert mit einer angegebenen Anzahl.
  
## <a name="syntax"></a>Syntax
  
```sql
REPLICATE(<str_expr>, <num_expr>)
```  
  
## <a name="arguments"></a>Argumente
  
*str_expr*  
   Ist ein Zeichenfolgenausdruck.
  
*num_expr*  
   Ist ein numerischer Ausdruck. Wenn *num_expr* negativ oder unendlich ist, ist das Ergebnis nicht definiert.
  
## <a name="return-types"></a>Rückgabetypen
  
  Gibt einen Zeichenfolgenausdruck zurück.
  
## <a name="remarks"></a>Bemerkungen

  Die maximale Länge des Ergebnisses beträgt 10.000 Zeichen ((length(*str_expr*)  *  *num_expr*) <= 10.000). Der Index wird von dieser Systemfunktion nicht verwendet.

## <a name="examples"></a>Beispiele
  
  Das folgende Beispiel zeigt, wie `REPLICATE` in einer Abfrage verwendet wird.
  
```sql
SELECT REPLICATE("a", 3) AS replicate
```  
  
 Hier ist das Resultset.
  
```json
[{"replicate": "aaa"}]
```  

## <a name="next-steps"></a>Nächste Schritte

- [Zeichenfolgenfunktionen in Azure Cosmos DB](sql-query-string-functions.md)
- [Systemfunktionen in Azure Cosmos DB](sql-query-system-functions.md)
- [Einführung in Azure Cosmos DB](introduction.md)
