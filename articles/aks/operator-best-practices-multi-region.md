---
title: Bewährte Methoden für Business Continuity & Disaster Recovery für AKS
description: Lernen Sie die Best Practices für Clusteroperator kennen, mit denen Sie die Betriebsbereitschaft Ihrer Anwendungen maximieren und so für Hochverfügbarkeit sorgen und sich auf die Notfallwiederherstellung in Azure Kubernetes Service (AKS) vorbereiten können.
services: container-service
author: lastcoolnameleft
ms.topic: conceptual
ms.date: 03/11/2021
ms.author: thfalgou
ms.custom: fasttrack-edit
ms.openlocfilehash: 5bcd30c22132bc53ff28fdefcb73f686e08e34ea
ms.sourcegitcommit: 5f482220a6d994c33c7920f4e4d67d2a450f7f08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/08/2021
ms.locfileid: "107105084"
---
# <a name="best-practices-for-business-continuity-and-disaster-recovery-in-azure-kubernetes-service-aks"></a>Best Practices für Geschäftskontinuität und Notfallwiederherstellung in Azure Kubernetes Service (AKS)

Bei der Verwaltung von Clustern in Azure Kubernetes Service (AKS) ist die Betriebsbereitschaft Ihrer Anwendungen von größter Bedeutung. Standardmäßig bietet AKS durch die Verwendung mehrerer Knoten pro [VMSS-Instanz (Virtual Machine Scale Sets)](../virtual-machine-scale-sets/overview.md) Hochverfügbarkeit. Diese mehreren Knoten bieten allerdings keinen Schutz Ihres Systems beim Ausfall einer ganzen Region. Um die Betriebszeit zu maximieren, planen Sie voraus, um die Geschäftskontinuität beizubehalten und die Notfallwiederherstellung vorzubereiten.

Thema dieses Artikels sind Planungen zur Geschäftskontinuität und Notfallwiederherstellung in AKS-Cluster. Folgendes wird vermittelt:

> [!div class="checklist"]
> * Planen für AKS-Cluster in mehreren Regionen.
> * Weiterleiten des Datenverkehrs über mehrere Cluster hinweg mithilfe von Azure Traffic Manager.
> * Verwenden der Georeplikation für Ihre Containerimageregistrierungen.
> * Mehrere Cluster übergreifendes Planen für den Anwendungszustand.
> * Mehrere Regionen übergreifendes Replizieren des Speichers.

## <a name="plan-for-multiregion-deployment"></a>Planen für die Bereitstellung in mehreren Regionen

> **Bewährte Methode**
>
> Wenn Sie mehrere AKS-Cluster bereitstellen, wählen Sie Regionen aus, in denen AKS verfügbar ist. Verwenden Sie Regionspaare.

Ein AKS-Cluster wird in einer einzelnen Region bereitgestellt. Um für Schutz Ihres Systems beim Ausfall dieser Region zu sorgen, stellen Sie Ihre Anwendung in mehreren AKS-Clustern in verschiedenen Regionen bereit. Beachten Sie bei der Planung des Bereitstellungsorts für Ihren AKS-Cluster Folgendes:

* [**Verfügbarkeit der AKS-Regionen**](./quotas-skus-regions.md#region-availability)
    * Wählen Sie Regionen in der Nähe Ihrer Benutzer aus. 
    * AKS wird fortlaufend auf neue Regionen ausgeweitet.
* [**Azure-Regionspaare**](../best-practices-availability-paired-regions.md)
    * Wählen Sie für den geografischen Bereich zwei miteinander gekoppelte Regionen aus.
    * Regionspaare koordinieren Plattformupdates und priorisieren bei Bedarf Wiederherstellungsvorgänge.
* **Dienstverfügbarkeit**
    * Entscheiden Sie, ob Ihre Regionspaare heiß/heiß, heiß/warm oder heiß/kalt sein sollen.
    * Möchten Sie beide Regionen gleichzeitig betreiben, sodass eine Region sofort zur Verarbeitung des Datenverkehrs *bereit* ist? Oder:
    * Möchten Sie, dass eine Region eine gewisse Vorlaufzeit hat, bis sie zur Verarbeitung des Datenverkehrs bereit ist?

AKS-Regionsverfügbarkeit und Regionspaare sollten zusammen geplant werden. Stellen Sie Ihre AKS-Cluster in Regionspaaren bereit, die für die gemeinsame Verwaltung der Notfallwiederherstellung in den Regionen konzipiert sind. Ein Beispiel: AKS ist in den Regionen USA, Osten und USA, Westen verfügbar. Diese Regionen sind gekoppelt. Wählen Sie diese beiden Regionen aus, wenn Sie Ihre Strategie für Geschäftskontinuität und Notfallwiederherstellung von AKS entwickeln.

Wenn Sie Ihre Anwendung bereitstellen, fügen Sie auch Ihrer CI/CD-Pipeline einen zusätzlichen Schritt hinzu, um die Bereitstellung in diesen AKS-Clustern zu ermöglichen. Durch das Aktualisieren der Bereitstellungspipelines wird verhindert, dass Anwendungen nur in einer Ihrer Regionen und nur einem Ihrer AKS-Cluster bereitgestellt werden. In diesem Szenario erhält Datenverkehr von Kunden, der an eine sekundäre Region gesendet wird, nicht die neusten Codeupdates.

## <a name="use-azure-traffic-manager-to-route-traffic"></a>Weiterleiten von Datenverkehr mit Azure Traffic Manager

> **Bewährte Methode**
>
> Azure Traffic Manager kann Ihren Datenverkehr an den nächstgelegenen AKS-Cluster und die nächstgelegene Anwendungsinstanz leiten. Um optimale Leistungs- und Redundanzwerte zu erzielen, leiten Sie den gesamten Anwendungsdatenverkehr über Traffic Manager an Ihren AKS-Cluster.

Wenn Sie mehrere AKS-Cluster in verschiedenen Regionen bereitgestellt haben, steuern Sie den Datenverkehrsfluss zu den in jedem Cluster ausgeführten Anwendungen mit Traffic Manager. [Azure Traffic Manager](../traffic-manager/index.yml) ist ein DNS-basierter Lastenausgleichsdienst, der Datenverkehr auf mehrere Regionen verteilen kann. Leiten Sie Benutzerdatenverkehr mit Traffic Manager basierend auf der Antwortzeit des Clusters oder der Geografie weiter.

![AKS mit Traffic Manager](media/operator-best-practices-bc-dr/aks-azure-traffic-manager.png)

Wenn Sie einen einzelnen AKS-Cluster haben, stellen Sie in der Regel eine Verbindung mit der Dienst-IP-Adresse oder dem DNS-Namen einer bestimmten Anwendung her. Bei einer Bereitstellung mit mehreren Clustern sollten Sie eine Verbindung mit einem Traffic Manager-DNS-Namen herstellen, der auf die Dienste in den einzelnen AKS-Clustern verweist. Definieren Sie diese Dienste mithilfe von Traffic Manager-Endpunkten. Jeder Endpunkt stellt die *IP-Adresse des Lastenausgleichsdiensts* dar. Leiten Sie mit dieser Konfiguration Netzwerkdatenverkehr vom Traffic Manager-Endpunkt in einer Region zum Endpunkt in einer anderen Region.

![Geografisches Routing über Traffic Manager](media/operator-best-practices-bc-dr/traffic-manager-geographic-routing.png)

Traffic Manager führt DNS-Lookups durch und gibt den am besten geeigneten Endpunkt zurück. Geschachtelte Profile können einen primären Speicherort priorisieren. Sie sollten beispielsweise eine Verbindung mit der nächstgelegenen geografischen Region herstellen. Wenn in dieser Region ein Problem vorliegt, leitet Traffic Manager Sie an eine sekundäre Region weiter. Diese Vorgehensweise stellt sicher, dass Sie auch dann eine Verbindung mit einer Anwendungsinstanz herstellen können, wenn die nächstgelegene geografische Region nicht verfügbar ist.

Informationen zum Einrichten der Endpunkte und der Weiterleitung finden Sie unter [Konfigurieren der geografischen Routingmethode für Datenverkehr mithilfe von Traffic Manager](../traffic-manager/traffic-manager-configure-geographic-routing-method.md).

### <a name="application-routing-with-azure-front-door-service"></a>Anwendungsrouting mit Azure Front Door Service

[Azure Front Door Service](../frontdoor/front-door-overview.md) verbindet Ihre Endbenutzer mithilfe des Split TCP-basierten Anycast-Protokolls sofort mit dem nächstgelegenen Front Door-POP (Point of Presence). Weitere Features von Azure Front Door Service sind:
* TLS-Terminierung
* Benutzerdefinierte Domäne
* Web Application Firewall
* URL Rewrite
* Sitzungsaffinität 

Überprüfen Sie die Anforderungen des Anwendungsdatenverkehrs, um zu ermitteln, welche Lösung sich am besten eignet.

### <a name="interconnect-regions-with-global-virtual-network-peering"></a>Verbinden von Regionen mit globalem Peering virtueller Netzwerke

Verbinden Sie die beiden virtuellen Netzwerke über das [Peering virtueller Netzwerke](../virtual-network/virtual-network-peering-overview.md) miteinander, um die Kommunikation zwischen Clustern zu ermöglichen. Das Peering virtueller Netzwerke verbindet virtuelle Netzwerke und bietet eine hohe Bandbreite für das Backbone-Netzwerk von Microsoft – auch über geografische Regionen hinweg.

Verwenden Sie vor dem Peering virtueller Netzwerke mit ausgeführten AKS-Clustern den Load Balancer Standard in Ihrem AKS-Cluster. Wenn diese Voraussetzung erfüllt ist, sind Kubernetes-Dienste über das Peering virtueller Netzwerke erreichbar.

## <a name="enable-geo-replication-for-container-images"></a>Aktivieren der Georeplikation für Containerimages

> **Bewährte Methode**
> 
> Speichern Sie Ihre Containerimages in Azure Container Registry (ACR), und erstellen Sie per Georeplikation in jeder AKS-Region eine Instanz der Registry.

Um Ihre Anwendungen in AKS bereitzustellen und zu registrieren, benötigen Sie eine Möglichkeit, die Containerimages zu speichern und abzurufen. Container Registry lässt sich in AKS integrieren, um Containerimages oder Helm-Charts sicher zu speichern. Container Registry unterstützt die Multimaster-Georeplikation, mit der Sie Ihre Images automatisch weltweit in Azure-Regionen replizieren können. 

So verbessern Sie die Leistung und Verfügbarkeit:
1. Erstellen Sie mithilfe der Georeplikation von Container Registry in jeder Region, in der Sie einen AKS-Cluster eingerichtet haben, eine Registrierung. 
1. Jeder AKS-Cluster pullt Containerimages aus der lokalen Container Registry-Instanz in der gleichen Region:

![Container Registry-Georeplikation für Containerimages](media/operator-best-practices-bc-dr/acr-geo-replication.png)

Wenn Sie mit der Container Registry-Georeplikation Images aus der gleichen Region pullen, erzielen Sie folgende Ergebnisse:

* **Schneller**: Sie pullen Images über latenzarme Hochgeschwindigkeits-Netzwerkverbindungen innerhalb derselben Azure-Region.
* **Zuverlässiger**: Wenn eine Region nicht verfügbar ist, pullt Ihr AKS-Cluster die Images aus einer verfügbaren Container Registry-Instanz.
* **Kostengünstiger**: Für den ausgehenden Netzwerkdatenverkehr zwischen Rechenzentren fallen keine Gebühren an.

Die Georeplikation ist ein Feature der *Premium*-SKU von Container Registry. Informationen zum Konfigurieren der Georeplikation finden Sie unter [Georeplikation in Container Registry](../container-registry/container-registry-geo-replication.md).

## <a name="remove-service-state-from-inside-containers"></a>Entfernen des Dienstzustands aus Containern

> **Bewährte Methode**
> 
> Speichern Sie den Dienstzustand nach Möglichkeit nicht im Container. Verwenden Sie stattdessen eine Azure-PaaS-Lösung (Platform as a Service), die die Replikation in mehreren Regionen unterstützt.

Der *Dienstzustand* bezieht sich auf die Daten im Arbeitsspeicher oder auf einem Datenträger, die ein Dienst für seine Ausführung benötigt. Hierzu gehören auch die Datenstrukturen und Membervariablen, die vom Dienst gelesen und geschrieben werden. Je nach Architektur des Diensts kann der Zustand auch Dateien oder andere Ressourcen umfassen, die auf dem Datenträger gespeichert sind. Der Zustand könnte z.B. die Dateien umfassen, die eine Datenbank zum Speichern von Daten und Transaktionsprotokollen verwendet.

Der Zustand kann ausgelagert oder mit dem Code zusammengestellt werden, der den Zustand ändert. In der Regel lagern Sie den Zustand mithilfe einer Datenbank oder einem anderen Datenspeicher aus, der über das Netzwerk auf verschiedenen Computern oder prozessextern auf dem gleichen Computer ausgeführt wird.

Container und Microservices sind dann am stabilsten, wenn die darin ausgeführten Prozesse ihren Zustand nicht beibehalten. Da Anwendungen fast immer einen Zustand enthalten, sollten Sie eine PaaS-Lösung verwenden, z. B.:
* Azure Cosmos DB
* Azure Database for PostgreSQL
* Azure Database for MySQL
* Azure SQL-Datenbank

Wie Sie portable Anwendungen erstellen, können Sie den folgenden Richtlinien entnehmen:

* [The 12-factor app methodology](https://12factor.net/) (Die Zwölf-Faktoren-Methodik für Apps)
* [Ausführen einer Webanwendung in mehreren Azure-Regionen](/azure/architecture/reference-architectures/app-service-web-app/multi-region)

## <a name="create-a-storage-migration-plan"></a>Erstellen eines Plans für die Speichermigration

> **Bewährte Methode**
>
> Wenn Sie Azure Storage verwenden, bereiten Sie die Migration Ihres Speichers von der primären zur Sicherungsregion vor, und testen Sie sie.

Ihre Anwendungen nutzen möglicherweise Azure Storage für ihre Daten. Wenn dies Fall ist, werden Ihre Anwendungen auf mehrere AKS-Cluster in verschiedenen Regionen verteilt. Sie müssen daher dafür sorgen, dass die Speicher synchronisiert werden. Zwei allgemeine Verfahren zum Replizieren von Speicher sind:

* Infrastrukturbasierte asynchrone Replikation
* Anwendungsbasierte asynchrone Replikation

### <a name="infrastructure-based-asynchronous-replication"></a>Infrastrukturbasierte asynchrone Replikation

Ihre Anwendungen erfordern möglicherweise auch dann noch einen beständigen Speicher, nachdem ein Pod gelöscht wurde. In Kubernetes können Sie persistente Volumes verwenden, um Daten dauerhaft zu speichern. Persistente Volumes werden auf einem virtuellen Computerknoten bereitgestellt und dann für die Pods verfügbar gemacht. Persistente Volumes folgen Pods, auch dann, wenn ein Pod auf einen anderen Knoten im gleichen Cluster verschoben wird.

Welche Replikationsstrategie Sie verwenden, hängt von Ihrer Speicherlösung ab. Für die folgenden gängigen Speicherlösungen sind eigene Anleitungen zur Notfallwiederherstellung und Replikation verfügbar:
* [Gluster](https://docs.gluster.org/en/latest/Administrator-Guide/Geo-Replication/)
* [Ceph](https://docs.ceph.com/docs/master/cephfs/disaster-recovery/)
* [Rook](https://rook.io/docs/rook/v1.2/ceph-disaster-recovery.html)
* [Portworx](https://docs.portworx.com/scheduler/kubernetes/going-production-with-k8s.html#disaster-recovery-with-cloudsnaps) 

In der Regel stellen Sie einen gemeinsamen Speicher bereit, in den Anwendungen ihre Daten schreiben. Diese Daten werden dann regionsübergreifend repliziert, und der Zugriff auf die Daten erfolgt lokal.

![Infrastrukturbasierte asynchrone Replikation](media/operator-best-practices-bc-dr/aks-infra-based-async-repl.png)

Wenn Sie Azure Managed Disks verwenden, können Sie [Velero on Azure][velero] und [Kasten][kasten] für die Replikation und Notfallwiederherstellung verwenden. Diese Sicherungslösungen sind nativ in Kubernetes integriert, werden jedoch nicht unterstützt.

### <a name="application-based-asynchronous-replication"></a>Anwendungsbasierte asynchrone Replikation

Kubernetes bietet zurzeit keine native Implementierung für die anwendungsbasierte asynchrone Replikation. Da Container und Kubernetes lose gekoppelt sind, sollte jeder herkömmliche Anwendungs- oder Sprachansatz funktionieren. In der Regel replizieren die Anwendungen selbst die Speicheranforderungen, die dann in den zugrunde liegenden Datenspeicher jedes Clusters geschrieben werden.

![Anwendungsbasierte asynchrone Replikation](media/operator-best-practices-bc-dr/aks-app-based-async-repl.png)

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel werden Überlegungen zur Geschäftskontinuität und Notfallwiederherstellung für AKS-Cluster erläutert. Weitere Informationen zu Clustervorgängen in AKS finden Sie in diesen Artikeln über Best Practices:

* [Best Practices für Clusterisolierung in Azure Kubernetes Service (AKS)][aks-best-practices-cluster-isolation]
* [Best Practices für grundlegende Schedulerfunktionen in Azure Kubernetes Service (AKS)][aks-best-practices-scheduler]

<!-- INTERNAL LINKS -->
[aks-best-practices-scheduler]: operator-best-practices-scheduler.md
[aks-best-practices-cluster-isolation]: operator-best-practices-cluster-isolation.md

[velero]: https://github.com/vmware-tanzu/velero-plugin-for-microsoft-azure/blob/master/README.md
[kasten]: https://www.kasten.io/