---
title: Dokumentation zur Azure-Netzwerkarchitektur
description: Erfahren Sie mehr über die Architekturdokumentation, die für Azure-Netzwerkdienste zur Referenz verfügbar ist.
services: networking
author: KumudD
ms.service: virtual-network
ms.topic: article
ms.date: 03/30/2021
ms.author: kumud
ms.openlocfilehash: 9b608312d66e6a3e7455c4577ea4644b33e4e82e
ms.sourcegitcommit: 3ee3045f6106175e59d1bd279130f4933456d5ff
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/31/2021
ms.locfileid: "106079815"
---
# <a name="azure-networking-architecture-documentation"></a>Dokumentation zur Azure-Netzwerkarchitektur

Dieser Artikel enthält Informationen zu Architekturleitfäden, mit deren Hilfe Sie die verschiedenen Netzwerkdienste in Azure erkunden können, die Ihnen zum Entwickeln Ihrer Anwendungen zur Verfügung stehen.

## <a name="networking-overview"></a>Netzwerkübersicht

In der folgenden Tabelle sind Artikel aufgeführt, die eine Netzwerkübersicht für ein virtuelles Rechenzentrum und eine Hub-Spoke-Topologie in Azure bereitstellen.

|Titel |BESCHREIBUNG  |
|---------|---------|
|[Virtuelle Datencenter](/azure/architecture/vdc/networking-virtual-datacenter)   | Stellt ein virtuelles Rechenzentrum in Azure aus Netzwerkperspektive dar.       |
|[Hub-Spoke-Topologie](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke)  |Bietet eine Übersicht über die Hub-Spoke-Netzwerktopologie in Azure sowie Informationen zu Abonnementlimits und mehreren Hubs.          |

## <a name="connect-to-azure-resources"></a>Herstellen einer Verbindung mit Azure-Ressourcen

Die folgende Tabelle enthält Artikel zu Azure-Netzwerkdiensten, die Konnektivität zwischen Azure-Ressourcen, Konnektivität zwischen einem lokalen Netzwerk und Azure-Ressourcen sowie Konnektivität zwischen Verzweigungen in Azure bereitstellen.

|Titel |BESCHREIBUNG  |
|---------|---------|
|[Hinzufügen von IP-Adressräumen zu virtuellen Netzwerken mit Peering](/azure/architecture/networking/prefixes/add-ip-space-peered-vnet)     | Stellt Skripts bereit, mit denen IP-Adressbereiche virtuellen Netzwerken mit Peering hinzugefügt werden können.        |
|[Verbinden von eigenständigen Servern mithilfe des Azure-Netzwerkadapters](/azure/architecture/hybrid/azure-network-adapter)   | Zeigt, wie Sie einen lokalen eigenständigen Server mithilfe des Azure-Netzwerkadapters, den Sie über Windows Admin Center bereitstellen, mit virtuellen Microsoft Azure-Netzwerken verbinden.        |
|[Wählen zwischen Peering virtueller Netzwerke und VPN-Gateways](/azure/architecture/reference-architectures/hybrid-networking/vnet-peering)   | Vergleicht zwei Verbindungsarten für virtuelle Netzwerke in Azure: Peering virtueller Netzwerke und VPN-Gateways.        |
|[Verbinden eines lokalen Netzwerks mit Azure](/azure/architecture/reference-architectures/hybrid-networking/)  | Vergleicht Optionen zum Herstellen einer Verbindung zwischen einem lokalen Netzwerk und einem virtuellen Azure-Netzwerk (VNet). Für jede Option ist eine detailliertere Referenzarchitektur verfügbar.        |
|[SD-WAN-Konnektivitätsarchitektur mit Azure Virtual WAN](../../virtual-wan/sd-wan-connectivity-architecture.md)|Beschreibt die verschiedenen Konnektivitätsoptionen für das Herstellen einer Verbindung zwischen einem privaten SD-WAN (Software-Defined WAN) und Azure Virtual WAN.|

## <a name="deploy-highly-available-applications"></a>Bereitstellen hochverfügbarer Anwendungen

Die in der folgenden Tabelle aufgeführten Artikel beschreiben, wie Sie Ihre Anwendungen mithilfe einer Kombination aus Azure-Netzwerkdiensten für Hochverfügbarkeit bereitstellen.

|Titel |BESCHREIBUNG  |
|---------|---------|
|[n-schichtige Anwendung für mehrere Regionen](/azure/architecture/reference-architectures/n-tier/multi-region-sql-server)  | Beschreibt eine regionsübergreifende n-schichtige Anwendung, die mithilfe von Traffic Manager eingehende Anforderungen an eine primäre Region weiterleitet. Sollte diese Region nicht mehr verfügbar sein, führt Traffic Manager ein Failover auf die sekundäre Region aus.      |
| [Mehrinstanzenfähige SaaS-Anwendungen in Azure](https://docs.microsoft.com/azure/architecture/example-scenario/multi-saas/multitenant-saas)       |   Verwendet eine mehrinstanzenfähige Lösung, die eine Kombination aus Front Door und Application Gateway umfasst.  Front-Door unterstützt den regionsübergreifenden Lastenausgleich des Datenverkehrs und Application Gateway routet den Datenverkehr intern in der Anwendung zu den verschiedenen Diensten, die die Geschäftsanforderungen der Kunden erfüllen, und führt dabei ebenfalls einen Lastenausgleich durch.  |
| [Für Hochverfügbarkeit und Notfallwiederherstellung konzipierte Multi-Tier-Webanwendung](https://docs.microsoft.com/azure/architecture/example-scenario/infrastructure/multi-tier-app-disaster-recovery)        |      Stellt resiliente Multi-Tier-Anwendungen bereit, die für Hochverfügbarkeit und Notfallwiederherstellung entwickelt wurden. Wenn die primäre Region nicht mehr verfügbar ist, führt Traffic Manager ein Failover zur sekundären Region aus.  |
|[IaaS: Webanwendung mit relationaler Datenbank](/azure/architecture/high-availability/ref-arch-iaas-web-and-db)    |   Beschreibt, wie Sie mit auf mehrere Zonen verteilten Ressourcen eine Hochverfügbarkeitsarchitektur für das Hosten einer IaaS-Webanwendung (Infrastructure-as-a-Service) und einer SQL Server-Datenbank bereitstellen.     |
|[Teilen des Standorts in Echtzeit mithilfe kostengünstiger serverloser Azure-Dienste](/azure/architecture/example-scenario/signalr/#azure-front-door)       |   Verwendet Azure Front Door, um die Verfügbarkeit Ihrer Anwendungen stärker zu erhöhen als dies durch Bereitstellung in einer einzelnen Region möglich wäre. Wenn ein regionaler Ausfall die primäre Region beeinträchtigt, können Sie mit Front Door ein Failover zur sekundären Region ausführen.      |
|[Hochverfügbare virtuelle Netzwerkgeräte](/azure/architecture/reference-architectures/dmz/nva-ha)     | Zeigt, wie eine Reihe virtueller Netzwerkappliances für Hochverfügbarkeit in Azure bereitgestellt wird.        |

## <a name="secure-your-network-resources"></a>Schützen Ihrer Netzwerkressourcen

Die in der folgenden Tabelle aufgeführten Artikel beschreiben, wie Sie Ihre Netzwerkressourcen mithilfe von Azure-Netzwerkdiensten schützen.

|Titel |BESCHREIBUNG  |
|---------|---------|
|[Bewährte Methoden für die Netzwerksicherheit](../../security/fundamentals/network-best-practices.md) |Hier werden bewährte Methoden von Azure zur Verbesserung der Netzwerksicherheit beschrieben.         |
[Entwerfen einer Firewall mit Lastenausgleich in Azure](/azure/architecture/example-scenario/firewalls/) | Bietet einen strukturierten Ansatz zum Entwerfen von Hochverfügbarkeitsfirewalls in Azure mithilfe virtueller Appliances von Drittanbietern.        |
|[Implementieren eines sicheren Hybridnetzwerks](/azure/architecture/reference-architectures/dmz/secure-vnet-dmz)     | Beschreibt eine Architektur, die eine DMZ (auch als Umkreisnetzwerk bezeichnet) zwischen dem lokalen Netzwerk und einem virtuellen Azure-Netzwerk implementiert. Sämtlicher eingehender und ausgehender Datenverkehr durchläuft die Azure Firewall.        |
|[Sichern und Steuern von Workloads mit Segmentierung auf Netzwerkebene](/azure/architecture/reference-architectures/hybrid-networking/network-level-segmentation) | Beschreibt die drei allgemeinen Muster zum Organisieren von Workloads in Azure aus Netzwerksicht.   Jedes dieser Muster bietet eine andere Art von Isolation und Konnektivität.      |
|[Firewall und Application Gateway für virtuelle Netzwerke](/azure/architecture/example-scenario/gateway/firewall-application-gateway) | Beschreibt Azure Virtual Network-Sicherheitsdienste wie Azure Firewall und Azure Application Gateway. Außerdem wird erläutert, in welchen Fällen einer dieser Dienste verwendet werden sollte, und es werden Netzwerkentwürfe vorgestellt, bei denen beide Dienste kombiniert werden.      |

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über [Azure Virtual Networks](../../virtual-network/virtual-networks-overview.md).
