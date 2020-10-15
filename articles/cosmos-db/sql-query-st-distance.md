---
title: ST_DISTANCE in der Abfragesprache für Azure Cosmos DB
description: Erfahren Sie mehr über die SQL-Systemfunktion ST_DISTANCE in Azure Cosmos DB.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/12/2020
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 02844569137a46ea030b2189191b84a9db24ed22
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "79537294"
---
# <a name="st_distance-azure-cosmos-db"></a>ST_DISTANCE (Azure Cosmos DB)
 Gibt den Abstand zwischen den beiden GeoJSON-Punkt-, Polygon-, MultiPolygon- oder LineString-Ausdrücken zurück. Weitere Informationen finden Sie im Artikel [Räumliche Daten und GeoJSON-Standortdaten in Azure Cosmos DB](sql-query-geospatial-intro.md).
  
## <a name="syntax"></a>Syntax
  
```sql
ST_DISTANCE (<spatial_expr>, <spatial_expr>)  
```  
  
## <a name="arguments"></a>Argumente
  
*spatial_expr*  
   Ist ein beliebiger gültiger GeoJSON-Punkt-, -Polygon- oder -LineString-Objektausdruck.  
  
## <a name="return-types"></a>Rückgabetypen
  
  Gibt einen numerischen Ausdruck zurück, der den Abstand enthält. Dieser wird für das Standardreferenzsystem in Metern angegeben.  
  
## <a name="examples"></a>Beispiele
  
  Das folgende Beispiel zeigt die Rückgabe aller Familiendokumente, die sich bis zu 30 km vom angegebenen Ort entfernt befinden. Dazu wird die integrierte `ST_DISTANCE`-Funktion verwendet. erforderlich.  
  
```sql
SELECT f.id
FROM Families f
WHERE ST_DISTANCE(f.location, {'type': 'Point', 'coordinates':[31.9, -4.8]}) < 30000  
```  
  
 Hier ist das Resultset.  
  
```json
[{  
  "id": "WakefieldFamily"  
}]  
```

## <a name="remarks"></a>Bemerkungen

Diese Systemfunktion profitiert von einem [räumlichen Index](index-policy.md#spatial-indexes).

## <a name="next-steps"></a>Nächste Schritte

- [Räumliche Funktionen in Azure Cosmos DB](sql-query-spatial-functions.md)
- [Systemfunktionen in Azure Cosmos DB](sql-query-system-functions.md)
- [Einführung in Azure Cosmos DB](introduction.md)
