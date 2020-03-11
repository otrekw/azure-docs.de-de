---
title: ROUND in der Abfragesprache für Azure Cosmos DB
description: Erfahren Sie mehr über die SQL-Systemfunktion ROUND in Azure Cosmos DB.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: b6aac5a963d0f58a3b21b9fb0958793169a3d444
ms.sourcegitcommit: f915d8b43a3cefe532062ca7d7dbbf569d2583d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/05/2020
ms.locfileid: "78302116"
---
# <a name="round-azure-cosmos-db"></a>ROUND (Azure Cosmos DB)
 Gibt einen numerischen Wert zurück, gerundet auf den nächsten ganzzahligen Wert.  
  
## <a name="syntax"></a>Syntax
  
```sql
ROUND(<numeric_expr>)  
```  
  
## <a name="arguments"></a>Argumente
  
*numeric_expr*  
   Ist ein numerischer Ausdruck.  
  
## <a name="return-types"></a>Rückgabetypen
  
  Gibt einen numerischen Ausdruck zurück.  
  
## <a name="remarks"></a>Bemerkungen
  
  Der durchgeführte Rundungsvorgang folgt der Mittelpunktrundung von null. Wenn die Eingabe ein numerischer Ausdruck ist, der genau zwischen zwei Integern liegt, ist das Ergebnis der nächste Integerwert abseits von null.  
  
  |<numeric_expr>|Gerundet|
  |-|-|
  |–6,5000|-7|
  |–0,5|-1|
  |0.5|1|
  |6,5000|7||
  
## <a name="examples"></a>Beispiele
  
  Im folgenden Beispiel werden die folgenden positiven und negativen Zahlen auf die nächste ganze Zahl gerundet.  
  
```sql
SELECT ROUND(2.4) AS r1, ROUND(2.6) AS r2, ROUND(2.5) AS r3, ROUND(-2.4) AS r4, ROUND(-2.6) AS r5  
```  
  
  Hier ist das Resultset.  
  
```json
[{r1: 2, r2: 3, r3: 3, r4: -2, r5: -3}]  
```  

## <a name="remarks"></a>Bemerkungen

Diese Systemfunktion profitiert von einem [Bereichsindex](index-policy.md#includeexclude-strategy).

## <a name="next-steps"></a>Nächste Schritte

- [Mathematische Funktionen in Azure Cosmos DB](sql-query-mathematical-functions.md)
- [Systemfunktionen in Azure Cosmos DB](sql-query-system-functions.md)
- [Einführung in Azure Cosmos DB](introduction.md)
