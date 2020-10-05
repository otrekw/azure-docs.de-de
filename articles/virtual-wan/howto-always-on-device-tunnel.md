---
title: Konfigurieren eines Always On-VPN-Tunnels
titleSuffix: Azure Virtual WAN
description: Schritte zum Konfigurieren eines Always On-VPN-Gerätetunnels für Virtual WAN
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: how-to
ms.date: 09/22/2020
ms.author: cherylmc
ms.openlocfilehash: e814487cb4dab9c8c19daab2ea3bb81391d4a98f
ms.sourcegitcommit: bdd5c76457b0f0504f4f679a316b959dcfabf1ef
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90983687"
---
# <a name="configure-an-always-on-vpn-device-tunnel-for-virtual-wan"></a>Konfigurieren eines Always On-VPN-Gerätetunnels für Virtual WAN

[!INCLUDE [intro](../../includes/vpn-gateway-vwan-always-on-intro.md)]

## <a name="prerequisites"></a>Voraussetzungen

Sie müssen eine Point-to-Site-Konfiguration erstellen und die Zuweisung des virtuelle Hubs bearbeiten. Anweisungen finden Sie in den folgenden Abschnitten:

* [Erstellen einer P2S-Konfiguration](virtual-wan-point-to-site-portal.md#p2sconfig)
* [Erstellen eines Hubs mit P2S-Gateway](virtual-wan-point-to-site-portal.md#hub)

## <a name="configure-the-device-tunnel"></a>Konfigurieren des Gerätetunnels

[!INCLUDE [device tunnel](../../includes/vpn-gateway-vwan-always-on-device.md)]

## <a name="to-remove-a-profile"></a>So entfernen Sie ein Profil

Führen Sie den folgenden Befehl aus, um das Profil zu entfernen:

![Screenshot eines PowerShell-Fensters, in dem der Befehl „Remove-VpnConnection -Name MachineCertTest“ ausgeführt wird.](./media/howto-always-on-device-tunnel/cleanup.png)

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Virtual WAN finden Sie unter [Häufig gestellte Fragen](virtual-wan-faq.md).