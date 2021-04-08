---
title: Back-End-Pool-Verwaltung
titleSuffix: Azure Load Balancer
description: Machen Sie die ersten Schritte, indem Sie erfahren, wie Sie den Back-End-Pool eines Azure Load Balancers konfigurieren und verwalten.
services: load-balancer
author: asudbring
ms.service: load-balancer
ms.topic: how-to
ms.date: 01/28/2021
ms.author: allensu
ms.openlocfilehash: c49a721a4db758965c9cf8d71f5d73b5754b6088
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "104654474"
---
# <a name="backend-pool-management"></a>Back-End-Pool-Verwaltung
Der Back-End-Pool ist eine kritische Komponente des Lastenausgleichs. Der Back-End-Pool definiert die Gruppe der Ressourcen, die den Datenverkehr für eine bestimmte Lastenausgleichsregel verarbeiten.

Es gibt zwei Möglichkeiten, einen Back-End-Pool zu konfigurieren:
* Netzwerkschnittstellenkarte (NIC)
* Kombination aus IP-Adresse und VNET-Ressourcen-ID (Virtuelles Netzwerk)

Konfigurieren Sie Ihren Back-End-Pool anhand der NIC, wenn Sie vorhandene virtuelle Computer und VM-Skalierungsgruppen verwenden. Diese Methode erstellt die direkteste Verknüpfung zwischen ihrer Ressource und dem Back-End-Pool. 

Wenn Sie Ihrem Back-End-Pool vorab einen IP-Adressbereich zuweisen, in dem Sie später virtuelle Computer und VM-Skalierungsgruppen erstellen möchten, konfigurieren Sie den Back-End-Pool anhand der Kombination aus IP-Adresse und VNET-ID.

Sie können IP- und NIC-basierte Back-End-Pools für dasselbe Lastenausgleichsmodul konfigurieren. Es ist aber nicht möglich, nur einen Back-End-Pool zu erstellen, in dem für die Netzwerkkarte bestimmte Back-End-Adressen und zusätzlich IP-Adressen gemischt vorkommen.

Die Konfigurationsabschnitte dieses Artikels konzentrieren sich auf Folgendes:

* Azure PowerShell
* Azure CLI
* REST-API
* Azure-Ressourcen-Manager-Vorlagen 

In diesen Abschnitten erhalten Sie Einblicke in die Strukturierung der Back-End-Pools für die einzelnen Konfigurationsoptionen.

## <a name="configuring-backend-pool-by-nic"></a>Konfigurieren des Back-End-Pools anhand der NIC
Der Back-End-Pool wird als Teil des Lastenausgleichsvorgangs erstellt. Die IP-Konfigurationseigenschaft der NIC wird verwendet, um Back-End-Pool-Mitglieder hinzuzufügen.

Die folgenden Beispiele konzentrieren sich auf die Vorgänge des Erstellens und Auffüllens für den Back-End-Pool, um diesen Workflow und die Beziehung hervorzuheben.

  >[!NOTE] 
  >Es ist wichtig, zu beachten, dass anhand der Netzwerkschnittstelle konfigurierte Back-End-Pools nicht als Teil eines Vorgangs im Back-End-Pool aktualisiert werden können. Jegliches Hinzufügen oder Löschen von Back-End-Ressourcen muss an der Netzwerkschnittstelle der Ressource erfolgen.

### <a name="powershell"></a>PowerShell
Erstellen Sie einen neuen Back-End-Pool:
 
```azurepowershell-interactive
$resourceGroup = "myResourceGroup"
$loadBalancerName = "myLoadBalancer"
$backendPoolName = "myBackendPool"

$backendPool = 
New-AzLoadBalancerBackendAddressPool -ResourceGroupName $resourceGroup -LoadBalancerName $loadBalancerName -BackendAddressPoolName $backendPoolName  
```

Erstellen Sie eine neue Netzwerkschnittstelle, und fügen Sie sie dem Back-End-Pool hinzu:

```azurepowershell-interactive
$resourceGroup = "myResourceGroup"
$loadBalancerName = "myLoadBalancer"
$backendPoolName = "myBackendPool"
$nicname = "myNic"
$location = "eastus"
$vnetname = <your-vnet-name>

$vnet = 
Get-AzVirtualNetwork -Name $vnetname -ResourceGroupName $resourceGroup

$nic = 
New-AzNetworkInterface -ResourceGroupName $resourceGroup -Location $location -Name $nicname -LoadBalancerBackendAddressPool $backendPoolName -Subnet $vnet.Subnets[0]
```

Rufen Sie die Back-End-Poolinformationen für den Lastenausgleich ab, um zu bestätigen, dass diese Netzwerkschnittstelle dem Back-End-Pool hinzugefügt ist:

```azurepowershell-interactive
$resourceGroup = "myResourceGroup"
$loadBalancerName = "myLoadBalancer"
$backendPoolName = "myBackendPool"

$lb =
Get-AzLoadBalancer -ResourceGroupName $res
Get-AzLoadBalancerBackendAddressPool -ResourceGroupName $resourceGroup -LoadBalancerName $loadBalancerName -BackendAddressPoolName $backendPoolName 
```

Erstellen Sie einen neuen virtuellen Computer, und fügen Sie die Netzwerkschnittstelle an, um sie im Back-End-Pool zu platzieren:

```azurepowershell-interactive
# Create a username and password for the virtual machine
$cred = Get-Credential

# Create a virtual machine configuration
$vmname = "myVM1"
$vmsize = "Standard_DS1_v2"
$pubname = "MicrosoftWindowsServer"
$nicname = "myNic"
$off = "WindowsServer"
$sku = "2019-Datacenter"
$resourceGroup = "myResourceGroup"
$location = "eastus"

$nic =
Get-AzNetworkInterface -Name $nicname -ResourceGroupName $resourceGroup

$vmConfig = 
New-AzVMConfig -VMName $vmname -VMSize $vmsize | Set-AzVMOperatingSystem -Windows -ComputerName $vmname -Credential $cred | Set-AzVMSourceImage -PublisherName $pubname -Offer $off -Skus $sku -Version latest | Add-AzVMNetworkInterface -Id $nic.Id
 
# Create a virtual machine using the configuration
$vm1 = New-AzVM -ResourceGroupName $resourceGroup -Zone 1 -Location $location -VM $vmConfig
```

### <a name="cli"></a>Befehlszeilenschnittstelle (CLI)
Erstellen Sie den Back-End-Pool:

```azurecli-interactive
az network lb address-pool create \
--resource-group myResourceGroup \
--lb-name myLB \
--name myBackendPool 
```

Erstellen Sie eine neue Netzwerkschnittstelle, und fügen Sie sie dem Back-End-Pool hinzu:

```azurecli-interactive
az network nic create \
--resource-group myResourceGroup \
--name myNic \
--vnet-name myVnet \
--subnet mySubnet \
--network-security-group myNetworkSecurityGroup \
--lb-name myLB \
--lb-address-pools myBackEndPool
```

Rufen Sie den Back-End-Pool ab, um zu bestätigen, dass die IP-Adresse korrekt hinzugefügt wurde:

```azurecli-interactive
az network lb address-pool show \
--resource-group myResourceGroup \
--lb-name myLb \
--name myBackendPool
```

Erstellen Sie einen neuen virtuellen Computer, und fügen Sie die Netzwerkschnittstelle an, um sie im Back-End-Pool zu platzieren:

```azurecli-interactive
az vm create \
--resource-group myResourceGroup \
--name myVM \
--nics myNic \
--image UbuntuLTS \
--admin-username azureuser \
--generate-ssh-keys
```

### <a name="resource-manager-template"></a>Resource Manager-Vorlage

Befolgen Sie diese [Resource Manager-Schnellstartvorlage](https://github.com/Azure/azure-quickstart-templates/tree/master/101-load-balancer-standard-create/), um einen Lastenausgleich und virtuelle Computer bereitzustellen und die virtuellen Computer über die Netzwerkschnittstelle zum Back-End-Pool hinzuzufügen.

Befolgen Sie diese [Resource Manager-Schnellstartvorlage](https://github.com/Azure/azure-quickstart-templates/tree/master/101-load-balancer-ip-configured-backend-pool), um einen Lastenausgleich und virtuelle Computer bereitzustellen und die virtuellen Computer über die IP-Adresse zum Back-End-Pool hinzuzufügen.


## <a name="configure-backend-pool-by-ip-address-and-virtual-network"></a>Konfigurieren des Back-End-Pools anhand von IP-Adresse und virtuellem Netzwerk
Verwenden Sie in Szenarien mit vorab aufgefüllten Back-End-Pool die IP-Adresse und das virtuelle Netzwerk.

Die gesamte Back-End-Poolverwaltung erfolgt direkt in dem Back-End-Poolobjekt, wie in den folgenden Beispielen gezeigt.

### <a name="powershell"></a>PowerShell
Erstellen Sie einen neuen Back-End-Pool:

```azurepowershell-interactive
$resourceGroup = "myResourceGroup"
$loadBalancerName = "myLoadBalancer"
$backendPoolName = "myBackendPool"
$vnetName = "myVnet"
$location = "eastus"
$nicName = "myNic"

$backendPool = New-AzLoadBalancerBackendAddressPool -ResourceGroupName $resourceGroup -LoadBalancerName $loadBalancerName -Name $backendPoolName  
```

Aktualisieren Sie den Back-End-Pool mit einer neuen IP aus einem vorhandenen virtuellen Netzwerk:
 
```azurepowershell-interactive
$virtualNetwork = 
Get-AzVirtualNetwork -Name $vnetName -ResourceGroupName $resourceGroup 
 
$ip1 = New-AzLoadBalancerBackendAddressConfig -IpAddress "10.0.0.5" -Name "TestVNetRef" -VirtualNetwork $virtualNetwork  
 
$backendPool.LoadBalancerBackendAddresses.Add($ip1) 

Set-AzLoadBalancerBackendAddressPool -InputObject $backendPool
```

Rufen Sie die Back-End-Poolinformationen für den Lastenausgleich ab, um zu bestätigen, dass die Back-End-Adressen dem Back-End-Pool hinzugefügt wurden:

```azurepowershell-interactive
Get-AzLoadBalancerBackendAddressPool -ResourceGroupName $resourceGroup -LoadBalancerName $loadBalancerName -Name $backendPoolName 
```
Erstellen Sie eine Netzwerkschnittstelle, und fügen Sie sie dem Back-End-Pool hinzu. Legen Sie die IP-Adresse auf eine der Back-End-Adressen fest:

```azurepowershell-interactive
$nic = 
New-AzNetworkInterface -ResourceGroupName $resourceGroup -Location $location -Name $nicName -PrivateIpAddress 10.0.0.4 -Subnet $virtualNetwork.Subnets[0]
```

Erstellen Sie eine VM, und fügen Sie die NIC mit einer IP-Adresse im Back-End-Pool an:
```azurepowershell-interactive
# Create a username and password for the virtual machine
$cred = Get-Credential

# Create a virtual machine configuration
$vmname = "myVM1"
$vmsize = "Standard_DS1_v2"
$pubname = "MicrosoftWindowsServer"
$nicname = "myNic"
$off = "WindowsServer"
$sku = "2019-Datacenter"
$resourceGroup = "myResourceGroup"
$location = "eastus"

$nic =
Get-AzNetworkInterface -Name $nicname -ResourceGroupName $resourceGroup

$vmConfig = 
New-AzVMConfig -VMName $vmname -VMSize $vmsize | Set-AzVMOperatingSystem -Windows -ComputerName $vmname -Credential $cred | Set-AzVMSourceImage -PublisherName $pubname -Offer $off -Skus $sku -Version latest | Add-AzVMNetworkInterface -Id $nic.Id

# Create a virtual machine using the configuration
$vm1 = New-AzVM -ResourceGroupName $resourceGroup -Zone 1 -Location $location -VM $vmConfig
```

### <a name="cli"></a>Befehlszeilenschnittstelle (CLI)
Mithilfe der CLI können Sie den Back-End-Pool entweder über Befehlszeilenparameter oder mit einer JSON-Konfigurationsdatei auffüllen. 

Erstellen Sie den Back-End-Pool über Befehlszeilenparametern, und füllen Sie ihn auf:

```azurecli-interactive
az network lb address-pool create \
--resource-group myResourceGroup \
--lb-name myLB \
--name myBackendPool \
--vnet {VNET resource ID} \
--backend-address name=addr1 ip-address=10.0.0.4 \
--backend-address name=addr2 ip-address=10.0.0.5
```

Erstellen Sie den Back-End-Pool mit der JSON-Konfigurationsdatei, und füllen Sie ihn auf:

```azurecli-interactive
az network lb address-pool create \
--resource-group myResourceGroup \
--lb-name myLB \
--name myBackendPool \
--vnet {VNET resource ID} \
--backend-address-config-file @config_file.json
```

JSON-Konfigurationsdatei:
```JSON
        [
          {
            "name": "address1",
            "virtualNetwork": "/subscriptions/{subscriptionId}/resourceGroups/{resource-group-name}/providers/Microsoft.Network/virtualNetworks/{vnet-name}",
            "ipAddress": "10.0.0.4"
          },
          {
            "name": "address2",
            "virtualNetwork": "/subscriptions/{subscriptionId}/resourceGroups/{resource-group-name}/providers/Microsoft.Network/virtualNetworks/{vnet-name}",
            "ipAddress": "10.0.0.5"
          }
        ]
```

Rufen Sie die Back-End-Poolinformationen für den Lastenausgleich ab, um zu bestätigen, dass die Back-End-Adressen dem Back-End-Pool hinzugefügt wurden:

```azurecli-interactive
az network lb address-pool show \
--resource-group myResourceGroup \
--lb-name MyLb \
--name MyBackendPool
```

Erstellen Sie eine Netzwerkschnittstelle, und fügen Sie sie dem Back-End-Pool hinzu. Legen Sie die IP-Adresse auf eine der Back-End-Adressen fest:

```azurecli-interactive
az network nic create \
  --resource-group myResourceGroup \
  --name myNic \
  --vnet-name myVnet \
  --subnet mySubnet \
  --network-security-group myNetworkSecurityGroup \
  --lb-name myLB \
  --private-ip-address 10.0.0.4
```

Erstellen Sie eine VM, und fügen Sie die NIC mit einer IP-Adresse im Back-End-Pool an:

```azurecli-interactive
az vm create \
  --resource-group myResourceGroup \
  --name myVM \
  --nics myNic \
  --image UbuntuLTS \
  --admin-username azureuser \
  --generate-ssh-keys
```
 
### <a name="limitations"></a>Einschränkungen
Für einen per IP-Adresse konfigurierten Back-End-Pool gelten folgende Einschränkungen:
  * Der Back-End-Pool kann nur für Load Balancer Standard-Instanzen verwendet werden.
  * Limit von 100 IP-Adressen im Back-End-Pool
  * Die Back-End-Ressourcen müssen sich im selben virtuellen Netzwerk wie der Lastenausgleich befinden.
  * Ein Lastenausgleich mit IP-basiertem Back-End-Pool kann nicht als Private Link-Dienst fungieren.
  * Diese Funktion wird im Azure-Portal derzeit nicht unterstützt.
  * Derzeit werden keine ACI-Container von dieser Funktion unterstützt.
  * Lastenausgleichsmodule oder Dienste wie Application Gateway können nicht im Back-End-Pool des Lastenausgleichsmoduls platziert werden.
  * NAT-Regel für eingehenden Datenverkehr können nicht per IP-Adresse angegeben werden.

## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel haben Sie die Back-End-Pool-Verwaltung von Azure Load Balancer kennen gelernt und wie Sie einen Back-End-Pool anhand von IP-Adresse und virtuellem Netzwerk konfigurieren.

Weitere Informationen zu [Azure Load Balancer](load-balancer-overview.md).

Informationen zur IP-basierten Back-End-Pool-Verwaltung finden Sie im Artikel zur [REST-API](https://docs.microsoft.com/rest/api/load-balancer/loadbalancerbackendaddresspools/createorupdate).
