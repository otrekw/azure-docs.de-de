---
title: 'Azure SQL-Datenbank: aktualisierbare Ledgertabellen'
description: Dieser Artikel enthält Informationen zu aktualisierbaren Ledgertabellen, dem Ledgerschema und Ledgersichten in Azure SQL-Datenbank.
ms.custom: ''
ms.date: 05/25/2021
ms.service: sql-database
ms.subservice: security
ms.reviewer: vanto
ms.topic: conceptual
author: JasonMAnderson
ms.author: janders
ms.openlocfilehash: df73b0719dc035e138d8cc2dbfb26da2fa9642dc
ms.sourcegitcommit: 58e5d3f4a6cb44607e946f6b931345b6fe237e0e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/25/2021
ms.locfileid: "110386302"
---
# <a name="azure-sql-database-updatable-ledger-tables"></a>Azure SQL-Datenbank: aktualisierbare Ledgertabellen

[!INCLUDE[appliesto-sqldb](../includes/appliesto-sqldb.md)]

> [!NOTE]
> Der Azure SQL-Datenbank-Ledger ist zurzeit als **Public Preview** (Öffentliche Vorschau) verfügbar.

Aktualisierbare Ledgertabellen sind Tabellen mit vom System verwalteten Versionen, für die Benutzer Updates und Löschungen durchführen können und die gleichzeitig Beweisfunktionen für Manipulationen bereitstellen. Bei Updates oder Löschungen werden alle früheren Versionen einer Zeile in einer sekundären Tabelle beibehalten, die als Verlaufstabelle bezeichnet wird. Die Verlaufstabelle hat dasselbe Schema wie die aktualisierbare Ledgertabelle. Bei einem Update einer Zeile verbleibt die neueste Version der Zeile in der Ledgertabelle, während ihre frühere Version vom System in die Verlaufstabelle eingefügt wird. Dieser Vorgang ist für die Anwendung vollkommen transparent. 

:::image type="content" source="media/ledger/ledger-table-architecture.png" alt-text="Architektur der Ledgertabelle":::

## <a name="updatable-ledger-tables-vs-temporal-tables"></a>Aktualisierbare Ledgertabellen im Vergleich zu temporalen Tabellen

Sowohl aktualisierbare Ledgertabellen als auch [temporale](/sql/relational-databases/tables/temporal-tables) Tabellen sind Tabellen, deren Versionsverwaltung vom System durchgeführt wird und für die die Datenbank-Engine einen Verlauf der Zeilenversionen in sekundären Verlaufstabellen erfasst. Beide Technologien bieten spezielle Vorteile. Aktualisierbare Ledgertabellen machen die aktuellen und die Verlaufsdaten manipulationssicher. Temporale Tabellen unterstützen das Abfragen von Daten zu jedem Zeitpunkt, anstatt nur der aktuell in einer Tabelle gespeicherten Daten.

Sie können beide Technologien zusammen verwenden, indem Sie Tabellen erstellen, die sowohl aktualisierbare Ledgertabellen als auch temporale Tabellen sind. Das Erstellen aktualisierbarer Ledgertabellen kann auf zwei Arten erfolgen:

- Sie erstellen im Azure-Portal eine neue Datenbank und wählen während der Ledgerkonfiguration **Enable ledger on all future tables in this database** (Ledger für alle zukünftigen Tabellen in dieser Datenbank aktivieren) aus, oder Sie geben in Ihrer Anweisung [CREATE DATABASE (Transact-SQL)](/sql/t-sql/statements/create-database-transact-sql) das Argument `LEDGER = ON` an. Damit erstellen Sie eine Ledgerdatenbank, mit der sichergestellt wird, dass alle zukünftigen Tabellen, die in Ihrer Datenbank erstellt werden, standardmäßig aktualisierbare Ledgertabellen sind.
- Sie erstellen eine neue Tabelle für eine Datenbank, in der auf Datenbankebene kein Ledger aktiviert ist, und geben in Ihrer Anweisung [CREATE TABLE (Transact-SQL)](/sql/t-sql/statements/create-table-transact-sql) das Argument `LEDGER = ON` an.

Weitere Informationen zu den Optionen, die beim Angeben des Arguments `LEDGER` in Ihrer T-SQL-Anweisung verfügbar sind, finden Sie unter [CREATE TABLE (Transact-SQL)](/sql/t-sql/statements/create-table-transact-sql).

> [!IMPORTANT]
> Nach dem Erstellen kann eine Ledgertabelle nicht mehr in eine anderen Tabellentyp konvertiert werden. Dadurch wird sichergestellt, dass Angreifer Ledgerfunktionen für eine Ledgertabelle nicht vorübergehend außer Kraft setzen, Änderungen vornehmen und dann die Ledgerfunktionalität erneut aktivieren können. 

### <a name="updatable-ledger-table-schema"></a>Schema aktualisierbarer Ledgertabellen

Eine aktualisierbare Ledgertabelle muss über die folgenden [GENERATED ALWAYS](/sql/t-sql/statements/create-table-transact-sql#generate-always-columns)-Spalten verfügen. Diese enthalten Metadaten, die angeben, durch welche Transaktionen Änderungen an der Tabelle vorgenommen wurden, und in welcher Reihenfolge die Vorgänge ausgeführt wurden, mit denen Zeilen von der jeweiligen Transaktion aktualisiert wurden.  Diese Daten sind für forensische Zwecke nützlich, um zu verstehen, wie Daten im Lauf der Zeit eingefügt wurden.

> [!NOTE]
> Wenn Sie in der Anweisung [CREATE TABLE (Transact-SQL)](/sql/t-sql/statements/create-table-transact-sql?view=azuresqldb-current&preserve-view=true) nicht die erforderlichen `GENERATED ALWAYS`-Spalten der Ledgertabelle und der Ledgerverlaufstabelle angeben, fügt das System die Spalten automatisch hinzu und verwendet dabei die folgenden Standardnamen. Weitere Informationen finden Sie in den Beispielen zum [Erstellen einer aktualisierbaren Ledgertabelle](/sql/t-sql/statements/create-table-transact-sql?view=azuresqldb-current&preserve-view=true#x-creating-a-updatable-ledger-table).

| Standardspaltenname | Datentyp | Beschreibung |
| --- | --- | --- |
| ledger_start_transaction_id | BIGINT | Die ID der Transaktion, mit der eine Zeilenversion erstellt wurde |
| ledger_end_transaction_id | BIGINT | Die ID der Transaktion, mit der eine Zeilenversion gelöscht wurde |
| ledger_start_sequence_number | BIGINT | Die Sequenznummer eines Vorgangs innerhalb einer Transaktion, mit der eine Zeilenversion erstellt wurde |
| ledger_end_sequence_number | BIGINT | Die Sequenznummer eines Vorgangs innerhalb einer Transaktion, mit der eine Zeilenversion gelöscht wurde |

## <a name="history-table"></a>Verlaufstabelle

Die Verlaufstabelle wird bei der Erstellung einer aktualisierbaren Ledgertabelle automatisch generiert. In der Verlaufstabelle werden die Verlaufswerte von Zeilen erfasst, die aufgrund von Updates oder Löschungen in der aktualisierbaren Ledgertabelle geändert wurden. Das Schema der Verlaufstabelle entspricht dem Schema der aktualisierbaren Ledgertabelle, der sie zugeordnet ist.

Wenn Sie eine aktualisierbare Ledgertabelle erstellen, können Sie entweder den Namen des Schemas für Ihre Verlaufstabelle und den Namen der Verlaufstabelle angeben, oder Sie lassen den Namen der Verlaufstabelle vom System generieren, das dann auch dasselbe Schema wie bei der Ledgertabelle hinzufügt. Verlaufstabellen mit vom System generierten Namen werden als „anonyme Verlaufstabellen“ bezeichnet. Die Namenskonvention für anonyme Verlaufstabellen lautet: `<schema>`.`<updatableledgertablename>`.MSSQL_LedgerHistoryFor_`<GUID>`.

## <a name="ledger-view"></a>Ledgersicht

Für jede aktualisierbare Ledgertabelle generiert das System automatisch eine Sicht, die als Ledgersicht bezeichnet wird. Die Ledgersicht ist ein Join der aktualisierbaren Ledgertabelle und der zugehörigen Verlaufstabelle. Die Ledgersicht meldet alle Zeilenänderungen, die an der aktualisierbaren Ledgertabelle vorgenommen wurden, indem die Verlaufsdaten in der Verlaufstabelle verknüpft werden. Dadurch können Benutzer, ihre Partner oder Prüfer alle Vorgänge im Verlauf analysieren und potenzielle Manipulationen erkennen. Jedem Zeilenvorgang wird die ID der ausgeführten Transaktion sowie die Angabe zugeordnet, ob es eine `DELETE`- oder `INSERT`-Transaktion war. So können Benutzer weitere Informationen zum Zeitpunkt der Transaktionsausführung und zur Identität des ausführenden Benutzers abrufen und mit anderen Vorgängen korrelieren, die im Rahmen dieser Transaktion ausgeführt wurden.

Wenn Sie z. B. den Transaktionsverlauf für ein einfaches Bankszenario nachverfolgen möchten, ist die Ledgersicht äußerst hilfreich, da sie eine Übersicht über die Transaktionen im Zeitverlauf bietet. So müssen Sie nicht die aktualisierbare Ledgertabelle und die Verlaufstabellen getrennt voneinander anzeigen oder eine eigene Sicht für diesen Zweck erstellen.

Ein Beispiel für die Verwendung der Ledgersicht finden Sie unter [Erstellen und Verwenden von aktualisierbaren Ledgertabellen](ledger-how-to-updatable-ledger-tables.md).

Das Schema der Ledgersicht spiegelt die Spalten, die in der aktualisierbaren Ledger- und der Verlaufstabelle definiert sind, nur die [GENERATED ALWAYS](/sql/t-sql/statements/create-table-transact-sql#generate-always-columns)-Spalten unterscheiden sich zwischen den aktualisierbaren Ledger- und Verlaufstabellen.

### <a name="ledger-view-schema"></a>Schema der Ledgersicht

> [!NOTE]
> Die Spaltennamen der Ledger-Ansicht können beim Erstellen der Tabelle mithilfe des `<ledger_view_option>`-Parameters mit der [CREATE TABLE (Transact-SQL)](/sql/t-sql/statements/create-table-transact-sql?view=azuresqldb-current&preserve-view=true)-Anweisung angepasst werden. Weitere Informationen finden Sie unter den [Optionen für die Ledger-Ansicht](/sql/t-sql/statements/create-table-transact-sql?view=azuresqldb-current&preserve-view=true#ledger-view-options) und in den entsprechenden Beispielen zu [CREATE TABLE (Transact-SQL)](/sql/t-sql/statements/create-table-transact-sql?view=azuresqldb-current&preserve-view=true).

| Standardspaltenname | Datentyp | Beschreibung |
| --- | --- | --- |
| ledger_transaction_id | BIGINT | Die Transaktions-ID, mit der eine Zeilenversion erstellt oder gelöscht wurde. |
| ledger_sequence_number | BIGINT | Die Sequenznummer eines Vorgangs auf Zeilenebene innerhalb der Transaktion für die Tabelle. |
| ledger_operation_type_id | TINYINT | Enthält `0` (**EINFÜGEN**) oder `1` (**LÖSCHEN**). Das Einfügen einer Zeile in die Ledger-Tabelle erstellt eine neue Zeile in der Ledger-Ansicht, bei der `0` in dieser Spalte enthalten ist. Das Löschen einer Zeile in der Ledger-Tabelle erstelle eine neue Zeile in der Ledger-Ansicht, bei der `1` in dieser Spalte enthalten ist. Das Aktualisieren einer Zeile in der Ledger-Tabelle erstellt zwei neue Zeilen in der Ledger-Ansicht. Eine Zeile enthält in dieser Spalte `1` (**DELETE**), und die andere Zeile enthält `1` (**INSERT**). |
| ledger_operation_type_desc | nvarchar(128) | Enthält `INSERT` oder `DELETE`. Einzelheiten dazu finden Sie weiter oben. |

## <a name="next-steps"></a>Nächste Schritte
 
- [Erstellen und Verwenden aktualisierbarer Ledgertabellen](ledger-how-to-updatable-ledger-tables.md)
- [Erstellen und Verwenden von Ledgertabellen, die nur Anfügevorgänge unterstützen](ledger-how-to-append-only-ledger-tables.md) 
