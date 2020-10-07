---
title: 'Azure Virtual WAN: Benutzer-VPN-Clientprofile'
description: Diese Informationen unterstützen Sie bei der Arbeit mit der Clientprofildatei.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: how-to
ms.date: 09/22/2020
ms.author: cherylmc
ms.openlocfilehash: 116ec518f7788e620fc6bd177e535c8f6af99d10
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2020
ms.locfileid: "91267770"
---
# <a name="working-with-user-vpn-client-profiles"></a>Arbeiten mit Benutzer-VPN-Clientprofilen

Die heruntergeladene Profildatei enthält Informationen, die zum Konfigurieren einer VPN-Verbindung erforderlich sind. Dieser Artikel hilft Ihnen beim Abrufen und Verstehen der Informationen, die für ein Benutzer-VPN-Clientprofil erforderlich sind.

[!INCLUDE [client profiles](../../includes/vpn-gateway-vwan-vpn-profile-download.md)]

* Der Ordner **OpenVPN** enthält das Profil *ovpn*, das geändert werden muss, um den Schlüssel und das Zertifikat einzuschließen. Weitere Informationen finden Sie unter [Konfigurieren von OpenVPN-Clients für Azure VPN Gateway](../virtual-wan/howto-openvpn-clients.md#windows).

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Virtual WAN-Benutzer-VPN finden Sie unter [Tutorial: Erstellen einer Benutzer-VPN-Verbindung per Azure Virtual WAN](virtual-wan-point-to-site-portal.md).
