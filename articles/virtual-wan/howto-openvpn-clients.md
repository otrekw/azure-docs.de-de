---
title: Konfigurieren von OpenVPN-Clients für Azure Virtual WAN
description: Erfahren Sie, wie Sie OpenVPN-Clients für Azure Virtual WAN konfigurieren. Der Artikel enthält Konfigurationsschritte für Windows-, Mac-, iOS- und Linux-Clients.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: how-to
ms.date: 04/27/2021
ms.author: cherylmc
ms.openlocfilehash: 83c70aca81eaa888186807d43fff5a7bbaccb700
ms.sourcegitcommit: e1d5abd7b8ded7ff649a7e9a2c1a7b70fdc72440
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/27/2021
ms.locfileid: "110579604"
---
# <a name="configure-an-openvpn-client-for-azure-virtual-wan"></a>Konfigurieren eines OpenVPN-Clients für Azure Virtual WAN

Dieser Artikel hilft Ihnen beim Konfigurieren von Clients mit dem **OpenVPN**-Protokoll&reg;. Sie können auch den Azure VPN Client für Windows 10 verwenden, um eine Verbindung über das OpenVPN-Protokoll herzustellen. Weitere Informationen finden Sie unter [Konfigurieren eines VPN-Clients für P2S OpenVPN-Verbindungen](openvpn-azure-ad-client.md).

## <a name="before-you-begin"></a>Voraussetzungen

Erstellen Sie eine Benutzer-VPN-Konfiguration (Point-to-Site). Stellen Sie sicher, dass Sie den Tunneltyp „OpenVPN“ auswählen. Die einzelnen Schritte finden Sie unter [Erstellen einer P2S-Konfiguration für Azure Virtual WAN](virtual-wan-point-to-site-portal.md#p2sconfig).

[!INCLUDE [configuration steps](../../includes/vpn-gateway-vwan-config-openvpn-clients.md)]

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Benutzer-VPN (Point-to-Site) finden Sie unter [Erstellen von Benutzer-VPN-Verbindungen](virtual-wan-point-to-site-portal.md).

**„OpenVPN“ ist eine Marke von OpenVPN Inc.**
