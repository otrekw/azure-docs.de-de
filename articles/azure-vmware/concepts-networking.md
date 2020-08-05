---
title: Konzepte – Netzwerkinterkonnektivität
description: Erfahren Sie mehr über wichtige Aspekte und Anwendungsfälle für Netzwerke und Interkonnektivität in Azure VMware Solution (AVS).
ms.topic: conceptual
ms.date: 07/23/2020
ms.openlocfilehash: c0416da9c745ccf92970ff39f623a782d5784983
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2020
ms.locfileid: "87062837"
---
# <a name="azure-vmware-solution-avs-preview-networking-and-interconnectivity-concepts"></a>Azure VMware Solution (AVS) Vorschau – Netzwerk- und Interkonnektivitätskonzepte

Die Netzwerkinterkonnektivität zwischen Ihren privaten Clouds in Azure VMware Solution (AVS) und lokalen Umgebungen oder virtuellen Netzwerken in Azure ermöglichen es Ihnen, auf Ihre private Cloud zuzugreifen und diese zu nutzen. In diesem Artikel werden einige wichtige Konzepte behandelt, die die Grundlage von Netzwerken und Interkonnektivität bilden.

Eine Möglichkeit, Interkonnektivität zu veranschaulichen, besteht darin, die beiden Implementierungsarten von privaten AVS-Clouds zu betrachten:

1. [**Die allgemeine reine Azure-Interkonnektivität**](#azure-virtual-network-interconnectivity) ermöglicht Ihnen die Verwaltung und Nutzung Ihrer privaten Cloud mit nur einem einzelnen virtuellen Netzwerk in Azure. Diese Implementierung eignet sich am besten für AVS-Evaluierungen oder -Implementierungen, die keinen Zugriff von lokalen Umgebungen erfordern.

1. [**Vollständige Interkonnektivität zwischen lokalen und privaten Clouds**](#on-premises-interconnectivity) erweitert die allgemeine reine Azure-Implementierung um die Interkonnektivität zwischen lokalen und privaten AVS-Clouds.
 
Weitere Informationen zu den Anforderungen und den beiden Arten von Interkonnektivitätsimplementierungen der privaten AVS-Cloud finden Sie in den folgenden Abschnitten.

## <a name="avs-private-cloud-use-cases"></a>Anwendungsfälle für die private AVS-Cloud

Die Anwendungsfälle für private AVS-Clouds umfassen:
- Neue VMware-VM-Workloads in der Cloud
- Bursting von VM-Workloads in die Cloud (nur lokal zu AVS)
- Migration von VM-Workloads in die Cloud (nur lokal zu AVS)
- Notfallwiederherstellung (AVS zu AVS oder lokal zu AVS)
- Nutzung von Azure-Diensten

 Bei allen Anwendungsfällen für den AVS-Dienst bestehen Verbindungen zwischen lokalen Standorten und privaten Clouds. 

## <a name="virtual-network-and-expressroute-circuit-requirements"></a>Anforderungen an ein virtuelles Netzwerk und die ExpressRoute-Leitung
 
Wenn Sie eine Verbindung von einem virtuellen Netzwerk in Ihrem Abonnement erstellen, wird die ExpressRoute-Leitung über Peering hergestellt. Sie verwendet einen Autorisierungsschlüssel und eine Peering-ID, die Sie im Azure-Portal anfordern. Das Peering ist eine private 1:1-Verbindung zwischen Ihrer privaten Cloud und dem virtuellen Netzwerk.

> [!NOTE] 
> Die ExpressRoute-Leitung ist nicht Teil einer privaten Cloudbereitstellung. Die Erläuterung der lokalen ExpressRoute-Leitung geht über den Rahmen dieses Dokuments hinaus. Wenn Sie eine lokale Verbindung mit Ihrer privaten Cloud benötigen, können Sie eine Ihrer vorhandenen ExpressRoute-Leitungen verwenden oder eine im Azure-Portal erwerben.

Wenn Sie eine private Cloud bereitstellen, erhalten Sie IP-Adressen für vCenter und NSX-T Manager. Um auf diese Verwaltungsschnittstellen zuzugreifen, müssen Sie zusätzliche Ressourcen in einem virtuellen Netzwerk in Ihrem Abonnement erstellen. Die Schritte zum Erstellen dieser Ressourcen und zum Einrichten des privaten Peerings mit ExpressRoute sind in den Tutorials beschrieben.

Das logische Netzwerk der privaten Cloud wird mit vorab bereitgestelltem NSX-T bereitgestellt. Ein Tier-0-Gateway und ein Tier-1-Gateway werden ebenfalls vorab für Sie bereitgestellt. Sie können ein Segment erstellen und an das vorhandene Tier-1-Gateway anfügen oder ein neues Tier-1-Gateway definieren und das Segment daran anfügen. Die logischen NSX-T-Netzwerkkomponenten unterstützen eine Ost-West-Konnektivität zwischen Workloads und zusätzlich eine Nord-Süd-Konnektivität mit dem Internet und Azure-Diensten. 

## <a name="routing-and-subnet-requirements"></a>Anforderungen an Routing und Subnetz

Das Routing ist BGP-basiert (Border Gateway Protocol), das automatisch erfolgt und standardmäßig für jede Bereitstellung einer privaten Cloud aktiviert wird. Für private AVS-Clouds müssen Sie Netzwerkadressräume privater Clouds mit CIDR-Netzwerkadressblöcken einer Präfixlänge von mindestens /22 für Subnetze planen, wie in der Tabelle unten gezeigt. Der Adressblock darf sich nicht mit Adressblöcken überlappen, die in anderen virtuellen Netzwerken in Ihrem Abonnement und Ihren lokalen Netzwerken verwendet werden. Innerhalb des Adressblocks werden Verwaltungs-, Bereitstellungs- und vMotion-Netzwerke automatisch bereitgestellt.

Beispiel für einen CIDR-Netzwerkadressblock vom Typ `/22`: `10.10.0.0/22`

Subnetze:

| Verwendung des Netzwerks             | Subnet | Beispiel        |
| ------------------------- | ------ | -------------- |
| Verwaltung von privaten Clouds  | `/24`  | `10.10.0.0/24` |
| vMotion-Netzwerk           | `/24`  | `10.10.1.0/24` |
| VM-Workloads              | `/24`  | `10.10.2.0/24` |
| ExpressRoute-Peering      | `/24`  | `10.10.3.8/30` |


## <a name="azure-virtual-network-interconnectivity"></a>Azure Virtual Network-Interkonnektivität

In der Implementierung vom virtuellen Netzwerk zur privaten Cloud können Sie Ihre private AVS-Cloud verwalten, Workloads in Ihrer privaten Cloud verarbeiten und auf Azure-Dienste über die ExpressRoute-Leitung zugreifen. 

Im folgenden Diagramm ist die grundlegende Netzwerkinterkonnektivität dargestellt, die während der Bereitstellung einer privaten Cloud hergestellt wird. Das Diagramm zeigt logische ExpressRoute-basierte Netzwerkverbindungen zwischen einem virtuellen Netzwerk in Azure und einer privaten Cloud. Die Interkonnektivität wird drei der primären Anwendungsfälle gerecht:
* Eingehender Zugriff auf vCenter Server und NSX-T Manager, der von den virtuellen Computern in Ihrem Azure-Abonnement und nicht von Ihren lokalen Systemen aus zugänglich ist. 
* Ausgehender Zugriff von VMs auf Azure-Dienste. 
* Eingehender Zugriff und Nutzung von Workloads, die in einer privaten Cloud ausgeführt werden.

:::image type="content" source="media/concepts/adjacency-overview-drawing-single.png" alt-text="Basiskonnektivität zwischen einem virtuellen Netzwerk und einer privaten Cloud" border="false":::

## <a name="on-premises-interconnectivity"></a>Lokale Interkonnektivität

Bei der Implementierung vom lokalen oder virtuellen Netzwerk bis hin zur vollständigen privaten Cloud können Sie von lokalen Umgebungen aus auf Ihre privaten AVS-Clouds zugreifen. Diese Implementierung ist eine Erweiterung der grundlegenden Implementierung, die im vorherigen Abschnitt beschrieben wurde. Wie bei der grundlegenden Implementierung ist eine ExpressRoute-Leitung erforderlich, aber bei dieser Implementierung wird sie verwendet, um eine Verbindung von lokalen Umgebungen mit Ihrer privaten Cloud in Azure herzustellen. 

Das nachstehende Diagramm zeigt die Interkonnektivität von lokalen zu privaten Clouds, die die folgenden Anwendungsfälle ermöglicht:
* Hot/Cold Cross-vCenter vMotion
* Verwaltungszugriff vom lokalen Standort auf die private AVS-Cloud

:::image type="content" source="media/concepts/adjacency-overview-drawing-double.png" alt-text="Vollständige Konnektivität zwischen einem virtuellen Netzwerk und einer lokalen privaten Cloud" border="false":::

Für eine vollständige Interkonnektivität mit Ihrer privaten Cloud aktivieren Sie ExpressRoute Global Reach und fordern dann einen Autorisierungsschlüssel und eine private Peering-ID für Global Reach im Azure-Portal an. Der Autorisierungsschlüssel und die Peering-ID werden verwendet, um Global Reach zwischen einer ExpressRoute-Leitung in Ihrem Abonnement und der ExpressRoute-Leitung für Ihre neue private Cloud zu aktivieren. Sobald die ExpressRoute-Leitungen verknüpft wurden, wird der Netzwerkdatenverkehr zwischen lokalen Umgebungen und der privaten Cloud über die beiden Leitungen geroutet.  Informationen über die Anforderung und Verwendung des Autorisierungsschlüssels und der Peering-ID finden Sie im [Tutorial zum Erstellen eines ExpressRoute Global Reach-Peerings zu einer privaten Cloud](tutorial-expressroute-global-reach-private-cloud.md).


## <a name="next-steps"></a>Nächste Schritte 

Im nächsten Schritt erfahren Sie mehr über [Speicherkonzepte für private Clouds](concepts-storage.md).

<!-- LINKS - external -->
[enable Global Reach]: ../expressroute/expressroute-howto-set-global-reach.md

<!-- LINKS - internal -->
