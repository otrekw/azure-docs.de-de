---
title: Konfigurieren eines Always On-VPN-Tunnels
titleSuffix: Azure Virtual WAN
description: Schritte zum Konfigurieren eines Always On-VPN-Gerätetunnels für Virtual WAN
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 03/13/2020
ms.author: cherylmc
ms.openlocfilehash: e3eea639eaa52c07e877476e9215144e916618d4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "79504038"
---
# <a name="configure-an-always-on-vpn-device-tunnel-for-virtual-wan"></a>Konfigurieren eines Always On-VPN-Gerätetunnels für Virtual WAN

[!INCLUDE [intro](../../includes/vpn-gateway-vwan-always-on-intro.md)]

## <a name="prerequisites"></a>Voraussetzungen

Sie müssen eine Point-to-Site-Konfiguration erstellen und die Zuweisung des virtuelle Hubs bearbeiten. Anweisungen finden Sie in den folgenden Abschnitten:

* [Erstellen einer P2S-Konfiguration](virtual-wan-point-to-site-portal.md#p2sconfig)
* [Bearbeiten der Hubzuweisung](virtual-wan-point-to-site-portal.md#edit)

## <a name="configure-the-device-tunnel"></a>Konfigurieren des Gerätetunnels

[!INCLUDE [device tunnel](../../includes/vpn-gateway-vwan-always-on-device.md)]

## <a name="to-remove-a-profile"></a>So entfernen Sie ein Profil

Führen Sie den folgenden Befehl aus, um das Profil zu entfernen:

![Cleanup](./media/howto-always-on-device-tunnel/cleanup.png)

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Virtual WAN finden Sie unter [Häufig gestellte Fragen](virtual-wan-faq.md).