---
title: 'Azure-VPN Gateway: Informationen zu P2S VPN-Clientprofilen'
description: Diese Informationen unterstützen Sie bei der Arbeit mit der Clientprofildatei.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: article
ms.date: 03/17/2020
ms.author: cherylmc
ms.openlocfilehash: b78d32dbb7b4e0d9a3b13ff741e6e38c12be0e62
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "79528505"
---
# <a name="about-p2s-vpn-client-profiles"></a>Informationen zu P2S VPN-Clientprofilen

Die heruntergeladene Profildatei enthält Informationen, die zum Konfigurieren einer VPN-Verbindung erforderlich sind. Dieser Artikel hilft Ihnen beim Abrufen und Verstehen der Informationen, die für ein VPN-Clientprofil erforderlich sind.

[!INCLUDE [client profiles](../../includes/vpn-gateway-vwan-vpn-profile-download.md)]

* Der Ordner **OpenVPN** enthält das Profil *ovpn*, das geändert werden muss, um den Schlüssel und das Zertifikat einzuschließen. Weitere Informationen finden Sie unter [Konfigurieren von OpenVPN-Clients für Azure VPN Gateway](vpn-gateway-howto-openvpn-clients.md#windows). Wenn Azure AD Authentifizierung auf dem VPN-Gateway ausgewählt ist, ist dieser Ordner in der ZIP-Datei nicht vorhanden. Stattdessen befindet sich die Datei „azurevpnconfig.xml“ im Ordner „AzureVPN“.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Point-to-Site-VPN-Verbindungen finden Sie unter [Informationen zu Point-to-Site-VPN](point-to-site-about.md).
