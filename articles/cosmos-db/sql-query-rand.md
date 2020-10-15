---
title: RAND in der Abfragesprache für Azure Cosmos DB
description: Erfahren Sie mehr über die SQL-Systemfunktion RAND in Azure Cosmos DB.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/16/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: ff098da778221868b0eddc17c426d2bf36eec0fe
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "88794340"
---
# <a name="rand-azure-cosmos-db"></a>RAND (Azure Cosmos DB)
 Gibt einen nach dem Zufallsprinzip generierten numerischen Wert aus [0,1] zurück.
 
## <a name="syntax"></a>Syntax
  
```sql
RAND ()  
```  

## <a name="return-types"></a>Rückgabetypen

  Gibt einen numerischen Ausdruck zurück.

## <a name="remarks"></a>Bemerkungen

  `RAND` ist eine nichtdeterministische Funktion. Wiederkehrende Aufrufe von `RAND` geben nicht die gleichen Ergebnisse zurück. Der Index wird von dieser Systemfunktion nicht verwendet.


## <a name="examples"></a>Beispiele
  
  Das folgende Beispiel gibt einen nach dem Zufallsprinzip generierten numerischen Wert zurück.
  
```sql
SELECT RAND() AS rand 
```  
  
 Hier ist das Resultset.  
  
```json
[{"rand": 0.87860053195618093}]  
``` 

## <a name="next-steps"></a>Nächste Schritte

- [Mathematische Funktionen in Azure Cosmos DB](sql-query-mathematical-functions.md)
- [Systemfunktionen in Azure Cosmos DB](sql-query-system-functions.md)
- [Einführung in Azure Cosmos DB](introduction.md)
