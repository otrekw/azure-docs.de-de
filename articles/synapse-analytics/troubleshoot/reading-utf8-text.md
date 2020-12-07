---
title: 'Problembehandlung: Lesen von UTF-8-Text aus CSV- oder Parquet-Dateien mithilfe eines serverlosen SQL-Pools'
description: Lesen von UTF-8-Text aus CSV- oder Parquet-Dateien mithilfe eines serverlosen SQL-Pools in Azure Synapse Analytics
author: julieMSFT
ms.author: jrasnick
ms.topic: troubleshooting
ms.service: synapse-analytics
ms.subservice: sql
ms.date: 11/24/2020
ms.openlocfilehash: 238880cb3f3628df7591e8d08e3057ebfd885900
ms.sourcegitcommit: 6a350f39e2f04500ecb7235f5d88682eb4910ae8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/01/2020
ms.locfileid: "96465395"
---
# <a name="troubleshoot-reading-utf-8-text-from-csv-or-parquet-files-using-serverless-sql-pool-in-azure-synapse-analytics"></a>Problembehandlung beim Lesen von UTF-8-Text aus CSV- oder Parquet-Dateien mithilfe eines serverlosen SQL-Pools in Azure Synapse Analytics

In diesem Artikel werden Schritte zur Problembehandlung beim Lesen von UTF-8-Text aus CSV- oder Parquet-Dateien mithilfe eines serverlosen SQL-Pools in Azure Synapse Analytics beschrieben.

Wenn UTF-8-Text mithilfe eines serverlosen SQL-Pools aus einer CSV- oder Parquet-Datei gelesen wird, werden einige Sonderzeichen wie „ü“ und „ö“ fälschlicherweise konvertiert, wenn die Abfrage VARCHAR-Spalten mit Nicht-UTF8-Sortierungen zurückgibt. Dies ist ein bekanntes Problem in SQL Server und Azure SQL. Die Nicht-UTF8-Sortierung ist die Standardeinstellung in Synapse SQL, sodass Kundenabfragen beeinträchtigt werden. Kunden, die englische Standardzeichen und eine Teilmenge von erweiterten lateinischen Zeichen verwenden, bemerken die Konvertierungsfehler möglicherweise nicht. Die falsche Konvertierung wird in [Always use UTF-8 collations to read UTF-8 text in serverless SQL pool](https://techcommunity.microsoft.com/t5/azure-synapse-analytics/always-use-utf-8-collations-to-read-utf-8-text-in-serverless-sql/ba-p/1883633) (Immer UTF-8-Sortierungen zum Lesen von UTF-8-Text im serverlosen SQL-Pool verwenden) ausführlich erläutert.

## <a name="workaround"></a>Problemumgehung

Die Umgehung für dieses Problem besteht darin, beim Lesen von UTF-8-Text aus CSV- oder Parquet-Dateien immer UTF-8-Sortierung zu verwenden.

-   In vielen Fällen müssen Sie nur UTF8-Sortierung für die Datenbank festlegen (Metadatenvorgang).
-   Wenn Sie keine UTF8-Sortierung für externe Tabellen angegeben haben, die UTF8-Daten lesen, müssen Sie betroffene externe Tabellen neu erstellen und UTF8-Sortierung für VARCHAR-Spalten (Metadatenvorgang) festlegen.


## <a name="next-steps"></a>Nächste Schritte

* [CETAS mit Synapse SQL](../sql/develop-tables-cetas.md)
* [Schnellstart: Verwenden eines serverlosen SQL-Pools](../quickstart-sql-on-demand.md)
