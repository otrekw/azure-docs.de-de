---
title: 'Tutorial: Erstellen eines NAT-Gateways – PowerShell'
titlesuffix: Azure Virtual Network NAT
description: Beginnen Sie mit dem Erstellen eines NAT-Gateways mithilfe von Azure PowerShell.
author: asudbring
ms.author: allensu
ms.service: virtual-network
ms.subservice: nat
ms.topic: tutorial
ms.date: 03/09/2021
ms.custom: template-tutorial
ms.openlocfilehash: 884697cee84c05916fe19fb8f9435de86bda291e
ms.sourcegitcommit: d135e9a267fe26fbb5be98d2b5fd4327d355fe97
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2021
ms.locfileid: "102619878"
---
# <a name="tutorial-create-a-nat-gateway-using-azure-powershell"></a>Tutorial: Erstellen eines NAT-Gateways mithilfe von Azure PowerShell

In diesem Tutorial erfahren Sie, wie Sie den Azure Virtual Network NAT-Dienst verwenden. Sie erstellen ein NAT-Gateway, um ausgehende Konnektivität für einen virtuellen Computer in Azure bereitzustellen. 

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen Sie ein virtuelles Netzwerk.
> * Erstellen Sie eine VM.
> * Erstellen Sie ein NAT-Gateway, und ordnen Sie es dem virtuellen Netzwerk zu.
> * Stellen Sie eine Verbindung mit dem virtuellen Computer her, und überprüfen Sie die NAT-IP-Adresse.

## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure-Konto mit einem aktiven Abonnement. Sie können [kostenlos ein Konto erstellen](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- Azure PowerShell (lokal installiert) oder Azure Cloud Shell

Wenn Sie PowerShell lokal installieren und verwenden möchten, müssen Sie für diesen Artikel mindestens Version 5.4.1 des Azure PowerShell-Moduls verwenden. Führen Sie `Get-Module -ListAvailable Az` aus, um die installierte Version zu ermitteln. Wenn Sie ein Upgrade ausführen müssen, finden Sie unter [Installieren des Azure PowerShell-Moduls](/powershell/azure/install-Az-ps) Informationen dazu. Wenn Sie PowerShell lokal ausführen, müssen Sie auch `Connect-AzAccount` ausführen, um eine Verbindung mit Azure herzustellen.

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Erstellen Sie mit [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) eine Ressourcengruppe. Eine Azure-Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen bereitgestellt und verwaltet werden.

Im folgenden Beispiel wird eine Ressourcengruppe mit dem Namen **myResourceGroupNAT** am Standort **eastus2** erstellt:

```azurepowershell-interactive
$rsg = @{
    Name = 'myResourceGroupNAT'
    Location = 'eastus2'
}
New-AzResourceGroup @rsg
```
## <a name="create-the-nat-gateway"></a>Erstellen des NAT-Gateways

In diesem Abschnitt erstellen Sie das NAT-Gateway und unterstützende Ressourcen.

* Für den Zugriff auf das Internet benötigen Sie mindestens eine öffentliche IP-Adresse für das NAT-Gateway. Verwenden Sie [New-AzPublicIpAddress](/powershell/module/az.network/new-azpublicipaddress) zum Erstellen einer öffentlichen IP-Ressource namens **myPublicIP** in **myResourceGroupNAT**. 

* Erstellen Sie mit [New-AzNatGateway](/powershell/module/az.network/new-aznatgateway) ein globales Azure-NAT-Gateway. Als Ergebnis dieses Befehls wird eine Gatewayressource namens **myNATgateway** erstellt, die die öffentliche IP-Adresse **myPublicIP** verwendet. Das Leerlauftimeout wird auf zehn Minuten festgelegt.  

* Erstellen Sie ein virtuelles Netzwerk namens **myVnet** mit einem Subnetz namens **mySubnet** mit [New-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/new-azvirtualnetworksubnetconfig) in **myResourceGroup**, indem Sie [New-AzVirtualNetwork](/powershell/module/az.network/new-azvirtualnetwork) verwenden. Der IP-Adressraum für das virtuelle Netzwerk ist **10.1.0.0/16**. Das Subnetz im virtuellen Netzwerk ist **10.1.0.0/24**.

* Erstellen Sie einen Azure Bastion-Host mit dem Namen **myBastionHost** für den Zugriff auf den virtuellen Computer. Verwenden Sie [New-AzBastion](/powershell/module/az.network/new-azbastion), um den Bastionhost zu erstellen. Erstellen Sie mit [New-AzPublicIpAddress](/powershell/module/az.network/new-azpublicipaddress) eine öffentliche IP-Adresse für den Bastionhost.

```azurepowershell-interactive
## Create public IP address for NAT gateway ##
$ip = @{
    Name = 'myPublicIP'
    ResourceGroupName = 'myResourceGroupNAT'
    Location = 'eastus2'
    Sku = 'Standard'
    AllocationMethod = 'Static'
}
$publicIP = New-AzPublicIpAddress @ip

## Create NAT gateway resource ##
$nat = @{
    ResourceGroupName = 'myResourceGroupNAT'
    Name = 'myNATgateway'
    IdleTimeoutInMinutes = '10'
    Sku = 'Standard'
    Location = 'eastus2'
    PublicIpAddress = $publicIP
}
$natGateway = New-AzNatGateway @nat

## Create subnet config and associate NAT gateway to subnet##
$subnet = @{
    Name = 'mySubnet'
    AddressPrefix = '10.1.0.0/24'
    NatGateway = $natGateway
}
$subnetConfig = New-AzVirtualNetworkSubnetConfig @subnet 

## Create Azure Bastion subnet. ##
$bastsubnet = @{
    Name = 'AzureBastionSubnet' 
    AddressPrefix = '10.1.1.0/24'
}
$bastsubnetConfig = New-AzVirtualNetworkSubnetConfig @bastsubnet

## Create the virtual network ##
$net = @{
    Name = 'myVNet'
    ResourceGroupName = 'myResourceGroupNAT'
    Location = 'eastus2'
    AddressPrefix = '10.1.0.0/16'
    Subnet = $subnetConfig,$bastsubnetConfig
}
$vnet = New-AzVirtualNetwork @net

## Create public IP address for bastion host. ##
$ip = @{
    Name = 'myBastionIP'
    ResourceGroupName = 'myResourceGroupNAT'
    Location = 'eastus2'
    Sku = 'Standard'
    AllocationMethod = 'Static'
}
$publicip = New-AzPublicIpAddress @ip

## Create bastion host ##
$bastion = @{
    ResourceGroupName = 'myResourceGroupNAT'
    Name = 'myBastion'
    PublicIpAddress = $publicip
    VirtualNetwork = $vnet
}
New-AzBastion @bastion -AsJob

```

## <a name="virtual-machine"></a>Virtueller Computer

In diesem Abschnitt erstellen Sie einen virtuellen Computer, um das NAT-Gateway zu testen und die öffentliche IP-Adresse der ausgehenden Verbindung zu überprüfen.

* Erstellen Sie mit [New-AzNetworkInterface](/powershell/module/az.network/new-aznetworkinterface) eine Netzwerkschnittstelle.

* Legen Sie mit [Get-Credential](/powershell/module/microsoft.powershell.security/get-credential) den Benutzernamen und das Kennwort des Administrators des virtuellen Computers fest.

* Erstellen Sie mithilfe der folgenden Befehle den virtuellen Computer:
    * [New-AzVM](/powershell/module/az.compute/new-azvm)
    * [New-AzVMConfig](/powershell/module/az.compute/new-azvmconfig)
    * [Set-AzVMOperatingSystem](/powershell/module/az.compute/set-azvmoperatingsystem)
    * [Set-AzVMSourceImage](/powershell/module/az.compute/set-azvmsourceimage)
    * [Add-AzVMNetworkInterface](/powershell/module/az.compute/add-azvmnetworkinterface)
    
```azurepowershell-interactive
# Set the administrator and password for the VMs. ##
$cred = Get-Credential

## Place the virtual network into a variable. ##
$vnet = Get-AzVirtualNetwork -Name 'myVNet' -ResourceGroupName 'myResourceGroupNAT'

## Create network interface for virtual machine. ##
$nic = @{
    Name = "myNicVM"
    ResourceGroupName = 'myResourceGroupNAT'
    Location = 'eastus2'
    Subnet = $vnet.Subnets[0]
}
$nicVM = New-AzNetworkInterface @nic

## Create a virtual machine configuration for VMs ##
$vmsz = @{
    VMName = "myVM"
    VMSize = 'Standard_DS1_v2'  
}
$vmos = @{
    ComputerName = "myVM"
    Credential = $cred
}
$vmimage = @{
    PublisherName = 'MicrosoftWindowsServer'
    Offer = 'WindowsServer'
    Skus = '2019-Datacenter'
    Version = 'latest'    
}
$vmConfig = New-AzVMConfig @vmsz `
    | Set-AzVMOperatingSystem @vmos -Windows `
    | Set-AzVMSourceImage @vmimage `
    | Add-AzVMNetworkInterface -Id $nicVM.Id

## Create the virtual machine for VMs ##
$vm = @{
    ResourceGroupName = 'myResourceGroupNAT'
    Location = 'eastus2'
    VM = $vmConfig
}
New-AzVM @vm

```

Warten Sie, bis die Erstellung des virtuellen Computers abgeschlossen ist, bevor Sie mit dem nächsten Abschnitt fortfahren.

## <a name="test-nat-gateway"></a>Testen des NAT-Gateways

In diesem Abschnitt testen Sie das NAT-Gateway. Sie ermitteln zunächst die öffentliche IP-Adresse des NAT-Gateways. Anschließend stellen Sie eine Verbindung mit dem virtuellen Testcomputer her und überprüfen die ausgehende Verbindung über das NAT-Gateway.
    
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)

1. Suchen Sie auf dem Bildschirm **Übersicht** die öffentliche IP-Adresse für das NAT-Gateway. Wählen Sie im linken Menü **Alle Dienste** > **Alle Ressourcen** und anschließend **myPublicIP** aus.

2. Notieren Sie sich die öffentliche IP-Adresse:

    :::image type="content" source="./media/tutorial-create-nat-gateway-portal/find-public-ip.png" alt-text="Ermitteln der öffentlichen IP-Adresse des NAT-Gateways" border="true":::

3. Wählen Sie im linken Menü **Alle Dienste** > **Alle Ressourcen** und anschließend in der Ressourcenliste den Eintrag **myVM** (in der Ressourcengruppe **myResourceGroupNAT**) aus.

4. Wählen Sie auf der Seite **Übersicht** die Option **Verbinden** und dann **Bastion** aus.

5. Wählen Sie die blaue Schaltfläche **Bastion verwenden** aus.

6. Geben Sie den Benutzernamen und das Kennwort ein, die Sie bei der VM-Erstellung verwendet haben.

7. Öffnen Sie **Internet Explorer** auf **myTestVM**.

8. Geben Sie in der Adressleiste **https://whatsmyip.com** ein.

9. Vergewissern Sie sich, dass die angezeigte IP-Adresse mit der NAT-Gatewayadresse übereinstimmt, die Sie im vorherigen Schritt notiert haben:

    :::image type="content" source="./media/tutorial-create-nat-gateway-portal/my-ip.png" alt-text="Internet Explorer mit externer ausgehenden IP-Adresse" border="true":::

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Falls Sie diese Anwendung nicht mehr benötigen, löschen Sie die das virtuelle Netzwerk, den virtuellen Computer und das NAT-Gateway wie folgt:

```azurepowershell-interactive
Remove-AzResourceGroup -Name 'myResourceGroupNAT' -Force
```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Azure Virtual Network NAT finden Sie unter folgendem Link:
> [!div class="nextstepaction"]
> [Übersicht über Virtual Network NAT](nat-overview.md)
