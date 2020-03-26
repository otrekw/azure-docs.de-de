---
title: 'Schnellstart: Weiterleiten von Webdatenverkehr mit PowerShell'
titleSuffix: Azure Application Gateway
description: Es wird beschrieben, wie Sie die Azure PowerShell zum Erstellen einer Azure Application Gateway-Instanz verwenden, mit der Webdatenverkehr an virtuelle Computer in einem Back-End-Pool geleitet wird.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: quickstart
ms.date: 03/05/2020
ms.author: victorh
ms.custom: mvc
ms.openlocfilehash: abb38dfc342c8ff692ed1a3a05376b5dcefe8a3d
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/26/2020
ms.locfileid: "78399560"
---
# <a name="quickstart-direct-web-traffic-with-azure-application-gateway-using-azure-powershell"></a>Schnellstart: Weiterleiten von Webdatenverkehr per Azure Application Gateway mithilfe von Azure PowerShell

In dieser Schnellstartanleitung verwenden Sie Azure PowerShell, um ein Anwendungsgateway zu erstellen. Anschließend testen Sie es, um sicherzustellen, dass es ordnungsgemäß funktioniert. 

Das Anwendungsgateway leitet den Webdatenverkehr Ihrer Anwendungen an bestimmte Ressourcen in einem Back-End-Pool weiter. Sie weisen den Ports Listener zu, erstellen Regeln und fügen Ressourcen zu einem Back-End-Pool hinzu. Der Einfachheit halber wird in diesem Artikel ein einfaches Setup mit einer öffentlichen Front-End-IP-Adresse, einem grundlegenden Listener zum Hosten einer einzelnen Website auf diesem Anwendungsgateway, einer Routingregel für grundlegende Anforderungen und zwei virtuellen Computern im Back-End-Pool verwendet.

Für diese Schnellstartanleitung kann auch die [Azure-Befehlszeilenschnittstelle](quick-create-cli.md) oder das [Azure-Portal](quick-create-portal.md) verwendet werden.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure-Konto mit einem aktiven Abonnement. Sie können [kostenlos ein Konto erstellen](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- [Azure PowerShell 1.0.0 oder höher](/powershell/azure/install-az-ps) (bei lokaler Ausführung von Azure PowerShell)

## <a name="connect-to-azure"></a>Herstellen einer Verbindung mit Azure

Führen Sie `Connect-AzAccount` aus, um eine Verbindung mit Azure herzustellen.

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

In Azure ordnen Sie verwandte Ressourcen einer Ressourcengruppe zu. Sie können entweder eine vorhandene Ressourcengruppe verwenden oder eine neue erstellen.

Verwenden Sie das Cmdlet `New-AzResourceGroup`, um eine neue Ressourcengruppe zu erstellen: 

```azurepowershell-interactive
New-AzResourceGroup -Name myResourceGroupAG -Location eastus
```
## <a name="create-network-resources"></a>Erstellen von Netzwerkressourcen

Für die Kommunikation in Azure zwischen den von Ihnen erstellten Ressourcen ist ein virtuelles Netzwerk erforderlich.  Das Subnetz für das Anwendungsgateway kann nur Anwendungsgateways enthalten. Andere Ressourcen sind nicht zulässig.  Sie können entweder ein neues Subnetz für Application Gateway erstellen oder ein bereits vorhandenes verwenden. In diesem Beispiel erstellen Sie zwei Subnetze: eines für das Anwendungsgateway und eines für die Back-End-Server. Je nach Anwendungsfall können Sie die Front-End-IP-Adresse von Application Gateway als öffentliche oder als private IP-Adresse konfigurieren. In diesem Beispiel verwenden Sie eine öffentliche Front-End-IP-Adresse.

1. Erstellen Sie die Subnetzkonfigurationen mithilfe von `New-AzVirtualNetworkSubnetConfig`.
2. Erstellen Sie das virtuelle Netzwerk mit den Subnetzkonfigurationen mithilfe von `New-AzVirtualNetwork`. 
3. Erstellen Sie die öffentliche IP-Adresse mithilfe von `New-AzPublicIpAddress`. 

```azurepowershell-interactive
$agSubnetConfig = New-AzVirtualNetworkSubnetConfig `
  -Name myAGSubnet `
  -AddressPrefix 10.0.1.0/24
$backendSubnetConfig = New-AzVirtualNetworkSubnetConfig `
  -Name myBackendSubnet `
  -AddressPrefix 10.0.2.0/24
New-AzVirtualNetwork `
  -ResourceGroupName myResourceGroupAG `
  -Location eastus `
  -Name myVNet `
  -AddressPrefix 10.0.0.0/16 `
  -Subnet $agSubnetConfig, $backendSubnetConfig
New-AzPublicIpAddress `
  -ResourceGroupName myResourceGroupAG `
  -Location eastus `
  -Name myAGPublicIPAddress `
  -AllocationMethod Static `
  -Sku Standard
```
## <a name="create-an-application-gateway"></a>Erstellen eines Anwendungsgateways

### <a name="create-the-ip-configurations-and-frontend-port"></a>Erstellen der IP-Konfigurationen und des Front-End-Ports

1. Erstellen Sie mithilfe von `New-AzApplicationGatewayIPConfiguration` die Konfiguration, die das von Ihnen erstellte Subnetz dem Anwendungsgateway zuordnet. 
2. Erstellen Sie mithilfe von `New-AzApplicationGatewayFrontendIPConfig` die Konfiguration, die die zuvor erstellte öffentliche IP-Adresse dem Anwendungsgateway zuordnet. 
3. Weisen Sie mithilfe von `New-AzApplicationGatewayFrontendPort` den Port 80 für den Zugriff auf das Anwendungsgateway zu.

```azurepowershell-interactive
$vnet   = Get-AzVirtualNetwork -ResourceGroupName myResourceGroupAG -Name myVNet
$subnet = Get-AzVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name myAGSubnet
$pip    = Get-AzPublicIPAddress -ResourceGroupName myResourceGroupAG -Name myAGPublicIPAddress 
$gipconfig = New-AzApplicationGatewayIPConfiguration `
  -Name myAGIPConfig `
  -Subnet $subnet
$fipconfig = New-AzApplicationGatewayFrontendIPConfig `
  -Name myAGFrontendIPConfig `
  -PublicIPAddress $pip
$frontendport = New-AzApplicationGatewayFrontendPort `
  -Name myFrontendPort `
  -Port 80
```

### <a name="create-the-backend-pool"></a>Erstellen des Back-End-Pools

1. Erstellen Sie mithilfe von `New-AzApplicationGatewayBackendAddressPool` den Back-End-Pool für das Anwendungsgateway. Der Back-End-Pool ist vorerst leer. Wenn Sie im nächsten Abschnitt die Netzwerkadapter für den Back-End-Server erstellen, fügen Sie sie dem Back-End-Pool hinzu.
2. Konfigurieren Sie mithilfe von `New-AzApplicationGatewayBackendHttpSetting` die Einstellungen für den Back-End-Pool.

```azurepowershell-interactive
$address1 = Get-AzNetworkInterface -ResourceGroupName myResourceGroupAG -Name myNic1
$address2 = Get-AzNetworkInterface -ResourceGroupName myResourceGroupAG -Name myNic2
$backendPool = New-AzApplicationGatewayBackendAddressPool `
  -Name myAGBackendPool
$poolSettings = New-AzApplicationGatewayBackendHttpSetting `
  -Name myPoolSettings `
  -Port 80 `
  -Protocol Http `
  -CookieBasedAffinity Enabled `
  -RequestTimeout 30
```

### <a name="create-the-listener-and-add-a-rule"></a>Erstellen des Listeners und Hinzufügen einer Regel

Azure erfordert einen Listener, damit das Anwendungsgateway Datenverkehr in geeigneter Weise an den Back-End-Pool weiterleiten kann. Azure erfordert auch eine Regel für den Listener, damit bekannt ist, welcher Back-End-Pool für eingehenden Datenverkehr verwendet werden soll. 

1. Erstellen Sie mithilfe von `New-AzApplicationGatewayHttpListener` einen Listener mit der zuvor erstellten Front-End-Konfiguration und dem zuvor erstellten Front-End-Port. 
2. Erstellen Sie mithilfe von `New-AzApplicationGatewayRequestRoutingRule` eine Regel mit dem Namen *rule1*. 

```azurepowershell-interactive
$defaultlistener = New-AzApplicationGatewayHttpListener `
  -Name myAGListener `
  -Protocol Http `
  -FrontendIPConfiguration $fipconfig `
  -FrontendPort $frontendport
$frontendRule = New-AzApplicationGatewayRequestRoutingRule `
  -Name rule1 `
  -RuleType Basic `
  -HttpListener $defaultlistener `
  -BackendAddressPool $backendPool `
  -BackendHttpSettings $poolSettings
```

### <a name="create-the-application-gateway"></a>Erstellen des Anwendungsgateways

Nachdem Sie nun die erforderlichen unterstützenden Ressourcen erstellt haben, erstellen Sie das Anwendungsgateway:

1. Geben Sie mithilfe von `New-AzApplicationGatewaySku` Parameter für das Anwendungsgateway an.
2. Erstellen Sie mithilfe von `New-AzApplicationGateway` das Anwendungsgateway.

```azurepowershell-interactive
$sku = New-AzApplicationGatewaySku `
  -Name Standard_v2 `
  -Tier Standard_v2 `
  -Capacity 2
New-AzApplicationGateway `
  -Name myAppGateway `
  -ResourceGroupName myResourceGroupAG `
  -Location eastus `
  -BackendAddressPools $backendPool `
  -BackendHttpSettingsCollection $poolSettings `
  -FrontendIpConfigurations $fipconfig `
  -GatewayIpConfigurations $gipconfig `
  -FrontendPorts $frontendport `
  -HttpListeners $defaultlistener `
  -RequestRoutingRules $frontendRule `
  -Sku $sku
```

### <a name="backend-servers"></a>Back-End-Server

Erstellen Sie nach der Erstellung der Application Gateway-Instanz die virtuellen Back-End-Computer, auf denen die Websites gehostet werden. Das Back-End kann Netzwerkadapter, VM-Skalierungsgruppen, öffentliche IP-Adressen, interne IP-Adressen, vollqualifizierte Domänennamen (Fully Qualified Domain Names, FQDNs) und Back-Ends mit mehreren Mandanten (beispielsweise Azure App Service) umfassen. In diesem Beispiel erstellen Sie zwei virtuelle Computer, die von Azure als Back-End-Server für das Anwendungsgateway verwendet werden. Sie installieren außerdem IIS auf den virtuellen Computern, um zu überprüfen, ob Azure das Anwendungsgateway erfolgreich erstellt hat.

#### <a name="create-two-virtual-machines"></a>Erstellen von zwei virtuellen Computern

1. Rufen Sie mithilfe von `Get-AzApplicationGatewayBackendAddressPool` die soeben erstellte Application Gateway-Back-End-Poolkonfiguration ab.
2. Erstellen Sie mithilfe von `New-AzNetworkInterface` eine Netzwerkschnittstelle.
3. Erstellen Sie mithilfe von `New-AzVMConfig` eine VM-Konfiguration.
4. Erstellen Sie mithilfe von `New-AzVM` den virtuellen Computer.

Wenn Sie das folgende Codebeispiel zum Erstellen der virtuellen Computer ausführen, werden Sie von Azure zur Eingabe von Anmeldeinformationen aufgefordert. Geben Sie *azureuser* als Benutzername und ein Kennwort ein:
    
```azurepowershell-interactive
$appgw = Get-AzApplicationGateway -ResourceGroupName myResourceGroupAG -Name myAppGateway
$backendPool = Get-AzApplicationGatewayBackendAddressPool -Name myAGBackendPool -ApplicationGateway $appgw
$vnet   = Get-AzVirtualNetwork -ResourceGroupName myResourceGroupAG -Name myVNet
$subnet = Get-AzVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name myBackendSubnet
$cred = Get-Credential
for ($i=1; $i -le 2; $i++)
{
  $nic = New-AzNetworkInterface `
    -Name myNic$i `
    -ResourceGroupName myResourceGroupAG `
    -Location EastUS `
    -Subnet $subnet `
    -ApplicationGatewayBackendAddressPool $backendpool
  $vm = New-AzVMConfig `
    -VMName myVM$i `
    -VMSize Standard_DS2_v2
  Set-AzVMOperatingSystem `
    -VM $vm `
    -Windows `
    -ComputerName myVM$i `
    -Credential $cred
  Set-AzVMSourceImage `
    -VM $vm `
    -PublisherName MicrosoftWindowsServer `
    -Offer WindowsServer `
    -Skus 2016-Datacenter `
    -Version latest
  Add-AzVMNetworkInterface `
    -VM $vm `
    -Id $nic.Id
  Set-AzVMBootDiagnostic `
    -VM $vm `
    -Disable
  New-AzVM -ResourceGroupName myResourceGroupAG -Location EastUS -VM $vm
  Set-AzVMExtension `
    -ResourceGroupName myResourceGroupAG `
    -ExtensionName IIS `
    -VMName myVM$i `
    -Publisher Microsoft.Compute `
    -ExtensionType CustomScriptExtension `
    -TypeHandlerVersion 1.4 `
    -SettingString '{"commandToExecute":"powershell Add-WindowsFeature Web-Server; powershell Add-Content -Path \"C:\\inetpub\\wwwroot\\Default.htm\" -Value $($env:computername)"}' `
    -Location EastUS
}
```

## <a name="test-the-application-gateway"></a>Testen des Anwendungsgateways

IIS ist für die Erstellung des Anwendungsgateways zwar nicht erforderlich, Sie haben die Installation in dieser Schnellstartanleitung aber ausgeführt, um zu überprüfen, ob Azure das Anwendungsgateway erfolgreich erstellt hat. Testen des Anwendungsgateways mit IIS:

1. Führen Sie `Get-AzPublicIPAddress` aus, um die öffentliche IP-Adresse des Anwendungsgateways abzurufen. 
2. Kopieren Sie die öffentliche IP-Adresse, und fügen Sie sie in die Adressleiste des Browsers ein. Wenn Sie den Browser aktualisieren, sollte der Name des virtuellen Computers angezeigt werden. Eine gültige Antwort bestätigt, dass das Anwendungsgateway erfolgreich erstellt wurde und eine Verbindung mit dem Back-End herstellen kann.

```azurepowershell-interactive
Get-AzPublicIPAddress -ResourceGroupName myResourceGroupAG -Name myAGPublicIPAddress
```

![Testen des Anwendungsgateways](./media/quick-create-powershell/application-gateway-iistest.png)


## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie die mit dem Anwendungsgateway erstellten Ressourcen nicht mehr benötigen, löschen Sie die Ressourcengruppe. Wenn Sie die Ressourcengruppe löschen, werden auch das Anwendungsgateway und alle zugehörigen Ressourcen entfernt. 

Rufen Sie zum Löschen der Ressourcengruppe das Cmdlet `Remove-AzResourceGroup` auf:

```azurepowershell-interactive
Remove-AzResourceGroup -Name myResourceGroupAG
```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Verwalten von Webdatenverkehr mit einem Anwendungsgateway per Azure PowerShell](./tutorial-manage-web-traffic-powershell.md)

