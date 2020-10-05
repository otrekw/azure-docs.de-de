---
title: Azure-Netzwerkarchitektur
description: Dieser Artikel enthält eine allgemeine Beschreibung des Microsoft Azure-Infrastrukturnetzwerks.
services: security
documentationcenter: na
author: TerryLanfear
manager: rkarlin
ms.assetid: 61e95a87-39c5-48f5-aee6-6f90ddcd336e
ms.service: security
ms.subservice: security-fundamentals
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/08/2020
ms.author: terrylan
ms.openlocfilehash: 3b047489f9cfa3623c11e324cf58114b707c10b7
ms.sourcegitcommit: d0541eccc35549db6381fa762cd17bc8e72b3423
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/09/2020
ms.locfileid: "89567865"
---
# <a name="azure-network-architecture"></a>Azure-Netzwerkarchitektur
Die Azure-Netzwerkarchitektur bietet Konnektivität zwischen dem Internet und den Azure-Rechenzentren. Alle bereitgestellten Workloads (IaaS, PaaS und SaaS) in Azure nutzen das Netzwerk der Azure-Rechenzentren.

## <a name="network-topology"></a>Netzwerktopologie
Die Netzwerkarchitektur eines Azure-Rechenzentrums besteht aus den folgenden Komponenten:

- Umkreisnetzwerk
- WAN (Wide Area Network)
- Netzwerk regionaler Gateways
- Rechenzentrumsnetzwerk

![Abbildung zum Azure-Netzwerk](./media/infrastructure-network/network-arch.png)

## <a name="network-components"></a>Netzwerkkomponenten
Kurzbeschreibung der Netzwerkkomponenten

- Umkreisnetzwerk

   - Abgrenzungspunkt zwischen Microsoft-Netzwerken und anderen Netzwerken (z. B. Internet, Unternehmensnetzwerk)
   - Bietet Internet- und [ExpressRoute](../../expressroute/expressroute-introduction.md)-Peering in Azure

- WAN (Wide Area Network)

   - Weltweites intelligentes Microsoft-Backbone-Netzwerk
   - Bietet Konnektivität zwischen [Azure-Regionen](https://azure.microsoft.com/global-infrastructure/geographies/)

- Regionales Gateway

   - Aggregationspunkt für alle Rechenzentren in einer Azure-Region
   - Bietet umfassende Konnektivität zwischen Rechenzentren innerhalb einer Azure-Region (z. B. mehrere Hundert Terabit pro Rechenzentrum)

- Rechenzentrumsnetzwerk

   - Bietet Konnektivität zwischen Servern innerhalb des Rechenzentrums mit einer das Abonnement geringfügig überschreitenden Bandbreite

Die oben aufgeführten Netzwerkkomponenten sind so konzipiert, dass sie maximale Verfügbarkeit zur Unterstützung von ständig verfügbaren Always On-Aktivitäten in der Cloud gewährleisten. Redundanz wurde von rein physischen Aspekten bis zum Steuerungsprotokoll berücksichtigt und in das Netzwerk integriert.

## <a name="datacenter-network-resiliency"></a>Resilienz im Rechenzentrumsnetzwerk
Im Folgenden wird das Designkonzept für Resilienz anhand des Rechenzentrumsnetzwerks veranschaulicht.

Das Rechenzentrumsnetzwerk ist eine angepasste Version eines [Clos-Netzwerks](https://en.wikipedia.org/wiki/Clos_network), das eine hohe Bandbreite in zwei Segmenten für Datenverkehr in der Cloud bietet. Das Netzwerk wird mit einer großen Anzahl von Standardgeräten erstellt, um die Auswirkungen beim Ausfall einzelner Hardwarekomponenten zu verringern. Diese Geräte sind strategisch an verschiedenen physischen Standorten mit separater Stromversorgung und eigenem Kühlsystem angeordnet, um Umwelteinflüsse zu minimieren.  Auf Steuerungsebene werden alle Netzwerkgeräte im Routingmodus auf Schicht 3 des OSI-Modells ausgeführt, sodass frühere Probleme durch Datenverkehrsschleifen beseitigt werden. Alle Pfade zwischen unterschiedlichen Ebenen sind aktiv, um hohe Redundanz und Bandbreite mithilfe von ECMP-Routing (Equal-Cost Multi-Path) bereitzustellen.

Das folgende Diagramm zeigt eine Darstellung des Rechenzentrumsnetzwerks mit den unterschiedlichen Ebenen von Netzwerkgeräten. Die Balken im Diagramm stellen Gruppen von Netzwerkgeräten dar, die Redundanz und Konnektivität mit hoher Bandbreite bereitstellen.

![Rechenzentrumsnetzwerk](./media/infrastructure-network/datacenter-network.png)

## <a name="next-steps"></a>Nächste Schritte
In den folgenden Artikeln erfahren Sie mehr über den Schutz der Azure-Infrastruktur durch Microsoft:

- [Azure-Einrichtungen, Gelände und physische Sicherheit](physical-security.md)
- [Verfügbarkeit der Azure-Infrastruktur](infrastructure-availability.md)
- [Komponenten und Grenzen des Azure-Informationssystems](infrastructure-components.md)
- [Azure-Produktionsnetzwerk](production-network.md)
- [Sicherheitsfeatures für Azure SQL-Datenbank](infrastructure-sql.md)
- [Produktionsvorgänge und Verwaltung von Azure](infrastructure-operations.md)
- [Azure-Infrastrukturüberwachung](infrastructure-monitoring.md)
- [Integrität der Azure-Infrastruktur](infrastructure-integrity.md)
- [Schutz der Azure-Kundendaten](protection-customer-data.md)
