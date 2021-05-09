---
title: 'Azure-VPN Gateway: Point-to-Site-VPN-Sitzungsverwaltung'
description: Dieser Artikel unterstützt Sie beim Anzeigen und Trennen von Point-to-Site-VPN-Sitzungen.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: how-to
ms.date: 04/26/2021
ms.author: cherylmc
ms.openlocfilehash: 0b0a4c712333516dcdd20dcde9ef4b9c74b5f228
ms.sourcegitcommit: 2f322df43fb3854d07a69bcdf56c6b1f7e6f3333
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/27/2021
ms.locfileid: "108018653"
---
# <a name="point-to-site-vpn-session-management"></a>Point-to-Site-VPN-Sitzungsverwaltung

Azure Virtual Network-Gateways bieten eine einfache Möglichkeit, aktuelle Point-to-Site-VPN-Sitzungen anzuzeigen und zu trennen. Dieser Artikel unterstützt Sie beim Anzeigen und Trennen von aktuellen Sitzungen. Der Sitzungsstatus wird alle fünf Minuten aktualisiert. Er wird nicht sofort aktualisiert. 


## <a name="portal"></a>Portal

>[!NOTE]
> Informationen zur Verbindungsquelle werden nur für IKEv2- und OpenVPN-Verbindungen bereitgestellt.
> 

So zeigen Sie eine Sitzung im Portal an und trennen die Verbindung

1. Navigieren Sie zum VPN-Gateway.
1. Wählen Sie unter dem Abschnitt **Überwachung** die Option **Point-to-Site-Sitzungen** aus.

   :::image type="content" source="./media/p2s-session-management/portal.png" alt-text="Portal-Beispiel":::
1. Sie können alle aktuellen Sitzungen im Fensterbereich anzeigen.
1. Wählen Sie **„...“** für die zu trennende Sitzung und dann **Trennen** aus.

Für VpnGw4- und VpnGw5-SKUs kann dieses Feature im Portal derzeit nicht verwendet werden. Wenn Sie über eines dieser Gateways verfügen, verwenden Sie die im nächsten Abschnitt beschriebene PowerShell-Methode.

## <a name="powershell"></a>PowerShell

So zeigen Sie eine Sitzung mithilfe von PowerShell an und trennen sie

1. Führen Sie den folgenden PowerShell-Befehl aus, um aktive Sitzungen aufzulisten:

   ```azurepowershell-interactive
   Get-AzVirtualNetworkGatewayVpnClientConnectionHealth -VirtualNetworkGatewayName <name of the gateway>  -ResourceGroupName <name of the resource group>
   ```
1. Kopieren Sie die **VpnConnectionId** der Sitzung, die Sie trennen möchten.

   :::image type="content" source="./media/p2s-session-management/powershell.png" alt-text="PowerShell-Beispiel":::
1. Führen Sie den folgenden Befehl aus, um die Sitzung zu trennen:

   ```azurepowershell-interactive
   Disconnect-AzVirtualNetworkGatewayVpnConnection -VirtualNetworkGatewayName <name of the gateway> -ResourceGroupName <name of the resource group> -VpnConnectionId <VpnConnectionId of the session>
   ```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Point-to-Site-Verbindungen finden Sie unter [Informationen zu Point-to-Site-VPN](point-to-site-about.md).
