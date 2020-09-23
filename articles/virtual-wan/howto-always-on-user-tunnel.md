---
title: Konfigurieren eines Always On-VPN-Benutzertunnels
titleSuffix: Azure Virtual WAN
description: In diesem Artikel wird beschrieben, wie Sie einen Always On-VPN-Benutzertunnel für Virtual WAN konfigurieren.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: how-to
ms.date: 09/10/2020
ms.author: cherylmc
ms.openlocfilehash: 14790d4be91ffd0463436b6ca1d5c8794c102697
ms.sourcegitcommit: 43558caf1f3917f0c535ae0bf7ce7fe4723391f9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/11/2020
ms.locfileid: "90016350"
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
