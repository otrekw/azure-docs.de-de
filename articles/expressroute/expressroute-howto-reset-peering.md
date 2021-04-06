---
title: 'Azure ExpressRoute: Zurücksetzen des Verbindungspeerings'
description: Erfahren Sie, wie Peerings einer Azure ExpressRoute-Verbindung mithilfe von Azure PowerShell deaktiviert und aktiviert werden.
services: expressroute
author: charwen
ms.service: expressroute
ms.topic: how-to
ms.date: 12/15/2020
ms.author: duau
ms.openlocfilehash: 0bde96ae5f4a9aff6f4a16a4f1544d9b39e5cb66
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "97559572"
---
# <a name="reset-expressroute-circuit-peerings"></a>Zurücksetzen von ExpressRoute-Verbindungspeerings

In diesem Artikel wird beschrieben, wie Peerings einer ExpressRoute-Verbindung mithilfe von PowerShell deaktiviert und aktiviert werden. Wenn Sie ein Peering erstellen, wird dieses standardmäßig aktiviert. Wenn Sie ein Peering deaktivieren, wird die BGP-Sitzung der primären und sekundären Verbindung Ihrer ExpressRoute-Verbindung heruntergefahren. Die Konnektivität des Peerings mit Microsoft geht verloren. Wenn Sie das Peering aktivieren, wird die BGP-Sitzung jeweils in der primären und sekundären Verbindung Ihrer ExpressRoute-Verbindung erneut gestartet. Die Konnektivität mit Microsoft wird für dieses Peering wiederhergestellt. Sie können das Microsoft-Peering und das private Azure-Peering unabhängig voneinander in einer ExpressRoute-Verbindung aktivieren und deaktivieren.

Es gibt zwei Szenarien, in denen das Zurücksetzen Ihrer ExpressRoute-Peerings hilfreich sein kann.
* Sie möchten Ihren Entwurf und die Implementierung der Notfallwiederherstellung testen. Wenn Sie z.B. über zwei ExpressRoute-Verbindungen verfügen, können Sie die Peerings in einer Verbindung deaktivieren und erzwingen, dass Ihr Netzwerkdatenverkehr ein Failover auf die andere Verbindung ausführt.
* Aktivieren Sie die bidirektionale Weiterleitungserkennung (Bidirectional Forwarding Detection, BFD) für das private Azure-Peering oder Microsoft-Peering Ihrer ExpressRoute-Verbindung. BFD ist standardmäßig im privaten Azure-Peering aktiviert, wenn Ihre ExpressRoute-Verbindung nach dem 1. August 2018 erstellt wurde, und im Microsoft-Peering nach dem 10. Januar 2020. Wenn Ihre Verbindung vor dem aufgeführten Datum erstellt wurde, müssen Sie das Peering zurücksetzen, um BFD zu aktivieren. 

### <a name="working-with-azure-powershell"></a>Arbeiten mit Azure PowerShell

[!INCLUDE [updated-for-az](../../includes/hybrid-az-ps.md)]

[!INCLUDE [expressroute-cloudshell](../../includes/expressroute-cloudshell-powershell-about.md)]

## <a name="reset-a-peering"></a>Zurücksetzen eines Peerings

1. Wenn Sie PowerShell lokal ausführen, öffnen Sie die PowerShell-Konsole mit erhöhten Rechten, und stellen Sie eine Verbindung mit Ihrem Konto her. Verwenden Sie das folgende Beispiel, um eine Verbindung herzustellen:

   ```azurepowershell
   Connect-AzAccount
   ```
2. Überprüfen Sie die Abonnements für das Konto, wenn Sie über mehrere Azure-Abonnements verfügen.

   ```azurepowershell-interactive
   Get-AzSubscription
   ```
3. Geben Sie das Abonnement an, das Sie verwenden möchten.

   ```azurepowershell-interactive
   Select-AzSubscription -SubscriptionName "Replace_with_your_subscription_name"
   ```
4. Führen Sie die folgenden Befehle aus, um Ihre ExpressRoute-Verbindung abzurufen.

   ```azurepowershell-interactive
   $ckt = Get-AzExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"
   ```
5. Identifizieren Sie das Peering, das Sie deaktivieren oder aktivieren möchten. *Peerings* ist ein Array. Im folgenden Beispiel handelt es sich bei „Peerings[0]“ um das private Azure-Peering und bei „Peerings[1]“ um das Microsoft-Peering.

   ```azurepowershell-interactive
   Name                             : ExpressRouteARMCircuit
   ResourceGroupName                : ExpressRouteResourceGroup
   Location                         : westus
   Id                               : /subscriptions/########-####-####-####-############/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
   Etag                             : W/"cd011bef-dc79-49eb-b4c6-81fb6ea5d178"
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
                                     "ServiceProviderName": "Coresite",
                                     "PeeringLocation": "Los Angeles",
                                     "BandwidthInMbps": 50
                                   }
   ServiceKey                       : ########-####-####-####-############
   Peerings                         : [
                                     {
                                       "Name": "AzurePrivatePeering",
                                       "Etag": "W/\"cd011bef-dc79-49eb-b4c6-81fb6ea5d178\"",
                                       "Id": "/subscriptions/########-####-####-####-############/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit/peerings/AzurePrivatePeering",
                                       "PeeringType": "AzurePrivatePeering",
                                       "State": "Enabled",
                                       "AzureASN": 12076,
                                       "PeerASN": 123,
                                       "PrimaryPeerAddressPrefix": "10.0.0.0/30",
                                       "SecondaryPeerAddressPrefix": "10.0.0.4/30",
                                       "PrimaryAzurePort": "",
                                       "SecondaryAzurePort": "",
                                       "VlanId": 789,
                                       "MicrosoftPeeringConfig": {
                                         "AdvertisedPublicPrefixes": [],
                                         "AdvertisedCommunities": [],
                                         "AdvertisedPublicPrefixesState": "NotConfigured",
                                         "CustomerASN": 0,
                                         "LegacyMode": 0,
                                         "RoutingRegistryName": "NONE"
                                       },
                                       "ProvisioningState": "Succeeded",
                                       "GatewayManagerEtag": "",
                                       "LastModifiedBy": "Customer",
                                       "Connections": []
                                     },
                                     {
                                       "Name": "MicrosoftPeering",
                                       "Etag": "W/\"cd011bef-dc79-49eb-b4c6-81fb6ea5d178\"",
                                       "Id": "/subscriptions/########-####-####-####-############/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit/peerings/MicrosoftPeering",
                                       "PeeringType": "MicrosoftPeering",
                                       "State": "Enabled",
                                       "AzureASN": 12076,
                                       "PeerASN": 123,
                                       "PrimaryPeerAddressPrefix": "3.0.0.0/30",
                                       "SecondaryPeerAddressPrefix": "3.0.0.4/30",
                                       "PrimaryAzurePort": "",
                                       "SecondaryAzurePort": "",
                                       "VlanId": 345,
                                       "MicrosoftPeeringConfig": {
                                         "AdvertisedPublicPrefixes": [
                                           "3.0.0.3/32"
                                         ],
                                         "AdvertisedCommunities": [],
                                         "AdvertisedPublicPrefixesState": "ValidationNeeded",
                                         "CustomerASN": 0,
                                         "LegacyMode": 0,
                                         "RoutingRegistryName": "NONE"
                                       },
                                       "ProvisioningState": "Succeeded",
                                       "GatewayManagerEtag": "",
                                       "LastModifiedBy": "Customer",
                                       "Connections": []
                                     }
                                   ]
   Authorizations                   : []
   AllowClassicOperations           : False
   GatewayManagerEtag               :
   ```
6. Führen Sie die folgenden Befehle aus, um den Peeringzustand zu ändern.

   ```azurepowershell-interactive
   $ckt.Peerings[0].State = "Disabled"
   Set-AzExpressRouteCircuit -ExpressRouteCircuit $ckt
   ```
   Das Peering muss sich in dem von Ihnen festgelegten Zustand befinden. 

## <a name="next-steps"></a>Nächste Schritte
Wenn Sie Hilfe beim Behandeln eines ExpressRoute-Problems benötigen, sehen Sie sich folgende Artikel an:
* [Überprüfen der ExpressRoute-Konnektivität](expressroute-troubleshooting-expressroute-overview.md)
* [Beheben von Problemen bei der Netzwerkleistung](expressroute-troubleshooting-network-performance.md)
