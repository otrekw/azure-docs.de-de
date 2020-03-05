---
title: 'Tutorial: Erstellen und Testen eines NAT-Gateways: Azure PowerShell'
titlesuffix: Azure Virtual Network NAT
description: In diesem Tutorial wird veranschaulicht, wie Sie mit Azure PowerShell ein NAT-Gateway erstellen und den NAT-Dienst testen.
services: virtual-network
documentationcenter: na
author: asudbring
manager: KumudD
Customer intent: I want to test a NAT gateway for outbound connectivity for my virtual network.
ms.service: virtual-network
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/18/2020
ms.author: allensu
ms.openlocfilehash: faaf32b08fdf2415b1c60192f917fc2aedc14704
ms.sourcegitcommit: 747a20b40b12755faa0a69f0c373bd79349f39e3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2020
ms.locfileid: "77660987"
---
# <a name="tutorial-create-a-nat-gateway-using-azure-powershell-and-test-the-nat-service"></a>Tutorial: Erstellen eines NAT-Gateways mit Azure PowerShell und Testen des NAT-Diensts

In diesem Tutorial erstellen Sie ein NAT-Gateway, um ausgehende Konnektivität für virtuelle Computer in Azure bereitzustellen. Sie stellen einen virtuellen Quell- und Zielcomputer bereit, um das NAT-Gateway zu testen. Sie testen das NAT-Gateway, indem Sie ausgehende Verbindungen mit einer öffentlichen IP-Adresse herstellen. Diese Verbindungen werden vom virtuellen Quell- zum Zielcomputer hergestellt. In diesem Tutorial werden die Quelle und das Ziel der Einfachheit halber in zwei unterschiedlichen virtuellen Netzwerken jeweils in derselben Ressourcengruppe bereitgestellt.

>[!NOTE] 
>Azure Virtual Network NAT ist zurzeit als öffentliche Vorschauversion erhältlich und nur in bestimmten [Regionen](./nat-overview.md#region-availability) verfügbar. Diese Vorschau wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. Weitere Informationen finden Sie unter [Ergänzende Nutzungsbedingungen für Microsoft Azure-Vorschauversionen](https://azure.microsoft.com/support/legal/preview-supplemental-terms).


[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Sie können dieses Tutorial mithilfe von Azure Cloud Shell absolvieren oder die entsprechenden Befehle lokal ausführen.  Falls Sie Azure Cloud Shell noch nicht verwendet haben, sollten Sie sich [jetzt anmelden](https://shell.azure.com).

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]


## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Erstellen Sie mit [az group create](https://docs.microsoft.com/cli/azure/group) eine Ressourcengruppe. Eine Azure-Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen bereitgestellt und verwaltet werden.

Im folgenden Beispiel wird eine Ressourcengruppe mit dem Namen **myResourceGroupNAT** am Standort **eastus2** erstellt:


```azurepowershell-interactive
$rsg = 'myResourceGroupNAT'
$loc = 'eastus2'

New-AzResourceGroup -Name $rsg -Location $loc
```

## <a name="create-the-nat-gateway"></a>Erstellen des NAT-Gateways

### <a name="create-a-public-ip-address"></a>Erstellen einer öffentlichen IP-Adresse

Für den Zugriff auf das Internet benötigen Sie mindestens eine öffentliche IP-Adresse für das NAT-Gateway. Verwenden Sie [New-AzPublicIpAddress](https://docs.microsoft.com/powershell/module/az.network/new-azpublicipaddress?view=latest) zum Erstellen einer öffentlichen IP-Ressource namens **myPublicIPsource** in **myResourceGroupNAT**. Das Ergebnis dieses Befehls wird zur späteren Verwendung in einer Variablen namens **$publicIPsource** gespeichert.

```azurepowershell-interactive
$rsg = 'myResourceGroupNAT'
$loc = 'eastus2'
$pips = 'myPublicIPsource'
$alm = 'Static'
$sku = 'Standard'

$publicIPsource = 
New-AzPublicIpAddress -Name $pips -ResourceGroupName $rsg -AllocationMethod $alm -Location $loc -Sku $sku
```

### <a name="create-a-public-ip-prefix"></a>Erstellen des Präfixes für öffentliche IP-Adressen

 Verwenden Sie [New-AzPublicIpAddress](https://docs.microsoft.com/powershell/module/az.network/new-azpublicipprefix?view=latest), um eine Präfixressource für öffentliche IP-Adressen mit dem Namen **myPublicIPprefixsource** in **myResourceGroupNAT** zu erstellen.  Das Ergebnis dieses Befehls wird zur späteren Verwendung in einer Variablen namens **$publicIPPrefixsource** gespeichert.

```azurepowershell-interactive
$rsg = 'myResourceGroupNAT'
$loc = 'eastus2'
$prips = 'mypublicIPprefixsource'

$publicIPPrefixsource = 
New-AzPublicIpPrefix -Name $prips -ResourceGroupName $rsg -Location $loc -PrefixLength 31
```

### <a name="create-a-nat-gateway-resource"></a>Erstellen einer NAT-Gatewayressource

In diesem Abschnitt erfahren Sie, wie Sie mit der NAT-Gatewayressource die folgenden Komponenten des NAT-Diensts erstellen und konfigurieren:
  - Öffentlicher IP-Adresspool und Präfix für öffentliche IP-Adressen für ausgehende Datenflüsse, die von der NAT-Gatewayressource übersetzt werden
  - Ändern des Leerlauftimeouts von vier Minuten (Standardwert) in zehn Minuten

Erstellen Sie mit [New-AzNatGateway](https://docs.microsoft.com/powershell/module/az.network/new-aznatgateway) ein globales Azure-NAT-Gateway. Als Ergebnis dieses Befehls wird eine Gatewayressource namens **myNATgateway** erstellt, die die öffentliche IP-Adresse **myPublicIPsource** und das Präfix für öffentliche IP-Adressen **myPublicIPprefixsource** verwendet. Das Leerlauftimeout wird auf zehn Minuten festgelegt.  Das Ergebnis dieses Befehls wird zur späteren Verwendung in einer Variablen namens **$natGateway** gespeichert.

```azurepowershell-interactive
$rsg = 'myResourceGroupNAT'
$loc = 'eastus2'
$sku = 'Standard'
$nnm = 'myNATgateway'

$natGateway = 
New-AzNatGateway -Name $nnm -ResourceGroupName $rsg -PublicIpAddress $publicIPsource -PublicIpPrefix $publicIPPrefixsource -Location $loc -Sku $sku -IdleTimeoutInMinutes 10      
  ```

Das NAT-Gateway ist nun verwendungsbereit, und Sie müssen nur noch konfigurieren, welche Subnetze eines virtuellen Netzwerks das Gateway verwenden sollen.

## <a name="prepare-the-source-for-outbound-traffic"></a>Vorbereiten der Quelle für ausgehenden Datenverkehr

Hier wird die Einrichtung einer vollständigen Testumgebung Schritt für Schritt beschrieben. Sie richten einen Test mit Open-Source-Tools ein, um das NAT-Gateway zu überprüfen. Wir beginnen mit der Quelle, für die das zuvor erstellte NAT-Gateway verwendet wird.

### <a name="configure-virtual-network-for-source"></a>Konfigurieren eines virtuellen Netzwerks für die Quelle

Erstellen Sie das virtuelle Netzwerk, und weisen Sie das Subnetz dem Gateway zu.

Erstellen Sie ein virtuelles Netzwerk namens **myVnetsource** mit einem Subnetz namens **mySubnetsource** mit [New-AzVirtualNetworkSubnetConfig](https://docs.microsoft.com/powershell/module/az.network/new-azvirtualnetworksubnetconfig?view=latest) in **myResourceGroupNAT**, indem Sie [New-AzVirtualNetwork](https://docs.microsoft.com/powershell/module/az.network/new-azvirtualnetwork?view=latest) verwenden. Der IP-Adressraum für das virtuelle Netzwerk ist **192.168.0.0/16**. Das Subnetz im virtuellen Netzwerk ist **192.168.0.0/24**.  Das Ergebnis der Befehle wird zur späteren Verwendung in Variablen mit den Namen **$subnetsource** und **$vnetsource** gespeichert.

```azurepowershell-interactive
$rsg = 'myResourceGroupNAT'
$sbn = 'mySubnetsource'
$spfx = '192.168.0.0/24'
$vnm = 'myVnetsource'
$vpfx = '192.168.0.0/16'
$loc = 'eastus2'

$subnetsource = 
New-AzVirtualNetworkSubnetConfig -Name $sbn -AddressPrefix $spfx -NatGateway $natGateway

$vnetsource = 
New-AzVirtualNetwork -Name $vnm -ResourceGroupName $rsg -Location $loc -AddressPrefix $vpfx -Subnet $subnet
```

Der gesamte ausgehende Datenverkehr an Ziele im Internet verwendet jetzt den NAT-Dienst.  Eine UDR muss nicht konfiguriert werden.

Bevor Sie das NAT-Gateway testen können, müssen Sie einen virtuellen Quellcomputer erstellen.  Sie weisen eine öffentliche IP-Adressressource als öffentliche IP-Adresse auf Instanzebene zu, um den externen Zugriff auf diese VM zu ermöglichen. Diese Adresse wird nur für den Zugriff im Rahmen des Tests verwendet.  Wir veranschaulichen, wie der NAT-Dienst Vorrang vor anderen Ausgangsoptionen erhält.

Zu Übungszwecken können Sie diesen virtuellen Computer auch ohne öffentliche IP-Adresse erstellen und dann einen weiteren virtuellen Computer erstellen, der als Jumpbox ohne öffentliche IP-Adresse verwendet wird.

### <a name="create-public-ip-for-source-vm"></a>Erstellen einer öffentlichen IP-Adresse für den virtuellen Quellcomputer

Sie erstellen eine öffentliche IP-Adresse, die für den Zugriff auf den virtuellen Computer verwendet werden soll.  Verwenden Sie [New-AzPublicIpAddress](https://docs.microsoft.com/powershell/module/az.network/new-azpublicipaddress?view=latest) zum Erstellen einer öffentlichen IP-Ressource namens **myPublicIPVM** in **myResourceGroupNAT**.  Das Ergebnis dieses Befehls wird zur späteren Verwendung in einer Variablen namens **$publicIpsourceVM** gespeichert.

```azurepowershell-interactive
$rsg = 'myResourceGroupNAT'
$loc = 'eastus2'
$sku = 'Standard'
$pipvm = 'myPublicIpsourceVM'
$alm = 'Static'

$publicIpsourceVM = 
New-AzPublicIpAddress -Name $pipvm -ResourceGroupName $rsg -AllocationMethod $alm -Location $loc -sku $sku
```

### <a name="create-an-nsg-and-expose-ssh-endpoint-for-vm"></a>Erstellen einer NSG und Verfügbarmachen des SSH-Endpunkts für den virtuellen Computer

Da öffentliche Standard-IP-Adressen standardmäßig sicher sind, erstellen Sie eine NSG, um eingehenden Zugriff für SSH zuzulassen. Der NAT-Dienst kennt die Richtung des Datenflusses. Diese NSG wird nicht für ausgehenden Datenverkehr verwendet, nachdem das NAT-Gateway in demselben Subnetz konfiguriert wurde. Verwenden Sie [New-AzNetworkSecurityGroup](https://docs.microsoft.com/powershell/module/az.network/new-aznetworksecuritygroup?view=latest), um eine NSG-Ressource mit dem Namen **myNSGsource** zu erstellen. Verwenden Sie [New-AzNetworkSecurityRuleConfig](https://docs.microsoft.com/powershell/module/az.network/new-aznetworksecurityruleconfig?view=latest), um in **myResourceGroupNAT** eine NSG-Regel namens **ssh** für den SSH-Zugriff zu erstellen. Das Ergebnis dieses Befehls wird zur späteren Verwendung in einer Variablen namens **$nsgsource** gespeichert.

```azurepowershell-interactive
$rsg = 'myResourceGroupNAT'
$loc = 'eastus2'
$rnm = 'ssh'
$rdsc = 'SSH access'
$acc = 'Allow'
$prt = 'Tcp'
$dir = 'Inbound'
$nsnm = 'myNSGsource'

$sshrule = 
New-AzNetworkSecurityRuleConfig -Name $rnm -Description $rdsc -Access $acc -Protocol $prt -Direction $dir -Priority 100 -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * -DestinationPortRange 22

$nsgsource = 
New-AzNetworkSecurityGroup -ResourceGroupName $rsg -Name $nsnm -Location $loc -SecurityRules $sshrule 
```

### <a name="create-nic-for-source-vm"></a>Erstellen einer NIC für den virtuellen Quellcomputer

Erstellen Sie mit [New-AzNetworkInterface](https://docs.microsoft.com/powershell/module/az.network/new-aznetworkinterface?view=azps-2.8.0) eine Netzwerkschnittstelle namens **myNicsource**. Mit diesem Befehl wird die öffentliche IP-Adresse der Netzwerksicherheitsgruppe zugeordnet. Das Ergebnis dieses Befehls wird zur späteren Verwendung in einer Variablen namens **$nicsource** gespeichert.

```azurepowershell-interactive
$rsg = 'myResourceGroupNAT'
$loc = 'eastus2'
$nin = 'myNicsource'

$nicsource = 
New-AzNetworkInterface -ResourceGroupName $rsg -Name $nin -NetworkSecurityGroupID $nsgsource.Id -PublicIPAddressID $publicIPVMsource.Id -SubnetID $vnetsource.Subnets[0].Id -Location $loc
```

### <a name="create-a-source-vm"></a>Erstellen eines virtuellen Quellcomputers

#### <a name="create-ssh-key-pair"></a>Erstellen eines SSH-Schlüsselpaars

Für diesen Schnellstart benötigen Sie ein SSH-Schlüsselpaar. Falls Sie bereits über ein SSH-Schlüsselpaar verfügen, können Sie diesen Schritt überspringen.

Verwenden Sie „ssh-keygen“, um ein SSH-Schlüsselpaar zu erstellen.

```azurepowershell-interactive
ssh-keygen -t rsa -b 2048
```
Ausführlichere Informationen zum Erstellen von SSH-Schlüsselpaaren, u.a. zur Verwendung von PuTTy, finden Sie unter [Verwenden von SSH-Schlüsseln mit Windows in Azure](https://docs.microsoft.com/azure/virtual-machines/linux/ssh-from-windows).

Wenn Sie das SSH-Schlüsselpaar mit Cloud Shell erstellen, wird es in einem Containerimage gespeichert. Dieses [Speicherkonto wird automatisch erstellt](https://docs.microsoft.com/azure/cloud-shell/persisting-shell-storage). Löschen Sie das Speicherkonto oder die darin enthaltene Dateifreigabe nicht, bis Sie Ihre Schlüssel abgerufen haben.

#### <a name="create-vm-configuration"></a>Erstellen der VM-Konfiguration

Wenn Sie einen virtuellen Computer in PowerShell erstellen möchten, erstellen Sie eine Konfiguration mit Einstellungen wie dem zu verwendenden Image, der Größe und den Authentifizierungsoptionen. Diese Konfiguration wird dann verwendet, um den virtuellen Computer zu erstellen.

Definieren Sie die SSH-Anmeldeinformationen, die Betriebssysteminformationen und die VM-Größe. In diesem Beispiel befindet sich der SSH-Schlüssel in „~/.ssh/id_rsa.pub“.

```azurepowershell-interactive
# Define a credential object

$securePassword = 
ConvertTo-SecureString ' ' -AsPlainText -Force
$cred = 
New-Object System.Management.Automation.PSCredential ("azureuser", $securePassword)

# Create a virtual machine configuration
$vmn = 'myVMsource'
$vms = 'Standard_D1'
$pub = 'Canonical'
$off = 'UbuntuServer'
$skus = '18.04-LTS'
$ver = 'latest'

$vmConfigsource = 
New-AzVMConfig -VMName $vmn -VMSize $vms

Set-AzVMOperatingSystem -VM $vmConfigsource -Linux -ComputerName $vmn -Credential $cred -DisablePasswordAuthentication

Set-AzVMSourceImage -VM $vmConfigsource -PublisherName $pub -Offer $off -Skus $skus -Version $ver

Add-AzVMNetworkInterface -VM $vmConfigsource -Id $nicsource.Id

# Configure the SSH key

$sshPublicKey = cat ~/.ssh/id_rsa.pub

Add-AzVMSshPublicKey -VM $vmConfigsource -KeyData $sshPublicKey -Path "/home/azureuser/.ssh/authorized_keys"

```
Kombinieren Sie die Konfigurationsdefinitionen, um mit [New-AzVM](/powershell/module/az.compute/new-azvm?view=azps-2.8.0) einen virtuellen Computer namens **myVMsource** in **myResourceGroupNAT** zu erstellen.

```azurepowershell-interactive
$rsg = 'myResourceGroupNAT'
$loc = 'eastus2'

New-AzVM -ResourceGroupName $rsg -Location $loc -VM $vmConfigsource
```

Die Rückgabe des Befehls erfolgt zwar sofort, aber es kann einige Minuten dauern, bis die VM bereitgestellt wird.

## <a name="prepare-destination-for-outbound-traffic"></a>Vorbereiten des Ziels für ausgehenden Datenverkehr

Wir erstellen jetzt ein Ziel für den ausgehenden Datenverkehr, der vom NAT-Dienst übersetzt wird, damit Sie einen Test durchführen können.

### <a name="configure-virtual-network-for-destination"></a>Konfigurieren des virtuellen Netzwerks für das Ziel

Wir müssen ein virtuelles Netzwerk erstellen, in dem der virtuelle Zielcomputer angeordnet werden kann.  Bei diesen Befehlen handelt es sich um die gleichen Schritte wie für den virtuellen Quellcomputer. Es wurden geringfügige Änderungen hinzugefügt, um den Zielendpunkt verfügbar zu machen.

Erstellen Sie ein virtuelles Netzwerk namens **myVnetdestination** mit einem Subnetz namens **mySubnetdestination** mit [New-AzVirtualNetworkSubnetConfig](https://docs.microsoft.com/powershell/module/az.network/new-azvirtualnetworksubnetconfig?view=latest) in **myResourceGroupNAT**, indem Sie [New-AzVirtualNetwork](https://docs.microsoft.com/powershell/module/az.network/new-azvirtualnetwork?view=latest) verwenden. Der IP-Adressraum für das virtuelle Netzwerk ist **192.168.0.0/16**. Das Subnetz im virtuellen Netzwerk ist **192.168.0.0/24**.  Das Ergebnis der Befehle wird zur späteren Verwendung in Variablen mit den Namen **$subnetdestination** und **$vnetdestination** gespeichert.

```azurepowershell-interactive
$rsg = 'myResourceGroupNAT'
$loc = 'eastus2'
$sbdn = 'mySubnetdestination'
$spfx = '192.168.0.0/24'
$vdn = 'myVnetdestination'
$vpfx = '192.168.0.0/16'

$subnetdestination = 
New-AzVirtualNetworkSubnetConfig -Name $sbdn -AddressPrefix $spfx

$vnetdestination = 
New-AzVirtualNetwork -Name $vdn -ResourceGroupName $rsg -Location $loc -AddressPrefix $vpfx -Subnet $subnetdestination
```

### <a name="create-public-ip-for-destination-vm"></a>Erstellen einer öffentlichen IP-Adresse für den virtuellen Zielcomputer

Sie erstellen eine öffentliche IP-Adresse, die für den Zugriff auf den virtuellen Quellcomputer verwendet werden soll.  Verwenden Sie [New-AzPublicIpAddress](https://docs.microsoft.com/powershell/module/az.network/new-azpublicipaddress?view=latest) zum Erstellen einer öffentlichen IP-Ressource namens **myPublicIPdestinationVM** in **myResourceGroupNAT**.  Das Ergebnis dieses Befehls wird zur späteren Verwendung in einer Variablen namens **$publicIpdestinationVM** gespeichert.

```azurepowershell-interactive
$rsg = 'myResourceGroupNAT'
$loc = 'eastus2'
$sku = 'Standard'
$all = 'Static'
$pipd = 'myPublicIPdestinationVM'

$publicIpdestinationVM = 
New-AzPublicIpAddress -Name $pipd -ResourceGroupName $rsg -AllocationMethod $all -Location $loc -Sku $sku
```

### <a name="create-an-nsg-and-expose-ssh-and-http-endpoint-for-vm"></a>Erstellen einer NSG und Verfügbarmachen des SSH- und HTTP-Endpunkts für den virtuellen Computer

Da öffentliche Standard-IP-Adressen standardmäßig sicher sind, erstellen Sie eine NSG, um eingehenden Zugriff für SSH zuzulassen. Verwenden Sie [New-AzNetworkSecurityGroup](https://docs.microsoft.com/powershell/module/az.network/new-aznetworksecuritygroup?view=latest), um eine NSG-Ressource mit dem Namen **myNSGdestination** zu erstellen. Verwenden Sie [New-AzNetworkSecurityRuleConfig](https://docs.microsoft.com/powershell/module/az.network/new-aznetworksecurityruleconfig?view=latest), um eine NSG-Regel namens **ssh** für den SSH-Zugriff zu erstellen.  Verwenden Sie [New-AzNetworkSecurityRuleConfig](https://docs.microsoft.com/powershell/module/az.network/new-aznetworksecurityruleconfig?view=latest), um eine NSG-Regel namens **http** für den HTTP-Zugriff zu erstellen. Beide Regeln werden in **myResourceGroupNAT** erstellt. Das Ergebnis dieses Befehls wird zur späteren Verwendung in einer Variablen namens **$nsgdestination** gespeichert.

```azurepowershell-interactive
$rsg = 'myResourceGroupNAT'
$loc = 'eastus2'
$snm = 'ssh'
$sdsc = 'SSH access'
$acc = 'Allow'
$prt = 'Tcp'
$dir = 'Inbound'
$hnm = 'http'
$hdsc = 'HTTP access'
$nsnm = 'myNSGdestination'

$sshrule = 
New-AzNetworkSecurityRuleConfig -Name $snm -Description $sdsc -Access $acc -Protocol $prt -Direction $dir -Priority 100 -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * -DestinationPortRange 22

$httprule = 
New-AzNetworkSecurityRuleConfig -Name $hnm -Description $hdsc -Access $acc -Protocol $prt -Direction $dir -Priority 101 -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * -DestinationPortRange 80

$nsgdestination = 
New-AzNetworkSecurityGroup -ResourceGroupName $rsg -Name $nsnm -Location $loc -SecurityRules $sshrule,$httprule
```

### <a name="create-nic-for-destination-vm"></a>Erstellen einer NIC für den virtuellen Zielcomputer

Erstellen Sie mit [New-AzNetworkInterface](https://docs.microsoft.com/powershell/module/az.network/new-aznetworkinterface?view=azps-2.8.0) eine Netzwerkschnittstelle namens **myNicdestination**. Mit diesem Befehl wird die öffentliche IP-Adresse der Netzwerksicherheitsgruppe zugeordnet. Das Ergebnis dieses Befehls wird zur späteren Verwendung in einer Variablen namens **$nicdestination** gespeichert.

```azurepowershell-interactive
$rsg = 'myResourceGroupNAT'
$loc = 'eastus2'
$nnm = 'myNicdestination'

$nicdestination = 
New-AzNetworkInterface -ResourceGroupName $rsg -Name $nnm -NetworkSecurityGroupID $nsgdestination.Id -PublicIPAddressID $publicIPdestinationVM.Id -SubnetID $vnetdestination.Subnets[0].Id -Location $loc
```

### <a name="create-a-destination-vm"></a>Erstellen eines virtuellen Zielcomputers

#### <a name="create-vm-configuration"></a>Erstellen der VM-Konfiguration

Wenn Sie einen virtuellen Computer in PowerShell erstellen möchten, erstellen Sie eine Konfiguration mit Einstellungen wie dem zu verwendenden Image, der Größe und den Authentifizierungsoptionen. Diese Konfiguration wird dann verwendet, um den virtuellen Computer zu erstellen.

Definieren Sie die SSH-Anmeldeinformationen, die Betriebssysteminformationen und die VM-Größe. In diesem Beispiel befindet sich der SSH-Schlüssel in „~/.ssh/id_rsa.pub“.

```azurepowershell-interactive
# Define a credential object

$securePassword = 
ConvertTo-SecureString ' ' -AsPlainText -Force
$cred = 
New-Object System.Management.Automation.PSCredential ("azureuser", $securePassword)

# Create a virtual machine configuration

$rsg = 'myResourceGroupNAT'
$loc = 'eastus2'
$vmd = 'myVMdestination'
$vms = 'Standard_D1'
$pub = 'Canonical'
$off = 'UbuntuServer'
$skus = '18.04-LTS'
$ver = 'latest'

$vmConfigdestination = New-AzVMConfig -VMName $vmd -VMSize $vms

Set-AzVMOperatingSystem -VM $vmConfigdestination -Linux -ComputerName $vmd -Credential $cred -DisablePasswordAuthentication

Set-AzVMSourceImage -VM $vmConfigdestination -PublisherName $pub -Offer $off -Skus $skus -Version $ver

Add-AzVMNetworkInterface -VM $vmConfigdestination -Id $nicdestination.Id

# Configure the SSH key

$sshPublicKey = cat ~/.ssh/id_rsa.pub

Add-AzVMSshPublicKey -VM $vmConfigdestination -KeyData $sshPublicKey -Path "/home/azureuser/.ssh/authorized_keys"

```
Kombinieren Sie die Konfigurationsdefinitionen, um mit [New-AzVM](/powershell/module/az.compute/new-azvm?view=azps-2.8.0) einen virtuellen Computer namens **myVMdestination** in **myResourceGroupNAT** zu erstellen.

```azurepowershell-interactive
$rsg = 'myResourceGroupNAT'
$loc = 'eastus2'

New-AzVM -ResourceGroupName $rsg -Location $loc -VM $vmConfigdestination
```

Die Rückgabe des Befehls erfolgt zwar sofort, aber es kann einige Minuten dauern, bis die VM bereitgestellt wird.

## <a name="prepare-a-web-server-and-test-payload-on-destination-vm"></a>Vorbereiten eines Webservers und Testen der Nutzlast auf dem virtuellen Zielcomputer

Als Erstes müssen Sie die IP-Adresse des virtuellen Zielcomputers ermitteln.  Verwenden Sie [Get-AzPublicIpAddress](https://docs.microsoft.com/powershell/module/az.network/get-azpublicipaddress?view=latest) zum Abrufen der öffentlichen IP-Adresse des virtuellen Computers. 

```azurepowershell-interactive
$rsg = 'myResourceGroupNAT'
$pipn = 'myPublicIPdestinationVM'
  
Get-AzPublicIpAddress -ResourceGroupName $rsg -Name $pipn | select IpAddress
``` 

>[!IMPORTANT]
>Kopieren Sie die öffentliche IP-Adresse, und fügen Sie sie in einen Editor ein, damit Sie sie in den nachfolgenden Schritten verwenden können. Geben Sie an, dass es sich um den virtuellen Zielcomputer handelt.

### <a name="sign-in-to-destination-vm"></a>Anmelden beim virtuellen Zielcomputer

Die SSH-Anmeldeinformationen sollten in der Cloud Shell-Instanz aus dem vorherigen Vorgang gespeichert werden.  Öffnen Sie [Azure Cloud Shell](https://shell.azure.com) im Browser. Verwenden Sie die im vorherigen Schritt abgerufene IP-Adresse, um eine SSH-Verbindung mit dem virtuellen Computer herzustellen. 

```bash
ssh azureuser@<ip-address-destination>
```

Kopieren Sie die folgenden Befehle, und fügen Sie sie ein, nachdem Sie sich angemeldet haben.  

```bash
sudo apt-get -y update && \
sudo apt-get -y upgrade && \
sudo apt-get -y dist-upgrade && \
sudo apt-get -y autoremove && \
sudo apt-get -y autoclean && \
sudo apt-get -y install nginx && \
sudo ln -sf /dev/null /var/log/nginx/access.log && \
sudo touch /var/www/html/index.html && \
sudo rm /var/www/html/index.nginx-debian.html && \
sudo dd if=/dev/zero of=/var/www/html/100k bs=1024 count=100
```

Mit diesen Befehlen wird der virtuelle Computer aktualisiert, nginx installiert und eine Datei mit 100 KB erstellt. Diese Datei wird mit dem NAT-Dienst vom virtuellen Quellcomputer abgerufen.

Schließen Sie die SSH-Sitzung mit dem virtuellen Zielcomputer.

## <a name="prepare-test-on-source-vm"></a>Vorbereiten des Tests auf dem virtuellen Quellcomputer

Als Erstes müssen Sie die IP-Adresse des virtuellen Quellcomputers ermitteln.  Verwenden Sie [Get-AzPublicIpAddress](https://docs.microsoft.com/powershell/module/az.network/get-azpublicipaddress?view=latest) zum Abrufen der öffentlichen IP-Adresse des virtuellen Computers: 

```azurepowershell-interactive
$rsg = 'myResourceGroupNAT'
$pipn = 'myPublicIPsourceVM'

Get-AzPublicIpAddress -ResourceGroupName $rsg -Name $pipn | select IpAddress
``` 

>[!IMPORTANT]
>Kopieren Sie die öffentliche IP-Adresse, und fügen Sie sie in einen Editor ein, damit Sie sie in den nachfolgenden Schritten verwenden können. Geben Sie an, dass es sich um den virtuellen Quellcomputer handelt.

### <a name="log-into-source-vm"></a>Anmelden beim virtuellen Quellcomputer

Die SSH-Anmeldeinformationen werden wiederum in Cloud Shell gespeichert. Öffnen Sie in Ihrem Browser eine neue Registerkarte für [Azure Cloud Shell](https://shell.azure.com).  Verwenden Sie die im vorherigen Schritt abgerufene IP-Adresse, um eine SSH-Verbindung mit dem virtuellen Computer herzustellen. 

```bash
ssh azureuser@<ip-address-source>
```

Kopieren Sie die folgenden Befehle, und fügen Sie sie ein, um den Test des NAT-Diensts vorzubereiten.

```bash
sudo apt-get -y update && \
sudo apt-get -y upgrade && \
sudo apt-get -y dist-upgrade && \
sudo apt-get -y autoremove && \
sudo apt-get -y autoclean && \
sudo apt-get install -y nload golang && \
echo 'export GOPATH=${HOME}/go' >> .bashrc && \
echo 'export PATH=${PATH}:${GOPATH}/bin' >> .bashrc && \
. ~/.bashrc &&
go get -u github.com/rakyll/hey

```

Mit diesen Befehlen wird Ihr virtueller Computer aktualisiert, go installiert, [hey](https://github.com/rakyll/hey) von GitHub installiert und Ihre Shellumgebung aktualisiert.

Sie können den NAT-Dienst nun testen.

## <a name="validate-nat-service"></a>Überprüfen des NAT-Diensts

Während der Anmeldung beim virtuellen Quellcomputer können Sie **cURL** und **hey** verwenden, um Anforderungen für die IP-Zieladresse zu generieren.

Verwenden Sie cURL, um die 100-KB-Datei abzurufen.  Ersetzen Sie **\<ip-address-destination>** im Beispiel unten durch die IP-Zieladresse, die Sie weiter oben kopiert haben.  Der Parameter **--output** gibt an, dass die abgerufene Datei verworfen wird.

```bash
curl http://<ip-address-destination>/100k --output /dev/null
```

Sie können auch eine Reihe von Anforderungen generieren, indem Sie **hey** verwenden. Ersetzen Sie erneut **\<ip-address-destination>** durch die IP-Zieladresse, die Sie weiter oben kopiert haben.

```bash
hey -n 100 -c 10 -t 30 --disable-keepalive http://<ip-address-destination>/100k
```

Mit diesem Befehl werden 100 Anforderungen (jeweils zehn) mit einem Timeout von 30 Sekunden generiert. Die TCP-Verbindung wird nicht wiederverwendet.  Mit jeder Anforderung werden 100 KB abgerufen.  Am Ende der Ausführung meldet **hey** einige statistische Daten, um anzugeben, welches Ergebnis für den NAT-Dienst erzielt wurde.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn die Ressourcengruppe und alle darin enthaltenen Ressourcen nicht mehr benötigt werden, können Sie sie mit dem Befehl [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup?view=latest) entfernen.

```azurepowershell-interactive 
Remove-AzResourceGroup -Name myResourceGroupNAT
```

## <a name="next-steps"></a>Nächste Schritte
In diesem Tutorial haben Sie ein NAT-Gateway und einen virtuellen Quell- und Zielcomputer erstellt und dann das NAT-Gateway getestet.

Sehen Sie sich die Metriken in Azure Monitor an, um Informationen zum Betrieb Ihres NAT-Diensts anzuzeigen. Diagnostizieren Sie Probleme wie die Ressourcenüberlastung von verfügbaren SNAT-Ports.  Die Ressourcenüberlastung von SNAT-Ports kann durch das Hinzufügen zusätzlicher Ressourcen bzw. Präfixressourcen für öffentliche IP-Adressen leicht behoben werden.

- Informieren Sie sich über [Virtual Network NAT](./nat-overview.md).
- Informieren Sie sich über [NAT-Gatewayressourcen](./nat-gateway-resource.md).
- Schnellstart zur Bereitstellung einer [NAT-Gatewayressource mithilfe der Azure CLI](./quickstart-create-nat-gateway-cli.md)
- Schnellstart zur Bereitstellung einer [NAT-Gatewayressource mithilfe von Azure PowerShell](./quickstart-create-nat-gateway-powershell.md)
- Schnellstart zur Bereitstellung einer [NAT-Gatewayressource mithilfe des Azure-Portals](./quickstart-create-nat-gateway-portal.md)
- [Senden Sie Feedback zur öffentlichen Vorschauversion](https://aka.ms/natfeedback).

> [!div class="nextstepaction"]

