---
title: 'Mit PowerShell: Onlinemigration von SQL Server zu SQL Managed Instance'
titleSuffix: Azure Database Migration Service
description: Hier erfahren Sie, wie Sie mit Azure PowerShell und Azure Database Migration Service eine Onlinemigration einer SQL Server-Instanz zu einer Instanz in Azure SQL Managed Instance durchführen.
services: database-migration
author: pochiraju
ms.author: rajpo
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: ''
ms.topic: how-to
ms.date: 12/16/2020
ms.openlocfilehash: 7f09db2e1f98d48e91dfea2642969ff4ca360967
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "98697735"
---
# <a name="migrate-sql-server-to-sql-managed-instance-online-with-powershell--azure-database-migration-service"></a>Onlinemigration von SQL Server zu Azure SQL Managed Instance mit PowerShell und Azure Database Migration Service

In diesem Artikel migrieren Sie die Datenbank **Adventureworks2016** online, die in einer lokalen Instanz von SQL Server 2005 (oder höher) wiederhergestellt wurde, mithilfe von Microsoft Azure PowerShell zu einer Instanz in Azure SQL Managed Instance. Mithilfe des Moduls `Az.DataMigration` in Microsoft Azure PowerShell können Sie Datenbanken aus einer SQL Server-Instanz zu einer Instanz in Azure SQL Managed Instance migrieren.

In diesem Artikel werden folgende Vorgehensweisen behandelt:
> [!div class="checklist"]
>
> * Erstellen Sie eine Ressourcengruppe.
> * Erstellen einer Instanz von Azure Database Migration Service
> * Erstellen eines Migrationsprojekts in einer Instanz von Azure Database Migration Service.
> * Führen Sie die Migration online aus.

[!INCLUDE [online-offline](../../includes/database-migration-service-offline-online.md)]

In diesem Artikel werden die Schritte für eine Onlinemigration beschrieben, es ist aber auch möglich, [offline](howto-sql-server-to-azure-sql-managed-instance-powershell-offline.md) zu migrieren.


## <a name="prerequisites"></a>Voraussetzungen

Zum Ausführen dieser Schritte benötigen Sie Folgendes:

* [SQL Server 2016 oder höher](https://www.microsoft.com/sql-server/sql-server-downloads) (beliebige Edition).
* Eine lokale Kopie der **AdventureWorks2016**-Datenbank, die [hier](/sql/samples/adventureworks-install-configure) zum Download verfügbar ist.
* Aktivieren Sie das TCP/IP-Protokoll, das in einer SQL Server Express-Installation standardmäßig deaktiviert ist. Aktivieren Sie das TCP/IP-Protokoll anhand des Artikels [Aktivieren oder Deaktivieren eines Servernetzwerkprotokolls](/sql/database-engine/configure-windows/enable-or-disable-a-server-network-protocol#SSMSProcedure).
* Konfigurieren Sie Ihre [Windows-Firewall für Datenbank-Engine-Zugriff](/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access).
* Ein Azure-Abonnement. Falls Sie kein Abonnement besitzen, können Sie ein [kostenloses Konto erstellen](https://azure.microsoft.com/free/), bevor Sie beginnen.
* Eine Instanz in Azure SQL Managed Instance. Sie können eine Instanz in Azure SQL Managed Instance erstellen, indem Sie die Anweisungen im Artikel [Erstellen einer Instanz in Azure SQL Managed Instance](../azure-sql/managed-instance/instance-create-quickstart.md) befolgen.
* Laden Sie den [Data Migration Assistant](https://www.microsoft.com/download/details.aspx?id=53595), Version 3.3 oder höher, herunter, und installieren Sie ihn.
* Ein Microsoft Azure Virtual Network, das mit dem Azure Resource Manager-Bereitstellungsmodell erstellt wurde, das dem Azure Database Migration Service entweder über [ExpressRoute](../expressroute/expressroute-introduction.md) oder über [VPN](../vpn-gateway/vpn-gateway-about-vpngateways.md) Site-to-Site-Konnektivität für Ihre lokalen Quellserver bereitstellt.
* Eine abgeschlossene Bewertung der lokalen Datenbank und Schemamigration mithilfe von Data Migration Assistant wie im Artikel [Durchführen einer SQL Server-Migrationsbewertung](/sql/dma/dma-assesssqlonprem) beschrieben.
* Laden Sie das `Az.DataMigration`-Modul (Version 0.7.2 oder höher) aus dem PowerShell-Katalog mit dem [PowerShell-Cmdlet „Install-Module“](/powershell/module/powershellget/Install-Module) herunter und installieren Sie es.
* Stellen Sie sicher, dass die für die Verbindung mit der SQL Server-Quellinstanz verwendeten Anmeldeinformationen über [CONTROL SERVER](/sql/t-sql/statements/grant-server-permissions-transact-sql)-Berechtigungen verfügen.
* Stellen Sie sicher, dass die für die Verbindung mit der Zielinstanz in Azure SQL Managed Instance verwendeten Anmeldeinformationen über die CONTROL DATABASE-Berechtigung für die Zieldatenbanken in Azure SQL Managed Instance verfügen.

    > [!IMPORTANT]
    > Für Onlinemigrationen müssen Sie bereits Ihre Azure Active Directory-Anmeldeinformationen eingerichtet haben. Weitere Informationen finden Sie im Artikel [Erstellen einer Azure AD-Anwendung und eines Dienstprinzipals mit Ressourcenzugriff mithilfe des Portals](../active-directory/develop/howto-create-service-principal-portal.md).

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Eine Azure-Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen bereitgestellt und verwaltet werden.

Erstellen Sie mit dem Befehl [`New-AzResourceGroup`](/powershell/module/az.resources/new-azresourcegroup) eine Ressourcengruppe.

Im folgenden Beispiel wird eine Ressourcengruppe mit dem Namen *myResourceGroup* in der Region *USA, Osten* erstellt.

```powershell
New-AzResourceGroup -ResourceGroupName myResourceGroup -Location EastUS
```

## <a name="create-an-instance-of-dms"></a>Erstellen einer Instanz von DMS

Mit dem Cmdlet `New-AzDataMigrationService` können Sie eine neue Instanz von Azure Database Migration Service erstellen.
Dieses Cmdlet erwartet die folgenden erforderlichen Parameter:

* *ResourceGroupName*: Mit dem Befehl [`New-AzResourceGroup`](/powershell/module/az.resources/new-azresourcegroup) können Sie wie oben gezeigt eine Azure-Ressourcengruppe erstellen und deren Namen als Parameter bereitstellen.
* *ServiceName*: Die Zeichenfolge, die dem gewünschten eindeutigen Dienstnamen für Azure Database Migration Service entspricht.
* *Standort*. Gibt den Standort des Diensts an. Geben Sie einen Azure-Rechenzentrumsstandort an, z. B. „USA, Westen“ oder „Asien, Südosten“.
* *Sku*: Dieser Parameter entspricht dem Sku-Namen des DMS. Derzeit unterstützte Sku-Namen sind *Basic_1vCore*, *Basic_2vCores*, *GeneralPurpose_4vCores*.
* *VirtualSubnetId*: Sie können das Cmdlet [`New-AzVirtualNetworkSubnetConfig`](/powershell/module/az.network/new-azvirtualnetworksubnetconfig) verwenden, um ein Subnetz zu erstellen.

Im folgenden Beispiel wird ein Dienst mit dem Namen *MyDMS* in der Ressourcengruppe *MyDMSResourceGroup*, die sich in der Region *USA, Osten* befindet, mit einem virtuellen Netzwerk namens *MyVNET* und einem Subnetz namens *MySubnet* erstellt.


```powershell
$vNet = Get-AzVirtualNetwork -ResourceGroupName MyDMSResourceGroup -Name MyVNET

$vSubNet = Get-AzVirtualNetworkSubnetConfig -VirtualNetwork $vNet -Name MySubnet

$service = New-AzDms -ResourceGroupName myResourceGroup `
  -ServiceName MyDMS `
  -Location EastUS `
  -Sku Basic_2vCores `  
  -VirtualSubnetId $vSubNet.Id`
```

## <a name="create-a-migration-project"></a>Erstellen eines Migrationsprojekts

Nachdem Sie eine Instanz von Azure Database Migration Service erstellt haben, erstellen Sie ein Migrationsprojekt. Ein Azure Database Migration Service-Projekt erfordert Verbindungsinformationen sowohl für die Quell- als auch die Zielinstanz sowie eine Liste der Datenbanken, die Sie als Teil des Projekts migrieren möchten.
Definieren Sie Verbindungszeichenfolgen für die Quell- und Zielkonnektivität.

Das folgende Skript definiert die Details der SQL Server-Quellverbindung: 

```powershell
# Source connection properties
$sourceDataSource = "<mysqlserver.domain.com/privateIP of source SQL>"
$sourceUserName = "domain\user"
$sourcePassword = "mypassword"
```

Das folgende Skript definiert die Details der SQL Managed Instance-Zielverbindung: 

```powershell
# Target MI connection properties
$targetMIResourceId = "/subscriptions/<subid>/resourceGroups/<rg>/providers/Microsoft.Sql/managedInstances/<myMI>"
$targetUserName = "<user>"
$targetPassword = "<password>"
```



### <a name="define-source-and-target-database-mapping"></a>Definieren der Zuordnung zwischen Quell- und Zieldatenbank

Bereitstellen von Datenbanken, die in diesem Migrationsprojekt migriert werden sollen

Das folgende Skript ordnet die Quelldatenbank der entsprechenden neuen Datenbank des SQL Managed Instance-Ziels mit dem angegebenen Namen zu.

```powershell
# Selected databases (Source database name to target database name mapping)
$selectedDatabasesMap = New-Object System.Collections.Generic.Dictionary"[String,String]" 
$selectedDatabasesMap.Add("<source  database name>", "<target database name> ")
```

Bei mehreren Datenbanken fügen Sie dem obigen Skript die Liste der Datenbanken hinzu, indem Sie das folgende Format verwenden:

```powershell
$selectedDatabasesMap = New-Object System.Collections.Generic.Dictionary"[String,String]" 
$selectedDatabasesMap.Add("<source  database name1>", "<target database name1> ")
$selectedDatabasesMap.Add("<source  database name2>", "<target database name2> ")
```

### <a name="create-dms-project"></a>Erstellen eines DMS-Projekts

Sie können ein Azure Database Migration Service-Projekt innerhalb der DMS-Instanz erstellen.

```powershell
# Create DMS project
$project = New-AzDataMigrationProject `
  -ResourceGroupName $dmsResourceGroupName `
  -ServiceName $dmsServiceName `
  -ProjectName $dmsProjectName `
  -Location $dmsLocation `
  -SourceType SQL `
  -TargetType SQLMI `

# Create selected databases object
$selectedDatabases = @();
foreach ($sourceDbName in $selectedDatabasesMap.Keys){
    $targetDbName = $($selectedDatabasesMap[$sourceDbName])
    $selectedDatabases += New-AzDmsSelectedDB -MigrateSqlServerSqlDbMi `
      -Name $sourceDbName `
      -TargetDatabaseName $targetDbName `
      -BackupFileShare $backupFileShare `
}
```



### <a name="create-a-backup-fileshare-object"></a>Erstellen eines Sicherungs-FileShare-Objekts

Erstellen Sie nun ein FileShare-Objekt, das die lokale SMB-Netzwerkfreigabe darstellt, in die Azure Database Migration Service mit dem Cmdlet „New-AzDmsFileShare“ die Quelldatenbanksicherungen ausführen kann.

```powershell
# SMB Backup share properties
$smbBackupSharePath = "\\shareserver.domain.com\mybackup"
$smbBackupShareUserName = "domain\user"
$smbBackupSharePassword = "<password>"

# Create backup file share object
$smbBackupSharePasswordSecure = ConvertTo-SecureString -String $smbBackupSharePassword -AsPlainText -Force
$smbBackupShareCredentials = New-Object System.Management.Automation.PSCredential ($smbBackupShareUserName, $smbBackupSharePasswordSecure)
$backupFileShare = New-AzDmsFileShare -Path $smbBackupSharePath -Credential $smbBackupShareCredentials
```

### <a name="define-the-azure-storage"></a>Definieren von Azure Storage 

Wählen Sie den Azure Storage-Container aus, der für die Migration verwendet werden soll: 

```powershell
# Storage resource id
$storageAccountResourceId = "/subscriptions/<subscriptionname>/resourceGroups/<rg>/providers/Microsoft.Storage/storageAccounts/<mystorage>"
```


### <a name="configure-azure-active-directory-app"></a>Konfigurieren der Azure Active Directory-App

Geben Sie die erforderlichen Details für Azure Active Directory für eine Onlinemigration von SQL Managed Instance an: 

```powershell
# AAD properties
$AADAppId = "<appid-guid>"
$AADAppKey = "<app-key>"

# Create AAD object
$AADAppKeySecure = ConvertTo-SecureString $AADAppKey -AsPlainText -Force
$AADApp = New-AzDmsAadApp -ApplicationId $AADAppId -AppKey $AADAppKeySecure
```


## <a name="create-and-start-a-migration-task"></a>Erstellen und Starten einer Migrationsaufgabe

Als Nächstes erstellen Sie eine Azure Database Migration Service-Aufgabe und starten diese. Rufen Sie die Quelle und das Ziel mithilfe von Variablen auf, und listen Sie die zu migrierenden Datenbanktabellen auf: 


```powershell
# Managed Instance online migration properties
$dmsTaskName = "testmigration1"

# Create source connection info
$sourceConnInfo = New-AzDmsConnInfo -ServerType SQL `
  -DataSource $sourceDataSource `
  -AuthType WindowsAuthentication `
  -TrustServerCertificate:$true
$sourcePasswordSecure = ConvertTo-SecureString -String $sourcePassword -AsPlainText -Force
$sourceCredentials = New-Object System.Management.Automation.PSCredential ($sourceUserName, $sourcePasswordSecure)

# Create target connection info
$targetConnInfo = New-AzDmsConnInfo -ServerType SQLMI `
    -MiResourceId $targetMIResourceId
$targetPasswordSecure = ConvertTo-SecureString -String $targetPassword -AsPlainText -Force
$targetCredentials = New-Object System.Management.Automation.PSCredential ($targetUserName, $targetPasswordSecure)
```

Im folgenden Beispiel wird eine Onlinemigrationsaufgabe erstellt und gestartet: 

```powershell
# Create DMS migration task
$migTask = New-AzDataMigrationTask -TaskType MigrateSqlServerSqlDbMiSync `
  -ResourceGroupName $dmsResourceGroupName `
  -ServiceName $dmsServiceName `
  -ProjectName $dmsProjectName `
  -TaskName $dmsTaskName `
  -SourceConnection $sourceConnInfo `
  -SourceCred $sourceCredentials `
  -TargetConnection $targetConnInfo `
  -TargetCred $targetCredentials `
  -SelectedDatabase  $selectedDatabases `
  -BackupFileShare $backupFileShare `
  -AzureActiveDirectoryApp $AADApp `
  -StorageResourceId $storageAccountResourceId
```

Weitere Informationen finden Sie unter [New-AzDataMigrationTask](/powershell/module/az.datamigration/new-azdatamigrationtask).

## <a name="monitor-the-migration"></a>Überwachen der Migration

Um die Migration zu überwachen, führen Sie die folgenden Aufgaben aus.

### <a name="check-the-status-of-task"></a>Überprüfen des Aufgabenstatus

```powershell
# Get migration task status details
$migTask = Get-AzDataMigrationTask `
                    -ResourceGroupName $dmsResourceGroupName `
                    -ServiceName $dmsServiceName `
                    -ProjectName $dmsProjectName `
                    -Name $dmsTaskName `
                    -ResultType DatabaseLevelOutput `
                    -Expand

# Task state will be either of 'Queued', 'Running', 'Succeeded', 'Failed', 'FailedInputValidation' or 'Faulted'
$taskState = $migTask.ProjectTask.Properties.State

# Display task state
$taskState | Format-List
```

Verwenden Sie Folgendes, um eine Liste von Fehlern zu erhalten:

```powershell
# Get task errors
$taskErrors = $migTask.ProjectTask.Properties.Errors

# Display task errors
foreach($taskError in $taskErrors){
    $taskError |  Format-List
}


# Get database level details
$databaseLevelOutputs = $migTask.ProjectTask.Properties.Output

# Display database level details
foreach($databaseLevelOutput in $databaseLevelOutputs){

    # This is the source database name.
    $databaseName = $databaseLevelOutput.SourceDatabaseName;

    Write-Host "=========="
    Write-Host "Start migration details for database " $databaseName
    # This is the status for that database - It will be either of:
    # INITIAL, FULL_BACKUP_UPLOADING, FULL_BACKUP_UPLOADED, LOG_FILES_UPLOADING,
    # CUTOVER_IN_PROGRESS, CUTOVER_INITIATED, CUTOVER_COMPLETED, COMPLETED, CANCELLED, FAILED
    $databaseMigrationState = $databaseLevelOutput.MigrationState;

    # Details about last restored backup. This contains file names, LSN, backup date, etc 
    $databaseLastRestoredBackup = $databaseLevelOutput.LastRestoredBackupSetInfo
        
    # Details about last restored backup. This contains file names, LSN, backup date, etc 
    $databaseLastRestoredBackup = $databaseLevelOutput.LastRestoredBackupSetInfo

    # Details about last Currently active/most recent backups. This contains file names, LSN, backup date, etc 
    $databaseActiveBackpusets = $databaseLevelOutput.ActiveBackupSets

    # Display info
    $databaseLevelOutput | Format-List

    Write-Host "Currently active/most recent backupset details:"
    $databaseActiveBackpusets  | select BackupStartDate, BackupFinishedDate, FirstLsn, LastLsn -ExpandProperty ListOfBackupFiles | Format-List

    Write-Host "Last restored backupset details:"
    $databaseLastRestoredBackupFiles  | Format-List

    Write-Host "End migration details for database " $databaseName
    Write-Host "=========="
}
```

## <a name="performing-the-cutover"></a>Durchführen der Übernahme 

Bei einer Onlinemigration wird eine vollständige Sicherung und Wiederherstellung der Datenbank durchgeführt, woraufhin die Arbeit mit der Wiederherstellung der im BackupFileShare gespeicherten Transaktionsprotokolle fortgesetzt wird.

Wenn die Datenbank in Azure SQL Managed Instance mit den neuen Daten aktualisiert wird und mit der Quelldatenbank synchronisiert ist, können Sie eine Übernahme durchführen.

Im folgende Beispiel wird die Übernahmemigration abgeschlossen. Benutzer rufen diesen Befehl nach ihrem eigenen Ermessen auf.

```powershell
$command = Invoke-AzDmsCommand -CommandType CompleteSqlMiSync `
                               -ResourceGroupName myResourceGroup `
                               -ServiceName $service.Name `
                               -ProjectName $project.Name `
                               -TaskName myDMSTask `
                               -DatabaseName "Source DB Name"
```

## <a name="deleting-the-instance-of-azure-database-migration-service"></a>Löschen der Instanz von Azure Database Migration Service

Nach Abschluss der Migration können Sie die Azure Database Migration Service-Instanz löschen:

```powershell
Remove-AzDms -ResourceGroupName myResourceGroup -ServiceName MyDMS
```

## <a name="additional-resources"></a>Zusätzliche Ressourcen

Informationen zu zusätzlichen Migrationsszenarien (Quelle/Ziel-Paare) finden Sie im [Leitfaden zur Datenbankmigration](https://datamigration.microsoft.com/) von Microsoft.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Azure Database Migration Service finden Sie im Artikel [Was ist Azure Database Migration Service?](./dms-overview.md).