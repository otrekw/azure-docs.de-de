---
title: Erstellen eines Agents für elastische Aufträge mithilfe von PowerShell
description: Hier erfahren Sie, wie Sie mithilfe von PowerShell einen Agent für elastische Aufträge erstellen.
services: sql-database
ms.service: sql-database
ms.subservice: scale-out
ms.custom: seo-lt-2019, sqldbrb=1, devx-track-azurepowershell
ms.devlang: ''
ms.topic: tutorial
author: johnpaulkee
ms.author: joke
ms.reviwer: sstein
ms.date: 03/13/2019
ms.openlocfilehash: 0fe6a3b1d0e5216d923cfe3997826e7a727f1a2b
ms.sourcegitcommit: 656c0c38cf550327a9ee10cc936029378bc7b5a2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2020
ms.locfileid: "89077346"
---
# <a name="create-an-elastic-job-agent-using-powershell"></a>Erstellen eines Agents für elastische Aufträge mithilfe von PowerShell
[!INCLUDE[appliesto-sqldb](../includes/appliesto-sqldb.md)]

Mit [elastischen Aufträgen (Vorschau)](job-automation-overview.md#elastic-database-jobs-preview) können einzelne oder mehrere T-SQL-Skripts (Transact-SQL) für zahlreiche Datenbanken gleichzeitig ausgeführt werden.

In diesem Tutorial erfahren Sie, wie Sie eine datenbankübergreifende Abfrage ausführen:

> [!div class="checklist"]
> * Erstellen eines elastischen Auftrags
> * Erstellen von Auftragsanmeldeinformationen, damit Aufträge Skripts für die Ziele ausführen können
> * Definieren der Ziele (Server, Pools für elastische Datenbanken, Datenbanken, Shardzuordnungen), für die der Auftrag ausgeführt werden soll
> * Erstellen von datenbankweit gültigen Anmeldeinformationen in den Zieldatenbanken, damit der Agent eine Verbindung herstellen und Aufträge ausführen kann
> * Erstellen eines Auftrags
> * Hinzufügen von Auftragsschritten zu einem Auftrag
> * Starten der Auftragsausführung
> * Überwachen eines Auftrags

## <a name="prerequisites"></a>Voraussetzungen

Die Upgradeversion der Aufträge für die elastische Datenbank umfasst neue PowerShell-Cmdlets zur Verwendung während der Migration. Mit diesen neuen Cmdlets werden alle Ihre vorhandenen Auftragsanmeldeinformationen, Ziele (einschließlich Datenbanken, Servern, benutzerdefinierten Sammlungen), Auftragsauslöser, Auftragszeitpläne, Auftragsinhalte und Aufträge zu einem neuen Agent für elastische Aufträge übertragen.

### <a name="install-the-latest-elastic-jobs-cmdlets"></a>Installieren der neuesten Cmdlets für elastische Aufträge

Wenn Sie noch kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

Installieren Sie das Modul **Az.Sql**, um die aktuellen Cmdlets für elastische Aufträge zu erhalten. Führen Sie die folgenden Befehle in PowerShell mit Administratorzugriff aus.

```powershell
# installs the latest PackageManagement and PowerShellGet packages
Find-Package PackageManagement | Install-Package -Force
Find-Package PowerShellGet | Install-Package -Force

# Restart your powershell session with administrative access

# Install and import the Az.Sql module, then confirm
Install-Module -Name Az.Sql
Import-Module Az.Sql

Get-Module Az.Sql
```

Neben dem Modul **Az.Sql** wird für dieses Tutorial auch das PowerShell-Modul *SqlServer* benötigt. Ausführliche Informationen finden Sie unter [Installieren des SQL Server PowerShell-Moduls](/sql/powershell/download-sql-server-ps-module).

## <a name="create-required-resources"></a>Erstellen der erforderlichen Ressourcen

Um einen Agent für elastische Aufträge erstellen zu können, benötigen Sie eine [Auftragsdatenbank](job-automation-overview.md#job-database) (S0 oder höher).

Das folgende Skript erstellt eine neue Ressourcengruppe, einen neuen Server und eine neue Datenbank, die Sie als Auftragsdatenbank verwenden können. Das zweite Skript erstellt einen zweiten Server mit zwei leeren Datenbanken, für die Aufträge ausgeführt werden können.

Für elastische Aufträge gelten keine speziellen Benennungsanforderungen. Sie können also beliebige Namenskonventionen verwenden, solange diese die [Anforderungen von Azure](/azure/architecture/best-practices/resource-naming) erfüllen.

```powershell
# sign in to Azure account
Connect-AzAccount

# create a resource group
Write-Output "Creating a resource group..."
$resourceGroupName = Read-Host "Please enter a resource group name"
$location = Read-Host "Please enter an Azure Region"
$rg = New-AzResourceGroup -Name $resourceGroupName -Location $location
$rg

# create a server
Write-Output "Creating a server..."
$agentServerName = Read-Host "Please enter an agent server name"
$agentServerName = $agentServerName + "-" + [guid]::NewGuid()
$adminLogin = Read-Host "Please enter the server admin name"
$adminPassword = Read-Host "Please enter the server admin password"
$adminPasswordSecure = ConvertTo-SecureString -String $AdminPassword -AsPlainText -Force
$adminCred = New-Object -TypeName "System.Management.Automation.PSCredential" -ArgumentList $adminLogin, $adminPasswordSecure
$agentServer = New-AzSqlServer -ResourceGroupName $resourceGroupName -Location $location `
    -ServerName $agentServerName -ServerVersion "12.0" -SqlAdministratorCredentials ($adminCred)

# set server firewall rules to allow all Azure IPs
Write-Output "Creating a server firewall rule..."
$agentServer | New-AzSqlServerFirewallRule -AllowAllAzureIPs
$agentServer

# create the job database
Write-Output "Creating a blank database to be used as the Job Database..."
$jobDatabaseName = "JobDatabase"
$jobDatabase = New-AzSqlDatabase -ResourceGroupName $resourceGroupName -ServerName $agentServerName -DatabaseName $jobDatabaseName -RequestedServiceObjectiveName "S0"
$jobDatabase
```

```powershell
# create a target server and sample databases - uses the same credentials
Write-Output "Creating target server..."
$targetServerName = Read-Host "Please enter a target server name"
$targetServerName = $targetServerName + "-" + [guid]::NewGuid()
$targetServer = New-AzSqlServer -ResourceGroupName $resourceGroupName -Location $location `
    -ServerName $targetServerName -ServerVersion "12.0" -SqlAdministratorCredentials ($adminCred)

# set target server firewall rules to allow all Azure IPs
$targetServer | New-AzSqlServerFirewallRule -AllowAllAzureIPs
$targetServer | New-AzSqlServerFirewallRule -StartIpAddress 0.0.0.0 -EndIpAddress 255.255.255.255 -FirewallRuleName AllowAll
$targetServer

# create sample databases to execute jobs against
$db1 = New-AzSqlDatabase -ResourceGroupName $resourceGroupName -ServerName $targetServerName -DatabaseName "database1"
$db1
$db2 = New-AzSqlDatabase -ResourceGroupName $resourceGroupName -ServerName $targetServerName -DatabaseName "database2"
$db2
```

## <a name="use-elastic-jobs"></a>Verwenden elastischer Aufträge

Wenn Sie elastische Aufträge verwenden möchten, führen Sie den folgenden Befehl aus, um das Feature in Ihrem Azure-Abonnement zu registrieren. Führen Sie diesen Befehl einmal für das Abonnement aus, in dem Sie den Agent für elastische Aufträge bereitstellen möchten. Abonnements, die nur Datenbanken enthalten, die als Auftragsziele fungieren, müssen nicht registriert werden.

```powershell
Register-AzProviderFeature -FeatureName sqldb-JobAccounts -ProviderNamespace Microsoft.Sql
```

### <a name="create-the-elastic-job-agent"></a>Erstellen des Agents für elastische Aufträge

Bei einem Agent für elastische Aufträge handelt es sich um eine Azure-Ressource zum Erstellen, Ausführen und Verwalten von Aufträgen. Der Agent führt Aufträge gemäß einem Zeitplan oder einmalig aus.

Für das Cmdlet **New-AzSqlElasticJobAgent** muss bereits eine Datenbank in Azure SQL-Datenbank vorhanden sein. Die Parameter *resourceGroupName*, *serverName* und *databaseName* müssen daher auf vorhandene Ressourcen verweisen.

```powershell
Write-Output "Creating job agent..."
$agentName = Read-Host "Please enter a name for your new Elastic Job agent"
$jobAgent = $jobDatabase | New-AzSqlElasticJobAgent -Name $agentName
$jobAgent
```

### <a name="create-the-job-credentials"></a>Erstellen der Auftragsanmeldeinformationen

Aufträge verwenden datenbankweit gültige Anmeldeinformationen, um bei der Ausführung eine Verbindung mit den in der Zielgruppe angegebenen Zieldatenbanken herzustellen und Skripts auszuführen. Diese datenbankweit gültigen Anmeldeinformationen werden auch zur Verbindungsherstellung mit der Masterdatenbank verwendet, um alle Datenbanken auf einem Server oder in einem Pool für elastische Datenbanken aufzuzählen, sofern eine dieser Optionen der Zielgruppe angehört.

Die datenbankweit gültigen Anmeldeinformationen müssen in der Auftragsdatenbank erstellt werden. Für eine erfolgreiche Auftragsausführung müssen alle Zieldatenbanken über eine Anmeldung mit ausreichenden Berechtigungen verfügen.

![Anmeldeinformationen für elastische Aufträge](./media/elastic-jobs-powershell-create/job-credentials.png)

Beachten Sie neben den Anmeldeinformationen in der Abbildung auch die hinzugefügten Befehle vom Typ **GRANT** im folgenden Skript. Diese Berechtigungen werden für das Skript benötigt, das wir für diesen Beispielauftrag ausgewählt haben. Da im Beispiel eine neue Tabelle in den Zieldatenbanken erstellt wird, benötigt jede Zieldatenbank die passenden Berechtigungen, um eine erfolgreiche Ausführung zu gewährleisten.

Führen Sie das folgende Skript aus, um die erforderlichen Auftragsanmeldeinformationen (in der Auftragsdatenbank) zu erstellen:

```powershell
# in the master database (target server)
# create the master user login, master user, and job user login
$params = @{
  'database' = 'master'
  'serverInstance' =  $targetServer.ServerName + '.database.windows.net'
  'username' = $adminLogin
  'password' = $adminPassword
  'outputSqlErrors' = $true
  'query' = "CREATE LOGIN masteruser WITH PASSWORD='password!123'"
}
Invoke-SqlCmd @params
$params.query = "CREATE USER masteruser FROM LOGIN masteruser"
Invoke-SqlCmd @params
$params.query = "CREATE LOGIN jobuser WITH PASSWORD='password!123'"
Invoke-SqlCmd @params

# for each target database
# create the jobuser from jobuser login and check permission for script execution
$targetDatabases = @( $db1.DatabaseName, $Db2.DatabaseName )
$createJobUserScript =  "CREATE USER jobuser FROM LOGIN jobuser"
$grantAlterSchemaScript = "GRANT ALTER ON SCHEMA::dbo TO jobuser"
$grantCreateScript = "GRANT CREATE TABLE TO jobuser"

$targetDatabases | % {
  $params.database = $_
  $params.query = $createJobUserScript
  Invoke-SqlCmd @params
  $params.query = $grantAlterSchemaScript
  Invoke-SqlCmd @params
  $params.query = $grantCreateScript
  Invoke-SqlCmd @params
}

# create job credential in Job database for master user
Write-Output "Creating job credentials..."
$loginPasswordSecure = (ConvertTo-SecureString -String "password!123" -AsPlainText -Force)

$masterCred = New-Object -TypeName "System.Management.Automation.PSCredential" -ArgumentList "masteruser", $loginPasswordSecure
$masterCred = $jobAgent | New-AzSqlElasticJobCredential -Name "masteruser" -Credential $masterCred

$jobCred = New-Object -TypeName "System.Management.Automation.PSCredential" -ArgumentList "jobuser", $loginPasswordSecure
$jobCred = $jobAgent | New-AzSqlElasticJobCredential -Name "jobuser" -Credential $jobCred
```

### <a name="define-the-target-databases-to-run-the-job-against"></a>Definieren der Zieldatenbanken für die Ausführung des Auftrags

Eine [Zielgruppe](job-automation-overview.md#target-group) definiert mindestens eine Gruppe von Datenbanken, für die ein Auftragsschritt ausgeführt wird.

Der folgende Codeausschnitt erstellt zwei Zielgruppen: *serverGroup* und *serverGroupExcludingDb2*. *serverGroup* ist auf alle Datenbanken ausgerichtet, die zum Zeitpunkt der Ausführung auf dem Server vorhanden sind. *serverGroupExcludingDb2* ist ebenfalls auf alle Datenbanken auf dem Server ausgerichtet, schließt allerdings *targetDb2* aus:

```powershell
Write-Output "Creating test target groups..."
# create ServerGroup target group
$serverGroup = $jobAgent | New-AzSqlElasticJobTargetGroup -Name 'ServerGroup'
$serverGroup | Add-AzSqlElasticJobTarget -ServerName $targetServerName -RefreshCredentialName $masterCred.CredentialName

# create ServerGroup with an exclusion of db2
$serverGroupExcludingDb2 = $jobAgent | New-AzSqlElasticJobTargetGroup -Name 'ServerGroupExcludingDb2'
$serverGroupExcludingDb2 | Add-AzSqlElasticJobTarget -ServerName $targetServerName -RefreshCredentialName $masterCred.CredentialName
$serverGroupExcludingDb2 | Add-AzSqlElasticJobTarget -ServerName $targetServerName -Database $db2.DatabaseName -Exclude
```

### <a name="create-a-job-and-steps"></a>Erstellen eines Auftrags und von Schritten

In diesem Beispiel werden ein Auftrag und zwei Auftragsschritte definiert, die durch den Auftrag ausgeführt werden sollen. Der erste Auftragsschritt (*step1*) erstellt in jeder Datenbank aus der Zielgruppe *ServerGroup* eine neue Tabelle (*Step1Table*). Der zweite Auftragsschritt (*step2*) erstellt in jeder Datenbank (mit Ausnahme von *TargetDb2*, da die zuvor definierte Zielgruppe diese Datenbank ausschließt) eine neue Tabelle (*Step2Table*).

```powershell
Write-Output "Creating a new job..."
$jobName = "Job1"
$job = $jobAgent | New-AzSqlElasticJob -Name $jobName -RunOnce
$job

Write-Output "Creating job steps..."
$sqlText1 = "IF NOT EXISTS (SELECT * FROM sys.tables WHERE object_id = object_id('Step1Table')) CREATE TABLE [dbo].[Step1Table]([TestId] [int] NOT NULL);"
$sqlText2 = "IF NOT EXISTS (SELECT * FROM sys.tables WHERE object_id = object_id('Step2Table')) CREATE TABLE [dbo].[Step2Table]([TestId] [int] NOT NULL);"

$job | Add-AzSqlElasticJobStep -Name "step1" -TargetGroupName $serverGroup.TargetGroupName -CredentialName $jobCred.CredentialName -CommandText $sqlText1
$job | Add-AzSqlElasticJobStep -Name "step2" -TargetGroupName $serverGroupExcludingDb2.TargetGroupName -CredentialName $jobCred.CredentialName -CommandText $sqlText2
```

### <a name="run-the-job"></a>Ausführung des Auftrags.

Verwenden Sie den folgenden Befehl, um den Auftrag sofort zu starten:

```powershell
Write-Output "Start a new execution of the job..."
$jobExecution = $job | Start-AzSqlElasticJob
$jobExecution
```

Nach erfolgreichem Abschluss des Vorgangs sehen Sie in „TargetDb1“ zwei neue Tabellen und in „TargetDb2“ nur eine neue Tabelle:

   ![Überprüfen der neuen Tabellen in SSMS](./media/elastic-jobs-powershell-create/job-execution-verification.png)

Sie können auch eine spätere Ausführung des Auftrags planen. Führen Sie den folgenden Befehl aus, um die Ausführung eines Auftrags für einen späteren Zeitpunkt zu planen:

```powershell
# run every hour starting from now
$job | Set-AzSqlElasticJob -IntervalType Hour -IntervalCount 1 -StartTime (Get-Date) -Enable
```

### <a name="monitor-status-of-job-executions"></a>Überwachen des Auftragsausführungsstatus

Die folgenden Codeausschnitte dienen zum Abrufen von Details zur Auftragsausführung:

```powershell
# get the latest 10 executions run
$jobAgent | Get-AzSqlElasticJobExecution -Count 10

# get the job step execution details
$jobExecution | Get-AzSqlElasticJobStepExecution

# get the job target execution details
$jobExecution | Get-AzSqlElasticJobTargetExecution -Count 2
```

In der folgenden Tabelle werden die möglichen Auftragsausführungsstatus aufgeführt:

|State|BESCHREIBUNG|
|:---|:---|
|**Erstellt** | Die Ausführung des Auftrags wurde gerade erstellt und wird noch nicht durchgeführt.|
|**InProgress** | Der Auftrag wird zurzeit ausgeführt.|
|**WaitingForRetry** | Die Aktion der Auftragsausführung konnte nicht abgeschlossen werden, und es wird auf einen erneuten Versuch gewartet.|
|**Erfolgreich** | Der Auftrag wurde erfolgreich ausgeführt.|
|**SucceededWithSkipped** | Der Auftrag wurde erfolgreich ausgeführt, aber einige seiner untergeordneten Schritte wurden übersprungen.|
|**Fehler** | Bei der Ausführung des Auftrags sind Fehler aufgetreten und die Anzahl der möglichen Wiederholungsversuche ist ausgeschöpft.|
|**TimedOut** | Für die Ausführung des Auftrags ist ein Timeout in Kraft getreten.|
|**Canceled** | Die Ausführung des Auftrags wurde abgebrochen.|
|**Übersprungen** | Die Auftragsausführung wurde übersprungen, weil eine andere Ausführung des gleichen Auftragsschritts bereits auf dem gleichen Ziel durchgeführt wurde.|
|**WaitingForChildJobExecutions** | Die Ausführung des Auftrags wartet auf den Abschluss untergeordneter Schritte.|

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Löschen Sie die Ressourcengruppe, um die in diesem Tutorial erstellten Azure-Ressourcen zu löschen.

> [!TIP]
> Wenn Sie diese Aufträge weiterverwenden möchten, überspringen Sie die Bereinigung der in diesem Artikel erstellten Ressourcen.

```powershell
Remove-AzResourceGroup -ResourceGroupName $resourceGroupName
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie ein Transact-SQL-Skript für eine Gruppe von Datenbanken ausgeführt. Sie haben gelernt, wie Sie folgende Aufgaben ausführen:

> [!div class="checklist"]
> * Erstellen eines elastischen Auftrags
> * Erstellen von Auftragsanmeldeinformationen, damit Aufträge Skripts für die Ziele ausführen können
> * Definieren der Ziele (Server, Pools für elastische Datenbanken, Datenbanken, Shardzuordnungen), für die der Auftrag ausgeführt werden soll
> * Erstellen von datenbankweit gültigen Anmeldeinformationen in den Zieldatenbanken, damit der Agent eine Verbindung herstellen und Aufträge ausführen kann
> * Erstellen eines Auftrags
> * Hinzufügen eines Auftragsschritts zum Auftrag
> * Starten der Auftragsausführung
> * Überwachen des Auftrags

> [!div class="nextstepaction"]
> [Use Transact-SQL (T-SQL) to create and manage Elastic Database Jobs](elastic-jobs-tsql-create-manage.md) (Erstellen und Verwalten von Aufträgen für die elastische Datenbank mithilfe von Transact-SQL (T-SQL))
