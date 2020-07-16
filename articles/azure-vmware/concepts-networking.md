---
title: Konzepte – Netzwerkinterkonnektivität
description: Erfahren Sie mehr über wichtige Aspekte und Anwendungsfälle für Netzwerke und Interkonnektivität in Azure VMware Solution (AVS).
ms.topic: conceptual
ms.date: 05/04/2020
ms.openlocfilehash: 35d886fe0f6a68e522d7f2cf20b450b5d9afc199
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "84629208"
---
# <a name="azure-vmware-solution-avs-preview-networking-and-interconnectivity-concepts"></a>Azure VMware Solution (AVS) Vorschau – Netzwerk- und Interkonnektivitätskonzepte

Die Netzwerkinterkonnektivität zwischen Ihren privaten Clouds in Azure VMware Solution (AVS) und lokalen Umgebungen oder virtuellen Netzwerken in Azure ermöglichen es Ihnen, auf Ihre private Cloud zuzugreifen und diese zu nutzen. In diesem Artikel werden einige wichtige Netzwerk- und Verbindungskonzepte erläutert, die die Basis für die Interkonnektivität bilden.

AVS bietet zwei Implementierungstypen für private Clouds: Implementierungen mit grundlegender, ausschließlich auf Azure basierender Interkonnektivität und Implementierungen mit vollständiger Interkonnektivität zwischen lokalen Netzwerken und einer privaten Cloud.

Die Anwendungsfälle für private AVS-Clouds umfassen:
- Neue VMware-VM-Workloads in der Cloud
- Bursting von VM-Workloads in die Cloud
- Migration von VM-Workloads zur Cloud
- Notfallwiederherstellung
- Nutzung von Azure-Diensten

 Bei allen Anwendungsfällen für den AVS-Dienst bestehen Verbindungen zwischen lokalen Standorten und privaten Clouds. Das grundlegende Interkonnektivitätsmodell eignet sich am besten für AVS-Evaluierungen oder -Implementierungen, die keinen Zugriff von lokalen Umgebungen erfordern.

In den folgenden Abschnitten werden die beiden Verbindungsarten für private AVS-Clouds beschrieben.  Die einfachste Form der Interkonnektivität stellt die Verbindung über Azure Virtual Network dar. Sie ermöglicht es Ihnen, Ihre private Cloud mit einem einzigen virtuellen Netzwerk in Azure zu verwalten und zu nutzen. In dem unter „Lokale Interkonnektivität“ beschriebenen Modell werden virtuelle Netzwerkverbindungen ausgeweitet, um auch Interkonnektivität zwischen lokalen Umgebungen und privaten AVS-Clouds zu gewährleisten.

## <a name="azure-virtual-network-interconnectivity"></a>Azure Virtual Network-Interkonnektivität

Im folgenden Diagramm ist die grundlegende Netzwerkinterkonnektivität dargestellt, die während der Bereitstellung einer privaten Cloud hergestellt wird. Das Diagramm zeigt logische ExpressRoute-basierte Netzwerkverbindungen zwischen einem virtuellen Netzwerk in Azure und einer privaten Cloud. Die Interkonnektivität wird drei der primären Anwendungsfälle gerecht:
- Eingehender Zugriff auf Verwaltungsnetzwerke, in denen sich vCenter Server und NSX-T Manager befinden.
    - Zugriff von VMs in Ihrem Azure-Abonnement, nicht von lokalen Systemen.
- Ausgehender Zugriff von VMs auf Azure-Dienste.
- Eingehender Zugriff und Nutzung von Workloads, die in einer privaten Cloud ausgeführt werden.

![Basiskonnektivität zwischen einem virtuellen Netzwerk und einer privaten Cloud](./media/concepts/adjacency-overview-drawing-single.png)

Die ExpressRoute-Leitung in diesem Szenario „Virtuelles Netzwerk zu privater Cloud“ wird hergestellt, wenn Sie eine Verbindung von einem virtuellen Netzwerk in Ihrem Abonnement mit der ExpressRoute-Leitung Ihrer privaten Cloud herstellen. Beim Peering werden ein Autorisierungsschlüssel und eine Leitungs-ID verwendet, die Sie im Azure-Portal anfordern. Die ExpressRoute-Verbindung, die über das Peering hergestellt wird, ist eine private 1:1-Verbindung zwischen Ihrer privaten Cloud und dem virtuellen Netzwerk. Sie können Ihre private Cloud verwalten, Workloads in Ihrer privaten Cloud nutzen und über die ExpressRoute-Verbindung auf Azure-Dienste zugreifen.

Wenn Sie eine private AVS-Cloud bereitstellen, ist ein einzelner /22-Adressraum eines privaten Netzwerks erforderlich. Dieser Adressraum darf sich nicht mit Adressräumen überlappen, die in anderen virtuellen Netzwerken Ihres Abonnements verwendet werden. Innerhalb des Adressraums werden Verwaltungs-, Bereitstellungs- und vMotion-Netzwerke automatisch bereitgestellt. Das Routing ist BGP-basiert, erfolgt automatisch und wird standardmäßig für jede Bereitstellung einer privaten Cloud aktiviert.

Wenn eine private Cloud bereitgestellt wird, erhalten Sie die IP-Adressen für vCenter und NSX-T Manager. Um auf diese Verwaltungsschnittstellen zuzugreifen, erstellen Sie zusätzliche Ressourcen in einem virtuellen Netzwerk in Ihrem Abonnement. Die Schritte zum Erstellen dieser Ressourcen und zum Einrichten des privaten Peerings mit ExpressRoute sind in den Tutorials beschrieben.

Sie entwerfen das logische Netzwerk der privaten Cloud und implementieren dieses mit NSX-T. NSX-T wird in einer privaten Cloud vorab bereitgestellt. Ein Tier-0-Gateway und ein Tier-1-Gateway werden ebenfalls vorab für Sie bereitgestellt. Sie können ein Segment erstellen und an das vorhandene Tier-1-Gateway anfügen oder ein neues Tier-1-Gateway definieren und das Segment daran anfügen. Die logischen NSX-T-Netzwerkkomponenten unterstützen eine Ost-West-Konnektivität zwischen Workloads und zusätzlich eine Nord-Süd-Konnektivität mit dem Internet und Azure-Diensten. 

## <a name="on-premises-interconnectivity"></a>Lokale Interkonnektivität

Sie können auch lokale Umgebungen mit privaten AVS-Clouds verbinden. Diese Art der Interkonnektivität stellt eine Erweiterung der grundlegenden Interkonnektivität dar, die im vorherigen Abschnitt beschrieben wurde.

![Vollständige Konnektivität zwischen einem virtuellen Netzwerk und einer lokalen privaten Cloud](./media/concepts/adjacency-overview-drawing-double.png)

Um vollständige Interkonnektivität mit einer privaten Cloud herzustellen, aktivieren Sie über das Azure-Portal ExpressRoute Global Reach zwischen einer ExpressRoute-Leitung einer privaten Cloud und einer ExpressRoute-Leitung an einem lokalen Standort. Diese Konfiguration stellt eine Erweiterung der Basiskonnektivität dar, weil der Zugriff auf private Clouds von lokalen Umgebungen aus ermöglicht wird.

Zum Verbinden von lokalen Umgebungen mit Ihrer privaten Cloud in Azure ist eine ExpressRoute-Leitung von der lokalen Umgebung zu Azure Virtual Network erforderlich. Diese ExpressRoute-Leitung ist Teil Ihres Abonnements und gehört nicht zur Bereitstellung einer privaten Cloud. Die Erläuterung der lokalen ExpressRoute-Leitung geht über den Rahmen dieses Dokuments hinaus. Wenn Sie eine lokale Verbindung mit Ihrer privaten Cloud benötigen, können Sie eine Ihrer vorhandenen ExpressRoute-Leitungen verwenden oder eine im Azure-Portal erwerben.

Sobald die ExpressRoute-Leitungen mit Global Reach verknüpft wurden, wird der Netzwerkdatenverkehr zwischen lokalen Umgebungen und der privaten Cloud über die beiden Leitungen geroutet. Im vorherigen Diagramm ist die Interkonnektivität zwischen lokalen Standorten und privater Cloud dargestellt. Die im Diagramm gezeigte Interkonnektivität unterstützt die folgenden Anwendungsfälle:

- Hot/Cold Cross-vCenter vMotion
- Verwaltungszugriff von der lokalen Umgebung auf die private AVS-Cloud

Um vollständige Konnektivität zu gewährleisten, können ein Autorisierungsschlüssel und eine private Peering-ID für Global Reach im Azure-Portal angefordert werden. Sie verwenden den Schlüssel und die ID, um Global Reach zwischen einer ExpressRoute-Leitung in Ihrem Abonnement und der ExpressRoute-Leitung für Ihre neue private Cloud zu aktivieren. Im [Tutorial zum Erstellen einer privaten Cloud](tutorial-create-private-cloud.md) sind die Verfahren zum Anfordern und Verwenden des Schlüssels und der ID beschreiben.

Aufgrund der Routinganforderungen der Lösung müssen die Adressräume für das Netzwerk der privaten Cloud geplant werden, um Überlappungen mit anderen virtuellen Netzwerken und lokalen Netzwerken zu vermeiden. Private AVS-Clouds benötigen mindestens einen `/22`-CIDR-Netzwerk-Adressblock für Subnetze, wie unten gezeigt. Dieses Netzwerk ergänzt Ihre lokalen Netzwerke. Für die Verbindungsherstellung mit lokalen Umgebungen und virtuellen Netzwerken darf der Netzwerkadressblock keine Überlappung aufweisen.

Beispiel für einen CIDR-Netzwerkadressblock vom Typ `/22`: `10.10.0.0/22`

Subnetze:

| Verwendung des Netzwerks             | Subnet | Beispiel        |
| ------------------------- | ------ | -------------- |
| Verwaltung von privaten Clouds            | `/24`    | `10.10.0.0/24`   |
| vMotion-Netzwerk       | `/24`    | `10.10.1.0/24`   |
| VM-Workloads | `/24`   | `10.10.2.0/24`   |
| ExpressRoute-Peering | `/24`    | `10.10.3.8/30`   |

## <a name="next-steps"></a>Nächste Schritte 

Im nächsten Schritt erfahren Sie mehr über [Speicherkonzepte für private Clouds](concepts-storage.md).

<!-- LINKS - external -->
[enable Global Reach]: https://docs.microsoft.com/azure/expressroute/expressroute-howto-set-global-reach

<!-- LINKS - internal -->

