---
title: Aktivieren von OpenVPN für P2S-VPN-Gateways
titleSuffix: Azure VPN Gateway
description: Hier erfahren Sie, wie Sie das OpenVPN-Protokoll in VPN-Gateways für Point-to-Site-Konfigurationen aktivieren.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: how-to
ms.date: 02/05/2021
ms.author: cherylmc
ms.openlocfilehash: 056e9a44009f90be23d66c5da005902ccc8ebebf
ms.sourcegitcommit: a5dd9799fa93c175b4644c9fe1509e9f97506cc6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2021
ms.locfileid: "108205429"
---
# <a name="configure-openvpn-for-point-to-site-vpn-gateways"></a>Konfigurieren von OpenVPN-Clients für Point-to-Site-VPN-Gateways

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

## <a name="powershell"></a>PowerShell

1. Aktivieren Sie OpenVPN auf Ihrem Gateway mit dem folgenden Beispiel:

   ```azurepowershell-interactive
   $gw = Get-AzVirtualNetworkGateway -ResourceGroupName $rgname -name $name
   Set-AzVirtualNetworkGateway -VirtualNetworkGateway $gw -VpnClientProtocol OpenVPN
   ```
1. Fahren Sie mit **Nächste Schritte** fort.

## <a name="next-steps"></a>Nächste Schritte

Informationen zum Konfigurieren von OpenVPN finden Sie unter [Konfigurieren von OpenVPN-Clients](vpn-gateway-howto-openvpn-clients.md).

**„OpenVPN“ ist eine Marke von OpenVPN Inc.**
