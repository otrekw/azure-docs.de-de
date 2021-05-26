---
title: Append-Only Ledger-Tabellen in Azure SQL Database
description: Dieser Artikel enthält Informationen über das Schema und die Ansichten der Append-Only Ledger-Tabelle in Azure SQL Database
ms.custom: ''
ms.date: 05/25/2021
ms.service: sql-database
ms.subservice: security
ms.reviewer: vanto
ms.topic: conceptual
author: JasonMAnderson
ms.author: janders
ms.openlocfilehash: 53783152a7968fff5375be82c9b72f0881c349c4
ms.sourcegitcommit: 58e5d3f4a6cb44607e946f6b931345b6fe237e0e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/25/2021
ms.locfileid: "110386369"
---
# <a name="azure-sql-database-append-only-ledger-tables"></a>Append-Only Ledger-Tabellen in Azure SQL Database

[!INCLUDE[appliesto-sqldb](../includes/appliesto-sqldb.md)]

> [!NOTE]
> Das Azure SQL-Datenbank-Ledger ist zurzeit als **Öffentliche Vorschau** verfügbar.

Append-Only Ledger-Tabellen lassen nur `INSERT`-Vorgänge in Ihren Tabellen zu, bei denen gewährleistet ist, dass privilegierte Benutzer wie z. B. Datenbankadministratoren (DBAs) die Daten nicht durch herkömmliche [DML (Datenbearbeitungssprache)](/sql/t-sql/queries/queries)-Vorgänge ändern können. Append-Only Ledger-Tabellen eignen sich ideal für Systeme, die Datensätze nicht aktualisieren oder löschen, z. B. SIEM-Systeme (Security Information Event and Management) oder Block Chain-Systeme, bei denen Daten aus der Block Chain in eine Datenbank repliziert werden müssen.  Da es entweder keine `UPDATE`- oder aber `DELETE`-Vorgänge in einer Append-Only Tabelle gibt, ist keine entsprechende Verlaufstabelle wie bei [aktualisierbaren Ledger-Tabellen](ledger-updatable-ledger-tables.md) notwendig.

:::image type="content" source="media/ledger/ledger-table-architecture-append-only.png" alt-text="Architektur von Ledger-Tabellen":::

Erstellen einer Append-Only Ledger-Tabelle, kann durch das Festlegen des `LEDGER = ON`-Arguments in Ihrer [CREATE TABLE (Transact-SQL)](/sql/t-sql/statements/create-table-transact-sql)-Anweisung und Angabe der `APPEND_ONLY = ON`-Option durchgeführt werden.

> [!IMPORTANT]
> Sobald eine Tabelle als Ledger-Tabelle erstellt wurde, kann sie nicht auf eine Tabelle zurück verwendet werden, die nicht über eine Ledger-Funktionalität verfügt. Dadurch wird sichergestellt, dass ein Angreifer die Ledger-Funktionalitäten nicht vorübergehend entfernen, Änderungen vornehmen und dann die Ledger-Funktionalitäten erneut aktivieren können.

### <a name="append-only-ledger-table-schema"></a>Append-Only Ledger-Tabellenschema

Eine aktualisierbare Ledger-Tabelle muss über die folgenden [GENERATED ALWAYS](/sql/t-sql/statements/create-table-transact-sql#generate-always-columns)-Spalten verfügen. Diese enthalten Metadaten, die angeben, durch welche Transaktionen Änderungen an der Tabelle vorgenommen wurden, und in welcher Reihenfolge die Vorgänge ausgeführt wurden, mit denen Zeilen von der jeweiligen Transaktion aktualisiert wurden. Beim Erstellen einer Append-Only Ledger-Tabelle werden `GENERATED ALWAYS`-Spalten in Ihrer Ledger-Tabelle erstellt. Diese Daten sind für forensische Zwecke nützlich, um zu verstehen, wie Daten im Laufe der Zeit eingefügt wurden.

Wenn Sie die Definitionen der `GENERATED ALWAYS`-Spalten in der [CREATE TABLE](/sql/t-sql/statements/create-table-transact-sql)-Anweisung nicht angeben, fügt das System diese automatisch hinzu, indem unten genannte Standardnamen verwendet werden.

| Standardspaltenname | Datentyp | Beschreibung |
|--|--|--|
| ledger_start_transaction_id | BIGINT | Die ID der Transaktion, mit der eine Zeilenversion erstellt wurde. |
| ledger_start_sequence_number | BIGINT | Die Sequenznummer eines Vorgangs innerhalb einer Transaktion, mit der eine Zeilenversion erstellt wurde. |

## <a name="ledger-view"></a>Ledger-Ansicht

Für jede Append-Only Ledger-Tabelle generiert das System automatisch eine Ansicht, die als Ledger-Ansicht bezeichnet wird. Die Ledger-Ansicht meldet alle eingefügte Zeilen, die in der Tabelle aufgetreten sind. Die Ledger-Ansicht ist in erster Linie hilfreich für [aktualisierbare Ledger-Tabellen](ledger-updatable-ledger-tables.md), anstatt Append-Only Ledger-Tabelle, da die Append-Only Ledger-Tabelle entweder keine `UPDATE`- oder aber `DELETE`-Funktionalitäten haben. Die Ledger-Ansicht für Append-Only Ledger-Tabellen ist für Konsistenz zwischen aktualisierbaren und Append-Only Ledger-Tabelle verfügbar.

### <a name="ledger-view-schema"></a>Ledger-Ansichtsschema

> [!NOTE]
> Die Spaltennamen der Ledger-Ansicht können beim Erstellen der Tabelle mithilfe des `<ledger_view_option>`-Parameters mit der [CREATE TABLE (Transact-SQL)](/sql/t-sql/statements/create-table-transact-sql?view=azuresqldb-current&preserve-view=true)-Anweisung angepasst werden. Weitere Informationen finden Sie unter den [Optionen für die Ledger-Ansicht](/sql/t-sql/statements/create-table-transact-sql?view=azuresqldb-current&preserve-view=true#ledger-view-options) und in den entsprechenden Beispielen zu [CREATE TABLE (Transact-SQL)](/sql/t-sql/statements/create-table-transact-sql?view=azuresqldb-current&preserve-view=true).

| Standardspaltenname | Datentyp | Beschreibung |
| --- | --- | --- |
| ledger_transaction_id | BIGINT | Die Transaktions-ID, mit der eine Zeilenversion erstellt oder gelöscht wurde. |
| ledger_sequence_number | BIGINT | Die Sequenznummer eines Vorgangs auf Zeilenebene innerhalb der Transaktion für die Tabelle. |
| ledger_operation_type_id | TINYINT | Enthält `0` (**EINFÜGEN**) oder `1` (**LÖSCHEN**). Das Einfügen einer Zeile in die Ledger-Tabelle erstellt eine neue Zeile in der Ledger-Ansicht, bei der `0` in dieser Spalte enthalten ist. Das Löschen einer Zeile in der Ledger-Tabelle erstelle eine neue Zeile in der Ledger-Ansicht, bei der `1` in dieser Spalte enthalten ist. Das Aktualisieren einer Zeile in der Ledger-Tabelle erstellt zwei neue Zeilen in der Ledger-Ansicht. Eine Zeile in dieser Spalte enthält `1` (**LÖSCHEN**), und die andere Zeile enthält `1` (**Einfügen**). LÖSCHEN sollte nicht in einer Append-Only Ledger-Tabelle erfolgen. |
| ledger_operation_type_desc | nvarchar(128) | Enthält `INSERT` oder `DELETE`. Einzelheiten dazu finden Sie weiter oben. |

## <a name="next-steps"></a>Nächste Schritte
  
- [Erstellen und Verwenden von Append-Only Ledger-Tabellen](ledger-how-to-append-only-ledger-tables.md)
- [Erstellen und Verwenden aktualisierbarer Ledger-Tabellen](ledger-how-to-updatable-ledger-tables.md)
