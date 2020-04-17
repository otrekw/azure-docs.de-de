---
title: 'Informationen zu Azure Edge Zones: Vorschau'
description: Erfahren Sie mehr über Edgecomputing-Angebote von Microsoft.
services: vnf-manager
author: ganesr
ms.service: vnf-manager
ms.topic: article
ms.date: 04/02/2020
ms.author: ganesr
ms.openlocfilehash: aaa849633591bfd34a9fca026c820ec2f9137844
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2020
ms.locfileid: "81410837"
---
# <a name="about-azure-edge-zones---preview"></a>Informationen zu Azure Edge Zones: Vorschau

Azure Edge Zones bezeichnet eine Gruppe von Angeboten von Microsoft Azure, die eine Datenverarbeitung in der Nähe des Benutzers ermöglicht. Sie können virtuelle Computer (VMs), Container und andere ausgewählte Azure-Dienste in Edge Zones bereitstellen, um die Anforderungen von Anwendungen hinsichtlich niedriger Wartezeit und hohem Durchsatz zu erfüllen.

Zu den typischen Anwendungsszenarien für Edge Zones gehören:

- Befehl und Steuerung in der Robotik in Echtzeit
- Analysen und Rückschlüsse in Echtzeit mit künstlicher Intelligenz und maschinellem Lernen
- Maschinelles Sehen
- Remoterendering für Mixed Reality und VDI-Szenarien
- Immersives Gaming für mehrere Spieler
- Medienstreaming und Inhaltsbereitstellung
- Überwachung und Sicherheit

Azure Edge Zones gibt es in drei verschiedenen Ausführungen:

- Azure Edge Zones
- Azure Edge Zones mit Netzbetreiber
- Azure Private Edge Zones

## <a name="azure-edge-zones"></a><a name="edge-zones"></a>Azure Edge Zones

![Edge Zones](./media/edge-zones-overview/edge-zones.png "Edge Zones")

Azure Edge Zones sind kleine Ausläufer von Azure, die sich in Bevölkerungszentren befinden, die weit von Azure-Regionen entfernt sind. Azure Edge Zones unterstützen virtuelle Computer, Container und einen ausgewählten Satz von Azure-Diensten, mit denen Sie wartezeitempfindliche und durchsatzintensive Anwendungen in der Nähe der Endbenutzer ausführen können. Azure Edge Zones sind Teil des globalen Microsoft-Netzwerks und bieten sichere, zuverlässige Verbindungen mit hoher Bandbreite zwischen Anwendungen, die in der Edge Zone in der Nähe des Benutzers ausgeführt werden, und dem vollständigen Satz von Azure-Diensten, die in den Azure-Regionen ausgeführt werden. Azure Edge Zones sind im Besitz von Microsoft und werden von Microsoft betrieben. Sie können dieselben Azure-Tools und dasselbe Portal zur Verwaltung und Bereitstellung von Diensten in den Edge Zones verwenden.

Typische Anwendungsfälle sind:

- Gaming und das Streaming von Spielen
- Medienstreaming und Inhaltsbereitstellung
- Analysen und Rückschlüsse in Echtzeit mit künstlicher Intelligenz und maschinellem Lernen
- Rendering für Mixed Reality

Azure Edge Zones werden in den folgenden Metropolen zur Verfügung gestellt:

- New York, NY
- Los Angeles, CA
- Miami, FL

Weitere Informationen [erhalten Sie beim Edge Zones-Team](https://aka.ms/EdgeZones).

## <a name="azure-edge-zones-with-carrier"></a><a name="carrier"></a>Azure Edge Zones mit Netzbetreiber

![Edge Zones mit Netzbetreiber](./media/edge-zones-overview/edge-carrier.png "Edge Zones mit Netzbetreiber")

Azure Edge Zones mit Netzbetreiber sind kleine Ausläufer von Azure, die in den Rechenzentren von Mobilfunkbetreibern in Ballungszentren platziert werden. Azure Edge Zones mit Netzbetreiberinfrastruktur befinden sich einen Hop vom 5G-Netz des Mobilfunkbetreibers entfernt und bieten eine Wartezeit von unter 10 Millisekunden für Anwendungen von Mobilgeräten. Azure Edge Zones mit Netzbetreiber werden in den Rechenzentren der Mobilfunkbetreiber bereitgestellt und sind an das globale Netzwerk von Microsoft angeschlossen. Sie bieten sichere und zuverlässige Verbindungen mit hoher Bandbreite zwischen Anwendungen, die in der Nähe des Benutzers ausgeführt werden, und dem vollständigen Satz von Azure-Diensten, die in den Azure-Regionen ausgeführt werden. Entwickler können dieselben vertrauten Tools zum Erstellen und Bereitstellen von Diensten in den Edge Zones verwenden.

Typische Anwendungsfälle sind:

- Gaming und das Streaming von Spielen
- Medienstreaming und Inhaltsbereitstellung
- Analysen und Rückschlüsse in Echtzeit mit künstlicher Intelligenz und maschinellem Lernen
- Rendering für Mixed Reality
- Verbundene Fahrzeuge
- Telemedizin

Edge Zones werden in Partnerschaft mit den folgenden Betreibern angeboten:

- AT&T (Atlanta, Dallas und Los Angeles)

## <a name="azure-private-edge-zones"></a><a name="private-edge-zones"></a>Azure Private Edge Zones

![Private Edge Zones](./media/edge-zones-overview/private-edge.png "Private Edge Zones")

Azure Private Edge Zones sind kleine Ausläufer von Azure, die lokal platziert werden. Azure Private Edge Zones basiert auf der [Azure Stack Edge](https://azure.microsoft.com/products/azure-stack/edge/)-Plattform und ermöglicht den Zugriff auf Computing- und Speicherdienste, die lokal bereitgestellt werden, mit geringer Wartezeit. Mit Private Edge Zones können Sie außerdem virtualisierte Netzwerkfunktionen (VNFs) wie mobile Paketkerne, Router, Firewalls und SD-WAN-Geräte sowie Anwendungen von Internetdienstanbietern als [von Azure verwaltete Anwendungen](https://azure.microsoft.com/services/managed-applications/) neben virtuellen Computern und Containern lokal bereitstellen. Azure Private Edge Zones umfasst eine cloudnative Orchestrierungslösung, mit der Sie den Lebenszyklus von virtualisierten Netzwerkfunktionen (VNF) und Anwendungen aus dem Azure-Portal verwalten können.

Mit Azure Private Edge Zones können Sie Anwendungen lokal entwickeln und bereitstellen, indem Sie dieselben vertrauten Tools verwenden, die auch für die Entwicklung und Bereitstellung von Anwendungen in Azure verwendet werden. Sie können auch private mobile Netze (privates LTE, privates 5G) und Sicherheitsfunktionen wie Firewalls betreiben und Ihre firmeninternen Netzwerke über mehrere Branches und Azure erweitern, indem Sie SD-WAN-Geräte auf denselben Private Edge Zone-Geräten verwenden und diese von Azure aus verwalten.

Typische Anwendungsfälle sind:

- Befehl und Steuerung in der Robotik in Echtzeit
- Analysen und Rückschlüsse in Echtzeit mit künstlicher Intelligenz und maschinellem Lernen
- Maschinelles Sehen
- Remoterendering für Mixed Reality und VDI-Szenarien
- Überwachung und Sicherheit

Wir verfügen über ein umfassendes Ökosystem von VNF-Anbietern, ISVs und MSP-Partnern, um End-to-End-Lösungen mit Private Edge Zones zu ermöglichen. Weitere Informationen [erhalten Sie beim Private Edge Zones-Team](https://aka.ms/EdgeZonesPartner).

## <a name="private-edge-zones---partners"></a><a name="private-edge-partners"></a>Private Edge Zones – Partner

![Private Edge Zone-Partner](./media/edge-zones-overview/partners.png "Private Edge Zone-Partner")

### <a name="virtualized-network-functions-vnfs"></a><a name="vnf"></a>Virtualisierte Netzwerkfunktionen (VNFs)

#### <a name="virtualized-evolved-packet-core-vepc-for-mobile-networks"></a><a name="vEPC"></a>Virtualized Evolved Packet Core (vEPC) für Mobilfunknetze

- [Bestätigte Netzwerke](https://www.affirmednetworks.com/)
- [Druid Software](https://www.druidsoftware.com/)
- [Expeto](https://www.expeto.io/)
- [Mavenir](https://mavenir.com/)
- [Metaswitch](https://www.metaswitch.com/)
- [Nokia Digital Automation Cloud](https://www.dac.nokia.com/)

#### <a name="mobile-radio-partners"></a><a name="mobile-radio"></a>Mobile Radio Partners

- [Commscope Ruckus](https://support.ruckuswireless.com/)

#### <a name="sd-wan-vendors"></a><a name="sdwan-vendors"></a>SD-WAN-Anbieter

- [NetFoundry](https://netfoundry.io/)
- [NuageNetworks von Nokia](https://www.nuagenetworks.net/)
- [VMware SD-WAN von Velocloud](https://www.velocloud.com/)

#### <a name="router-vendors"></a><a name="router-vendors"></a>Routeranbieter

- [Arista](https://www.arista.com/)

Weitere Informationen dazu, wie Sie Partner werden können, erhalten Sie beim [Private Edge Zones-Team](https://aka.ms/EdgeZonesPartner).

#### <a name="firewall-vendors"></a><a name="firewall-vendors"></a>Firewallanbieter

- Palo Alto Networks

### <a name="managed-solutions-providers---mobile-operators-and-global-system-integrators"></a><a name="msp-mobile"></a>Anbieter verwalteter Lösungen: Mobilfunkanbieter und globale Systemintegratoren

| Globale Systemintegratoren und Anbieter | Mobilfunkanbieter |
| --- | --- |
| Amdocs                       | Etisalat             |
| American Tower               | NTT Communications   |
| Century Link                 | Proximus             |
| Expeto                       | Rogers               |
| Federated Wireless           | SK Telecom           |
| Infosys                      | Telefonica           |
| Tech Mahindra                | Telstra              |
|        *                     | Vodafone             |

Weitere Informationen dazu, wie Sie Partner werden können, erhalten Sie beim [Private Edge Zones-Team](https://aka.ms/EdgeZonesPartner).

## <a name="private-edge-zones---solutions"></a><a name="solutions-private-edge"></a>Private Edge Zones: Lösungen

### <a name="private-mobile-network-on-private-edge-zones"></a><a name="private-mobile-private-edge"></a>Private Mobile Network in Private Edge Zones

![Private Mobile Network in Private Edge Zones](./media/edge-zones-overview/mobile-networks.png "Private Mobile Network in Private Edge Zones")

Sie können jetzt ein privates Mobilfunknetz in Private Edge Zones bereitstellen. Private Mobilfunknetze ermöglichen extrem niedrige Wartezeiten, hohe Kapazitäten und ein zuverlässiges und sicheres Drahtlosnetzwerk, das für geschäftskritische Anwendungen erforderlich ist. Private Mobilfunknetze können Szenarien wie die Steuerung und Kontrolle von fahrerlosen Transportsystemen (FTS) in einem Lagerhaus, Echtzeitkommunikation zwischen Robotern in einer intelligenten Fabrik und Augmented Reality-Anwendungen sowie Edge-Anwendungen für die virtuelle Realität ermöglichen.

Die vEPC-Netzwerkfunktion (Virtualized Evolved Packet Core) bildet das Kernstück eines privaten Mobilfunknetzes. Sie können jetzt einen vEPC in Private Edge Zones bereitstellen. Eine Liste der vEPC-Partner, die in Private Edge Zones verfügbar sind, finden Sie unter [ISVs für vEPC](#vEPC).

Die Bereitstellung einer privaten Mobilfunknetzlösung in Private Edge Zones erfordert weitere Komponenten wie mobile Zugangspunkte, SIM-Karten und andere VNFs wie Router. Der Zugriff auf lizenzierte oder nicht lizenzierte Frequenzen ist entscheidend für den Aufbau eines privaten Mobilfunknetzes. Zusätzlich benötigen Sie möglicherweise Hilfe bei der RF-Planung, dem physischen Layout, der Installation und dem Support. Eine Liste der Partner finden Sie unter [Mobilfunkpartner](#mobile-radio).

Microsoft stellt ein Partnerökosystem zur Verfügung, das bei allen Aspekten dieses Prozesses helfen kann – von der Planung des Netzwerks, dem Erwerb der erforderlichen Geräte, der Einrichtung der Hardware bis hin zur Verwaltung der Konfiguration von Azure. Mit einer Reihe von bewährten Partnern, die eng mit Microsoft zusammenarbeiten, können Sie sicher sein, dass die Lösung zuverlässig und benutzerfreundlich sein wird. Sie können sich auf Ihre wichtigsten Szenarien konzentrieren, während Sie sich beim Rest auf die Hilfe von Microsoft und seinen Partnern verlassen können.

### <a name="sd-wan-on-private-edge-zones"></a><a name="sdwan-private-edge"></a>SD-WAN in Private Edge Zones

![SD-WAN in Private Edge Zones](./media/edge-zones-overview/sd-wan.png "SD-WAN in Private Edge Zones")
 
Mit SD-WAN als Technologie können Sie Wide Area Networks (WAN) der Enterprise-Klasse mit erhöhter Bandbreite, Hochleistungszugang zur Cloud, Diensteinfügung, Zuverlässigkeit, Richtlinienverwaltung und umfassender Netzwerktransparenz erstellen. SD-WAN bietet eine problemlose Zweigstellenkonnektivität, die von redundanten zentralen Controllern orchestriert wird, bei geringeren Betriebskosten.
Mit SD-WAN in Private Edge Zones können Sie vom auf die Investitionskosten fokussierten Modell zu einem Software-as-a-Service-Modell (SaaS) übergehen, um IT-Budgets zu reduzieren. Sie können einen SD-WAN-Partner Ihrer Wahl als Orchestrator oder Controller einsetzen, um neue Dienste zu aktivieren und diese sofort im gesamten Netzwerk zu verbreiten.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen erhalten Sie bei den folgenden Teams:

* [Edge Zones-Team](https://aka.ms/EdgeZones)
* [Private Edge Zones-Team: Partner werden](https://aka.ms/EdgeZonesPartner)
