---
title: Konfigurieren der gegenseitigen Authentifizierung in Azure Application Gateway mit PowerShell
description: Es wird beschrieben, wie Sie eine Application Gateway-Instanz so konfigurieren, dass die gegenseitige Authentifizierung mit PowerShell möglich ist.
services: application-gateway
author: mscatyao
ms.service: application-gateway
ms.topic: how-to
ms.date: 04/02/2021
ms.author: caya
ms.openlocfilehash: 95534760c09ca9e1f7f09d6079886216127c7eb0
ms.sourcegitcommit: 3f684a803cd0ccd6f0fb1b87744644a45ace750d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/02/2021
ms.locfileid: "106230859"
---
# <a name="configure-mutual-authentication-with-application-gateway-through-powershell-preview"></a>Konfigurieren der gegenseitigen Authentifizierung in Application Gateway mit PowerShell (Vorschau)
In diesem Artikel wird beschrieben, wie Sie PowerShell verwenden, um die gegenseitige Authentifizierung auf Ihrer Application Gateway-Instanz zu konfigurieren. Gegenseitige Authentifizierung bedeutet Folgendes: Application Gateway authentifiziert den Client, der die Anforderung sendet, indem das Clientzertifikat verwendet wird, das Sie auf die Application Gateway-Instanz hochladen. 

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Für diesen Artikel ist Version 1.0.0 oder höher des Azure PowerShell-Moduls erforderlich. Führen Sie `Get-Module -ListAvailable Az` aus, um die Version zu ermitteln. Wenn Sie ein Upgrade ausführen müssen, finden Sie unter [Installieren des Azure PowerShell-Moduls](/powershell/azure/install-az-ps) Informationen dazu. Wenn Sie PowerShell lokal ausführen, müssen Sie auch `Login-AzAccount` ausführen, um eine Verbindung mit Azure herzustellen.

## <a name="before-you-begin"></a>Voraussetzungen

Zum Konfigurieren der gegenseitigen Authentifizierung über eine Application Gateway-Instanz benötigen Sie ein Clientzertifikat zum Hochladen auf das Gateway. Das Clientzertifikat wird verwendet, um das Zertifikat zu überprüfen, das vom Client für Application Gateway angegeben wird. Sie können zu Testzwecken ein selbstsigniertes Zertifikat verwenden. Dies wird jedoch nicht für Produktionsworkloads empfohlen, da sie schwieriger zu verwalten und nicht vollständig sicher sind.

Weitere Informationen, vor allem zur Art der Clientzertifikate, die Sie hochladen können, finden Sie unter [Übersicht über die gegenseitige Authentifizierung mit Application Gateway](./mutual-authentication-overview.md#certificates-supported-for-mutual-authentication).

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Erstellen Sie unter Ihrem Abonnement zunächst eine neue Ressourcengruppe. 

```azurepowershell
$resourceGroup = New-AzResourceGroup -Name $rgname -Location $location -Tags @{ testtag = "APPGw tag"}
```
## <a name="create-a-virtual-network"></a>Erstellen eines virtuellen Netzwerks

Stellen Sie ein virtuelles Netzwerk bereit, in dem Ihre Application Gateway-Instanz bereitgestellt werden kann.

```azurepowershell
$gwSubnet = New-AzVirtualNetworkSubnetConfig -Name $gwSubnetName -AddressPrefix 10.0.0.0/24
$vnet = New-AzVirtualNetwork -Name $vnetName -ResourceGroupName $rgname -Location $location -AddressPrefix 10.0.0.0/16 -Subnet $gwSubnet
$vnet = Get-AzVirtualNetwork -Name $vnetName -ResourceGroupName $rgname
$gwSubnet = Get-AzVirtualNetworkSubnetConfig -Name $gwSubnetName -VirtualNetwork $vnet
```

## <a name="create-a-public-ip"></a>Erstellen einer öffentlichen IP-Adresse

Erstellen Sie eine öffentliche IP-Adresse für Ihre Application Gateway-Instanz. 

```azurepowershell
$publicip = New-AzPublicIpAddress -ResourceGroupName $rgname -name $publicIpName -location $location -AllocationMethod Static -sku Standard
```

## <a name="create-the-application-gateway-ip-configuration"></a>Erstellen der IP-Konfiguration für Application Gateway

Erstellen Sie die IP-Konfigurationen und den Front-End-Port. 

```azurepowershell
$gipconfig = New-AzApplicationGatewayIPConfiguration -Name $gipconfigname -Subnet $gwSubnet
$fipconfig = New-AzApplicationGatewayFrontendIPConfig -Name $fipconfigName -PublicIPAddress $publicip
$port = New-AzApplicationGatewayFrontendPort -Name $frontendPortName  -Port 443
```

## <a name="configure-frontend-ssl"></a>Konfigurieren von SSL für das Front-End 

Konfigurieren Sie die SSL-Zertifikate für Ihre Application Gateway-Instanz.

```azurepowershell
$password = ConvertTo-SecureString "P@ssw0rd" -AsPlainText -Force
$sslCertPath = $basedir + "/ScenarioTests/Data/ApplicationGatewaySslCert1.pfx"
$sslCert = New-AzApplicationGatewaySslCertificate -Name $sslCertName -CertificateFile $sslCertPath -Password $password
```

## <a name="configure-client-authentication"></a>Konfigurieren der Clientauthentifizierung 

Konfigurieren Sie die Clientauthentifizierung auf Ihrer Application Gateway-Instanz. Weitere Informationen zum Extrahieren von vertrauenswürdigen ZS-Clientzertifikatketten für die Nutzung finden Sie unter [Exportieren einer vertrauenswürdigen ZS-Clientzertifikatkette für die Clientauthentifizierung](./mutual-authentication-certificate-management.md).

> [!IMPORTANT]
> Achten Sie darauf, dass Sie die gesamte ZS-Clientzertifikatkette in einer Datei und nur eine Kette pro Datei hochladen.  

> [!NOTE]
> Wir empfehlen Ihnen die Nutzung von TLS 1.2 mit gegenseitiger Authentifizierung, da TLS 1.2 in Zukunft obligatorisch sein wird. 

```azurepowershell
$clientCertFilePath = $basedir + "/ScenarioTests/Data/TrustedClientCertificate.cer"
$trustedClient01 = New-AzApplicationGatewayTrustedClientCertificate -Name $trustedClientCert01Name -CertificateFile $clientCertFilePath
$sslPolicy = New-AzApplicationGatewaySslPolicy -PolicyType Predefined -PolicyName "AppGwSslPolicy20170401S"
$clientAuthConfig = New-AzApplicationGatewayClientAuthConfiguration -VerifyClientCertIssuerDN
$sslProfile01 = New-AzApplicationGatewaySslProfile -Name $sslProfile01Name -SslPolicy $sslPolicy -ClientAuthConfiguration $clientAuthConfig -TrustedClientCertificates $trustedClient01
$listener = New-AzApplicationGatewayHttpListener -Name $listenerName -Protocol Https -SslCertificate $sslCert -FrontendIPConfiguration $fipconfig -FrontendPort $port -SslProfile $sslProfile01
```

## <a name="configure-the-backend-pool-and-settings"></a>Konfigurieren des Back-End-Pools und der zugehörigen Einstellungen

Richten Sie den Back-End-Pool und die zugehörigen Einstellungen für Ihre Application Gateway-Instanz ein. Richten Sie optional das vertrauenswürdige Stammzertifikat für das Back-End ein, um die End-to-End-SSL-Verschlüsselung zu ermöglichen.  

```azurepowershell
$certFilePath = $basedir + "/ScenarioTests/Data/ApplicationGatewayAuthCert.cer"
$trustedRoot = New-AzApplicationGatewayTrustedRootCertificate -Name $trustedRootCertName -CertificateFile $certFilePath
$pool = New-AzApplicationGatewayBackendAddressPool -Name $poolName -BackendIPAddresses www.microsoft.com, www.bing.com
$poolSetting = New-AzApplicationGatewayBackendHttpSettings -Name $poolSettingName -Port 443 -Protocol Https -CookieBasedAffinity Enabled -PickHostNameFromBackendAddress -TrustedRootCertificate $trustedRoot
```

## <a name="configure-the-rule"></a>Konfigurieren der Regel

Richten Sie eine Regel auf Ihrer Application Gateway-Instanz ein.

```azurepowershell
$rule = New-AzApplicationGatewayRequestRoutingRule -Name $ruleName -RuleType basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool
```

## <a name="set-up-default-ssl-policy-for-future-listeners"></a>Einrichten der SSL-Standardrichtlinie für zukünftige Listener

Sie haben beim Einrichten der gegenseitigen Authentifizierung eine listenerspezifische SSL-Richtlinie eingerichtet. In diesem Schritt können Sie optional die SSL-Standardrichtlinie für Listener festlegen, die Sie in Zukunft erstellen. 

```azurepowershell
$sslPolicyGlobal = New-AzApplicationGatewaySslPolicy -PolicyType Predefined -PolicyName "AppGwSslPolicy20170401"
```

## <a name="create-the-application-gateway"></a>Erstellen des Anwendungsgateways

Stellen Sie Ihre Application Gateway-Instanz nun mit allen oben erstellten Komponenten bereit.

```azurepowershell
$sku = New-AzApplicationGatewaySku -Name Standard_v2 -Tier Standard_v2
$appgw = New-AzApplicationGateway -Name $appgwName -ResourceGroupName $rgname -Zone 1,2 -Location $location -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig -GatewayIpConfigurations $gipconfig -FrontendPorts $port -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku -SslPolicy $sslPolicyGlobal -TrustedRootCertificate $trustedRoot -AutoscaleConfiguration $autoscaleConfig -TrustedClientCertificates $trustedClient01 -SslProfiles $sslProfile01 -SslCertificates $sslCert
```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie die Ressourcengruppe, das Anwendungsgateway und alle zugehörigen Ressourcen nicht mehr benötigen, entfernen Sie sie mit dem Befehl [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup).

```azurepowershell
Remove-AzResourceGroup -Name $rgname
```

## <a name="renew-expired-client-ca-certificates"></a>Verlängern abgelaufener ZS-Clientzertifikate

Falls Ihr ZS-Clientzertifikat abgelaufen ist, können Sie das Zertifikat auf Ihrem Gateway mit den folgenden Schritten aktualisieren: 

1. Melden Sie sich bei Azure an.
    ```azurepowershell
    Connect-AzAccount
    Select-AzSubscription -Subscription "<sub name>"
    ```
2. Rufen Sie Ihre Application Gateway-Konfiguration ab.
    ```azurepowershell
    $gateway = Get-AzApplicationGateway -Name "<gateway-name>" -ResourceGroupName "<resource-group-name>"
    ```
3. Entfernen Sie das vertrauenswürdige Clientzertifikat vom Gateway. 
    ```azurepowershell
    Remove-AzApplicationGatewayTrustedClientCertificate -Name "<name-of-client-certificate>" -ApplicationGateway $gateway
    ``` 
4. Fügen Sie das neue Zertifikat auf dem Gateway hinzu. 
    ```azurepowershell
    Add-AzApplicationGatewayTrustedClientCertificate -ApplicationGateway $gateway -Name "<name-of-new-cert>" -CertificateFile "<path-to-certificate-file>"
    ```
5. Aktualisieren Sie das Gateway mit dem neuen Zertifikat. 
    ```azurepowershell
    Set-AzApplicationGateway -ApplicationGateway $gateway
    ```

## <a name="next-steps"></a>Nächste Schritte

- [Verwalten von Webdatenverkehr mit einem Anwendungsgateway per Azure CLI](./tutorial-manage-web-traffic-cli.md)