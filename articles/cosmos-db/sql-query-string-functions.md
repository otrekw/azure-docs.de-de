---
title: Zeichenfolgenfunktionen in der Abfragesprache für Azure Cosmos DB
description: Erfahren Sie mehr über SQL-Systemfunktionen für Zeichenfolgen in Azure Cosmos DB.
author: timsander1
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: conceptual
ms.date: 05/04/2021
ms.author: tisande
ms.custom: query-reference
ms.openlocfilehash: 4fe60a119626dca51f5dd4f38a81f6a2ef99c83c
ms.sourcegitcommit: 02d443532c4d2e9e449025908a05fb9c84eba039
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2021
ms.locfileid: "108743721"
---
# <a name="string-functions-azure-cosmos-db"></a>Zeichenfolgenfunktionen (Azure Cosmos DB)
[!INCLUDE[appliesto-sql-api](includes/appliesto-sql-api.md)]

Mit Zeichenfolgenfunktionen können Sie Vorgänge für Zeichenfolgen in Azure Cosmos DB ausführen.

## <a name="functions"></a>Functions

Die folgenden Skalarfunktionen führen einen Vorgang für einen Zeichenfolgen-Eingabewert durch und geben eine Zeichenfolge, einen numerischen Wert oder einen booleschen Wert zurück:
  
| Systemfunktion                                 | Indexnutzung        | Indexnutzung in Abfragen mit [skalaren Aggregatfunktionen](index-overview.md#index-utilization-for-scalar-aggregate-functions) | Bemerkungen                                                      |
| ----------------------------------------------- | ------------------ | ------------------------------------------------------ | ------------------------------------------------------------ |
| [CONCAT](sql-query-concat.md)                   | Vollständige Überprüfung          | Vollständige Überprüfung                                              |                                                              |
| [CONTAINS](sql-query-contains.md)               | Vollständige Indexüberprüfung    | Vollständige Überprüfung                                              |                                                              |
| [ENDSWITH](sql-query-endswith.md)               | Vollständige Indexüberprüfung    | Vollständige Überprüfung                                              |                                                              |
| [INDEX_OF](sql-query-index-of.md)               | Vollständige Überprüfung          | Vollständige Überprüfung                                              |                                                              |
| [LEFT](sql-query-left.md)                       | Genaue Indexüberprüfung | Genaue Indexüberprüfung                                     |                                                              |
| [LENGTH](sql-query-length.md)                   | Vollständige Überprüfung          | Vollständige Überprüfung                                              |                                                              |
| [LOWER](sql-query-lower.md)                     | Vollständige Überprüfung          | Vollständige Überprüfung                                              |                                                              |
| [LTRIM](sql-query-ltrim.md)                     | Vollständige Überprüfung          | Vollständige Überprüfung                                              |                                                              |
| [REGEXMATCH](sql-query-regexmatch.md)           | Vollständige Indexüberprüfung    | Vollständige Überprüfung                                              |                                                              |
| [REPLACE](sql-query-replace.md)                 | Vollständige Überprüfung          | Vollständige Überprüfung                                              |                                                              |
| [REPLICATE](sql-query-replicate.md)             | Vollständige Überprüfung          | Vollständige Überprüfung                                              |                                                              |
| [REVERSE](sql-query-reverse.md)                 | Vollständige Überprüfung          | Vollständige Überprüfung                                              |                                                              |
| [RIGHT](sql-query-right.md)                     | Vollständige Überprüfung          | Vollständige Überprüfung                                              |                                                              |
| [RTRIM](sql-query-rtrim.md)                     | Vollständige Überprüfung          | Vollständige Überprüfung                                              |                                                              |
| [STARTSWITH](sql-query-startswith.md)           | Genaue Indexüberprüfung | Genaue Indexüberprüfung                                     | Wird zu „Erweiterte Indexüberprüfung“, wenn die Option „Keine Groß-/Kleinschreibung“ auf „true“ gesetzt ist. |
| [STRINGEQUALS](sql-query-stringequals.md)       | Indexsuche         | Indexsuche                                             | Wird zu „Erweiterte Indexüberprüfung“, wenn die Option „Keine Groß-/Kleinschreibung“ auf „true“ gesetzt ist. |
| [StringToArray](sql-query-stringtoarray.md)     | Vollständige Überprüfung          | Vollständige Überprüfung                                              |                                                              |
| [StringToBoolean](sql-query-stringtoboolean.md) | Vollständige Überprüfung          | Vollständige Überprüfung                                              |                                                              |
| [StringToNull](sql-query-stringtonull.md)       | Vollständige Überprüfung          | Vollständige Überprüfung                                              |                                                              |
| [StringToNumber](sql-query-stringtonumber.md)   | Vollständige Überprüfung          | Vollständige Überprüfung                                              |                                                              |

Erfahren Sie mehr über [Indexnutzung](index-overview.md#index-usage) in Azure Cosmos DB.

## <a name="next-steps"></a>Nächste Schritte

- [Systemfunktionen in Azure Cosmos DB](sql-query-system-functions.md)
- [Einführung in Azure Cosmos DB](introduction.md)
- [Benutzerdefinierte Funktionen](sql-query-udfs.md)
- [Aggregate](sql-query-aggregate-functions.md)
