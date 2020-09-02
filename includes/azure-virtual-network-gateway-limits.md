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
ms.openlocfilehash: 01796985a05f88b59786be7bbe0b06907cf0bc25
ms.sourcegitcommit: b33c9ad17598d7e4d66fe11d511daa78b4b8b330
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/25/2020
ms.locfileid: "88854110"
---
| Resource                                | Begrenzung        |
|-----------------------------------------|------------------------------|
| VNET-Adresspräfixe                   | 600 pro VPN-Gateway          |
| Aggregierte BGP-Routen                    | 4\.000 pro VPN-Gateway        |
| Adresspräfixe für das lokale Netzwerkgateway  | 1\.000 pro lokalem Netzwerkgateway               |
| S2S-Verbindungen                         | [Abhängig von der Gateway-SKU](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways#gwsku)|
| P2S-Verbindungen                         | [Abhängig von der Gateway-SKU](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways#gwsku) |
| P2S-Routengrenzwert: IKEv2                 | 256 außerhalb von Windows **/** 25 unter Windows           |
| P2S-Routengrenzwert: OpenVPN               | 1000                         |
| Maximal flows                              | 100.000 für VpnGw1/AZ **/** 512.000 für VpnGw2-4/AZ|