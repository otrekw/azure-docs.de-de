---
title: Konfigurieren eines Always On-VPN-Benutzertunnels
titleSuffix: Azure Virtual WAN
description: In diesem Artikel wird beschrieben, wie Sie einen Always On-VPN-Benutzertunnel für Virtual WAN konfigurieren.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 03/13/2020
ms.author: cherylmc
ms.openlocfilehash: dd5b215b143fbaf487325744a158bb8b05707951
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "79504042"
---
# <a name="configure-an-always-on-vpn-user-tunnel-for-virtual-wan"></a>Konfigurieren eines Always On-VPN-Benutzertunnels für Virtual WAN

[!INCLUDE [intro](../../includes/vpn-gateway-vwan-always-on-intro.md)]

## <a name="prerequisites"></a>Voraussetzungen

Sie müssen eine Point-to-Site-Konfiguration erstellen und die Zuweisung des virtuelle Hubs bearbeiten. Anweisungen finden Sie in den folgenden Abschnitten:

* [Erstellen einer P2S-Konfiguration](virtual-wan-point-to-site-portal.md#p2sconfig)
* [Bearbeiten der Hubzuweisung](virtual-wan-point-to-site-portal.md#edit)

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
