---
title: 'Schnellstart: Wiederherstellen einer Sicherung (SSMS)'
titleSuffix: Azure SQL Managed Instance
description: In dieser Schnellstartanleitung erfahren Sie, wie Sie eine Datenbanksicherung mithilfe von SQL Server Management Studio (SSMS) in Azure SQL Managed Instance wiederherstellen.
services: sql-database
ms.service: sql-managed-instance
ms.subservice: operations
ms.custom: ''
ms.devlang: ''
ms.topic: quickstart
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: sstein, bonova
ms.date: 12/14/2018
ms.openlocfilehash: 9b2333e38415a2c0ad50ce36c213ead711c70ab4
ms.sourcegitcommit: 1756a8a1485c290c46cc40bc869702b8c8454016
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/09/2020
ms.locfileid: "96928799"
---
# <a name="quickstart-restore-a-database-to-azure-sql-managed-instance-with-ssms"></a>Schnellstart: Wiederherstellen einer Datenbank in Azure SQL Managed Instance mit SSMS
[!INCLUDE[appliesto-sqlmi](../includes/appliesto-sqlmi.md)]

In dieser Schnellstartanleitung verwenden Sie SQL Server Management Studio (SSMS), um eine Datenbank (die Sicherungsdatei „Wide World Importers – Standard“) aus Azure Blob Storage in [Azure SQL Managed Instance](sql-managed-instance-paas-overview.md) wiederherzustellen.

> [!VIDEO https://www.youtube.com/embed/RxWYojo_Y3Q]

> [!NOTE]
> Weitere Informationen zur Migration unter Verwendung von Azure Database Migration Service finden Sie unter [Tutorial: Migrieren von SQL Server zu SQL Managed Instance mithilfe von DMS (offline)](../../dms/tutorial-sql-server-to-managed-instance.md).
> Weitere Informationen zu verschiedenen Migrationsmethoden finden Sie unter [Migration von SQL Server zu einer verwalteten Azure SQL-Instanz](migrate-to-instance-from-sql-server.md).

## <a name="prerequisites"></a>Voraussetzungen

In dieser Schnellstartanleitung gilt Folgendes:

- Es werden Ressourcen aus der Schnellstartanleitung [Erstellen einer verwalteten Azure SQL-Instanz](instance-create-quickstart.md) verwendet.
- Die aktuelle Version von [SSMS](/sql/ssms/sql-server-management-studio-ssms) muss installiert sein.
- Die Verbindungsherstellung mit SQL Managed Instance muss über SSMS erfolgen. Informationen zum Herstellen einer Verbindung finden Sie in den folgenden Schnellstartanleitungen:
  - [Aktivieren eines öffentlichen Endpunkts](public-endpoint-configure.md) für SQL Managed Instance (wird für dieses Tutorial empfohlen)
  - [Verbinden mit SQL Managed Instance über einen virtuellen Azure-Computer](connect-vm-instance-configure.md)
  - [Konfigurieren einer Point-to-Site-Verbindung mit SQL Managed Instance über einen lokalen Computer](point-to-site-p2s-configure.md)

> [!NOTE]
> Weitere Informationen zum Sichern und Wiederherstellen einer SQL Server-Datenbank mithilfe von Azure Blob Storage und einem [SAS-Schlüssel (Shared Access Signature)](../../storage/common/storage-sas-overview.md) finden Sie unter [SQL Server-Sicherung über URLs](/sql/relational-databases/backup-restore/sql-server-backup-to-url?view=sql-server-2017).

## <a name="restore-from-a-backup-file"></a>Wiederherstellen aus einer Sicherungsdatei

Führen Sie in SQL Server Management Studio die folgenden Schritte aus, um die Wide World Importers-Datenbank in SQL Managed Instance wiederherzustellen. Die Datenbanksicherungsdatei ist in einem vorkonfigurierten Azure Blob Storage-Konto gespeichert.

1. Öffnen Sie SSMS, und stellen Sie eine Verbindung mit Ihrer verwalteten Instanz her.
2. Klicken Sie im **Objekt-Explorer** mit der rechten Maustaste auf Ihre verwaltete Instanz, und wählen Sie **Neue Abfrage** aus, um ein neues Abfragefenster zu öffnen.
3. Führen Sie das folgende SQL-Skript aus. Dieses Skript verwendet ein vorkonfiguriertes Speicherkonto und einen SAS-Schlüssel zum [Erstellen von Anmeldeinformationen](/sql/t-sql/statements/create-credential-transact-sql) in Ihrer verwalteten Instanz.
 
   > [!IMPORTANT]
   > `CREDENTIAL` muss dem Containerpfad entsprechen, mit `https` beginnen und darf nicht mit einem Schrägstrich enden. `IDENTITY` muss den Wert `SHARED ACCESS SIGNATURE` haben. `SECRET` muss das Shared Access Signature-Token sein und darf kein vorangestelltes `?` enthalten.

   ```sql
   CREATE CREDENTIAL [https://mitutorials.blob.core.windows.net/databases]
   WITH IDENTITY = 'SHARED ACCESS SIGNATURE'
   , SECRET = 'sv=2017-11-09&ss=bfqt&srt=sco&sp=rwdlacup&se=2028-09-06T02:52:55Z&st=2018-09-04T18:52:55Z&spr=https&sig=WOTiM%2FS4GVF%2FEEs9DGQR9Im0W%2BwndxW2CQ7%2B5fHd7Is%3D'
   ```

    ![Erstellen von Anmeldeinformationen](./media/restore-sample-database-quickstart/credential.png)

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

    ![Screenshot zeigt das Skript, das in Objekt-Explorer ausgeführt wird, mit einer Erfolgsmeldung.](./media/restore-sample-database-quickstart/restore.png)

6. Führen Sie das folgende Skript aus, um den Status Ihrer Wiederherstellung nachzuverfolgen.

   ```sql
   SELECT session_id as SPID, command, a.text AS Query, start_time, percent_complete
      , dateadd(second,estimated_completion_time/1000, getdate()) as estimated_completion_time
   FROM sys.dm_exec_requests r
   CROSS APPLY sys.dm_exec_sql_text(r.sql_handle) a
   WHERE r.command in ('BACKUP DATABASE','RESTORE DATABASE')
   ```

7. Nachdem die Wiederherstellung abgeschlossen wurde, zeigen Sie die Datenbank im Objekt-Explorer an. Mithilfe der Ansicht [sys.dm_operation_status](/sql/relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database) können Sie überprüfen, ob die Datenbankwiederherstellung abgeschlossen wurde.

> [!NOTE]
> Der Vorgang der Datenbankwiederherstellung ist asynchron und wiederholbar. Möglicherweise erhalten Sie eine Fehlermeldung in SQL Server Management Studio, falls die Verbindung unterbrochen oder ein Timeout überschritten wird. Azure SQL-Datenbank versucht weiterhin, die Datenbank im Hintergrund wiederherzustellen, und Sie können den Wiederherstellungsfortschritt mithilfe der Ansichten [sys.dm_exec_requests](/sql/relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql) und [sys.dm_operation_status](/sql/relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database) verfolgen.
> In einigen Phasen des Wiederherstellungsprozesses wird ein eindeutiger Bezeichner anstelle des tatsächlichen Datenbanknamens in den Systemansichten angezeigt. Informationen zum unterschiedlichen Verhalten bei der `RESTORE`-Anweisung finden Sie [hier](./transact-sql-tsql-differences-sql-server.md#restore-statement).

## <a name="next-steps"></a>Nächste Schritte

- Wenn in Schritt 5 eine Datenbankwiederherstellung mit der Nachrichten-ID 22003 beendet wird, erstellen Sie eine neue Sicherungsdatei mit Sicherungsprüfsummen, und führen Sie die Wiederherstellung erneut durch. Weitere Informationen finden Sie unter [Aktivieren oder Deaktivieren von Sicherungsprüfsummen während der Sicherung oder Wiederherstellung](/sql/relational-databases/backup-restore/enable-or-disable-backup-checksums-during-backup-or-restore-sql-server).
- Informationen zum Beheben von Problemen bei URL-basierten Sicherungen finden Sie unter [SQL Server-Sicherung über URLs – bewährte Methoden und Problembehandlung](/sql/relational-databases/backup-restore/sql-server-backup-to-url-best-practices-and-troubleshooting).
- Eine Übersicht über App-Verbindungsoptionen finden Sie unter [Herstellen einer Verbindung zwischen einer Anwendung und einer verwalteten SQL-Instanz](connect-application-instance.md).
- Informationen zu Abfragen mit Ihren bevorzugten Tools oder Sprachen finden Sie unter [Schnellstarts: Artikel zum Verbinden mit und Abfragen von Azure SQL-Datenbank und Azure SQL Managed Instance](../database/connect-query-content-reference-guide.md).
