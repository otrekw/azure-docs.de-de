---
title: Migrieren einer Datenbank von SQL Server zu einer Azure Arc-fähigen SQL Managed Instance-Instanz
description: Migrieren einer Datenbank von SQL Server zu einer Azure Arc-fähigen SQL Managed Instance-Instanz
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-data
author: vin-yu
ms.author: vinsonyu
ms.reviewer: mikeray
ms.date: 09/22/2020
ms.topic: how-to
ms.openlocfilehash: 86563b0a44bade2cedaf76af3c247821756111fe
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "90931497"
---
# <a name="migrate-sql-server-to-azure-arc-enabled-sql-managed-instance"></a>Migration: SQL Server zu einer Azure Arc-fähigen SQL Managed Instance-Instanz

In diesem Szenario werden Sie durch die Schritte zum Migrieren einer Datenbank von einer SQL Server-Instanz zu einer Azure SQL Managed Instance-Instanz in Azure Arc geführt. Dabei werden zwei verschiedene Sicherungs- und Wiederherstellungsmethoden verwendet.

[!INCLUDE [azure-arc-data-preview](../../../includes/azure-arc-data-preview.md)]

## <a name="use-azure-blob-storage"></a>Verwendung von Azure Blob Storage 

Verwenden Sie Azure Blob Storage für die Migration zu einer Azure Arc-fähigen SQL Managed Instance-Instanz.

Bei dieser Methode wird Azure Blob Storage als temporärer Speicher genutzt, in dem Sie Sicherungen erstellen und aus dem Sie Daten wiederherstellen können.

### <a name="prerequisites"></a>Voraussetzungen

- [Installieren von Azure Data Studio](install-client-tools.md)
- [Installieren von Azure Storage-Explorer](https://azure.microsoft.com/features/storage-explorer/)
- Azure-Abonnement

### <a name="step-1-provision-azure-blob-storage"></a>Schritt 1: Stellen Sie Azure Blob Storage bereit.

1. Befolgen Sie die Schritte unter [Erstellen eines Azure Blob Storage-Kontos](../../storage/blobs/storage-blob-create-account-block-blob.md?tabs=azure-portal).
1. Starten Sie Azure Storage-Explorer.
1. [Melden Sie sich bei Azure an](../../vs-azure-tools-storage-manage-with-storage-explorer.md?tabs=windows#sign-in-to-azure), um auf den im vorherigen Schritt erstellten Blob Storage-Speicher zuzugreifen.
1. Klicken Sie mit der rechten Maustaste auf das Blob Storage-Konto, und wählen Sie **Blobcontainer erstellen** aus, um einen neuen Container zu erstellen, in dem die Sicherungsdatei gespeichert wird.

### <a name="step-2-get-storage-blob-credentials"></a>Schritt 2: Rufen Sie die Storage Blob-Anmeldeinformationen ab.

1. Klicken Sie im Azure Storage-Explorer mit der rechten Maustaste auf den gerade erstellten Blobcontainer, und wählen Sie **Shared Access Signature abrufen** aus.

1. Wählen Sie Berechtigungen zum **Lesen**, **Schreiben** und **Auflisten** aus.

1. Klicken Sie auf **Erstellen**

   Notieren Sie sich den URI und die Abfragezeichenfolge in diesem Bildschirm. Diese werden später benötigt. Klicken Sie auf die Schaltfläche **Kopieren**, um die Informationen beispielsweise in einem Editor oder in OneNote zu speichern.

1. Schließen Sie das Fenster **Shared Access Signature**.

### <a name="step-3-backup-database-file-to-azure-blob-storage"></a>Schritt 3: Sichern Sie die Datenbankdatei im Azure Blob Storage-Speicher.

In diesem Schritt stellen wir eine Verbindung mit der SQL Server-Quellinstanz her und erstellen die Sicherungsdatei der Datenbank, die zur SQL Managed Instance-Instanz in Azure Arc migriert werden soll.

1. Starten Sie Azure Data Studio.
1. Stellen Sie eine Verbindung mit der SQL Server-Instanz her, die die Datenbank enthält, die Sie zur SQL Managed Instance-Instanz in Azure Arc migrieren möchten.
1. Klicken Sie mit der rechten Maustaste auf die Datenbank, und wählen Sie **Neue Abfrage** aus.
1. Bereiten Sie die Abfrage im folgenden Format vor, und ersetzen Sie die an `<...>` erkennbaren Platzhalter durch die Informationen der Shared Access Signature, die Sie in den vorherigen Schritten abgerufen haben.  Nachdem Sie die Werte ersetzt haben, führen Sie die Abfrage aus.

   ```sql
   IF NOT EXISTS  
   (SELECT * FROM sys.credentials
   WHERE name = 'https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>')  
   CREATE CREDENTIAL [https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>]
     WITH IDENTITY = 'SHARED ACCESS SIGNATURE',  
      SECRET = '<SAS_TOKEN>';  
   ```

1. Bereiten Sie auch den Befehl **BACKUP DATABASE** wie folgt vor, um eine Sicherungsdatei im Blobcontainer zu erstellen.  Nachdem Sie die Werte ersetzt haben, führen Sie die Abfrage aus.

   ```sql
   BACKUP DATABASE <database name> TO URL = 'https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>'
   ```

1. Öffnen Sie den Azure Storage-Explorer, und überprüfen Sie, ob die im vorherigen Schritt erstellte Sicherungsdatei im Blobcontainer angezeigt wird.

### <a name="step-4-restore-the-database-from-azure-blob-storage-to-sql-managed-instance---azure-arc"></a>Schritt 4: Stellen Sie die Datenbank aus dem Azure Blob Storage-Speicher auf der SQL Managed Instance-Instanz in Azure Arc wieder her.

1. Melden Sie sich in Azure Data Studio an, und stellen Sie eine Verbindung mit der SQL Managed Instance-Instanz in Azure Arc her.
1. Erweitern Sie die **Systemdatenbanken**, klicken Sie mit der rechten Maustaste auf die **master**-Datenbank, und wählen Sie **Neue Abfrage** aus.
1. Bereiten Sie im Fenster des Abfrage-Editors dieselbe Abfrage aus dem vorherigen Schritt vor, und führen Sie diese aus, um die Anmeldeinformationen zu erstellen.

   ```sql
   IF NOT EXISTS  
   (SELECT * FROM sys.credentials
   WHERE name = 'https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>')  
   CREATE CREDENTIAL [https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>]
     WITH IDENTITY = 'SHARED ACCESS SIGNATURE',  
     SECRET = '<SAS_TOKEN>';  
   ```

1. Bereiten Sie den unten angegebenen Befehl vor, um sicherzustellen, dass die Sicherungsdatei lesbar und intakt ist, und führen Sie ihn aus.

   ```console
   RESTORE FILELISTONLY FROM URL = 'https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>/<file name>.bak'
   ```

1. Bereiten Sie den Befehl **RESTORE DATABASE** wie folgt vor, und führen Sie diesen aus, um die Sicherungsdatei in einer Datenbank auf einer SQL Managed Instance-Instanz in Azure Arc wiederherzustellen.

   ```sql
   RESTORE DATABASE <database name> FROM URL = 'https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>/<file name>'
   WITH MOVE 'Test' to '/var/opt/mssql/data/<file name>.mdf'
   ,MOVE 'Test_log' to '/var/opt/mssql/data/<file name>.ldf'
   ,RECOVERY  
   ,REPLACE  
   ,STATS = 5;  
   GO
   ```

Hier finden Sie weitere Informationen zum Erstellen von Sicherungen über URLs:

[Dokumente zum Erstellen von Sicherungen über URLs](/sql/relational-databases/backup-restore/sql-server-backup-to-url)

[Erstellen einer Sicherung über eine URL mithilfe von SQL Server Management Studio (SSMS)](/sql/relational-databases/tutorial-sql-server-backup-and-restore-to-azure-blob-storage-service)

-------

## <a name="method-2-copy-the-backup-file-into-an-azure-sql-managed-instance---azure-arc-pod-using-kubectl"></a>Methode 2: Kopieren der Sicherungsdatei in einen Azure SQL Managed Instance-Pod in Azure Arc mithilfe von kubectl

Diese Methode veranschaulicht, wie Sie mithilfe einer beliebigen Methode eine Sicherungsdatei erstellen und diese dann in den lokalen Speicher im Azure SQL Managed Instance-Pod kopieren, sodass Sie von dort eine Wiederherstellung ausführen können, die dem Verfahren in einem typischen Dateisystem unter Windows oder Linux sehr ähnlich ist. In diesem Szenario verwenden Sie den Befehl `kubectl cp`, um die Datei von einem Speicherort in das Dateisystem des Pods zu kopieren.

### <a name="prerequisites"></a>Voraussetzungen

- Installieren und Konfigurieren von kubectl, um auf den Kubernetes-Cluster zu verweisen, in dem Azure Arc-Datendienste bereitgestellt werden
- Auf dem SQL-Server, auf dem Sie die Sicherungsdatei erstellen möchten, muss ein Tool wie Azure Data Studio oder SQL Server Management Server installiert und mit dem SQL-Server verbunden sein. ODER im lokalen Dateisystem muss bereits eine erstellte BAK-Datei vorhanden sein.

### <a name="step-1-backup-the-database-if-you-havent-already"></a>Schritt 1: Sichern Sie die Datenbank, falls dies noch nicht geschehen ist.

Sichern Sie die SQL Server-Datenbank in Ihrem lokalen Dateipfad wie eine beliebige typische SQL Server-Sicherung auf einem Datenträger:

 ```sql
BACKUP DATABASE Test
TO DISK = 'c:\tmp\test.bak'
WITH FORMAT, MEDIANAME = 'Test’ ;
GO
```

### <a name="step-2-copy-the-backup-file-into-the-pods-file-system"></a>Schritt 2: Kopieren Sie die Sicherungsdatei in das Dateisystem des Pods.

Suchen Sie den Namen des Pods, in dem die SQL-Instanz bereitgestellt wird. Dieser sollte in der Regel wie folgt aussehen: `pod/<sqlinstancename>-0`.

Rufen Sie die Liste aller Pods ab, indem Sie folgenden Befehl ausführen:

 ```console
kubectl get pods -n <namespace of data controller>
```

Beispiel:

Kopieren Sie die Sicherungsdatei aus dem lokalen Speicher in den SQL-Pod im Cluster.

 ```console
kubectl cp <source file location> <pod name>:var/opt/mssql/data/<file name> -n <namespace name>

#Example:
kubectl cp C:\Backupfiles\test.bak sqlinstance1-0:var/opt/mssql/data/test.bak -n arc
```

### <a name="step-3-restore-the-database"></a>Schritt 3: Wiederherstellen der Datenbank

Bereiten Sie den Befehl RESTORE vor, und führen Sie diesen aus, um die Sicherungsdatei auf der Azure SQL Managed Instance-Instanz in Azure Arc wiederherzustellen.

```sql
RESTORE DATABASE test FROM DISK = '/var/opt/mssql/data/<file name>.bak'
WITH MOVE '<database name>' to '/var/opt/mssql/data/<file name>.mdf'  
,MOVE '<database name>' to '/var/opt/mssql/data/<file name>_log.ldf'  
,RECOVERY  
,REPLACE  
,STATS = 5;  
GO
```

Beispiel:

```sql
RESTORE DATABASE test FROM DISK = '/var/opt/mssql/data/test.bak'
WITH MOVE 'test' to '/var/opt/mssql/data/test.mdf'  
,MOVE 'test' to '/var/opt/mssql/data/test_log.ldf'  
,RECOVERY  
,REPLACE  
,STATS = 5;  
GO
```


## <a name="next-steps"></a>Nächste Schritte

[Erfahren Sie mehr über Features und Funktionen von Azure Arc-fähigen SQL Managed Instance-Instanzen](managed-instance-features.md).

[Beginnen Sie mit der Erstellung eines Datencontrollers](create-data-controller.md)

[Wurde bereits ein Datencontroller erstellt? Erstellen von Azure Arc-fähigen SQL Managed Instance-Instanzen](create-sql-managed-instance.md)