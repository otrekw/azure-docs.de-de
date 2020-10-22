---
title: 'Konzept: Integrieren einer Azure VMware Solution-Bereitstellung in eine Hub-and-Spoke-Architektur'
description: Es werden die Empfehlungen vorgestellt, gemäß denen eine Azure VMware Solution-Bereitstellung in eine vorhandene oder neue Hub-and-Spoke-Architektur in Azure integriert wird.
ms.topic: conceptual
ms.date: 10/14/2020
ms.openlocfilehash: 66c6cc4841b4b36775fda89b29dc588100c3ad87
ms.sourcegitcommit: 1b47921ae4298e7992c856b82cb8263470e9e6f9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/14/2020
ms.locfileid: "92058470"
---
# <a name="integrate-azure-vmware-solution-in-a-hub-and-spoke-architecture"></a>Integrieren von Azure VMware Solution in eine Hub-and-Spoke-Architektur

In diesem Artikel finden Sie Empfehlungen für die Integration einer Azure VMware Solution-Bereitstellung in eine vorhandene oder neue [Hub-and-Spoke-Architektur](/azure/architecture/reference-architectures/hybrid-networking/shared-services) in Azure. 


Für das Hub-and-Spoke-Szenario wird eine Hybrid Cloud-Umgebung mit Workloads für folgende Komponenten angenommen:

* Natives Azure mit IaaS- oder PaaS-Diensten
* Azure VMware Solution 
* vSphere lokal

## <a name="architecture"></a>Aufbau

Der *Hub* ist ein virtuelles Azure-Netzwerk, das als zentraler Konnektivitätspunkt für Ihr lokales Rechenzentrum und Ihre private Azure VMware Solution-Cloud fungiert. Die *Spokes* sind virtuelle Netzwerke, die gleichrangig mit dem Hub verbunden sind, um Kommunikation zwischen ihnen zu ermöglichen.

Der Datenverkehr zwischen dem lokalen Rechenzentrum, der privaten Azure VMware Solution-Cloud und dem Hub läuft über Azure ExpressRoute-Verbindungen. Ein virtuelles Spoke-Netzwerke enthält in der Regel IaaS-basierte Workloads, kann aber auch PaaS-Dienste wie [App Service-Umgebung](../app-service/environment/intro.md), die direkt in Virtual Network integriert sind, oder andere PaaS-Dienste mit aktiviertem [Azure Private Link](../private-link/index.yml) enthalten.

>[!IMPORTANT]
>Sie können ein vorhandenes ExpressRoute-Gateway verwenden, um eine Verbindung mit Azure VMware Solution herzustellen, sofern der Grenzwert von vier ExpressRoute-Verbindungen pro virtuellem Netzwerk nicht überschritten wird.  Für den lokalen Zugriff auf Azure VMware Solution über ExpressRoute benötigen Sie ExpressRoute Global Reach, da das ExpressRoute-Gateway kein transitives Routing zwischen den Verbindungen unterstützt.

Das Diagramm zeigt ein Beispiel für eine Hub-and-Spoke-Bereitstellung in Azure, die über ExpressRoute Global Reach mit dem lokalen Rechenzentrum und der Azure VMware Solution-Cloud verbunden ist.

:::image type="content" source="./media/hub-spoke/azure-vmware-solution-hub-and-spoke-deployment.png" alt-text="Hub-and-Spoke-Bereitstellung mit Azure VMware Solution-Integration" border="false" lightbox="./media/hub-spoke/azure-vmware-solution-hub-and-spoke-deployment.png":::

Diese Architektur besteht aus den folgenden Hauptkomponenten:

-   **Lokaler Standort:** Lokale Rechenzentren des Kunden, die über ExpressRoute-Verbindungen mit Azure verbunden sind.

-   **Private Azure VMware Solution-Cloud:** Das aus einem oder mehreren vSphere-Clustern bestehende Azure VMware Solution SDDC (Software Defined Data Center), jeweils mit maximal 16 Knoten.

-   **ExpressRoute-Gateway:** Ermöglicht die Kommunikation zwischen privater Azure VMware Solution-Cloud, freigegebenen Diensten im virtuellen Hub-Netzwerk und Workloads, die in virtuellen Spoke-Netzwerken ausgeführt werden.

-   **ExpressRoute Global Reach:** Ermöglicht die Konnektivität zwischen lokaler und privater Azure VMware Solution-Cloud.


  > [!NOTE]
  > **S2S-VPN-Überlegungen:** Für die Bereitstellung von Azure VMware Solution-Produktionsumgebungen wird Azure S2S-VPN wegen Netzwerkanforderungen für VMware HCX nicht unterstützt. Es kann jedoch für eine Proof of Concept-Bereitstellung verwendet werden.


-   **Virtuelles Hub-Netzwerk:** Fungiert als zentraler Verbindungspunkt für Ihr lokales Netzwerk und Ihre private Azure VMware Solution-Cloud.

-   **Virtuelles Spoke-Netzwerk**

    -   **IaaS-Spoke:** In einer IaaS-Spoke werden IaaS-basierte Azure-Workloads gehostet, wozu VM-Verfügbarkeitsgruppen und VM-Skalierungsgruppen sowie die entsprechenden Netzwerkkomponenten gehören.

    -   **PaaS-Spoke:** In einer PaaS-Spoke werden Azure-PaaS-Dienste über private Adressierung gehostet, wozu [privater Endpunkt](../private-link/private-endpoint-overview.md) (Private Endpoint) und [Private Link](../private-link/private-link-overview.md) verwendet werden.

-   **Azure Firewall:** Fungiert als das zentrale Element zum Segmentieren von Datenverkehr zwischen den Spokes und Azure VMware Solution.

-   **Application Gateway:** Macht Webanwendungen, die entweder auf Azure IaaS/PaaS- oder Azure VMware Solution-VMs ausgeführt werden, verfügbar und schützt diese. Dieser Dienst lässt sich mit anderen Diensten wie API Management kombinieren.

## <a name="network-and-security-considerations"></a>Netzwerk- und Sicherheitsaspekte

ExpressRoute-Verbindungen ermöglichen Datenverkehr zwischen lokalen Systemen, Azure VMware Solution und dem Azure-Netzwerkfabric. Für Azure VMware Solution wird [ExpressRoute Global Reach](../expressroute/expressroute-global-reach.md) verwendet, um diese Konnektivität zu implementieren.

Da ein ExpressRoute-Gateway kein transitives Routing zwischen den verbundenen Verbindungen bereitstellt, muss für die lokale Konnektivität ebenfalls ExpressRoute Global Reach für die Kommunikation zwischen der lokalen vSphere-Umgebung und Azure VMware Solution verwendet werden. 

* **Datenverkehr von lokalen Standorten zu Azure VMware Solution**

  :::image type="content" source="./media/hub-spoke/on-premises-azure-vmware-solution-traffic-flow.png" alt-text="Hub-and-Spoke-Bereitstellung mit Azure VMware Solution-Integration" border="false" lightbox="./media/hub-spoke/on-premises-azure-vmware-solution-traffic-flow.png":::


* **Datenverkehr von Azure VMware Solution zu Hub VNET**

  :::image type="content" source="./media/hub-spoke/azure-vmware-solution-hub-vnet-traffic-flow.png" alt-text="Hub-and-Spoke-Bereitstellung mit Azure VMware Solution-Integration" border="false" lightbox="./media/hub-spoke/azure-vmware-solution-hub-vnet-traffic-flow.png":::


Weitere Einzelheiten zu den Azure VMware Solution-Netzwerk- und -Konnektivitätskonzepten finden Sie in der [Produktdokumentation von Azure VMware Solution](./concepts-networking.md).

### <a name="traffic-segmentation"></a>Datenverkehrsegmentierung

[Azure Firewall](../firewall/index.yml) ist das zentrale Element der Hub-and-Spoke-Topologie und wird im virtuellen Hub-Netzwerk bereitgestellt. Sie verwenden Azure Firewall oder ein anderes von Azure unterstütztes virtuelles Netzwerkgerät, um Datenverkehrsregeln einzurichten und die Kommunikation zwischen den verschiedenen Spokes und Azure VMware Solution-Workloads zu segmentieren.

Erstellen Sie Routingtabellen, um den Datenverkehr an Azure Firewall zu leiten.  Erstellen Sie für die virtuellen Spoke-Netzwerke eine Route, mit der die Standardroute auf die interne Schnittstelle von Azure Firewall festgelegt wird. Über diese Vorgabe kann die Firewall, wenn eine Workload im virtuellen Netzwerk den Azure VMware Solution-Adressraum erreichen muss, die Workload auswerten und die entsprechende Datenverkehrsregel anwenden, um sie zuzulassen oder abzulehnen.  

:::image type="content" source="media/hub-spoke/create-route-table-to-direct-traffic.png" alt-text="Hub-and-Spoke-Bereitstellung mit Azure VMware Solution-Integration" lightbox="media/hub-spoke/create-route-table-to-direct-traffic.png":::


> [!IMPORTANT]
> Eine Route, die das Adresspräfix 0.0.0.0/0 in der **GatewaySubnet**-Einstellung hat, wird nicht unterstützt.

Legen Sie Routen für bestimmte Netzwerke in der entsprechenden Routingtabelle fest. Beispielsweise Routen zum Erreichen von IP-Präfixen zu Azure VMware Solution-Verwaltung und -Workloads aus den Spoke-Workloads und umgekehrt.

:::image type="content" source="media/hub-spoke/specify-gateway-subnet-for-route-table.png" alt-text="Hub-and-Spoke-Bereitstellung mit Azure VMware Solution-Integration" lightbox="media/hub-spoke/specify-gateway-subnet-for-route-table.png":::

Eine zweite Ebene der Datenverkehrssegmentierung mithilfe der Netzwerksicherheitsgruppen in den Spokes und im Hub, um eine präzisere Datenverkehrsrichtlinie zu erstellen.

> [!NOTE]
> **Datenverkehr von lokalen Workloads zu Azure VMware Solution:** Datenverkehr zwischen lokalen Workloads, entweder vSphere- oder anderweitig basiert, wird durch Global Reach ermöglicht, aber der Datenverkehr kommt nicht durch Azure Firewall auf dem Hub hindurch. In diesem Szenario müssen Sie Mechanismen zur Datenverkehrssegmentierung entweder lokal oder in Azure VMware Solution implementieren.

### <a name="application-gateway"></a>Application Gateway

Azure Application Gateway V1 und V2 wurden mit Web-Apps getestet, die auf Azure VMware Solution-VMs als Back-End-Pool ausgeführt werden. Application Gateway ist derzeit die einzige unterstützte Methode, um Web-Apps für das Internet verfügbar zu machen, die auf virtuellen Azure VMware Solution-Computern ausgeführt werden. Application Gateway kann die Apps auch für interne Benutzer sicher verfügbar machen.

Details und Informationen zu den Anforderungen finden Sie im Artikel für Azure VMware Solution zu [Application Gateway](./protect-azure-vmware-solution-with-application-gateway.md).

:::image type="content" source="media/hub-spoke/azure-vmware-solution-second-level-traffic-segmentation.png" alt-text="Hub-and-Spoke-Bereitstellung mit Azure VMware Solution-Integration" border="false":::


### <a name="jump-box-and-azure-bastion"></a>Jumpbox und Azure Bastion

Greifen Sie über eine Jumpbox (auch „Jumpserver“ genannt) auf die Azure VMware Solution-Umgebung zu. Eine Jumpbox ist eine Windows 10- oder Windows Server-VM, die im freigegebenen Dienstsubnetz im virtuellen Hub-Netzwerk bereitgestellt wird.

>[!IMPORTANT]
>Azure Bastion ist der Dienst, der empfohlen wird, um eine Verbindung mit der Jumpbox herzustellen, damit die Azure VMware Solution-Instanz nicht im Internet veröffentlicht wird. Sie können Azure Bastion nicht verwenden, um eine Verbindung mit Azure VMware Solution-VMs herzustellen, da es sich hierbei nicht um Azure-IaaS-Objekte handelt.  

Als bewährte Sicherheitsmethode sollten Sie den [Microsoft Azure Bastion](../bastion/index.yml)-Dienst im virtuellen Hub-Netzwerk bereitstellen. Azure Bastion bietet nahtlosen RDP- und SSH-Zugriff auf VMs, die in Azure bereitgestellt werden, ohne dass diesen Ressourcen öffentliche IP-Adressen bereitgestellt werden müssen. Sobald Sie den Azure Bastion-Dienst bereitgestellt haben, können Sie über das Azure-Portal auf die ausgewählte VM zugreifen. Nachdem Sie die Verbindung hergestellt haben, wird eine neue Registerkarte geöffnet, auf der der Desktop der Jumpbox angezeigt wird. Über diesen Desktop können Sie auf die Verwaltungsebene der privaten Azure VMware Solution-Cloud zugreifen.

> [!IMPORTANT]
> Sie sollten weder der Jumpbox-VM eine öffentliche IP-Adresse zuweisen noch den TCP-Port 3389 für das öffentliche Internet verfügbar machen. 


:::image type="content" source="media/hub-spoke/azure-bastion-hub-vnet.png" alt-text="Hub-and-Spoke-Bereitstellung mit Azure VMware Solution-Integration" border="false":::


## <a name="azure-dns-resolution-considerations"></a>Aspekte zu Azure DNS Auflösung

Für Azure DNS-Auflösung stehen zwei Optionen zur Verfügung:

-   Verwenden Sie die Azure Active Directory-Domänencontroller, die im Hub bereitgestellt werden (beschrieben unter [Überlegungen zur Identität](#identity-considerations)) als Namenserver.

-   Konfigurieren Sie eine private Azure DNS-Zone, und stellen Sie diese bereit.

Die beste Vorgehensweise besteht darin, beide Optionen zu kombinieren, um eine zuverlässige Namensauflösung für Azure VMware Solution, lokale Systeme und Azure bereitzustellen.

Als allgemeine Entwurfsempfehlung sollten Sie die vorhandene Azure DNS-Infrastruktur (in diesem Fall Active Directory-integriertes DNS), die auf mindestens zwei Azure-VMs bereitgestellt ist, die im virtuellen Hub-Netzwerk bereitgestellt und in den virtuellen Spoke-Netzwerken konfiguriert sind, damit diese Azure DNS-Server in den DNS-Einstellungen verwendet werden.

Privates Azure-DNS kann weiterhin verwendet werden, wobei die Zone mit privatem Azure-DNS mit den virtuellen Netzwerken verknüpft ist. Außerdem werden DNS-Server als Hybrid-Konfliktlöser mit bedingter Weiterleitung an lokale/Azure VMware Solution-bezogene DNS-Namen verwendet, wozu die Infrastruktur für privates Azure-DNS des Kunden genutzt wird.

Es gibt verschiedene Aspekte, die für private Azure DNS-Zonen zu berücksichtigen sind:

* Die automatische Registrierung sollte für Azure DNS aktiviert werden, sodass der Lebenszyklus der DNS-Einträge für die in virtuellen Spoke-Netzwerken bereitgestellten VMs automatisch verwaltet wird.
* Die maximale Anzahl von privaten DNS-Zonen, mit denen ein virtuelles Netzwerk bei aktivierter automatischer Registrierung verknüpft werden kann, beträgt nur eins.
* Die maximale Anzahl von privaten DNS-Zonen, mit denen ein virtuelles Netzwerk verknüpft werden kann, beträgt 1000, wenn die automatische Registrierung nicht aktiviert ist.

Lokale und Azure VMware Solution-Server können mit bedingten Weiterleitungen an Konfliktlöser-VMs in Azure für die Zone mit privatem Azure-DNS konfiguriert werden.

## <a name="identity-considerations"></a>Überlegungen zur Identität

Aus Gründen der Identität besteht die beste Vorgehensweise darin, mindestens einen AD-Domänencontroller auf dem Hub bereitzustellen, wozu das freigegebene Dienstsubnetz verwendet wird, idealerweise zwei dieser Controller in Form einer Zonenbereitstellung oder in einer VM-Verfügbarkeitsgruppe. Informationen dazu, wie Sie Ihre lokale AD-Domäne auf Azure erweitern, finden Sie unter [Erweitern der lokalen Active Directory-Domäne auf Azure](/azure/architecture/reference-architectures/identity/adds-extend-domain).

Stellen Sie außerdem einen weiteren Domänencontroller auf der Azure VMware Solution-Seite bereit, der in der vSphere-Umgebung als Identitäts- und DNS-Quelle fungiert.

Als beste Vorgehensweise empfiehlt es sich, [AD-Domänen in Azure Active Directory](/azure/architecture/reference-architectures/identity/azure-ad) zu integrieren.

<!-- LINKS - external -->
[Azure Architecture Center]: /azure/architecture/

[Hub & Spoke topology]: /azure/architecture/reference-architectures/hybrid-networking/hub-spoke

[Azure networking documentation]: ../networking/index.yml

<!-- LINKS - internal -->
