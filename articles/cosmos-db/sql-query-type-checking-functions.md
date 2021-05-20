---
title: Typüberprüfungsfunktionen in der Abfragesprache für Azure Cosmos DB
description: Erfahren Sie mehr über SQL-Systemfunktionen zur Typüberprüfung in Azure Cosmos DB.
author: timsander1
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: conceptual
ms.date: 05/04/2021
ms.author: tisande
ms.custom: query-reference
ms.openlocfilehash: 0a97517e524bbd6f13a108562a85037ae9695187
ms.sourcegitcommit: 02d443532c4d2e9e449025908a05fb9c84eba039
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2021
ms.locfileid: "108768911"
---
# <a name="type-checking-functions-azure-cosmos-db"></a>Typüberprüfungsfunktionen (Azure Cosmos DB)
[!INCLUDE[appliesto-sql-api](includes/appliesto-sql-api.md)]

Mit den Funktionen für die Typprüfung können Sie den Typ eines Ausdrucks in einer SQL-Abfrage prüfen. Anhand von Typprüfungsfunktionen können Sie während der Ausführung die Typen von Eigenschaften innerhalb von Elementen ermitteln, wenn diese variabel oder unbekannt sind. 

## <a name="functions"></a>Functions

Die folgenden Funktionen unterstützen die Typüberprüfung für Eingabewerte und geben jeweils einen booleschen Wert zurück:

| Systemfunktion                           | Indexnutzung | [Indexnutzung in Abfragen mit skalaren Aggregatfunktionen](index-overview.md#index-utilization-for-scalar-aggregate-functions) | Bemerkungen |
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
- [Einführung in Azure Cosmos DB](introduction.md)
- [Benutzerdefinierte Funktionen](sql-query-udfs.md)
- [Aggregate](sql-query-aggregate-functions.md)
