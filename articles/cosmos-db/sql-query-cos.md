---
title: COS in der Abfragesprache für Azure Cosmos DB
description: Erfahren Sie, wie die SQL-Systemfunktion „Kosinus“ (COS) in Azure Cosmos DB den trigonometrischen Kosinus des angegebenen Winkels im Bogenmaß im angegebenen Ausdruck zurückgibt.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/03/2020
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 40d277ff38691e2cb74bd4d5d78a666c304acfcd
ms.sourcegitcommit: f915d8b43a3cefe532062ca7d7dbbf569d2583d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/05/2020
ms.locfileid: "78304020"
---
# <a name="cos-azure-cosmos-db"></a>COS (Azure Cosmos DB)
 Gibt den trigonometrischen Kosinus des angegebenen Winkels in „rad“ (Bogenmaß) im angegebenen Ausdruck zurück.  
  
## <a name="syntax"></a>Syntax
  
```sql
COS(<numeric_expr>)  
```  
  
## <a name="arguments"></a>Argumente
  
*numeric_expr*  
   Ist ein numerischer Ausdruck.  
  
## <a name="return-types"></a>Rückgabetypen
  
  Gibt einen numerischen Ausdruck zurück.  
  
## <a name="examples"></a>Beispiele
  
  Durch das folgende Beispiel wird der `COS` des angegebenen Winkels berechnet.  
  
```sql
SELECT COS(14.78) AS cos  
```  
  
 Hier ist das Resultset.  
  
```json
[{"cos": -0.59946542619465426}]  
```  

## <a name="remarks"></a>Bemerkungen

Der Index wird von dieser Systemfunktion nicht verwendet.

## <a name="next-steps"></a>Nächste Schritte

- [Mathematische Funktionen in Azure Cosmos DB](sql-query-mathematical-functions.md)
- [Systemfunktionen in Azure Cosmos DB](sql-query-system-functions.md)
- [Einführung in Azure Cosmos DB](introduction.md)
