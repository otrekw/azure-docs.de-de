---
title: Ressourcen für die Entwicklung von Synapse SQL-Features
description: Entwicklungskonzepte, Entwurfsentscheidungen, Empfehlungen und Programmiertechniken für Synapse SQL.
services: synapse-analytics
author: filippopovic
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: sql
ms.date: 04/15/2020
ms.author: fipopovi
ms.reviewer: jrasnick
ms.openlocfilehash: f4c61b43855e4a845956a937d9d9cfe738d6f47a
ms.sourcegitcommit: 3fc3457b5a6d5773323237f6a06ccfb6955bfb2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/11/2020
ms.locfileid: "90032944"
---
# <a name="design-decisions-and-coding-techniques-for-synapse-sql-features-in-azure-synapse-analytics"></a>Entwurfsentscheidungen und Programmiertechniken für Synapse SQL-Features in Azure Synapse Analytics
In diesem Artikel finden Sie eine Liste von Ressourcen für SQL-Pool- und SQL On-Demand-Funktionen (Vorschauversion) von Synapse SQL. Die empfohlenen Artikel sind in zwei Abschnitte unterteilt: Wesentliche Entwurfsentscheidungen und Entwicklungs- und Programmiertechniken.

Das Ziel dieser Artikel ist es, Ihnen bei der Entwicklung des optimalen technischen Ansatzes für die Synapse SQL-Komponenten innerhalb von Synapse Analytics zu helfen.

## <a name="key-design-decisions"></a>Wesentliche Entwurfsentscheidungen
Die folgenden Artikel heben Konzepte und Entwurfsentscheidungen für die Synapse SQL-Entwicklung hervor:

| Artikel | SQL-Pool | SQL On-Demand |
| ------- | -------- | ------------- |
| [Verbindungen](connect-overview.md)                    | Ja | Ja |
| [Ressourcenklassen und Parallelität](../sql-data-warehouse/resource-classes-for-workload-management.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json) | Ja    | Nein |
| [Transaktionen](develop-transactions.md)              | Ja | Nein |
| [Benutzerdefinierte Schemas](develop-user-defined-schemas.md) | Ja | Ja |
| [Verteilen von Tabellen in SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-tables-distribute.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)                 | Ja | Nein |
| [Ursachen für eine schlechte Qualität des Columnstore-Index](../sql-data-warehouse/sql-data-warehouse-tables-index.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)                           | Ja | Nein |
| [Tabellenpartitionen](../sql-data-warehouse/sql-data-warehouse-tables-partition.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)                     | Ja | Nein |
| [Statistik](develop-tables-statistics.md)            | Ja | Ja |
| [CTAS](../sql-data-warehouse/sql-data-warehouse-develop-ctas.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)                                             | Ja | Nein |
| [Externe Tabellen](develop-tables-external-tables.md) | Ja | Ja |
| [CETAS](develop-tables-cetas.md)                     | Ja | Ja |


## <a name="recommendations"></a>Empfehlungen

Im Folgenden finden Sie wichtige Artikel, die bestimmte Programmiertechniken, Tipps und Empfehlungen für die Entwicklung hervorheben:

| Artikel | SQL-Pool | SQL On-Demand |
| ------- | -------- | ------------- |
| [Gespeicherten Prozeduren](develop-stored-procedures.md)  | Ja                | Nein                      |
| [Bezeichnungen](develop-label.md)                           | Ja                | Nein                      |
| [Ansichten](develop-views.md)                             | Ja                | Ja                     |
| [Temporäre Tabellen](develop-tables-temporary.md)       | Ja                | Ja                     |
| [Dynamischer SQL-Code](develop-dynamic-sql.md)                 | Ja                | Ja                     |
| [Schleifen](develop-loops.md)                         | Ja                | Ja                     |
| [Gruppierungsoptionen](develop-group-by-options.md)       | Ja                | Nein                      |
| [Variablenzuweisung](develop-variable-assignment.md) | Ja                | Ja                     |

## <a name="next-steps"></a>Nächste Schritte
Weitere Referenzinformationen finden Sie unter [SQL-Pool – T-SQL-Anweisungen](../sql-data-warehouse/sql-data-warehouse-reference-tsql-statements.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json).

