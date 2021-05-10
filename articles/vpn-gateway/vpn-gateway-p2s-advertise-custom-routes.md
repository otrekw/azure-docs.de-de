---
title: Ankündigen von benutzerdefinierten Routen für Point-to-Site-Clients des VPN Gateways
titleSuffix: Azure VPN Gateway
description: Erfahren Sie, wie Sie benutzerdefinierte Routen für Ihre Point-to-Site-Clients des VPN Gateways ankündigen.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: how-to
ms.date: 04/29/2021
ms.author: cherylmc
ms.openlocfilehash: f2d822fdf62d24718c3732ccb77fa5c5f50cd506
ms.sourcegitcommit: fc9fd6e72297de6e87c9cf0d58edd632a8fb2552
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/30/2021
ms.locfileid: "108293078"
---
# <a name="advertise-custom-routes-for-p2s-vpn-clients"></a>Ankündigen von benutzerdefinierten Routen für P2S-VPN-Clients

Möglicherweise möchten Sie benutzerdefinierte Routen für alle Ihre Point-to-Site-Clients ankündigen. Beispielsweise dann, wenn Sie Speicherendpunkte in Ihrem VNET aktiviert haben und möchten, dass die Remotebenutzer über die VPN-Verbindung auf diese Speicherkonten zugreifen können. Sie können die IP-Adresse des Speicherendpunkts allen Remotebenutzern ankündigen, damit der Datenverkehr zum Speicherkonto über den VPN-Tunnel und nicht über das öffentliche Internet geleitet wird.

![Beispiel für Multi-Site-Verbindung per Azure VPN Gateway](./media/vpn-gateway-p2s-advertise-custom-routes/custom-routes.png)

## <a name="to-advertise-custom-routes"></a>So kündigen Sie benutzerdefinierte Routen an

Wenn Sie benutzerdefinierte Routen ankündigen möchten, verwenden Sie das `Set-AzVirtualNetworkGateway cmdlet`. Im folgenden Beispiel wird gezeigt, wie Sie die IP-Adresse für die [Contoso-Speicherkontotabellen](https://contoso.table.core.windows.net) ankündigen.

1. Pingen Sie *contoso.table.core.windows.net*, und notieren Sie sich die IP-Adresse. Beispiel:

    ```cmd
    C:\>ping contoso.table.core.windows.net
    Pinging table.by4prdstr05a.store.core.windows.net [13.88.144.250] with 32 bytes of data:
    ```

2. Führen Sie die folgenden PowerShell-Befehle aus:

    ```azurepowershell-interactive
    $gw = Get-AzVirtualNetworkGateway -Name <name of gateway> -ResourceGroupName <name of resource group>
    Set-AzVirtualNetworkGateway -VirtualNetworkGateway $gw -CustomRoute 13.88.144.250/32
    ```

3. Verwenden Sie zum Hinzufügen mehrerer benutzerdefinierter Routen ein Komma und Leerzeichen, um die Adressen voneinander zu trennen. Beispiel:

    ```azurepowershell-interactive
    Set-AzVirtualNetworkGateway -VirtualNetworkGateway $gw -CustomRoute x.x.x.x/xx , y.y.y.y/yy
    ```
## <a name="to-view-custom-routes"></a>So zeigen Sie benutzerdefinierte Routen an

Verwenden Sie das folgende Beispiel, um benutzerdefinierte Routen anzuzeigen:

  ```azurepowershell-interactive
  $gw = Get-AzVirtualNetworkGateway -Name <name of gateway> -ResourceGroupName <name of resource group>
  $gw.CustomRoutes | Format-List
  ```
## <a name="to-delete-custom-routes"></a>So löschen Sie benutzerdefinierte Routen

Verwenden Sie das folgende Beispiel, um benutzerdefinierte Routen zu löschen:

  ```azurepowershell-interactive
  $gw = Get-AzVirtualNetworkGateway -Name <name of gateway> -ResourceGroupName <name of resource group>
  Set-AzVirtualNetworkGateway -VirtualNetworkGateway $gw -CustomRoute @0
  ```
## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum P2S-Routing finden Sie unter [Informationen zum Point-to-Site-Routing](vpn-gateway-about-point-to-site-routing.md).
