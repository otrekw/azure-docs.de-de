---
title: 'Aktivieren Sie MFA für VPN-Benutzer: Azure AD-Authentifizierung'
description: Aktivieren der mehrstufigen Authentifizierung für VPN-Benutzer
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: how-to
ms.date: 09/03/2020
ms.author: alzam
ms.openlocfilehash: 7e29aafe55c8007182c6183d53d988d8a18dd82c
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "89424973"
---
# <a name="enable-azure-multi-factor-authentication-mfa-for-vpn-users"></a>Aktivieren von Azure Multi-Factor Authentication (MFA) für VPN-Benutzer

[!INCLUDE [overview](../../includes/vpn-gateway-vwan-openvpn-enable-mfa-overview.md)]

## <a name="enable-authentication"></a><a name="enableauth"></a>Aktivieren der Authentifizierung

[!INCLUDE [enable authentication](../../includes/vpn-gateway-vwan-openvpn-enable-auth.md)]

## <a name="configure-sign-in-settings"></a><a name="enablesign"></a>Konfigurieren von Anmeldeeinstellungen

[!INCLUDE [sign in](../../includes/vpn-gateway-vwan-openvpn-sign-in.md)]

## <a name="option-1---per-user-access"></a><a name="peruser"></a>Option 1: Zugriff pro Benutzer

[!INCLUDE [per user](../../includes/vpn-gateway-vwan-openvpn-per-user.md)]

## <a name="option-2---conditional-access"></a><a name="conditional"></a>Option 2: bedingter Zugriff

[!INCLUDE [conditional access](../../includes/vpn-gateway-vwan-openvpn-conditional.md)]

## <a name="next-steps"></a>Nächste Schritte

Um eine Verbindung mit Ihrem virtuellen Netzwerk herzustellen, müssen Sie ein VPN-Clientprofil erstellen und konfigurieren. Entsprechende Informationen finden Sie unter [Konfigurieren eines VPN-Clients für P2S-VPN-Verbindungen](openvpn-azure-ad-client.md).