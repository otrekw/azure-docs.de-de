---
title: OFFSET LIMIT-Klausel in Azure Cosmos DB
description: Hier erfahren Sie, wie Sie mit der OFFSET LIMIT-Klausel beim Abfragen in Azure Cosmos DB einige bestimmte Werte überspringen und übernehmen.
author: timsander1
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 06/10/2019
ms.author: mjbrown
ms.openlocfilehash: a8df220be211c3c8d8cdeab8a8aebfd35e77ebf8
ms.sourcegitcommit: c32050b936e0ac9db136b05d4d696e92fefdf068
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/08/2020
ms.locfileid: "75732585"
---
# <a name="offset-limit-clause-in-azure-cosmos-db"></a>OFFSET LIMIT-Klausel in Azure Cosmos DB

Die OFFSET LIMIT-Klausel ist eine optionale Klausel zum Überspringen einer bestimmten Anzahl von Werten aus der Abfrage. In der OFFSET LIMIT-Klausel müssen sowohl für OFFSET als auch für LIMIT Werte angegeben werden.

Wenn OFFSET LIMIT in Verbindung mit einer ORDER BY-Klausel verwendet wird, wird das Resultset erstellt, indem zunächst Werte übersprungen und dann die sortierten Werte angenommen werden. Wenn keine ORDER BY-Klausel verwendet wird, gilt eine deterministische Reihenfolge der Werte.

## <a name="syntax"></a>Syntax
  
```sql  
OFFSET <offset_amount> LIMIT <limit_amount>
```  
  
## <a name="arguments"></a>Argumente

- `<offset_amount>`

   Mit diesem Argument wird die ganzzahlige Anzahl von Elementen festgelegt, die die Abfrageergebnisse überspringen sollen.

- `<limit_amount>`
  
   Mit diesem Argument wird die ganzzahlige Anzahl von Elementen festgelegt, die die Abfrageergebnisse enthalten sollen.

## <a name="remarks"></a>Bemerkungen
  
  In der OFFSET LIMIT-Klausel müssen sowohl für OFFSET als auch für LIMIT Werte angegeben werden. Wenn eine optionale `ORDER BY`-Klausel verwendet wird, wird das Resultset durch Überspringen der sortierten Werte erzeugt. Andernfalls gibt die Abfrage eine feste Reihenfolge der Werte zurück. Diese Klausel wird jetzt für Abfragen in einer einzelnen Partition sowie für partitionsübergreifende Abfragen unterstützt.

## <a name="examples"></a>Beispiele

Dies ist z. B. eine Abfrage, die den ersten Wert überspringt und den zweiten Wert (in der Reihenfolge der Wohnortnamen) zurückgibt:

```sql
    SELECT f.id, f.address.city
    FROM Families f
    ORDER BY f.address.city
    OFFSET 1 LIMIT 1
```

Die Ergebnisse sind:

```json
    [
      {
        "id": "AndersenFamily",
        "city": "Seattle"
      }
    ]
```

Dies ist eine Abfrage, die den ersten Wert überspringt und den zweiten Wert (ohne Sortierung) zurückgibt:

```sql
   SELECT f.id, f.address.city
    FROM Families f
    OFFSET 1 LIMIT 1
```

Die Ergebnisse sind:

```json
    [
      {
        "id": "WakefieldFamily",
        "city": "Seattle"
      }
    ]
```

## <a name="next-steps"></a>Nächste Schritte

- [Erste Schritte](sql-query-getting-started.md)
- [SELECT-Klausel](sql-query-select.md)
- [ORDER BY-Klausel](sql-query-order-by.md)
