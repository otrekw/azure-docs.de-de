---
title: Konfigurieren eines Always On-VPN-Tunnels
titleSuffix: Azure VPN Gateway
description: Erfahren Sie, wie Sie mithilfe von Gateways unter Windows 10 Always On persistente Gerätetunnel zu Azure einrichten und konfigurieren.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: how-to
ms.date: 09/03/2020
ms.author: cherylmc
ms.openlocfilehash: ab951375fdf9c5317e2c3a2f67271666d0987142
ms.sourcegitcommit: bf1340bb706cf31bb002128e272b8322f37d53dd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/03/2020
ms.locfileid: "89435846"
---
# <a name="configure-an-always-on-vpn-device-tunnel"></a>Konfigurieren eines Always On-VPN-Gerätetunnels

[!INCLUDE [intro](../../includes/vpn-gateway-vwan-always-on-intro.md)]

## <a name="configure-the-gateway"></a>Konfigurieren des Gateways

Konfigurieren Sie das VPN-Gateway für die Verwendung von IKEv2 und zertifikatbasierter Authentifizierung mithilfe des Artikels [Konfigurieren einer Point-to-Site-VPN-Verbindung mit einem VNET unter Verwendung der nativen Azure-Zertifikatauthentifizierung: Azure-Portal](vpn-gateway-howto-point-to-site-resource-manager-portal.md).

## <a name="configure-the-device-tunnel"></a>Konfigurieren des Gerätetunnels

[!INCLUDE [device tunnel](../../includes/vpn-gateway-vwan-always-on-device.md)]

## <a name="to-remove-a-profile"></a>So entfernen Sie ein Profil

Führen Sie den folgenden Befehl aus, um das Profil zu entfernen:

![Cleanup](./media/vpn-gateway-howto-always-on-device-tunnel/cleanup.png)

## <a name="next-steps"></a>Nächste Schritte

Informationen zur Problembehandlung finden Sie unter [Azure Point-to-Site-Verbindungsprobleme](vpn-gateway-troubleshoot-vpn-point-to-site-connection-problems.md).
