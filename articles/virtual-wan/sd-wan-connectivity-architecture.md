---
title: SD-WAN-Verbindungsarchitektur
titleSuffix: Azure Virtual WAN
description: Erfahren Sie etwas über das Verbinden eines privaten SD-WAN mit einem Azure Virtual WAN.
services: virtual-wan
author: skishen525
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 09/22/2020
ms.author: sukishen
ms.openlocfilehash: 87e9549419bccc36d743871755e782a71e93e5e0
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2020
ms.locfileid: "91267464"
---
# <a name="sd-wan-connectivity-architecture-with-azure-virtual-wan"></a>SD-WAN-Konnektivitätsarchitektur mit Azure Virtual WAN

Azure Virtual WAN ist ein Netzwerkdienst, der viele Cloudverbindungs- und Sicherheitsdienste mit einer einzigen Betriebsschnittstelle vereint. Zu diesen Diensten gehören Zweigniederlassungen (über Site-to-Site-VPN), Remotebenutzer (Point-to-Site-VPN), private Verbindungen (ExpressRoute), transitive Konnektivität innerhalb der Cloud für VNETs, Verbindungen zwischen VPNs und ExpressRoute, Routing, Azure Firewall und Verschlüsselung für private Verbindungen.

Obwohl es sich bei Azure Virtual WAN selbst um ein softwaredefiniertes WAN (SD-WAN) handelt, ist es auch darauf ausgelegt, eine nahtlose Verbindung mit den lokalen SD-WAN-Technologien und -Diensten zu ermöglichen. Viele dieser Dienste werden von unserem [Virtual WAN](virtual-wan-locations-partners.md)-Ökosystem und von Azure Networking Managed Services-Partnern [(MSPs)](../networking/networking-partners-msp.md) angeboten. Unternehmen, die Ihr privates WAN in ein SD-WAN umwandeln, haben beim Verbinden Ihres privaten SD-WAN mit Azure Virtual WAN verschiedene Optionen. Unternehmen können zwischen diesen Optionen wählen:

* Modell mit direkter Verbindung
* Modell mit indirekter Verbindung
* Modell mit verwaltetem Hybrid-WAN mit dem bevorzugten Anbieter für verwaltete Dienste ([Managed Service Provider, MSP](../networking/networking-partners-msp.md))

In allen diesen Fällen ist die Verbindung von Virtual WAN mit dem SD-WAN aus der Sicht der Verbindung ähnlich, kann jedoch aufseiten der Orchestrierung und des Betriebs variieren.

## <a name="direct-interconnect-model"></a><a name="direct"></a>Modell mit direkter Verbindung

![Modell mit direkter Verbindung](./media/sd-wan-connectivity-architecture/direct.png)

Bei diesem Architekturmodell ist die Einrichtung an der Zweigniederlassung des Kunden (Customer-Premises Equipment, CPE) des SD-WAN direkt über IPsec-Verbindungen mit Virtual WAN-Hubs verbunden. Die Zweigniederlassungs-CPE kann auch über das private SD-WAN mit anderen Zweigniederlassungen verbunden werden, oder Sie können Virtual WAN für Verbindungen zwischen Zweigniederlassungen nutzen. Zweigniederlassungen, die auf ihre Workloads in Azure zugreifen müssen, können über die IPsec-Tunnel, die in Virtual WAN-Hubs enden, direkt und sicher auf Azure zugreifen.

SD-WAN-CPE-Partner können die normalerweise mühsame und fehleranfällige IPsec-Konnektivität von ihren jeweiligen CPE-Geräten aus automatisieren. Diese Automatisierung ermöglicht es dem SD-WAN-Controller, über die Virtual WAN-API mit Azure zu kommunizieren, um die Virtual WAN-Standorte zu konfigurieren und die erforderliche IPsec-Tunnelkonfiguration an die Zweigniederlassungs-CPEs zu übermitteln. Unter [Automatisierungsrichtlinien](virtual-wan-configure-automation-providers.md) finden Sie eine Beschreibung der Automatisierung von Virtual WAN-Verbindungen durch verschiedene SD-WAN-Partner.

Die SD-WAN-CPE ist auch weiterhin der Ort, an dem die Datenverkehrsoptimierung und die Pfadauswahl implementiert und erzwungen werden. 

In diesem Modell werden einige proprietäre Datenverkehrsoptimierungen, die auf Echtzeit-Datenverkehrsmerkmalen basieren, möglicherweise nicht unterstützt, da die Verbindung mit Virtual WAN über IPsec erfolgt und das IPsec-VPN auf dem Virtual WAN-VPN-Gateway endet. Beispielsweise ist die dynamische Pfadauswahl in der Zweigniederlassungs-CPE möglich, da das Gerät an der Zweigniederlassung verschiedene Netzwerkpaketinformationen mit einem anderen SD-WAN-Knoten austauscht und dadurch den besten Link für den unterschiedlich priorisierten Datenverkehr an der Zweigniederlassung erkennt. Dieses Feature kann hilfreich sein, wenn eine Optimierung für den letzten Streckenabschnitt (Zweigniederlassung zum nächsten Microsoft-POP) erforderlich ist.

Mit Virtual WAN können Benutzer Azure-Pfadauswahl erhalten, eine richtlinienbasierte Pfadauswahl über mehrere ISP-Links aus der Zweigniederlassungs-CPE zu Virtual WAN-VPN-Gateways. Virtual WAN ermöglicht das Einrichten mehrerer Links (Pfade) aus derselben Zweigniederlassungs-CPE des SD-WAN. Jeder Link stellt eine Doppeltunnelverbindung von einer eindeutigen öffentlichen IP-Adresse der SD-WAN-CPE mit zwei verschiedenen Instanzen des Azure Virtual WAN-VPN-Gateways dar. SD-WAN-Anbieter können den optimalen Pfad zu Azure implementieren, basierend auf Datenverkehrsrichtlinien, die von Ihrer Richtlinien-Engine für die CPE-Links festgelegt werden. Auf Azure-Seite werden alle eingehenden Verbindungen gleich behandelt.

## <a name="indirect-interconnect-model"></a><a name="indirect"></a>Modell mit indirekter Verbindung

![Modell mit indirekter Verbindung](./media/sd-wan-connectivity-architecture/indirect.png)

In diesem Architekturmodell sind die Zweigniederlassungs-CPEs des SD-WAN indirekt mit Virtual WAN-Hubs verbunden. Wie in der Abbildung ersichtlich ist, wird eine CPE in einem virtuellen SD-WAN in einem Unternehmens-VNET bereitgestellt. Diese virtuelle CPE ist wiederum über IPsec mit den Virtual WAN-Hubs verbunden. Die virtuelle CPE übernimmt also die Aufgabe eines SD-WAN-Gateways zu Azure. Zweigniederlassungen, die auf ihre Workloads in Azure zugreifen müssen, können über das v-CPE-Gateway darauf zugreifen.

Da die Verbindungen mit Azure über das v-CPE-Gateway (NVA) erfolgen, findet der gesamte Datenverkehr zu und von Azure-Workload-VNETs zu anderen SD-WAN-Zweigniederlassungen über das NVA statt. In diesem Modell ist der Benutzer für die Verwaltung und den Betrieb des SD-WAN-NVA zuständig, einschließlich Hochverfügbarkeit, Skalierbarkeit und Routing.
  
## <a name="managed-hybrid-wan-model"></a><a name="hybrid"></a>Modell mit verwaltetem Hybrid-WAN

![Modell mit verwaltetem Hybrid-WAN](./media/sd-wan-connectivity-architecture/hybrid.png)

In diesem Architekturmodell können Unternehmen einen verwalteten SD-WAN-Dienst nutzen, der von einem MSP-Partner (Managed Service Provider) angeboten wird. Dieses Modell ähnelt den oben beschriebenen direkten und indirekten Modellen. In diesem Modell werden jedoch der SD-WAN-Entwurf, die Orchestrierung und der Betrieb vom SD-WAN-Anbieter bereitgestellt.

[Azure-Netzwerk-MSP-Partner](../networking/networking-partners-msp.md) können das SD-WAN und den Virtual WAN-Dienst mit [Azure Lighthouse](https://azure.microsoft.com/services/azure-lighthouse/) im Azure-Abonnement des Unternehmenskunden implementieren und das End-to-End-Hybrid-WAN im Auftrag des Kunden betreiben. Diese MSPs können auch Azure ExpressRoute im virtuellen WAN implementieren und als verwalteten End-to-End-Dienst betreiben.

## <a name="additional-information"></a>Zusätzliche Informationen

* [Häufig gestellte Fragen (FAQ)](virtual-wan-faq.md)
* [Lösen von Remotekonnektivität](work-remotely-support.md)
