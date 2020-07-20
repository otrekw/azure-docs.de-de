---
title: Konfigurieren eines Always On-VPN-Benutzertunnels
titleSuffix: Azure Virtual WAN
description: In diesem Artikel wird beschrieben, wie Sie einen Always On-VPN-Benutzertunnel für Virtual WAN konfigurieren.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: how-to
ms.date: 06/22/2020
ms.author: cherylmc
ms.openlocfilehash: 03f67053a5a199c8c64efb05d2b6a65ad6707650
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85564047"
---
# <a name="configure-an-always-on-vpn-user-tunnel-for-virtual-wan"></a>Konfigurieren eines Always On-VPN-Benutzertunnels für Virtual WAN

[!INCLUDE [intro](../../includes/vpn-gateway-vwan-always-on-intro.md)]

## <a name="prerequisites"></a>Voraussetzungen

Sie müssen eine Point-to-Site-Konfiguration erstellen und die Zuweisung des virtuelle Hubs bearbeiten. Anweisungen finden Sie in den folgenden Abschnitten:

* [Erstellen einer P2S-Konfiguration](virtual-wan-point-to-site-portal.md#p2sconfig)
* [Erstellen eines Hubs mit P2S-Gateway](virtual-wan-point-to-site-portal.md#hub)

## <a name="configure-a-user-tunnel"></a>Konfigurieren eines Benutzertunnels

[!INCLUDE [user tunnel](../../includes/vpn-gateway-vwan-always-on-user.md)]

## <a name="to-remove-a-profile"></a>So entfernen Sie ein Profil

Führen Sie die folgenden Schritte aus, um ein Profil zu entfernen:

1. Führen Sie den folgenden Befehl aus:

   ```powershell
   C:\> Remove-VpnConnection UserTest  
   ```

1. Trennen Sie die Verbindung, und deaktivieren Sie das Kontrollkästchen **Automatisch verbinden**.

   ![Cleanup](./media/howto-always-on-user-tunnel/disconnect.jpg)

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Virtual WAN finden Sie unter [Häufig gestellte Fragen](virtual-wan-faq.md).
