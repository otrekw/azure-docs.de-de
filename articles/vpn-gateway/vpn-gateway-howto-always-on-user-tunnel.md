---
title: Konfigurieren eines Always On-VPN-Benutzertunnels
titleSuffix: Azure VPN Gateway
description: In diesem Artikel wird beschrieben, wie Sie einen Always On-VPN-Benutzertunnel für Ihr VPN-Gateway konfigurieren.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 03/12/2020
ms.author: cherylmc
ms.openlocfilehash: 56934dd13661d8f623e673e2817e87618675c7ed
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "79502259"
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
