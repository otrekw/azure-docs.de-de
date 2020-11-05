---
title: Aggregatfunktionen in Azure Cosmos DB
description: Informationen zur Syntax von SQL-Aggregatfunktionen und Typen von Aggregatfunktionen, die von Azure Cosmos DB unterstützt werden.
author: timsander1
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: conceptual
ms.date: 03/16/2020
ms.author: tisande
ms.openlocfilehash: 7c988f379e94bf2f69854c90d45af42fe2a7ec4f
ms.sourcegitcommit: fa90cd55e341c8201e3789df4cd8bd6fe7c809a3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2020
ms.locfileid: "93332779"
---
# <a name="aggregate-functions-in-azure-cosmos-db"></a>Aggregatfunktionen in Azure Cosmos DB
[!INCLUDE[appliesto-sql-api](includes/appliesto-sql-api.md)]

Aggregatfunktionen führen eine Berechnung für eine Gruppe von Werten in der `SELECT`-Klausel durch und geben einen einzelnen Wert zurück. Bei der folgenden Abfrage wird beispielsweise die Anzahl von Elementen im `Families`-Container zurückgegeben:

## <a name="examples"></a>Beispiele

```sql
    SELECT COUNT(1)
    FROM Families f
```

Die Ergebnisse sind:

```json
    [{
        "$1": 2
    }]
```

Sie können auch nur den skalaren Wert des Aggregats zurückgeben, indem Sie das Schlüsselwort VALUE verwenden. Bei dieser Abfrage wird beispielsweise die Anzahl der Werte als einzelne Zahl zurückgegeben:

```sql
    SELECT VALUE COUNT(1)
    FROM Families f
```

Die Ergebnisse sind:

```json
    [ 2 ]
```

Sie können auch Aggregationen mit Filtern kombinieren. Bei der folgenden Abfrage wird beispielsweise die Anzahl von Elementen mit dem Bundesstaat `WA` in der Adresse zurückgegeben.

```sql
    SELECT VALUE COUNT(1)
    FROM Families f
    WHERE f.address.state = "WA"
```

Die Ergebnisse sind:

```json
    [ 1 ]
```

## <a name="types-of-aggregate-functions"></a>Typen von Aggregatfunktionen

Die SQL-API unterstützt die folgenden Aggregatfunktionen. `SUM` und `AVG` werden für numerische Werte verwendet. `COUNT`, `MIN` und `MAX` werden für Zahlen, Zeichenfolgen, boolesche Werte und NULL-Werte verwendet.

| Funktion | BESCHREIBUNG |
|-------|-------------|
| COUNT | Gibt die Anzahl der Elemente im Ausdruck zurück. |
| SUM   | Gibt die Summe aller Werte im Ausdruck zurück. |
| MIN   | Gibt den kleinsten Wert im Ausdruck zurück. |
| MAX   | Gibt den größten Wert im Ausdruck zurück. |
| DURCHSCHN.   | Gibt den Durchschnitt aller Werte im Ausdruck zurück. |

Sie können auch die Ergebnisse einer Array-Iteration aggregieren.

> [!NOTE]
> Im Daten-Explorer des Azure-Portals geben Aggregationsabfragen möglicherweise die teilweise aggregierten Ergebnisse für nur eine Abfrageseite zurück. Das SDK erzeugt einen einzelnen kumulativen Wert für alle Seiten. Zum Durchführen von Aggregationsabfragen mithilfe von Code benötigen Sie .NET SDK 1.12.0, .NET Core SDK 1.1.0 oder Java SDK 1.9.5 oder höher.

## <a name="remarks"></a>Bemerkungen

Diese Aggregatsystemfunktionen profitierten von einem [Bereichsindex](index-policy.md#includeexclude-strategy). Wenn Sie `COUNT`, `SUM`, `MIN`, `MAX` oder `AVG` für eine Eigenschaft erwarten, sollten Sie [den relevanten Pfad in die Indizierungsrichtlinie einfügen](index-policy.md#includeexclude-strategy).

## <a name="next-steps"></a>Nächste Schritte

- [Einführung in Azure Cosmos DB](introduction.md)
- [System functions (Systemfunktionen)](sql-query-system-functions.md)
- [User defined functions (UDFs) in Azure Cosmos DB (Benutzerdefinierte Funktionen (UDFs) in Azure Cosmos DB)](sql-query-udfs.md)