---
title: Einschränkungen des Azure SQL-Datenbank-Ledgers
description: Einschränkungen des Ledgerfeatures in Azure SQL-Datenbank
ms.custom: ''
ms.date: 05/25/2021
ms.service: sql-database
ms.subservice: security
ms.reviewer: vanto
ms.topic: conceptual
author: JasonMAnderson
ms.author: janders
ms.openlocfilehash: 1a3451356f0c38e8006b4724873f2b82f39d6a85
ms.sourcegitcommit: 58e5d3f4a6cb44607e946f6b931345b6fe237e0e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/25/2021
ms.locfileid: "110386309"
---
# <a name="limitations-for-azure-sql-database-ledger"></a>Einschränkungen des Azure SQL-Datenbank-Ledgers

[!INCLUDE[appliesto-sqldb](../includes/appliesto-sqldb.md)]

> [!NOTE]
> Der Azure SQL-Datenbank-Ledger ist zurzeit als **öffentliche Vorschauversion** verfügbar.

Dieser Artikel bietet eine Übersicht über die Einschränkungen von Ledgertabellen in Azure SQL-Datenbank.  

## <a name="limitations"></a>Einschränkungen

| Funktion | Einschränkung |
| :--- | :--- |
| Deaktivieren einer [Ledgerdatenbank](ledger-database-ledger.md)   | Nach der Aktivierung kann eine Ledgerdatenbank nicht mehr deaktiviert werden. |
| Maximale Anzahl von Spalten | Nach der Erstellung fügen [aktualisierbare Ledgertabellen](ledger-updatable-ledger-tables.md) der Ledgertabelle vier [GENERATED ALWAYS](/sql/t-sql/statements/create-table-transact-sql#generate-always-columns)-Spalten hinzu, und [Ledgertabellen, die nur Anfügevorgänge unterstützen,](ledger-append-only-ledger-tables.md) fügen der Ledgertabelle zwei Spalten hinzu. Diese neuen Spalten werden von der maximal unterstützten Anzahl von Spalten in Azure SQL-Datenbank (1024) abgezogen. |
| Eingeschränkte Datentypen | Die Datentypen XML, SqlVariant, benutzerdefiniert und FILESTREAM werden nicht unterstützt. |
| In-Memory-Tabellen | In-Memory-Tabellen werden nicht unterstützt. |
| Spaltensätze mit geringer Dichte | Sparsespaltensätze werden nicht unterstützt. |
| Ledgerkürzung | Das Löschen älterer Daten in [Ledgertabellen, die nur Anfügevorgänge unterstützen,](ledger-append-only-ledger-tables.md) oder in der Verlaufstabelle [aktualisierbarer Ledgertabellen](ledger-updatable-ledger-tables.md) wird nicht unterstützt. |
| Umwandeln bestehender Tabellen in Ledgertabellen | Bestehende Tabellen in einer Datenbank, die nicht ledgerfähig sind, können nicht in Ledgertabellen umgewandelt werden. |
|LRS-Unterstützung für [automatisierte Digestverwaltung](ledger-digest-management-and-database-verification.md) | Die automatisierte Digestverwaltung mit Ledgertabellen mithilfe von [unveränderlichen Azure Storage-Blobs](../../storage/blobs/storage-blob-immutable-storage.md) bietet Benutzern nicht die Möglichkeit, [lokal redundante Speicherkonten (LRS)](../../storage/common/storage-redundancy.md#locally-redundant-storage) zu verwenden.|

## <a name="remarks"></a>Bemerkungen

- Wenn eine Ledgerdatenbank erstellt wird, sind alle neuen Tabellen, die standardmäßig (ohne Angabe der `APPEND_ONLY = ON`-Klausel) in der Datenbank erstellt werden, [aktualisierbare Ledgertabellen](ledger-updatable-ledger-tables.md). [Ledgertabellen, die nur Anfügevorgänge unterstützen,](ledger-append-only-ledger-tables.md) können [mithilfe von CREATE TABLE-Anweisungen (Transact-SQL)](/sql/t-sql/statements/create-table-transact-sql) erstellt werden.
- Ledgertabellen können keine Dateitabelle (FILETABLE) sein.
- Ledgertabellen können keine Volltextindizes enthalten.
- Ledgertabellen können nicht umbenannt werden.
- Ledgertabellen können nicht in ein anderes Schema verschoben werden.
- Ledgertabellen können nur Nullwerte zulassende Spalten hinzugefügt werden, und auch nur dann, wenn WITH VALUES nicht darauf angewendet wurde.
- Spalten in Ledgertabellen können nicht gelöscht werden.
- Für Ledgertabellen sind nur deterministisch berechnete Spalten zulässig.
- Bestehende Spalten können nicht auf eine Weise geändert werden, die das Spaltenformat ändert.
  - Folgende Änderungen sind zulässig:
    - NULL-Zulässigkeit
    - Sortierung für nvarchar-/ntext-Spalten, wenn die Codepage für char-/text-Spalten nicht geändert wird
    - Ändern der Länge von Spalten mit variabler Länge
    - Sparseness
- SWITCH IN/OUT ist für Ledgertabellen nicht zulässig.
- Die Langzeitaufbewahrung wird nicht für Datenbanken unterstützt, die `LEDGER = ON` konfiguriert haben.
- `LEDGER` oder `SYSTEM_VERSIONING` können für Ledgertabellen nicht deaktiviert werden.
- Die APIs `UPDATETEXT` und `WRITETEXT` können nicht für Ledgertabellen verwendet werden.
- Eine Transaktion kann bis zu 200 Ledgertabellen aktualisieren.
- Alle Einschränkungen temporaler Tabellen werden an aktualisierbare Ledgertabellen vererbt.
- Die Änderungsnachverfolgung ist für Ledgertabellen nicht zulässig.
- Ledgertabellen dürfen keinen nicht gruppierten Rowstore-Index haben, wenn sie über einen gruppierten Columnstore-Index verfügen.

## <a name="next-steps"></a>Nächste Schritte

- [Aktualisierbare Ledgertabellen](ledger-updatable-ledger-tables.md)   
- [Ledger-Tabellen, die nur Anfügevorgänge unterstützen](ledger-append-only-ledger-tables.md)   
- [Datenbankledger](ledger-database-ledger.md)   
- [Digestverwaltung und Datenbanküberprüfung](ledger-digest-management-and-database-verification.md)   
