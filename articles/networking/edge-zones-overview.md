---
title: Informationen zur Vorschau von Azure Edge Zone
description: 'In diesem Artikel erfahren Sie mehr über eines der Edgecomputing-Angebote von Microsoft: Azure Edge Zone.'
services: vnf-manager
author: cherylmc
ms.service: vnf-manager
ms.topic: article
ms.date: 01/13/2021
ms.author: cherylmc
ms.openlocfilehash: 04555303d5128db6c183d27a0c5fcb69063fdc28
ms.sourcegitcommit: 0aec60c088f1dcb0f89eaad5faf5f2c815e53bf8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/14/2021
ms.locfileid: "98185409"
---
# <a name="about-azure-edge-zone-preview"></a>Informationen zur Vorschau von Azure Edge Zone

Azure Edge Zone umfasst eine Gruppe von Angeboten aus Microsoft Azure, die eine Datenverarbeitung in der Nähe des Benutzers ermöglichen. Sie können virtuelle Computer (VMs), Container und andere ausgewählte Azure-Dienste in Edge Zones bereitstellen, um die Anforderungen von Anwendungen hinsichtlich niedriger Wartezeit und hohem Durchsatz zu erfüllen.

Zu den typischen Anwendungsszenarien für Edge Zones gehören:

- Echtzeitsteuerung von Robotern
- Analysen und Rückschlüsse in Echtzeit mit künstlicher Intelligenz und maschinellem Lernen
- Maschinelles Sehen
- Remoterendering für Mixed Reality- und VDI-Szenarios
- Immersives Gaming für mehrere Spieler
- Medienstreaming und Content Delivery
- Überwachung und Sicherheit

Es gibt drei Typen von Zonen in Azure Edge Zone:

- Azure Edge Zones
- Azure Edge Zones mit Netzbetreiber
- Azure Private Edge Zones

## <a name="azure-edge-zones"></a><a name="edge-zones"></a>Azure Edge Zones

![Azure Edge Zone-Zonen](./media/edge-zones-overview/edge-zones.png "Azure Edge Zones")

Azure Edge Zone-Zonen sind kleine Ausläufer des Azure-Netzwerks, die sich in Ballungsräumen befinden, die weit von den eigentlichen Azure-Regionen entfernt sind. Azure Edge Zone-Zonen unterstützen virtuelle Computer, Container und ausgewählte Azure-Dienste, mit denen Sie latenzempfindliche und durchsatzintensive Anwendungen in der Nähe der Endbenutzer ausführen können. Azure Edge Zone-Zonen gehören zu dem globalen Netzwerk von Microsoft. Sie bieten sichere und zuverlässige Konnektivität mit einer hohen Bandbreite zwischen Anwendungen, die in der Edgezone in der Nähe der Benutzer ausgeführt werden. Azure Edge Zone-Zonen gehören Microsoft und werden von Microsoft betrieben. Sie können dieselben Azure-Tools und dasselbe Portal verwenden, um Dienste in Azure Edge Zone-Zonen zu verwalten und bereitzustellen.

Typische Anwendungsfälle sind:

- Gaming und das Streaming von Spielen
- Medienstreaming und Content Delivery
- Analysen und Rückschlüsse in Echtzeit mit künstlicher Intelligenz und maschinellem Lernen
- Rendering für Mixed Reality

Azure Edge Zone-Zonen sind in den folgenden Metropolregionen verfügbar:

- New York, NY
- Los Angeles, CA
- Miami, FL

Weitere Informationen erhalten Sie beim [Supportteam für Azure Edge Zone-Zonen](https://aka.ms/EdgeZones).

## <a name="azure-edge-zones-with-carrier"></a><a name="carrier"></a>Azure Edge Zones mit Netzbetreiber

![Edge Zones mit Netzbetreiber](./media/edge-zones-overview/edge-carrier.png "Edge Zones mit Netzbetreiber")

Azure Edge Zone-Zonen mit Netzbetreiber sind kleine Ausläufer des Azure-Netzwerks, die in den Rechenzentren von Mobilfunkbetreibern in Ballungszentren platziert werden. Die Infrastruktur von Azure Edge Zone-Zonen mit Netzbetreiber befindet sich nur einen Hop vom 5G-Netzwerk des Mobilfunkanbieters entfernt. Aufgrund dieser Nähe beträgt die Latenz zwischen Anwendungen und mobilen Geräten weniger als 10 Millisekunden.

Azure Edge Zone-Zonen mit Netzbetreiber werden in den Rechenzentren von Mobilfunkbetreibern bereitgestellt und sind an das globale Netzwerk von Microsoft angebunden. Sie bieten sichere und zuverlässige Konnektivität mit einer hohen Bandbreite zwischen Anwendungen, die in der Nähe der Benutzer ausgeführt werden. Entwickler können dieselben vertrauten Tools zum Erstellen und Bereitstellen von Diensten in den Edge Zones verwenden.

Typische Anwendungsfälle sind:

- Gaming und das Streaming von Spielen
- Medienstreaming und Content Delivery
- Analysen und Rückschlüsse in Echtzeit mit künstlicher Intelligenz und maschinellem Lernen
- Rendering für Mixed Reality
- Verbundene Fahrzeuge
- Telemedizin

Edge Zones werden in Partnerschaft mit den folgenden Betreibern angeboten:

- AT&T (Atlanta, Dallas und Los Angeles)

ISVs, die an optimierten und skalierbaren Anwendungen in 5G-Netzwerken arbeiten, können beim Erstellen und Testen von Plattformen mit extrem geringer Latenz sowie in mobilen und vernetzten Szenarien jetzt den neuen Azure Edge Zone-Vorschaustandort Los Angeles mit AT&T verwenden. Registrieren Sie sich für das Early Adopter-Programm, um von sicherer Konnektivität mit hoher Bandbreite zu profitieren.

Weitere Informationen erhalten Sie beim [Supportteam für Azure Edge Zone-Zonen](https://aka.ms/EdgeZones).

## <a name="azure-private-edge-zones"></a><a name="private-edge-zones"></a>Azure Private Edge Zones

![Private Edge Zones](./media/edge-zones-overview/private-edge.png "Private Edge Zones")

Private Azure Edge Zone-Zonen sind kleine Ausläufer des Azure-Netzwerks, die lokal platziert werden. Private Azure Edge Zone-Zonen werden auf der Plattform [Azure Stack Edge](https://azure.microsoft.com/products/azure-stack/edge/) betrieben. Sie ermöglichen eine geringe Latenz beim Zugriff auf Computing- und Speicherdienste, die lokal bereitgestellt wurden. Mit privaten Azure Edge Zone-Zonen können Sie außerdem Anwendungen von ISVs und virtualisierte Netzwerkfunktionen (Virtualized Network Functions, VNFs) als [von Azure verwaltete Anwendungen](https://azure.microsoft.com/services/managed-applications/) neben virtuellen Computern und Containern lokal bereitstellen. Zu diesen VNFs können mobile Paketkerne, Router, Firewalls und SD-WAN-Geräte zählen. Private Azure Edge Zone-Zonen beinhalten eine cloudnative Orchestrierungslösung, mit der Sie den Lebenszyklus von VNFs und Anwendungen im Azure-Portal verwalten können.

Mit privaten Azure Edge Zone-Zonen können Sie Anwendungen lokal mithilfe derselben vertrauten Tools entwickeln und bereitstellen, die Sie auch für die Entwicklung und Bereitstellung von Anwendungen in Azure verwenden.

Außerdem bieten private Azure Edge Zone-Zonen Ihnen folgende Möglichkeiten:

- Ausführen von privaten mobilen Netzwerken (Private LTE, privates 5G).
- Implementieren von Sicherheitsfunktionen wie Firewalls
- Erweitern Ihrer lokalen Netzwerke auf mehrere Zweigstellen und Azure, indem SD-WAN-Geräte auf denselben Geräten für private Azure Edge Zone-Zonen verwendet und über Azure verwaltet werden

Typische Anwendungsfälle sind:

- Echtzeitsteuerung von Robotern
- Analysen und Rückschlüsse in Echtzeit mit künstlicher Intelligenz und maschinellem Lernen
- Maschinelles Sehen
- Remoterendering für Mixed Reality- und VDI-Szenarios
- Überwachung und Sicherheit

Wir verfügen über ein umfassendes Ökosystem von VNF-Anbietern, ISVs und MSP-Partnern, um End-to-End-Lösungen zu ermöglichen, die private Azure Edge Zone-Zonen verwenden. Weitere Informationen erhalten Sie beim [Supportteam für private Azure Edge Zone-Zonen](https://aka.ms/EdgeZonesPartner).

### <a name="private-edge-zone-partners"></a><a name="private-edge-partners"></a>Partner für private Azure Edge Zone-Zonen

![Partner für private Azure Edge Zone-Zonen](./media/edge-zones-overview/partners.png "Partner für private Azure Edge Zone-Zonen")

#### <a name="virtualized-network-functions-vnfs"></a><a name="vnf"></a>Virtualisierte Netzwerkfunktionen

##### <a name="virtualized-evolved-packet-core-vepc-for-mobile-networks"></a><a name="vEPC"></a>Virtualized Evolved Packet Core (vEPC) für Mobilfunknetze

- [Bestätigte Netzwerke](https://www.affirmednetworks.com/)
- [Celona](https://www.celona.io/azure-edge)
- [Druid Software](https://www.druidsoftware.com/)
- [Expeto](https://www.expeto.io/)
- [Mavenir](https://mavenir.com/)
- [Metaswitch](https://www.metaswitch.com/)
- [Nokia Digital Automation Cloud](https://www.dac.nokia.com/)

##### <a name="mobile-radio-partners"></a><a name="mobile-radio"></a>Partner für Digitalradio

- [Celona](https://www.celona.io/azure-edge)
- [Commscope Ruckus](https://support.ruckuswireless.com/)

##### <a name="sd-wan-vendors"></a><a name="sdwan-vendors"></a>SD-WAN-Anbieter

- [128 Technology](https://www.128technology.com/)
- [NetFoundry](https://netfoundry.io/)
- [Nuage Networks von Nokia](https://www.nuagenetworks.net/)
- [Versa Networks](https://www.versa-networks.com/)
- [VMware SD-WAN von Velocloud](https://www.velocloud.com/)

##### <a name="router-vendors"></a><a name="router-vendors"></a>Routeranbieter

- [Arista](https://www.arista.com/)

##### <a name="firewall-vendors"></a><a name="firewall-vendors"></a>Firewallanbieter

- [Palo Alto Networks](https://www.paloaltonetworks.com/)

##### <a name="managed-solutions-providers-mobile-operators-and-global-system-integrators-gsis"></a><a name="msp-mobile"></a>Anbieter von verwalteten Lösungen: Betreiber von Mobilfunknetzen und globale Systemintegratoren

| Globale Systemintegratoren und Betreiber | Mobilfunkanbieter |
| --- | --- |
| Amdocs                       | Etisalat             |
| American Tower               | NTT Communications   |
| CenturyLink                  | Proximus             |
| Expeto                       | Rogers               |
| Federated Wireless           | SK Telecom           |
| Infosys                      | Telefonica           |
| Tech Mahindra                | Telstra              |
|                              | Vodafone             |

Weitere Informationen dazu, wie Sie Partner werden können, erhalten Sie beim [Supportteam für private Azure Zone-Zonen](https://aka.ms/EdgeZonesPartner).

### <a name="private-edge-zone-solutions"></a><a name="solutions-private-edge"></a>Lösungen für private Azure Edge Zone-Zonen

#### <a name="private-mobile-network-on-private-edge-zones"></a><a name="private-mobile-private-edge"></a>Private Mobilfunknetze in privaten Azure Edge Zone-Zonen

![Private Mobilfunknetze in privaten Azure Edge Zone-Zonen](./media/edge-zones-overview/mobile-networks.png "Private Mobilfunknetze in privaten Azure Edge Zone-Zonen")

Sie können jetzt ein privates Mobilfunknetz in einer privaten Azure Edge Zone-Zone bereitstellen. Private Mobilfunknetze ermöglichen eine extrem niedrige Latenz, hohe Kapazitäten und ein zuverlässiges und sicheres Drahtlosnetzwerk, das für unternehmenskritische Anwendungen erforderlich ist.

Private Mobilfunknetze können Szenarios wie die folgenden ermöglichen:
- Steuerung von fahrerlosen Transportfahrzeugen (Automated Guided Vehicles, AGVs) in Lagerhäusern
- Echtzeitkommunikation zwischen Robotern in intelligenten Fabriken
- Augmented Reality und Virtual Reality in Edgeanwendungen

Die Netzwerkfunktion „Virtualized Evolved Packet Core“ (vEPC) bildet das Kernstück eines privaten Mobilfunknetzes. Sie können jetzt einen vEPC in Private Edge Zones bereitstellen. Eine Liste der in privaten Azure Edge Zone-Zonen verfügbaren vEPC-Partner finden Sie unter [Virtualized Evolved Packet Core (vEPC) for mobile networks](#vEPC).

Die Bereitstellung eines privaten Mobilfunknetzes in einer privaten Azure Edge Zone-Zone erfordert weitere Komponenten wie mobile Zugriffspunkte, SIM-Karten und andere VNFs wie Router. Der Zugriff auf lizenzierte oder nicht lizenzierte Frequenzen ist entscheidend für den Aufbau eines privaten Mobilfunknetzes. Zudem benötigen Sie möglicherweise Hilfe bei der RF-Planung, dem Einrichten der Hardware, der Installation und dem Support. Eine Liste der Partner finden Sie unter [Mobilfunkpartner](#mobile-radio).

Microsoft bietet ein Partnerökosystem, in dem Sie für jeden Schritt des Prozesses Unterstützung finden. Die Partner von Microsoft können Ihnen bei der Netzwerkplanung, beim Erwerb der erforderlichen Geräte, beim Einrichten der Hardware und beim Verwalten der Konfiguration in Azure behilflich sein. Unsere geprüften Partner, die eng mit Microsoft zusammenarbeiten, stellen sicher, dass Ihre Lösung zuverlässig und benutzerfreundlich sein wird. Währenddessen können Sie sich auf die wichtigsten Szenarios konzentrieren – bei allem Übrigen können Sie sich auf Microsoft und das Partnernetzwerk verlassen.

#### <a name="sd-wan-on-private-edge-zones"></a><a name="sdwan-private-edge"></a>SD-WAN in Private Edge Zones

![SD-WAN in Private Edge Zones](./media/edge-zones-overview/sd-wan.png "SD-WAN in Private Edge Zones")

Mit SD-WAN können Sie Fernnetze (Wide Area Networks, WANs) für Unternehmen erstellen. Dies hat folgende Vorteile:

- Höhere Bandbreite
- Hochleistungszugriff auf die Cloud
- Diensteinbindung
- Zuverlässigkeit
- Richtlinienverwaltung
- Umfassende Netzwerksichtbarkeit

SD-WAN bietet eine nahtlose Konnektivität zwischen Zweigstellenbüros, die zu geringeren Betriebskosten von redundanten zentralen Controllern orchestriert wird.
Mit SD-WAN in privaten Azure Edge Zone-Zonen können Sie von einem auf die Investitionskosten fokussierten Modell zu einem Software-as-a-Service-Modell (SaaS) übergehen, um IT-Budgets zu reduzieren. Sie können außerdem einen SD-WAN-Partner Ihrer Wahl als Orchestrator oder Controller einsetzen, um neue Dienste zu implementieren und diese sofort im gesamten Netzwerk verfügbar zu machen.

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie weitere Informationen wünschen, kontaktieren Sie die folgenden Teams:

* [Supportteam für Azure Edge Zone-Zonen](https://aka.ms/EdgeZones)
* [Supportteam für private Azure Edge Zone-Zonen, um ein Partner zu werden](https://aka.ms/EdgeZonesPartner)
