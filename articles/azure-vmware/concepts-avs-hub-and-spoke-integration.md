---
title: 'Konzept: Integrieren einer Azure VMware Solution-Bereitstellung (AVS-Bereitstellung) in eine Hub-and-Spoke-Architektur'
description: Es werden die Empfehlungen vorgestellt, gemäß denen eine AVS-Bereitstellung (Azure VMware Solution) in eine vorhandene oder neue Hub-and-Spoke-Architektur in Azure integriert wird.
ms.topic: conceptual
ms.date: 06/23/2020
ms.openlocfilehash: 82937e04fc0a5101c353702b92b6b068d027d7ad
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85374973"
---
# <a name="integrate-azure-vmware-solution-avs-in-a-hub-and-spoke-architecture"></a>Integrieren von Azure VMware Solution (AVS) in eine Hub-and-Spoke-Architektur

In diesem Artikel finden Sie Empfehlungen für die Integration einer Azure VMware Solution-Bereitstellung (AVS-Bereitstellung) in eine vorhandene oder neue [Hub-and-Spoke-Architektur](https://docs.microsoft.com/azure/architecture/reference-architectures/hybrid-networking/shared-services) in Azure. 

Für das Hub-and-Spoke-Szenario wird eine Hybrid Cloud-Umgebung mit Workloads für folgende Komponenten angenommen:

* Natives Azure mit IaaS- oder PaaS-Diensten
* AVS 
* vSphere lokal

## <a name="architecture"></a>Aufbau

Der *Hub* ist ein virtuelles Azure-Netzwerk, das als zentraler Konnektivitätspunkt für Ihr lokales Rechenzentrum und Ihre private AVS-Cloud fungiert. Die *Spokes* sind virtuelle Netzwerke, die gleichrangig mit dem Hub verbunden sind, um Kommunikation zwischen ihnen zu ermöglichen.

Der Datenverkehr zwischen dem lokalen Rechenzentrum, der privaten AVS-Cloud und dem Hub läuft über ExpressRoute-Verbindungen. Ein virtuelles Spoke-Netzwerke enthält in der Regel IaaS-basierte Workloads, kann aber auch PaaS-Dienste wie [App Service-Umgebung](../app-service/environment/intro.md), die direkt in Virtual Network integriert sind, oder andere PaaS-Dienste mit aktiviertem [Azure Private Link](https://docs.microsoft.com/azure/private-link/) enthalten. 

Das Diagramm zeigt ein Beispiel für eine Hub-and-Spoke-Bereitstellung in Azure, die über ExpressRoute mit dem lokalen Rechenzentrum und der AVS-Cloud verbunden ist.

:::image type="content" source="./media/hub-spoke/avs-hub-and-spoke-deployment.png" alt-text="Hub-and-Spoke-Bereitstellung mit AVS-Integration":::




Diese Architektur besteht aus den folgenden Hauptkomponenten:

-   **Lokaler Standort:** Lokale Rechenzentren des Kunden, die über ExpressRoute-Verbindungen mit Azure verbunden sind.

-   **Private AVS-Cloud:** Das aus einem oder mehreren vSphere-Clustern bestehende AVS Software Defined Data Center (SDDC), jeweils mit maximal 16 Knoten.

-   **ExpressRoute-Gateway:** Ermöglicht die Kommunikation zwischen privater AVS-Cloud, dem lokalen Netzwerk, freigegebenen Diensten im virtuellen Hub-Netzwerk und Workloads, die in virtuellen Spoke-Netzwerken ausgeführt werden.

    > [!NOTE]
    > **S2S-VPN-Überlegungen:** Für die Bereitstellung von AVS-Produktionsumgebungen wird Azure S2S wegen Netzwerkanforderungen für HCX nicht unterstützt. Für eine Proof of Concept- oder nicht zur Produktion vorgesehene Bereitstellung, die kein HCX erfordert, kann S2S jedoch verwendet werden.

-   **Virtuelles Hub-Netzwerk:** Fungiert als zentraler Verbindungspunkt für Ihr lokales Netzwerk und Ihre private AVS-Cloud.

-   **Virtuelles Spoke-Netzwerk**

    -   **IaaS-Spoke:** In einer IaaS-Spoke werden IaaS-basierte Azure-Workloads gehostet, wozu VM-Verfügbarkeitsgruppen und VM-Skalierungsgruppen sowie die entsprechenden Netzwerkkomponenten gehören.

    -   **PaaS-Spoke:** In einer PaaS-Spoke werden Azure-PaaS-Dienste über private Adressierung gehostet, wozu [privater Endpunkt](https://docs.microsoft.com/azure/private-link/private-endpoint-overview) (Private Endpoint) und [Private Link](https://docs.microsoft.com/azure/private-link/private-link-overview) verwendet werden.

-   **Azure Firewall:** Fungiert als das zentrale Element zum Segmentieren von Datenverkehr zwischen den Spokes, den lokalen Rechenzentren und AVS.

-   **Application Gateway:** Macht Webanwendungen, die entweder auf Azure IaaS/PaaS- oder AVS-VMs ausgeführt werden, verfügbar und schützt diese. Dieser Dienst lässt sich mit anderen Diensten wie API Management kombinieren.

## <a name="network-and-security-considerations"></a>Netzwerk- und Sicherheitsaspekte

ExpressRoute-Verbindungen ermöglichen Datenverkehr zwischen lokalen Systemen, AVS und dem Azure-Netzwerkfabric. Für AVS wird [ExpressRoute Global Reach](https://docs.microsoft.com/azure/expressroute/expressroute-global-reach) verwendet, um diese Konnektivität zu implementieren.

Auch für lokale Konnektivität kann ExpressRoute Global Reach verwenden, dies ist jedoch nicht zwingend erforderlich.

* **Datenverkehr zwischen lokalem Rechenzentrum und AVS**

  :::image type="content" source="media/hub-spoke/on-prem-to-avs-traffic-flow.png" alt-text="Datenverkehr zwischen lokalem Rechenzentrum und AVS":::


* **Datenverkehr zwischen AVS und Hub-VNET**

  :::image type="content" source="media/hub-spoke/avs-to-hub-vnet-traffic-flow.png" alt-text="Datenverkehr zwischen AVS und virtuellem Hub-Netzwerk":::


Weitere Details zu den AVS-Netzwerk- und -Konnektivitätskonzepten finden Sie in der [AVS-Produktdokumentation](https://docs.microsoft.com/azure/azure-vmware/concepts-networking).

### <a name="traffic-segmentation"></a>Datenverkehrsegmentierung

[Azure Firewall](https://docs.microsoft.com/azure/firewall/) ist das zentrale Element der Hub-and-Spoke-Topologie und wird im virtuellen Hub-Netzwerk bereitgestellt. Sie verwenden Azure Firewall oder ein anderes von Azure unterstütztes virtuelles Netzwerkgerät, um Datenverkehrsregeln einzurichten und die Kommunikation zwischen den verschiedenen Spokes, lokalen Systemen und AVS-Workloads zu segmentieren.

Erstellen Sie Routingtabellen, um den Datenverkehr an Azure Firewall zu leiten.  Erstellen Sie für die virtuellen Spoke-Netzwerke eine Route, mit der die Standardroute auf die interne Schnittstelle von Azure Firewall festgelegt wird. Über diese Vorgabe kann die Firewall, wenn eine Workload im virtuellen Netzwerk den AVS-Adressraum erreichen muss, die Workload auswerten und die entsprechende Datenverkehrsregel anwenden, um sie zuzulassen oder abzulehnen.  

:::image type="content" source="media/hub-spoke/create-route-table-to-direct-traffic.png" alt-text="Routingtabellen erstellen, um Datenverkehr an Azure Firewall zu leiten":::


> [!IMPORTANT]
> Eine Route, die das Adresspräfix 0.0.0.0/0 in der **GatewaySubnet**-Einstellung hat, wird nicht unterstützt.

Legen Sie Routen für bestimmte Netzwerke in der entsprechenden Routingtabelle fest. So zum Beispiel Routen zum Erreichen von AVS-Verwaltung und -Workloads, IP-Präfixen von lokalen Systemen und umgekehrt, wodurch Datenverkehr von den lokalen Systemen an die private AVS-Cloud über Azure Firewall geleitet wird.

:::image type="content" source="media/hub-spoke/specify-gateway-subnet-for-route-table.png" alt-text="Routen für bestimmte Netzwerke in der entsprechenden Routingtabelle festlegen":::

Eine zweite Ebene der Datenverkehrssegmentierung mithilfe der Netzwerksicherheitsgruppen in den Spokes und im Hub, um eine präzisere Datenverkehrsrichtlinie zu erstellen. 


### <a name="application-gateway"></a>Application Gateway

Azure Application Gateway V1 und V2 wurden mit Web-Apps getestet, die auf AVS-VMs als Back-End-Pool ausgeführt werden. Application Gateway ist derzeit die einzige unterstützte Methode, um Web-Apps, die auf AVS-VMs ausgeführt werden, im Internet verfügbar zu machen. Application Gateway kann die Apps auch für interne Benutzer sicher verfügbar machen.

:::image type="content" source="media/hub-spoke/avs-second-level-traffic-segmentation.png" alt-text="Zweite Ebene der Datenverkehrssegmentierung über die Netzwerksicherheitsgruppen":::


### <a name="jumpbox-and-azure-bastion"></a>Jumpbox und Azure Bastion

Greifen Sie über einen Jump Server (Jumpbox) auf die AVS-Umgebung zu. Ein Jump Server ist eine Windows 10- oder Windows Server-VM, die im freigegebenen Dienstsubnetz im virtuellen Hub-Netzwerk bereitgestellt wird.

Als bewährte Sicherheitsmethode sollten Sie den [Microsoft Azure Bastion](https://docs.microsoft.com/azure/bastion/)-Dienst im virtuellen Hub-Netzwerk bereitstellen. Azure Bastion bietet nahtlosen RDP- und SSH-Zugriff auf VMs, die in Azure bereitgestellt werden, ohne dass diesen Ressourcen öffentliche IP-Adressen bereitgestellt werden müssen. Sobald Sie den Azure Bastion-Dienst bereitgestellt haben, können Sie über das Azure-Portal auf die ausgewählte VM zugreifen. Nachdem Sie die Verbindung hergestellt haben, wird eine neue Registerkarte geöffnet, auf der der Jumpbox-Desktop angezeigt wird. Über diesen Desktop können Sie auf die Verwaltungsebene der privaten AVS-Cloud zugreifen.

> [!IMPORTANT]
> Sie sollten weder der Jumpbox-VM eine öffentliche IP-Adresse zuweisen noch den TCP-Port 3389 für das öffentliche Internet verfügbar machen. 


:::image type="content" source="media/hub-spoke/azure-bastion-hub-vnet.png" alt-text="Virtuelles Hub-Netzwerk mit Azure Bastion":::


## <a name="azure-dns-resolution-considerations"></a>Aspekte zu Azure DNS Auflösung

Für Azure DNS-Auflösung stehen zwei Optionen zur Verfügung:

-   Verwenden Sie die Azure Active Directory-Domänencontroller, die im Hub bereitgestellt werden (beschrieben unter [Überlegungen zur Identität](#identity-considerations)) als Namenserver.

-   Konfigurieren Sie eine private Azure DNS-Zone, und stellen Sie diese bereit.

Die beste Vorgehensweise besteht darin, beide Optionen zu kombinieren, um eine zuverlässige Namensauflösung für AVS, lokale Systeme und Azure bereitzustellen.

Als allgemeine Entwurfsempfehlung sollten Sie die vorhandene Azure DNS-Infrastruktur (in diesem Fall Active Directory-integriertes DNS), die auf mindestens zwei Azure-VMs bereitgestellt ist, die im virtuellen Hub-Netzwerk bereitgestellt und in den virtuellen Spoke-Netzwerken konfiguriert sind, damit diese Azure DNS-Server in den DNS-Einstellungen verwendet werden.

Azure Privates DNS kann weiterhin verwendet werden, wobei die Zone mit privatem Azure-DNS mit den virtuellen Netzwerken verknüpft ist. Außerdem werden DNS-Server als Hybrid-Konfliktlöser mit bedingter Weiterleitung an lokale/AVS-bezogene DNS-Namen verwendet, wozu die Privates DNS-Azure-Infrastruktur des Kunden genutzt wird.

Es gibt verschiedene Aspekte, die für private Azure DNS-Zonen zu berücksichtigen sind:

* Die automatische Registrierung sollte für Azure DNS aktiviert werden, sodass der Lebenszyklus der DNS-Einträge für die in virtuellen Spoke-Netzwerken bereitgestellten VMs automatisch verwaltet wird.
* Die maximale Anzahl von privaten DNS-Zonen, mit denen ein virtuelles Netzwerk bei aktivierter automatischer Registrierung verknüpft werden kann, beträgt nur eins.
* Die maximale Anzahl von privaten DNS-Zonen, mit denen ein virtuelles Netzwerk verknüpft werden kann, beträgt 1000, wenn die automatische Registrierung nicht aktiviert ist.

Lokale und AVS-Server können mit bedingten Weiterleitungen an Konfliktlöser-VMs in Azure für die Zone mit privatem Azure-DNS konfiguriert werden.

## <a name="identity-considerations"></a>Überlegungen zur Identität

Aus Gründen der Identität besteht die beste Vorgehensweise darin, mindestens einen AD-Domänencontroller auf dem Hub bereitzustellen, wozu das freigegebene Dienstsubnetz verwendet wird, idealerweise zwei dieser Controller in Form einer Zonenbereitstellung oder in einer VM-Verfügbarkeitsgruppe. Informationen dazu, wie Sie Ihre lokale AD-Domäne auf Azure erweitern, finden Sie unter [Erweitern der lokalen Active Directory-Domäne auf Azure](https://docs.microsoft.com/azure/architecture/reference-architectures/identity/adds-extend-domain).

Stellen Sie außerdem einen weiteren Domänencontroller auf der AVS-Seite bereit, der in der vSphere-Umgebung als Identitäts- und DNS-Quelle fungiert.

Legen Sie für vCenter und SSO eine Identitätsquelle im Azure-Portal über **Verwalten \> Identität\> Identitätsquellen** fest.

Als beste Vorgehensweise empfiehlt es sich, [AD-Domänen in Azure Active Directory](https://docs.microsoft.com/azure/architecture/reference-architectures/identity/azure-ad) zu integrieren.

<!-- LINKS - external -->
[Azure Architecture Center]: https://docs.microsoft.com/azure/architecture/

[Hub & Spoke topology]: https://docs.microsoft.com/azure/architecture/reference-architectures/hybrid-networking/hub-spoke

[Azure networking documentation]: https://docs.microsoft.com/azure/networking/

<!-- LINKS - internal -->


