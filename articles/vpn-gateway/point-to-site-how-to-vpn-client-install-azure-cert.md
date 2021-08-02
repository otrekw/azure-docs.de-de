---
title: Installieren eines Point-to-Site-Clientzertifikats
titleSuffix: Azure VPN Gateway
description: Hier erfahren Sie, wie Sie Clientzertifikate für die P2S-Zertifikatauthentifizierung unter Windows, Mac und Linux installieren.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: how-to
ms.date: 06/03/2021
ms.author: cherylmc
ms.openlocfilehash: d4ab1009b38d250a39455a9e8a470cd7f1156793
ms.sourcegitcommit: 70ce9237435df04b03dd0f739f23d34930059fef
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2021
ms.locfileid: "111527104"
---
# <a name="install-client-certificates-for-p2s-certificate-authentication-connections"></a>Installieren eines Clientzertifikats für die Zertifikatauthentifizierung bei P2S-Verbindungen

Wenn ein P2S-VPN-Gateway so konfiguriert ist, dass es eine Zertifikatauthentifizierung erfordert, muss auf jedem Client ein lokales Clientzertifikat installiert sein. Sie können ein Clientzertifikat entweder mithilfe eines selbstsignierten Stammzertifikats oder mithilfe eines Stammzertifikats, das mit einer Lösung einer Unternehmenszertifizierungsstelle generiert wurde, erstellen. 

Wenn Sie ein Clientzertifikat generieren, wird das Zertifikat in der Regel automatisch auf dem Clientcomputer installiert, von dem es generiert wurde. Wenn Sie von einem anderen Clientcomputer aus eine Verbindung mit Ihrem VNet herstellen möchten, müssen Sie ein Clientzertifikat auf dem Computer installieren, von dem aus Sie eine Verbindung herstellen. Dies ist zusätzlich zur Konfiguration des VPN-Clients auf diesem Computer erforderlich.

Sie können mehrere Methoden verwenden, um selbstsignierte Zertifikate zu generieren und zu exportieren. Weitere Informationen finden Sie in den folgenden Artikeln:

* [PowerShell](vpn-gateway-certificates-point-to-site.md)
* [MakeCert](vpn-gateway-certificates-point-to-site-makecert.md)
* [Linux](vpn-gateway-certificates-point-to-site-linux.md) 

## <a name="windows"></a><a name="installwin"></a>Windows

[!INCLUDE [Install on Windows](../../includes/vpn-gateway-certificates-install-client-cert-include.md)]

## <a name="mac"></a><a name="installmac"></a>Mac

>[!NOTE]
>Mac-VPN-Clients werden nur für das Resource Manager-Bereitstellungsmodell unterstützt. Für das klassische Bereitstellungsmodell werden sie nicht unterstützt.
>
>

[!INCLUDE [Install on Mac](../../includes/vpn-gateway-certificates-install-mac-client-cert-include.md)]

## <a name="linux"></a><a name="installlinux"></a>Linux

Das Linux-Clientzertifikat wird auf dem Client als Teil der Clientkonfiguration installiert. Anweisungen finden Sie unter [Clientkonfiguration – Linux](point-to-site-vpn-client-configuration-azure-cert.md#linuxinstallcli).

## <a name="next-steps"></a>Nächste Schritte

Fahren Sie mit den Point-to-Site-Konfigurationsschritten zum [Erstellen und Installieren von VPN-Clientkonfigurationsdateien](point-to-site-vpn-client-configuration-azure-cert.md) fort.
