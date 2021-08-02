---
title: Ankündigen von benutzerdefinierten Routen für Point-to-Site-Clients des VPN Gateways
titleSuffix: Azure VPN Gateway
description: Erfahren Sie, wie Sie benutzerdefinierte Routen für Ihre Point-to-Site-Clients des VPN Gateways ankündigen. Dieser Artikel enthält die Schritte für das erzwungene Tunneln von VPN-Clients.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: how-to
ms.date: 06/08/2021
ms.author: cherylmc
ms.openlocfilehash: b37b63fda6b142fc1aba458c007b6fe0eb5db814
ms.sourcegitcommit: a434cfeee5f4ed01d6df897d01e569e213ad1e6f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/09/2021
ms.locfileid: "111810946"
---
# <a name="advertise-custom-routes-for-p2s-vpn-clients"></a>Ankündigen von benutzerdefinierten Routen für P2S-VPN-Clients

Möglicherweise möchten Sie benutzerdefinierte Routen für alle Ihre Point-to-Site-Clients ankündigen. Beispielsweise dann, wenn Sie Speicherendpunkte in Ihrem VNET aktiviert haben und möchten, dass die Remotebenutzer über die VPN-Verbindung auf diese Speicherkonten zugreifen können. Sie können die IP-Adresse des Speicherendpunkts allen Remotebenutzern ankündigen, damit der Datenverkehr zum Speicherkonto über den VPN-Tunnel und nicht über das öffentliche Internet geleitet wird. Sie können auch benutzerdefinierte Routen verwenden, um erzwungenes Tunneln für VPN-Clients zu konfigurieren.

:::image type="content" source="./media/vpn-gateway-p2s-advertise-custom-routes/custom-routes.png" alt-text="Diagramm der Ankündigung benutzerdefinierter Routen":::

## <a name="advertise-custom-routes"></a>So kündigen Sie benutzerdefinierte Routen an.

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

## <a name="advertise-custom-routes---forced-tunneling"></a>Ankündigen benutzerdefinierter Routen – erzwungenes Tunneln

Sie können den gesamten Datenverkehr an den VPN-Tunnel leiten, indem Sie 0.0.0.0/1 und 128.0.0.0/1 als benutzerdefinierte Routen an die Clients ankündigen. Der Grund für die Aufteilung von 0.0.0.0/0 in zwei kleinere Subnetze ist, dass diese kleineren Präfixe spezifischer sind als die Standardroute, die möglicherweise bereits auf dem lokalen Netzwerkadapter konfiguriert ist und daher beim Weiterleiten von Datenverkehr bevorzugt wird.

1. Verwenden Sie die folgenden Befehle, um das erzwungene Tunneln zu aktivieren:

    ```azurepowershell-interactive    
    $gw = Get-AzVirtualNetworkGateway -Name <name of gateway> -ResourceGroupName <name of resource group>
    Set-AzVirtualNetworkGateway -VirtualNetworkGateway $gw -CustomRoute 0.0.0.0/1 , 128.0.0.0/1
    ```
## <a name="view-custom-routes"></a>Anzeigen benutzerdefinierter Routen

Verwenden Sie das folgende Beispiel, um benutzerdefinierte Routen anzuzeigen:

  ```azurepowershell-interactive
  $gw = Get-AzVirtualNetworkGateway -Name <name of gateway> -ResourceGroupName <name of resource group>
  $gw.CustomRoutes | Format-List
  ```
## <a name="delete-custom-routes"></a>Löschen benutzerdefinierter Routen

Verwenden Sie das folgende Beispiel, um benutzerdefinierte Routen zu löschen:

  ```azurepowershell-interactive
  $gw = Get-AzVirtualNetworkGateway -Name <name of gateway> -ResourceGroupName <name of resource group>
  Set-AzVirtualNetworkGateway -VirtualNetworkGateway $gw -CustomRoute @0
  ```
## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum P2S-Routing finden Sie unter [Informationen zum Point-to-Site-Routing](vpn-gateway-about-point-to-site-routing.md).
