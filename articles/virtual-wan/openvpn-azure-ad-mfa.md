---
title: 'Aktivieren Sie MFA für VPN-Benutzer: Azure AD-Authentifizierung'
description: Aktivieren der mehrstufigen Authentifizierung für VPN-Benutzer
services: virtual-wan
author: anzaman
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 01/16/2020
ms.author: alzam
ms.openlocfilehash: 13f012af95bb2b6098317e59e5293fb72804a6a6
ms.sourcegitcommit: 64def2a06d4004343ec3396e7c600af6af5b12bb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/19/2020
ms.locfileid: "77471523"
---
# <a name="enable-azure-multi-factor-authentication-mfa-for-vpn-users"></a>Aktivieren von Azure Multi-Factor Authentication (MFA) für VPN-Benutzer

[!INCLUDE [overview](../../includes/vpn-gateway-vwan-openvpn-enable-mfa-overview.md)]

## <a name="enableauth"></a>Aktivieren der Authentifizierung

[!INCLUDE [enable authentication](../../includes/vpn-gateway-vwan-openvpn-enable-auth.md)]

## <a name="enablesign"></a>Konfigurieren von Anmeldeeinstellungen

[!INCLUDE [sign in](../../includes/vpn-gateway-vwan-openvpn-sign-in.md)]

## <a name="peruser"></a>Option 1: Zugriff pro Benutzer

[!INCLUDE [per user](../../includes/vpn-gateway-vwan-openvpn-per-user.md)]

## <a name="conditional"></a>Option 2: bedingter Zugriff

[!INCLUDE [conditional access](../../includes/vpn-gateway-vwan-openvpn-conditional.md)]

## <a name="next-steps"></a>Nächste Schritte

Um eine Verbindung mit Ihrem virtuellen Netzwerk herzustellen, müssen Sie ein VPN-Clientprofil erstellen und konfigurieren. Informationen finden Sie unter [Konfigurieren der Azure AD-Authentifizierung für eine Point-to-Site-Verbindung mit Azure](virtual-wan-point-to-site-azure-ad.md).
