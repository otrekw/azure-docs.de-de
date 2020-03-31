---
title: LOG in der Abfragesprache für Azure Cosmos DB
description: Erfahren Sie mehr über die SQL-Systemfunktion LOG in Azure Cosmos DB, die den natürlichen Logarithmus des angegebenen numerischen Ausdrucks zurückgibt.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 9213ef03f383dec7109652246411fac154b4a7f9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "78302507"
---
# <a name="log-azure-cosmos-db"></a>LOG (Azure Cosmos DB)
 Gibt den natürlichen Logarithmus des angegebenen numerischen Ausdrucks zurück.  
  
## <a name="syntax"></a>Syntax
  
```sql
LOG (<numeric_expr> [, <base>])  
```  
  
## <a name="arguments"></a>Argumente
  
*numeric_expr*  
   Ist ein numerischer Ausdruck.  
  
*base*  
   Optionales numerisches Argument, das die Basis des Logarithmus festlegt.  
  
## <a name="return-types"></a>Rückgabetypen
  
  Gibt einen numerischen Ausdruck zurück.  
  
## <a name="remarks"></a>Bemerkungen
  
  Standardmäßig gibt LOG() den natürlichen Logarithmus zurück. Sie können die Basis des Logarithmus mithilfe des optionalen base-Parameters in einen anderen Wert ändern.  
  
  Der natürliche Logarithmus ist der Logarithmus zur Basis **e**, wobei **e** eine irrationale Konstante mit einem Wert von ungefähr 2,718281828 ist.  
  
  Der natürliche Logarithmus des Exponentialwerts einer Zahl ist die Zahl selbst: LOG( EXP(n) ) = n. Und der Exponentialwert des natürlichen Logarithmus einer Zahl ist die Zahl selbst: EXP( LOG (n) ) = n.  
  
## <a name="examples"></a>Beispiele
  
  Im folgenden Beispiel wird eine Variable deklariert und der Logarithmuswert der angegebenen Variablen zurückgegeben (10).  
  
```sql
SELECT LOG(10) AS log  
```  
  
 Hier ist das Resultset.  
  
```json
[{log: 2.3025850929940459}]  
```  
  
 Im folgenden Beispiel wird `LOG` für den Exponenten einer Zahl berechnet.  
  
```sql
SELECT EXP(LOG(10)) AS expLog  
```  
  
 Hier ist das Resultset.  
  
```json
[{expLog: 10.000000000000002}]  
```  

## <a name="remarks"></a>Bemerkungen

Der Index wird von dieser Systemfunktion nicht verwendet.

## <a name="next-steps"></a>Nächste Schritte

- [Mathematische Funktionen in Azure Cosmos DB](sql-query-mathematical-functions.md)
- [Systemfunktionen in Azure Cosmos DB](sql-query-system-functions.md)
- [Einführung in Azure Cosmos DB](introduction.md)
