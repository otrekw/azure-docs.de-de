---
title: 'Schnellstart: Wiederherstellen einer Sicherung (SSMS)'
titleSuffix: Azure SQL SQL Managed Instance
description: In dieser Schnellstartanleitung erfahren Sie, wie Sie eine Datenbanksicherung mithilfe von SQL Server Management (SSMS) auf einer verwalteten Azure SQL-Instanz wiederherstellen.
services: sql-database
ms.service: sql-database
ms.subservice: operations
ms.custom: ''
ms.devlang: ''
ms.topic: quickstart
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: sstein, carlrab, bonova
ms.date: 12/14/2018
ms.openlocfilehash: c750912e942d5dadeb97e6675427f1730912704a
ms.sourcegitcommit: 309cf6876d906425a0d6f72deceb9ecd231d387c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/01/2020
ms.locfileid: "84267578"
---
# <a name="quickstart-restore-a-database-to-an-azure-sql-managed-instance-with-ssms"></a>Schnellstart: Wiederherstellen einer Datenbank auf einer verwalteten Azure SQL-Instanz mit SSMS
[!INCLUDE[appliesto-sqlmi](../includes/appliesto-sqlmi.md)]

In diesem Schnellstart verwenden Sie SQL Server Management Studio (SSMS), um eine Datenbank (die Sicherungsdatei „Wide World Importers – Standard“) aus Azure Blob Storage in einer [verwalteten Azure SQL-Instanz](sql-managed-instance-paas-overview.md) wiederherzustellen.

> [!VIDEO https://www.youtube.com/embed/RxWYojo_Y3Q]

> [!NOTE]
> Weitere Informationen zur Migration unter Verwendung von Azure Database Migration Service (DMS) finden Sie unter [Migrieren einer verwalteten SQL-Instanz mit DMS](../../dms/tutorial-sql-server-to-managed-instance.md).
> Weitere Informationen zu verschiedenen Migrationsmethoden finden Sie unter [Migration von SQL Server zu einer verwalteten Azure SQL-Instanz](migrate-to-instance-from-sql-server.md).

## <a name="prerequisites"></a>Voraussetzungen

In dieser Schnellstartanleitung gilt Folgendes:

- Es werden Ressourcen aus der Schnellstartanleitung [Erstellen einer verwalteten SQL-Instanz](instance-create-quickstart.md) verwendet.
- Erfordert die Installation der aktuellen Version von [SQL Server Management Studio (SSMS)](https://docs.microsoft.com/sql/ssms/sql-server-management-studio-ssms).
- Die Verbindungsherstellung mit Ihrer verwalteten SQL-Instanz muss über SSMS erfolgen. Informationen zum Herstellen einer Verbindung finden Sie in den folgenden Schnellstartanleitungen:
  - [Aktivieren des öffentlichen Endpunkts](public-endpoint-configure.md) für die verwaltete SQL-Instanz (wird für dieses Tutorial empfohlen)
  - [Verbinden eines virtuellen Azure-Computers mit einer verwalteten SQL-Instanz](connect-vm-instance-configure.md)
  - [Konfigurieren einer Point-to-Site-Verbindung von einem lokalen Computer mit einer verwalteten SQL-Instanz](point-to-site-p2s-configure.md)

> [!NOTE]
> Weitere Informationen zum Sichern und Wiederherstellen einer SQL Server-Datenbank mithilfe von Azure Blob Storage und einem [SAS-Schlüssel (Shared Access Signature)](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1) finden Sie unter [SQL Server-Sicherung über URLs](https://docs.microsoft.com/sql/relational-databases/backup-restore/sql-server-backup-to-url?view=sql-server-2017).

## <a name="restore-from-a-backup-file"></a>Wiederherstellen aus einer Sicherungsdatei

Führen Sie in SQL Server Management Studio (SSMS) die folgenden Schritte aus, um die Wide World Importers-Datenbank in Ihrer verwalteten SQL-Instanz wiederherzustellen. Die Datenbanksicherungsdatei ist in einem vorkonfigurierten Azure Blob Storage-Konto gespeichert.

1. Öffnen Sie SSMS, und stellen Sie eine Verbindung mit Ihrer verwalteten SQL-Instanz her.
2. Klicken Sie im **Objekt-Explorer** mit der rechten Maustaste auf Ihre verwaltete SQL-Instanz, und wählen Sie **Neue Abfrage** aus, um ein neues Abfragefenster zu öffnen.
3. Führen Sie das folgende SQL-Skript aus. Dieses Skript verwendet ein vorkonfiguriertes Speicherkonto und einen SAS-Schlüssel zum [Erstellen von Anmeldeinformationen](https://docs.microsoft.com/sql/t-sql/statements/create-credential-transact-sql) in Ihrer verwalteten SQL-Instanz.

   ```sql
   CREATE CREDENTIAL [https://mitutorials.blob.core.windows.net/databases]
   WITH IDENTITY = 'SHARED ACCESS SIGNATURE'
   , SECRET = 'sv=2017-11-09&ss=bfqt&srt=sco&sp=rwdlacup&se=2028-09-06T02:52:55Z&st=2018-09-04T18:52:55Z&spr=https&sig=WOTiM%2FS4GVF%2FEEs9DGQR9Im0W%2BwndxW2CQ7%2B5fHd7Is%3D'
   ```

    ![erstellen von anmeldeinformationen](./media/restore-sample-database-quickstart/credential.png)

4. Führen Sie zum Überprüfen Ihrer Anmeldeinformationen das folgende Skript aus. Dieses Skript verwendet eine [Container](https://azure.microsoft.com/services/container-instances/)-URL, um eine Sicherungsdateiliste abzurufen.

   ```sql
   RESTORE FILELISTONLY FROM URL =
      'https://mitutorials.blob.core.windows.net/databases/WideWorldImporters-Standard.bak'
   ```

    ![Dateiliste](./media/restore-sample-database-quickstart/file-list.png)

5. Führen Sie das folgende Skript aus, um die Wide World Importers-Datenbank wiederherzustellen.

   ```sql
   RESTORE DATABASE [Wide World Importers] FROM URL =
     'https://mitutorials.blob.core.windows.net/databases/WideWorldImporters-Standard.bak'
   ```

    ![Wiederherstellen](./media/restore-sample-database-quickstart/restore.png)

6. Führen Sie das folgende Skript aus, um den Status Ihrer Wiederherstellung nachzuverfolgen.

   ```sql
   SELECT session_id as SPID, command, a.text AS Query, start_time, percent_complete
      , dateadd(second,estimated_completion_time/1000, getdate()) as estimated_completion_time
   FROM sys.dm_exec_requests r
   CROSS APPLY sys.dm_exec_sql_text(r.sql_handle) a
   WHERE r.command in ('BACKUP DATABASE','RESTORE DATABASE')
   ```

7. Nachdem die Wiederherstellung abgeschlossen wurde, zeigen Sie die Datenbank im Objekt-Explorer an. Mithilfe der Sicht [sys.dm_operation_status](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database) können Sie überprüfen, ob die Datenbankwiederherstellung abgeschlossen wurde.

> [!NOTE]
> Der Vorgang der Datenbankwiederherstellung ist asynchron und wiederholbar. Möglicherweise erhalten Sie eine Fehlermeldung in SQL Server Management Studio, falls die Verbindung unterbrochen oder ein Timeout überschritten wird. Azure SQL-Datenbank versucht weiterhin, die Datenbank im Hintergrund wiederherzustellen, und Sie können den Wiederherstellungsfortschritt mithilfe der Sichten [sys.dm_exec_requests](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql) und [sys.dm_operation_status](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database) verfolgen.
> In einigen Phasen des Wiederherstellungsprozesses wird der eindeutige Bezeichner anstelle des tatsächlichen Daten Banknamens in den Systemsichten angezeigt. Informationen zum unterschiedlichen Verhalten bei der `RESTORE`-Anweisung finden Sie [hier](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-transact-sql-information#restore-statement).

## <a name="next-steps"></a>Nächste Schritte

- Wenn in Schritt 5 eine Datenbankwiederherstellung mit der Nachrichten-ID 22003 beendet wird, erstellen Sie eine neue Sicherungsdatei mit Sicherungsprüfsummen, und führen Sie die Wiederherstellung erneut durch. Weitere Informationen finden Sie unter [Aktivieren oder Deaktivieren von Sicherungsprüfsummen während der Sicherung oder Wiederherstellung](https://docs.microsoft.com/sql/relational-databases/backup-restore/enable-or-disable-backup-checksums-during-backup-or-restore-sql-server).
- Informationen zum Beheben von Problemen bei URL-basierten Sicherungen finden Sie unter [SQL Server-Sicherung über URLs – bewährte Methoden und Problembehandlung](https://docs.microsoft.com/sql/relational-databases/backup-restore/sql-server-backup-to-url-best-practices-and-troubleshooting).
- Eine Übersicht über App-Verbindungsoptionen finden Sie unter [Herstellen einer Verbindung zwischen einer Anwendung und einer verwalteten SQL-Instanz](connect-application-instance.md).
- Informationen zu Abfragen mit Ihren bevorzugten Tools oder Sprachen finden Sie unter [Schnellstarts: Azure SQL-Datenbank – Verbindungsherstellung und Abfragen](../database/connect-query-content-reference-guide.md).
