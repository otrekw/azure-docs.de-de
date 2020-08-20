---
title: Konfigurieren von OpenVPN-Clients für Azure VPN Gateway | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie OpenVPN für Azure VPN Gateway für Windows-, Linux- und Mac-Betriebssystemclients konfigurieren.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: how-to
ms.date: 12/12/2019
ms.author: cherylmc
ms.openlocfilehash: 92447a541726c1c11b7b10d6d52cf91cfc07f945
ms.sourcegitcommit: bfeae16fa5db56c1ec1fe75e0597d8194522b396
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/10/2020
ms.locfileid: "88036860"
---
# <a name="configure-openvpn-clients-for-azure-vpn-gateway"></a>Konfigurieren von OpenVPN-Clients für Azure VPN Gateway

Dieser Artikel hilft Ihnen beim Konfigurieren von Clients mit dem **OpenVPN&reg;-Protokoll**.

## <a name="before-you-begin"></a>Voraussetzungen

Vergewissern Sie sich, dass Sie die Schritte zum Konfigurieren von OpenVPN für Ihr VPN-Gateway abgeschlossen haben. Einzelheiten finden Sie unter [Konfigurieren von OpenVPN für Azure VPN Gateway](vpn-gateway-howto-openvpn.md).

[!INCLUDE [configuration steps](../../includes/vpn-gateway-vwan-config-openvpn-clients.md)]

## <a name="next-steps"></a>Nächste Schritte

Wenn die VPN-Clients Zugriff auf Ressourcen in einem anderen VNET haben sollen, führen Sie die Anweisungen im Artikel [Konfigurieren einer VNET-zu-VNET-VPN-Gatewayverbindung](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md) aus, um eine VNET-zu-VNET-Verbindung einzurichten. Stellen Sie sicher, dass BGP (Border Gateway Protocol) für die Gateways und die Verbindungen aktiviert ist, da andernfalls kein Datenverkehr fließt.

**„OpenVPN“ ist eine Marke von OpenVPN Inc.**
