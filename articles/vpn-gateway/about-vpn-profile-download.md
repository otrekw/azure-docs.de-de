---
title: 'Azure-VPN Gateway: Informationen zu P2S VPN-Clientprofilen'
description: Diese Informationen unterstützen Sie bei der Arbeit mit der Clientprofildatei.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: article
ms.date: 05/13/2020
ms.author: cherylmc
ms.openlocfilehash: 4526611fa8b80a7b97ee7317f5e285c50f05b0de
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2020
ms.locfileid: "83650666"
---
# <a name="about-p2s-vpn-client-profiles"></a>Informationen zu P2S VPN-Clientprofilen

Die heruntergeladene Profildatei enthält Informationen, die zum Konfigurieren einer VPN-Verbindung erforderlich sind. Dieser Artikel hilft Ihnen beim Abrufen und Verstehen der Informationen, die für ein VPN-Clientprofil erforderlich sind.

[!INCLUDE [client profiles](../../includes/vpn-gateway-vwan-vpn-profile-download.md)]

* Der Ordner **OpenVPN** enthält das Profil *ovpn*, das geändert werden muss, um den Schlüssel und das Zertifikat einzuschließen. Weitere Informationen finden Sie unter [Konfigurieren von OpenVPN-Clients für Azure VPN Gateway](vpn-gateway-howto-openvpn-clients.md#windows). Wenn die Azure AD-Authentifizierung auf dem VPN-Gateway ausgewählt ist, ist dieser Ordner in der ZIP-Datei nicht vorhanden. Navigieren Sie stattdessen zum Ordner „AzureVPN“, und suchen Sie nach der Datei „azurevpnconfig.xml“.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Point-to-Site-VPN-Verbindungen finden Sie unter [Informationen zu Point-to-Site-VPN](point-to-site-about.md).
