---
title: IS_BOOL in der Azure Cosmos DB-Abfragesprache
description: Erfahren Sie mehr zur SQL-Systemfunktion IS_BOOL in Azure Cosmos DB.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: b7f1cfb09121309e246b314d57a5e4e475bd0983
ms.sourcegitcommit: f915d8b43a3cefe532062ca7d7dbbf569d2583d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/05/2020
ms.locfileid: "78303867"
---
# <a name="is_bool-azure-cosmos-db"></a>IS_BOOL (Azure Cosmos DB)
 Gibt einen booleschen Wert zurück, der angibt, ob der angegebene Ausdruck vom Typ „boolesch“ ist.  
  
## <a name="syntax"></a>Syntax
  
```sql
IS_BOOL(<expr>)  
```  
  
## <a name="arguments"></a>Argumente
  
*expr*  
   Ist ein beliebiger Ausdruck.  
  
## <a name="return-types"></a>Rückgabetypen
  
  Gibt einen booleschen Ausdruck zurück.  
  
## <a name="examples"></a>Beispiele
  
  Im folgenden Beispiel werden JSON-Objekte des Typs „Boolean“, „Number“, „String“, „NULL“, „Object“, „Array“ und „undefined“ mithilfe der `IS_BOOL`-Funktion überprüft.  
  
```sql
SELECT   
    IS_BOOL(true) AS isBool1,   
    IS_BOOL(1) AS isBool2,  
    IS_BOOL("value") AS isBool3,   
    IS_BOOL(null) AS isBool4,  
    IS_BOOL({prop: "value"}) AS isBool5,   
    IS_BOOL([1, 2, 3]) AS isBool6,  
    IS_BOOL({prop: "value"}.prop2) AS isBool7  
```  
  
 Hier ist das Resultset.  
  
```json
[{"isBool1":true,"isBool2":false,"isBool3":false,"isBool4":false,"isBool5":false,"isBool6":false,"isBool7":false}]
```  

## <a name="remarks"></a>Bemerkungen

Diese Systemfunktion profitiert von einem [Bereichsindex](index-policy.md#includeexclude-strategy).

## <a name="next-steps"></a>Nächste Schritte

- [Funktionen für die Typüberprüfung in Azure Cosmos DB](sql-query-type-checking-functions.md)
- [Systemfunktionen in Azure Cosmos DB](sql-query-system-functions.md)
- [Einführung in Azure Cosmos DB](introduction.md)
