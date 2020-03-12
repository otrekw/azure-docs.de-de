---
title: Aggregatfunktionen in Azure Cosmos DB
description: Informationen zur Syntax von SQL-Aggregatfunktionen und Typen von Aggregatfunktionen, die von Azure Cosmos DB unterstützt werden.
author: timsander1
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/05/2020
ms.author: tisande
ms.openlocfilehash: df9700dd51c8915ff28c34cf0a29c2f5e48baa44
ms.sourcegitcommit: 668b3480cb637c53534642adcee95d687578769a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/07/2020
ms.locfileid: "78897831"
---
# <a name="aggregate-functions-in-azure-cosmos-db"></a>Aggregatfunktionen in Azure Cosmos DB

Aggregatfunktionen führen eine Berechnung für eine Gruppe von Werten in der SELECT-Klausel durch und geben einen einzelnen Wert zurück. Bei der folgenden Abfrage wird beispielsweise die Anzahl von Elementen im `Families`-Container zurückgegeben:

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

Die SQL-API unterstützt die folgenden Aggregatfunktionen. SUM und AVG werden für numerische Werte ausgeführt. COUNT, MIN und MAX werden für Zahlen, Zeichenfolgen, boolesche Werte und NULL-Werte verwendet.

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

## <a name="next-steps"></a>Nächste Schritte

- [Einführung in Azure Cosmos DB](introduction.md)
- [System functions (Systemfunktionen)](sql-query-system-functions.md)
- [User defined functions (UDFs) in Azure Cosmos DB (Benutzerdefinierte Funktionen (UDFs) in Azure Cosmos DB)](sql-query-udfs.md)