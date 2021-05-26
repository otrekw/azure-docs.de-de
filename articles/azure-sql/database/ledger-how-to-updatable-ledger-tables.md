---
title: Erstellen und Verwenden aktualisierbarer Ledgertabellen
description: Erfahren Sie, wie Sie aktualisierbare Ledgertabellen in Azure SQL-Datenbank erstellen und verwenden.
ms.custom: ''
ms.date: 05/25/2021
ms.service: sql-database
ms.subservice: security
ms.reviewer: vanto
ms.topic: how-to
author: JasonMAnderson
ms.author: janders
ms.openlocfilehash: 43af922a23fa4e48a9b9efa7d292b4f03b06b94d
ms.sourcegitcommit: 58e5d3f4a6cb44607e946f6b931345b6fe237e0e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/25/2021
ms.locfileid: "110386317"
---
# <a name="create-and-use-updatable-ledger-tables"></a>Erstellen und Verwenden aktualisierbarer Ledgertabellen

[!INCLUDE[appliesto-sqldb](../includes/appliesto-sqldb.md)]

> [!NOTE]
> Der Azure SQL-Datenbank-Ledger ist zurzeit als **Public Preview** (Öffentliche Vorschau) verfügbar.

In diesem Artikel erfahren Sie, wie Sie eine [aktualisierbare Ledgertabelle](ledger-updatable-ledger-tables.md) in Azure SQL-Datenbank erstellen, Werte in Ihre aktualisierbare Ledgertabelle einfügen, diese Daten aktualisieren und die Ergebnisse mithilfe der Ledgersicht anzeigen. Dazu wird als Beispiel eine Bankanwendung verwendet, die das Guthaben von Bankkunden auf deren Konten nachverfolgt. In diesem Beispiel erhalten Sie einen praxisnahen Blick auf die Beziehung zwischen der aktualisierbaren Ledgertabelle und der zugehörigen Verlaufstabelle sowie der Ledgersicht.

## <a name="prerequisite"></a>Voraussetzungen

- Sie verfügen über eine Azure SQL-Datenbank mit aktiviertem Ledger. Falls Sie noch keine Azure SQL-Datenbank-Instanz erstellt haben, finden Sie unter [Schnellstart: Erstellen einer Azure SQL-Datenbank-Instanz mit aktiviertem Ledger](ledger-create-a-single-database-with-ledger-enabled.md) weitere Informationen.
- [SQL Server Management Studio (SSMS)](/sql/ssms/download-sql-server-management-studio-ssms) oder [Azure Data Studio](/sql/azure-data-studio/download-azure-data-studio)

## <a name="creating-an-updatable-ledger-table"></a>Erstellen einer aktualisierbaren Ledgertabelle

Erstellen Sie eine Kontosaldotabelle mit dem folgenden Schema.  

| Spaltenname | Datentyp      | BESCHREIBUNG                         |
| ----------- | -------------- | ----------------------------------- |
| CustomerID  | INT            | Kunden-ID: Primärschlüssel, gruppiert |
| LastName    | varchar(50)   | Nachname des Kunden                  |
| FirstName   | varchar(50)   | Vorname des Kunden                 |
| Balance     | decimal(10,2) | Kontostand                     |

> [!IMPORTANT]
> Für das Erstellen von aktualisierbaren Ledgertabellen ist die Berechtigung **ENABLE LEDGER** erforderlich. Weitere Informationen zu Berechtigungen im Zusammenhang mit Ledgertabellen finden Sie unter [Berechtigungen](/sql/relational-databases/security/permissions-database-engine#asdbpermissions). 

1. Erstellen Sie in [SQL Server Management Studio (SSMS)](/sql/ssms/download-sql-server-management-studio-ssms) oder [Azure Data Studio](/sql/azure-data-studio/download-azure-data-studio) ein neues Schema und die neue Tabelle `[Account].[Balance]`.

   ```sql
   CREATE SCHEMA [Account]
   GO
   
   CREATE TABLE [Account].[Balance]
   (
       [CustomerID] INT NOT NULL PRIMARY KEY CLUSTERED,
       [LastName] VARCHAR (50) NOT NULL,
       [FirstName] VARCHAR (50) NOT NULL,
       [Balance] DECIMAL (10,2) NOT NULL
   )
   WITH 
   (
    SYSTEM_VERSIONING = ON,
    LEDGER = ON
   );
   GO
   ```

    > [!NOTE]
    > Die Angabe des Arguments `LEDGER = ON` ist optional, wenn Sie beim Erstellen Ihrer Azure SQL-Datenbank-Instanz die Ledgerdatenbank aktiviert haben. 
    >
    > Im obigen Beispiel generiert das System die Namen der [GENERATED ALWAYS](/sql/t-sql/statements/create-table-transact-sql#generate-always-columns)-Spalten in der Tabelle, den Namen der [Ledgersicht](ledger-updatable-ledger-tables.md#ledger-view) und die Namen der [Spalten in der Ledgersicht](ledger-updatable-ledger-tables.md#ledger-view-schema).
    >
    > Die Spaltennamen der Ledgersicht können beim Erstellen der Tabelle mithilfe des Parameters `<ledger_view_option>` der Anweisung [CREATE TABLE (Transact-SQL)](/sql/t-sql/statements/create-table-transact-sql?view=azuresqldb-current&preserve-view=true) angepasst werden. Die `GENERATED ALWAYS`-Spalten sowie der Name der [Verlaufstabelle](ledger-updatable-ledger-tables.md#history-table) können ebenfalls angepasst werden. Weitere Informationen finden Sie unter den [Optionen für die Ledgersicht](/sql/t-sql/statements/create-table-transact-sql?view=azuresqldb-current&preserve-view=true#ledger-view-options) und in den entsprechenden Beispielen zu [CREATE TABLE (Transact-SQL)](/sql/t-sql/statements/create-table-transact-sql?view=azuresqldb-current&preserve-view=true##x-creating-a-updatable-ledger-table).

1. Während Ihre [aktualisierbare Ledgertabelle](ledger-updatable-ledger-tables.md) erstellt wird, werden auch die zugehörige Verlaufstabelle und die Ledgersicht generiert. Führen Sie den folgenden T-SQL-Befehl aus, um die neue Tabelle und die neue Sicht anzuzeigen.

   ```sql
   SELECT 
   ts.[name] + '.' + t.[name] AS [ledger_table_name]
   , hs.[name] + '.' + h.[name] AS [history_table_name]
   , vs.[name] + '.' + v.[name] AS [ledger_view_name]
   FROM sys.tables AS t
   JOIN sys.tables AS h ON (h.[object_id] = t.[history_table_id])
   JOIN sys.views v ON (v.[object_id] = t.[ledger_view_id])
   JOIN sys.schemas ts ON (ts.[schema_id] = t.[schema_id])
   JOIN sys.schemas hs ON (hs.[schema_id] = h.[schema_id])
   JOIN sys.schemas vs ON (vs.[schema_id] = v.[schema_id])
   ```

   :::image type="content" source="media/ledger/ledger-updatable-how-to-new-tables.png" alt-text="Abfragen neuer Ledgertabellen":::

1. Fügen Sie den Kunden `Nick Jones` als neuen Kunden mit einem Anfangssaldo von 50 US-Dollar ein.

   ```sql
   INSERT INTO [Account].[Balance]
   VALUES (1, 'Jones', 'Nick', 50)
   ```

1. Fügen Sie drei neue Kunden (`John Smith`, `Joe Smith` und `Mary Michaels`) mit einem Anfangssaldo von 500, 30 bzw. 200 US-Dollar ein.

   ```sql
   INSERT INTO [Account].[Balance]
   VALUES (2, 'Smith', 'John', 500),
   (3, 'Smith', 'Joe', 30),
   (4, 'Michaels', 'Mary', 200)
   ```

1. Zeigen Sie die aktualisierbare Ledgertabelle `[Account].[Balance]` an, und geben Sie dabei die [GENERATED ALWAYS](/sql/t-sql/statements/create-table-transact-sql#generate-always-columns)-Spalten an, die der Tabelle hinzugefügt wurden.

   ```sql
   SELECT * 
         ,[ledger_start_transaction_id]
         ,[ledger_end_transaction_id]
         ,[ledger_start_sequence_number]
         ,[ledger_end_sequence_number]
   FROM [Account].[Balance] 
   ```

   Im Ergebnisfenster werden zunächst die Werte angezeigt, die Sie mit Ihren T-SQL-Befehlen eingefügt haben, sowie die Systemmetadaten, die für die Datenherkunft verwendet werden.

   - `ledger_start_transaction_id` gibt die eindeutige Transaktions-ID an, die der Transaktion zugeordnet ist, mit der die Daten eingefügt wurden. Da `John`, `Joe` und `Mary` mit derselben Transaktion eingefügt wurden, haben sie alle dieselbe Transaktions-ID.
   - `ledger_start_sequence_number` gibt die Reihenfolge an, in der Werte von der Transaktion eingefügt wurden.

   :::image type="content" source="media/ledger/sql-updatable-how-to-1.png" alt-text="Ledgertabelle: Beispiel 1":::

1. Aktualisieren Sie den Saldo von `Nick` von `50` auf `100`.

   ```sql
   UPDATE [Account].[Balance] SET [Balance] = 100
   WHERE [CustomerID] = 1
   ```

1. Kopieren Sie den eindeutigen Namen Ihrer Verlaufstabelle. Sie benötigen sie im nächsten Schritt.

   ```sql
   SELECT 
   ts.[name] + '.' + t.[name] AS [ledger_table_name]
   , hs.[name] + '.' + h.[name] AS [history_table_name]
   , vs.[name] + '.' + v.[name] AS [ledger_view_name]
   FROM sys.tables AS t
   JOIN sys.tables AS h ON (h.[object_id] = t.[history_table_id])
   JOIN sys.views v ON (v.[object_id] = t.[ledger_view_id])
   JOIN sys.schemas ts ON (ts.[schema_id] = t.[schema_id])
   JOIN sys.schemas hs ON (hs.[schema_id] = h.[schema_id])
   JOIN sys.schemas vs ON (vs.[schema_id] = v.[schema_id])
   ```

   :::image type="content" source="media/ledger/sql-updatable-how-to-2.png" alt-text="Ledgertabelle: Beispiel 2":::

1. Zeigen Sie die aktualisierbare Ledgertabelle `[Account].[Balance]` zusammen mit der zugehörigen Verlaufstabelle und der Ledgersicht an.

   > [!IMPORTANT]
   > Ersetzen Sie `<history_table_name>` durch den Namen, den Sie im vorherigen Schritt kopiert haben.

   ```sql
   SELECT * 
         ,[ledger_start_transaction_id]
         ,[ledger_end_transaction_id]
         ,[ledger_start_sequence_number]
         ,[ledger_end_sequence_number]
   FROM [Account].[Balance] 
   GO
   
   SELECT * FROM <Your unique history table name>
   GO 
   
   SELECT * FROM Account.Balance_Ledger
   ORDER BY ledger_transaction_id
   GO
   ```

   > [!TIP]
   > Es wird empfohlen, den Änderungsverlauf über die [Ledgersicht](ledger-updatable-ledger-tables.md#ledger-view) und nicht über die [Verlaufstabelle](ledger-updatable-ledger-tables.md#history-table) abzufragen.

1. Der Kontostand von `Nick` wurde in der aktualisierbaren Ledgertabelle erfolgreich in `100` geändert.
1. In der Verlaufstabelle wird für `Nick` nun der vorherige Saldo von `50` angezeigt.
1. Die Ledgersicht zeigt, dass das Aktualisieren der Ledgertabelle ein `DELETE`-Vorgang der ursprünglichen Zeile mit `50` ist, bei dem der Saldo mit einem entsprechenden `INSERT`-Vorgang einer neuen Zeile mit `100` und dem neuen Saldo für `Nick` geändert wird.

   :::image type="content" source="media/ledger/sql-updatable-how-to-3.png" alt-text="Ledgertabelle: Beispiel 3":::


## <a name="next-steps"></a>Nächste Schritte

- [Datenbankledger](ledger-database-ledger.md)  
- [Digestverwaltung und Datenbanküberprüfung](ledger-digest-management-and-database-verification.md)   
- [Aktualisierbare Ledgertabellen](ledger-updatable-ledger-tables.md)
- [Ledgertabellen, die nur Anfügevorgänge unterstützen](ledger-append-only-ledger-tables.md) 
- [Erstellen und Verwenden von Ledgertabellen, die nur Anfügevorgänge unterstützen](ledger-how-to-append-only-ledger-tables.md) 
- [Zugreifen auf die in Azure Confidential Ledger (ACL) gespeicherten Digests](ledger-how-to-access-acl-digest.md)
- [Überprüfen einer Ledgertabelle zum Erkennen von Manipulationen](ledger-verify-database.md)
