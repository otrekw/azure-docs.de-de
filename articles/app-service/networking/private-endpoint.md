---
title: Herstellen einer privaten Verbindung mit einer Web-App mithilfe eines privaten Azure-Endpunkts
description: Herstellen einer privaten Verbindung mit einer Web-App mithilfe eines privaten Azure-Endpunkts
author: ericgre
ms.assetid: 2dceac28-1ba6-4904-a15d-9e91d5ee162c
ms.topic: article
ms.date: 03/12/2020
ms.author: ericg
ms.service: app-service
ms.workload: web
ms.openlocfilehash: 893a7a2c7483fccc3bbc7bd198929f65917457b3
ms.sourcegitcommit: b8d0d72dfe8e26eecc42e0f2dbff9a7dd69d3116
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2020
ms.locfileid: "79036948"
---
# <a name="using-private-endpoints-for-azure-web-app-preview"></a>Verwenden privater Endpunkte für eine Azure-Web-App (Vorschau)

Sie können private Endpunkte für Ihre Azure-Web-App verwenden, um Clients in Ihrem privaten Netzwerk den sicheren Zugriff auf die App über Private Link zu ermöglichen. Der private Endpunkt verwendet eine IP-Adresse aus dem Adressraum Ihres virtuellen Azure-Netzwerks. Der Netzwerkdatenverkehr zwischen dem Client in Ihrem privaten Netzwerk und der Web-App wird über das virtuelle Netzwerk und Private Link im Microsoft-Backbonenetzwerk geleitet, sodass keine Verfügbarmachung im öffentlichen Internet erfolgt.

Die Verwendung privater Endpunkte für Ihre Web-App bietet Ihnen folgende Möglichkeiten:

- Schützen Ihrer Web-App durch Konfigurieren des Dienstendpunkts, wodurch eine öffentliche Verfügbarmachung beseitigt wird.
- Sicheres Verbinden mit der Web-App aus lokalen Netzwerken, die eine Verbindung mit dem virtuellen Netzwerk über VPN oder ExpressRoute mit privatem Peering herstellen.

Wenn Sie nur eine sichere Verbindung zwischen Ihrem virtuellen Netzwerk und Ihrer Web-App benötigen, ist ein Dienstendpunkt die einfachste Lösung. Wenn Sie auch von einem lokalen Standort aus über ein Azure-Gateway auf die Web-App zugreifen können müssen, ist ein privater Endpunkt mit einem VNET mit regionalem Peering oder mit globalem Peering die Lösung.  

Weitere Informationen zum [Dienstendpunkt][serviceendpoint]

## <a name="conceptual-overview"></a>Konzeptionelle Übersicht

Ein privater Endpunkt ist eine spezielle Netzwerkschnittstelle (NIC, Netzwerkadapter) für Ihre Azure-Web-App in Ihrem Subnetz in Ihrem virtuellen Netzwerk (VNET).
Wenn Sie einen privaten Endpunkt für Ihre Web-App erstellen, stellt dieser eine sichere Verbindung zwischen Clients in Ihrem privaten Netzwerk und Ihrer Web-App bereit. Dem privaten Endpunkt wird eine IP-Adresse aus dem IP-Adressbereich Ihres VNET zugewiesen.
Für die Verbindung zwischen dem privaten Endpunkt und der Web-App wird ein sicherer [Private Link][privatelink] verwendet. Der private Endpunkt wird nur für eingehende Flows in Ihre Web-App verwendet. Ausgehende Flows verwenden diesen privaten Endpunkt nicht, aber Sie können ausgehende Flows über die [VNET-Integrationsfunktion][vnetintegrationfeature] in Ihr Netzwerk in einem anderen Subnetz einfügen.

Das Subnetz, in dem Sie den privaten Endpunkt einbinden, kann über andere Ressourcen verfügen. Sie benötigen also kein dediziertes leeres Subnetz.
Sie können den privaten Endpunkt in einer anderen Region als die Web-App bereitstellen. 

> [!Note]
>Die VNET-Integrationsfunktion kann nicht dasselbe Subnetz wie der private Endpunkt verwenden. Dies ist eine Einschränkung der VNET-Integrationsfunktion.

Unter dem Aspekt der Sicherheit:

- Wenn Sie den Dienstendpunkt für Ihre Web-App aktivieren, deaktivieren Sie jeglichen öffentlichen Zugriff.
- Sie können mehrere private Endpunkte in anderen VNETs und Subnetzen aktivieren, einschließlich VNETs in anderen Regionen.
- Die IP-Adresse der NIC des privaten Endpunkts muss dynamisch sein, bleibt jedoch unverändert, bis Sie den privaten Endpunkt löschen.
- Der NIC des privaten Endpunkts kann keine NSG zugeordnet sein.
- Dem Subnetz, das den privaten Endpunkt hostet, kann eine Netzwerksicherheitsgruppe zugeordnet sein, aber Sie müssen die Erzwingung von Netzwerkrichtlinien für den privaten Endpunkt deaktivieren. Weitere Informationen hierzu finden Sie in [diesem Artikel][disablesecuritype]. Als Resultat hieraus können Sie den Zugriff auf den privaten Endpunkt nach keiner Netzwerksicherheitsgruppe filtern.
- Wenn Sie einen privaten Endpunkt für Ihre Web-App aktivieren, wird die Konfiguration der [Zugriffsbeschränkungen][accessrestrictions] der Web-App nicht ausgewertet.
- Sie können das Risiko der Datenexfiltration aus dem VNET verringern, indem Sie alle NSG-Regeln entfernen, in denen das Zieltag das Internet oder Azure-Dienste sind. Durch das Hinzufügen eines Web-App-Dienstendpunkts in Ihrem Subnetz können Sie jede Web-App erreichen, die im selben Stempel gehostet und im Internet verfügbar gemacht wird.

Ein privater Endpunkt ist im Tarif „PremiumV2“ sowie „Isoliert“ mit externer ASE verfügbar.

In den Web-HTTP-Protokollen Ihrer Web-App finden Sie die Clientquell-IP. Wir haben das TCP-Proxyprotokoll implementiert, das die Client-IP-Eigenschaft an die Web-App weiterleitet. [hier finden Sie weitere Informationen][tcpproxy]

![Globale Übersicht][1]


## <a name="dns"></a>DNS

Da sich diese Funktion in der Vorschauphase befindet, ändern wir den DNS-Eintrag während dieser Zeit nicht. Sie müssen den DNS-Eintrag selbst auf Ihrem privaten DNS-Server oder in Ihrer privaten Azure DNS-Zone verwalten. Wenn Sie einen benutzerdefinierten DNS-Namen verwenden müssen, müssen Sie den benutzerdefinierten Namen in Ihrer Web-App hinzufügen. Während der Vorschauphase muss der benutzerdefinierte Name wie jeder andere benutzerdefinierte Name mithilfe der öffentlichen DNS-Auflösung überprüft werden. [Technische Referenz zu benutzerdefiniertem DNS][dnsvalidation]

## <a name="pricing"></a>Preise

Ausführliche Preisinformationen finden Sie unter [Azure Private Link – Preise][pricing].

## <a name="limitations"></a>Einschränkungen

Wir verbessern regelmäßig die Funktionen private Verbindung (Private Link) und privater Endpunkt. Aktuelle Informationen zu Einschränkungen finden Sie in [diesem Artikel][pllimitations].

## <a name="next-steps"></a>Nächste Schritte

Informationen zum Bereitstellen des privaten Endpunkts für Ihre Web-App über das Portal finden Sie unter [Herstellen einer privaten Verbindung mit einer Web-App][howtoguide].


<!--Image references-->
[1]: ./media/private-endpoint/schemaglobaloverview.png

<!--Links-->
[serviceendpoint]: https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoints-overview
[privatelink]: https://docs.microsoft.com/azure/private-link/private-link-overview
[vnetintegrationfeature]: https://docs.microsoft.com/azure/app-service/web-sites-integrate-with-vnet
[disablesecuritype]: https://docs.microsoft.com/azure/private-link/disable-private-endpoint-network-policy
[accessrestrictions]: https://docs.microsoft.com/azure/app-service/app-service-ip-restrictions
[tcpproxy]: https://docs.microsoft.com/azure/private-link/rivate-link-service-overview#getting-connection-information-using-tcp-proxy-v2
[dnsvalidation]: https://docs.microsoft.com/azure/app-service/app-service-web-tutorial-custom-domain
[pllimitations]: https://docs.microsoft.com/azure/private-link/private-endpoint-overview#limitations
[pricing]: https://azure.microsoft.com/pricing/details/private-link/
[howtoguide]: https://docs.microsoft.com/azure/private-link/create-private-endpoint-webapp-portal
