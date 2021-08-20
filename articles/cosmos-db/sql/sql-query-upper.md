---
title: UPPER in der Abfragesprache für Azure Cosmos DB
description: Erfahren Sie mehr über die SQL-Systemfunktion UPPER in Azure Cosmos DB.
author: ginamr
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: conceptual
ms.date: 04/08/2021
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 1a1d3c19bd76805fb26d365d4093a796cc134a41
ms.sourcegitcommit: 2d412ea97cad0a2f66c434794429ea80da9d65aa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/14/2021
ms.locfileid: "122342954"
---
# <a name="upper-azure-cosmos-db"></a>UPPER (Azure Cosmos DB)
[!INCLUDE[appliesto-sql-api](../includes/appliesto-sql-api.md)]

 Gibt eine Zeichenfolge zurück, nachdem Kleinbuchstaben in Großbuchstaben konvertiert wurden.  

Der Index wird von der Systemfunktion UPPER nicht verwendet. Wenn Sie häufig Vergleiche ohne Berücksichtigung der Groß-/Kleinschreibung durchführen möchten, beansprucht die Systemfunktion UPPER möglicherweise eine beträchtliche Anzahl von Anforderungseinheiten. Wenn dies der Fall ist, können Sie statt der Verwendung der Systemfunktion UPPER zum Normalisieren der Daten bei jedem Vergleich die Groß-/Kleinschreibung beim Einfügen normalisieren. Dann wird eine Abfrage wie SELECT * FROM c WHERE UPPER(c.name) = 'BOB' einfach SELECT * FROM c WHERE c.name = 'BOB'.

## <a name="syntax"></a>Syntax
  
```sql
UPPER(<str_expr>)  
```  
  
## <a name="arguments"></a>Argumente
  
*str_expr*  
   Ist ein Zeichenfolgenausdruck.  
  
## <a name="return-types"></a>Rückgabetypen
  
  Gibt einen Zeichenfolgenausdruck zurück.  
  
## <a name="examples"></a>Beispiele
  
  Das folgende Beispiel zeigt, wie `UPPER` in einer Abfrage verwendet wird.  
  
```sql
SELECT UPPER("Abc") AS upper  
```  
  
 Hier ist das Resultset.  
  
```json
[{"upper": "ABC"}]  
```

## <a name="remarks"></a>Bemerkungen

Diese Systemfunktion verwendet keine [Indizes](../index-overview.md#index-usage).

## <a name="next-steps"></a>Nächste Schritte

- [Zeichenfolgenfunktionen in Azure Cosmos DB](sql-query-string-functions.md)
- [Systemfunktionen in Azure Cosmos DB](sql-query-system-functions.md)
- [Einführung in Azure Cosmos DB](../introduction.md)
