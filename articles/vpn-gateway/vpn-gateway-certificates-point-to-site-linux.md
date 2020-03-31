---
title: 'Generieren und Exportieren von Zertifikaten für Point-to-Site-Verbindungen: Linux: Befehlszeilenschnittstelle (CLI)'
description: Erstellen eines selbstsignierten Stammzertifikats, Exportieren des öffentlichen Schlüssels und Generieren von Clientzertifikaten mit der Linux-Befehlszeilenschnittstelle (strongSwan)
titleSuffix: Azure VPN Gateway
services: vpn-gateway
author: anzaman
ms.service: vpn-gateway
ms.topic: article
ms.date: 08/14/2019
ms.author: alzam
ms.openlocfilehash: a0f996ff2805da4dd5af400642eef2506c228d33
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "75779755"
---
# <a name="generate-and-export-certificates"></a>Generieren und Exportieren von Zertifikaten

Punkt-zu-Standort-Verbindungen verwenden Zertifikate zur Authentifizierung. In diesem Artikel erfahren Sie, wie Sie mithilfe der Linux-Befehlszeilenschnittstelle und strongSwan ein selbstsigniertes Stammzertifikat erstellen und Clientzertifikate generieren. Anleitungen für andere Zertifikate finden Sie in den Artikeln zu [PowerShell](vpn-gateway-certificates-point-to-site.md) und [MakeCert](vpn-gateway-certificates-point-to-site-makecert.md). Informationen zum Installieren von strongSwan mithilfe der grafischen Benutzeroberfläche anstelle der CLI finden Sie in den Schritten im Artikel [Clientkonfiguration](point-to-site-vpn-client-configuration-azure-cert.md#install).

## <a name="install-strongswan"></a>Installieren von strongSwan

[!INCLUDE [strongSwan Install](../../includes/vpn-gateway-strongswan-install-include.md)]

## <a name="generate-and-export-certificates"></a>Generieren und Exportieren von Zertifikaten

[!INCLUDE [strongSwan certificates](../../includes/vpn-gateway-strongswan-certificates-include.md)]

## <a name="next-steps"></a>Nächste Schritte

Fahren Sie mit Ihrer Point-to-Site-Konfiguration zum [Erstellen und Installieren von VPN-Clientkonfigurationsdateien](point-to-site-vpn-client-configuration-azure-cert.md#linuxinstallcli) fort.
