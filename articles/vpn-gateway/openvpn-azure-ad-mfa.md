---
title: 'Aktivieren Sie MFA für VPN-Benutzer: Azure AD-Authentifizierung'
description: Aktivieren der mehrstufigen Authentifizierung für VPN-Benutzer
services: vpn-gateway
author: anzaman
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 02/14/2020
ms.author: alzam
ms.openlocfilehash: 34ef1b73b06870fd4eaabe88147cd98b281c1f11
ms.sourcegitcommit: 64def2a06d4004343ec3396e7c600af6af5b12bb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/19/2020
ms.locfileid: "77472348"
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

Um eine Verbindung mit Ihrem virtuellen Netzwerk herzustellen, müssen Sie ein VPN-Clientprofil erstellen und konfigurieren. Entsprechende Informationen finden Sie unter [Konfigurieren eines VPN-Clients für P2S-VPN-Verbindungen](openvpn-azure-ad-client.md).