---
title: 'Azure ExpressRoute: Konnektivitätsmodelle'
description: Überprüfen Sie die Konnektivität zwischen dem Netzwerk des Kunden, Microsoft Azure und Microsoft 365-Diensten. Kunden können MPLS-Anbieter, Cloud-Exchanges und Ethernet verwenden.
services: expressroute
author: duongau
ms.service: expressroute
ms.topic: conceptual
ms.date: 10/07/2020
ms.author: duau
ms.openlocfilehash: fb35a03b5dd8780445eb27f485966e3c3452feea
ms.sourcegitcommit: d103a93e7ef2dde1298f04e307920378a87e982a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/13/2020
ms.locfileid: "91978752"
---
# <a name="expressroute-connectivity-models"></a>ExpressRoute-Konnektivitätsmodelle
Sie können eine Verbindung zwischen Ihrem lokalen Netzwerk und der Microsoft-Cloud auf vier verschiedene Arten erstellen: [CloudExchange-Zusammenstellung](#CloudExchange), [Point-to-Point-Ethernet-Verbindung](#Ethernet), [Any-to-Any-Verbindung (IPVPN)](#IPVPN) und [ExpressRoute Direct](#Direct). Konnektivitätsanbieter können ein oder mehrere Konnektivitätsmodelle bereitstellen. Sie können zusammen mit Ihrem Konnektivitätsanbieter das Modell auswählen, das für Sie am besten geeignet ist.
<br><br>

:::image type="content" source="./media/expressroute-connectivity-models/expressroute-connectivity-models-diagram.png" alt-text="ExpressRoute-Konnektivitätsmodelle – Diagramm":::

## <a name="co-located-at-a-cloud-exchange"></a><a name="CloudExchange"></a>Über einen Cloud Exchange
Wenn Sie sich in einer Einrichtung mit einem Cloud Exchange befinden, können Sie virtuelle Querverbindungen zur Microsoft Cloud über den Ethernet Exchange des Co-Location-Anbieters bestellen. Co-Location-Anbieter stellen entweder Layer 2-Querverbindungen oder verwaltete Layer 3-Querverbindungen zwischen Ihrer Infrastruktur in der Co-Location-Einrichtung und der Microsoft Cloud bereit.

## <a name="point-to-point-ethernet-connections"></a><a name="Ethernet"></a>Point-to-Point-Ethernet-Verbindungen
Sie können Ihre lokalen Rechenzentren/Büros über Point-to-Point-Ethernet-Links mit der Microsoft Cloud verbinden. Point-to-Point-Ethernet-Anbieter können Layer 2-Verbindungen oder verwaltete Layer 3-Verbindungen zwischen Ihrem Standort und der Microsoft Cloud bereitstellen.

## <a name="any-to-any-ipvpn-networks"></a><a name="IPVPN"></a>Any-to-Any-Netzwerke (IPVPN)
Sie können Ihr WAN in die Microsoft Cloud integrieren. IPVPN-Anbieter (normalerweise MPLS VPN) bieten Any-to-Any-Konnektivität zwischen Ihren Niederlassungen und den Rechenzentren an. Die Microsoft Cloud kann mit Ihrem WAN verbunden werden, um sie wie eine normale Niederlassung erscheinen zu lassen. WAN-Anbieter stellen in der Regel verwaltete Layer 3-Konnektivität bereit. ExpressRoute-Funktionen und -Features sind für alle oben genannten Konnektivitätsmodelle identisch.

## <a name="direct-from-expressroute-sites"></a><a name="Direct"></a>Direkt von ExpressRoute-Standorten
Sie können sich direkt mit dem globalen Netzwerk von Microsoft verbinden, und zwar an strategisch über die ganze Welt verteilten Peeringstandorten. [ExpressRoute Direct](expressroute-erdirect-about.md) bietet duale Konnektivität mit 100 oder 10 GBit/s, die eine Aktiv/Aktiv-Konnektivität nach Maß unterstützt.

## <a name="next-steps"></a>Nächste Schritte
* Informieren Sie sich über ExpressRoute-Verbindungen und Routingdomänen. Siehe [ExpressRoute-Verbindungen und Routingdomänen](expressroute-circuit-peerings.md).
* Erfahren Sie mehr zu ExpressRoute-Funktionen. Siehe [ExpressRoute - Technische Übersicht](expressroute-introduction.md)
* Suchen Sie nach einem Service Provider. Siehe [ExpressRoute-Partner und Peeringstandorte](expressroute-locations.md).
* Stellen Sie sicher, dass alle Voraussetzungen erfüllt sind. Informationen finden Sie unter [ExpressRoute-Voraussetzungen](expressroute-prerequisites.md).
* Sehen Sie sich die Anforderungen für [Routing](expressroute-routing.md), [NAT](expressroute-nat.md) und [QoS](expressroute-qos.md) an.
* Konfigurieren Sie Ihre ExpressRoute-Verbindung.
  * [Erstellen Sie eine ExpressRoute-Verbindung.](expressroute-howto-circuit-portal-resource-manager.md)
  * [Konfigurieren des Routings](expressroute-howto-routing-portal-resource-manager.md)
  * [Verknüpfen eines VNet mit einer ExpressRoute-Verbindung](expressroute-howto-linkvnet-portal-resource-manager.md)