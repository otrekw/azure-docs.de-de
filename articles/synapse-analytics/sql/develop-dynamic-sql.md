---
title: Verwenden von dynamischen SQL in Synapse SQL
description: Tipps zur Verwendung von dynamischem SQL in Synapse SQL.
services: synapse-analytics
author: filippopovic
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: sql
ms.date: 04/15/2020
ms.author: fipopovi
ms.reviewer: jrasnick
ms.custom: ''
ms.openlocfilehash: 4a4ce92278d5f6704fec4a5b9d30b44ddf89b0ad
ms.sourcegitcommit: 3fc3457b5a6d5773323237f6a06ccfb6955bfb2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/11/2020
ms.locfileid: "90033437"
---
# <a name="dynamic-sql-in-synapse-sql"></a>Dynamisches SQL in Synapse SQL
In diesem Artikel finden Sie Tipps zur Verwendung von dynamischem SQL und zur Entwicklung von Lösungen mit Synapse SQL.

## <a name="dynamic-sql-example"></a>Beispiel für dynamisches SQL

Beim Entwickeln von Anwendungscode müssen Sie unter Umständen dynamisches SQL verwenden, um flexible, generische und modulare Lösungen bereitstellen zu können.

> [!NOTE]
> SQL-Pools unterstützen zurzeit keine Blobdatentypen. Dass keine Blobdatentypen unterstützt werden, könnte die Einschränkung der Größe Ihrer Zeichenfolgen zur Folge haben, da Blobdatentypen sowohl den Typ VARCHAR(MAX) als auch NVARCHAR(MAX) enthalten. Wenn Sie diese Typen im Anwendungscode zum Erstellen großer Zeichenfolgen verwendet haben, müssen Sie den Code in Segmente unterteilen und stattdessen die EXEC-Anweisung verwenden.

Ein einfaches Beispiel:

```sql
DECLARE @sql_fragment1 VARCHAR(8000)=' SELECT name '
,       @sql_fragment2 VARCHAR(8000)=' FROM sys.system_views '
,       @sql_fragment3 VARCHAR(8000)=' WHERE name like ''%table%''';

EXEC( @sql_fragment1 + @sql_fragment2 + @sql_fragment3);
```

Wenn die Zeichenfolge kurz ist, können Sie [sp_executesql](/sql/relational-databases/system-stored-procedures/sp-executesql-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) wie üblich verwenden.

> [!NOTE]
> Anweisungen, die als dynamisches SQL ausgeführt werden, unterliegen weiterhin allen T-SQL-Validierungsregeln.

## <a name="next-steps"></a>Nächste Schritte

Weitere Hinweise zur Entwicklung finden Sie in der [Entwicklungsübersicht](develop-overview.md).
