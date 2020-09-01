---
title: Konzepte – Netzwerkinterkonnektivität
description: Erfahren Sie mehr über wichtige Aspekte und Anwendungsfälle für Netzwerke und Interkonnektivität in Azure VMware Solution.
ms.topic: conceptual
ms.date: 07/23/2020
ms.openlocfilehash: 3420f6aa61ced7632175f3e12edda9de72639517
ms.sourcegitcommit: 62717591c3ab871365a783b7221851758f4ec9a4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/22/2020
ms.locfileid: "88750570"
---
# <a name="azure-vmware-solution-preview-networking-and-interconnectivity-concepts"></a>Azure VMware Solution (Vorschau) – Netzwerk- und Interkonnektivitätskonzepte

Azure VMware Solution (AVS) bietet eine private VMware-Cloudumgebung, auf die Benutzer und Anwendungen von lokalen und Azure-basierten Umgebungen oder Ressourcen aus zugreifen können. Dienste wie Azure ExpressRoute und VPN-Verbindungen stellen die Konnektivität bereit. Diese Dienste benötigen bestimmte Netzwerkadressbereiche und Firewallports für die Aktivierung der Dienste.  

Wenn eine private Cloud bereitgestellt wird, werden private Netzwerke für Verwaltung, Bereitstellung und vMotion erstellt. Diese werden für den Zugriff auf vCenter und NSX-T Manager sowie für vMotion oder die Bereitstellung virtueller Computer verwendet. Auf all diese privaten Netzwerke kann über ein VNET in Azure oder über lokale Umgebungen zugegriffen werden. ExpressRoute Global Reach wird verwendet, um private Clouds mit lokalen Umgebungen zu verbinden. Für diese Verbindung ist ein VNET mit einer ExpressRoute-Leitung in Ihrem Abonnement erforderlich.

Auch bei der Bereitstellung einer privaten Cloud wird der Zugriff auf das Internet und auf Azure-Dienste bereitgestellt, damit VMs in Produktionsnetzwerken diese nutzen können.  Der Internetzugriff ist für neue private Clouds standardmäßig deaktiviert und kann jederzeit aktiviert oder deaktiviert werden.

Eine Möglichkeit, Interkonnektivität zu veranschaulichen, besteht darin, die beiden Implementierungsarten von privaten AVS-Clouds zu betrachten:

1. [**Die allgemeine reine Azure-Interkonnektivität**](#azure-virtual-network-interconnectivity) ermöglicht Ihnen die Verwaltung und Nutzung Ihrer privaten Cloud mit nur einem einzelnen virtuellen Netzwerk in Azure. Diese Implementierung eignet sich am besten für AVS-Evaluierungen oder -Implementierungen, die keinen Zugriff von lokalen Umgebungen erfordern.

1. [**Vollständige Interkonnektivität zwischen lokalen und privaten Clouds**](#on-premises-interconnectivity) erweitert die allgemeine reine Azure-Implementierung um die Interkonnektivität zwischen lokalen und privaten AVS-Clouds.
 
In diesem Artikel werden einige wichtige Konzepte, die Netzwerke und Interkonnektivität bilden, sowie Anforderungen und Beschränkungen behandelt. Es werden auch weitere Informationen zu den beiden Arten der Implementierungen der privaten AVS-Cloudinterkonnektivität behandeln. In diesem Artikel erfahren Sie, wie Sie Ihr Netzwerk ordnungsgemäß für die Verwendung von AVS konfigurieren.

## <a name="avs-private-cloud-use-cases"></a>Anwendungsfälle für die private AVS-Cloud

Die Anwendungsfälle für private AVS-Clouds umfassen:
- Neue VMware-VM-Workloads in der Cloud
- Bursting von VM-Workloads in die Cloud (nur lokal zu AVS)
- Migration von VM-Workloads in die Cloud (nur lokal zu AVS)
- Notfallwiederherstellung (AVS zu AVS oder lokal zu AVS)
- Nutzung von Azure-Diensten

> [!TIP]
> Bei allen Anwendungsfällen für den AVS-Dienst bestehen Verbindungen zwischen lokalen Standorten und privaten Clouds.

## <a name="azure-virtual-network-interconnectivity"></a>Azure Virtual Network-Interkonnektivität

In der Implementierung vom virtuellen Netzwerk zur privaten Cloud können Sie Ihre private Azure VMware Solution-Cloud verwalten, Workloads in Ihrer privaten Cloud verarbeiten und auf Azure-Dienste über die ExpressRoute-Leitung zugreifen. 

Im folgenden Diagramm ist die grundlegende Netzwerkinterkonnektivität dargestellt, die während der Bereitstellung einer privaten Cloud hergestellt wird. Das Diagramm zeigt logische ExpressRoute-basierte Netzwerkverbindungen zwischen einem virtuellen Netzwerk in Azure und einer privaten Cloud. Die Interkonnektivität wird drei der primären Anwendungsfälle gerecht:
* Eingehender Zugriff auf vCenter Server und NSX-T Manager, der von den virtuellen Computern in Ihrem Azure-Abonnement und nicht von Ihren lokalen Systemen aus zugänglich ist. 
* Ausgehender Zugriff von VMs auf Azure-Dienste. 
* Eingehender Zugriff und Nutzung von Workloads, die in einer privaten Cloud ausgeführt werden.

:::image type="content" source="media/concepts/adjacency-overview-drawing-single.png" alt-text="Basiskonnektivität zwischen einem virtuellen Netzwerk und einer privaten Cloud" border="false":::

## <a name="on-premises-interconnectivity"></a>Lokale Interkonnektivität

Bei der Implementierung vom lokalen oder virtuellen Netzwerk bis hin zur vollständigen privaten Cloud können Sie von lokalen Umgebungen aus auf Ihre privaten Azure VMware Solution-Clouds zugreifen. Diese Implementierung ist eine Erweiterung der grundlegenden Implementierung, die im vorherigen Abschnitt beschrieben wurde. Wie bei der grundlegenden Implementierung ist eine ExpressRoute-Leitung erforderlich, aber bei dieser Implementierung wird sie verwendet, um eine Verbindung von lokalen Umgebungen mit Ihrer privaten Cloud in Azure herzustellen. 

Das nachstehende Diagramm zeigt die Interkonnektivität von lokalen zu privaten Clouds, die die folgenden Anwendungsfälle ermöglicht:
* Hot/Cold Cross-vCenter vMotion
* Verwaltungszugriff von der lokalen Umgebung auf die private Azure VMware Solution-Cloud

:::image type="content" source="media/concepts/adjacency-overview-drawing-double.png" alt-text="Vollständige Konnektivität zwischen einem virtuellen Netzwerk und einer lokalen privaten Cloud" border="false":::

Für eine vollständige Interkonnektivität mit Ihrer privaten Cloud aktivieren Sie ExpressRoute Global Reach und fordern dann einen Autorisierungsschlüssel und eine private Peering-ID für Global Reach im Azure-Portal an. Der Autorisierungsschlüssel und die Peering-ID werden verwendet, um Global Reach zwischen einer ExpressRoute-Leitung in Ihrem Abonnement und der ExpressRoute-Leitung für Ihre neue private Cloud zu aktivieren. Sobald die ExpressRoute-Leitungen verknüpft wurden, wird der Netzwerkdatenverkehr zwischen lokalen Umgebungen und der privaten Cloud über die beiden Leitungen geroutet.  Informationen über die Anforderung und Verwendung des Autorisierungsschlüssels und der Peering-ID finden Sie im [Tutorial zum Erstellen eines ExpressRoute Global Reach-Peerings zu einer privaten Cloud](tutorial-expressroute-global-reach-private-cloud.md).

## <a name="next-steps"></a>Nächste Schritte 

- Erfahren Sie mehr über die [Überlegungen zu und Anforderungen für Netzwerkkonnektivität](tutorial-network-checklist.md). 
- Erfahren Sie mehr über [Speicherkonzepte für private Clouds](concepts-storage.md).


<!-- LINKS - external -->
[enable Global Reach]: ../expressroute/expressroute-howto-set-global-reach.md

<!-- LINKS - internal -->

