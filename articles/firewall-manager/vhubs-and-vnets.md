---
title: Welche Optionen bietet die Azure Firewall Manager-Architektur?
description: Vergleich und Gegenüberstellung der Architekturen für virtuelle Hubnetzwerke und geschützte virtuelle Hubs bei Verwendung mit Azure Firewall Manager
author: vhorne
ms.service: firewall-manager
services: firewall-manager
ms.topic: article
ms.date: 02/18/2020
ms.author: victorh
ms.openlocfilehash: b946a360ced05500a4ef89cda7c623d8ae16658e
ms.sourcegitcommit: 6e87ddc3cc961945c2269b4c0c6edd39ea6a5414
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/18/2020
ms.locfileid: "77444867"
---
# <a name="what-are-the-azure-firewall-manager-architecture-options"></a>Welche Optionen bietet die Azure Firewall Manager-Architektur?

Azure Firewall Manager kann die Sicherheitsverwaltung für zwei Netzwerkarchitekturtypen bieten:

- **Geschützter virtueller Hub**

   Ein [Azure Virtual WAN Hub](../virtual-wan/virtual-wan-about.md#resources) ist eine von Microsoft verwaltete Ressource, mit der Sie ganz einfach Hub-and-Spoke-Architekturen erstellen können. Wenn einem solchen Hub Sicherheits- und Routingrichtlinien zugeordnet werden, wird dieser als *[geschützter virtueller Hub](secured-virtual-hub.md)* bezeichnet. 
- **Virtuelles Hubnetzwerk**

   Hierbei handelt es sich um ein virtuelles Azure-Standardnetzwerk, das Sie selbst erstellen und verwalten. Wenn einem solchen Hub Sicherheitsrichtlinien zugeordnet werden, wird dies als *virtuelles Hubnetzwerk* bezeichnet. Zurzeit wird nur die Azure Firewall-Richtlinie unterstützt. Sie können virtuelle Spoke-Netzwerke, die Ihre Workloadserver und -dienste enthalten, per Peering verknüpfen. Sie können auch Firewalls in eigenständigen virtuellen Netzwerken verwalten, die nicht per Peering mit einem Spoke verknüpft sind.

## <a name="comparison"></a>Vergleich

In der folgenden Tabelle werden diese beiden Architekturoptionen miteinander verglichen, sodass Sie entscheiden können, welche sich für die Sicherheitsanforderungen Ihrer Organisation am besten eignet:


|  |**Virtuelles Hubnetzwerk**|**Geschützter virtueller Hub**  |
|---------|---------|---------|
|**Zugrunde liegende Ressource**     |Virtuelles Netzwerk|Virtual WAN-Hub|
|**Hub and Spoke**     |Verwendet das Peering virtueller Netzwerke|Automatisiert über virtuelle Hubnetzwerkverbindung|
|**Lokale Konnektivität**     |VPN Gateway mit bis zu 10 Gbit/s und 30 Site-to-Site-Verbindungen; ExpressRoute|Besser skalierbares VPN Gateway mit bis zu 20 Gbit/s und 1000 Site-to-Site-Verbindungen; ExpressRoute|
|**Automatisierte Branchkonnektivität über SDWAN**      |Nicht unterstützt|Unterstützt|
|**Hubs pro Region**     |Mehrere virtuelle Netzwerke pro Region|Ein geschützter virtueller Hub pro Region; mehrere Hubs möglich mit mehreren Virtual WANs|
|**Azure Firewall – mehrere öffentliche IP-Adressen**      |Vom Kunden bereitgestellt|Automatisch generiert; bei allgemeiner Verfügbarkeit erhältlich|
|**Azure Firewall-Verfügbarkeitszonen**     |Unterstützt|In der Vorschau nicht verfügbar; bei allgemeiner Verfügbarkeit erhältlich|
|**Erweiterte Internetsicherheit mit Security-as-a-Service-Lösungen von Drittanbietern**     |Vom Kunden hergestellte und verwaltet VPN-Konnektivität mit dem gewünschten Partnerdienst|Automatisiert über Flow für vertrauenswürdigen Sicherheitspartner und Partnerverwaltungslösung|
|**Zentralisierte Routenverwaltung zum Weiterleiten von Datenverkehr an den Hub**     |Kundenseitig verwaltete benutzerdefinierte Route|Unterstützt über BGP|
|**Web Application Firewall auf Application Gateway** |Unterstützt in virtuellem Netzwerk|Derzeit in Spoke-Netzwerk unterstützt|
|**Virtuelles Netzwerkgerät**|Unterstützt in virtuellem Netzwerk|Derzeit in Spoke-Netzwerk unterstützt|

## <a name="next-steps"></a>Nächste Schritte

- [Übersicht über die Bereitstellung von Azure Firewall Manager (Vorschau)](deployment-overview.md)
- Weitere Informationen über [geschützte virtuelle Hubs](secured-virtual-hub.md)