---
title: Verwenden von dynamischem SQL
description: Tipps für Entwicklungslösungen mit dynamischem SQL in Synapse SQL-Pools.
services: synapse-analytics
author: XiaoyuMSFT
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: sql-dw
ms.date: 04/17/2018
ms.author: xiaoyul
ms.reviewer: igorstan
ms.custom: seo-lt-2019
ms.openlocfilehash: bce79b8e18b3ec6f1fd139af280086281bbdda98
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "85213464"
---
# <a name="dynamic-sql-in-synapse-sql-pool"></a>Dynamisches SQL im Synapse SQL-Pool

In diesem Artikel finden Sie Tipps für Entwicklungslösungen mit dynamischem SQL in SQL-Pools.

## <a name="dynamic-sql-example"></a>Beispiel für dynamisches SQL

Beim Entwickeln von Anwendungscode für einen SQL-Pool müssen Sie unter Umständen dynamisches SQL verwenden, um flexible, generische und modulare Lösungen bereitstellen zu können. SQL-Pools unterstützen zurzeit keine Blobdatentypen.

Dass keine Blobdatentypen unterstützt werden, könnte die Einschränkung der Größe Ihrer Zeichenfolgen zur Folge haben, da Blobdatentypen sowohl den Typ VARCHAR(MAX) als auch NVARCHAR(MAX) enthalten.

Wenn Sie diese Typen im Anwendungscode zum Erstellen großer Zeichenfolgen verwendet haben, müssen Sie den Code in Blöcke unterteilen und stattdessen die EXEC-Anweisung verwenden.

Ein einfaches Beispiel:

```sql
DECLARE @sql_fragment1 VARCHAR(8000)=' SELECT name '
,       @sql_fragment2 VARCHAR(8000)=' FROM sys.system_views '
,       @sql_fragment3 VARCHAR(8000)=' WHERE name like ''%table%''';

EXEC( @sql_fragment1 + @sql_fragment2 + @sql_fragment3);
```

Wenn die Zeichenfolge kurz ist, können Sie [sp_executesql](/sql/relational-databases/system-stored-procedures/sp-executesql-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) wie üblich verwenden.

> [!NOTE]
> Anweisungen, die als dynamisches SQL ausgeführt werden, unterliegen weiterhin allen T-SQL-Validierungsregeln.

## <a name="next-steps"></a>Nächste Schritte

Weitere Hinweise zur Entwicklung finden Sie in der [Entwicklungsübersicht](sql-data-warehouse-overview-develop.md).
