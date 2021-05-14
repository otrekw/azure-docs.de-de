---
title: Best Practices für Netzwerkressourcen
titleSuffix: Azure Kubernetes Service
description: Lernen Sie die Best Practices für virtuelle Netzwerkressourcen und -konnektivität in Azure Kubernetes Service (AKS) für Clusteroperator kennen.
services: container-service
ms.topic: conceptual
ms.date: 03/10/2021
ms.openlocfilehash: 1e0212766e7d5443664d57a97cfa9ea9d0035da3
ms.sourcegitcommit: 5f482220a6d994c33c7920f4e4d67d2a450f7f08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/08/2021
ms.locfileid: "107104948"
---
# <a name="best-practices-for-network-connectivity-and-security-in-azure-kubernetes-service-aks"></a>Best Practices für Netzwerkkonnektivität und Sicherheit in Azure Kubernetes Service (AKS)

Beim Erstellen und Verwalten von Clustern in Azure Kubernetes Service (AKS) stellen Sie die Netzwerkkonnektivität für Ihre Knoten und Anwendungen bereit. Die Netzwerkressourcen umfassen IP-Adressbereiche, Lastenausgleichsmodule und Controller für eingehenden Datenverkehr. Um eine hohe Servicequalität für Ihre Anwendungen zu gewährleisten, müssen Sie diese Ressourcen strategisch planen und konfigurieren.

In diesem Artikel zu bewährten Methoden liegt der Schwerpunkt auf der Netzwerkkonnektivität und der Sicherheit für Clusteroperatoren. In diesem Artikel werden folgende Vorgehensweisen behandelt:

> [!div class="checklist"]
> * Vergleichen der Netzwerkmodi kubenet und Azure CNI (Container Networking Interface) in AKS
> * Planen der erforderlichen IP-Adressierung und -Konnektivität
> * Verteilen von Datenverkehr mit Lastenausgleichsmodulen, Eingangsdatencontrollern oder einer Web Application Firewall (WAF)
> * Herstellen sicherer Verbindungen mit Clusterknoten

## <a name="choose-the-appropriate-network-model"></a>Auswählen des geeigneten Netzwerkmodells

> **Best Practices-Leitfaden** 
> 
> Verwenden Sie Azure CNI-Netzwerke in AKS für die Integration in vorhandene virtuelle oder lokale Netzwerke. Dieses Netzwerkmodell ermöglicht eine stärkere Trennung von Ressourcen und Steuerelementen in einer Unternehmensumgebung.

Virtuelle Netzwerke bieten die grundlegende Konnektivität für AKS-Knoten und Kunden für den Zugriff auf Ihre Anwendungen. Es gibt zwei verschiedene Möglichkeiten, AKS-Cluster in virtuellen Netzwerken bereitzustellen:

* **Azure CNI-Netzwerk**

    Wird in einem virtuellen Netzwerk bereitgestellt und verwendet das [Azure CNI][cni-networking]- Kubernetes-Plugin. Pods erhalten einzelne IP-Adressen, die an anderen Netzwerkdienste oder lokale Ressourcen weitergeleitet werden können.
* **Kubenet-Netzwerk**

    Azure verwaltet die virtuellen Netzwerkressourcen während der Bereitstellung des Clusters und verwendet das Kubernetes-Plug-In [kubenet][kubenet].


Bei Produktionsbereitstellungen sind sowohl kubenet als auch Azure CNI gültige Optionen.

### <a name="cni-networking"></a>CNI-Netzwerke

Das Azure CNI ist ein herstellerneutrales Protokoll, mit dem die Containerruntime Anforderungen an einen Netzwerkanbieter richten kann. Es weist Pods und Knoten IP-Adressen zu und stellt Features zur IP-Adressverwaltung (IPAM) bereit, wenn Sie die Verbindung zu bestehenden virtuellen Azure-Netzwerken herstellen. Jeder Knoten und jede Podressource erhält eine IP-Adresse im virtuellen Azure-Netzwerk, und es ist kein zusätzliches Routing erforderlich, um mit anderen Ressourcen oder Diensten zu kommunizieren.

![Diagramm mit zwei Knoten jeweils mit Bridges für die Verbindungsherstellung mit einem Azure VNET](media/operator-best-practices-network/advanced-networking-diagram.png)

Ein beachtlicher Vorteil des Azure CNI-Netzwerks besteht darin, dass eine Trennung von Steuerung und Verwaltung von Ressourcen möglich ist. Aus Sicherheitssicht möchten Sie diese Ressourcen oft von verschiedenen Teams verwalten und absichern lassen. Mit Azure CNI-Netzwerken können Sie die Verbindung zu vorhandenen Azure-Ressourcen, lokalen Ressourcen oder anderen Diensten direkt über die jedem Pod zugeordneten IP-Adressen herstellen.

Wenn Sie Azure CNI-Netzwerke verwenden, befindet sich die virtuelle Netzwerkressource in einer vom AKS-Cluster getrennten Ressourcengruppe. Delegieren Sie Berechtigungen für die AKS-Clusteridentität, um auf diese Ressourcen zugreifen und sie verwalten zu können. Die vom AKS-Cluster verwendete Clusteridentität muss mindestens Berechtigungen [Netzwerkmitwirkender](../role-based-access-control/built-in-roles.md#network-contributor) für das Subnetz in Ihrem virtuellen Netzwerk haben. 

Wenn Sie eine [benutzerdefinierte Rolle](../role-based-access-control/custom-roles.md) anstelle der integrierten Rolle des Netzwerkmitwirkenden definieren möchten, sind die folgenden Berechtigungen erforderlich:
  * `Microsoft.Network/virtualNetworks/subnets/join/action`
  * `Microsoft.Network/virtualNetworks/subnets/read`

Standardmäßig verwendet AKS eine verwaltete Identität als Clusteridentität. Sie können jedoch stattdessen auch einen Dienstprinzipal verwenden. Weitere Informationen:
* AKS-Dienstprinzipaldelegierung finden Sie unter [Delegieren des Zugriffs auf andere Azure-Ressourcen][sp-delegation]. 
* Verwaltete Identitäten finden Sie unter [Verwenden verwalteter Identitäten.](use-managed-identity.md)

Da jeder Knoten und Pod seine eigene IP-Adresse erhält, planen Sie die Adressbereiche für die AKS-Subnetze. Bedenken Sie Folgendes:
* Das Subnetz muss groß genug sein, um IP-Adressen für jeden Knoten, jeden Pod und jede bereitgestellte Netzwerkressource zu bieten. 
    * Sowohl beim kubenet- als auch beim Azure CNI-Netzwerk ist die Anzahl der Pods für jeden ausgeführten Knoten standardmäßig begrenzt.
* Jeder AKS-Cluster muss in einem eigenen Subnetz platziert werden. 
* Vermeiden Sie die Verwendung von IP-Adressbereichen, die sich mit vorhandenen Netzwerkressourcen überschneiden. 
    * Erforderlich, um Konnektivität mit lokalen oder Peernetzwerken in Azure zu ermöglichen.
* Um Aufskalierungsereignisse oder Clusterupgrades verarbeiten zu können, benötigen Sie zusätzliche IP-Adressen, die im zugewiesenen Subnetz zur Verfügung stehen. 
    * Dieser zusätzliche Adressraum ist besonders wichtig, wenn Sie Windows Server-Container verwenden, da diese Knotenpools ein Upgrade erfordern, damit die neuesten Sicherheitspatches angewendet werden. Weitere Informationen zu Windows Server-Knoten finden Sie unter [Durchführen eines Upgrades für einen Knotenpool in AKS][nodepool-upgrade].

Informationen zum Berechnen der erforderlichen IP-Adresse finden Sie unter [Konfigurieren von Azure CNI-Netzwerken in AKS][advanced-networking].

Beim Erstellen eines Clusters mit Azure CNI-Netzwerken geben Sie andere Adressbereiche für den Cluster an, z. B. die Docker-Bridgeadresse, die DNS-Dienst-IP und den Dienstadressbereich. Stellen Sie im Allgemeinen sicher, dass diese Adressbereiche:
* Sich nicht überlappen.
* Sich nicht mit Netzwerken überlappen, die dem Cluster zugeordnet sind, einschließlich aller virtuellen Netzwerke, Subnetze, lokaler Netzwerke und Peernetzwerke. 

Ausführliche Informationen zu Grenzwerten und zur Größenanpassung für diese Adressbereiche finden Sie unter [Konfigurieren von Azure CNI-Netzwerken in AKS][advanced-networking].

### <a name="kubenet-networking"></a>Kubenet-Netzwerke

Für „kubernet“ ist es zwar nicht erforderlich, dass Sie die virtuellen Netzwerke vor der Bereitstellung des Clusters einrichten, aber es gibt Nachteile durch das Warten:

* Da sich Knoten und Pods in verschiedenen IP-Subnetzen befinden, wird der Datenverkehr zwischen Pods und Knoten über benutzerdefiniertes Routing (UDR) und IP-Weiterleitung abgewickelt. Dieses zusätzliche Routing kann die Netzwerkleistung reduzieren.
* Verbindungen zu bestehenden lokalen Netzwerken oder das Peering mit anderen virtuellen Netzwerken von Azure können komplex sein.

Da Sie das virtuelle Netzwerk und die Subnetze nicht getrennt vom AKS-Cluster erstellen, eignet sich Kubenet ideal für:
* Kleine Entwicklungs- oder Testworkloads. 
* Einfache Websites mit geringem Datenverkehr.
* Heben und Verschieben von Workloads in Container.

Für die meisten Produktionsbereitstellungen sollten Sie Azure CNI-Netzwerke planen und verwenden.

Sie können auch [Ihre eigenen IP-Adressbereiche und virtuellen Netzwerke mit kubenet][aks-configure-kubenet-networking] konfigurieren. Ähnlich wie bei Azure CNI-Netzwerken dürfen sich diese Adressbereiche nicht gegenseitig oder mit Netzwerken überlappen, die dem Cluster zugeordnet sind (alle virtuellen Netzwerke, Subnetze, lokaler Netzwerke und Peernetzwerke). 

Ausführliche Informationen zu Grenzwerten und zur Größenanpassung für diese Adressbereiche finden Sie unter [Verwenden von kubenet-Netzwerken mit Ihren eigenen IP-Adressbereichen in AKS][aks-configure-kubenet-networking].

## <a name="distribute-ingress-traffic"></a>Verteilen des Eingangsdatenverkehrs

> **Best Practices-Leitfaden** 
> 
> Um HTTP- oder HTTPS-Datenverkehr an Ihre Anwendungen zu verteilen, verwenden Sie Eingangsressourcen und -controller. Im Vergleich zu einem normalen Azure Load Balancer bieten Eingangscontroller zusätzliche Features und können als native Kubernetes-Ressourcen verwaltet werden.

Ein Azure Load Balancer kann zwar den Kundendatenverkehr auf Anwendungen in Ihrem AKS-Cluster verteilen, aber ist nur begrenzt in der Lage, diesen Datenverkehr zu erkennen. Eine Lastenausgleichsressource wird auf Ebene 4 ausgeführt und verteilt den Datenverkehr basierend auf dem Protokoll oder den Ports. 

Die meisten Webanwendungen, die HTTP oder HTTPS verwenden, sollten Kubernetes-Eingangsressourcen und -controller verwenden, die auf Ebene 7 ausgeführt werden. Eingangsdatenverkehr kann den Datenverkehr basierend auf der URL der Anwendung verteilen und die TLS/SSL-Terminierung behandeln. Eingangsdatenverkehr reduziert auch die Anzahl der IP-Adressen, die Sie freigeben und zuordnen. 

Bei einem Load Balancer benötigt jede Anwendung typischerweise eine öffentliche IP-Adresse, die dem Dienst im AKS-Cluster zugewiesen und zugeordnet wird. Bei Verwendung einer Eingangsressource kann eine einzige IP-Adresse den Datenverkehr auf mehrere Anwendungen verteilen.

![Diagramm mit Eingangsdatenverkehrsfluss in einem AKS-Cluster](media/operator-best-practices-network/aks-ingress.png)

 Es gibt zwei Komponenten für den Eingangsdatenverkehr:

 * Eine *Eingangsressource*
 * Ein *Eingangscontroller*

### <a name="ingress-resource"></a>Eingangsressource

Die *Eingangsressource* ist ein YAML-Manifest von `kind: Ingress`. Sie definiert den Host, die Zertifikate und die Regeln zum Weiterleiten des Datenverkehrs an die Dienste, die in Ihrem AKS-Cluster ausgeführt werden. 

Das folgende YAML-Beispielmanifest verteilt Datenverkehr für *myapp.com* an einen von zwei Diensten, *blogservice* oder *storeservice*. Der Kunde wird basierend auf der URL, auf die zugegriffen wird, an den einen oder anderen Dienst weitergeleitet.

```yaml
kind: Ingress
metadata:
 name: myapp-ingress
   annotations: kubernetes.io/ingress.class: "PublicIngress"
spec:
 tls:
 - hosts:
   - myapp.com
   secretName: myapp-secret
 rules:
   - host: myapp.com
     http:
      paths:
      - path: /blog
        backend:
         serviceName: blogservice
         servicePort: 80
      - path: /store
        backend:
         serviceName: storeservice
         servicePort: 80
```

### <a name="ingress-controller"></a>Eingangscontroller

Ein *Eingangscontroller* ist ein Daemon, der auf einem AKS-Knoten ausgeführt wird und diesen auf eingehende Anforderungen überwacht. Datenverkehr wird dann entsprechend den in der Eingangsressource definierten Regeln verteilt. Während der gängigste Eingangscontroller auf [NGINX] basiert, beschränkt AKS Sie nicht auf einen bestimmten Controller. Sie können [Contour][contour], [HAProxy,][haproxy] [Traefik][traefik] usw. verwenden.

Eingangscontroller müssen auf einem Linux-Knoten geplant werden. Geben Sie an, dass die Ressource auf einem Linux-basierten Knoten ausgeführt werden soll, indem Sie einen Knotenselektor in Ihrem YAML-Manifest oder Ihrem bereitgestellten Helm-Diagramm verwenden. Weitere Informationen finden Sie unter [Verwenden von Knotenselektoren, um zu steuern, wo Pods in AKS geplant werden][concepts-node-selectors].

> [!NOTE]
> Windows Server-Knoten dürfen nicht auf dem Eingangscontroller ausgeführt werden.

Es gibt viele Szenarien für Eingangsdatenverkehr, einschließlich der folgenden Anleitungen:

* [Erstellen eines einfachen Eingangscontrollers mit Konnektivität mit einem externen Netzwerk][aks-ingress-basic]
* [Erstellen eines Eingangscontrollers, der ein internes, privates Netzwerk und eine IP-Adresse verwendet][aks-ingress-internal]
* [Erstellen eines Eingangscontrollers, der Ihre eigenen TLS-Zertifikate verwendet][aks-ingress-own-tls]
* Erstellen eines Eingangscontrollers, der Let's Encrypt für das automatische Generieren von TLS-Zertifikaten [mit einer dynamischen öffentlichen IP-Adresse][aks-ingress-tls] oder [mit einer statischen öffentlichen IP-Adresse][aks-ingress-static-tls] verwendet

## <a name="secure-traffic-with-a-web-application-firewall-waf"></a>Absichern des Datenverkehrs mit einer Web Application Firewall (WAF)

> **Best Practices-Leitfaden**
> 
> Verwenden Sie eine Web Application Firewall (WAF) wie [Barracuda WAF für Azure][barracuda-waf] oder Azure Application Gateway, um eingehenden Datenverkehr auf mögliche Angriffe zu überprüfen. Diese fortschrittlicheren Netzwerkressourcen können den Datenverkehr auch über reine HTTP- und HTTPS-Verbindungen oder eine einfache TLS-Terminierung hinaus routen.

Normalerweise ist ein Eingangscontroller eine Kubernetes-Ressource in Ihrem AKS-Cluster, die den Datenverkehr auf Dienste und Anwendungen verteilt. Der Controller wird als Daemon auf einem AKS-Knoten ausgeführt und verbraucht einige der Ressourcen des Knotens wie CPU, Speicher und Netzwerkbandbreite. In größeren Umgebungen haben Sie folgende Möglichkeiten:
* Sie können einen Teil dieses Datenverkehrsroutings oder der TLS-Terminierung häufig auf eine Netzwerkressource außerhalb des AKS-Clusters auslagern.
* Sie können eingehenden Datenverkehr auf potenzielle Angriffe überprüfen.

![Eine Web Application Firewall (WAF) wie Azure App Gateway kann den Datenverkehr für Ihren AKS-Cluster schützen und verteilen.](media/operator-best-practices-network/web-application-firewall-app-gateway.png)

Für diese zusätzliche Sicherheitsebene filtert eine Web Application Firewall (WAF) den eingehenden Datenverkehr. Anhand einer Reihe von Regeln überwacht Open Web Application Security Project (OWASP) den Datenverkehr auf Angriffe wie websiteübergreifende Skripts oder Cookie-Poisoning. [Azure Application Gateway][app-gateway] (aktuell in AKS in der Vorschauphase) ist eine WAF, die in AKS-Cluster integriert ist, um diese Sicherheitsfeatures bereitzustellen, bevor der Datenverkehr Ihren AKS-Cluster und Ihre Anwendungen erreicht. 

Da auch andere Lösungen von Drittanbietern diese Funktionen bieten, können Sie bestehende Investitionen in Ihr bevorzugtes Produkt oder vorhandenes Know-how weiterhin nutzen.

Load Balancer oder Eingangsressourcen werden in Ihrem AKS-Cluster weiterhin ausgeführt und optimieren die Verteilung des Datenverkehrs. App Gateway kann zentral als Eingangscontroller mit einer Ressourcendefinition verwaltet werden. [Erstellen Sie zunächst einen Application Gateway-Eingangscontroller.][app-gateway-ingress]

## <a name="control-traffic-flow-with-network-policies"></a>Steuern des Datenverkehrsflusses mit Netzwerkrichtlinien

> **Best Practices-Leitfaden** 
>
> Verwenden Sie Netzwerkrichtlinien, um Datenverkehr zu den Pods zuzulassen oder zu verweigern. Standardmäßig ist der gesamte Datenverkehr zwischen den Pods in einem Cluster zulässig. Aus Sicherheitsgründen sollten Sie Regeln definieren, um die Kommunikation zwischen den Pods einzuschränken.

Netzwerkrichtlinien sind ein in AKS verfügbares Kubernetes-Feature, mit dem Sie den Datenverkehrsfluss zwischen Pods steuern können. Anhand von Einstellungen wie zugewiesene Bezeichnungen, Namespace oder Port für den Datenverkehr können Sie Datenverkehr zulassen oder verweigern. Netzwerkrichtlinien sind eine cloudnative Möglichkeit, den Datenverkehrsfluss für Pods zu steuern. Wenn Pods in einem AKS-Cluster dynamisch erstellt werden, können automatisch die erforderlichen Netzwerkrichtlinien angewendet werden.

Um Netzwerkrichtlinien verwenden zu können, aktivieren Sie das Feature beim Erstellen eines neuen AKS-Clusters. Ohne einen vorhandenen AKS-Cluster können Sie keine Netzwerkrichtlinie aktivieren. Planen Sie voraus, um die Netzwerkrichtlinie für die erforderlichen Cluster zu aktivieren. 

>[!NOTE]
>Netzwerkrichtlinien sollten nur für Linux-basierte Knoten und Pods in AKS verwendet werden.

Sie erstellen eine Netzwerkrichtlinie mit einem YAML-Manifest als Kubernetes-Ressource. Richtlinien werden auf definierte Pods angewendet, wobei Eingangs- oder Ausgangsregeln den Datenverkehrsfluss definieren. 

Im folgenden Beispiel wird eine Netzwerkrichtlinie auf Pods mit der zugewiesenen Bezeichnung *app: backend* angewendet. Die Eingangsregel erlaubt ausschließlich Datenverkehr von Pods mit der Bezeichnung *app: frontend*:

```yaml
kind: NetworkPolicy
apiVersion: networking.k8s.io/v1
metadata:
  name: backend-policy
spec:
  podSelector:
    matchLabels:
      app: backend
  ingress:
  - from:
    - podSelector:
        matchLabels:
          app: frontend
```

Eine Anleitung für die ersten Schritte mit Richtlinien finden Sie unter [Sicherer Datenverkehr zwischen Pods durch Netzwerkrichtlinien in Azure Kubernetes Service (AKS)][use-network-policies].

## <a name="securely-connect-to-nodes-through-a-bastion-host"></a>Herstellen einer sicheren Verbindung zu Knoten über einen Bastionhost

> **Best Practices-Leitfaden** 
>
> Machen Sie für Ihre AKS-Knoten keine Remotekonnektivität verfügbar. Erstellen Sie einen Bastionhost oder eine Jumpbox in einem virtuellen Verwaltungsnetzwerk. Verwenden Sie den Bastionhost, um den Datenverkehr sicher in Ihren AKS-Cluster für Remoteverwaltungsaufgaben zu routen.

Die meisten Vorgänge in AKS können Sie mit den Azure-Verwaltungstools oder über den Kubernetes API-Server ausführen. AKS-Knoten sind nur in einem privaten Netzwerk verfügbar und sind nicht mit dem öffentlichen Internet verbunden. Um eine Verbindung mit Knoten herzustellen und Wartung und Support bereitzustellen, leiten Sie Ihre Verbindungen über einen Bastionhost oder eine Jumpbox weiter. Stellen Sie sicher, dass sich dieser Host in einem separaten virtuellen Verwaltungsnetzwerk befindet, das sicher per Peering mit dem virtuellen AKS-Clusternetzwerk verbunden ist.

![Verbinden mit AKS-Knoten über einen Bastionhost oder eine Jumpbox](media/operator-best-practices-network/connect-using-bastion-host-simplified.png)

Auch das Verwaltungsnetzwerk für den Bastionhost sollte abgesichert sein. Verwenden Sie ein [Azure ExpressRoute][expressroute] oder [VPN-Gateway][vpn-gateway], um sich mit einem lokalen Netzwerk zu verbinden und den Zugriff über Netzwerksicherheitsgruppen zu steuern.

## <a name="next-steps"></a>Nächste Schritte

Dieser Artikel konzentriert sich auf Netzwerkkonnektivität und Sicherheit. Weitere Informationen zu Netzwerkgrundlagen in Kubernetes finden Sie unter [Netzwerkkonzepte für Anwendungen in Azure Kubernetes Service (AKS)][aks-concepts-network].

<!-- LINKS - External -->
[cni-networking]: https://github.com/Azure/azure-container-networking/blob/master/docs/cni.md
[kubenet]: https://kubernetes.io/docs/concepts/cluster-administration/network-plugins/#kubenet
[app-gateway-ingress]: https://github.com/Azure/application-gateway-kubernetes-ingress
[nginx]: https://www.nginx.com/products/nginx/kubernetes-ingress-controller
[contour]: https://github.com/heptio/contour
[haproxy]: https://www.haproxy.org
[traefik]: https://github.com/containous/traefik
[barracuda-waf]: https://www.barracuda.com/products/webapplicationfirewall/models/5

<!-- INTERNAL LINKS -->
[aks-concepts-network]: concepts-network.md
[sp-delegation]: kubernetes-service-principal.md#delegate-access-to-other-azure-resources
[expressroute]: ../expressroute/expressroute-introduction.md
[vpn-gateway]: ../vpn-gateway/vpn-gateway-about-vpngateways.md
[aks-ingress-internal]: ingress-internal-ip.md
[aks-ingress-static-tls]: ingress-static-ip.md
[aks-ingress-basic]: ingress-basic.md
[aks-ingress-tls]: ingress-tls.md
[aks-ingress-own-tls]: ingress-own-tls.md
[app-gateway]: ../application-gateway/overview.md
[use-network-policies]: use-network-policies.md
[advanced-networking]: configure-azure-cni.md
[aks-configure-kubenet-networking]: configure-kubenet.md
[concepts-node-selectors]: concepts-clusters-workloads.md#node-selectors
[nodepool-upgrade]: use-multiple-node-pools.md#upgrade-a-node-pool