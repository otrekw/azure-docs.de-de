---
title: Tabellendatentypen in Synapse SQL
description: Empfehlungen zum Definieren von Tabellendatentypen in Synapse SQL
services: synapse-analytics
author: filippopovic
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 04/15/2020
ms.author: fipopovi
ms.reviewer: jrasnick
ms.custom: ''
ms.openlocfilehash: a92d73941aac045534b6708c4b92d741c39ff781
ms.sourcegitcommit: 11e2521679415f05d3d2c4c49858940677c57900
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/31/2020
ms.locfileid: "87494966"
---
# <a name="table-data-types-in-synapse-sql"></a>Tabellendatentypen in Synapse SQL

Empfehlungen zum Definieren von Tabellendatentypen in Synapse SQL 

## <a name="data-types"></a>Datentypen

Synapse SQL unterstützt die am häufigsten verwendeten Datentypen. Eine Liste der unterstützten Datentypen finden Sie in der CREATE TABLE-Anweisung unter [Datentypen](/sql/t-sql/statements/create-table-azure-sql-data-warehouse#DataTypes). 

## <a name="minimize-row-length"></a>Minimieren der Zeilenlänge

Durch das Minimieren der Größe von Datentypen wird die Zeilenlänge verkürzt. Dies führt zu einer besseren Abfrageleistung. Verwenden Sie den kleinsten Datentyp, der für Ihre Daten funktioniert.

- Vermeiden Sie es, für Zeichenspalten eine hohe Standardlänge zu definieren. Wenn der längste Wert 25 Zeichen umfasst, sollten Sie die Spalte VARCHAR(25) definieren.
- Vermeiden Sie die Verwendung von [NVARCHAR][NVARCHAR], wenn Sie nur VARCHAR benötigen.
- Verwenden Sie anstelle von NVARCHAR(MAX) oder VARCHAR(MAX) nach Möglichkeit NVARCHAR(4000) oder VARCHAR(8000).

> [!NOTE]
> Wenn Sie die SQL-Pooltabellen mit externen PolyBase-Tabellen laden, darf die definierte Länge der Tabellenzeile 1 MB nicht überschreiten. Wenn eine Zeile mit Daten variabler Länge 1 MB überschreitet, können Sie die Zeile mit BCP, jedoch nicht mit PolyBase laden.

## <a name="identify-unsupported-data-types"></a>Identifizieren nicht unterstützter Datentypen

Wenn Sie die Datenbank von einer anderen SQL-Datenbank migrieren, kommen unter Umständen einige Datentypen vor, die in Synapse SQL nicht unterstützt werden. Verwenden Sie diese Abfrage, um nicht unterstützte Datentypen in Ihrem vorhandenen SQL-Schema zu ermitteln.

```sql
SELECT  t.[name], c.[name], c.[system_type_id], c.[user_type_id], y.[is_user_defined], y.[name]
FROM sys.tables  t
JOIN sys.columns c on t.[object_id]    = c.[object_id]
JOIN sys.types   y on c.[user_type_id] = y.[user_type_id]
WHERE y.[name] IN ('geography','geometry','hierarchyid','image','text','ntext','sql_variant','xml')
 AND  y.[is_user_defined] = 1;
```

## <a name="workarounds-for-unsupported-data-types"></a><a name="unsupported-data-types"></a>Problemumgehungen für nicht unterstützte Datentypen

Die folgende Liste zeigt die Datentypen, die nicht von Synapse SQL unterstützt werden, und bietet Alternativen, die Sie anstelle der nicht unterstützten Datentypen verwenden können.

| Nicht unterstützte Datentypen | Problemumgehung |
| --- | --- |
| [geometry](/sql/t-sql/spatial-geometry/spatial-types-geometry-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) |[varbinary](/sql/t-sql/data-types/binary-and-varbinary-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) |
| [geography](/sql/t-sql/spatial-geography/spatial-types-geography) |[varbinary](/sql/t-sql/data-types/binary-and-varbinary-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) |
| [hierarchyid](/sql/t-sql/data-types/hierarchyid-data-type-method-reference) |[nvarchar](/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)(4000) |
| [image](/sql/t-sql/data-types/ntext-text-and-image-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) |[varbinary](/sql/t-sql/data-types/binary-and-varbinary-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) |
| [text](/sql/t-sql/data-types/ntext-text-and-image-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) |[varchar](/sql/t-sql/data-types/char-and-varchar-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) |
| [ntext](/sql/t-sql/data-types/ntext-text-and-image-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) |[nvarchar](/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) |
| [sql_variant](/sql/t-sql/data-types/sql-variant-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) |Spalte in mehrere Spalten mit starker Typisierung unterteilen. |
| [Tabelle](/sql/t-sql/data-types/table-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) |Wenn Sie den SQL-Pool verwenden, ist die Konvertierung in temporäre Tabellen möglich. Wenn Sie SQL (Vorschauversion) verwenden, können Sie erwägen, Daten mit [CETAS](../sql/develop-tables-cetas.md) im Speicher zu speichern. |
| [timestamp](/sql/t-sql/data-types/date-and-time-types) |Überarbeiten Sie Code mit [datetime2](/sql/t-sql/data-types/datetime2-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) und der [CURRENT_TIMESTAMP](/sql/t-sql/functions/current-timestamp-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)-Funktion. Nur Konstanten werden als Standardwerte unterstützt. Daher kann „current_timestamp“ nicht als Standardeinschränkung definiert werden. Wenn Sie Zeilenversionswerte aus einer Spalte vom Typ „timestamp“ migrieren müssen, sollten Sie [BINARY](/sql/t-sql/data-types/binary-and-varbinary-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)(8) oder [VARBINARY](/sql/t-sql/data-types/binary-and-varbinary-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)(8) für NOT NULL- oder NULL-Zeilenversionswerte verwenden. |
| [xml](/sql/t-sql/xml/xml-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) |[varchar](/sql/t-sql/data-types/char-and-varchar-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) |
| [Benutzerdefinierter Typ](/sql/relational-databases/native-client/features/using-user-defined-types) |Führen Sie, wenn möglich, eine Rückkonvertierung in den nativen Datentyp durch. |
| Standardwerte | Standardwerte unterstützen nur Literale und Konstanten. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Entwickeln von Tabellen finden Sie unter [Einführung in das Entwerfen von Tabellen in Azure SQL Data Warehouse](develop-overview.md).
