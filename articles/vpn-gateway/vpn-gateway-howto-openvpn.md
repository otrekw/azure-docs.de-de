---
title: Konfigurieren von OpenVPN in Azure VPN Gateway
description: Erfahren Sie, wie Sie PowerShell verwenden, um das OpenVPN-Protokoll in Azure VPN Gateway für eine Point-to-Site-Umgebung zu aktivieren.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: how-to
ms.date: 02/05/2021
ms.author: cherylmc
ms.openlocfilehash: 4b48e98ab35b620030b15165d4c9341c0f11b440
ms.sourcegitcommit: d4734bc680ea221ea80fdea67859d6d32241aefc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/14/2021
ms.locfileid: "100393274"
---
# <a name="configure-openvpn-for-azure-point-to-site-vpn-gateway"></a>Konfigurieren von OpenVPN-Clients für Azure VPN Gateway für Point-to-Site

In diesem Artikel erfahren Sie, wie Sie **OpenVPN® Protocol** für Azure VPN Gateway einrichten. Sie können die Anweisungen für das Portal oder PowerShell nutzen.

## <a name="prerequisites"></a>Voraussetzungen

* In diesem Artikel wird davon ausgegangen, dass Sie in einer Point-to-Site-Umgebung arbeiten. Erstellen Sie andernfalls mithilfe einer der folgenden Methoden eine.

  * [Portal: Erstellen einer Point-to-Site-Verbindung](vpn-gateway-howto-point-to-site-resource-manager-portal.md)

  * [PowerShell: Erstellen einer Point-to-Site-Verbindung](vpn-gateway-howto-point-to-site-rm-ps.md)

* Vergewissern Sie sich, dass für Ihr VPN-Gateway nicht die Basic-SKU verwendet wird. Die Basic-SKU wird für OpenVPN nicht unterstützt.

## <a name="portal"></a>Portal

1. Navigieren Sie im Portal zu **Gateway für virtuelle Netzwerke > Point-to-Site-Konfiguration**.
1. Wählen Sie für **Tunneltyp** in der Dropdownliste **OpenVPN (SSL)** aus.

   :::image type="content" source="./media/vpn-gateway-howto-openvpn/portal.png" alt-text="Auswählen von „OpenVPN (SSL)“ in der Dropdownliste":::
1. Speichern Sie Ihre Änderungen, und fahren Sie mit den Anweisungen unter **Nächste Schritte** fort.

## <a name="enable-openvpn-on-your-gateway-using-powershell"></a>Aktivieren Sie OpenVPN mithilfe von PowerShell auf Ihrem Gateway.

1. Aktivieren Sie OpenVPN auf Ihrem Gateway mit dem folgenden Beispiel:

   ```azurepowershell-interactive
   $gw = Get-AzVirtualNetworkGateway -ResourceGroupName $rgname -name $name
   Set-AzVirtualNetworkGateway -VirtualNetworkGateway $gw -VpnClientProtocol OpenVPN
   ```
1. Fahren Sie mit **Nächste Schritte** fort.

## <a name="next-steps"></a>Nächste Schritte

Informationen zum Konfigurieren von OpenVPN finden Sie unter [Konfigurieren von OpenVPN-Clients](vpn-gateway-howto-openvpn-clients.md).

**„OpenVPN“ ist eine Marke von OpenVPN Inc.**
