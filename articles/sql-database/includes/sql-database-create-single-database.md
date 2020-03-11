---
author: MashaMSFT
ms.service: sql-database
ms.subservice: single-database
ms.topic: include
ms.date: 02/14/2020
ms.author: mathoma
ms.reviewer: vanto
ms.openlocfilehash: d800d273cce995c618422a3a9d0934b2657e6ef5
ms.sourcegitcommit: 225a0b8a186687154c238305607192b75f1a8163
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/29/2020
ms.locfileid: "78194296"
---
In diesem Schritt erstellen Sie eine Azure SQL-Einzeldatenbank. 

> [!IMPORTANT]
> Richten Sie auf dem für diesen Artikel verwendeten Computer Firewallregeln für die öffentliche IP-Adresse des Computers ein.
>
> Weitere Informationen finden Sie unter [Erstellen einer Firewallregel auf Datenbankebene](/sql/relational-databases/system-stored-procedures/sp-set-database-firewall-rule-azure-sql-database). Eine Beschreibung, wie Sie die IP-Adresse für die Firewallregel auf Serverebene für Ihren Computer ermitteln, finden Sie unter [Erstellen einer Firewall auf Serverebene](../sql-database-server-level-firewall-rule.md).  

# <a name="portal"></a>[Portal](#tab/azure-portal)

Erstellen Sie Ihre Ressourcengruppe und einzelne Datenbanken über das Azure-Portal.

1. Wählen Sie im linken Menü im [Azure-Portal](https://portal.azure.com) die Option **Azure SQL** aus. Wenn **Azure SQL** in der Liste nicht aufgeführt ist, wählen Sie **Alle Dienste** aus, und geben Sie dann *Azure SQL* ins Suchfeld ein. (Optional:) Wählen Sie den Stern neben **Azure SQL** aus, um die Option als Favorit zu markieren und als Element im linken Navigationsbereich hinzuzufügen. 
2. Wählen Sie **+Hinzufügen** aus, um die Seite **SQL-Bereitstellungsoption auswählen** zu öffnen. Sie können weitere Informationen zu den verschiedenen Datenbanken anzeigen, indem Sie auf der Kachel **Datenbanken** die Option **Details anzeigen** auswählen.
3. Wählen Sie **Erstellen** aus:

   ![Erstellen einer Einzeldatenbank](../media/sql-database-get-started-portal/create-single-database.png)

4. Geben Sie auf der Registerkarte **Grundlagen** im Abschnitt **Projektdetails** die folgenden Werte ein, bzw. wählen Sie sie aus:

   - **Abonnement**: Wählen Sie in der Dropdownliste das richtige Abonnement aus, falls es nicht angezeigt wird.
   - **Ressourcengruppe**: Wählen Sie **Neu erstellen**, geben Sie `myResourceGroup` ein, und wählen Sie **OK**.

     ![Neue SQL-Datenbank: Registerkarte „Grundlagen“](../media/sql-database-get-started-portal/new-sql-database-basics.png)

5. Geben Sie im Abschnitt **Datenbankdetails** die folgenden Werte ein, bzw. wählen Sie sie aus:

   - **Datenbankname**: Geben Sie `mySampleDatabase` ein.
   - **Server**: Wählen Sie **Neu erstellen**, geben Sie die folgenden Werte ein, und wählen Sie anschließend die Option **Auswählen** aus.
       - **Servername**: Geben Sie `mysqlserver` mit einigen Zahlen ein, um die Eindeutigkeit sicherzustellen.
       - **Serveradministratoranmeldung**: Geben Sie `azureuser`ein.
       - **Kennwort**: Geben Sie ein komplexes Kennwort ein, das die Anforderungen für Kennwörter erfüllt.
       - **Standort**: Wählen Sie in der Dropdownliste einen Standort aus, z. B. `West US`.

         ![Neuer Server](../media/sql-database-get-started-portal/new-server.png)

      > [!IMPORTANT]
      > Notieren Sie sich die Serveradministratoranmeldung (Anmelde-ID) und das Kennwort, damit Sie sich für diesen und andere Schnellstarts beim Server und bei den Datenbanken anmelden können. Falls Sie die Anmeldeinformationen vergessen, können Sie auf der Seite **SQL Server** die Anmelde-ID abrufen oder das Kennwort zurücksetzen. Um die Seite **SQL Server** zu öffnen, wählen Sie nach dem Erstellen der Datenbank auf der Seite **Übersicht** für die Datenbank den Servernamen aus.

   - **Möchten Sie einen Pool für elastische SQL-Datenbanken verwenden?** Wählen Sie die Option **Nein**.
   - **Compute und Speicher**: Wählen Sie **Datenbank konfigurieren** aus. 

     ![SQL-Datenbankdetails](../media/sql-database-get-started-portal/sql-db-basic-db-details.png)

   - Wählen Sie **Bereitgestellt** aus.  Oder wählen Sie **Serverlos** aus, um eine serverlose Datenbank zu erstellen.

     ![Bereitgestellt Gen4](../media/sql-database-get-started-portal/create-database-provisioned.png)

   - Überprüfen Sie die Einstellungen für **Virtuelle Kerne** und **Maximale Datengröße**. Ändern Sie diese wie gewünscht. 
     - Optional können Sie auch **Konfiguration ändern** auswählen, um die Hardwaregeneration zu ändern.
   - Wählen Sie **Übernehmen**.

6. Wählen Sie die Registerkarte **Netzwerk** aus, und entscheiden Sie, ob Sie [**anderen Azure-Diensten und -Ressourcen den Zugriff auf diesen Server gestatten**](../sql-database-networkaccess-overview.md) oder einen [privaten Endpunkt](../../private-link/private-endpoint-overview.md) hinzufügen möchten.

   ![Registerkarte „Netzwerk“](../media/sql-database-get-started-portal/create-database-networking.png)

7. Wählen Sie die Registerkarte **Zusätzliche Einstellungen**. 
8. Wählen Sie im Abschnitt **Datenquelle** unter **Vorhandene Daten verwenden** die Option `Sample`.

   ![Weitere Einstellungen der SQL-Datenbank](../media/sql-database-get-started-portal/create-sql-database-additional-settings.png)

   > [!IMPORTANT]
   > Sie müssen die Daten **Sample (AdventureWorksLT)** auswählen, um diesen Schnellstart und andere Schnellstarts für Azure SQL-Datenbank, in denen diese Daten verwendet werden, einfach ausführen zu können.

9. Behalten Sie die restlichen Standardwerte bei, und wählen Sie unten im Formular die Option **Bewerten + erstellen**.
10. Überprüfen Sie die endgültigen Einstellungen, und wählen Sie **Erstellen**.

11. Klicken Sie im Formular **SQL-Datenbank** auf **Erstellen**, um die Ressourcengruppe, den Server und die Datenbank bereitzustellen.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Erstellen Sie Ihre Ressourcengruppe und einzelne Datenbanken mit PowerShell.

   ```powershell-interactive
   # Set variables for your server and database
   $subscriptionId = '<SubscriptionID>'
   $resourceGroupName = "myResourceGroup-$(Get-Random)"
   $location = "West US"
   $adminLogin = "azureuser"
   $password = "PWD27!"+(New-Guid).Guid
   $serverName = "mysqlserver-$(Get-Random)"
   $databaseName = "mySampleDatabase"

   # The ip address range that you want to allow to access your server 
   # (leaving at 0.0.0.0 will prevent outside-of-azure connections to your DB)
   $startIp = "0.0.0.0"
   $endIp = "0.0.0.0"

   # Show randomized variables
   Write-host "Resource group name is" $resourceGroupName 
   Write-host "Password is" $password  
   Write-host "Server name is" $serverName 

   # Connect to Azure
   Connect-AzAccount

   # Set subscription ID
   Set-AzContext -SubscriptionId $subscriptionId 

   # Create a resource group
   Write-host "Creating resource group..."
   $resourceGroup = New-AzResourceGroup -Name $resourceGroupName -Location $location -Tag @{Owner="SQLDB-Samples"}
   $resourceGroup

   # Create a server with a system wide unique server name
   Write-host "Creating primary logical server..."
   $server = New-AzSqlServer -ResourceGroupName $resourceGroupName `
      -ServerName $serverName `
      -Location $location `
      -SqlAdministratorCredentials $(New-Object -TypeName System.Management.Automation.PSCredential `
      -ArgumentList $adminLogin, $(ConvertTo-SecureString -String $password -AsPlainText -Force))
   $server

   # Create a server firewall rule that allows access from the specified IP range
   Write-host "Configuring firewall for primary logical server..."
   $serverFirewallRule = New-AzSqlServerFirewallRule -ResourceGroupName $resourceGroupName `
      -ServerName $serverName `
      -FirewallRuleName "AllowedIPs" -StartIpAddress $startIp -EndIpAddress $endIp
   $serverFirewallRule

   # Create General Purpose Gen4 database with 1 vCore
   Write-host "Creating a gen5 2 vCore database..."
   $database = New-AzSqlDatabase  -ResourceGroupName $resourceGroupName `
      -ServerName $serverName `
      -DatabaseName $databaseName `
      -Edition GeneralPurpose `
      -VCore 2 `
      -ComputeGeneration Gen5 `
      -MinimumCapacity 2 `
      -SampleName "AdventureWorksLT"
   $database
   ```

In diesem Teil des Artikels werden die folgenden PowerShell-Cmdlets verwendet:

| Get-Help | Notizen |
|---|---|
| [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [New-AzSqlServer](/powershell/module/az.sql/new-azsqlserver) | Erstellt einen SQL-Datenbank-Server, der Einzeldatenbanken und Pools für elastische Datenbanken hostet. |
| [New-AzSqlServerFirewallRule](/powershell/module/az.sql/new-azsqlserverfirewallrule) | Erstellt eine Firewallregel für einen logischen Server. | 
| [New-AzSqlDatabase](/powershell/module/az.sql/new-azsqldatabase) | Erstellt eine neue Azure SQL-Einzeldatenbank. | 

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

Erstellen Sie Ihre Ressourcengruppe und einzelne Datenbanken mit der Azure CLI.

   ```azurecli-interactive
   #!/bin/bash
   # set variables
   $subscription = "<subscriptionID>"
   $randomIdentifier = $(Get-Random)

   $resourceGroup = "resource-$randomIdentifier"
   $location = "East US"
   
   $login = "sampleLogin"
   $password = "samplePassword123!"

   $server = "server-$randomIdentifier"
   $database = "database-$randomIdentifier"
  
   az login # connect to Azure
   az account set -s $subscription # set subscription context for the Azure account

   echo "Creating resource group..."
   az group create --name $resourceGroup --location $location

   echo "Creating primary logical server..."
   az sql server create --name $server --resource-group $resourceGroup --location $location --admin-user $login --admin-password $password

   echo "Creating a gen5 2 vCore database..."
   az sql db create --resource-group $resourceGroup --server $server --name $database --sample-name AdventureWorksLT --edition GeneralPurpose --family Gen5 --capacity 2
   ```

Das Skript verwendet die folgenden Befehle. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [az account set](/cli/azure/account?view=azure-cli-latest#az-account-set) | Legt ein Abonnement als aktuelles aktives Abonnement fest. | 
| [az group create](/cli/azure/group#az-group-create) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [az sql server create](/cli/azure/sql/server#az-sql-server-create) | Erstellt einen SQL-Datenbank-Server, der Einzeldatenbanken und Pools für elastische Datenbanken hostet. |
| [az sql server firewall-rule create](/cli/azure/sql/server/firewall-rule) | Erstellt die Firewallregeln eines Servers. | 
| [az sql db create](/cli/azure/sql/db?view=azure-cli-latest) | Erstellt eine Datenbank. | 


---
