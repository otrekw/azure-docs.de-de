---
title: Definieren von Datentypen
description: Empfehlungen zum Definieren von Tabellendatentypen in einem Synapse SQL-Pool.
services: synapse-analytics
author: filippopovic
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: sql-dw
ms.date: 01/06/2020
ms.author: fipopovi
ms.reviewer: jrasnick
ms.custom: ''
ms.openlocfilehash: 8313a26c32620c3077433a8cc0570b360bc5b6bf
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85204964"
---
# <a name="table-data-types-in-synapse-sql-pool"></a>Tabellendatentypen in einem Synapse SQL-Pool

In diesem Artikel finden Sie Empfehlungen zum Definieren von Tabellendatentypen in einem SQL-Pool.

## <a name="supported-data-types"></a>Unterstützte Datentypen

Ein SQL-Pool unterstützt die am häufigsten verwendeten Datentypen. Eine Liste der unterstützten Datentypen finden Sie in der CREATE TABLE-Anweisung unter [Datentypen](/sql/t-sql/statements/create-table-azure-sql-data-warehouse?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest#DataTypes).

## <a name="minimize-row-length"></a>Minimieren der Zeilenlänge

Durch das Minimieren der Größe von Datentypen wird die Zeilenlänge verkürzt. Dies führt zu einer besseren Abfrageleistung. Verwenden Sie den kleinsten Datentyp, der für Ihre Daten funktioniert.

- Vermeiden Sie es, für Zeichenspalten eine hohe Standardlänge zu definieren. Wenn der längste Wert 25 Zeichen umfasst, sollten Sie die Spalte VARCHAR(25) definieren.
- Vermeiden Sie die Verwendung von [NVARCHAR](/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest), wenn Sie nur VARCHAR benötigen.
- Verwenden Sie anstelle von NVARCHAR(MAX) oder VARCHAR(MAX) nach Möglichkeit NVARCHAR(4000) oder VARCHAR(8000).

Wenn Sie die Tabellen mit externen PolyBase-Tabellen laden, darf die definierte Länge der Tabellenzeile 1 MB nicht überschreiten. Wenn eine Zeile mit Daten variabler Länge 1 MB überschreitet, können Sie die Zeile mit BCP, jedoch nicht mit PolyBase laden.

## <a name="identify-unsupported-data-types"></a>Identifizieren nicht unterstützter Datentypen

Wenn Sie die Datenbank von einer anderen SQL-Datenbank migrieren, kommen unter Umständen einige Datentypen vor, die in einem SQL-Pool nicht unterstützt werden. Verwenden Sie die folgende Abfrage, um nicht unterstützte Datentypen in Ihrem vorhandenen SQL-Schema zu ermitteln:

```sql
SELECT  t.[name], c.[name], c.[system_type_id], c.[user_type_id], y.[is_user_defined], y.[name]
FROM sys.tables  t
JOIN sys.columns c on t.[object_id]    = c.[object_id]
JOIN sys.types   y on c.[user_type_id] = y.[user_type_id]
WHERE y.[name] IN ('geography','geometry','hierarchyid','image','text','ntext','sql_variant','xml')
 AND  y.[is_user_defined] = 1;
```

## <a name="workarounds-for-unsupported-data-types"></a><a name="unsupported-data-types"></a>Problemumgehungen für nicht unterstützte Datentypen

In der folgenden Liste sind die Datentypen aufgeführt, die der SQL-Pool nicht unterstützt. Außerdem werden nützliche Alternativen für nicht unterstützte Datentypen angegeben.

| Nicht unterstützte Datentypen | Problemumgehung |
| --- | --- |
| [geometry](/sql/t-sql/spatial-geometry/spatial-types-geometry-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) |[varbinary](/sql/t-sql/data-types/binary-and-varbinary-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) |
| [geography](/sql/t-sql/spatial-geography/spatial-types-geography) |[varbinary](/sql/t-sql/data-types/binary-and-varbinary-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) |
| [hierarchyid](/sql/t-sql/data-types/hierarchyid-data-type-method-reference) |[nvarchar](/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)(4000) |
| [image](/sql/t-sql/data-types/ntext-text-and-image-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) |[varbinary](/sql/t-sql/data-types/binary-and-varbinary-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) |
| [text](/sql/t-sql/data-types/ntext-text-and-image-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) |[varchar](/sql/t-sql/data-types/char-and-varchar-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) |
| [ntext](/sql/t-sql/data-types/ntext-text-and-image-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) |[nvarchar](/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) |
| [sql_variant](/sql/t-sql/data-types/sql-variant-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) |Spalte in mehrere Spalten mit starker Typisierung unterteilen. |
| [Tabelle](/sql/t-sql/data-types/table-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) |In temporäre Tabellen konvertieren. |
| [timestamp](/sql/t-sql/data-types/date-and-time-types) |Überarbeiten Sie Code mit [datetime2](/sql/t-sql/data-types/datetime2-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) und der [CURRENT_TIMESTAMP](/sql/t-sql/functions/current-timestamp-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)-Funktion. Nur Konstanten werden als Standardwerte unterstützt. Daher kann „current_timestamp“ nicht als Standardeinschränkung definiert werden. Wenn Sie Zeilenversionswerte aus einer Spalte vom Typ „timestamp“ migrieren müssen, sollten Sie [BINARY](/sql/t-sql/data-types/binary-and-varbinary-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)(8) oder [VARBINARY](/sql/t-sql/data-types/binary-and-varbinary-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)(8) für NOT NULL- oder NULL-Zeilenversionswerte verwenden. |
| [xml](/sql/t-sql/xml/xml-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) |[varchar](/sql/t-sql/data-types/char-and-varchar-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) |
| [Benutzerdefinierter Typ](/sql/relational-databases/native-client/features/using-user-defined-types?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) |Führen Sie, wenn möglich, eine Rückkonvertierung in den nativen Datentyp durch. |
| Standardwerte | Standardwerte unterstützen nur Literale und Konstanten. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Entwickeln von Tabellen finden Sie unter [Einführung in das Entwerfen von Tabellen in Azure SQL Data Warehouse](sql-data-warehouse-tables-overview.md).
