---
title: Arbeiten mit Arrays und Objekten in Azure Cosmos DB
description: Hier erfahren Sie mehr über die SQL-Syntax zum Erstellen von Arrays und Objekten in Azure Cosmos DB. Außerdem enthält dieser Artikel einige Beispiele zum Ausführen von Vorgängen für Arrayobjekte.
author: timsander1
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 12/02/2019
ms.author: tisande
ms.openlocfilehash: 5b2801b0a71f04803955e9d8bc18a97133019996
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2020
ms.locfileid: "87090772"
---
# <a name="working-with-arrays-and-objects-in-azure-cosmos-db"></a>Arbeiten mit Arrays und Objekten in Azure Cosmos DB

Eine Schlüsselfunktion der SQL-API in Azure Cosmos DB ist die Array- und Objekterstellung.

## <a name="arrays"></a>Arrays

Sie können Arrays wie im folgenden Beispiel gezeigt erstellen:

```sql
    SELECT [f.address.city, f.address.state] AS CityState
    FROM Families f
```

Die Ergebnisse sind:

```json
    [
      {
        "CityState": [
          "Seattle",
          "WA"
        ]
      },
      {
        "CityState": [
          "NY", 
          "NY"
        ]
      }
    ]
```

Sie können auch den [ARRAY-Ausdruck](sql-query-subquery.md#array-expression) verwenden, um ein Array aus den Ergebnissen der [Unterabfrage](sql-query-subquery.md) zu erstellen. Diese Abfrage ruft alle unterschiedlichen Vornamen von Kindern in einem Array ab.

```sql
SELECT f.id, ARRAY(SELECT DISTINCT VALUE c.givenName FROM c IN f.children) as ChildNames
FROM f
```

## <a name="iteration"></a><a id="Iteration"></a>Iteration

Die SQL-API unterstützt eine Iteration über JSON-Arrays mit einem neuen Konstrukt, das mit dem [IN-Schlüsselwort](sql-query-keywords.md#in) in der FROM-Quelle hinzugefügt wurde. Siehe folgendes Beispiel:

```sql
    SELECT *
    FROM Families.children
```

Die Ergebnisse sind:

```json
    [
      [
        {
          "firstName": "Henriette Thaulow",
          "gender": "female",
          "grade": 5,
          "pets": [{ "givenName": "Fluffy"}]
        }
      ], 
      [
        {
            "familyName": "Merriam",
            "givenName": "Jesse",
            "gender": "female",
            "grade": 1
        }, 
        {
            "familyName": "Miller",
            "givenName": "Lisa",
            "gender": "female",
            "grade": 8
        }
      ]
    ]
```

Die nächste Abfrage führt eine Iteration über `children` im Container `Families` aus. Das Ausgabearray unterscheidet sich von der vorherigen Abfrage. Dieses Beispiel teilt `children` und fügt das Ergebnis in ein einzelnes Array ein:  

```sql
    SELECT *
    FROM c IN Families.children
```

Die Ergebnisse sind:

```json
    [
      {
          "firstName": "Henriette Thaulow",
          "gender": "female",
          "grade": 5,
          "pets": [{ "givenName": "Fluffy" }]
      },
      {
          "familyName": "Merriam",
          "givenName": "Jesse",
          "gender": "female",
          "grade": 1
      },
      {
          "familyName": "Miller",
          "givenName": "Lisa",
          "gender": "female",
          "grade": 8
      }
    ]
```

Sie können die einzelnen Einträge im Array weiter filtern wie im folgenden Beispiel gezeigt:

```sql
    SELECT c.givenName
    FROM c IN Families.children
    WHERE c.grade = 8
```

Die Ergebnisse sind:

```json
    [{
      "givenName": "Lisa"
    }]
```

Sie können auch das Ergebnis einer Array-Iteration aggregieren. Die folgende Abfrage zählt beispielsweise die Anzahl der Kinder in allen Familien:

```sql
    SELECT COUNT(child)
    FROM child IN Families.children
```

Die Ergebnisse sind:

```json
    [
      {
        "$1": 3
      }
    ]
```

## <a name="next-steps"></a>Nächste Schritte

- [Erste Schritte](sql-query-getting-started.md)
- [Azure Cosmos DB-.NET-Beispiele](https://github.com/Azure/azure-cosmos-dotnet-v3)
- [Joins](sql-query-join.md)