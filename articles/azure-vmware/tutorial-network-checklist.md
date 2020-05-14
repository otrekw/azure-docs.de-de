---
title: 'Tutorial: Netzwerkprüfliste'
description: Netzwerkanforderungen und Details zu Netzwerkkonnektivität und -ports
ms.topic: tutorial
ms.date: 05/04/2020
ms.openlocfilehash: 42bd579a455c7efe3b8f6e4d4154e687726adb1d
ms.sourcegitcommit: d9cd51c3a7ac46f256db575c1dfe1303b6460d04
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/04/2020
ms.locfileid: "82739525"
---
# <a name="networking-checklist-for-azure-vmware-solution-avs"></a>Netzwerkprüfliste für die Azure-VMware-Lösung (Azure VMware Solution, AVS)

Die Azure-VMware-Lösung bietet eine private VMware-Cloudumgebung, auf die Benutzer und Anwendungen von lokalen und Azure-basierten Umgebungen oder Ressourcen aus zugreifen können. Die Konnektivität wird durch Netzwerkdienste wie Azure ExpressRoute und VPN-Verbindungen bereitgestellt und setzt bestimmte Netzwerkadressbereiche und Firewallports voraus, um die Dienste verwenden zu können. In diesem Artikel erfahren Sie, wie Sie Ihr Netzwerk ordnungsgemäß für die Verwendung von AVS konfigurieren.

In diesem Tutorial wird Folgendes vermittelt:

> [!div class="checklist"]
> * Konnektivitätsanforderungen für das Netzwerk
> * DHCP in AVS

## <a name="network-connectivity"></a>Netzwerkkonnektivität

Die private AVS-Cloud wird über eine Azure ExpressRoute-Verbindung mit Ihrem virtuellen Azure-Netzwerk verbunden. Über diese Verbindung mit hoher Bandbreite und geringer Wartezeit können Sie von Ihrer privaten Cloudumgebung aus auf Dienste zugreifen, die in Ihrem Azure-Abonnement ausgeführt werden.

Private AVS-Clouds benötigen mindestens einen CIDR-Netzwerkadressblock vom Typ `/22` für Subnetze, wie unten dargestellt. Dieses Netzwerk ergänzt Ihre lokalen Netzwerke. Für die Verbindungsherstellung mit lokalen Umgebungen und virtuellen Netzwerken darf der Netzwerkadressblock keine Überlappung aufweisen.

Beispiel für einen CIDR-Netzwerkadressblock vom Typ `/22`: `10.10.0.0/22`

Subnetze:

| Verwendung des Netzwerks             | Subnetz | Beispiel        |
| ------------------------- | ------ | -------------- |
| Verwaltung von privaten Clouds            | `/24`    | `10.10.0.0/24`   |
| vMotion-Netzwerk       | `/24`    | `10.10.1.0/24`   |
| VM-Workloads | `/24`   | `10.10.2.0/24`   |
| ExpressRoute-Peering | `/24`    | `10.10.3.8/30`   |

### <a name="network-ports-required-to-communicate-with-the-service"></a>Erforderliche Netzwerkports für die Kommunikation mit dem Dienst

Quelle|Ziel|Protokoll |Port |Beschreibung  |Kommentar
---|-----|:-----:|:-----:|-----|-----
DNS-Server für die private Cloud  |Lokaler DNS-Server  |UDP |53|DNS-Client: Weiterleitung von Anforderungen der PC-vCenter-Instanz für beliebige lokale DNS-Abfragen (siehe DNS-Abschnitt unten) |
Lokaler DNS-Server  |DNS-Server für die private Cloud  |UDP |53|DNS-Client: Weiterleitung von Anforderungen lokaler Dienste an DNS-Server für die private Cloud (siehe DNS-Abschnitt unten)
Lokales Netzwerk  |vCenter Server für die private Cloud  |TCP (HTTP)  |80|Von vCenter Server wird Port 80 für direkte HTTP-Verbindungen benötigt. Vom Port 80 werden Anforderungen an den HTTPS-Port 443 umgeleitet. Diese Umleitung ist hilfreich, wenn Sie `http://server` anstelle von `https://server` verwenden.  <br><br>WS-Management (erfordert auch, dass der Port 443 geöffnet ist) <br><br>Wenn Sie nicht die gebündelte SQL Server 2008-Datenbank der vCenter Server-Instanz, sondern eine benutzerdefinierte Microsoft SQL-Datenbank verwenden, wird der Port 80 von den SQL Server Reporting Services verwendet. Wenn Sie vCenter Server installieren, werden Sie vom Installationsprogramm aufgefordert, den HTTP-Port für die vCenter Server-Instanz zu ändern. Ändern Sie den HTTP-Port für vCenter Server in einen benutzerdefinierten Wert, um eine erfolgreiche Installation zu gewährleisten. Von Microsoft Internet Information Services (IIS) wird ebenfalls Port 80 verwendet. Weitere Informationen finden Sie unter „Konflikt zwischen vCenter Server und IIS für Port 80“.
Verwaltungsnetzwerk für die private Cloud |Lokales Active Directory  |TCP  |389|Dieser Port muss in der lokalen Instanz sowie in allen Remoteinstanzen von vCenter Server geöffnet sein. Bei diesem Port handelt es sich um die LDAP-Portnummer für die Verzeichnisdienste für die vCenter Server-Gruppe. Das vCenter Server-System muss an den Port 389 gebunden werden, auch wenn Sie diese vCenter Server-Instanz nicht in eine Linked Mode-Gruppe einbinden. Wird an diesem Port ein anderer Dienst ausgeführt, empfiehlt es sich gegebenenfalls, ihn zu entfernen oder seinen Port zu ändern. Der LDAP-Dienst kann an einem beliebigen Port zwischen 1025 und 65535 ausgeführt werden.  Wenn diese Instanz als Microsoft Windows Active Directory fungiert, ändern Sie die Portnummer von 389 in einen verfügbaren Port zwischen 1025 und 65535. Dieser Port ist optional und dient dazu, eine lokale AD-Instanz als Identitätsquelle für die vCenter-Instanz der privaten Cloud zu konfigurieren.
Lokales Netzwerk  |vCenter Server für die private Cloud  |TCP (HTTPS)  |443|Dieser Port ermöglicht den Zugriff auf vCenter über das lokale Netzwerk. Der Standardport, der vom vCenter Server-System verwendet wird, um auf Verbindungen des vSphere-Clients zu lauschen. Öffnen Sie den Port 443 in der Firewall, damit das vCenter Server-System Daten vom vSphere-Client empfangen kann. Das vCenter Server-System verwendet den Port 443 auch zur Überwachung der Datenübertragung von SDK-Clients. Dieser Port wird auch für folgende Dienste verwendet: WS-Management (erfordert auch, dass der Port 80 geöffnet ist). vSphere-Clientzugriff auf vSphere Update Manager. Clientverbindungen einer Drittanbieter-Netzwerkverwaltung mit vCenter Server. Clientzugriff einer Drittanbieter-Netzwerkverwaltung auf Hosts. 
Webbrowser  | Hybrid Cloud Manager  | TCP (HTTPS) | 9443 | Hybrid Cloud Manager-Schnittstelle für die Verwaltung virtueller Geräte für die Hybrid Cloud Manager-Systemkonfiguration.
Administratornetzwerk  | Hybrid Cloud Manager |ssh |22| Administrator-SSH-Zugriff auf Hybrid Cloud Manager.
HCM |   Cloudgateway|TCP (HTTPS) |8123| Senden hostbasierter Replikationsdienstanweisungen an das Hybrid Cloud-Gateway.
HCM |   Cloudgateway  |    HTTP TCP (HTTPS) |    9443  |  Senden von Verwaltungsanweisungen an das lokale Hybrid Cloud-Gateway über die REST-API.
Cloudgateway|      L2C |    TCP (HTTP)  |   443 |   Senden von Verwaltungsanweisungen vom Cloudgateway an L2C, wenn L2C den gleichen Pfad verwendet wie das Hybrid Cloud-Gateway.
Cloudgateway |     ESXi-Hosts |     TCP |    80, 902  |   Verwaltung und OVF-Bereitstellung
Cloudgateway (lokal)|     Cloudgateway (remote)|     UDP  |   4500 | Erforderlich für IPsec<br>   Internetschlüsselaustausch (IKEv2) zur Kapselung von Workloads für den bidirektionalen Tunnel. NAT-Traversal (NAT-T) wird ebenfalls unterstützt.
Cloudgateway (lokal)  |   Cloudgateway (remote)  |   UDP  |   500   | Erforderlich für IPsec<br> Internetschlüsselaustausch (ISAKMP) für den bidirektionalen Tunnel.
Lokales vCenter-Netzwerk|      Verwaltungsnetzwerk für die private Cloud|      TCP  |    8000    |  vMotion für virtuelle Computer aus der lokalen vCenter-Instanz zur vCenter-Instanz in der privaten Cloud   |     

## <a name="dhcp"></a>DHCP

Anwendungen und Workloads, die in einer privaten Cloudumgebung ausgeführt werden, erfordern Namensauflösung und DHCP-Dienste zum Nachschlagen und für die IP-Adresszuweisung. Zur Bereitstellung dieser Dienste ist eine ordnungsgemäße DHCP- und DNS-Infrastruktur erforderlich. Sie können einen virtuellen Computer konfigurieren, um diese Dienste in Ihrer privaten Cloudumgebung bereitzustellen.  
Es empfiehlt sich, den in NSX integrierten DHCP-Dienst oder einen lokalen DHCP-Server in der privaten Cloud zu verwenden, anstatt DHCP-Broadcastdatenverkehr über das WAN in die lokale Umgebung zurückzuleiten.

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Konnektivitätsanforderungen für das Netzwerk
> * DHCP in AVS

## <a name="next-steps"></a>Nächste Schritte

Nach der ordnungsgemäßen Einrichtung des Netzwerks können Sie im nächsten Tutorial Ihre private AVS-Cloud erstellen.

> [!div class="nextstepaction"]
> [Tutorial: Bereitstellen einer privaten AVS-Cloud in Azure](tutorial-create-private-cloud.md)
