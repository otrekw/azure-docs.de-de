---
title: Mathematische Funktionen in der Abfragesprache für Azure Cosmos DB
description: Erfahren Sie etwas über die mathematischen Funktionen in Azure Cosmos DB, mit denen Berechnungen basierend auf Eingabewerten, die als Argument bereitgestellt werden und einen numerischen Wert zurückgeben, durchgeführt werden.
author: ginamr
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: conceptual
ms.date: 06/22/2021
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 9e3114220b6250afb9d633b073342ba6f2cf92b8
ms.sourcegitcommit: 2d412ea97cad0a2f66c434794429ea80da9d65aa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/14/2021
ms.locfileid: "122356329"
---
# <a name="mathematical-functions-azure-cosmos-db"></a>Mathematische Funktionen (Azure Cosmos DB)  
[!INCLUDE[appliesto-sql-api](../includes/appliesto-sql-api.md)]

Jede mathematische Funktion führt eine Berechnung durch, basierend auf Eingabewerten, die als Argument bereitgestellt werden, und gibt einen numerischen Wert zurück.

Sie können Abfragen wie im folgenden Beispiel ausführen:

```sql
    SELECT VALUE ABS(-4)
```

Es wird folgendes Ergebnis ausgegeben:

```json
    [4]
```

## <a name="functions"></a>Functions

Die folgenden unterstützten integrierten mathematischen Funktionen führen eine Berechnung durch, die normalerweise auf Eingabeargumenten basiert, und geben einen numerischen Ausdruck zurück. In der Spalte für die **Indexverwendung** wird ggf. davon ausgegangen, dass Sie die mathematische Systemfunktion mit einem anderen Wert mit einem Gleichheitsfilter vergleichen.
 
| Systemfunktion                 | Indexnutzung | [Indexnutzung in Abfragen mit skalaren Aggregatfunktionen](../index-overview.md#index-utilization-for-scalar-aggregate-functions) | Bemerkungen                                                      |
| ------------------------------- | ----------- | ------------------------------------------------------ | ------------------------------------------------------------ |
| [ABS](sql-query-abs.md)         | Indexsuche  | Indexsuche                                             |                                                              |
| [ACOS](sql-query-acos.md)       | Vollständige Überprüfung   | Vollständige Überprüfung                                              |                                                              |
| [ASIN](sql-query-asin.md)       | Vollständige Überprüfung   | Vollständige Überprüfung                                              |                                                              |
| [ATAN](sql-query-atan.md)       | Vollständige Überprüfung   | Vollständige Überprüfung                                              |                                                              |
| [ATN2](sql-query-atn2.md)       | Vollständige Überprüfung   | Vollständige Überprüfung                                              |                                                              |
| [CEILING](sql-query-ceiling.md) | Indexsuche  | Indexsuche                                             |                                                              |
| [COS](sql-query-cos.md)         | Vollständige Überprüfung   | Vollständige Überprüfung                                              |                                                              |
| [COT](sql-query-cot.md)         | Vollständige Überprüfung   | Vollständige Überprüfung                                              |                                                              |
| [DEGREES](sql-query-degrees.md) | Indexsuche  | Indexsuche                                             |                                                              |
| [EXP](sql-query-exp.md)         | Vollständige Überprüfung   | Vollständige Überprüfung                                              |                                                              |
| [FLOOR](sql-query-floor.md)     | Indexsuche  | Indexsuche                                             |                                                              |
| [LOG](sql-query-log.md)         | Vollständige Überprüfung   | Vollständige Überprüfung                                              |                                                              |
| [LOG10](sql-query-log10.md)     | Vollständige Überprüfung   | Vollständige Überprüfung                                              |                                                              |
| [PI](sql-query-pi.md)           | –         | –                                                    | PI () gibt einen konstanten Wert zurück. Da das Ergebnis deterministisch ist, können Vergleiche mit PI() den Index verwenden. |
| [POWER](sql-query-power.md)     | Vollständige Überprüfung   | Vollständige Überprüfung                                              |                                                              |
| [RADIANS](sql-query-radians.md) | Indexsuche  | Indexsuche                                             |                                                              |
| [RAND](sql-query-rand.md)       | –         | –                                                    | Rand() gibt eine Zufallszahl zurück. Da das Ergebnis nicht deterministisch ist, können Vergleiche mit Rand() den Index nicht verwenden. |
| [ROUND](sql-query-round.md)     | Indexsuche  | Indexsuche                                             |                                                              |
| [SIGN](sql-query-sign.md)       | Indexsuche  | Indexsuche                                             |                                                              |
| [SIN](sql-query-sin.md)         | Vollständige Überprüfung   | Vollständige Überprüfung                                              |                                                              |
| [SQRT](sql-query-sqrt.md)       | Vollständige Überprüfung   | Vollständige Überprüfung                                              |                                                              |
| [SQUARE](sql-query-square.md)   | Vollständige Überprüfung   | Vollständige Überprüfung                                              |                                                              |
| [TAN](sql-query-tan.md)         | Vollständige Überprüfung   | Vollständige Überprüfung                                              |                                                              |
| [TRUNC](sql-query-trunc.md)     | Indexsuche  | Indexsuche                                              |                                                              |
## <a name="next-steps"></a>Nächste Schritte

- [Systemfunktionen in Azure Cosmos DB](sql-query-system-functions.md)
- [Einführung in Azure Cosmos DB](../introduction.md)
- [Benutzerdefinierte Funktionen](sql-query-udfs.md)
- [Aggregate](sql-query-aggregate-functions.md)
