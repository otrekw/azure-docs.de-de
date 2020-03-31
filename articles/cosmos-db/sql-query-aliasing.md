---
title: Aliase in Azure Cosmos DB
description: Erfahren Sie, wie Sie Aliase in SQL-Abfragen von Azure Cosmos DB verwenden, um zwei Eigenschaften mit demselben Namen zu unterscheiden.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 12/02/2019
ms.author: mjbrown
ms.openlocfilehash: 74849eec4c5808a584894321269c49c41f0b8a5c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "74873470"
---
# <a name="aliasing-in-azure-cosmos-db"></a>Aliase in Azure Cosmos DB

Sie können explizit Aliase für Werte in Abfragen verwenden. Wenn eine Abfrage zwei Eigenschaften mit demselben Namen hat, verwenden Sie Aliase, um mindestens eine der Eigenschaften umzubenennen, sodass sie im projizierten Ergebnis eindeutig sind.

## <a name="examples"></a>Beispiele

Das für die Aliasvergabe verwendete AS-Schlüsselwort ist optional, wie es im folgenden Beispiel bei der Projektion des zweiten Werts als `NameInfo` gezeigt wird:

```sql
    SELECT 
           { "state": f.address.state, "city": f.address.city } AS AddressInfo,
           { "name": f.id } NameInfo
    FROM Families f
    WHERE f.id = "AndersenFamily"
```

Die Ergebnisse sind:

```json
    [{
      "AddressInfo": {
        "state": "WA",
        "city": "Seattle"
      },
      "NameInfo": {
        "name": "AndersenFamily"
      }
    }]
```

## <a name="next-steps"></a>Nächste Schritte

- [Azure Cosmos DB-.NET-Beispiele](https://github.com/Azure/azure-cosmos-dotnet-v3)
- [SELECT-Klausel](sql-query-select.md)
- [FROM-Klausel](sql-query-from.md)
