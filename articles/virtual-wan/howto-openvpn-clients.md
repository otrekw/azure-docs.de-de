---
title: Konfigurieren von OpenVPN-Clients für Azure Virtual WAN
description: Dieser Artikel unterstützt Sie beim Konfigurieren von OpenVPN-Clients für Azure Virtual WAN. Er enthält Konfigurationsschritte für Windows-, Mac-, iOS- und Linux-Clients.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: how-to
ms.date: 04/27/2021
ms.author: cherylmc
ms.openlocfilehash: 91b4a9a69912f7a5980348cc2b9a874673fa0eb2
ms.sourcegitcommit: 62e800ec1306c45e2d8310c40da5873f7945c657
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2021
ms.locfileid: "108163389"
---
# <a name="configure-an-openvpn-client-for-azure-virtual-wan"></a>Konfigurieren eines OpenVPN-Clients für Azure Virtual WAN

Dieser Artikel hilft Ihnen beim Konfigurieren von Clients mit dem **OpenVPN**-Protokoll&reg;. Sie können auch den Azure VPN Client für Windows 10 verwenden, um eine Verbindung über das OpenVPN-Protokoll herzustellen. Weitere Anweisungen finden Sie [hier](openvpn-azure-ad-client.md).

## <a name="before-you-begin"></a>Voraussetzungen

Erstellen Sie eine Benutzer-VPN-Konfiguration (Point-to-Site). Stellen Sie sicher, dass Sie den Tunneltyp „OpenVPN“ auswählen. Die einzelnen Schritte finden Sie unter [Erstellen einer P2S-Konfiguration für Azure Virtual WAN](virtual-wan-point-to-site-portal.md#p2sconfig).

[!INCLUDE [configuration steps](../../includes/vpn-gateway-vwan-config-openvpn-clients.md)]

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Benutzer-VPN (Point-to-Site) finden Sie unter [Erstellen von Benutzer-VPN-Verbindungen](virtual-wan-point-to-site-portal.md).

**„OpenVPN“ ist eine Marke von OpenVPN Inc.**
