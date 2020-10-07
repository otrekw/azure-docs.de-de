---
title: 'Azure-VPN Gateway: Point-to-Site-VPN-Sitzungsverwaltung'
description: Dieser Artikel unterstützt Sie beim Anzeigen und Trennen von Point-to-Site-VPN-Sitzungen.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: how-to
ms.date: 09/23/2020
ms.author: cherylmc
ms.openlocfilehash: 2f2184507e17e3ecae40bb33be4202c183d32b77
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2020
ms.locfileid: "91274232"
---
# <a name="point-to-site-vpn-session-management"></a>Point-to-Site-VPN-Sitzungsverwaltung

Azure Virtual Network-Gateways bieten eine einfache Möglichkeit, aktuelle Point-to-Site-VPN-Sitzungen anzuzeigen und zu trennen. Dieser Artikel unterstützt Sie beim Anzeigen und Trennen von aktuellen Sitzungen.

>[!NOTE]
>Der Sitzungsstatus wird alle fünf Minuten aktualisiert. Er wird nicht sofort aktualisiert.
>

## <a name="portal"></a>Portal

So zeigen Sie eine Sitzung im Portal an und trennen die Verbindung

1. Navigieren Sie zum VPN-Gateway.
1. Wählen Sie unter dem Abschnitt **Überwachung** die Option **Point-to-Site-Sitzungen** aus.

   :::image type="content" source="./media/p2s-session-management/portal.png" alt-text="Portal-Beispiel":::
1. Sie können alle aktuellen Sitzungen im Fensterbereich anzeigen.
1. Wählen Sie **„...“** für die zu trennende Sitzung und dann **Trennen** aus.

## <a name="powershell"></a>PowerShell

So zeigen Sie eine Sitzung mithilfe von PowerShell an und trennen sie

1. Führen Sie den folgenden PowerShell-Befehl aus, um aktive Sitzungen aufzulisten:

   ```azurepowershell-interactive
   Get-AzVirtualNetworkGatewayVpnClientConnectionHealth -VirtualNetworkGatewayName <name of the gateway>  -ResourceGroupName <name of the resource group>
   ```
1. Kopieren Sie die **VpnConnectionId** der Sitzung, die Sie trennen möchten.

   :::image type="content" source="./media/p2s-session-management/powershell.png" alt-text="Portal-Beispiel":::
1. Führen Sie den folgenden Befehl aus, um die Sitzung zu trennen:

   ```azurepowershell-interactive
   Disconnect-AzVirtualNetworkGatewayVpnConnection -VirtualNetworkGatewayName <name of the gateway> -ResourceGroupName <name of the resource group> -VpnConnectionId <VpnConnectionId of the session>
   ```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Point-to-Site-Verbindungen finden Sie unter [Informationen zu Point-to-Site-VPN](point-to-site-about.md).
