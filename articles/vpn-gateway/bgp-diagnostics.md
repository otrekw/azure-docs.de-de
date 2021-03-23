---
title: Anzeigen des BGP-Status und der zugehörigen Metriken
titleSuffix: Azure VPN Gateway
description: Zeigen Sie wichtige Informationen für die Problembehandlung im Zusammenhang mit BGP an.
services: vpn-gateway
author: anzaman
ms.service: vpn-gateway
ms.topic: sample
ms.date: 03/10/2021
ms.author: alzam
ms.openlocfilehash: f97bbccc980705699af822ba2730233239cdfd5f
ms.sourcegitcommit: 6776f0a27e2000fb1acb34a8dddc67af01ac14ac
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/11/2021
ms.locfileid: "103148845"
---
# <a name="view-bgp-metrics-and-status"></a>Anzeigen der BGP-Metriken und des zugehörigen Status

Sie können BGP-Metriken und den zugehörigen Status im Azure-Portal oder mithilfe von Azure PowerShell anzeigen.

## <a name="azure-portal"></a>Azure-Portal

Im Azure-Portal können Sie BGP-Peers, erlernte Routen und angekündigte Routen anzeigen. Darüber hinaus können Sie CSV-Dateien mit diesen Daten herunterladen.

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com) zu Ihrem Gateway für virtuelle Netzwerke.
1. Wählen Sie unter **Überwachung** die Option **BGP-Peers** aus, um die Seite „BGP-Peers“ zu öffnen.

   :::image type="content" source="./media/bgp-diagnostics/bgp-portal.jpg" alt-text="Screenshot: Metriken im Azure-Portal":::

### <a name="learned-routes"></a>Erlernte Routen

1. Sie können bis zu 50 erlernte Routen im Portal anzeigen.

   :::image type="content" source="./media/bgp-diagnostics/learned-routes.jpg" alt-text="Screenshot: Erlernte Routen":::

1. Außerdem können Sie die Datei mit den erlernten Routen herunterladen. Wenn Sie über mehr als 50 erlernte Routen verfügen, müssen Sie die CSV-Datei herunterladen und anzeigen, um alle anzeigen zu können. Wählen Sie zum Herunterladen die Option **Erlernte Routen herunterladen** aus.

   :::image type="content" source="./media/bgp-diagnostics/download-routes.jpg" alt-text="Screenshot: Herunterladen erlernter Routen":::
1. Zeigen Sie dann die Datei an.

   :::image type="content" source="./media/bgp-diagnostics/learned-routes-file.jpg" alt-text="Screenshot: Heruntergeladene erlernte Routen":::

### <a name="advertised-routes"></a>Angekündigte Routen

1. Wählen Sie zum Anzeigen angekündigter Routen am Ende des gewünschten Netzwerks die drei Punkte ( **...** ) aus, und klicken Sie dann auf **Angekündigte Routen anzeigen**.

   :::image type="content" source="./media/bgp-diagnostics/select-route.png" alt-text="Screenshot: Anzeigen angekündigter Routen" lightbox="./media/bgp-diagnostics/route-expand.png":::
1. Auf der Seite **Für Peer angekündigte Routen** können bis zu 50 angekündigte Routen angezeigt werden.

   :::image type="content" source="./media/bgp-diagnostics/advertised-routes.jpg" alt-text="Screenshot: Angekündigte Routen":::
1. Außerdem können Sie die Datei mit den angekündigten Routen herunterladen. Wenn Sie über mehr als 50 angekündigte Routen verfügen, müssen Sie die CSV-Datei herunterladen und anzeigen, um alle anzeigen zu können. Wählen Sie zum Herunterladen die Option **Angekündigte Routen herunterladen** aus.

   :::image type="content" source="./media/bgp-diagnostics/download-advertised.jpg" alt-text="Screenshot: Auswählen heruntergeladener angekündigter Routen":::
1. Zeigen Sie dann die Datei an.

   :::image type="content" source="./media/bgp-diagnostics/advertised-routes-file.jpg" alt-text="Screenshot: Heruntergeladene angekündigte Routen":::

### <a name="bgp-peers"></a>BGP-Peers

1. Sie können bis zu 50 BGP-Peers im Portal anzeigen.

   :::image type="content" source="./media/bgp-diagnostics/peers.jpg" alt-text="Screenshot: BGP-Peers":::
1. Außerdem können Sie die Datei mit den BGP Peers herunterladen. Wenn Sie über mehr als 50 BGP-Peers verfügen, müssen Sie die CSV-Datei herunterladen und anzeigen, um alle anzeigen zu können. Wählen Sie zum Herunterladen auf der Portalseite die Option **BGP-Peers herunterladen** aus.

   :::image type="content" source="./media/bgp-diagnostics/download-peers.jpg" alt-text="Screenshot: Herunterladen der BGP-Peers":::
1. Zeigen Sie dann die Datei an.

   :::image type="content" source="./media/bgp-diagnostics/peers-file.jpg" alt-text="Screenshot: Heruntergeladene BGP-Peers":::

## <a name="powershell"></a>PowerShell

Verwenden Sie **Get-AzVirtualNetworkGatewayBGPPeerStatus**, um alle BGP-Peers sowie den Status anzuzeigen.

[!INCLUDE [VPN Gateway PowerShell instructions](../../includes/vpn-gateway-cloud-shell-powershell-about.md)]

```azurepowershell-interactive
Get-AzVirtualNetworkGatewayBgpPeerStatus -ResourceGroupName resourceGroup -VirtualNetworkGatewayName gatewayName

Asn               : 65515
ConnectedDuration : 9.01:04:53.5768637
LocalAddress      : 10.1.0.254
MessagesReceived  : 14893
MessagesSent      : 14900
Neighbor          : 10.0.0.254
RoutesReceived    : 1
State             : Connected
```

Verwenden Sie **Get-AzVirtualNetworkGatewayLearnedRoute**, um alle Routen anzuzeigen, die das Gateway durch BGP kennengelernt hat.

```azurepowershell-interactive
Get-AzVirtualNetworkGatewayLearnedRoute -ResourceGroupName resourceGroup -VirtualNetworkGatewayname gatewayName

AsPath       :
LocalAddress : 10.1.0.254
Network      : 10.1.0.0/16
NextHop      :
Origin       : Network
SourcePeer   : 10.1.0.254
Weight       : 32768

AsPath       :
LocalAddress : 10.1.0.254
Network      : 10.0.0.254/32
NextHop      :
Origin       : Network
SourcePeer   : 10.1.0.254
Weight       : 32768

AsPath       : 65515
LocalAddress : 10.1.0.254
Network      : 10.0.0.0/16
NextHop      : 10.0.0.254
Origin       : EBgp
SourcePeer   : 10.0.0.254
Weight       : 32768
```

Verwenden Sie **Get-AzVirtualNetworkGatewayAdvertisedRoute**, um alle Routen anzuzeigen, die das Gateway über BGP an seine Peers sendet.

```azurepowershell-interactive
Get-AzVirtualNetworkGatewayAdvertisedRoute -VirtualNetworkGatewayName gatewayName -ResourceGroupName resourceGroupName -Peer 10.0.0.254
```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu BGP finden Sie unter [Konfigurieren von BGP für Azure-VPN-Gateways](bgp-howto.md).
