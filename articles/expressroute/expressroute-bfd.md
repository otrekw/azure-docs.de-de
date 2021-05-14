---
title: 'Azure ExpressRoute: Konfigurieren von BFD'
description: Dieser Artikel enthält Anweisungen zum Konfigurieren von BFD (Bidirectional Forwarding Detection) über das private Peering einer ExpressRoute-Leitung.
services: expressroute
author: duongau
ms.service: expressroute
ms.topic: article
ms.date: 12/14/2020
ms.author: duau
ms.openlocfilehash: 1ad91b00c14744ab1f0cbb414defeaffb85559fe
ms.sourcegitcommit: 4a54c268400b4158b78bb1d37235b79409cb5816
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2021
ms.locfileid: "108127789"
---
# <a name="configure-bfd-over-expressroute"></a>Konfigurieren von BFD über ExpressRoute

ExpressRoute unterstützt Bidirectional Forwarding Detection (BFD) sowohl über privates als auch Microsoft-Peering. Wenn Sie BFD über ExpressRoute aktivieren, können Sie die Erkennung von Verbindungsfehlern zwischen MSEE-Geräten (Microsoft Enterprise Edge) und den Routern beschleunigen, auf denen die ExpressRoute-Leitung konfiguriert wird (CE/PE). Sie können ExpressRoute über Ihre Edgeroutinggeräte oder über Edgeroutinggeräte von Partnern konfigurieren (wenn Sie den verwalteten Layer 3-Verbindungsdienst verwenden). In diesem Dokument werden die Notwendigkeit von BFD und die Vorgehensweise zur Aktivierung von BFD über ExpressRoute erläutert.

## <a name="need-for-bfd"></a>Notwendigkeit von BFD

Das folgende Diagramm zeigt den Vorteil der Aktivierung von BFD über die ExpressRoute-Leitung: [![1]][1]

Sie können die ExpressRoute-Leitung entweder über Layer 2-Verbindungen oder über verwaltete Layer 3-Verbindungen aktivieren. In beiden Fällen ist die übergeordnete BGP-Sitzung für die Erkennung von Verbindungsfehlern im Pfad verantwortlich, wenn sich im ExpressRoute-Verbindungspfad mehrere Layer 2-Geräte befinden.

Auf den MSEE-Geräten sind die BGP-Werte „keep-alive“ und „hold-time“ in der Regel mit 60 bzw. 180 Sekunden konfiguriert. Aus diesem Grund kann es bei einem Verbindungsfehler bis zu drei Minuten dauern, um den Fehler zu erkennen und den Datenverkehr über eine alternative Verbindung weiterzuleiten.

Sie können die BGP-Zeitgeber steuern, indem Sie auf Ihrem Edgepeeringgerät niedrigere Werte für „keep-alive“ und „hold-time“ konfigurieren. Wenn auf den beiden Peeringgeräten nicht dieselben BGP-Zeitgeberwerte verwendet werden, wird die BGP-Sitzung mit dem kleineren Wert festgelegt. Der BGP-Wert für „keep-alive“ kann auf nur drei Sekunden, der Wert für „hold-time“ auf nur zehn Sekunden festgelegt werden. Das Festlegen eines sehr aggressiven BGP-Zeitgeberwerts wird nicht empfohlen, weil das Protokoll prozessintensiv ist.

In diesem Szenario kann BFD helfen. Mit BFD können Verbindungsfehler mit geringem Mehraufwand in einem Zeitintervall von unter 1 Sekunde erkannt werden. 


## <a name="enabling-bfd"></a>Aktivieren von BFD

BFD wird standardmäßig unter allen neu erstellten privaten ExpressRoute-Peeringschnittstellen auf den MSEE-Geräten konfiguriert. Um BFD zu aktivieren, müssen Sie das Feature nur auf Ihren primären und sekundären Geräten konfigurieren. Die Konfiguration von BFD erfolgt in zwei Schritten. Sie konfigurieren BFD in der Schnittstelle und verknüpfen das Feature anschließend mit der BGP-Sitzung.

Eine CE/PE-Beispielkonfiguration (unter Verwendung von Cisco IOS XE) ist unten dargestellt. 

```console
interface TenGigabitEthernet2/0/0.150
   description private peering to Azure
   encapsulation dot1Q 15 second-dot1q 150
   ip vrf forwarding 15
   ip address 192.168.15.17 255.255.255.252
   bfd interval 300 min_rx 300 multiplier 3


router bgp 65020
   address-family ipv4 vrf 15
      network 10.1.15.0 mask 255.255.255.128
      neighbor 192.168.15.18 remote-as 12076
      neighbor 192.168.15.18 fall-over bfd
      neighbor 192.168.15.18 activate
      neighbor 192.168.15.18 soft-reconfiguration inbound
   exit-address-family
```

>[!NOTE]
>Zum Aktivieren von BFD unter einem bereits vorhandenen privaten Peering müssen Sie das Peering zurücksetzen. Informationen dazu finden Sie unter [Zurücksetzen von ExpressRoute-Peerings][ResetPeering].
>

## <a name="bfd-timer-negotiation"></a>Aushandlung des BFD-Zeitgebers

Zwischen BFD-Peers bestimmt der langsamere der beiden Peers die Übertragungsrate. BFD-Übertragung auf MSEE-Geräten/Empfangsintervalle werden auf 300 Millisekunden festgelegt. In bestimmten Szenarien kann das Intervall auf einen höheren Wert von 750 Millisekunden festgelegt werden. Durch Konfigurieren eines höheren Werts können Sie längere Intervalle erzwingen, eine Verkürzung ist jedoch nicht möglich.

>[!NOTE]
>Wenn Sie georedundante ExpressRoute-Leitungen konfiguriert haben oder eine S2S-IPsec-VPN-Verbindung als Backup verwenden. Die Aktivierung von BFD hilft bei einem schnelleren Failover nach einem ExpressRoute-Konnektivitätsfehler. 
>

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen oder Hilfe finden Sie unter den folgenden Links:

- [Erstellen und Ändern einer ExpressRoute-Verbindung][CreateCircuit]
- [Erstellen und Ändern des Routings für eine ExpressRoute-Verbindung][CreatePeering]

<!--Image References-->
[1]: ./media/expressroute-bfd/bfd-need.png "BFD beschleunigt die Zeit zur Erkennung von Verbindungsfehlern"

<!--Link References-->
[CreateCircuit]: ./expressroute-howto-circuit-portal-resource-manager.md
[CreatePeering]: ./expressroute-howto-routing-portal-resource-manager.md
[ResetPeering]: ./expressroute-howto-reset-peering.md