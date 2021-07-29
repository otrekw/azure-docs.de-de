---
title: 'Schnellstart: Erstellen einer Instanz von Azure SQL Managed Instance'
description: Erstellen Sie eine Instanz von Azure SQL Managed Instance mithilfe von Azure PowerShell.
services: sql-managed-instance
ms.service: sql-managed-instance
ms.subservice: deployment-configuration
ms.custom: contperf-fy21q1, devx-track-azurecli, devx-track-azurepowershell
ms.devlang: ''
ms.topic: quickstart
author: MashaMSFT
ms.author: mathoma
ms.reviewer: ''
ms.date: 06/25/2021
ms.openlocfilehash: 6697cb9b89d3cb960f2def9ff4dcf478080b7b07
ms.sourcegitcommit: 6bd31ec35ac44d79debfe98a3ef32fb3522e3934
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2021
ms.locfileid: "113220982"
---
# <a name="quickstart-create-a-managed-instance-using-azure-powershell"></a>Schnellstart: Erstellen einer verwalteten Instanz mit Azure PowerShell

In diesem Schnellstart erstellen Sie eine Instanz von [Azure SQL Managed Instance](sql-managed-instance-paas-overview.md) mithilfe von Azure PowerShell. 


## <a name="prerequisite"></a>Voraussetzung

- Ein aktives Azure-Abonnement. Falls Sie nicht über ein Abonnement verfügen, können Sie ein [kostenloses Konto erstellen](https://azure.microsoft.com/free/).
- Die neueste Version von [Azure PowerShell](/powershell/azure/install-az-ps) 

## <a name="set-variables"></a>Festlegen von Variablen

Zum Erstellen einer verwalteten SQL-Instanz müssen mehrere Ressourcen in Azure erstellt werden. Daher werden zur Vereinfachung der Benutzererfahrung bei den Azure PowerShell-Befehlen Variablen verwendet. Definieren Sie die Variablen, und führen Sie dann die Cmdlets in jedem Abschnitt innerhalb derselben PowerShell-Sitzung aus. 

```azurepowershell-interactive
$NSnetworkModels = "Microsoft.Azure.Commands.Network.Models"
$NScollections = "System.Collections.Generic"
# The SubscriptionId in which to create these objects
$SubscriptionId = ''
# Set the resource group name and location for your managed instance
$resourceGroupName = "myResourceGroup-$(Get-Random)"
$location = "eastus2"
# Set the networking values for your managed instance
$vNetName = "myVnet-$(Get-Random)"
$vNetAddressPrefix = "10.0.0.0/16"
$miSubnetName = "myMISubnet-$(Get-Random)"
$miSubnetAddressPrefix = "10.0.0.0/24"
#Set the managed instance name for the new managed instance
$instanceName = "myMIName-$(Get-Random)"
# Set the admin login and password for your managed instance
$miAdminSqlLogin = "SqlAdmin"
$miAdminSqlPassword = "ChangeYourAdminPassword1"
# Set the managed instance service tier, compute level, and license mode
$edition = "General Purpose"
$vCores = 4
$maxStorage = 128
$computeGeneration = "Gen5"
$license = "LicenseIncluded" #"BasePrice" or LicenseIncluded if you have don't have SQL Server licence that can be used for AHB discount
```

## <a name="create-resource-group"></a>Ressourcengruppe erstellen 

Stellen Sie zunächst eine Verbindung mit Azure her, legen Sie Ihren Abonnementkontext fest, und erstellen Sie Ihre Ressourcengruppe. 

Führen Sie dazu das folgende PowerShell-Skript aus: 

```azurepowershell-interactive

## Connect to Azure
Connect-AzAccount

# Set subscription context
Set-AzContext -SubscriptionId $SubscriptionId 

# Create a resource group
$resourceGroup = New-AzResourceGroup -Name $resourceGroupName -Location $location -Tag @{Owner="SQLDB-Samples"}
```

## <a name="configure-networking"></a>Konfigurieren der Netzwerkeinstellungen

Nachdem Ihre Ressourcengruppe erstellt wurde, konfigurieren Sie die Netzwerkressourcen, wie z. B. das virtuelle Netzwerk, Subnetze, die Netzwerksicherheitsgruppe und die Routingtabelle. In diesem Beispiel wird die Verwendung des Skripts für das **Delegatsubnetz für die Bereitstellung von Managed Instance** veranschaulicht, das auf GitHub als Datei [delegate-subnet.ps1](https://github.com/microsoft/sql-server-samples/tree/master/samples/manage/azure-sql-db-managed-instance/delegate-subnet) verfügbar ist.

Führen Sie dazu das folgende PowerShell-Skript aus: 

```azurepowershell-interactive

# Configure virtual network, subnets, network security group, and routing table
$virtualNetwork = New-AzVirtualNetwork `
                      -ResourceGroupName $resourceGroupName `
                      -Location $location `
                      -Name $vNetName `
                      -AddressPrefix $vNetAddressPrefix

                  Add-AzVirtualNetworkSubnetConfig `
                      -Name $miSubnetName `
                      -VirtualNetwork $virtualNetwork `
                      -AddressPrefix $miSubnetAddressPrefix |
                  Set-AzVirtualNetwork
                  
$scriptUrlBase = 'https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/manage/azure-sql-db-managed-instance/delegate-subnet'

$parameters = @{
    subscriptionId = $SubscriptionId
    resourceGroupName = $resourceGroupName
    virtualNetworkName = $vNetName
    subnetName = $miSubnetName
    }

Invoke-Command -ScriptBlock ([Scriptblock]::Create((iwr ($scriptUrlBase+'/delegateSubnet.ps1?t='+ [DateTime]::Now.Ticks)).Content)) -ArgumentList $parameters

$virtualNetwork = Get-AzVirtualNetwork -Name $vNetName -ResourceGroupName $resourceGroupName
$miSubnet = Get-AzVirtualNetworkSubnetConfig -Name $miSubnetName -VirtualNetwork $virtualNetwork
$miSubnetConfigId = $miSubnet.Id
```

## <a name="create-managed-instance"></a>Erstellen einer verwalteten Instanz 

Für mehr Sicherheit erstellen Sie ein komplexes und zufälliges Kennwort für die Anmeldeinformationen Ihrer Instanz von SQL Managed Instance: 

```azurepowershell-interactive
# Create credentials
$secpassword = ConvertTo-SecureString $miAdminSqlPassword -AsPlainText -Force
$credential = New-Object System.Management.Automation.PSCredential ($miAdminSqlLogin, $secpassword)
```

Erstellen Sie dann Ihre Instanz von SQL Managed Instance: 

```azurepowershell-interactive
# Create managed instance
New-AzSqlInstance -Name $instanceName `
                      -ResourceGroupName $resourceGroupName -Location $location -SubnetId $miSubnetConfigId `
                      -AdministratorCredential $credential `
                      -StorageSizeInGB $maxStorage -VCore $vCores -Edition $edition `
                      -ComputeGeneration $computeGeneration -LicenseType $license
```

Dieser Vorgang dauert möglicherweise etwas länger. Weitere Informationen finden Sie unter [Verwaltungsvorgänge](management-operations-overview.md).


## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Behalten Sie die Ressourcengruppe und die verwaltete Instanz bei, um mit den nächsten Schritten fortzufahren, in denen Sie erfahren, wie Sie mithilfe einer Client-VM eine Verbindung mit Ihrer Instanz von SQL Managed Instance herstellen. 

Wenn Sie die Verwendung dieser Ressourcen beendet haben, können Sie die erstellte Ressourcengruppe löschen. Hierbei werden auch der Server und die darin enthaltene Einzeldatenbank gelöscht.

```azurepowershell-interactive
# Clean up deployment 
Remove-AzResourceGroup -ResourceGroupName $resourceGroupName
```


## <a name="next-steps"></a>Nächste Schritte

Nachdem Ihre Instanz von SQL Managed Instance erstellt wurde, stellen Sie eine Client-VM bereit, um eine Verbindung mit Ihrer verwalteten SQL-Instanz herzustellen und eine Beispieldatenbank wiederherzustellen. 

> [!div class="nextstepaction"]
> [Erstellen einer Client-VM](connect-vm-instance-configure.md)
> [Wiederherstellen einer Datenbank](restore-sample-database-quickstart.md)


