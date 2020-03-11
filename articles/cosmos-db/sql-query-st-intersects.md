---
title: ST_INTERSECTS in der Abfragesprache für Azure Cosmos DB
description: Erfahren Sie mehr über die SQL-Systemfunktion ST_INTERSECTS in Azure Cosmos DB.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: f3c3878956b90ffb45556ed819046af9eb7618f1
ms.sourcegitcommit: f915d8b43a3cefe532062ca7d7dbbf569d2583d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/05/2020
ms.locfileid: "78303136"
---
# <a name="st_intersects-azure-cosmos-db"></a>ST_INTERSECTS (Azure Cosmos DB)
 Gibt einen booleschen Ausdruck zurück, der angibt, ob das im ersten Argument angegebene GeoJSON-Objekt (Punkt, Polygon oder LineString) sich mit dem GeoJSON-Objekt (Punkt, Polygon oder LineString) im zweiten Argument überschneidet.  
  
## <a name="syntax"></a>Syntax
  
```sql
ST_INTERSECTS (<spatial_expr>, <spatial_expr>)  
```  
  
## <a name="arguments"></a>Argumente
  
*spatial_expr*  
   Ist ein GeoJSON-Point-, -Polygon- oder -LineString-Objektausdruck.  
  
## <a name="return-types"></a>Rückgabetypen
  
  Gibt einen booleschen Wert zurück.  
  
## <a name="examples"></a>Beispiele
  
  Das folgende Beispiel zeigt, wie alle Bereiche gefunden werden, die sich mit dem angegebenen Polygon überschneiden.  
  
```sql
SELECT a.id
FROM Areas a
WHERE ST_INTERSECTS(a.location, {  
    'type':'Polygon',
    'coordinates': [[[31.8, -5], [32, -5], [32, -4.7], [31.8, -4.7], [31.8, -5]]]  
})  
```  
  
 Hier ist das Resultset.  
  
```json
[{ "id": "IntersectingPolygon" }]  
```  

## <a name="remarks"></a>Bemerkungen

Diese Systemfunktion profitiert von einem [räumlichen Index](index-policy.md#spatial-indexes).

## <a name="next-steps"></a>Nächste Schritte

- [Räumliche Funktionen in Azure Cosmos DB](sql-query-spatial-functions.md)
- [Systemfunktionen in Azure Cosmos DB](sql-query-system-functions.md)
- [Einführung in Azure Cosmos DB](introduction.md)
