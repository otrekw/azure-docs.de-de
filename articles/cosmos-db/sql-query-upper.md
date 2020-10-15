---
title: UPPER in der Abfragesprache für Azure Cosmos DB
description: Erfahren Sie mehr über die SQL-Systemfunktion UPPER in Azure Cosmos DB.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/04/2020
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 5129b4fffafb6918f655263cac2f5564635acf36
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "78303969"
---
# <a name="upper-azure-cosmos-db"></a>UPPER (Azure Cosmos DB)
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

Der Index wird von dieser Systemfunktion nicht verwendet.

## <a name="next-steps"></a>Nächste Schritte

- [Zeichenfolgenfunktionen in Azure Cosmos DB](sql-query-string-functions.md)
- [Systemfunktionen in Azure Cosmos DB](sql-query-system-functions.md)
- [Einführung in Azure Cosmos DB](introduction.md)
