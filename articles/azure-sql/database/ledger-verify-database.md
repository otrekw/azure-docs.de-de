---
title: Überprüfen einer Ledgertabelle zum Erkennen von Manipulationen
description: In diesem Artikel wird erläutert, wie Sie überprüfen können, ob eine Azure SQL-Datenbank-Tabelle manipuliert wurde.
ms.service: sql-database
ms.subservice: security
ms.devlang: ''
ms.topic: how-to
author: JasonMAnderson
ms.author: janders
ms.reviewer: vanto
ms.date: 05/25/2021
ms.openlocfilehash: 6669b071d79c78b35dc2cfb94f99fa9597f44bd6
ms.sourcegitcommit: 58e5d3f4a6cb44607e946f6b931345b6fe237e0e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/25/2021
ms.locfileid: "110386285"
---
# <a name="how-to-verify-a-ledger-table-to-detect-tampering"></a>Überprüfen einer Ledgertabelle zum Erkennen von Manipulationen

[!INCLUDE[appliesto-sqldb](../includes/appliesto-sqldb.md)]

> [!NOTE]
> Der Azure SQL-Datenbank-Ledger ist zurzeit als **Public Preview** (Öffentliche Vorschau) verfügbar.

In diesem Artikel überprüfen Sie die Integrität der Daten in Ihren Azure SQL-Datenbank-Ledgertabellen. Wenn Sie beim [Erstellen Ihrer Azure SQL-Datenbank](ledger-create-a-single-database-with-ledger-enabled.md) die Option **Automatischen Digestspeicher aktivieren** aktiviert haben, folgen Sie den Anleitungen des Azure-Portals, damit das Transact-SQL-Skript (T-SQL-Skript) automatisch generiert wird, das für die Überprüfung des Datenbankledgers im [Abfrage-Editor](connect-query-portal.md) erforderlich ist. Folgen Sie andernfalls den T-SQL-Anleitungen mithilfe von [SQL Server Management Studio (SSMS)](/sql/ssms/download-sql-server-management-studio-ssms) oder [Azure Data Studio](/sql/azure-data-studio/download-azure-data-studio).

## <a name="prerequisite"></a>Voraussetzung

- Ein aktives Azure-Abonnement. Falls Sie nicht über ein Abonnement verfügen, können Sie ein [kostenloses Konto erstellen](https://azure.microsoft.com/free/).
- [Erstellen einer Azure SQL-Datenbank mit aktiviertem Ledger](ledger-create-a-single-database-with-ledger-enabled.md)
- [Erstellen und Verwenden von aktualisierbaren Ledgertabellen](ledger-how-to-updatable-ledger-tables.md) oder [Erstellen und Verwenden von Ledgertabellen, die nur Anfügevorgänge unterstützen](ledger-how-to-append-only-ledger-tables.md)

## <a name="run-ledger-verification-for-azure-sql-database"></a>Durchführen der Ledgerüberprüfung für Azure SQL-Datenbank

# <a name="portal"></a>[Portal](#tab/azure-portal)

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com/), wählen Sie **Alle Ressourcen** aus, und suchen Sie die SQL-Datenbank, die Sie überprüfen möchten. Wählen Sie diese Datenbank aus.

     :::image type="content" source="media/ledger/ledger-portal-all-resources.png" alt-text="Azure-Portal mit der Registerkarte „Alle Ressourcen“":::

1. Wählen Sie unter **Sicherheit** die Option **Ledger** aus.

   :::image type="content" source="media/ledger/ledger-portal-manage-ledger.png" alt-text="Azure-Portal mit Registerkarte zur Ledgersicherheit":::

1. Wählen Sie im Bereich **Ledger** die Schaltfläche **Datenbank überprüfen** und dann im Fenster im bereits ausgefüllten Text das Symbol **Kopieren** aus.

     :::image type="content" source="media/ledger/ledger-portal-verify.png" alt-text="Azure-Portal mit der Schaltfläche „Datenbank überprüfen“":::

1. Öffnen Sie im Menü links den **Abfrage-Editor**.

     :::image type="content" source="media/ledger/ledger-portal-open-query-editor.png" alt-text="Azure-Portal mit der Schaltfläche „Abfrage-Editor“":::

1. Fügen Sie im **Abfrage-Editor** das in Schritt 3 kopierte T-SQL-Skript ein, und wählen Sie **Ausführen** aus.

   :::image type="content" source="media/ledger/ledger-portal-run-query-editor.png" alt-text="Azure-Portal bei der Ausführung des Abfrage-Editors zum Überprüfen der Datenbank":::

1. Bei erfolgreicher Überprüfung wird im Fenster **Ergebnisse** Folgendes zurückgegeben.

   - Wenn Ihre Datenbank nicht manipuliert wurde, lautet die Meldung so:

   ```output
   Ledger verification successful
   ```

   - Wenn ihre Datenbank manipuliert wurde, wird der folgende Fehler im Fenster **Meldungen** angezeigt:
  
   ```output
   Failed to execute query. Error: The hash of block xxxx in the database ledger does not match the hash provided in the digest for this block.
   ```

# <a name="t-sql"></a>[T-SQL](#tab/t-sql)

1. Stellen Sie über [SQL Server Management Studio](/sql/ssms/download-sql-server-management-studio-ssms) oder [Azure Data Studio](/sql/azure-data-studio/download-azure-data-studio) eine Verbindung mit Ihrer Datenbank her.
1. Erstellen Sie eine neue Abfrage mit der folgenden T-SQL-Anweisung.

   ```sql
   /****** This will retrieve the latest digest file  ******/
   EXECUTE sp_generate_database_ledger_digest
   ```

1. Führen Sie die Abfrage aus. Die Ergebnisse enthalten den neuesten Datenbankhash und stellen den Hash der Datenbank zum aktuellen Zeitpunkt dar. Kopieren Sie den Inhalt der Ergebnisse, die im nächsten Schritt verwendet werden sollen.

   :::image type="content" source="media/ledger/ledger-retrieve-digest.png" alt-text="Abrufen von Digestergebnissen mithilfe von Azure Data Studio":::

1. Erstellen Sie eine neue Abfrage mit der folgenden T-SQL-Anweisung. Ersetzen Sie darin `<YOUR DATABASE DIGEST>` durch den im vorhergehenden Schritt kopierten Hash.

   ```
   /****** Verifies the integrity of the ledger using the referenced digest  ******/
   EXECUTE sp_verify_database_ledger N'
   <YOUR DATABASE DIGEST>
   '
   ```

1. Führen Sie die Abfrage aus. Die **Meldungen** enthalten die nachstehende Erfolgsmeldung.  

   :::image type="content" source="media/ledger/ledger-verify-message.png" alt-text="Meldung nach Ausführung der T-SQL-Abfrage für die Ledgerüberprüfung mithilfe von Azure Data Studio":::

   > [!TIP]
   > Bei Durchführung der Ledgerüberprüfung mit dem neuesten Digest wird nur die Datenbank überprüft – ab dem Zeitpunkt, zu dem der Digest generiert wurde, bis zu dem Zeitpunkt, zu dem die Überprüfung durchgeführt wurde. Wenn Sie überprüfen möchten, ob die Verlaufsdaten in Ihrer Datenbank manipuliert wurden, führen Sie die Überprüfung mithilfe mehrerer Datenbank-Digestdateien durch. Beginnen Sie bei dem Zeitpunkt, zu dem Sie die Datenbank überprüfen möchten. Ein Beispiel für eine Überprüfung, bei der mehrere Digests übergeben werden, sieht ungefähr wie die folgende Abfrage aus:

   ```
   EXECUTE sp_verify_database_ledger N'
   [
       {
           "database_name":  "ledgerdb",
           "block_id":  0,
           "hash":  "0xDC160697D823C51377F97020796486A59047EBDBF77C3E8F94EEE0FFF7B38A6A",
           "last_transaction_commit_time":  "2020-11-12T18:01:56.6200000",
           "digest_time":  "2020-11-12T18:39:27.7385724"
       },
       {
           "database_name":  "ledgerdb",
           "block_id":  1,
           "hash":  "0xE5BE97FDFFA4A16ADF7301C8B2BEBC4BAE5895CD76785D699B815ED2653D9EF8",
           "last_transaction_commit_time":  "2020-11-12T18:39:35.6633333",
           "digest_time":  "2020-11-12T18:43:30.4701575"
       }
   ]
   ```

---

## <a name="next-steps"></a>Nächste Schritte

- [Azure SQL-Datenbank-Ledger – Übersicht](ledger-overview.md)
- [Datenbankledger](ledger-database-ledger.md)
- [Digestverwaltung und Datenbanküberprüfung](ledger-digest-management-and-database-verification.md)
- [Ledgertabellen, die nur Anfügevorgänge unterstützen](ledger-append-only-ledger-tables.md)
- [Aktualisierbare Ledgertabellen](ledger-updatable-ledger-tables.md)
- [Zugreifen auf die in Azure Confidential Ledger (ACL) gespeicherten Digests](ledger-how-to-access-acl-digest.md)
