---
title: include file
description: include file
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 09/17/2020
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 32e4658af48a0ae3bde08de18cf1d8204878d671
ms.sourcegitcommit: bdd5c76457b0f0504f4f679a316b959dcfabf1ef
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2020
ms.locfileid: "91025059"
---
### <a name="is-bgp-supported-on-all-azure-vpn-gateway-skus"></a>Wird BGP von allen Azure-VPN-Gateway-SKUs unterstützt?
BGP wird mit Ausnahme der SKU „Basic“ für alle Azure-VPN-Gateway-SKUs unterstützt.

### <a name="can-i-use-bgp-with-azure-policy-based-vpn-gateways"></a>Kann ich BGP mit richtlinienbasierten Azure-VPN-Gateways verwenden?
Nein. BGP wird nur für routenbasierte VPN-Gateways unterstützt.

### <a name="what-asn-autonomous-system-numbers-can-i-use"></a>Welche autonomen Systemnummern (ASNs) kann ich verwenden?
Sie können eigene öffentliche ASNs oder private ASNs für Ihre lokalen Netzwerke sowie für Ihre virtuellen Azure-Netzwerke verwenden – mit **Ausnahme** der von Azure oder IANA reservierten Bereiche:

> [!IMPORTANT]
>
> Die folgenden ASNs sind für Azure oder IANA reserviert:
> * Von Azure reservierte ASNs:
>    * Öffentliche ASNs: 8074, 8075, 12076
>    * Private ASNs: 65515, 65517, 65518, 65519, 65520
> * [Von IANA reservierte](http://www.iana.org/assignments/iana-as-numbers-special-registry/iana-as-numbers-special-registry.xhtml) ASNs
>    * 23456, 64496-64511, 65535-65551 und 429496729
>
> Diese ASNs können beim Herstellen einer Verbindung mit Azure-VPN-Gateways nicht für Ihre lokalen VPN-Geräte angegeben werden.
>

### <a name="can-i-use-32-bit-4-byte-asns-autonomous-system-numbers"></a>Kann ich ASNs (Autonome Systemnummern) mit 32 Bit (4 Bytes) verwenden?
Ja, Azure-VPN-Gateways unterstützen nun 32-Bit-ASNs (4 Bytes). Verwenden Sie PowerShell, die Befehlszeilenschnittstelle oder das SDK, um die Verwendung einer ASN im Dezimalformat zu konfigurieren.

### <a name="what-private-asns-can-i-use"></a>Welche privaten ASNs kann ich verwenden?
Die verwendbaren Bereiche privater ASNs, die verwendet werden können, sind:

* 64512–65514, 65521–65534

Diese ASNs sind nicht für die Verwendung durch IANA oder Azure reserviert und können daher Ihrem Azure-VPN Gateway zugewiesen werden.

### <a name="what-address-does-azure-vpn-gateway-use-for-bgp-peer-ip"></a>Welche Adresse verwendet das Azure-VPN-Gateway als BGP-Peer-IP-Adresse?

* Das Azure-VPN-Gateway ordnet standardmäßig eine einzelne IP-Adresse aus dem GatewaySubnet-Bereich für Aktiv/Standby-VPN-Gateways oder zwei IP-Adressen für Aktiv/Aktiv-VPN-Gateways zu. Diese Adressen werden automatisch zugeordnet, wenn Sie das VPN-Gateway erstellen. Die tatsächlich zugeordneten BGP-IP-Adressen können mithilfe des PowerShell-Cmdlets „Get-AzVirtualNetworkGateway“ (Eigenschaft „bgpPeeringAddress“) oder über das Azure-Portal (unter der Eigenschaft „BGP-ASN konfigurieren“ auf der Gatewaykonfigurationsseite) ermittelt werden.

* Wenn Ihre lokalen VPN-Router IP-Adressen vom Typ **APIPA** (169.254.x.x) als BGP-IP-Adressen verwenden, müssen Sie für Ihr Azure-VPN-Gateway eine zusätzliche **Azure-APIPA-BGP-IP-Adresse** angeben. Vom Azure-VPN-Gateway wird die APIPA-Adresse für die Verwendung mit dem lokalen, im lokalen Netzwerkgateway angegebenen APIPA-BGP-Peer oder die private IP-Adresse für einen APIPA-fremden lokalen BGP-Peer ausgewählt. Weitere Informationen finden Sie unter [Konfigurieren von BGP für Azure-VPN-Gateways](../articles/vpn-gateway/bgp-howto.md).

### <a name="what-are-the-requirements-for-the-bgp-peer-ip-addresses-on-my-vpn-device"></a>Welche Anforderungen müssen die BGP-Peer-IP-Adressen auf meinem VPN-Gerät erfüllen?
Ihre lokale BGP-Peeradresse darf **NICHT** der öffentlichen IP-Adresse Ihres VPN-Geräts entsprechen oder aus dem VNET-Adressraum des VPN-Gateways stammen. Verwenden Sie als BGP-Peer-IP-Adresse eine andere IP-Adresse für das VPN-Gerät. Dabei kann es sich um eine Adresse handeln, die der Loopbackschnittstelle auf dem Gerät zugewiesen ist (reguläre IP-Adresse oder APIPA-Adresse). Wenn von Ihrem Gerät eine APIPA-Adresse für BGP verwendet wird, müssen Sie wie unter [Konfigurieren von BGP für Azure-VPN-Gateways](../articles/vpn-gateway/bgp-howto.md) beschrieben eine APIPA-BGP-IP-Adresse für Ihr Azure-VPN-Gateway angeben. Geben Sie diese Adresse im entsprechenden lokalen Netzwerkgateway an, das den Standort darstellt.

### <a name="what-should-i-specify-as-my-address-prefixes-for-the-local-network-gateway-when-i-use-bgp"></a>Was muss ich bei Verwendung von BGP als Adresspräfixe für das lokale Netzwerkgateway angeben?

> [!IMPORTANT]
>
>Hierbei handelt es sich um eine Änderung gegenüber der zuvor dokumentierten Anforderung. Wenn Sie BGP für eine Verbindung verwenden, sollten Sie das Feld **Adressraum** für die zugehörige lokale Netzwerkgatewayressource ***leer*** lassen. Vom Azure-VPN-Gateway wird intern eine Hostroute zur lokalen BGP-Peer-IP-Adresse über den IPsec-Tunnel hinzugefügt. Fügen Sie die /32-Route **NICHT** dem Adressraumfeld hinzu. Sie ist redundant und kann diesem Feld nicht hinzugefügt werden, wenn als BGP-IP-Adresse des lokalen VPN-Geräts eine APIPA-Adresse verwendet wird. Wenn Sie im Adressraumfeld andere Präfixe hinzufügen, werden diese zusätzlich zu den über BGP ermittelten Routen als **statische Routen** für das Azure-VPN-Gateway hinzugefügt.
>

### <a name="can-i-use-the-same-asn-for-both-on-premises-vpn-networks-and-azure-vnets"></a>Kann ich die gleiche ASN sowohl für lokale VPN-Netzwerke als auch für Azure-VNETs verwenden?
Nein. Lokalen Netzwerken und Azure-VNETs muss jeweils eine unterschiedliche ASN zugewiesen werden, wenn diese per BGP miteinander verbunden werden. Azure-VPN-Gateways ist standardmäßig die ASN 65515 zugewiesen. Dabei spielt es keine Rolle, ob BGP für Ihre standortübergreifende Konnektivität aktiviert ist. Diesen Standardwert können Sie überschreiben, indem Sie beim Erstellen des VPN-Gateways eine andere ASN zuweisen oder die ASN nach der Gatewayerstellung ändern. Ihre lokalen ASNs müssen den entsprechenden lokalen Azure-Netzwerkgateways zugewiesen werden.

### <a name="what-address-prefixes-will-azure-vpn-gateways-advertise-to-me"></a>Welche Adresspräfixe kündigen Azure-VPN-Gateways mir gegenüber an?
Das Azure-VPN-Gateway kündigt Ihren lokalen BGP-Geräten gegenüber folgende Routen an:

* Ihre VNET-Adresspräfixe
* Adresspräfixe für die einzelnen lokalen Netzwerkgateways, die mit dem Azure-VPN-Gateway verbunden sind
* Routen, die in anderen, mit dem Azure VPN Gateway verbundenen BGP-Peeringsitzungen ermittelt wurden ( **mit Ausnahme der Standardrouten oder Routen, die sich mit VNET-Präfixen überschneiden**)

### <a name="how-many-prefixes-can-i-advertise-to-azure-vpn-gateway"></a>Wie viele Präfixe kann ich Azure-VPN-Gateways ankündigen?
Wir unterstützen bis zu 4.000 Präfixe. Die BGP-Sitzung wird verworfen, wenn die Anzahl von Präfixen den Grenzwert überschreitet.

### <a name="can-i-advertise-default-route-00000-to-azure-vpn-gateways"></a>Kann ich die Standardroute (0.0.0.0/0) für Azure VPN Gateways ankündigen?
Ja.

Beachten Sie, dass dadurch der gesamte ausgehende VNET-Datenverkehr an Ihren lokalen Standort gesendet wird. Außerdem können VNET-VMs dadurch keine öffentliche Kommunikation aus dem Internet direkt akzeptieren. Dies betrifft beispielsweise RDP- oder SSH-Verbindungen mit den virtuellen Computern über das Internet.

### <a name="can-i-advertise-the-exact-prefixes-as-my-virtual-network-prefixes"></a>Kann ich die gleichen Präfixe wie meine Virtual Network-Präfixe ankündigen?

Nein. Das Ankündigen der gleichen Präfixe wie Ihre Virtual Network-Adresspräfixe wird von der Azure-Plattform blockiert oder gefiltert. Sie können jedoch ein Präfix ankündigen, das eine Obermenge der Inhalte Ihres virtuellen Netzwerks ist. 

Sie können beispielsweise 10.0.0.0/8 ankündigen, wenn für Ihr virtuelles Netzwerk der Adressraum 10.0.0.0/16 verwendet wird. Das Ankündigen von 10.0.0.0/16 oder 10.0.0.0/24 ist dagegen nicht möglich.

### <a name="can-i-use-bgp-with-my-vnet-to-vnet-connections"></a>Kann ich BGP mit meinen VNET-zu-VNET-Verbindungen verwenden?
Ja. BGP kann sowohl für standortübergreifende Verbindungen als auch für VNET-zu-VNET-Verbindungen verwendet werden.

### <a name="can-i-mix-bgp-with-non-bgp-connections-for-my-azure-vpn-gateways"></a>Kann ich BGP-Verbindungen mit BGP-fremden Verbindungen für meine Azure-VPN-Gateways kombinieren?
Ja. Für ein Azure-VPN-Gateway kann eine Kombination aus BGP-Verbindungen und BGP-fremden Verbindungen verwendet werden.

### <a name="does-azure-vpn-gateway-support-bgp-transit-routing"></a>Wird BGP-Transitrouting vom Azure-VPN-Gateway unterstützt?
Ja. BGP-Transitrouting wird unterstützt. Einzige Einschränkung: Azure-VPN-Gateways kündigen gegenüber anderen BGP-Peers **KEINE** Standardrouten an. Wenn Sie Transitrouting über mehrere Azure-VPN-Gateways hinweg nutzen möchten, müssen Sie BGP für alle VNET-zu-VNET-Zwischenverbindungen aktivieren. Weitere Informationen finden Sie in der [Übersicht über BGP](../articles/vpn-gateway/vpn-gateway-bgp-overview.md).

### <a name="can-i-have-more-than-one-tunnel-between-azure-vpn-gateway-and-my-on-premises-network"></a>Kann ich zwischen Azure-VPN-Gateway und meinem lokalen Netzwerk mehrere Tunnel verwenden?
Ja. Zwischen einem Azure-VPN-Gateway und Ihrem lokalen Netzwerk können mehrere S2S-VPN-Tunnel eingerichtet werden. Beachten Sie, dass diese Tunnel alle auf die Gesamtanzahl von Tunneln für Ihre Azure-VPN-Gateways angerechnet werden und dass Sie BGP für beide Tunnel aktivieren müssen.

Wenn also etwa zwischen Ihrem Azure-VPN-Gateway und einem Ihrer lokalen Netzwerke zwei redundante Tunnel bestehen, werden dadurch zwei Tunnel des Gesamtkontingents für Ihr Azure-VPN-Gateway belegt.

### <a name="can-i-have-multiple-tunnels-between-two-azure-vnets-with-bgp"></a>Kann ich zwischen zwei Azure-VNETs mit BGP mehrere Tunnel verwenden?
Ja. Aber mindestens eines der virtuellen Netzwerkgateways muss über eine Aktiv/Aktiv-Konfiguration verfügen.

### <a name="can-i-use-bgp-for-s2s-vpn-in-an-expressroutes2s-vpn-coexistence-configuration"></a>Kann ich BGP für S2S-VPN-Verbindungen in einer Konfiguration mit ExpressRoute und S2S-VPN verwenden?
Ja. 

### <a name="what-should-i-add-to-my-on-premises-vpn-device-for-the-bgp-peering-session"></a>Was muss ich meinem lokalen VPN-Gerät für die BGP-Peeringsitzung hinzufügen?
Sie müssen eine Hostroute der Azure-BGP-Peer-IP-Adresse auf Ihrem VPN-Gerät hinzufügen, die auf den IPsec-S2S-VPN-Tunnel verweist. Lautet die Azure-VPN-Peer-IP-Adresse also etwa 10.12.255.30, müssen Sie eine Hostroute für 10.12.255.30 mit einer Nexthop-Schnittstelle der entsprechenden IPSec-Tunnelschnittstelle auf Ihrem VPN-Gerät hinzufügen.
