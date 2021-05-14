---
title: Konfigurieren von OpenVPN-Clients für P2S-VPN-Gateways
titleSuffix: Azure VPN Gateway
description: Hier erfahren Sie, wie Sie OpenVPN-Clients für Azure VPN Gateway konfigurieren. Dieser Artikel unterstützt Sie beim Konfigurieren von Windows-, Linux-, iOS- und Mac-Clients.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: how-to
ms.date: 04/29/2021
ms.author: cherylmc
ms.openlocfilehash: 5a7b125ef89d5d598e22fc4458b5b1c7f8c4a41c
ms.sourcegitcommit: fc9fd6e72297de6e87c9cf0d58edd632a8fb2552
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/30/2021
ms.locfileid: "108289352"
---
# <a name="configure-openvpn-clients-for-azure-vpn-gateway"></a>Konfigurieren von OpenVPN-Clients für Azure VPN Gateway

Dieser Artikel hilft Ihnen beim Konfigurieren von Clients mit dem **OpenVPN&reg;-Protokoll**.

## <a name="before-you-begin"></a>Voraussetzungen

Vergewissern Sie sich, dass Sie die Schritte zum Konfigurieren von OpenVPN für Ihr VPN-Gateway abgeschlossen haben. Einzelheiten finden Sie unter [Konfigurieren von OpenVPN für Azure VPN Gateway](vpn-gateway-howto-openvpn.md).

[!INCLUDE [configuration steps](../../includes/vpn-gateway-vwan-config-openvpn-clients.md)]

## <a name="next-steps"></a>Nächste Schritte

Wenn die VPN-Clients Zugriff auf Ressourcen in einem anderen VNET haben sollen, führen Sie die Anweisungen im Artikel [Konfigurieren einer VNET-zu-VNET-VPN-Gatewayverbindung](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md) aus, um eine VNET-zu-VNET-Verbindung einzurichten. Stellen Sie sicher, dass BGP (Border Gateway Protocol) für die Gateways und die Verbindungen aktiviert ist, da andernfalls kein Datenverkehr fließt.

**„OpenVPN“ ist eine Marke von OpenVPN Inc.**
