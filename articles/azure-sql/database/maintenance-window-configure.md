---
title: Konfigurieren von Wartungsfenstern (Vorschau)
description: Erfahren Sie, wie Sie die Zeit festlegen, zu der eine geplante Wartung für Ihre Azure SQL-Datenbanken, Pools für elastische Datenbanken und Datenbanken für verwaltete Instanzen ausgeführt werden soll.
services: sql-database
ms.service: sql-db-mi
ms.subservice: service
ms.topic: how-to
author: stevestein
ms.author: sstein
ms.reviewer: ''
ms.date: 03/02/2021
ms.openlocfilehash: ae31b329cbea82ac664af24e42ec58d094b3a7ae
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/02/2021
ms.locfileid: "101660693"
---
# <a name="configure-maintenance-window-preview"></a>Konfigurieren von Wartungsfenstern (Vorschau)
[!INCLUDE[appliesto-sqldb-sqlmi](../includes/appliesto-sqldb-sqlmi.md)]


Konfigurieren Sie während oder jederzeit nach der Erstellung von Ressourcen das [Wartungsfenster (Vorschau)](maintenance-window.md) für eine Azure SQL-Datenbank, einen Pool für elastische Datenbanken oder eine Datenbank für eine verwaltete Azure SQL-Instanz. 

Das als *Systemstandard* vordefinierte Wartungsfenster ist täglich auf die Zeit von 17 Uhr bis 8 Uhr (Ortszeit der Azure-Region, in der sich die Ressource befindet) festgelegt, um Unterbrechungen während der Hauptgeschäftszeiten zu vermeiden. Wenn der *Systemstandard* für das Wartungsfenster nicht optimal ist, wählen Sie eines der anderen verfügbaren Wartungsfenster aus.

Die Möglichkeit, zu einem anderen Wartungsfenster zu wechseln, ist nicht für jede Dienstebene oder in jeder Region verfügbar. Weitere Informationen zur Verfügbarkeit finden Sie unter [Verfügbarkeit von Wartungsfenstern](maintenance-window.md#availability).


## <a name="configure-maintenance-window-during-database-creation"></a>Konfigurieren des Wartungsfensters beim Erstellen der Datenbank 

# <a name="portal"></a>[Portal](#tab/azure-portal)

Um das Wartungsfenster beim Erstellen einer Datenbank, eines Pools für elastische Datenbanken oder einer verwalteten Instanz zu konfigurieren, legen Sie auf der Seite **Zusätzliche Einstellungen** das gewünschte **Wartungsfenster** fest. 

## <a name="set-the-maintenance-window-while-creating-a-single-database-or-elastic-pool"></a>Festlegen des Wartungsfensters beim Erstellen einer Einzeldatenbank oder eines Pools für elastische Datenbanken

Ausführliche Informationen zum Erstellen einer neuen Datenbank oder eines Pools finden Sie unter [Erstellen einer Azure SQL-Einzeldatenbank](single-database-create-quickstart.md).

   :::image type="content" source="media/maintenance-window-configure/additional-settings.png" alt-text="Registerkarte „Zusätzliche Einstellungen“ beim Erstellen einer Datenbank":::


## <a name="set-the-maintenance-window-while-creating-a-managed-instance"></a>Festlegen des Wartungsfensters beim Erstellen einer verwalteten Instanz

Ausführliche Informationen zum Erstellen einer neuen verwalteten Instanz finden Sie unter [Erstellen einer verwalteten Azure SQL-Instanz](../managed-instance/instance-create-quickstart.md).

   :::image type="content" source="media/maintenance-window-configure/additional-settings-mi.png" alt-text="Registerkarte „Zusätzliche Einstellungen“ beim Erstellen einer verwalteten Instanz":::




# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

In den folgenden Beispielen wird gezeigt, wie das Wartungsfenster mit Azure PowerShell konfiguriert wird. Sie können [Azure PowerShell installieren](/powershell/azure/install-az-ps)oder die Azure Cloud Shell verwenden.

## <a name="launch-azure-cloud-shell"></a>Starten von Azure Cloud Shell

Azure Cloud Shell ist eine kostenlose interaktive Shell, mit der Sie die Schritte in diesem Artikel ausführen können. Sie verfügt über allgemeine vorinstallierte Tools und ist für die Verwendung mit Ihrem Konto konfiguriert. 

Wählen Sie zum Öffnen von Cloud Shell oben rechts in einem Codeblock einfach die Option **Ausprobieren**. Sie können Cloud Shell auch auf einer separaten Browserregisterkarte starten, indem Sie zu [https://shell.azure.com/powershell](https://shell.azure.com/powershell) navigieren. Wählen Sie **Kopieren**, um die Blöcke mit dem Code zu kopieren. Fügen Sie ihn anschließend in Cloud Shell ein, und drücken Sie die EINGABETASTE, um ihn auszuführen.


## <a name="discover-available-maintenance-windows"></a>Ermitteln verfügbarer Wartungsfenster

Beim Festlegen des Wartungsfensters stehen für jede Region eigene Wartungsfensteroptionen zur Verfügung, die der Zeitzone für die Region entsprechen, in der sich die Datenbank oder der Pool befindet. 

### <a name="discover-sql-database-and-elastic-pool-maintenance-windows"></a>Ermitteln von Wartungsfenstern für SQL-Datenbank und Pools für elastische Datenbanken 

Im folgenden Beispiel werden mit dem Cmdlet [Get-AzMaintenancePublicConfiguration](/powershell/module/az.maintenance/get-azmaintenancepublicconfiguration) die verfügbaren Wartungsfenster für die Region *eastus2* zurückgegeben. Legen Sie für Datenbanken und Pools für elastische Datenbanken die Option `MaintenanceScope` auf `SQLDB` fest.

   ```powershell-interactive
   $location = "eastus2"

   Write-Host "Available maintenance schedules in ${location}:"
   $configurations = Get-AzMaintenancePublicConfiguration
   $configurations | ?{ $_.Location -eq $location -and $_.MaintenanceScope -eq "SQLDB"}
   ```

### <a name="discover-sql-managed-instance-maintenance-windows"></a>Ermitteln von Wartungsfenstern für verwaltete SQL-Instanzen 

Im folgenden Beispiel werden mit dem Cmdlet [Get-AzMaintenancePublicConfiguration](/powershell/module/az.maintenance/get-azmaintenancepublicconfiguration) die verfügbaren Wartungsfenster für die Region *eastus2* zurückgegeben. Legen Sie für verwaltete Instanzen die Option `MaintenanceScope` auf `SQLManagedInstance` fest.

   ```powershell-interactive
   $location = "eastus2"

   Write-Host "Available maintenance schedules in ${location}:"
   $configurations = Get-AzMaintenancePublicConfiguration
   $configurations | ?{ $_.Location -eq $location -and $_.MaintenanceScope -eq "SQLManagedInstance"}
   ```


## <a name="set-the-maintenance-window-while-creating-a-single-database"></a>Festlegen des Wartungsfensters beim Erstellen einer Einzeldatenbank

Im folgenden Beispiel wird eine neue Datenbank erstellt, und das Wartungsfenster wird mit dem Cmdlet [New-AzSqlDatabase](/powershell/module/az.sql/new-azsqldatabase) festgelegt. Die Option `-MaintenanceConfigurationId` muss auf einen gültigen Wert für die Region der Datenbank festgelegt werden. Informationen zum Abrufen gültiger Werte für Ihre Region finden Sie unter [Ermitteln verfügbarer Wartungsfenster](#discover-available-maintenance-windows).


   ```powershell-interactive
    # Set variables for your database
    $resourceGroupName = "your_resource_group_name"
    $serverName = "your_server_name"
    $databaseName = "your_db_name"
    
    # Set selected maintenance window
    $maintenanceConfig = "SQL_EastUS2_DB_1"

    Write-host "Creating a gen5 2 vCore database with maintenance window ${maintenanceConfig} ..."
    $database = New-AzSqlDatabase `
      -ResourceGroupName $resourceGroupName `
      -ServerName $serverName `
      -DatabaseName $databaseName `
      -Edition GeneralPurpose `
      -ComputeGeneration Gen5 `
      -VCore 2 `
      -MaintenanceConfigurationId $maintenanceConfig
    $database
   ```



## <a name="set-the-maintenance-window-while-creating-an-elastic-pool"></a>Festlegen des Wartungsfensters beim Erstellen eines Pools für elastische Datenbanken

Im folgenden Beispiel wird ein neuer Pool für elastische Datenbanken erstellt, und das Wartungsfenster wird mit dem Cmdlet [New-AzSqlElasticPool](/powershell/module/az.sql/new-azsqlelasticpool) festgelegt. Das Wartungsfenster wird für den Pool für elastische Datenbanken festgelegt, sodass für alle Datenbanken im Pool der Wartungsfensterzeitplan des Pools gilt. Die Option `-MaintenanceConfigurationId` muss auf einen gültigen Wert für die Region des Pools festgelegt werden. Informationen zum Abrufen gültiger Werte für Ihre Region finden Sie unter [Ermitteln verfügbarer Wartungsfenster](#discover-available-maintenance-windows).


   ```powershell-interactive
    # Set variables for your pool
    $resourceGroupName = "your_resource_group_name"
    $serverName = "your_server_name"
    $poolName = "your_pool_name"
    
    # Set selected maintenance window
    $maintenanceConfig = "SQL_EastUS2_DB_2"

    Write-host "Creating a Standard 50 pool with maintenance window ${maintenanceConfig} ..."
    $pool = New-AzSqlElasticPool `
      -ResourceGroupName $resourceGroupName `
      -ServerName $serverName `
      -ElasticPoolName $poolName `
      -Edition "Standard" `
      -Dtu 50 `
      -DatabaseDtuMin 10 `
      -DatabaseDtuMax 20 `
      -MaintenanceConfigurationId $maintenanceConfig
    $pool
   ```

## <a name="set-the-maintenance-window-while-creating-a-managed-instance"></a>Festlegen des Wartungsfensters beim Erstellen einer verwalteten Instanz

Im folgenden Beispiel wird eine neue verwaltete Instanz erstellt, und das Wartungsfenster wird mit dem Cmdlet [New-AzSqlInstance](/powershell/module/az.sql/new-azsqlinstance) festgelegt. Das Wartungsfenster wird für die-Instanz festgelegt, sodass für alle Datenbanken in der Instanz der Wartungsfensterzeitplan der Instanz gilt. Für die Option `-MaintenanceConfigurationId` muss der Wert für *MaintenanceConfigName* für die Region der Instanz gültig sein. Informationen zum Abrufen gültiger Werte für Ihre Region finden Sie unter [Ermitteln verfügbarer Wartungsfenster](#discover-available-maintenance-windows).


   ```powershell
   New-AzSqlInstance -Name "your_mi_name" `
     -ResourceGroupName "your_resource_group_name" `
     -Location "your_mi_location" `
     -SubnetId /subscriptions/{SubID}/resourceGroups/{ResourceGroup}/providers/Microsoft.Network/virtualNetworks/{VNETName}/subnets/{SubnetName} `
     -MaintenanceConfigurationId "/subscriptions/{SubID}/providers/Microsoft.Maintenance/publicMaintenanceConfigurations/SQL_{Region}_{MaintenanceConfigName}" `
     -AsJob
   ```

# <a name="cli"></a>[BEFEHLSZEILENSCHNITTSTELLE (CLI)](#tab/azure-cli)

In den folgenden Beispielen wird gezeigt, wie das Wartungsfenster über die Azure-Befehlszeilenschnittstelle (CLI) konfiguriert wird. Sie können die [Azure CLI installieren](/cli/azure/install-azure-cli) oder Azure Cloud Shell verwenden. 

Das Konfigurieren des Wartungsfensters über die Azure-Befehlszeilenschnittstelle (CLI) ist nur für verwaltete SQL-Instanzen verfügbar.

## <a name="launch-azure-cloud-shell"></a>Starten von Azure Cloud Shell

Azure Cloud Shell ist eine kostenlose interaktive Shell, mit der Sie die Schritte in diesem Artikel ausführen können. Sie verfügt über allgemeine vorinstallierte Tools und ist für die Verwendung mit Ihrem Konto konfiguriert. 

Wählen Sie zum Öffnen von Cloud Shell oben rechts in einem Codeblock einfach die Option **Ausprobieren**. Sie können Cloud Shell auch auf einer separaten Browserregisterkarte starten, indem Sie zu [https://shell.azure.com/cli](https://shell.azure.com/cli) navigieren. Wählen Sie **Kopieren**, um die Blöcke mit dem Code zu kopieren. Fügen Sie ihn anschließend in Cloud Shell ein, und drücken Sie die EINGABETASTE, um ihn auszuführen.

## <a name="discover-available-maintenance-windows"></a>Ermitteln verfügbarer Wartungsfenster

Beim Festlegen des Wartungsfensters stehen für jede Region eigene Wartungsfensteroptionen zur Verfügung, die der Zeitzone für die Region entsprechen, in der sich die Datenbank oder der Pool befindet.

### <a name="discover-sql-database-and-elastic-pool-maintenance-windows"></a>Ermitteln von Wartungsfenstern für SQL-Datenbank und Pools für elastische Datenbanken

Im folgenden Beispiel werden mit dem Befehl [az maintenance public-configuration list](/cli/azure/ext/maintenance/maintenance/public-configuration?view=azure-cli-latest&preserve-view=true#ext_maintenance_az_maintenance_public_configuration_list) die verfügbaren Wartungsfenster für die Region *eastus2* zurückgegeben. Legen Sie für Datenbanken und Pools für elastische Datenbanken die Option `maintenanceScope` auf `SQLDB` fest.

   ```azurecli
   location="eastus2"

   az maintenance public-configuration list --query "[?location=='$location'&&contains(maintenanceScope,'SQLDB')]"
   ```

### <a name="discover-sql-managed-instance-maintenance-windows"></a>Ermitteln von Wartungsfenstern für verwaltete SQL-Instanzen

Im folgenden Beispiel werden mit dem Befehl [az maintenance public-configuration list](/cli/azure/ext/maintenance/maintenance/public-configuration?view=azure-cli-latest&preserve-view=true#ext_maintenance_az_maintenance_public_configuration_list) die verfügbaren Wartungsfenster für die Region *eastus2* zurückgegeben. Legen Sie für verwaltete Instanzen die Option `maintenanceScope` auf `SQLManagedInstance` fest.

   ```azurecli
   az maintenance public-configuration list --query "[?location=='eastus2'&&contains(maintenanceScope,'SQLManagedInstance')]"
   ```

## <a name="set-the-maintenance-window-while-creating-a-single-database"></a>Festlegen des Wartungsfensters beim Erstellen einer Einzeldatenbank

Im folgenden Beispiel wird eine neue Datenbank erstellt, und das Wartungsfenster wird mit dem Befehl [az sql db create](/cli/azure/sql/db#az_sql_db_create) festgelegt. Der Wert für `--maint-config-id` (oder `-m`) muss für die Region der Datenbank gültig sein. Informationen zum Abrufen gültiger Werte für Ihre Region finden Sie unter [Ermitteln verfügbarer Wartungsfenster](#discover-available-maintenance-windows).


   ```azurecli
    # Set variables for your database
    resourceGroupName="your_resource_group_name"
    serverName="your_server_name"
    databaseName="your_db_name"

    # Set selected maintenance window
    maintenanceConfig="SQL_EastUS2_DB_1"

    # Create database
    az sql db create \
      --resource-group $resourceGroupName \
      --server $serverName \
      --name $databaseName \
      --edition GeneralPurpose \
      --family Gen5 \
      --capacity 2 \
      --maint-config-id $maintenanceConfig
   ```

## <a name="set-the-maintenance-window-while-creating-an-elastic-pool"></a>Festlegen des Wartungsfensters beim Erstellen eines Pools für elastische Datenbanken

Im folgenden Beispiel wird ein neuer Pool für elastische Datenbanken erstellt, und das Wartungsfenster wird mit dem Befehl [az sql elastic-pool create](/cli/azure/sql/elastic-pool#az_sql_elastic_pool_create) festgelegt. Das Wartungsfenster wird für den Pool für elastische Datenbanken festgelegt, sodass für alle Datenbanken im Pool der Wartungsfensterzeitplan des Pools gilt. Der Wert für `--maint-config-id` (oder `-m`) muss für die Region des Pools gültig sein. Informationen zum Abrufen gültiger Werte für Ihre Region finden Sie unter [Ermitteln verfügbarer Wartungsfenster](#discover-available-maintenance-windows).


   ```azurecli
    # Set variables for your pool
    resourceGroupName="your_resource_group_name"
    serverName="your_server_name"
    poolName="your_pool_name"

    # Set selected maintenance window
    maintenanceConfig="SQL_EastUS2_DB_2"

    # Create elastic pool
    az sql elastic-pool create \
      --resource-group $resourceGroupName \
      --server $serverName \
      --name $poolName \
      --edition GeneralPurpose \
      --family Gen5 \
      --capacity 2 \
      --maint-config-id $maintenanceConfig
   ```

## <a name="set-the-maintenance-window-while-creating-a-managed-instance"></a>Festlegen des Wartungsfensters beim Erstellen einer verwalteten Instanz

Im folgenden Beispiel wird eine neue verwaltete Instanz erstellt, und das Wartungsfenster wird mit dem Befehl [az sql mi create](/cli/azure/sql/mi#az_sql_mi_create) festgelegt. Das Wartungsfenster wird für die-Instanz festgelegt, sodass für alle Datenbanken in der Instanz der Wartungsfensterzeitplan der Instanz gilt. Der Wert für *MaintenanceConfigName* muss für die Region der Instanz gültig sein. Informationen zum Abrufen gültiger Werte für Ihre Region finden Sie unter [Ermitteln verfügbarer Wartungsfenster](#discover-available-maintenance-windows).

   ```azurecli
   az sql mi create -g mygroup -n myinstance -l mylocation -i -u myusername -p mypassword --subnet /subscriptions/{SubID}/resourceGroups/{ResourceGroup}/providers/Microsoft.Network/virtualNetworks/{VNETName}/subnets/{SubnetName} -m /subscriptions/{SubID}/providers/Microsoft.Maintenance/publicMaintenanceConfigurations/SQL_{Region}_{MaintenanceConfigName}
   ```

-----

## <a name="configure-maintenance-window-for-existing-databases"></a>Konfigurieren von Wartungsfenstern für vorhandene Datenbanken


Wenn Sie eine Wartungsfensterauswahl auf eine Datenbank anwenden, kann in manchen Fällen ein kurzer Failover (mehrere Sekunden) auftreten, während Azure die erforderlichen Änderungen anwendet.

# <a name="portal"></a>[Portal](#tab/azure-portal)

Mit den folgenden Schritten wird im Azure-Portal das Wartungsfenster für eine vorhandene Datenbank, einen Pool für elastische Datenbanken oder eine verwaltete Instanz festgelegt:


## <a name="set-the-maintenance-window-for-an-existing-database-or-elastic-pool"></a>Festlegen des Wartungsfensters für eine vorhandene Datenbank oder einen vorhandenen Pool für elastische Datenbanken

1. Navigieren Sie zu der SQL-Datenbank oder dem Pool für elastische Datenbanken, für die bzw. den Sie das Wartungsfenster festlegen möchten.
1. Wählen Sie im Menü **Einstellungen** die Option **Wartung** aus, und wählen Sie dann das gewünschte Wartungsfenster aus.

   :::image type="content" source="media/maintenance-window-configure/maintenance.png" alt-text="Seite „SQL-Datenbank-Wartung“":::


## <a name="set-the-maintenance-window-for-an-existing-managed-instance"></a>Festlegen des Wartungsfensters für eine vorhandene verwaltete Instanz

1. Navigieren Sie zu der verwalteten-Instanz, für die Sie das Wartungsfenster festlegen möchten.
1. Wählen Sie im Menü **Einstellungen** die Option **Wartung** aus, und wählen Sie dann das gewünschte Wartungsfenster aus.

   :::image type="content" source="media/maintenance-window-configure/maintenance.png" alt-text="Seite „SQL-Datenbank-Wartung“":::



# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

## <a name="set-the-maintenance-window-for-an-existing-database"></a>Festlegen des Wartungsfensters für eine vorhandene Datenbank

Im folgenden Beispiel wird mit dem Cmdlet [Set-AzSqlDatabase](/powershell/module/az.sql/set-azsqldatabase) das Wartungsfenster für eine vorhandene Datenbank festgelegt. Der Wert für `-MaintenanceConfigurationId` muss für die Region der Datenbank gültig sein. Informationen zum Abrufen gültiger Werte für Ihre Region finden Sie unter [Ermitteln verfügbarer Wartungsfenster](#discover-available-maintenance-windows).

   ```powershell-interactive
    # Select different maintenance window
    $maintenanceConfig = "SQL_EastUS2_DB_2"

    Write-host "Changing database maintenance window to ${maintenanceConfig} ..."
    $database = Set-AzSqlDatabase `
      -ResourceGroupName $resourceGroupName `
      -ServerName $serverName `
      -DatabaseName $databaseName `
      -MaintenanceConfigurationId $maintenanceConfig
    $database
   ```

## <a name="set-the-maintenance-window-on-an-existing-elastic-pool"></a>Festlegen des Wartungsfensters für einen vorhandenen Pool für elastische Datenbanken

Im folgenden Beispiel wird mit dem Cmdlet [Set-AzSqlElasticPool](/powershell/module/az.sql/set-azsqlelasticpool) das Wartungsfenster für einen vorhandenen Pool für elastische Datenbanken festgelegt. Es ist wichtig sicherzustellen, dass der Wert für `$maintenanceConfig` für die Region des Pools gültig ist.  Informationen zum Abrufen der gültigen Werten für eine Region finden Sie unter [Ermitteln verfügbarer Wartungsfenster](#discover-available-maintenance-windows).

   ```powershell-interactive
    # Select different maintenance window
    $maintenanceConfig = "SQL_EastUS2_DB_1"
    
    Write-host "Changing pool maintenance window to ${maintenanceConfig} ..."
    $pool = Set-AzSqlElasticPool `
      -ResourceGroupName $resourceGroupName `
      -ServerName $serverName `
      -ElasticPoolName $poolName `
      -MaintenanceConfigurationId $maintenanceConfig
    $pool
   ```



## <a name="set-the-maintenance-window-on-an-existing-managed-instance"></a>Festlegen des Wartungsfensters für eine vorhandene verwaltete Instanz

Im folgenden Beispiel wird mit dem Cmdlet [Set-AzSqlInstance](/powershell/module/az.sql/set-azsqlinstance) das Wartungsfenster für eine vorhandene verwaltete Instanz festgelegt. Es ist wichtig sicherzustellen, dass der Wert für `$maintenanceConfig` für die Region der Instanz gültig ist.  Informationen zum Abrufen der gültigen Werten für eine Region finden Sie unter [Ermitteln verfügbarer Wartungsfenster](#discover-available-maintenance-windows).


   ```powershell-interactive
   Set-AzSqlInstance -Name "your_mi_name" `
     -ResourceGroupName "your_resource_group_name" `
     -MaintenanceConfigurationId "/subscriptions/{SubID}/providers/Microsoft.Maintenance/publicMaintenanceConfigurations/SQL_{Region}_{MaintenanceConfigName}" `
     -AsJob
   ```

# <a name="cli"></a>[BEFEHLSZEILENSCHNITTSTELLE (CLI)](#tab/azure-cli)

In den folgenden Beispielen wird gezeigt, wie das Wartungsfenster über die Azure-Befehlszeilenschnittstelle (CLI) konfiguriert wird. Sie können die [Azure CLI installieren](/cli/azure/install-azure-cli) oder Azure Cloud Shell verwenden.

## <a name="set-the-maintenance-window-for-an-existing-database"></a>Festlegen des Wartungsfensters für eine vorhandene Datenbank

Im folgenden Beispiel wird mit dem Befehl [az sql db update](/cli/azure/sql/db#az_sql_db_update) das Wartungsfenster für eine vorhandene Datenbank festgelegt. Der Wert für `--maint-config-id` (oder `-m`) muss für die Region der Datenbank gültig sein. Informationen zum Abrufen gültiger Werte für Ihre Region finden Sie unter [Ermitteln verfügbarer Wartungsfenster](#discover-available-maintenance-windows).

   ```azurecli
    # Select different maintenance window
    maintenanceConfig="SQL_EastUS2_DB_2"

    # Update database
    az sql db update \
      --resource-group $resourceGroupName \
      --server $serverName \
      --name $databaseName \
      --maint-config-id $maintenanceConfig
   ```

## <a name="set-the-maintenance-window-on-an-existing-elastic-pool"></a>Festlegen des Wartungsfensters für einen vorhandenen Pool für elastische Datenbanken

Im folgenden Beispiel wird mit dem Befehl [az sql elastic-pool update](/cli/azure/sql/elastic-pool#az_sql_elastic_pool_update) das Wartungsfenster für einen vorhandenen Pool für elastische Datenbanken festgelegt. Es ist wichtig sicherzustellen, dass der Wert für `maintenanceConfig` für die Region des Pools gültig ist.  Informationen zum Abrufen der gültigen Werten für eine Region finden Sie unter [Ermitteln verfügbarer Wartungsfenster](#discover-available-maintenance-windows).

   ```azurecli
    # Select different maintenance window
    maintenanceConfig="SQL_EastUS2_DB_1"

    # Update pool
    az sql elastic-pool update \
      --resource-group $resourceGroupName \
      --server $serverName \
      --name $poolName \
      --maint-config-id $maintenanceConfig
   ```

## <a name="set-the-maintenance-window-on-an-existing-managed-instance"></a>Festlegen des Wartungsfensters für eine vorhandene verwaltete Instanz

Im folgenden Beispiel wird das Wartungsfenster mit dem Befehl [az sql mi update](/cli/azure/sql/mi#az_sql_mi_update) festgelegt. Das Wartungsfenster wird für die-Instanz festgelegt, sodass für alle Datenbanken in der Instanz der Wartungsfensterzeitplan der Instanz gilt. Für die Option `-MaintenanceConfigurationId` muss der Wert für *MaintenanceConfigName* für die Region der Instanz gültig sein. Informationen zum Abrufen gültiger Werte für Ihre Region finden Sie unter [Ermitteln verfügbarer Wartungsfenster](#discover-available-maintenance-windows).

   ```azurecli
   az sql mi update -g mygroup  -n myinstance -m /subscriptions/{SubID}/providers/Microsoft.Maintenance/publicMaintenanceConfigurations/SQL_{Region}_{MainteanceConfigName}
   ```

-----

## <a name="cleanup-resources"></a>Bereinigen von Ressourcen

Stellen Sie sicher, dass Sie abschließend alle nicht benötigten Ressourcen löschen, um unnötige Kosten zu vermeiden.

# <a name="portal"></a>[Portal](#tab/azure-portal)

1. Navigieren Sie zu der SQL-Datenbank oder dem Pool für elastische Datenbanken, die bzw. den Sie nicht mehr benötigen.
1. Wählen Sie im Menü **Übersicht** die Option zum Löschen der Ressource aus.


# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

   ```powershell-interactive
   # Delete database
   Remove-AzSqlDatabase `
      -ResourceGroupName $resourceGroupName `
      -ServerName $serverName `
      -DatabaseName $databaseName

   # Delete elastic pool
   Remove-AzSqlElasticPool `
      -ResourceGroupName $resourceGroupName `
      -ServerName $serverName `
      -ElasticPoolName $poolName
   ```

# <a name="cli"></a>[BEFEHLSZEILENSCHNITTSTELLE (CLI)](#tab/azure-cli)

   ```azurecli
   az sql db delete \
      --resource-group $resourceGroupName \
      --server $serverName \
      --name $databaseName

   az sql elastic-pool delete \
      --resource-group $resourceGroupName \
      --server $serverName \
      --name $poolName
   ```

-----

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zum Wartungsfenster finden Sie unter [Wartungsfenster (Vorschau)](maintenance-window.md).
- Weitere Informationen finden Sie unter [Wartungsfenster – Häufig gestellte Fragen](maintenance-window-faq.yml).
- Informationen zur Leistungsoptimierung finden Sie unter [Überwachung und Leistungsoptimierung in Azure SQL-Datenbank und Azure SQL Managed Instance](monitor-tune-overview.md).
