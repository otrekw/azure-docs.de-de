---
title: 'PowerShell: Durchführen einer Offlinemigration für eine MySQL-Datenbank zu Azure Database for MySQL mit DMS'
titleSuffix: Azure Database Migration Service
description: Hier erfahren Sie, wie Sie eine lokale MySQL-Datenbank mithilfe von Azure Database Migration Service per PowerShell-Skript zu Azure Database for MySQL migrieren.
services: dms
author: sumitgaurin
ms.author: sgaur
manager: arthiaga
ms.reviewer: arthiaga
ms.service: dms
ms.workload: data-services
ms.custom: seo-lt-2019
ms.topic: tutorial
ms.date: 04/11/2021
ms.openlocfilehash: f01f718bca7d6fc8c6226f682289ef46068aa126
ms.sourcegitcommit: 4a54c268400b4158b78bb1d37235b79409cb5816
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2021
ms.locfileid: "108125839"
---
# <a name="migrate-mysql-to-azure-database-for-mysql-offline-with-powershell--azure-database-migration-service"></a>Durchführen einer Offlinemigration von MySQL zu Azure Database for MySQL mit PowerShell und Azure Database Migration Service

In diesem Artikel wird eine in einer lokalen Instanz wiederhergestellte MySQL-Datenbank mittels Offlinemigrationsfunktion von Azure Database Migration Service über Microsoft Azure PowerShell zu Azure Database for MySQL migriert. Der Artikel dokumentiert eine Reihe von PowerShell-Skripts, die nacheinander ausgeführt werden können, um die Offlinemigration der MySQL-Datenbank zu Azure durchzuführen. Das vollständige PowerShell-Skript, das in diesem Tutorial beschrieben wird, steht in unserem [GitHub-Repository](https://github.com/Azure/azure-mysql/tree/master/Azure%20DMS%20-%20MySQL%20Offline%20Migration%20Script) zum Download bereit.


> [!NOTE]
> Aktuell ist es nicht möglich, eine vollständige Datenbankmigration mit dem Az.DataMigration-Modul durchzuführen. Das PowerShell-Beispielskript wird vorerst in der vorliegenden Form bereitgestellt und verwendet die [DMS-REST-API](/rest/api/datamigration/tasks/get), um die Automatisierung der Migration zu ermöglichen. Dieses Skript wird geändert oder ausgemustert, sobald eine offizielle Unterstützung für das Az.DataMigration-Modul und die Azure CLI vorliegt. 

> [!NOTE]
> Amazon Relational Database Service (RDS) für MySQL und Amazon Aurora (MySQL-basiert) werden ebenfalls als Quellen für die Migration unterstützt.

> [!IMPORTANT]
> Das Onlinemigrationsszenario „MySQL zu Azure Database for MySQL“ wird ab dem 1. Juni 2021 durch ein parallelisiertes, äußerst leistungsfähiges Offlinemigrationsszenario ersetzt. Für Onlinemigrationen können Sie dieses neue Angebot zusammen mit [Datenreplikation](../mysql/concepts-data-in-replication.md) verwenden. Alternativ können Sie Open-Source-Tools wie [MyDumper/MyLoader](https://centminmod.com/mydumper.html) mit der Datenreplikation für Onlinemigrationen verwenden. 

Der Artikel hilft bei der Automatisierung des Szenarios, in dem Quell- und Zieldatenbanknamen identisch oder unterschiedlich sein können und im Rahmen der Migration entweder alle oder einige Tabellen in der Zieldatenbank mit dem gleichen Namen und der gleichen Tabellenstruktur migriert werden müssen. In dem Artikel wird zwar davon ausgegangen, dass die Quelle eine MySQL-Datenbankinstanz ist und es sich beim Ziel um Azure Database for MySQL handelt, er kann jedoch auch für die Migration zwischen Azure Database for MySQL-Instanzen verwendet werden. Hierzu müssen lediglich der Name und die Anmeldeinformationen des Quellservers geändert werden. Darüber hinaus wird auch die Migration von MySQL-Servern mit niedrigerer Version (ab v5.6) zu höheren Versionen unterstützt.

[!INCLUDE [preview features callout](../../includes/dms-boilerplate-preview.md)]

In diesem Artikel werden folgende Vorgehensweisen behandelt:
> [!div class="checklist"]
>
> * Migrieren des Datenbankschemas
> * Erstellen einer Ressourcengruppe
> * Erstellen einer Instanz von Azure Database Migration Service
> * Erstellen eines Migrationsprojekts in einer Instanz von Azure Database Migration Service
> * Konfigurieren des Migrationsprojekt für die Verwendung der Offlinemigrationsfunktion für MySQL
> * Ausführen der Migration

## <a name="prerequisites"></a>Voraussetzungen

Zum Ausführen dieser Schritte benötigen Sie Folgendes:

* Sie benötigen ein Azure-Konto mit einem aktiven Abonnement. Sie können [kostenlos ein Konto erstellen](https://azure.microsoft.com/free).
* Eine lokale MySQL-Datenbank ab Version 5.6. Laden Sie bei Bedarf mindestens die Version 5.6 von [MySQL Community Edition](https://dev.mysql.com/downloads/mysql/) herunter, und installieren Sie sie.
* [Erstellen einer Instanz in Azure Database for MySQL](../mysql/quickstart-create-mysql-server-database-using-azure-portal.md) Im Artikel [Azure-Datenbank für MySQL: Verwenden von MySQL Workbench zum Verbinden und Abfragen von Daten](../mysql/connect-workbench.md) finden Sie ausführliche Informationen zur Verbindungsherstellung sowie zum Erstellen einer Datenbank mithilfe der Workbench-Anwendung. Die Version von Azure Database for MySQL muss mindestens der lokalen MySQL-Version entsprechen. MySQL 5.7 kann beispielsweise zu Azure Database for MySQL 5.7 migriert oder auf die Version 8 aktualisiert werden.  
* Erstellen Sie ein Microsoft Azure Virtual Network für Azure Database Migration Service, indem Sie das Azure Resource Manager-Bereitstellungsmodell verwenden, das Site-to-Site-Konnektivität für Ihre lokalen Quellserver entweder über [ExpressRoute](../expressroute/expressroute-introduction.md) oder über [VPN](../vpn-gateway/vpn-gateway-about-vpngateways.md) bereitstellt. Weitere Informationen zum Erstellen eines virtuellen Netzwerks finden Sie in der [Dokumentation zu Virtual Network](../virtual-network/index.yml) und insbesondere in den Schnellstartartikeln mit Schritt-für-Schritt-Anleitungen.

    > [!NOTE]
    > Fügen Sie bei Verwendung von ExpressRoute mit Netzwerkpeering zu Microsoft im Rahmen des VNet-Setups den [Dienstendpunkt](../virtual-network/virtual-network-service-endpoints-overview.md) *Microsoft.Sql* dem Subnetz hinzu, in dem der Dienst bereitgestellt werden soll. Diese Konfiguration ist erforderlich, weil Azure Database Migration Service über keine Internetverbindung verfügt.

* Stellen Sie sicher, dass die Netzwerksicherheitsgruppen-Regeln des virtuellen Netzwerks nicht den ausgehenden Port 443 von ServiceTag für Storage und AzureMonitor blockieren. Ausführlichere Informationen zur NSG-Datenverkehrsfilterung in einem virtuellen Netzwerk finden Sie im Artikel [Filtern des Netzwerkdatenverkehrs mit Netzwerksicherheitsgruppen](../virtual-network/virtual-network-vnet-plan-design-arm.md).
* Öffnen Sie Ihre Windows-Firewall, um Verbindungen aus dem virtuellen Netzwerk für Azure Database Migration Service zuzulassen und den Zugriff auf den MySQL-Quellserver zu ermöglichen (standardmäßig TCP-Port 3306).
* Wenn Sie eine Firewallappliance vor Ihren Quelldatenbanken verwenden, müssen Sie möglicherweise Firewallregeln hinzufügen, um Verbindungen aus dem virtuellen Netzwerk für Azure Database Migration Service zuzulassen und den Zugriff auf die Quelldatenbanken für die Migration zu ermöglichen.
* Erstellen Sie für die Azure Database for MySQL-Zielinstanz eine [Firewallregel](../azure-sql/database/firewall-configure.md) auf Serverebene, oder [konfigurieren Sie VNet-Dienstendpunkte](../mysql/howto-manage-vnet-using-portal.md), um den Zugriff auf die Zieldatenbanken durch das virtuelle Netzwerk für Azure Database Migration Service zu ermöglichen.
* Die MySQL-Quellinstanz muss unter einer unterstützten Version von MySQL Community Edition ausgeführt werden. Führen Sie zum Ermitteln der Version der MySQL-Instanz im MySQL-Hilfsprogramm oder in MySQL Workbench den folgenden Befehl aus:

    ```
    SELECT @@version;
    ```

* Azure Database for MySQL unterstützt nur InnoDB-Tabellen. Informationen zum Konvertieren von MyISAM-Tabellen in InnoDB finden Sie im Artikel [Converting Tables from MyISAM to InnoDB](https://dev.mysql.com/doc/refman/5.7/en/converting-tables-to-innodb.html) (Konvertieren von Tabellen von MyISAM zu InnoDB).
* Der Benutzer muss über Berechtigungen zum Lesen von Daten aus der Quelldatenbank verfügen.
* Im Leitfaden wird PowerShell v7.1 mit „PSEdition_Core“ verwendet. Eine entsprechende Installationsanleitung finden Sie [hier](/powershell/scripting/install/installing-powershell?view=powershell-7.1&preserve-view=true).
* Laden Sie die folgenden Module aus dem PowerShell-Katalog herunter, und installieren Sie sie mithilfe des PowerShell-Cmdlets [Install-Module](/powershell/module/powershellget/Install-Module). Das PowerShell-Befehlsfenster muss dabei als Administrator geöffnet werden.
    * Az.Resources
    * Az.Network
    * Az.DataMigration

```powershell
Install-Module Az.Resources
Install-Module Az.Network
Install-Module Az.DataMigration
Import-Module Az.Resources
Import-Module Az.Network
Import-Module Az.DataMigration
```

## <a name="migrate-database-schema"></a>Migrieren des Datenbankschemas

Um alle Datenbankobjekte wie Tabellenschemas, Indizes und gespeicherte Prozeduren zu übertragen, muss das Schema aus der Quelldatenbank extrahiert und auf die Zieldatenbank angewendet werden. Zum Extrahieren des Schemas können Sie „mysqldump“ mit dem Parameter `--no-data` verwenden. Hierzu benötigen Sie einen Computer, der sowohl eine Verbindung mit der MySQL-Quelldatenbank als auch eine Verbindung mit der Azure Database for MySQL-Zieldatenbank herstellen kann.

Führen Sie den folgenden Befehl aus, um das Schema mit „mysqldump“ zu exportieren:

```
mysqldump -h [servername] -u [username] -p[password] --databases [db name] --no-data > [schema file path]
```

Beispiel:

```
mysqldump -h 10.10.123.123 -u root -p --databases migtestdb --no-data > d:\migtestdb.sql
```

Führen Sie den folgenden Befehl aus, um das Schema in die Azure Database for MySQL-Zielinstanz zu importieren:

```
mysql.exe -h [servername] -u [username] -p[password] [database]< [schema file path]
 ```

Beispiel:

```
mysql.exe -h mysqlsstrgt.mysql.database.azure.com -u docadmin@mysqlsstrgt -p migtestdb < d:\migtestdb.sql
 ```

Wenn Ihr Schema Fremdschlüssel enthält, wird das parallele Laden von Daten während der Migration von der Migrationsaufgabe übernommen. Fremdschlüssel müssen bei der Schemamigration nicht entfernt werden.

Falls die Datenbank Trigger enthält, wird vor der vollständigen Migration der Daten aus der Quelldatenbank die Datenintegrität in der Zieldatenbank erzwungen. Es wird empfohlen, während der Migration die Trigger in allen Tabellen der Zieldatenbank zu deaktivieren und die Trigger nach der Migration wieder zu aktivieren.

Führen Sie in MySQL Workbench das folgende Skript für die Zieldatenbank aus, um das Skript zum Entfernen von Triggern und das Skript zum Hinzufügen von Triggern zu extrahieren:

```sql
SELECT
    SchemaName,
    GROUP_CONCAT(DropQuery SEPARATOR ';\n') as DropQuery,
    Concat('DELIMITER $$ \n\n', GROUP_CONCAT(AddQuery SEPARATOR '$$\n'), '$$\n\nDELIMITER ;') as AddQuery
FROM
(
SELECT 
    TRIGGER_SCHEMA as SchemaName,
    Concat('DROP TRIGGER `', TRIGGER_NAME, "`") as DropQuery,
    Concat('CREATE TRIGGER `', TRIGGER_NAME, '` ', ACTION_TIMING, ' ', EVENT_MANIPULATION, 
            '\nON `', EVENT_OBJECT_TABLE, '`\n' , 'FOR EACH ', ACTION_ORIENTATION, ' ',
            ACTION_STATEMENT) as AddQuery
FROM  
    INFORMATION_SCHEMA.TRIGGERS
ORDER BY EVENT_OBJECT_SCHEMA, EVENT_OBJECT_TABLE, ACTION_TIMING, EVENT_MANIPULATION, ACTION_ORDER ASC
) AS Queries
GROUP BY SchemaName
```

Führen Sie die generierte Abfrage zum Entfernen von Triggern (Spalte „DropQuery“) im Ergebnis aus, um Trigger in der Zieldatenbank zu entfernen. Die Abfrage zum Hinzufügen von Triggern kann gespeichert und nach Abschluss der Datenmigration verwendet werden.

## <a name="log-in-to-your-microsoft-azure-subscription"></a>Anmelden bei Ihrem Microsoft Azure-Abonnement

Verwenden Sie den PowerShell-Befehl [Connect-AzAccount](/powershell/module/az.accounts/connect-azaccount), um sich unter Verwendung von PowerShell bei Ihrem Azure-Abonnement anzumelden. Eine entsprechende Anleitung finden Sie im Artikel [Anmelden mit Azure PowerShell](/powershell/azure/authenticate-azureps).

Durch das folgende Skript wird das Standardabonnement für die PowerShell-Sitzung nach der Anmeldung festgelegt und eine Hilfsprotokollierungsfunktion für formatierte Konsolenprotokolle erstellt:

```powershell
[string] $SubscriptionName = "mySubscription"
$ErrorActionPreference = "Stop";

Connect-AzAccount
Set-AzContext -Subscription $SubscriptionName
$global:currentSubscriptionId = (Get-AzContext).Subscription.Id;

function LogMessage([string] $Message, [bool] $IsProcessing = $false) {
    if ($IsProcessing) {
        Write-Host "$(Get-Date -Format "yyyy-MM-dd HH:mm:ss"): $Message" -ForegroundColor Yellow
    }
    else {
        Write-Host "$(Get-Date -Format "yyyy-MM-dd HH:mm:ss"): $Message" -ForegroundColor Green
    }    
}
```

## <a name="register-the-microsoftdatamigration-resource-provider"></a>Registrieren des Ressourcenanbieters „Microsoft.DataMigration“

Die Registrierung des Ressourcenanbieters muss für jedes Azure-Abonnement lediglich einmal durchgeführt werden. Ohne die Registrierung kann keine Instanz von **Azure Database Migration Service** erstellt werden.

Registrieren Sie den Ressourcenanbieter mithilfe des Befehls [Register-AzResourceProvider](/powershell/module/az.resources/register-azresourceprovider). Durch das folgende Skript wird der für **Azure Database Migration Service** erforderliche Ressourcenanbieter registriert:

```powershell
Register-AzResourceProvider -ProviderNamespace Microsoft.DataMigration
```

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Eine Azure-Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen bereitgestellt und verwaltet werden. Erstellen Sie eine Ressourcengruppe, bevor Sie DMS-Ressourcen erstellen.

Erstellen Sie mit dem Befehl [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) eine Ressourcengruppe.

Im folgenden Beispiel wird eine Ressourcengruppe namens *myResourceGroup* in der Region *USA, Westen 2* unter dem Standardabonnement *mySubscription* erstellt.

```powershell
# Get the details of resource group
[string] $Location = "westus2"
[string] $ResourceGroupName = "myResourceGroup"

$resourceGroup = Get-AzResourceGroup -Name $ResourceGroupName
if (-not($resourceGroup)) {
    LogMessage -Message "Creating resource group $ResourceGroupName..." -IsProcessing $true
    $resourceGroup = New-AzResourceGroup -Name $ResourceGroupName -Location $Location
    LogMessage -Message "Created resource group - $($resourceGroup.ResourceId)."
}
else { LogMessage -Message "Resource group $ResourceGroupName exists." }
```

## <a name="create-an-instance-of-azure-database-migration-service"></a>Erstellen einer Instanz von Azure Database Migration Service

Mit dem Befehl [New-AzDataMigrationService](/powershell/module/az.datamigration/new-azdatamigrationservice) können Sie eine neue Instanz von Azure Database Migration Service erstellen. Von diesem Befehl werden die folgenden erforderlichen Parameter erwartet:
* *ResourceGroupName*: Mit dem Befehl [New-AzRmResourceGroup](/powershell/module/az.resources/new-azresourcegroup) können Sie wie oben gezeigt eine Azure-Ressourcengruppe erstellen und deren Namen als Parameter bereitstellen.
* *ServiceName*: Die Zeichenfolge, die dem gewünschten eindeutigen Dienstnamen für Azure Database Migration Service entspricht. 
* *Standort*. Gibt den Standort des Diensts an. Geben Sie einen Azure-Rechenzentrumsstandort an, z. B. „USA, Westen“ oder „Asien, Südosten“.
* *Sku*: Dieser Parameter entspricht dem Sku-Namen des DMS. Aktuell werden folgende SKU-Namen unterstützt: *Standard_1vCore*, *Standard_2vCores*, *Standard_4vCores* und *Premium_4vCores*.
* *VirtualSubnetId*: Die Informationen eines Subnetzes können mithilfe des Befehls [Get-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/get-azvirtualnetworksubnetconfig) abgerufen werden. 

Vom folgenden Skript wird erwartet, dass das virtuelle Netzwerk *myVirtualNetwork* mit einem Subnetz namens *default* vorhanden ist. Ist dies der Fall, wird unter der in **Schritt 3** erstellten Ressourcengruppe und in der gleichen Region eine Database Migration Service-Instanz namens *myDmService* erstellt:

```powershell
# Get a reference to the DMS service - Create if not exists
[string] $VirtualNetworkName = "myVirtualNetwork"
[string] $SubnetName = "default"
[string] $ServiceName = "myDmService"

$dmsServiceResourceId = "/subscriptions/$($global:currentSubscriptionId)/resourceGroups/$ResourceGroupName/providers/Microsoft.DataMigration/services/$ServiceName"
$dmsService = Get-AzResource -ResourceId $dmsServiceResourceId -ErrorAction SilentlyContinue

# Create Azure DMS service if not existing
# Possible values for SKU currently are Standard_1vCore,Standard_2vCores,Standard_4vCores,Premium_4vCores
if (-not($dmsService)) {   
    $virtualNetwork = Get-AzVirtualNetwork -ResourceGroupName $ResourceGroupName -Name $VirtualNetworkName
    if (-not ($virtualNetwork)) { throw "ERROR: Virtual Network $VirtualNetworkName does not exists" }

    $subnet = Get-AzVirtualNetworkSubnetConfig -VirtualNetwork $virtualNetwork -Name $SubnetName
    if (-not ($subnet)) { throw "ERROR: Virtual Network $VirtualNetworkName does not contains Subnet $SubnetName" }

    LogMessage -Message "Creating Azure Data Migration Service $ServiceName..." -IsProcessing $true
    $dmsService = New-AzDataMigrationService `
        -ResourceGroupName $ResourceGroupName `
        -Name $ServiceName `
        -Location $resourceGroup.Location `
        -Sku Premium_4vCores `
        -VirtualSubnetId $Subnet.Id
    
    $dmsService = Get-AzResource -ResourceId $dmsServiceResourceId
    LogMessage -Message "Created Azure Data Migration Service - $($dmsService.ResourceId)."
}
else { LogMessage -Message "Azure Data Migration Service $ServiceName exists." }
```

## <a name="create-a-migration-project"></a>Erstellen eines Migrationsprojekts

Nach der Erstellung einer Azure Database Migration Service-Instanz muss ein Migrationsprojekt erstellt werden. Ein Migrationsprojekt dient zum Angeben der Art der durchzuführenden Migration.

Durch das folgende Skript wird ein Migrationsprojekt mit dem Namen *myfirstmysqlofflineproject* für die Offlinemigration von MySQL zu Azure Database for MySQL unter der in **Schritt 4** erstellten Database Migration Service-Instanz und in der gleichen Region erstellt:

```powershell
# Get a reference to the DMS project - Create if not exists
[string] $ProjectName = "myfirstmysqlofflineproject"

$dmsProjectResourceId = "/subscriptions/$($global:currentSubscriptionId)/resourceGroups/$($dmsService.ResourceGroupName)/providers/Microsoft.DataMigration/services/$($dmsService.Name)/projects/$projectName"
$dmsProject = Get-AzResource -ResourceId $dmsProjectResourceId -ErrorAction SilentlyContinue

# Create Azure DMS Project if not existing
if (-not($dmsProject)) {
    LogMessage -Message "Creating Azure DMS project $projectName for MySQL migration ..." -IsProcessing $true

    $newProjectProperties = @{"sourcePlatform" = "MySQL"; "targetPlatform" = "AzureDbForMySQL" }
    $dmsProject = New-AzResource `
        -ApiVersion 2018-03-31-preview `
        -Location $dmsService.Location `
        -ResourceId $dmsProjectResourceId `
        -Properties $newProjectProperties `
        -Force

    LogMessage -Message "Created Azure DMS project $projectName - $($dmsProject.ResourceId)."
}
else { LogMessage -Message "Azure DMS project $projectName exists." }
```

## <a name="create-a-database-connection-info-object-for-the-source-and-target-connections"></a>Erstellen eines Objekts mit Datenbankverbindungsinformationen für die Quell- und Zielverbindung

Nach der Erstellung des Migrationsprojekts müssen die Datenbankverbindungsinformationen erstellt werden. Diese Verbindungsinformationen werden verwendet, um im Rahmen des Migrationsprozesses eine Verbindung mit dem Quell- und Zielserver herzustellen.

Im folgenden Skript werden der Servername, der Benutzername und das Kennwort für die Quell- und Zielinstanz von MySQL verwendet und die Verbindungsinformationsobjekte erstellt. Das Skript fordert den Benutzer zur Eingabe des Kennworts für die Quell- und Zielinstanz von MySQL auf. Bei Skripts ohne Benutzerinteraktion können die Anmeldeinformationen aus Azure Key Vault abgerufen werden. 

```powershell
# Initialize the source and target database server connections
[string] $SourceServerName = "13.66.136.192"
[string] $SourceUserName = "docadmin@mysqlserver"
[securestring] $SourcePassword = Read-Host "Enter MySQL Source Server Password" -AsSecureString

[string] $TargetServerName = "migdocdevwus2mysqlsstrgt.mysql.database.azure.com"
[string] $TargetUserName = "docadmin@migdocdevwus2mysqlsstrgt"
[securestring] $TargetPassword = Read-Host "Enter MySQL Target Server Password" -AsSecureString

function InitConnection(
    [string] $ServerName,
    [string] $UserName,
    [securestring] $Password) {
    $connectionInfo = @{
        "dataSource"             = "";
        "serverName"             = "";
        "port"                   = 3306;
        "userName"               = "";
        "password"               = "";
        "authentication"         = "SqlAuthentication";
        "encryptConnection"      = $true;
        "trustServerCertificate" = $true;
        "additionalSettings"     = "";
        "type"                   = "MySqlConnectionInfo" 
    }

    $connectionInfo.dataSource = $ServerName;
    $connectionInfo.serverName = $ServerName;
    $connectionInfo.userName = $UserName;
    $connectionInfo.password = (ConvertFrom-SecureString -AsPlainText $password).ToString();
    $connectionInfo;
}

# Initialize the source and target connections
LogMessage -Message "Initializing source and target connection objects ..." -IsProcessing $true
$sourceConnInfo = InitConnection `
    $SourceServerName `
    $SourceUserName `
    $SourcePassword;

$targetConnInfo = InitConnection `
    $TargetServerName `
    $TargetUserName `
    $TargetPassword;

LogMessage -Message "Source and target connection object initialization complete."
```

## <a name="extract-the-list-of-table-names-from-the-target-database"></a>Extrahieren der Liste der Tabellennamen aus der Zieldatenbank

Die Datenbanktabellenliste kann mithilfe einer Migrationsaufgabe und unter Verwendung von Verbindungsinformationen extrahiert werden. Die Tabellenliste wird sowohl aus der Quell- als auch der Zieldatenbank extrahiert, um eine ordnungsgemäße Zuordnung und Überprüfung zur ermöglichen. 

Im folgenden Skript werden die Namen der Quell- und Zieldatenbank verwendet, und die Tabellenliste wird mithilfe der Migrationsaufgabe *GetUserTablesMySql* aus den Datenbanken extrahiert: 

```powershell
# Run scenario to get the tables from the target database to build
# the migration table mapping
[string] $TargetDatabaseName = "migtargetdb"
[string] $SourceDatabaseName = "migsourcedb"

function RunScenario([object] $MigrationService, 
    [object] $MigrationProject, 
    [string] $ScenarioTaskName, 
    [object] $TaskProperties, 
    [bool] $WaitForScenario = $true) {
    # Check if the scenario task already exists, if so remove it
    LogMessage -Message "Removing scenario if already exists..." -IsProcessing $true
    Remove-AzDataMigrationTask `
        -ResourceGroupName $MigrationService.ResourceGroupName `
        -ServiceName $MigrationService.Name `
        -ProjectName $MigrationProject.Name `
        -TaskName $ScenarioTaskName `
        -Force;

    # Start the new scenario task using the provided properties
    LogMessage -Message "Initializing scenario..." -IsProcessing $true
    New-AzResource `
        -ApiVersion 2018-03-31-preview `
        -Location $MigrationService.Location `
        -ResourceId "/subscriptions/$($global:currentSubscriptionId)/resourceGroups/$($MigrationService.ResourceGroupName)/providers/Microsoft.DataMigration/services/$($MigrationService.Name)/projects/$($MigrationProject.Name)/tasks/$($ScenarioTaskName)" `
        -Properties $TaskProperties `
        -Force | Out-Null;
    
    LogMessage -Message "Waiting for $ScenarioTaskName scenario to complete..." -IsProcessing $true
    if ($WaitForScenario) {
        $progressCounter = 0;
        do {
            if ($null -ne $scenarioTask) {
                Start-Sleep 10;
            }

            # Get calls can time out and will return a cancellation exception in that case
            $scenarioTask = Get-AzDataMigrationTask `
                -ResourceGroupName $MigrationService.ResourceGroupName `
                -ServiceName $MigrationService.Name `
                -ProjectName $MigrationProject.Name `
                -TaskName $ScenarioTaskName `
                -Expand `
                -ErrorAction Ignore;

            Write-Progress -Activity "Scenario Run $ScenarioTaskName  (Marquee Progress Bar)" `
                -Status $scenarioTask.ProjectTask.Properties.State `
                -PercentComplete $progressCounter
            
            $progressCounter += 10;
            if ($progressCounter -gt 100) { $progressCounter = 10 }
        }
        while (($null -eq $scenarioTask) -or ($scenarioTask.ProjectTask.Properties.State -eq "Running") -or ($scenarioTask.ProjectTask.Properties.State -eq "Queued"))
    }
    Write-Progress -Activity "Scenario Run $ScenarioTaskName" `
        -Status $scenarioTask.ProjectTask.Properties.State `
        -Completed
                 
    # Now get it using REST APIs so we can expand the output  
    LogMessage -Message "Getting expanded task results ..." -IsProcessing $true  
    $psToken = (Get-AzAccessToken -ResourceUrl https://management.azure.com).Token;
    $token = ConvertTo-SecureString -String $psToken -AsPlainText -Force;
    $taskResource = Invoke-RestMethod `
        -Method GET `
        -Uri "https://management.azure.com$($scenarioTask.ProjectTask.Id)?api-version=2018-03-31-preview&`$expand=output" `
        -ContentType "application/json" `
        -Authentication Bearer `
        -Token $token;
    
    $taskResource.properties;
}

# create the get table task properties by initializing the connection and 
# database name
$getTablesTaskProperties = @{
    "input"    = @{
        "connectionInfo"    = $null;
        "selectedDatabases" = $null;
    };
    "taskType" = "GetUserTablesMySql";
};

LogMessage -Message "Running scenario to get the list of tables from the target database..." -IsProcessing $true
$getTablesTaskProperties.input.connectionInfo = $targetConnInfo;
$getTablesTaskProperties.input.selectedDatabases = @($TargetDatabaseName);
# Create a name for the task
$getTableTaskName = "$($TargetDatabaseName)GetUserTables"
# Get the list of tables from the source
$getTargetTablesTask = RunScenario -MigrationService $dmsService `
    -MigrationProject $dmsProject `
    -ScenarioTaskName $getTableTaskName `
    -TaskProperties $getTablesTaskProperties;

if (-not ($getTargetTablesTask)) { throw "ERROR: Could not get target database $TargetDatabaseName table information." }
LogMessage -Message "List of tables from the target database acquired."

LogMessage -Message "Running scenario to get the list of tables from the source database..." -IsProcessing $true
$getTablesTaskProperties.input.connectionInfo = $sourceConnInfo;
$getTablesTaskProperties.input.selectedDatabases = @($SourceDatabaseName);
# Create a name for the task
$getTableTaskName = "$($SourceDatabaseName)GetUserTables"
# Get the list of tables from the source
$getSourceTablesTask = RunScenario -MigrationService $dmsService `
    -MigrationProject $dmsProject `
    -ScenarioTaskName $getTableTaskName `
    -TaskProperties $getTablesTaskProperties;

if (-not ($getSourceTablesTask)) { throw "ERROR: Could not get source database $SourceDatabaseName table information." }
LogMessage -Message "List of tables from the source database acquired."

```

## <a name="build-table-mapping-based-on-user-configuration"></a>Erstellen einer Tabellenzuordnung auf der Grundlage der Benutzerkonfiguration

Im Rahmen der Konfiguration der Migrationsaufgabe wird eine Zuordnung zwischen Quell- und Zieltabelle erstellt. Die Zuordnung wird auf Tabellennamenebene erstellt, es wird jedoch davon ausgegangen, dass die Tabellenstruktur (Spaltenanzahl, Spaltennamen, Datentypen usw.) der zugeordneten Tabellen identisch ist.

Durch das folgende Skript wird eine Zuordnung auf der Grundlage der in **Schritt 7** extrahierten Ziel- und Quelltabellenliste erstellt. Wenn nur ein Teil der Daten geladen werden soll, kann der Benutzer eine Liste mit Tabellen bereitstellen, um die Tabellen herauszufiltern. Ohne Benutzereingabe werden alle Zieltabellen zugeordnet. Durch das Skript wird auch überprüft, ob in der Quelle eine Tabelle gleichen Namens vorhanden ist. Ist der Tabellenname in der Quelle nicht vorhanden, wird die Zieltabelle für die Migration ignoriert. 

```powershell
# Create the source to target table map
# Optional table settings
# DEFAULT: $IncludeTables = $null => include all tables for migration
# DEFAULT: $ExcludeTables = $null => exclude no tables from migration
# Exclude list has higher priority than include list
# Array of qualified source table names which should be migrated
[string[]] $IncludeTables = @("migsourcedb.coupons", "migsourcedb.daily_cash_sheets");
[string[]] $ExcludeTables = $null;

LogMessage -Message "Creating the table map based on the user input and database table information ..." `
    -IsProcessing $true

$targetTables = $getTargetTablesTask.Output.DatabasesToTables."$TargetDatabaseName";
$sourceTables = $getSourceTablesTask.Output.DatabasesToTables."$SourceDatabaseName";
$tableMap = New-Object 'system.collections.generic.dictionary[string,string]';

$schemaPrefixLength = $($SourceDatabaseName + ".").Length;
$tableMappingError = $false
foreach ($srcTable in $sourceTables) {
    # Removing the database name prefix from the table name so that comparison
    # can be done in cases where database name given are different
    $tableName = $srcTable.Name.Substring($schemaPrefixLength, `
            $srcTable.Name.Length - $schemaPrefixLength)

    # In case the table is part of exclusion list then ignore the table
    if ($null -ne $ExcludeTables -and $ExcludeTables -contains $srcTable.Name) {
        continue;
    }

    # Either the include list is null or the table is part of the include list then add it in the mapping
    if ($null -eq $IncludeTables -or $IncludeTables -contains $srcTable.Name) {
        # Check if the table exists in the target. If not then log TABLE MAPPING ERROR
        if (-not ($targetTables | Where-Object { $_.name -ieq "$($TargetDatabaseName).$tableName" })) {
            $tableMappingError = $true
            Write-Host "TABLE MAPPING ERROR: $($targetTables.name) does not exists in target." -ForegroundColor Red
            continue;
        }  

        $tableMap.Add("$($SourceDatabaseName).$tableName", "$($TargetDatabaseName).$tableName");
    }     
}

# In case of any table mapping errors identified, throw an error and stop the process
if ($tableMappingError) { throw "ERROR: One or more table mapping errors were identified. Please see previous messages." }
# In case no tables are in the mapping then throw error
if ($tableMap.Count -le 0) { throw "ERROR: Could not create table mapping." }
LogMessage -Message "Migration table mapping created for $($tableMap.Count) tables."
```

## <a name="create-and-configure-the-migration-task-inputs"></a>Erstellen und Konfigurieren der Eingaben für die Migrationsaufgabe

Nach dem Erstellen der Tabellenzuordnung müssen die Eingaben für die Migrationsaufgabe vom Typ *Migrate.MySql.AzureDbForMySql* erstellt und die Eigenschaften konfiguriert werden.

Durch das folgende Skript wird die Migrationsaufgabe erstellt, und die Verbindungen, Datenbanknamen und Tabellenzuordnungen werden festgelegt:

```powershell
# Create and configure the migration scenario based on the connections
# and the table mapping
$offlineMigTaskProperties = @{
    "input"    = @{
        "sourceConnectionInfo"  = $null;
        "targetConnectionInfo"  = $null;
        "selectedDatabases"     = $null;
        "optionalAgentSettings" = @{
            "EnableCacheBatchesInMemory"         = $true;
            "DisableIncrementalRowStatusUpdates" = $true;
        };
        "startedOn"             = $null;
    };
    "taskType" = "Migrate.MySql.AzureDbForMySql";
};
$offlineSelectedDatabase = @{
    "name"               = $null;
    "targetDatabaseName" = $null;
    "tableMap"           = $null;
};

LogMessage -Message "Preparing migration scenario configuration ..." -IsProcessing $true

# Select the database to be migrated
$offlineSelectedDatabase.name = $SourceDatabaseName;
$offlineSelectedDatabase.tableMap = New-Object PSObject -Property $tableMap;
$offlineSelectedDatabase.targetDatabaseName = $TargetDatabaseName;

# Set connection info and the database mapping
$offlineMigTaskProperties.input.sourceConnectionInfo = $sourceConnInfo;
$offlineMigTaskProperties.input.targetConnectionInfo = $targetConnInfo;
$offlineMigTaskProperties.input.selectedDatabases = @($offlineSelectedDatabase);
$offlineMigTaskProperties.input.startedOn = [System.DateTimeOffset]::UtcNow.ToString("O");
```

## <a name="configure-performance-tuning-parameters"></a>Konfigurieren von Leistungsoptimierungsparametern

Für das PowerShell-Modul sind einige optionale Parameter verfügbar, die basierend auf der Umgebung optimiert werden können. Diese Parameter können verwendet werden, um die Leistung der Migrationsaufgabe zu verbessern. Alle diese Parameter sind optional und standardmäßig auf NULL festgelegt.

> [!NOTE]
> Durch die folgenden Leistungskonfigurationen konnte bei der Migration in der Premium-SKU ein erhöhter Durchsatz erzielt werden:
> * WriteDataRangeBatchTaskCount = 12
> * DelayProgressUpdatesInStorageInterval = 30 Sekunden
> * ThrottleQueryTableDataRangeTaskAtBatchCount = 36

Im folgenden Skript werden die Benutzerwerte der Parameter verwendet und die Parameter in den Eigenschaften der Migrationsaufgabe festgelegt:

```powershell
# Setting optional parameters from fine tuning the data transfer rate during migration
# DEFAULT values for all the configurations is $null
LogMessage -Message "Adding optional migration performance tuning configuration ..." -IsProcessing $true
# Partitioning settings
# Optional setting that configures the maximum number of parallel reads on tables located on the source database.
[object] $DesiredRangesCount = 4
# Optional setting that configures that size of the largest batch that will be committed to the target server.
[object] $MaxBatchSizeKb = 4096
# Optional setting that configures the minimum number of rows in each batch written to the target.
[object] $MinBatchRows = $null
# Task count settings
# Optional setting that configures the number of databases that will be prepared for migration in parallel.
[object] $PrepareDatabaseForBulkImportTaskCount = $null
# Optional setting that configures the number of tables that will be prepared for migration in parallel.
[object] $PrepareTableForBulkImportTaskCount = $null
# Optional setting that configures the number of threads available to read ranges on the source.
[object] $QueryTableDataRangeTaskCount = 8
# Optional setting that configures the number of threads available to write batches to the target.
[object] $WriteDataRangeBatchTaskCount = 12
# Batch cache settings
# Optional setting that configures how much memory will be used to cache batches in memory before reads on the source are throttled.
[object] $MaxBatchCacheSizeMb = $null
# Optional setting that configures the amount of available memory at which point reads on the source will be throttled.
[object] $ThrottleQueryTableDataRangeTaskAtAvailableMemoryMb = $null
# Optional setting that configures the number of batches cached in memory that will trigger read throttling on the source.
[object] $ThrottleQueryTableDataRangeTaskAtBatchCount = 36
# Performance settings
# Optional setting that configures the delay between updates of result objects in Azure Table Storage.
[object] $DelayProgressUpdatesInStorageInterval = "00:00:30"

function AddOptionalSetting($optionalAgentSettings, $settingName, $settingValue) {
    # If no value specified for the setting, don't bother adding it to the input
    if ($null -eq $settingValue) {
        return;
    }

    # Add a new property to the JSON object to capture the setting which will be customized
    $optionalAgentSettings | add-member -MemberType NoteProperty -Name $settingName -Value $settingValue
}

# Set any optional settings in the input based on parameters to this cmdlet
AddOptionalSetting $offlineMigTaskProperties.input.optionalAgentSettings "DesiredRangesCount" $DesiredRangesCount;
AddOptionalSetting $offlineMigTaskProperties.input.optionalAgentSettings "MaxBatchSizeKb" $MaxBatchSizeKb;
AddOptionalSetting $offlineMigTaskProperties.input.optionalAgentSettings "MinBatchRows" $MinBatchRows;
AddOptionalSetting $offlineMigTaskProperties.input.optionalAgentSettings "PrepareDatabaseForBulkImportTaskCount" $PrepareDatabaseForBulkImportTaskCount;
AddOptionalSetting $offlineMigTaskProperties.input.optionalAgentSettings "PrepareTableForBulkImportTaskCount" $PrepareTableForBulkImportTaskCount;
AddOptionalSetting $offlineMigTaskProperties.input.optionalAgentSettings "QueryTableDataRangeTaskCount" $QueryTableDataRangeTaskCount;
AddOptionalSetting $offlineMigTaskProperties.input.optionalAgentSettings "WriteDataRangeBatchTaskCount" $WriteDataRangeBatchTaskCount;
AddOptionalSetting $offlineMigTaskProperties.input.optionalAgentSettings "MaxBatchCacheSizeMb" $MaxBatchCacheSizeMb;
AddOptionalSetting $offlineMigTaskProperties.input.optionalAgentSettings "ThrottleQueryTableDataRangeTaskAtAvailableMemoryMb" $ThrottleQueryTableDataRangeTaskAtAvailableMemoryMb;
AddOptionalSetting $offlineMigTaskProperties.input.optionalAgentSettings "ThrottleQueryTableDataRangeTaskAtBatchCount" $ThrottleQueryTableDataRangeTaskAtBatchCount;
AddOptionalSetting $offlineMigTaskProperties.input.optionalAgentSettings "DelayProgressUpdatesInStorageInterval" $DelayProgressUpdatesInStorageInterval;
```

## <a name="creating-and-running-the-migration-task"></a>Erstellen und Ausführen der Migrationsaufgabe

Nachdem die Eingabe für die Aufgabe konfiguriert wurde, wird die Aufgabe erstellt und im Agent ausgeführt. Durch das Skript wird die Aufgabenausführung ausgelöst und auf den Abschluss der Migration gewartet.

Durch das folgende Skript wird die konfigurierte Migrationsaufgabe aufgerufen und auf deren Abschluss gewartet:

```powershell
# Running the migration scenario
[string] $TaskName = "mysqlofflinemigrate"

LogMessage -Message "Running data migration scenario ..." -IsProcessing $true
$summary = @{
    "SourceServer"   = $SourceServerName;
    "SourceDatabase" = $SourceDatabaseName;
    "TargetServer"   = $TargetServerName;
    "TargetDatabase" = $TargetDatabaseName;
    "TableCount"     = $tableMap.Count;
    "StartedOn"      = $offlineMigTaskProperties.input.startedOn;
}

Write-Host "Job Summary:" -ForegroundColor Yellow
Write-Host $(ConvertTo-Json $summary) -ForegroundColor Yellow

$migrationResult = RunScenario -MigrationService $dmsService `
    -MigrationProject $dmsProject `
    -ScenarioTaskName $TaskName `
    -TaskProperties $offlineMigTaskProperties

LogMessage -Message "Migration completed with status - $($migrationResult.state)"
#Checking for any errors or warnings captured by the task during migration
$dbLevelResult = $migrationResult.output | Where-Object { $_.resultType -eq "DatabaseLevelOutput" } 
$migrationLevelResult = $migrationResult.output | Where-Object { $_.resultType -eq "MigrationLevelOutput" }
if ($dbLevelResult.exceptionsAndWarnings) {
    Write-Host "Following database errors were captured: $($dbLevelResult.exceptionsAndWarnings)" -ForegroundColor Red
}
if ($migrationLevelResult.exceptionsAndWarnings) {
    Write-Host "Following migration errors were captured: $($migrationLevelResult.exceptionsAndWarnings)" -ForegroundColor Red
}
if ($migrationResult.errors.details) {
    Write-Host "Following task level migration errors were captured: $($migrationResult.errors.details)" -ForegroundColor Red
}
```

## <a name="deleting-the-database-migration-service"></a>Löschen der Database Migration Service-Instanz

Die gleiche Database Migration Service-Instanz kann für mehrere Migrationen verwendet werden. Die Instanz muss also nur einmal erstellt werden und ist dann wiederverwendbar. Wenn Sie die Database Migration Service-Instanz nicht mehr benötigen, können Sie den Dienst mithilfe des Befehls [Remove-AzDataMigrationService](/powershell/module/az.datamigration/remove-azdatamigrationservice?) löschen.

Durch das folgende Skript werden die Azure Database Migration Service Instanz und die zugeordneten Projekte gelöscht:

```powershell
Remove-AzDataMigrationService -ResourceId $($dmsService.ResourceId)
```

## <a name="next-steps"></a>Nächste Schritte

* Informationen zu bekannten Problemen und Einschränkungen bei Migrationen mit DMS finden Sie im Artikel [Troubleshooting von häufigen Problemen und Fehlern bei Azure Database Migration Service](./known-issues-troubleshooting-dms.md).
* Informationen zur Behandlung von Problemen mit der Quelldatenbankkonnektivität bei der Verwendung von DMS finden Sie im Artikel [Problembehandlung von DMS-Fehlern beim Herstellen einer Verbindung mit Quelldatenbanken](./known-issues-troubleshooting-dms-source-connectivity.md).
* Informationen zu Azure Database Migration Service finden Sie im Artikel [Was ist Azure Database Migration Service?](./dms-overview.md).
* Informationen zu Azure Database for MySQL finden Sie im Artikel [Was ist Azure-Datenbank für MySQL?](../mysql/overview.md).
* Ein Tutorial zur Verwendung von DMS über das Portal finden Sie im Artikel [Tutorial: Durchführen einer Offlinemigration von MySQL zu Azure Database for MySQL mithilfe von DMS](./tutorial-mysql-azure-mysql-offline-portal.md).
