---
title: Generieren und Exportieren von Zertifikaten für Benutzer-VPN-Verbindungen | Azure Virtual WAN
description: Hier erhalten Sie Informationen zum Erstellen selbstsignierter Stammzertifikate, Exportieren des öffentlichen Schlüssels und Generieren von Clientzertifikaten mit PowerShell unter Windows 10 oder Windows Server 2016.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 03/18/2020
ms.author: cherylmc
ms.openlocfilehash: 0303bac88f34c895a4a680cd5bff0e9d1513d2e5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "80059917"
---
# <a name="generate-and-export-certificates-for-user-vpn-connections"></a>Generieren und Exportieren von Zertifikaten für Benutzer-VPN-Verbindungen

Benutzer-VPN-Verbindungen (Point-to-Site) verwenden Zertifikate zur Authentifizierung. In diesem Artikel wird beschrieben, wie Sie mithilfe von PowerShell unter Windows 10 oder Windows Server 2016 ein selbstsigniertes Stammzertifikat erstellen und Clientzertifikate generieren.

Sie müssen die in diesem Artikel beschriebenen Schritte auf einem Computer unter Windows 10 oder Windows Server 2016 ausführen. Die zum Generieren von Zertifikaten verwendeten PowerShell-Cmdlets sind Teil des Betriebssystems und können in anderen Versionen von Windows nicht ausgeführt werden. Der Windows 10- oder Windows Server 2016-Computer wird lediglich zum Generieren der Zertifikate benötigt. Nachdem die Zertifikate generiert wurden, können sie hochgeladen oder unter jedem unterstützten Clientbetriebssystem installiert werden.

[!INCLUDE [Export public key](../../includes/vpn-gateway-generate-export-certificates-include.md)]

## <a name="next-steps"></a>Nächste Schritte

Weiter mit den [Virtual WAN-Schritten für VPN-Verbindungen von Benutzern](virtual-wan-about.md)
