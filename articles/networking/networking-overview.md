---
title: Übersicht über Azure-Netzwerkdienste
description: Erfahren Sie etwas über Netzwerkdienste in Azure, darunter Konnektivitäts-, Anwendungsschutz-, Anwendungsbereitstellungs- und Netzwerküberwachungs-Dienste.
services: networking
documentationcenter: na
author: KumudD
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.workload: infrastructure-services
ms.date: 03/12/2020
ms.author: kumud
ms.openlocfilehash: 39601da6cc8e10ba8f4a24eeddb97cfe677c031f
ms.sourcegitcommit: 3be3537ead3388a6810410dfbfe19fc210f89fec
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/10/2020
ms.locfileid: "89651731"
---
# <a name="azure-networking-services-overview"></a>Übersicht über Azure-Netzwerkdienste

Die Azure-Netzwerkdienste bieten eine Vielzahl von Netzwerkfunktionen, die einzeln oder zusammen verwendet werden können. Klicken Sie auf eine der folgenden Schlüsselfunktionen, um weitere Informationen zu erhalten:
- [**Konnektivitätsdienste:** ](#connect) Vernetzen Sie Azure-Ressourcen und lokale Ressourcen mithilfe einer Kombination folgender Netzwerkdienste in Azure: Virtual Network (VNET), Virtual WAN, ExpressRoute, VPN Gateway, Virtual Network NAT Gateway, Azure DNS, Peering Service und Azure Bastion.
- [**Anwendungsschutzdienste**](#protect): Diese Dienste schützen Ihrer Anwendungen mithilfe einer Kombination folgender Netzwerkdienste in Azure: Private Link, DDoS Protection, Firewall, Netzwerksicherheitsgruppen, Web Application Firewall und Virtual Network-Endpunkte.
- [**Dienste zur Anwendungsbereitstellung**](#deliver): Stellen Sie Anwendungen im Azure-Netzwerk mithilfe einer Kombination aus folgenden Netzwerkdiensten in Azure bereit: Content Delivery Network (CDN), Azure Front Door Service, Traffic Manager, Application Gateway, Internet Analyzer und Load Balancer.
- [**Netzwerküberwachung**](#monitor): Überwachen Sie Ihre Netzwerkressourcen mithilfe einer Kombination folgender Netzwerkdienste in Azure: Network Watcher, ExpressRoute-Monitor, Azure Monitor oder Terminalzugangspunkte für virtuelle Netzwerke.

## <a name="connectivity-services"></a><a name="connect"></a>Konnektivitätsdienste
 
In diesem Abschnitt werden Dienste beschrieben, die Konnektivität zwischen Azure-Ressourcen, einem lokalen Netzwerk und Azure-Ressourcen sowie Branch-to-Branch-Konnektivität in Azure ermöglichen: Virtual Network (VNET), Virtual WAN, ExpressRoute, VPN Gateway, Virtual Network NAT Gateway, Azure DNS, Azure Peering Service und Azure Bastion.

|Dienst|Einsatzzweck|Szenarien|
|---|---|---|
|[Virtuelles Netzwerk](#vnet)|Ermöglicht, dass Azure-Ressourcen sicher untereinander sowie mit dem Internet und mit lokalen Netzwerken kommunizieren| <p>[Erstellen von Netzwerksicherheitsgruppen über das Azure-Portal](../virtual-network/tutorial-filter-network-traffic.md)</p> <p>[Weiterleiten von Netzwerkdatenverkehr](../virtual-network/tutorial-create-route-table-portal.md)</p> <p>[Einschränken des Netzwerkzugriffs auf Ressourcen](../virtual-network/tutorial-restrict-network-access-to-resources.md)</p> <p>[Herstellen von Verbindungen zwischen virtuellen Netzwerken](../virtual-network/tutorial-connect-virtual-networks-portal.md)</p>|
|[ExpressRoute](#expressroute)|Erweitert Ihre lokalen Netzwerke über eine private Verbindung, die von einem Konnektivitätsanbieter bereitgestellt wird, auf die Cloud von Microsoft|<p>[Erstellen und Ändern einer ExpressRoute-Verbindung](../expressroute/expressroute-howto-circuit-portal-resource-manager.md)</p> <p>[Erstellen und Ändern des Peerings für eine ExpressRoute-Verbindung](../expressroute/expressroute-howto-routing-portal-resource-manager.md)</p> <p>[Verknüpfen eines VNet mit einer ExpressRoute-Verbindung](../expressroute/expressroute-howto-linkvnet-portal-resource-manager.md)</p> <p>[Konfigurieren und Verwalten von Routenfiltern für ExpressRoute-Verbindungen](../expressroute/how-to-routefilter-portal.md)</p>|
|[VPN Gateway](#vpngateway)|Wird verwendet, um verschlüsselten Datenverkehr zwischen einem virtuellen Azure-Netzwerk und einem lokalen Standort über das öffentliche Internet zu senden|<p>[Site-to-Site-Verbindungen](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md)</p> <p>[VNET-to-VNET-Verbindungen](../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md)</p> <p>[Point-to-Site-Verbindungen](../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md)</p>|
|[Virtual WAN](#virtualwan)|Optimiert und automatisiert die Branchkonnektivität zu und mit Azure: Azure-Regionen dienen als Hubs, die Sie auswählen können, um Branches mit ihnen zu verbinden.|<p>[Site-to-Site-Verbindungen](../virtual-wan/virtual-wan-site-to-site-portal.md), [ExpressRoute-Verbindungen](../virtual-wan/virtual-wan-expressroute-portal.md)</p>|
|[Azure DNS](#dns)|Hostet DNS-Domänen, die die Namensauflösung mithilfe Microsoft Azure-Infrastruktur ermöglichen|<p>[Hosten Ihrer Domäne in Azure DNS](../dns/dns-delegate-domain-azure-dns.md)</p><p>[Erstellen von DNS-Einträgen für eine Web-App](../dns/dns-web-sites-custom-domain.md)</p> <p>[Erstellen eines Aliasdatensatzes für Traffic Manager](../dns/tutorial-alias-tm.md)</p> <p>[Erstellen eines Aliasdatensatzes für eine öffentliche IP-Adresse](../dns/tutorial-alias-pip.md)</p> <p>[Erstellen eines Aliasdatensatzes für Zonenressourcendatensätze](../dns/tutorial-alias-rr.md)</p>|
|[Azure Bastion](#bastion)|Konfigurieren Sie sichere und nahtlose RDP-/SSH-Verbindungen mit Ihren virtuellen Computern über TLS direkt im Azure-Portal. Beim Herstellen einer Verbindung über Azure Bastion benötigen Ihre virtuellen Computer keine öffentliche IP-Adresse.|<p>[Erstellen eines Azure Bastion-Hosts](../bastion/bastion-create-host-portal.md)</p><p>[Herstellen einer Verbindung mit einer Linux-VM über SSH](../bastion/bastion-connect-vm-ssh.md)</p><p>[Herstellen einer Verbindung mit einer Windows-VM über RDP](../bastion/bastion-connect-vm-rdp.md)</p>|
|[Virtual Network NAT Gateway](#nat)|Erstellt ein NAT-Gateway, um ausgehende Konnektivität für einen virtuellen Computer in Azure ermöglichen|<p>[Erstellen eines NAT-Gateways](../virtual-network/quickstart-create-nat-gateway-portal.md)</p>|
|[Azure Peering Service](#azurepeeringservice)|Arbeiten Sie mit Dienstanbietern zusammen, um ein optimales und zuverlässiges Routing zur Microsoft-Cloud über das öffentliche Netzwerk zu erzielen.|<p>[Registrierung für Azure Peering Service](../peering-service/azure-portal.md)</p>|
||||


### <a name="virtual-network"></a><a name="vnet"></a>Virtual Network

Azure Virtual Network (VNET) ist der grundlegende Baustein für Ihr privates Netzwerk in Azure. VNETs können für folgende Zwecke verwendet werden:
- **Kommunikation zwischen Azure-Ressourcen** Sie können virtuelle Computer und verschiedene andere Arten von Azure-Ressourcen in einem virtuellen Netzwerk bereitstellen, wie beispielsweise Azure App Service-Umgebungen, den Azure Kubernetes Service (AKS) und Azure Virtual Machine Scale Sets. Eine vollständige Liste mit Azure-Ressourcen, die Sie in einem virtuellen Netzwerk bereitstellen können, finden Sie unter [Integration virtueller Netzwerke für Azure-Dienste](../virtual-network/virtual-network-for-azure-services.md).
- **Gegenseitige Kommunikation:** Sie können virtuelle Netzwerke mittels VNet-Peering miteinander verbinden und so die Kommunikation zwischen Ressourcen in beiden virtuellen Netzwerken ermöglichen. Die verbundenen virtuellen Netzwerke können sich in der gleichen Azure-Region oder in verschiedenen Azure-Regionen befinden. Weitere Informationen finden Sie unter [Peering in virtuellen Netzwerken](../virtual-network/virtual-network-peering-overview.md).
- **Kommunikation mit dem Internet:** Alle Ressourcen in einem VNET können standardmäßig in ausgehender Richtung mit dem Internet kommunizieren. Zur Kommunikation in eingehender Richtung muss der entsprechenden Ressource eine öffentliche IP-Adresse oder eine öffentliche Load Balancer-Instanz zugewiesen werden. Die [öffentlichen IP-Adressen](../virtual-network/virtual-network-public-ip-address.md) und die öffentliche [Load Balancer-Instanz](../load-balancer/load-balancer-overview.md) können auch zum Verwalten der ausgehenden Verbindungen verwendet werden.
- **Kommunikation mit lokalen Netzwerken:** Sie können Ihre lokalen Computer und Netzwerke mithilfe von [VPN Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md) oder [ExpressRoute](../expressroute/expressroute-introduction.md) mit einem virtuellen Netzwerk vernetzen.

Weitere Informationen finden Sie unter [Was ist Azure Virtual Network?](../virtual-network/virtual-networks-overview.md).

### <a name="expressroute"></a><a name="expressroute"></a>ExpressRoute
Mit ExpressRoute können Sie Ihre lokalen Netzwerke über eine private Verbindung, die von einem Konnektivitätsanbieter bereitgestellt wird, auf die Microsoft-Cloud erweitern. Diese Verbindung ist privat. Der Datenverkehr wird nicht über das Internet übertragen. Mit ExpressRoute können Sie Verbindungen zu Microsoft-Clouddiensten herstellen, z.B. Microsoft Azure, Office 365 und Dynamics 365.  Weitere Informationen finden Sie unter [Was ist ExpressRoute?](../expressroute/expressroute-introduction.md).

![Azure ExpressRoute](./media/networking-overview/expressroute-connection-overview.png)

### <a name="vpn-gateway"></a><a name="vpngateway"></a>VPN Gateway
VPN Gateway unterstützt Sie beim Herstellen verschlüsselter lokaler Verbindungen mit Ihrem virtuellen Netzwerk von lokalen Standorten aus. Sie können auch verschlüsselte Verbindungen zwischen VNETs herstellen. Für VPN Gateway-Verbindungen sind verschiedene Konfigurationen wie Site-to-Site, Point-to-Site oder VNET-to-VNET verfügbar.
Das folgende Diagramm veranschaulicht mehrere Site-to-Site-VPN-Verbindungen mit demselben virtuellen Netzwerk.

![Site-to-Site-Verbindungen über Azure VPN Gateway](./media/networking-overview/vpngateway-multisite-connection-diagram.png)

Weitere Informationen zu den verschiedenen VPN-Verbindungen finden Sie unter [VPN Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md).

### <a name="virtual-wan"></a><a name="virtualwan"></a>Virtual WAN
Der Netzwerkdienst Azure Virtual WAN bietet optimierte und automatisierte Branch-Konnektivität mit und durch Azure. Azure-Regionen dienen als Hubs, die Sie auswählen können, um Branches mit ihnen zu verbinden. Sie können das Azure-Backbone nutzen, um auch Verbindungen mit Branches herzustellen und die Branch-to-VNET-Konnektivität zu nutzen. Azure Virtual WAN vereint zahlreiche Azure-Cloudkonnektivitätsdienste wie Site-to-Site-VPN, ExpressRoute und Point-to-Site-Benutzer-VPN auf einer einzigen Betriebsoberfläche. Die Konnektivität mit Azure-VNets wird mithilfe virtueller Netzwerkverbindungen hergestellt. Weitere Informationen finden Sie unter [Was ist Azure Virtual WAN?](../virtual-wan/virtual-wan-about.md).

![Virtual WAN-Diagramm](./media/networking-overview/virtualwan1.png)

### <a name="azure-dns"></a><a name="dns"></a>Azure DNS
Azure DNS ist ein Hostingdienst für DNS-Domänen, der eine Namensauflösung mittels Microsoft Azure-Infrastruktur bietet. Indem Sie Ihre Domänen in Azure hosten, können Sie Ihre DNS-Einträge unter Verwendung der gleichen Anmeldeinformationen, APIs, Tools und Abrechnungsabläufe wie bei Ihren anderen Azure-Diensten verwalten. Weitere Informationen finden Sie unter [Was ist Azure DNS?](../dns/dns-overview.md)

### <a name="azure-bastion"></a><a name="bastion"></a>Azure Bastion
Azure Bastion ist ein neuer vollständig verwalteter PaaS-Dienst, den Sie in Ihrem virtuellen Netzwerk bereitstellen können. Dieser Dienst bietet sichere und nahtlose RDP- und SSH-Verbindungen mit Ihren virtuellen Computern über TLS direkt im Azure-Portal. Beim Herstellen einer Verbindung über Azure Bastion benötigen Ihre virtuellen Computer keine öffentliche IP-Adresse. Weitere Informationen finden Sie unter [Was ist Azure Bastion?](../bastion/bastion-overview.md).

![Azure Bastion-Architektur](./media/networking-overview/architecture.png)

### <a name="virtual-network-nat-gateway"></a><a name="nat"></a>Virtual Network NAT Gateway
Virtual Network NAT (Network Address Translation, Netzwerkadressenübersetzung) vereinfacht für virtuelle Netzwerke die Einrichtung von ausschließlich ausgehenden Internetverbindungen. Bei der Konfiguration in einem Subnetz werden für die gesamte Konnektivität in ausgehender Richtung die von Ihnen angegebenen statischen öffentlichen IP-Adressen verwendet. Die ausgehende Konnektivität ist möglich, ohne dass ein Lastenausgleich oder öffentliche IP-Adressen direkt virtuellen Computern zugeordnet werden. Weitere Informationen finden Sie unter [Was ist das Virtual Network NAT Gateway?](../virtual-network/nat-overview.md). 

![Virtual Network NAT Gateway](./media/networking-overview/flow-map.png)

### <a name="azure-peering-service"></a><a name="azurepeeringservice"></a> Azure Peering Service
Azure Peering Service verbessert die Kundenkonnektivität mit Microsoft Cloud Services wie Office 365, Dynamics 365, SaaS-Diensten (Software-as-a-Service), Azure oder sämtlichen anderen Microsoft-Diensten, auf die über das öffentliche Internet zugegriffen werden kann. Weitere Informationen finden Sie unter [Was ist Azure Peering Service?](../peering-service/about.md).

## <a name="application-protection-services"></a><a name="protect"></a>Anwendungsschutzdienste

In diesem Abschnitt werden die Azure-Netzwerkdienste beschrieben, mit denen Sie Ihre Netzwerkressourcen schützen können. Diese Dienste schützen Ihrer Anwendungen mithilfe einer Kombination folgender Netzwerkdienste in Azure: Private Link, DDoS Protection, Firewall, Netzwerksicherheitsgruppen, Web Application Firewall und Virtual Network-Endpunkte.

|Dienst|Einsatzzweck|Szenario|
|---|---|---|
|[DDoS Protection](#ddosprotection) |Hochverfügbarkeit für Ihre Anwendungen mit Schutz vor übermäßigen Gebühren für IP-Datenverkehr|[Verwalten von Azure DDoS Protection](../virtual-network/manage-ddos-protection.md)|
|[Web Application Firewall](#waf)|<p>[Azure WAF mit Application Gateway](../web-application-firewall/ag/ag-overview.md) bietet regionalen Schutz für Entitäten im öffentlichen und privaten Adressraum.</p><p>[Azure WAF mit Front Door](../web-application-firewall/afds/afds-overview.md) bietet Schutz am Netzwerkedge für öffentliche Endpunkte.</p>|<p>[Konfigurieren von Bot-Schutzregeln](../frontdoor/waf-front-door-policy-configure-bot-protection.md)</p> <p>[Konfigurieren von benutzerdefiniertem Antwortcode](../frontdoor/waf-front-door-configure-custom-response-code.md)</p> <p>[Konfigurieren von IP-Einschränkungsregeln](../frontdoor/waf-front-door-configure-ip-restriction.md)</p> <p>[Konfigurieren von Ratenbegrenzungsregeln](../frontdoor/waf-front-door-rate-limit-powershell.md)</p> |
|[Azure Firewall](#firewall)|Azure Firewall ist ein verwalteter, cloudbasierter Netzwerksicherheitsdienst, der Ihre Azure Virtual Network-Ressourcen schützt. Es ist eine vollständig zustandsbehaftete Firewall-as-a-Service mit integrierter Hochverfügbarkeit und uneingeschränkter Cloudskalierbarkeit.|<p>[Bereitstellen einer Azure Firewall-Instanz in einem VNET](../firewall/tutorial-firewall-deploy-portal.md)</p> <p>[Bereitstellen einer Azure Firewall-Instanz in einem Hybridnetzwerk](../firewall/tutorial-hybrid-ps.md)</p> <p>[Filtern von eingehendem Datenverkehr mit Azure Firewall-DNAT](../firewall/tutorial-firewall-dnat.md)</p>|
|[Netzwerksicherheitsgruppen](#nsg)|Vollständige, präzise Kontrolle von verteilten Endknoten in VMs und Subnetzen für alle Netzwerkdatenverkehrflows|[Filtern des Netzwerkdatenverkehrs mithilfe von Netzwerksicherheitsgruppen](../virtual-network/tutorial-filter-network-traffic.md)|
|[VNET-Dienstendpunkte](#serviceendpoints)|Diese ermöglichen es Ihnen, den Netzwerkzugriff auf einige Azure-Dienstressourcen auf ein Subnetz eines virtuellen Netzwerks einzuschränken.|[Beschränken des Netzwerkzugriffs auf PaaS-Ressourcen](../virtual-network/tutorial-restrict-network-access-to-resources-powershell.md)|
[Private Link](#privatelink)|Greifen Sie über einen privaten Endpunkt in Ihrem virtuellen Netzwerk auf Azure-PaaS-Dienste (beispielsweise Azure Storage und SQL Database) sowie auf in Azure gehostete kundeneigene Dienste/Partnerdienste zu.|<p>[Erstellen eines privaten Endpunkts](../private-link/create-private-endpoint-portal.md)</p><p>[Erstellen eines Private Link-Diensts](../private-link/create-private-link-service-portal.md)</p>|
|||
### <a name="ddos-protection"></a><a name="ddosprotection"></a>DDoS Protection 
[Azure DDoS Protection](../virtual-network/manage-ddos-protection.md) bietet Gegenmaßnahmen gegen die komplexesten DDoS-Bedrohungen. Der Dienst bietet erweiterte DDoS-Risikominderungsfunktionen für die Anwendungen und Ressourcen, die in Ihren virtuellen Netzwerken bereitgestellt werden. Außerdem haben Kunden, die Azure DDoS Protection verwenden, Zugriff auf DDoS Rapid Response-Support, durch den sich DDoS-Experten bei einem aktiven Angriff einschalten können.

![DDoS Protection](./media/networking-overview/ddos-protection.png)

### <a name="web-application-firewall"></a><a name="waf"></a>Web Application Firewall

[Azure Web Application Firewall](../web-application-firewall/overview.md) (WAF) schützt Ihre Webanwendungen vor gängigen Webexploits und Sicherheitsrisiken wie die Einschleusung von SQL-Befehlen und Cross-Site Scripting. WAF bietet über verwaltete Regeln integrierten Schutz vor den zehn häufigsten Websicherheitslücken (laut OWASP). Darüber hinaus können Kunden benutzerdefinierte Regeln konfigurieren, bei denen es sich um vom Kunden verwaltete Regeln handelt, um zusätzlichen Schutz anhand des Quell-IP-Adressbereichs und der Anforderungsattribute wie Header, Cookies, Formulardatenfelder oder Abfragezeichenfolgenparameter zu bieten.

Kunden können [Azure WAF mit Application Gateway](../application-gateway/waf-overview.md) einsetzen, um Entitäten im öffentlichen und privaten Adressraum regional zu schützen. Kunden können auch [Azure WAF mit Front Door](../frontdoor/waf-overview.md) einsetzen, um öffentliche Endpunkte am Netzwerkedge zu schützen.

![Web Application Firewall](./media/networking-overview/waf-overview.png)


### <a name="azure-firewall"></a><a name="firewall"></a>Azure Firewall
Azure Firewall ist ein verwalteter, cloudbasierter Netzwerksicherheitsdienst, der Ihre Azure Virtual Network-Ressourcen schützt. Mit Azure Firewall können Sie Richtlinien zur Anwendungs- und Netzwerkkonnektivität übergreifend für Abonnements und virtuelle Netzwerke zentral erstellen, erzwingen und protokollieren. Azure Firewall verwendet eine statische öffentliche IP-Adresse für Ihre virtuellen Netzwerkressourcen, die es außenstehenden Firewalls ermöglicht, Datenverkehr aus Ihrem virtuellen Netzwerk zu identifizieren. 

Weitere Informationen zu Azure Firewall finden Sie in der [zugehörigen Dokumentation](../firewall/overview.md).

![Firewallübersicht](./media/networking-overview/firewall-threat.png)

### <a name="network-security-groups"></a><a name="nsg"></a>Netzwerksicherheitsgruppen
Sie können Netzwerkdatenverkehr von und zu Azure-Ressourcen in einem virtuellen Azure-Netzwerk mithilfe einer Netzwerksicherheitsgruppe filtern. Weitere Informationen finden Sie unter [Sicherheitsübersicht](../virtual-network/security-overview.md).

### <a name="service-endpoints"></a><a name="serviceendpoints"></a>Dienstendpunkte
Mit Dienstendpunkten von virtuellen Netzwerken (VNETs) werden der Bereich privater Adressen Ihres virtuellen Netzwerks und die Identität Ihres VNET über eine direkte Verbindung auf die Azure-Dienste erweitert. Endpunkte ermöglichen es Ihnen, Ihre kritischen Ressourcen von Azure-Diensten auf Ihre virtuellen Netzwerke zu beschränken und so zu schützen. Der Datenverkehr aus Ihrem VNET an den Azure-Dienst verbleibt immer im Backbone-Netzwerk von Microsoft Azure. Weitere Informationen finden Sie unter [VNET-Dienstendpunkte](../virtual-network/virtual-network-service-endpoints-overview.md).

![VNET-Dienstendpunkte](./media/networking-overview/vnet-service-endpoints-overview.png)

### <a name="azure-private-link"></a><a name="privatelink"></a>Azure Private Link
Mit [Azure Private Link](../private-link/private-link-overview.md) können Sie über einen privaten Endpunkt in Ihrem virtuellen Netzwerk auf Azure-PaaS-Dienste (beispielsweise Azure Storage und SQL Database) sowie auf in Azure gehostete kundeneigene Dienste/Partnerdienste zugreifen.
Der Datenverkehr zwischen Ihrem virtuellen Netzwerk und dem Dienst verläuft über das Microsoft-Backbone-Netzwerk. Es ist nicht mehr erforderlich, dass Sie Ihren Dienst über das öffentliche Internet verfügbar machen. Sie können Ihren eigenen Private Link-Dienst in Ihrem virtuellen Netzwerk erstellen und Ihren Kunden zur Verfügung stellen.

![Privater Endpunkt – Übersicht](./media/networking-overview/private-endpoint.png)


## <a name="application-delivery-services"></a><a name="deliver"></a>Anwendungsbereitstellungsdienste

In diesem Abschnitt werden die Netzwerkdienste in Azure beschrieben, die bei der Bereitstellung von Anwendungen helfen – Content Delivery Network, Azure Front Door Service, Traffic Manager, Load Balancer und Application Gateway.

|Dienst|Einsatzzweck|Szenario|
|---|---|---|
|[Content Delivery Network](#cdn)|Dieser Dienst übermittelt Inhalte mit hoher Bandbreite an Benutzer. In CDN-Instanzen werden zwischengespeicherte Inhalte auf Edgeservern an POP-Standorten (Point of Presence) gespeichert, die sich in der Nähe der Endbenutzer befinden, um die Wartezeit zu verringern.|<p>[Hinzufügen von CDN zu einer Web-App](../cdn/cdn-add-to-web-app.md)</p> <p>[Zugriff auf Speicherblobs unter Verwendung einer benutzerdefinierten Azure CDN-Domäne über HTTPS](..//cdn/cdn-storage-custom-domain-https.md)</p> <p>[Hinzufügen einer benutzerdefinierten Domäne zum Azure CDN-Endpunkt](../cdn/cdn-map-content-to-custom-domain.md)</p> <p>[Konfigurieren von HTTPS in einer benutzerdefinierten Azure CDN-Domäne](../cdn/cdn-custom-ssl.md?tabs=option-1-default-enable-https-with-a-cdn-managed-certificate)</p>|
|[Azure Front Door Service](#frontdoor)|Ermöglicht die Definition, Verwaltung und Überwachung des globalen Routings für Ihren Webdatenverkehr, indem Optimierungen für die bestmögliche Leistung und sofortige globale Failover durchgeführt werden, um die Hochverfügbarkeit sicherzustellen|<p>[Hinzufügen einer benutzerdefinierten Domäne zu Azure Front Door Service](../frontdoor/front-door-custom-domain.md)</p> <p>[Konfigurieren von HTTPS in einer benutzerdefinierten Front Door-Domäne](../frontdoor/front-door-custom-domain-https.md)</p><p>[Einrichten einer Web Application Firewall-Richtlinie für Geofilterung](../frontdoor/front-door-tutorial-geo-filtering.md)|
|[Traffic Manager](#trafficmanager)|Verteilt den Datenverkehr anhand des DNS auf Dienste in Azure-Regionen auf der ganzen Welt und sorgt gleichzeitig für Hochverfügbarkeit und Reaktionsfähigkeit|<p> [Weiterleiten von Datenverkehr für geringe Latenz](../traffic-manager/tutorial-traffic-manager-improve-website-response.md)</p><p>[Weiterleiten von Datenverkehr an einen Prioritätsendpunkt](../traffic-manager/traffic-manager-configure-priority-routing-method.md)</p><p> [Steuern des Datenverkehrs mit gewichteten Endpunkten](../traffic-manager/tutorial-traffic-manager-weighted-endpoint-routing.md)</p><p>[Weiterleiten von Datenverkehr basierend auf dem geografischen Standort des Endpunkts](../traffic-manager/traffic-manager-configure-geographic-routing-method.md)</p> <p> [Weiterleiten von Datenverkehr basierend auf dem Subnetz des Benutzers](../traffic-manager/tutorial-traffic-manager-subnet-routing.md)</p>|
|[Load Balancer](#loadbalancer)|Bietet regionalen Lastenausgleich durch Weiterleiten des Datenverkehrs über Verfügbarkeitszonen und in Ihre VNETs und internen Lastenausgleich durch Weiterleiten des Datenverkehrs über Ihre und zwischen Ihren Ressourcen, um eine regionale Anwendung zu erstellen|<p> [Vornehmen eines Lastausgleichs für Internetdatenverkehr virtueller Computer](../load-balancer/tutorial-load-balancer-standard-manage-portal.md)</p> <p>[Lastenausgleich für den Datenverkehrs für mehrere VMs in einem virtuellen Netzwerk](../load-balancer/tutorial-load-balancer-standard-internal-portal.md)<p>[Portweiterleitung des Datenverkehrs an einen bestimmten Port auf bestimmten VMs](../load-balancer/tutorial-load-balancer-port-forwarding-portal.md)</p><p> [Konfigurieren von Lastenausgleichs- und Ausgangsregeln](../load-balancer/configure-load-balancer-outbound-cli.md)</p>|
|[Application Gateway](#applicationgateway)|Azure Application Gateway ist ein Lastenausgleich für Webdatenverkehr, mit dem Sie eingehenden Datenverkehr für Ihre Webanwendungen verwalten können.|<p>[Weiterleiten von Webdatenverkehr über Azure Application Gateway](../application-gateway/quick-create-portal.md)</p><p>[Tutorial: Konfigurieren eines Anwendungsgateways mit TLS-Terminierung mithilfe des Azure-Portals](../application-gateway/create-ssl-portal.md)</p><p>[Erstellen eines Anwendungsgateways mit Umleitung auf URL-Pfadbasis](../application-gateway/create-url-route-portal.md) </p>|
|

### <a name="content-delivery-network"></a><a name="cdn"></a>Content Delivery Network
Azure Content Delivery Network (CDN) bietet Entwicklern eine globale Lösung zur schnellen Übermittlung von Inhalten mit hoher Bandbreite an Benutzer. Hierzu werden Inhalte auf physischen Knoten zwischengespeichert, die strategisch auf der ganzen Welt verteilt sind. Weitere Informationen zu Azure CDN finden Sie unter [Azure Content Delivery Network](../cdn/cdn-overview.md).

![Azure CDN](./media/networking-overview/cdn-overview.png)

### <a name="azure-front-door-service"></a><a name="frontdoor"></a>Azure Front Door Service
Über Azure Front Door Service können Sie das globale Routing für Ihren Webdatenverkehr definieren, verwalten und überwachen, indem Sie es für beste Leistung und sofortiges globales Failover bei Hochverfügbarkeit optimieren. Mit Front Door können Sie Ihre globalen (mehrere Regionen) Consumer- und Unternehmensanwendungen in stabile und hochleistungsfähige personalisierte moderne Anwendungen, APIs und Inhalte transformieren, die eine globale Zielgruppe mit Azure erreichen. Weitere Informationen finden Sie unter [Azure Front Door](../frontdoor/front-door-overview.md).


### <a name="traffic-manager"></a><a name="trafficmanager"></a>Traffic Manager

Azure Traffic Manager ist ein DNS-basierter Lastenausgleich für Datenverkehr, der es Ihnen ermöglicht, den Datenverkehr optimal auf Dienste in den globalen Azure-Regionen zu verteilen, und gleichzeitig Hochverfügbarkeit und Reaktionsfähigkeit sicherzustellen. Traffic Manager bietet verschiedene Methoden für das Datenverkehrsrouting, um den Datenverkehr zu verteilen, z. B. nach Priorität, Gewichtung, Leistung, geografischen Daten, mehr Mehrwert oder Subnetz. Weitere Informationen zu diesen Methoden finden Sie unter [Traffic Manager-Methoden für das Datenverkehrsrouting](../traffic-manager/traffic-manager-routing-methods.md).

Das folgende Diagramm veranschaulicht das auf Endpunktpriorität basierende Routing mit Traffic Manager:

![Prioritätsbasierte Methode für das Datenverkehrsrouting in Azure Traffic Manager](./media/networking-overview/priority.png)

Weitere Informationen zu Azure-Traffic Manager finden Sie unter [Was ist Azure Traffic Manager?](../traffic-manager/traffic-manager-overview.md).

### <a name="load-balancer"></a><a name="loadbalancer"></a>Load Balancer
Der Azure Load Balancer ist äußerst leistungsfähig und bietet Lastenausgleich der Ebene 4 mit niedriger Latenz für alle UDP- und TCP-Protokolle. Der Dienst verwaltet eingehende und ausgehende Verbindungen. Sie können öffentliche und interne Endpunkte mit Lastenausgleich konfigurieren. Sie können Regeln definieren, um eingehende Verbindungen anhand von TCP- und HTTP-Integritätstestoptionen zu Back-End-Pool-Zielen zuzuordnen, um die Dienstverfügbarkeit zu verwalten. Weitere Informationen zum Load Balancer finden Sie im Artikel mit der [Übersicht über den Load Balancer](../load-balancer/load-balancer-overview.md).

Die folgende Abbildung zeigt eine Multi-Tier-Anwendung mit Internetzugriff, die sowohl externen als auch internen Lastenausgleich nutzt:

![Beispiel für Azure Load Balancer](./media/networking-overview/load-balancer.png)


### <a name="application-gateway"></a><a name="applicationgateway"></a>Application Gateway
Azure Application Gateway ist ein Lastenausgleich für Webdatenverkehr, mit dem Sie eingehenden Datenverkehr für Ihre Webanwendungen verwalten können. Es handelt sich um ein ADC-as-a-Service-Angebot (Application Delivery Controller), das verschiedene Schicht-7-Lastenausgleichsfunktionen für Ihre Anwendung bietet. Weitere Informationen finden Sie unter [Was ist Azure Application Gateway?](../application-gateway/overview.md).

Im folgenden Diagramm wird das auf URL-Pfaden basierende Routing mit Application Gateway veranschaulicht.

![Application Gateway-Beispiel](./media/networking-overview/figure1-720.png)

## <a name="network-monitoring-services"></a><a name="monitor"></a>Netzwerküberwachungsdienste
In diesem Abschnitt werden die Azure-Netzwerkdienste beschrieben, mit denen Sie Ihre Netzwerkressourcen überwachen können: Network Watcher, ExpressRoute-Monitor, Azure Monitor oder Terminalzugangspunkte für virtuelle Netzwerke.

|Dienst|Einsatzzweck|Szenario|
|---|---|---|
|[Network Watcher](#networkwatcher)|Unterstützt die Überwachung und Behandlung von Konnektivitätsproblemen, die Diagnose von Problemen mit VPNs, NSGs und Routing, das Erfassen von Paketen auf VMs und das Automatisieren von Diagnosetools mit Azure Functions und Logic Apps|<p>[Diagnostizieren eines Problems mit dem Filter für VM-Datenverkehr](../network-watcher/diagnose-vm-network-traffic-filtering-problem.md)</p><p>[Diagnostizieren von VM-Routingproblemen](../network-watcher/diagnose-vm-network-routing-problem.md)</p><p>[Überwachen der Kommunikation zwischen virtuellen Computern](../network-watcher/connection-monitor.md)</p><p>[Diagnostizieren von Problemen mit der Kommunikation zwischen Netzwerken](../network-watcher/diagnose-communication-problem-between-networks.md)</p><p>[Protokollieren des ein- und ausgehenden Netzwerkdatenverkehrs eines virtuellen Computers](../network-watcher/network-watcher-nsg-flow-logging-portal.md)</p>|
|[ExpressRoute-Monitor](#expressroutemonitor)|Bietet eine Echtzeitüberwachung der Netzwerkleistung, der Verfügbarkeit und der Auslastung, hilft bei der automatischen Ermittlung der Netzwerktopologie, bietet schnellere Fehlerisolation, erkennt vorübergehende Netzwerkprobleme, hilft bei der Analyse des Verlauf der Netzwerkleistung, unterstützt mehrere Abonnements|<p>[Konfigurieren des Netzwerkleistungsmonitors für ExpressRoute](../expressroute/how-to-npm.md)</p><p>[ExpressRoute-Überwachung, -Metriken und -Warnungen](../expressroute/expressroute-monitoring-metrics-alerts.md)</p>|
|[Azure Monitor](#azuremonitor)|Diese Lösung hilft Ihnen, die Leistung Ihrer Anwendungen zu verstehen, und erkennt proaktiv Probleme, die sich auf diese und die Ressourcen auswirken, von denen sie abhängen.|<p>[Traffic Manager-Metriken und -Warnungen](../traffic-manager/traffic-manager-metrics-alerts.md)</p><p>[Azure Monitor-Diagnose für Load Balancer Standard](../load-balancer/load-balancer-standard-diagnostics.md)</p><p>[Überwachen von Azure Firewall-Protokollen und -Metriken](../firewall/tutorial-diagnostics.md)</p><p>[Überwachung und Protokollierung von Azure Web Application Firewall](../frontdoor/waf-front-door-monitor.md)</p>|
|[Terminalzugangspunkt für virtuelle Netzwerke](#vnettap)|Ermöglicht kontinuierliches Streaming des Netzwerkdatenverkehrs virtueller Computer an den Paketcollector, bietet Verwaltungslösungen für die Netzwerk- und Anwendungsleistung sowie Tools für Sicherheitsanalysen|[Erstellen einer Ressource für Terminalzugangspunkte für virtuelle Netzwerke](../virtual-network/tutorial-tap-virtual-network-cli.md)|
|

### <a name="network-watcher"></a><a name="networkwatcher"></a>Network Watcher
Mit Azure Network Watcher werden Tools für die Überwachung, Diagnose, Metrikanzeige und Aktivierung/Deaktivierung von Protokollen für Ressourcen in einem virtuellen Azure-Netzwerk bereitgestellt. Weitere Informationen finden Sie unter [Was ist Network Watcher?](../network-watcher/network-watcher-monitoring-overview.md?toc=%2fazure%2fnetworking%2ftoc.json).
### <a name="expressroute-monitor"></a><a name="expressroutemonitor"></a>ExpressRoute-Monitor
Weitere Informationen zum Anzeigen von ExpressRoute-Verbindungsmetriken, -Ressourcenprotokollen und -Warnungen finden Sie unter [ExpressRoute-Überwachung, -Metriken und -Warnungen](../expressroute/expressroute-monitoring-metrics-alerts.md?toc=%2fazure%2fnetworking%2ftoc.json).
### <a name="azure-monitor"></a><a name="azuremonitor"></a>Azure Monitor
Azure Monitor maximiert die Verfügbarkeit und Leistung Ihrer Anwendungen durch die Bereitstellung einer umfassenden Lösung für das Sammeln, Analysieren und Reagieren auf Telemetriedaten aus Ihren cloudbasierten und lokalen Umgebungen. Diese Lösung hilft Ihnen, die Leistung Ihrer Anwendungen zu verstehen, und erkennt proaktiv Probleme, die sich auf sie auswirken, und Ressourcen, von denen sie abhängen. Weitere Informationen finden Sie unter [Überblick über Azure Monitor](../azure-monitor/overview.md?toc=%2fazure%2fnetworking%2ftoc.json).
### <a name="virtual-network-tap"></a><a name="vnettap"></a>Terminalzugangspunkt für virtuelle Netzwerke
Mit dem TAP (Terminal Access Point) für virtuelle Azure-Netzwerke können Sie Ihren VM-Netzwerkdatenverkehr kontinuierlich an einen Netzwerkpaketcollector oder ein Analysetool streamen. Der Collector oder das Analysetool wird von einem Partner für [virtuelle Netzwerkappliances](https://azure.microsoft.com/solutions/network-appliances/) bereitgestellt. 

In der folgenden Abbildung wird gezeigt, wie der TAP des virtuellen Netzwerks funktioniert. 

![Funktionsweise eines TAP eines virtuellen Netzwerks](./media/networking-overview/virtual-network-tap-architecture.png)

Weitere Informationen finden Sie unter [Was sind Terminalzugangspunkte für virtuelle Netzwerke?](../virtual-network/virtual-network-tap-overview.md).

## <a name="next-steps"></a>Nächste Schritte

- Erstellen Sie Ihr erstes VNet, und verbinden Sie einige VMs damit, indem Sie die Schritte des Artikels [Erstellen Ihres ersten virtuellen Netzwerks](../virtual-network/quick-create-portal.md?toc=%2fazure%2fnetworking%2ftoc.json) ausführen.
- Verbinden Sie Ihren Computer über eine Point-to-Site-Verbindung mit einem VNET, indem Sie die Schritte im Artikel [Konfigurieren einer Point-to-Site-Verbindung](../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md?toc=%2fazure%2fnetworking%2ftoc.json) befolgen.
- Führen Sie einen Lastenausgleich des Internetdatenverkehrs zu öffentlichen Servern durch, indem Sie die Schritte im Artikel [Erstellen eines Load Balancers mit Internetzugriff](../load-balancer/load-balancer-get-started-internet-portal.md?toc=%2fazure%2fnetworking%2ftoc.json) ausführen.
 
 
   
