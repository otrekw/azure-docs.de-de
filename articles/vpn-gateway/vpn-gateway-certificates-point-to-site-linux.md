---
title: 'Generieren und Exportieren von Zertifikaten für Point-to-Site-Verbindungen: CLI'
description: Hier erfahren Sie, wie Sie ein selbstsigniertes Stammzertifikat erstellen, den öffentlichen Schlüssel exportieren und Clientzertifikate mit der Linux-Befehlszeilenschnittstelle (strongSwan) generieren.
titleSuffix: Azure VPN Gateway
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: how-to
ms.date: 09/02/2020
ms.author: alzam
ms.openlocfilehash: e26c73aba047c5dfb1c1169ec2d100519c87f45f
ms.sourcegitcommit: fc9fd6e72297de6e87c9cf0d58edd632a8fb2552
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/30/2021
ms.locfileid: "108291242"
---
# <a name="generate-and-export-certificates"></a>Generieren und Exportieren von Zertifikaten

Punkt-zu-Standort-Verbindungen verwenden Zertifikate zur Authentifizierung. In diesem Artikel erfahren Sie, wie Sie mithilfe der Linux-Befehlszeilenschnittstelle und strongSwan ein selbstsigniertes Stammzertifikat erstellen und Clientzertifikate generieren. Anleitungen für andere Zertifikate finden Sie in den Artikeln zu [PowerShell](vpn-gateway-certificates-point-to-site.md) und [MakeCert](vpn-gateway-certificates-point-to-site-makecert.md). Informationen zum Installieren von strongSwan mithilfe der grafischen Benutzeroberfläche anstelle der CLI finden Sie in den Schritten im Artikel [Clientkonfiguration](point-to-site-vpn-client-configuration-azure-cert.md#install).

## <a name="install-strongswan"></a>Installieren von strongSwan

[!INCLUDE [strongSwan Install](../../includes/vpn-gateway-strongswan-install-include.md)]

## <a name="generate-and-export-certificates"></a>Generieren und Exportieren von Zertifikaten

[!INCLUDE [strongSwan certificates](../../includes/vpn-gateway-strongswan-certificates-include.md)]

## <a name="next-steps"></a>Nächste Schritte

Fahren Sie mit Ihrer Point-to-Site-Konfiguration zum [Erstellen und Installieren von VPN-Clientkonfigurationsdateien](point-to-site-vpn-client-configuration-azure-cert.md#linuxinstallcli) fort.
