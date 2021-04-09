---
title: include file
description: include file
services: networking
author: anzaman
ms.service: VPN Gateway
ms.topic: include
ms.date: 08/25/2020
ms.author: alzam
ms.custom: include file
ms.openlocfilehash: 9fe9ef5549ced3b73d18d553fa0b62ec019684fe
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "95559629"
---
| Resource                                | Begrenzung        |
|-----------------------------------------|------------------------------|
| VNET-Adresspräfixe                   | 600 pro VPN-Gateway          |
| Aggregierte BGP-Routen                    | 4\.000 pro VPN-Gateway        |
| Adresspräfixe für das lokale Netzwerkgateway  | 1\.000 pro lokalem Netzwerkgateway               |
| S2S-Verbindungen                         | [Abhängig von der Gateway-SKU](../articles/vpn-gateway/vpn-gateway-about-vpngateways.md#gwsku)|
| P2S-Verbindungen                         | [Abhängig von der Gateway-SKU](../articles/vpn-gateway/vpn-gateway-about-vpngateways.md#gwsku) |
| P2S-Routengrenzwert: IKEv2                 | 256 außerhalb von Windows **/** 25 unter Windows           |
| P2S-Routengrenzwert: OpenVPN               | 1000                         |
| Maximal flows                              | 100.000 für VpnGw1/AZ **/** 512.000 für VpnGw2-4/AZ|