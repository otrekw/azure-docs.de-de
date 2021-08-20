---
title: Typüberprüfungsfunktionen in der Abfragesprache für Azure Cosmos DB
description: Erfahren Sie mehr über SQL-Systemfunktionen zur Typüberprüfung in Azure Cosmos DB.
author: timsander1
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: conceptual
ms.date: 05/26/2021
ms.author: tisande
ms.custom: query-reference
ms.openlocfilehash: 61dd347254a6e691250172cd428d8ef88f13e800
ms.sourcegitcommit: 2d412ea97cad0a2f66c434794429ea80da9d65aa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/14/2021
ms.locfileid: "122350683"
---
# <a name="type-checking-functions-azure-cosmos-db"></a>Typüberprüfungsfunktionen (Azure Cosmos DB)
[!INCLUDE[appliesto-sql-api](../includes/appliesto-sql-api.md)]

Mit den Funktionen für die Typprüfung können Sie den Typ eines Ausdrucks in einer SQL-Abfrage prüfen. Anhand von Typprüfungsfunktionen können Sie während der Ausführung die Typen von Eigenschaften innerhalb von Elementen ermitteln, wenn diese variabel oder unbekannt sind. 

## <a name="functions"></a>Functions

Die folgenden Funktionen unterstützen die Typüberprüfung für Eingabewerte und geben jeweils einen booleschen Wert zurück. In der Spalte für die **Indexverwendung** wird ggf. davon ausgegangen, dass Sie die Typüberprüfungsfunktionen mit einem Gleichheitsfilter mit einem anderen Wert vergleichen.

| Systemfunktion                           | Indexnutzung | [Indexnutzung in Abfragen mit skalaren Aggregatfunktionen](../index-overview.md#index-utilization-for-scalar-aggregate-functions) | Bemerkungen |
| ----------------------------------------- | ----------- | ------------------------------------------------------------ | ------- |
| [IS_ARRAY](sql-query-is-array.md)         | Vollständige Überprüfung   | Vollständige Überprüfung                                                    |         |
| [IS_BOOL](sql-query-is-bool.md)           | Indexsuche  | Indexsuche                                                   |         |
| [IS_DEFINED](sql-query-is-defined.md)     | Indexsuche  | Indexsuche                                                   |         |
| [IS_NULL](sql-query-is-null.md)           | Indexsuche  | Indexsuche                                                   |         |
| [IS_NUMBER](sql-query-is-number.md)       | Indexsuche  | Indexsuche                                                   |         |
| [IS_OBJECT](sql-query-is-object.md)       | Vollständige Überprüfung   | Vollständige Überprüfung                                                    |         |
| [IS_PRIMITIVE](sql-query-is-primitive.md) | Indexsuche  | Indexsuche                                                   |         |
| [IS_STRING](sql-query-is-string.md)       | Indexsuche  | Indexsuche                                                   |         

## <a name="next-steps"></a>Nächste Schritte

- [Systemfunktionen in Azure Cosmos DB](sql-query-system-functions.md)
- [Einführung in Azure Cosmos DB](../introduction.md)
- [Benutzerdefinierte Funktionen](sql-query-udfs.md)
- [Aggregate](sql-query-aggregate-functions.md)
