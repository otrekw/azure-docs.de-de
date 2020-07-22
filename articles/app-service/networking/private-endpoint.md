---
title: Herstellen einer privaten Verbindung mit einer Azure-Web-App mithilfe eines privaten Endpunkts
description: Herstellen einer privaten Verbindung mit einer Web-App mithilfe eines privaten Azure-Endpunkts
author: ericgre
ms.assetid: 2dceac28-1ba6-4904-a15d-9e91d5ee162c
ms.topic: article
ms.date: 07/07/2020
ms.author: ericg
ms.service: app-service
ms.workload: web
ms.custom: fasttrack-edit, references_regions
ms.openlocfilehash: fdad2f7c2ce4f82529866b4235ebebab8da664d3
ms.sourcegitcommit: bcb962e74ee5302d0b9242b1ee006f769a94cfb8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/07/2020
ms.locfileid: "86054575"
---
# <a name="using-private-endpoints-for-azure-web-app-preview"></a>Verwenden privater Endpunkte für eine Azure-Web-App (Vorschau)

> [!Note]
> Mit der Vorschauaktualisierung haben wir die Funktion für den Schutz vor Datenexfiltration freigegeben.
>
> Die Vorschauversion ist in allen öffentlichen Regionen für PremiumV2 Windows- und Linux-Web-Apps sowie elastische Premiumfunktionen verfügbar. 

Sie können private Endpunkte für Ihre Azure-Web-App verwenden, um Clients in Ihrem privaten Netzwerk den sicheren Zugriff auf die App über Private Link zu ermöglichen. Der private Endpunkt verwendet eine IP-Adresse aus dem Adressraum Ihres virtuellen Azure-Netzwerks. Der Netzwerkdatenverkehr zwischen einem Client in Ihrem privaten Netzwerk und der Web-App wird über das virtuelle Netzwerk und Private Link im Microsoft-Backbonenetzwerk geleitet, sodass keine Verfügbarmachung im öffentlichen Internet erfolgt.

Die Verwendung privater Endpunkte für Ihre Web-App bietet Ihnen folgende Möglichkeiten:

- Schützen Ihrer Web-App durch Konfigurieren des privaten Endpunkts, wodurch eine öffentliche Verfügbarmachung beseitigt wird.
- Sicheres Verbinden mit der Web-App aus lokalen Netzwerken, die eine Verbindung mit dem virtuellen Netzwerk über VPN oder ExpressRoute mit privatem Peering herstellen.
- Vermeiden Sie die Exfiltration von Daten aus Ihrem VNet. 

Wenn Sie nur eine sichere Verbindung zwischen Ihrem virtuellen Netzwerk und Ihrer Web-App benötigen, ist ein Dienstendpunkt die einfachste Lösung. Wenn Sie auch aus einem lokalen Netz über ein Azure-Gateway auf die Web-App zugreifen können müssen, ist ein privater Endpunkt mit einem VNet mit regionalem Peering oder mit globalem Peering die Lösung.  

Weitere Informationen finden Sie unter [Dienstendpunkte][serviceendpoint].

## <a name="conceptual-overview"></a>Konzeptionelle Übersicht

Ein privater Endpunkt ist eine spezielle Netzwerkschnittstelle (NIC, Netzwerkadapter) für Ihre Azure-Web-App in einem Subnetz in Ihrem virtuellen Netzwerk (VNet).
Wenn Sie einen privaten Endpunkt für Ihre Web-App erstellen, stellt dieser eine sichere Verbindung zwischen Clients in Ihrem privaten Netzwerk und Ihrer Web-App bereit. Dem privaten Endpunkt wird eine IP-Adresse aus dem IP-Adressbereich Ihres VNet zugewiesen.
Für die Verbindung zwischen dem privaten Endpunkt und der Web-App wird ein sicherer [Private Link][privatelink] verwendet. Der private Endpunkt wird nur für eingehende Flows in Ihre Web-App verwendet. Ausgehende Flows verwenden diesen privaten Endpunkt nicht, aber Sie können ausgehende Flows über die [VNet-Integrationsfunktion][vnetintegrationfeature] in Ihr Netzwerk in einem anderen Subnetz einfügen.

Das Subnetz, in dem Sie den privaten Endpunkt einbinden, kann über andere Ressourcen verfügen. Sie benötigen also kein dediziertes leeres Subnetz.
Sie können den privaten Endpunkt auch in einer anderen Region als der der Web-App bereitstellen. 

> [!Note]
>Die VNet-Integrationsfunktion kann nicht dasselbe Subnetz wie der private Endpunkt verwenden. Dies ist eine Einschränkung der VNet-Integrationsfunktion.

Unter dem Aspekt der Sicherheit:

- Wenn Sie private Endpunkte für Ihre Web-App aktivieren, deaktivieren Sie jeglichen öffentlichen Zugriff.
- Sie können mehrere private Endpunkte in anderen VNets und Subnetzen aktivieren, einschließlich VNets in anderen Regionen.
- Die IP-Adresse der NIC des privaten Endpunkts muss dynamisch sein, bleibt jedoch unverändert, bis Sie den privaten Endpunkt löschen.
- Der NIC des privaten Endpunkts kann keine NSG zugeordnet sein.
- Dem Subnetz, das den privaten Endpunkt hostet, kann eine Netzwerksicherheitsgruppe zugeordnet sein, aber Sie müssen die Erzwingung von Netzwerkrichtlinien für den privaten Endpunkt deaktivieren. Weitere Informationen hierzu finden Sie unter [Deaktivieren von Netzwerkrichtlinien für private Endpunkte][disablesecuritype]. Als Resultat hieraus können Sie den Zugriff auf den privaten Endpunkt nach keiner Netzwerksicherheitsgruppe filtern.
- Wenn Sie einen privaten Endpunkt für Ihre Web-App aktivieren, wird die Konfiguration der [Zugriffsbeschränkungen][accessrestrictions] der Web-App nicht ausgewertet.
- Sie können das Risiko der Datenexfiltration aus dem VNet beseitigen, indem Sie alle NSG-Regeln entfernen, in denen das Zieltag das Internet oder Azure-Dienste sind. Wenn Sie einen privaten Endpunkt für eine Web-App bereitstellen, können Sie diese spezifische Web-App nur über den privaten Endpunkt erreichen. Wenn Sie über eine andere Web-App verfügen, müssen Sie einen weiteren dedizierten privaten Endpunkt für diese andere Web-App bereitstellen.

In den Web-HTTP-Protokollen Ihrer Web-App finden Sie die Clientquell-IP. Dieses Feature wird mithilfe des TCP-Proxyprotokolls implementiert, das die Client-IP-Eigenschaft an die Web-App weiterleitet. Weitere Informationen finden Sie unter [Abrufen von Verbindungsinformationen mithilfe von TCP-Proxy v2][tcpproxy].


  > [!div class="mx-imgBorder"]
  > ![Privater Web-App-Endpunkt – globale Übersicht](media/private-endpoint/global-schema-web-app.png)

## <a name="dns"></a>DNS

Wenn Sie einen privaten Endpunkt für die Web-App verwenden, muss die angeforderte URL dem Namen Ihrer Web-App entsprechen. Der Standardname lautet mywebappname.azurewebsites.net.

Standardmäßig ist der öffentliche Name Ihrer Web-App ohne einen privaten Endpunkt ein kanonischer Name für den Cluster.
Der Name wird beispielsweise wie folgt aufgelöst:

|Name |type |Wert |
|-----|-----|------|
|mywebapp.azurewebsites.net|CNAME|clustername.azurewebsites.windows.net|
|clustername.azurewebsites.windows.net|CNAME|cloudservicename.cloudapp.net|
|cloudservicename.cloudapp.net|Ein|40.122.110.154| 


Wenn Sie einen privaten Endpunkt bereitstellen, wird der DNS-Eintrag so aktualisiert, dass er auf den kanonischen Namen mywebapp.privatelink.azurewebsites.net verweist.
Der Name wird beispielsweise wie folgt aufgelöst:

|Name |type |Wert |Anmerkung |
|-----|-----|------|-------|
|mywebapp.azurewebsites.net|CNAME|mywebapp.privatelink.azurewebsites.net|
|mywebapp.privatelink.azurewebsites.net|CNAME|clustername.azurewebsites.windows.net|
|clustername.azurewebsites.windows.net|CNAME|cloudservicename.cloudapp.net|
|cloudservicename.cloudapp.net|Ein|40.122.110.154|<– Diese öffentliche IP-Adresse ist nicht der private Endpunkt, führt zu einem Fehler 403|

Sie müssen einen privaten DNS-Server oder eine private Azure DNS-Zone einrichten. Für Tests können Sie den Hosteintrag des Testcomputers ändern.
Die DNS-Zone, die Sie erstellen müssen, ist: **privatelink.azurewebsites.net**. Registrieren Sie den Eintrag für Ihre Web-App mit einem A-Eintrag und der IP des privaten Endpunkts.
Der Name wird beispielsweise wie folgt aufgelöst:

|Name |type |Wert |Anmerkung |
|-----|-----|------|-------|
|mywebapp.azurewebsites.net|CNAME|mywebapp.privatelink.azurewebsites.net|
|mywebapp.privatelink.azurewebsites.net|Ein|10.10.10.8|<– Sie verwalten diesen Eintrag in Ihrem DNS-System, sodass er auf die IP-Adresse Ihres privaten Endpunkts verweist|

Nachdem Sie diese DNS-Konfiguration vorgenommen haben, können Sie Ihre Web-App privat unter dem Standardnamen mywebappname.azurewebsites.net erreichen.


Wenn Sie einen benutzerdefinierten DNS-Namen verwenden müssen, müssen Sie den benutzerdefinierten Namen in Ihrer Web-App hinzufügen. Während der Vorschauphase muss der benutzerdefinierte Name wie jeder andere benutzerdefinierte Name mithilfe der öffentlichen DNS-Auflösung überprüft werden. Weitere Informationen finden Sie unter [Benutzerdefinierte DNS-Validierung][dnsvalidation].

Für die Kudu-Konsole oder die Kudu-REST-API (z. B. zusammen mit selbstgehosteten DevOps-Agents bereitgestellt) müssen Sie zwei Einträge in Ihrer privaten Azure DNS-Zone oder auf Ihrem benutzerdefinierten DNS-Server erstellen. 

| Name | type | Wert |
|-----|-----|-----|
| mywebapp.privatelink.azurewebsites.net | Ein | PrivateEndpointIP | 
| mywebapp.scm.privatelink.azurewebsites.net | Ein | PrivateEndpointIP | 



## <a name="pricing"></a>Preise

Ausführliche Preisinformationen finden Sie unter [Azure Private Link – Preise][pricing].

## <a name="limitations"></a>Einschränkungen

Wenn Sie die Azure Functions in einem elastischen Premium-Plan mit einem privaten Endpunkt verwenden, um die Funktion im Azure-Webportal auszuführen, benötigen Sie direkten Netzwerkzugriff, oder Sie erhalten einen HTTP 403-Fehler. Anders ausgedrückt, muss Ihr Browser den privaten Endpunkt erreichen können, um die Funktion aus dem Azure-Webportal ausführen zu können. 

Während der Vorschauphase wird nur der Produktionsslot hinter dem privaten Endpunkt verfügbar gemacht, auf andere Slots muss über einen öffentlichen Endpunkt zugegriffen werden.

Wir verbessern regelmäßig die Funktionen private Verbindung (Private Link) und privater Endpunkt. Aktuelle Informationen zu Einschränkungen finden Sie in [diesem Artikel][pllimitations].

## <a name="next-steps"></a>Nächste Schritte

- Informationen zum Bereitstellen des privaten Endpunkts für Ihre Web-App über das Portal finden Sie unter [Herstellen einer privaten Verbindung mit einer Web-App mithilfe eines privaten Azure-Endpunkts (Vorschau)][howtoguide1].
- Informationen zum Bereitstellen des privaten Endpunkts für Ihre Web-App über die Azure CLI finden Sie unter [Erstellen einer App Service-App und Bereitstellen eines privaten Endpunkts mithilfe der Azure-Befehlszeilenschnittstelle][howtoguide2].
- Informationen zum Bereitstellen des privaten Endpunkts für Ihre Web-App mithilfe von PowerShell finden Sie unter [Erstellen einer App Service-App und Bereitstellen eines privaten Endpunkts mithilfe von PowerShell][howtoguide3].



<!--Links-->
[serviceendpoint]: https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoints-overview
[privatelink]: https://docs.microsoft.com/azure/private-link/private-link-overview
[vnetintegrationfeature]: https://docs.microsoft.com/azure/app-service/web-sites-integrate-with-vnet
[disablesecuritype]: https://docs.microsoft.com/azure/private-link/disable-private-endpoint-network-policy
[accessrestrictions]: https://docs.microsoft.com/azure/app-service/app-service-ip-restrictions
[tcpproxy]: ../../private-link/private-link-service-overview.md#getting-connection-information-using-tcp-proxy-v2
[dnsvalidation]: https://docs.microsoft.com/azure/app-service/app-service-web-tutorial-custom-domain
[pllimitations]: https://docs.microsoft.com/azure/private-link/private-endpoint-overview#limitations
[pricing]: https://azure.microsoft.com/pricing/details/private-link/
[howtoguide1]: https://docs.microsoft.com/azure/private-link/create-private-endpoint-webapp-portal
[howtoguide2]: https://docs.microsoft.com/azure/app-service/scripts/cli-deploy-privateendpoint
[howtoguide3]: https://docs.microsoft.com/azure/app-service/scripts/powershell-deploy-private-endpoint
