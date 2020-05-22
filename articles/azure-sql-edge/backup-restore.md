---
title: 'Sichern und Wiederherstellen von Datenbanken: Azure SQL Edge (Vorschau)'
description: Informieren Sie sich über die Sicherungs- und Wiederherstellungsfunktionen in Azure SQL Edge (Vorschau).
keywords: ''
services: sql-database-edge
ms.service: sql-database-edge
ms.topic: conceptual
author: SQLSourabh
ms.author: sourabha
ms.reviewer: sstein
ms.date: 05/19/2020
ms.openlocfilehash: 5c7bdbc49d8f1c6af7f38911919c660b03e1a37a
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2020
ms.locfileid: "83593999"
---
# <a name="backup-and-restore-databases-in-azure-sql-edge-preview"></a>Sichern und Wiederherstellen von Datenbanken in Azure SQL Edge (Vorschau) 

Azure SQL Edge basiert auf den neuesten Versionen der Microsoft SQL Server-Datenbank-Engine unter Linux und bietet ähnliche Funktionen zum Sichern und Wiederherstellen von Datenbanken, wie Sie in SQL Server unter Linux und für SQL Server-Instanzen, die in Containern ausgeführt werden, verfügbar sind. Durch die Sicherungs- und Wiederherstellungskomponente wird eine wichtige Vorrichtung zum Schutz von in Azure SQL Edge-Datenbanken gespeicherten Daten bereitgestellt. Um die Gefahr eines schwerwiegenden Datenverlusts zu minimieren, wird empfohlen, Ihre Datenbanken regelmäßig zu sichern, um Änderungen an Ihren Daten zu schützen. Eine gut geplante Sicherungs- und Wiederherstellungsstrategie hilft, Datenbanken vor Datenverlusten zu schützen, die durch eine Vielzahl von Fehlern verursacht werden können. Testen Sie Ihre Strategie, indem Sie einen Sicherungssatz und anschließend Ihre Datenbank wiederherstellen, um sich auf die effektive Reaktion auf einen Notfall vorzubereiten.

Weitere Informationen dazu, warum Sicherungen wichtig sind, finden Sie unter [Sichern und Wiederherstellen von SQL Server-Datenbanken](/sql/relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases/).

Azure SQL Edge unterstützt die Sicherung und Wiederherstellung mit lokalem Speicher und mit Azure-Blobs. Weitere Informationen zur Sicherung und Wiederherstellung mit Azure Blob Storage finden Sie unter [SQL Server-Sicherung und -Wiederherstellung mit dem Microsoft Azure Blob-Dienst](/sql/relational-databases/backup-restore/sql-server-backup-and-restore-with-microsoft-azure-blob-storage-service/) und [SQL Server-Sicherung über URLs](/sql/relational-databases/backup-restore/sql-server-backup-to-url).

## <a name="backing-up-a-database-in-azure-sql-edge"></a>Sichern einer Datenbank in Azure SQL Edge

Azure SQL Edge unterstützt die gleichen Sicherungstypen, die von SQL Server unterstützt werden. Eine vollständige Liste der Sicherungstypen, die in SQL Server unterstützt werden, finden Sie unter [Sicherung: Übersicht](/sql/relational-databases/backup-restore/backup-overview-sql-server/).

> [!IMPORTANT] 
> In Azure SQL Edge erstellte Datenbanken verwenden standardmäßig das einfache Wiederherstellungsmodell. Daher können keine Protokollsicherungen für diese Datenbanken ausgeführt werden. Wenn für diese Datenbanken Protokollsicherungen erforderlich sind, muss ein Administrator das Datenbankwiederherstellungsmodell in das vollständige Wiederherstellungsmodell ändern. Eine vollständige Liste der von SQL Server unterstützten Wiederherstellungsmodelle finden Sie unter [Übersicht über das Wiederherstellungsmodell](/sql/relational-databases/backup-restore/recovery-models-sql-server#RMov).

### <a name="backup-to-local-disk"></a>Sicherung auf lokalem Datenträger

Im unten angegebenen Beispiel wird der Transact-SQL-Befehl BACKUP DATABASE verwendet, um eine Datenbanksicherung im Container zu erstellen. Im Rahmen dieses Beispiels wird ein neuer Ordner mit dem Namen „backup“ erstellt, um die Sicherungsdateien zu speichern.

1. Erstellen Sie einen Ordner für die Sicherungen. Dieser Befehl muss auf dem Host ausgeführt werden, auf dem der Azure SQL Edge-Container ausgeführt wird. Ersetzen Sie im Befehl unten **<AzureSQLEdge_Container_Name>** durch den Namen des Azure SQL Edge-Containers in Ihrer Bereitstellung.

    ```bash
    sudo docker exec -it <AzureSQLEdge_Container_Name> mkdir /var/opt/mssql/backup
    ```

2. Stellen Sie mithilfe von SQL Server Management Studio (SSMS) oder Azure Data Studio (ADS) eine Verbindung mit der Azure SQL Edge-Instanz her, und führen Sie den Befehl „Backup Database“ aus, um die Sicherung Ihrer Benutzerdatenbank zu erstellen. Im folgenden Beispiel wird eine Sicherung der Datenbank *IronOreSilicaPrediction* erstellt.

    ```sql
    BACKUP DATABASE [IronOreSilicaPrediction] 
    TO DISK = N'/var/opt/mssql/backup/IronOrePredictDB.bak' 
    WITH NOFORMAT, NOINIT,  NAME = N'IronOreSilicaPrediction-Full Database Backup', 
    SKIP, NOREWIND, NOUNLOAD, COMPRESSION,  STATS = 10
    GO
    ```

3. Nachdem Sie den Befehl ausgeführt haben und die Sicherung der Datenbank erfolgreich war, werden im Abschnitt „Results“ (Ergebnisse) von SSMS oder ADS Meldungen angezeigt, die den folgenden Meldungen ähneln.

    ```txt
    10 percent processed.
    20 percent processed.
    30 percent processed.
    40 percent processed.
    50 percent processed.
    60 percent processed.
    70 percent processed.
    80 percent processed.
    90 percent processed.
    100 percent processed.
    Processed 51648 pages for database 'IronOreSilicaPrediction', file 'IronOreSilicaPrediction' on file 1.
    Processed 2 pages for database 'IronOreSilicaPrediction', file 'IronOreSilicaPrediction_log' on file 1.
    BACKUP DATABASE successfully processed 51650 pages in 3.588 seconds (112.461 MB/sec).

    Completion time: 2020-04-09T23:54:48.4957691-07:00
    ```

### <a name="backup-to-url"></a>Sicherung über URLs

Azure SQL Edge unterstützt Sicherungen in Seitenblobs und in Blockblobs. Weitere Informationen zu Seitenblobs und Blockblobs finden Sie unter [Sicherung in Blockblobs im Vergleich zu Seitenblobs](https://docs.microsoft.com/sql/relational-databases/backup-restore/sql-server-backup-to-url?view=sql-server-ver15#blockbloborpageblob). Im folgenden Beispiel wird die Datenbank *IronOreSilicaPrediction* in einem Blockblob gesichert. 

1. Der erste Schritt beim Konfigurieren von Sicherungen für Blockblobs besteht darin, ein SAS-Token (Shared Access Signature) zu generieren, mit dem SQL Server-Anmeldeinformationen für Azure SQL Edge erstellt werden kann. Das Skript erstellt eine Shared Access Signature, die einer gespeicherten Zugriffsrichtlinie zugeordnet ist. Weitere Informationen finden Sie unter [Shared Access Signatures, Teil 1: Grundlegendes zum SAS-Modell](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/). Das Skript schreibt auch den T-SQL-Befehl, der zum Erstellen der Anmeldeinformationen unter SQL Server erforderlich ist. Im folgenden Skript wird davon ausgegangen, dass Sie bereits über ein Azure-Abonnement mit einem Speicherkonto und einen Speichercontainer für die Sicherungen verfügen.

    ```PowerShell
    # Define global variables for the script  
    $subscriptionName='<your subscription name>'   # the name of subscription name you will use
    $resourcegroupName = '<your resource group name>' # the name of resource group you will use
    $storageAccountName= '<your storage account name>' # the storage account name you will use for backups
    $containerName= '<your storage container name>'  # the storage container name to which you will attach the SAS policy with its SAS token  
    $policyName = 'SASPolicy' # the name of the SAS policy  

    # adds an authenticated Azure account for use in the session
    Login-AzAccount

    # set the tenant, subscription and environment for use in the rest of
    Select-AzSubscription -Subscription $subscriptionName

    # Generate the SAS token
    $sa = Get-AzStorageAccount -ResourceGroupName $resourcegroupName -Name $storageAccountName 
    $storagekey = Get-AzStorageAccountKey -ResourceGroupName $resourcegroupName -Name $storageAccountName 
    $storageContext = New-AzStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storagekey[0].Value
    $cbc = Get-AzStorageContainer -Name $containerName -Context $storageContext
    $policy = New-AzStorageContainerStoredAccessPolicy -Container $containerName -Policy $policyName -Context $storageContext -ExpiryTime $(Get-Date).ToUniversalTime().AddYears(10) -Permission "rwld"
    $sas = New-AzStorageContainerSASToken -Policy $policyName -Context $storageContext -Container $containerName
    Write-Host 'Shared Access Signature= '$($sas.Substring(1))''

    # Outputs the Transact SQL to the clipboard and to the screen to create the credential using the Shared Access Signature  
    Write-Host 'Credential T-SQL'  
    $tSql = "CREATE CREDENTIAL [{0}] WITH IDENTITY='Shared Access Signature', SECRET='{1}'" -f $cbc.CloudBlobContainer.Uri.AbsoluteUri,$sas.Substring(1)
    $tSql | clip  
    Write-Host $tSql
    ```

    Nachdem Sie das Skript erfolgreich ausgeführt haben, kopieren Sie den Befehl CREATE CREDENTIAL in ein Abfragetool, stellen eine Verbindung mit einer SQL Server-Instanz her und führen den Befehl zum Erstellen der Anmeldeinformationen mit der Shared Access Signature aus.

2. Stellen Sie mithilfe von SQL Server Management Studio (SSMS) oder Azure Data Studio (ADS) eine Verbindung mit der Azure SQL Edge-Instanz her, und erstellen Sie die Anmeldeinformationen dann mithilfe des Befehls aus dem vorherigen Schritt. Stellen Sie sicher, dass Sie den Befehl CREATE CREDENTIAL durch die tatsächliche Ausgabe aus dem vorherigen Schritt ersetzen.

    ```sql
    IF NOT EXISTS  
    (SELECT * FROM sys.credentials
    WHERE name = 'https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>')  
    CREATE CREDENTIAL [https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>]
       WITH IDENTITY = 'SHARED ACCESS SIGNATURE',  
       SECRET = '<SAS_TOKEN>';
    ```

3. Der folgende Befehl erstellt eine Sicherung von *IronOreSilicaPrediction* im Azure-Speichercontainer.

    ```sql
    BACKUP DATABASE IronOreSilicaPrediction
    TO URL = 'https://<mystorageaccountname>.blob.core.windows.net/<mycontainername>/IronOreSilicaPrediction.bak'
    With MAXTRANSFERSIZE = 4194304,BLOCKSIZE=65536;  
    GO
    ```

## <a name="restoring-a-database-in-azure-sql-edge"></a>Wiederherstellen einer Datenbank in Azure SQL Edge

Azure SQL Edge unterstützt die Wiederherstellung von einem lokalen Datenträger, von einem Netzwerkspeicherort oder aus einem Azure Blob Storage-Konto. Eine Übersicht über Wiederherstellungsvorgänge in SQL Server finden Sie unter [Übersicht über Wiederherstellungsvorgänge ](https://docs.microsoft.com/sql/relational-databases/backup-restore/restore-and-recovery-overview-sql-server?view=sql-server-ver15). Eine Übersicht über das einfache Wiederherstellungsmodell in SQL Server finden Sie unter [Vollständige Datenbankwiederherstellungen (einfaches Wiederherstellungsmodell)](https://docs.microsoft.com/sql/relational-databases/backup-restore/complete-database-restores-simple-recovery-model?view=sql-server-ver15).

### <a name="restore-from-local-disk"></a>Wiederherstellung von lokalem Datenträger

In diesem Beispiel wird die *IronOreSilicaPrediction*-Sicherung, die im vorherigen Beispiel durchgeführt wurde, für die Wiederherstellung als neue Datenbank mit einem anderen Namen verwendet.

1. Wenn die Datenbanksicherungsdatei nicht bereits im Container vorhanden ist, können Sie den folgenden Befehl verwenden, um die Datei in den Container zu kopieren. Im folgenden Beispiel wird davon ausgegangen, dass die Sicherungsdatei auf dem lokalen Host vorhanden und in den Ordner „/var/opt/mssqlbBackup“ in einen Azure SQL Edge-Container mit dem Namen „sql1“ kopiert wird.

    ```bash
    sudo docker cp IronOrePredictDB.bak sql1:/var/opt/mssql/backup
    ```

2. Stellen Sie mithilfe von SQL Server Management Studio (SSMS) oder Azure Data Studio (ADS) eine Verbindung mit der Azure SQL Edge-Instanz her, um den Wiederherstellungsbefehl auszuführen. Im folgenden Beispiel wird **IronOrePredictDB.bak** wiederhergestellt, um eine neue Datenbank **IronOreSilicaPrediction_2** zu erstellen.

    ```sql
    Restore FilelistOnly from disk = N'/var/opt/mssql/backup/IronOrePredictDB.bak'

    Restore Database IronOreSilicaPrediction_2
    From disk = N'/var/opt/mssql/backup/IronOrePredictDB.bak'
    WITH MOVE 'IronOreSilicaPrediction' TO '/var/opt/mssql/data/IronOreSilicaPrediction_Primary_2.mdf',
    MOVE 'IronOreSilicaPrediction_log' TO '/var/opt/mssql/data/IronOreSilicaPrediction_Primary_2.ldf'
    ```

3. Nachdem Sie den Wiederherstellungsvorgangbefehl ausgeführt haben und der Wiederherstellungsvorgang erfolgreich war, sehen im Ausgabefenster Meldungen, die den folgenden Meldungen ähneln. 

    ```txt
    Processed 51648 pages for database 'IronOreSilicaPrediction_2', file 'IronOreSilicaPrediction' on file 1.
    Processed 2 pages for database 'IronOreSilicaPrediction_2', file 'IronOreSilicaPrediction_log' on file 1.
    RESTORE DATABASE successfully processed 51650 pages in 6.543 seconds (61.670 MB/sec).

    Completion time: 2020-04-13T23:49:21.1600986-07:00
    ```

### <a name="restore-from-url"></a>Wiederherstellen aus URL

Azure SQL Edge unterstützt auch das Wiederherstellen einer Datenbank aus einem Azure Storage-Konto. Wiederherstellungen können aus den Blockblob- oder Seitenblobsicherungen ausgeführt werden. Im unten gezeigten Beispiel wird die Datenbanksicherungsdatei *IronOreSilicaPrediction_2020_04_16.bak* aus einem Blockblob wiederhergestellt, um die Datenbank *IronOreSilicaPrediction_3* zu erstellen.

```sql
RESTORE DATABASE IronOreSilicaPrediction_3
FROM URL = 'https://mystorageaccount.blob.core.windows.net/mysecondcontainer/IronOreSilicaPrediction_2020_04_16.bak'
WITH MOVE 'IronOreSilicaPrediction' TO '/var/opt/mssql/data/IronOreSilicaPrediction_Primary_3.mdf', 
MOVE 'IronOreSilicaPrediction_log' TO '/var/opt/mssql/data/IronOreSilicaPrediction_Primary_3.ldf',
STATS = 10;
```


