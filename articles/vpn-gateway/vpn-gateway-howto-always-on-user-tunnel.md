---
title: Konfigurieren eines Always On-VPN-Benutzertunnels
titleSuffix: Azure VPN Gateway
description: Hier erfahren Sie, wie Sie einen Always On-VPN-Benutzertunnel für Ihr VPN-Gateway konfigurieren.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: how-to
ms.date: 04/29/2021
ms.author: cherylmc
ms.openlocfilehash: 59c12333c6cf8ea9d251ce6387e3dce4ff23468b
ms.sourcegitcommit: fc9fd6e72297de6e87c9cf0d58edd632a8fb2552
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/30/2021
ms.locfileid: "108287318"
---
# <a name="configure-an-always-on-vpn-user-tunnel"></a>Konfigurieren eines Always On-VPN-Benutzertunnels

[!INCLUDE [intro](../../includes/vpn-gateway-vwan-always-on-intro.md)]

## <a name="configure-the-gateway"></a>Konfigurieren des Gateways

 Konfigurieren Sie das VPN-Gateway für die Verwendung von IKEv2 und zertifikatbasierter Authentifizierung gemäß der Anleitung im Artikel [Konfigurieren einer Point-to-Site-VPN-Verbindung mit einem VNET unter Verwendung der nativen Azure-Zertifikatauthentifizierung: Azure-Portal](vpn-gateway-howto-point-to-site-resource-manager-portal.md).

## <a name="configure-a-user-tunnel"></a>Konfigurieren eines Benutzertunnels

[!INCLUDE [user configuration](../../includes/vpn-gateway-vwan-always-on-user.md)]

## <a name="to-remove-a-profile"></a>So entfernen Sie ein Profil

Führen Sie die folgenden Schritte aus, um ein Profil zu entfernen:

1. Führen Sie den folgenden Befehl aus:

   ```powershell
   C:\> Remove-VpnConnection UserTest  
   ```

1. Trennen Sie die Verbindung, und deaktivieren Sie das Kontrollkästchen **Automatisch verbinden**.

   ![Cleanup](./media/vpn-gateway-howto-always-on-user-tunnel/disconnect.jpg)

## <a name="next-steps"></a>Nächste Schritte

Informationen zum Beheben möglicher Verbindungsprobleme finden Sie unter [Azure Point-to-Site-Verbindungsprobleme](vpn-gateway-troubleshoot-vpn-point-to-site-connection-problems.md).
