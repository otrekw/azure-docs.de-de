---
title: RAND in der Abfragesprache für Azure Cosmos DB
description: Erfahren Sie mehr über die SQL-Systemfunktion RAND in Azure Cosmos DB.
author: ginamr
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: conceptual
ms.date: 09/16/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: cd903a6f067c116277a7ec3c2d7d01ce8e2dedad
ms.sourcegitcommit: 2d412ea97cad0a2f66c434794429ea80da9d65aa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/14/2021
ms.locfileid: "122350535"
---
# <a name="rand-azure-cosmos-db"></a>RAND (Azure Cosmos DB)
[!INCLUDE[appliesto-sql-api](../includes/appliesto-sql-api.md)]

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
- [Einführung in Azure Cosmos DB](../introduction.md)
