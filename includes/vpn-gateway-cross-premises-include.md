---
title: include file
description: include file
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 12/17/2020
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: b283add6cff1400cc3141f4fba3f0f3939ee34aa
ms.sourcegitcommit: ad677fdb81f1a2a83ce72fa4f8a3a871f712599f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/17/2020
ms.locfileid: "97665067"
---
|  | **Punkt-zu-Standort** | **Standort-zu-Standort** | **ExpressRoute** |
| --- | --- | --- | --- |
| **Von Azure unterstützte Dienste** |Cloud Services und Virtual Machines |Cloud Services und Virtual Machines |[Dienstliste](../articles/expressroute/expressroute-faqs.md#supported-services) |
| **Typische Bandbreiten** |Basiert auf der Gateway-SKU |In der Regel insgesamt < 1 GBit/s |50 MBit/s, 100 MBit/s, 200 MBit/s, 500 MBit/s, 1 GBit/s, 2 GBit/s, 5 GBit/s, 10 GBit/s |
| **Unterstützte Protokolle** |Secure Sockets Tunneling Protocol (SSTP), OpenVPN und IPsec |IPsec |Direkte Verbindung über VLANs, VPN-Technologien des NSP (MPLS, VPLS ...) |
| **Routing** |RouteBased (dynamisch) |Wir unterstützen PolicyBased-VPNs (statisches Routing) und RouteBased-VPNs (dynamisches Routing) |BGP |
| **Verbindungsstabilität** |Aktiv-passiv |Aktiv-passiv oder aktiv-aktiv |Aktiv-aktiv |
| **Typisches Anwendungsbeispiel** |Schützen des Zugriffs auf virtuelle Azure-Netzwerke für Remotebenutzer |Entwicklungs-, Test- und Laborszenarien und kleinere bis mittlere Produktionsworkloads für Clouddienste und virtuelle Computer |Zugriff auf alle Azure-Dienste (überprüfte Liste), unternehmensbezogene und wichtige Workloads, Sicherung, Big Data, Azure als DR-Standort |
| **SLA** |[SLA](https://azure.microsoft.com/support/legal/sla/) |[SLA](https://azure.microsoft.com/support/legal/sla/) |[SLA](https://azure.microsoft.com/support/legal/sla/) |
| **Preise** |[Preise](https://azure.microsoft.com/pricing/details/vpn-gateway/) |[Preise](https://azure.microsoft.com/pricing/details/vpn-gateway/) |[Preise](https://azure.microsoft.com/pricing/details/expressroute/) |
| **Technische Dokumentation** |[VPN Gateway-Dokumentation](https://azure.microsoft.com/documentation/services/vpn-gateway/) |[VPN Gateway-Dokumentation](https://azure.microsoft.com/documentation/services/vpn-gateway/) |[ExpressRoute-Dokumentation](https://azure.microsoft.com/documentation/services/expressroute/) |
| **Häufig gestellte Fragen** |[Häufig gestellte Fragen zum VPN-Gateway](../articles/vpn-gateway/vpn-gateway-vpn-faq.md) |[Häufig gestellte Fragen zum VPN-Gateway](../articles/vpn-gateway/vpn-gateway-vpn-faq.md) |[ExpressRoute – FAQ](../articles/expressroute/expressroute-faqs.md) |
