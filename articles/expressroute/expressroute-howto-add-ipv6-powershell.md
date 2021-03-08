---
title: 'Azure ExpressRoute: Hinzufügen von IPv6-Unterstützung mithilfe von Azure PowerShell'
description: Hier erfahren Sie, wie Sie IPv6-Unterstützung zum Herstellen einer Verbindung zu Azure-Bereitstellungen mithilfe von Azure PowerShell hinzufügen können.
services: expressroute
author: duongau
ms.service: expressroute
ms.topic: how-to
ms.date: 2/9/2021
ms.author: duau
ms.openlocfilehash: 402714b55d7513e41458503b12c68768d0c6ad5e
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/03/2021
ms.locfileid: "101743499"
---
# <a name="add-ipv6-support-for-private-peering-using-azure-powershell-preview"></a>Hinzufügen von IPv6-Unterstützung für privates Peering mithilfe von Azure PowerShell (Vorschau)

In diesem Artikel wird beschrieben, wie Sie IPv6-Unterstützung zum Herstellen einer Verbindung zu Ihren Ressourcen in Azure über ExpressRoute mithilfe von Azure PowerShell hinzufügen können.

> [!Note]
> Dieses Feature steht zurzeit als Vorschauversion in [Azure-Regionen mit Verfügbarkeitszonen](https://docs.microsoft.com/azure/availability-zones/az-region#azure-regions-with-availability-zones) zur Verfügung. Deshalb kann Ihre ExpressRoute-Verbindung über einen beliebigen Peeringstandort erstellt werden, aber die IPv6-basierten Bereitstellungen, mit denen sie verknüpft ist, müssen sich in einer Region mit Verfügbarkeitszonen befinden.

## <a name="working-with-azure-powershell"></a>Arbeiten mit Azure PowerShell

[!INCLUDE [updated-for-az](../../includes/hybrid-az-ps.md)]

[!INCLUDE [expressroute-cloudshell](../../includes/expressroute-cloudshell-powershell-about.md)]

## <a name="register-for-public-preview"></a>Registrieren für die Public Preview
Bevor Sie IPv6-Unterstützung hinzufügen können, müssen Sie Ihr Abonnement registrieren. Führen Sie zu Ihrer Registrierung folgende Schritte über Azure PowerShell aus:
1.  Melden Sie sich bei Azure an, und wählen Sie das Abonnement aus. Sie müssen diesen Schritt für das Abonnement, das Ihre ExpressRoute-Verbindung enthält, und das Abonnement ausführen, das Ihre Azure-Bereitstellungen enthält (falls diese unterschiedlich sind).

    ```azurepowershell-interactive
    Connect-AzAccount 

    Select-AzSubscription -Subscription "<SubscriptionID or SubscriptionName>"
    ```

2. Fordern Sie die Registrierung Ihres Abonnements für Public Preview mit dem folgenden Befehl an:
    ```azurepowershell-interactive
    Register-AzProviderFeature -FeatureName AllowIpv6PrivatePeering -ProviderNamespace Microsoft.Network
    ```

Ihre Anforderung wird dann vom ExpressRoute-Team innerhalb von 2–3 Werktagen genehmigt.

## <a name="add-ipv6-private-peering-to-your-expressroute-circuit"></a>Hinzufügen von privatem IPv6-Peering zu Ihrer ExpressRoute-Verbindung

1. [Erstellen Sie eine ExpressRoute-Verbindung](https://docs.microsoft.com/azure/expressroute/expressroute-howto-circuit-arm), oder verwenden Sie eine vorhandene Verbindung. Rufen Sie die Verbindung durch Ausführen des Befehls **Get-AzExpressRouteCircuit** ab:

    ```azurepowershell-interactive
    $ckt = Get-AzExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"
    ```

2. Rufen Sie die private Peering-Konfiguration für die Verbindung durch Ausführen von **Get-AzExpressRouteCircuitPeeringConfig** ab:

    ```azurepowershell-interactive
    Get-AzExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -ExpressRouteCircuit $ckt
    ```

3. Fügen Sie Ihrer vorhandenen privaten IPv4-Peering-Konfiguration ein privates IPv6-Peering hinzu. Geben Sie zwei /126 IPv6-Subnetze an, die Sie für Ihre primäre bzw. sekundäre Verknüpfung besitzen. Über jedes dieser Subnetze weisen Sie die erste verwendbare IP-Adresse für Ihren Router zu, da die zweite verwendbare IP-Adresse von Microsoft für den eigenen Router genutzt wird.

> [!Note]
> Die Peer-ASN und VLAN-ID sollten mit denen in Ihrer privaten IPv4-Peering-Konfiguration identisch sein.

    ```azurepowershell-interactive
    Set-AzExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -ExpressRouteCircuit $ckt -PeeringType AzurePrivatePeering -PeerASN 100 -PrimaryPeerAddressPrefix "3FFE:FFFF:0:CD30::/126" -SecondaryPeerAddressPrefix "3FFE:FFFF:0:CD30::4/126" -VlanId 200 -PeerAddressType IPv6

    Set-AzExpressRouteCircuit -ExpressRouteCircuit $ckt
    ```

4. Nachdem die Konfiguration erfolgreich gespeichert wurde, stellen Sie die Verbindung durch Ausführen des Befehls **Get-AzExpressRouteCircuit** erneut her. Die Antwort sollte ähnlich wie im folgenden Beispiel aussehen:

    ```azurepowershell
    Name                             : ExpressRouteARMCircuit
    ResourceGroupName                : ExpressRouteResourceGroup
    Location                         : eastus
    Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
    Etag                             : W/"################################"
    ProvisioningState                : Succeeded
    Sku                              : {
                                         "Name": "Standard_MeteredData",
                                         "Tier": "Standard",
                                         "Family": "MeteredData"
                                       }
    CircuitProvisioningState         : Enabled
    ServiceProviderProvisioningState : Provisioned
    ServiceProviderNotes             :
    ServiceProviderProperties        : {
                                         "ServiceProviderName": "Equinix",
                                         "PeeringLocation": "Washington DC",
                                         "BandwidthInMbps": 50
                                       }
    ExpressRoutePort                 : null
    BandwidthInGbps                  :
    Stag                             : 29
    ServiceKey                       : **************************************
    Peerings                         : [
                                         {
                                           "Name": "AzurePrivatePeering",
                                           "Etag": "W/\"facc8972-995c-4861-a18d-9a82aaa7167e\"",
                                           "Id": "/subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit/peerings/AzurePrivatePeering",
                                           "PeeringType": "AzurePrivatePeering",
                                           "State": "Enabled",
                                           "AzureASN": 12076,
                                           "PeerASN": 100,
                                           "PrimaryPeerAddressPrefix": "192.168.15.16/30",
                                           "SecondaryPeerAddressPrefix": "192.168.15.20/30",
                                           "PrimaryAzurePort": "",
                                           "SecondaryAzurePort": "",
                                           "VlanId": 200,
                                           "ProvisioningState": "Succeeded",
                                           "GatewayManagerEtag": "",
                                           "LastModifiedBy": "Customer",
                                           "Ipv6PeeringConfig": {
                                             "State": "Enabled",
                                             "PrimaryPeerAddressPrefix": "3FFE:FFFF:0:CD30::/126",
                                             "SecondaryPeerAddressPrefix": "3FFE:FFFF:0:CD30::4/126"
                                           },
                                           "Connections": [],
                                           "PeeredConnections": []
                                         },
                                       ]
    Authorizations                   : []
    AllowClassicOperations           : False
    GatewayManagerEtag               :
    ```

## <a name="update-your-connection-to-an-existing-virtual-network"></a>Aktualisieren Ihrer Verbindung zu einem vorhandenen virtuellen Netzwerk

Führen Sie die folgenden Schritte aus, wenn Sie eine Umgebung von Azure-Ressourcen in einer Region mit Verfügbarkeitszonen haben, bei der Sie Ihr privates IPv6-Peering verwenden möchten.

1. Rufen Sie das virtuelle Netzwerk ab, mit dem Ihre ExpressRoute-Verbindung verknüpft ist.

    ```azurepowershell-interactive
    $vnet = Get-AzVirtualNetwork -Name "VirtualNetwork" -ResourceGroupName "ExpressRouteResourceGroup"
    ```

2. Fügen Sie Ihrem virtuellen Netzwerk einen IPv6-Adressraum hinzu.

    ```azurepowershell-interactive
    $vnet.AddressSpace.AddressPrefixes.add("ace:daa:daaa:deaa::/64")
    Set-AzVirtualNetwork -VirtualNetwork $vnet
    ```

3. Fügen Sie den IPv6-Adressraum Ihrem Gatewaysubnetz hinzu. Das IPv6-Gatewaysubnetz sollte /64 oder größer sein.

    ```azurepowershell-interactive
    Set-AzVirtualNetworkSubnetConfig -Name GatewaySubnet -VirtualNetwork $vnet -AddressPrefix "10.0.0.0/26", "ace:daa:daaa:deaa::/64"
    Set-AzVirtualNetwork -VirtualNetwork $vnet
    ```

4. Wenn Sie ein zonenredundantes Gateway haben, führen Sie Folgendes aus, um die IPv6-Konnektivität zu aktivieren. Andernfalls können Sie mithilfe einer zonenredundanten SKU (ErGw1AZ, ErGw2AZ, ErGw3AZ) [das Gateway des virtuellen Netzwerks erstellen](https://docs.microsoft.com/azure/expressroute/expressroute-howto-add-gateway-resource-manager).

    ```azurepowershell-interactive
    $gw = Get-AzVirtualNetworkGateway -Name "GatewayName" -ResourceGroupName "ExpressRouteResourceGroup"
    Set-AzVirtualNetworkGateway -VirtualNetworkGateway $gw
    ```

## <a name="create-a-connection-to-a-new-virtual-network"></a>Herstellen einer Verbindung zu einem neuen virtuellen Netzwerk

Führen Sie die folgenden Schritte aus, wenn Sie vorhaben, eine Verbindung zu neuen Azure-Ressourcen in einer Region mit Verfügbarkeitszonen über Ihr privates IPv6-Peering herzustellen.

1. Erstellen Sie ein virtuelles Netzwerk mit dualem Stapel mit dem IPv4- und dem IPv6-Adressraum. Weitere Informationen finden Sie unter [Erstellen eines virtuellen Netzwerks](https://docs.microsoft.com/azure/virtual-network/quick-create-portal#create-a-virtual-network).

2. [Erstellen Sie das Gatewaysubnetz mit dualem Stapel](https://docs.microsoft.com/azure/expressroute/expressroute-howto-add-gateway-resource-manager#add-a-gateway).

3. [Erstellen Sie das Gateway des virtuellen Netzwerks](https://docs.microsoft.com/azure/expressroute/expressroute-howto-add-gateway-resource-manager#add-a-gateway) mithilfe einer zonenredundanten SKU (ErGw1AZ, ErGw2AZ, ErGw3AZ). Wenn Sie FastPath verwenden möchten, verwenden Sie dafür ErGw3AZ.

4. [Verknüpfen Sie Ihr virtuelles Netzwerks mit Ihrer ExpressRoute-Verbindung](https://docs.microsoft.com/azure/expressroute/expressroute-howto-linkvnet-arm).

## <a name="limitations"></a>Einschränkungen
Obwohl IPv6-Unterstützung für Verbindungen zu Bereitstellungen in Regionen mit Verfügbarkeitszonen zur Verfügung steht, werden die folgenden Anwendungsfälle nicht unterstützt:

* Verbindungen mit Bereitstellungen in Azure über eine ExpressRoute-Gateway-SKU ohne Verfügbarkeitszone
* Verbindungen mit Bereitstellungen in Regionen ohne Verfügbarkeitszone
* Global Reach-Verbindungen zwischen ExpressRoute-Verbindungen

## <a name="next-steps"></a>Nächste Schritte

Informationen zur Problembehandlung bei ExpressRoute-Verbindungen finden Sie in den folgenden Artikeln:

* [Überprüfen der ExpressRoute-Konnektivität](expressroute-troubleshooting-expressroute-overview.md)
* [Beheben von Problemen bei der Netzwerkleistung](expressroute-troubleshooting-network-performance.md)
