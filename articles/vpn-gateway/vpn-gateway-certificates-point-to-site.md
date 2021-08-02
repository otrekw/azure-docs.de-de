---
title: 'Generieren und Exportieren von Zertifikaten für P2S: PowerShell'
titleSuffix: Azure VPN Gateway
description: Erfahren Sie, wie Sie ein selbstsigniertes Stammzertifikat erstellen, einen öffentlichen Schlüssel exportieren und Clientzertifikate für VPN Gateway Point-to-Site-Verbindungen generieren.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: how-to
ms.date: 06/03/2021
ms.author: cherylmc
ms.openlocfilehash: 48240793cee233d8e97ab79e6421b02eddc86f23
ms.sourcegitcommit: 70ce9237435df04b03dd0f739f23d34930059fef
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2021
ms.locfileid: "111527566"
---
# <a name="generate-and-export-certificates-for-point-to-site-using-powershell"></a>Generieren und Exportieren von Zertifikaten für Punkt-zu-Standort-Verbindungen mithilfe von PowerShell

Punkt-zu-Standort-Verbindungen verwenden Zertifikate zur Authentifizierung. In diesem Artikel wird beschrieben, wie Sie mithilfe von PowerShell unter Windows 10 oder Windows Server 2016 ein selbstsigniertes Stammzertifikat erstellen und Clientzertifikate generieren. Anleitungen für andere Zertifikate finden Sie unter [Zertifikate – Linux](vpn-gateway-certificates-point-to-site-linux.md) oder [Zertifikate – MakeCert](vpn-gateway-certificates-point-to-site-makecert.md).

Die Schritte in diesem Artikel gelten für Windows 10 oder Windows Server 2016. Die zum Generieren von Zertifikaten verwendeten PowerShell-Cmdlets sind Teil des Betriebssystems und können in anderen Versionen von Windows nicht ausgeführt werden. Der Windows 10- oder Windows Server 2016-Computer wird lediglich zum Generieren der Zertifikate benötigt. Nachdem die Zertifikate generiert wurden, können sie hochgeladen oder unter jedem unterstützten Clientbetriebssystem installiert werden.

Wenn Sie keinen Zugriff auf einen Windows 10- oder Windows Server 2016-Computer haben, können Sie Zertifikate mit [MakeCert](vpn-gateway-certificates-point-to-site-makecert.md) generieren. Unabhängig davon, mit welcher Methode sie generiert wurden, können die Zertifikate unter jedem [unterstützten](vpn-gateway-howto-point-to-site-resource-manager-portal.md#faq) Clientbetriebssystem installiert werden.

[!INCLUDE [generate and export certificates](../../includes/vpn-gateway-generate-export-certificates-include.md)]

## <a name="install-an-exported-client-certificate"></a><a name="install"></a>Installieren eines exportierten Clientzertifikats

Jeder Client, der über eine P2S-Verbindung eine Verbindung mit dem VNet herstellt, erfordert, dass ein Clientzertifikat lokal installiert ist.

Informationen zum Installieren eines Clientzertifikats finden Sie unter [Installieren eines Clientzertifikats für Point-to-Site-Verbindungen mit Azure-Zertifikatauthentifizierung](point-to-site-how-to-vpn-client-install-azure-cert.md).

## <a name="next-steps"></a>Nächste Schritte

Setzen Sie die Punkt-zu-Standort-Konfiguration fort.

* Schritte für das **Resource Manager**-Bereitstellungsmodell finden Sie unter [Konfigurieren einer Point-to-Site-Verbindung mit einem VNET unter Verwendung der Zertifikatauthentifizierung: Azure-Portal](vpn-gateway-howto-point-to-site-resource-manager-portal.md).
* Schritte für das **klassische** Bereitstellungsmodell finden Sie unter [Konfigurieren einer Point-to-Site-Verbindung mit einem VNet über das Azure-Portal](vpn-gateway-howto-point-to-site-classic-azure-portal.md) (klassisch).