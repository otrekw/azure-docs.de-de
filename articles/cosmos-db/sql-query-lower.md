---
title: LOWER in der Abfragesprache für Azure Cosmos DB
description: Erfahren Sie mehr über die SQL-Systemfunktion LOWER in Azure Cosmos DB, die einen Zeichenfolgenausdruck zurückgibt, nachdem Zeichendaten aus Großbuchstaben in Kleinbuchstaben konvertiert wurden.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 80dba57d4fe05630eb5ae4f8fc96bd0aa214c6d4
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "78302269"
---
# <a name="lower-azure-cosmos-db"></a>LOWER (Azure Cosmos DB)
 Gibt eine Zeichenfolge zurück, nachdem Großbuchstaben in Kleinbuchstaben konvertiert wurden.  

Der Index wird von der Systemfunktion LOWER nicht verwendet. Wenn Sie häufig Vergleiche ohne Berücksichtigung der Groß-/Kleinschreibung durchführen möchten, beansprucht die Systemfunktion LOWER möglicherweise eine beträchtliche Anzahl von Anforderungseinheiten. Wenn dies der Fall ist, können Sie statt der Verwendung der Systemfunktion LOWER zum Normalisieren der Daten bei jedem Vergleich die Groß-/Kleinschreibung beim Einfügen normalisieren. Dann wird eine Abfrage wie SELECT * FROM c WHERE LOWER(c.name) = 'bob' einfach SELECT * FROM c WHERE c.name = 'bob'.

## <a name="syntax"></a>Syntax
  
```sql
LOWER(<str_expr>)  
```  
  
## <a name="arguments"></a>Argumente
  
*str_expr*  
   Ist ein Zeichenfolgenausdruck.  
  
## <a name="return-types"></a>Rückgabetypen
  
  Gibt einen Zeichenfolgenausdruck zurück.  
  
## <a name="examples"></a>Beispiele
  
  Das folgende Beispiel zeigt, wie `LOWER` in einer Abfrage verwendet wird.  
  
```sql
SELECT LOWER("Abc") AS lower
```  
  
 Hier ist das Resultset.  
  
```json
[{"lower": "abc"}]  
  
```  

## <a name="remarks"></a>Bemerkungen

Der Index wird von dieser Systemfunktion nicht verwendet.

## <a name="next-steps"></a>Nächste Schritte

- [Zeichenfolgenfunktionen in Azure Cosmos DB](sql-query-string-functions.md)
- [Systemfunktionen in Azure Cosmos DB](sql-query-system-functions.md)
- [Einführung in Azure Cosmos DB](introduction.md)
