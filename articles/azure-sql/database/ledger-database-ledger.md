---
title: Datenbank-Ledger
description: Dieser Artikel enthält Informationen zu Ledger-Datenbanktabellen und zugeordneten Ansichten in der Azure SQL-Datenbank
ms.custom: ''
ms.date: 05/25/2021
ms.service: sql-database
ms.subservice: security
ms.reviewer: vanto
ms.topic: conceptual
author: JasonMAnderson
ms.author: janders
ms.openlocfilehash: 031674854af41877483ece5c3969a0208a7a8167
ms.sourcegitcommit: 58e5d3f4a6cb44607e946f6b931345b6fe237e0e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/25/2021
ms.locfileid: "110386342"
---
# <a name="what-is-the-database-ledger"></a>Was ist das Datenbank-Ledger

[!INCLUDE[appliesto-sqldb](../includes/appliesto-sqldb.md)]

> [!NOTE]
> Das Azure SQL-Datenbank-Ledger ist zurzeit als **öffentliche Vorschau** verfügbar.

Das Datenbank-Ledger verwendet logisch eine Block Chain und [Merkle-Datenstrukturen](/archive/msdn-magazine/2018/march/blockchain-blockchain-fundamentals). Das Datenbank-Ledger erfasst schrittweise den Zustand der Datenbank, wie er sich im Laufe der Zeit entwickelt, während Updates in den Ledger-Tabellen stattfinden. Um dies zu erreichen, speichert das Datenbank-Ledger einen Eintrag für jede Transaktion und erfasst Metadaten zur Transaktion, z. B. den Commit-Zeitstempel. Es speichert außerdem die Identität des Benutzers, der ihn ausgeführt hat, sowie die Merkle-Stammstruktur in jeder Ledger-Tabelle aktualisierten Zeilen. Diese Einträge werden dann an eine manipulationssichere Datenstruktur angefügt, um die Überprüfung der Integrität in Zukunft zu ermöglichen.

:::image type="content" source="media/ledger/merkle-tree.png" alt-text="SQL Ledger Merkle-Struktur":::

Weitere Informationen dazu, wie Azure SQL-Datenbank Ledger Datenintegrität bietet, finden Sie unter [Digest-Verwaltung und Datenbanküberprüfung](ledger-digest-management-and-database-verification.md).

## <a name="where-are-database-transaction-and-block-data-stored"></a>Wo werden Datenbanktransaktions- und Blockdaten gespeichert?

Die Daten zu Transaktionen und Blöcken werden physisch als Zeilen in zwei neuen Systemkatalogsichten gespeichert:

- [**sys.database_ledger_transactions**](/sql/relational-databases/system-catalog-views/sys-database-ledger-transactions-transact-sql): Verwaltet eine Zeile mit den Informationen jeder Transaktion im Ledger, einschließlich der ID des Blocks, zu dem diese Transaktion gehört, und der Ordnungszahl der Transaktion innerhalb des Blocks. 
- [ **sys.database_ledger_blocks**](/sql/relational-databases/system-catalog-views/sys-database-ledger-blocks-transact-sql): Verwaltet eine Zeile für jeden Block im Ledger, einschließlich des Stamms der Merkle-Struktur über den Transaktionen innerhalb des Blocks und des Hashs des vorherigen Blocks, um eine Block Chain zu bilden.

Führen Sie zum Anzeigen des Datenbank-Ledgers die folgenden T-SQL-Anweisungen in [SQL Server Management Studio](/sql/ssms/download-sql-server-management-studio-ssms) oder [Azure Data Studio](/sql/azure-data-studio/download-azure-data-studio) aus.

> [!IMPORTANT]
> Zum Anzeigen des Datenbank-Ledgers ist die Berechtigung **VIEW LEDGER CONTENT** erforderlich. Weitere Informationen zu Berechtigungen im Zusammenhang mit Ledger-Tabellen finden Sie unter [Berechtigungen](/sql/relational-databases/security/permissions-database-engine#asdbpermissions). 

```sql
SELECT * FROM sys.database_ledger_transactions
GO

SELECT * FROM sys.database_ledger_blocks
GO
```

Das folgende Beispiel zeigt eine Ledger-Tabelle, die aus vier Transaktionen besteht, aus denen ein Block in der Block Chain des Datenbank-Ledgers besteht.

:::image type="content" source="media/ledger/database-ledger-1.png" alt-text="Beispiel-Ledger-Tabelle":::

Ein -Block wird alle 30 Sekunden geschlossen, oder wenn der Benutzer manuell einen Datenbank-Digest durch Ausführen der [sys.sp_generate_database_ledger_digest](/sql/relational-databases/system-stored-procedures/sys-sp-generate-database-ledger-digest-transact-sql) gespeicherten Prozedur generiert. Wenn ein Block geschlossen wird, werden neue Transaktionen in einen neuen Block eingefügt. Der Blockgenerierungsprozess ruft dann alle Transaktionen ab, die zum *geschlossenen* Block gehören, sowohl aus der In-Memory-Warteschlange als auch aus der [sys.database_ledger_transactions](/sql/relational-databases/system-catalog-views/sys-database-ledger-transactions-transact-sql) Systemkatalogsicht, berechnet den Merkle-Strukturstamm für diese Transaktionen und den Hash des vorherigen Blocks und bewahrt den geschlossenen Block in der [sys.database_ledger_blocks](/sql/relational-databases/system-catalog-views/sys-database-ledger-blocks-transact-sql) Systemkatalogsicht auf. Da es sich hierbei um ein reguläres Tabellenupdate handelt, wird die Dauerhaftigkeit automatisch vom System garantiert. Um die einzelne Kette von Blöcken aufrechtzuerhalten, ist dieser Vorgang single-threaded, aber er ist auch effizient, da er nur die Hashes über die Transaktionsinformationen berechnet und asynchron abläuft, wodurch die Transaktionsleistung nicht beeinträchtigt wird.   

## <a name="next-steps"></a>Nächste Schritte

- [Digest-Verwaltung und Datenbanküberprüfung](ledger-digest-management-and-database-verification.md)
- [Azure SQL-Datenbank-Ledger – Übersicht](ledger-overview.md) 
- [Sicherheitskatalogsichten (Transact-SQL)](/sql/relational-databases/system-catalog-views/security-catalog-views-transact-sql)
