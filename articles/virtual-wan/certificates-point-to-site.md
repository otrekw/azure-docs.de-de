---
title: Generieren und Exportieren von Zertifikaten für Benutzer-VPN-Verbindungen | Azure Virtual WAN
description: Hier erfahren Sie, wie Sie ein selbstsigniertes Stammzertifikat erstellen, einen öffentlichen Schlüssel exportieren und Clientzertifikate für Virtual WAN-Benutzer-VPN-Verbindungen (Point-to-Site) mit PowerShell generieren.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: how-to
ms.date: 04/27/2021
ms.author: cherylmc
ms.openlocfilehash: d126f68ed1eebae9dda7ad957748cb3258b5ad89
ms.sourcegitcommit: 62e800ec1306c45e2d8310c40da5873f7945c657
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2021
ms.locfileid: "108165297"
---
# <a name="generate-and-export-certificates-for-user-vpn-connections"></a>Generieren und Exportieren von Zertifikaten für Benutzer-VPN-Verbindungen

Benutzer-VPN-Verbindungen (Point-to-Site) verwenden Zertifikate zur Authentifizierung. In diesem Artikel wird beschrieben, wie Sie mithilfe von PowerShell unter Windows 10 oder Windows Server 2016 ein selbstsigniertes Stammzertifikat erstellen und Clientzertifikate generieren.

Sie müssen die in diesem Artikel beschriebenen Schritte auf einem Computer unter Windows 10 oder Windows Server 2016 ausführen. Die zum Generieren von Zertifikaten verwendeten PowerShell-Cmdlets sind Teil des Betriebssystems und können in anderen Versionen von Windows nicht ausgeführt werden. Der Windows 10- oder Windows Server 2016-Computer wird lediglich zum Generieren der Zertifikate benötigt. Nachdem die Zertifikate generiert wurden, können sie hochgeladen oder unter jedem unterstützten Clientbetriebssystem installiert werden.

[!INCLUDE [Export public key](../../includes/vpn-gateway-generate-export-certificates-include.md)]

## <a name="next-steps"></a>Nächste Schritte

Weiter mit den [Virtual WAN-Schritten für VPN-Verbindungen von Benutzern](virtual-wan-about.md)
