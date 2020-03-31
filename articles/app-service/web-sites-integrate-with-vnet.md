---
title: Integration einer App in Azure Virtual Network
description: Integrieren von Apps in Azure App Service mit Azure Virtual Networks.
author: ccompy
ms.assetid: 90bc6ec6-133d-4d87-a867-fcf77da75f5a
ms.topic: article
ms.date: 02/27/2020
ms.author: ccompy
ms.custom: seodec18
ms.openlocfilehash: 89aa78e0d26598eacf436ca88cc6c5549f91d2fc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "78673224"
---
# <a name="integrate-your-app-with-an-azure-virtual-network"></a>Integrieren Ihrer App in ein Azure Virtual Network
In diesem Dokument wird die Azure App Service-Funktion für die Integration in ein virtuelles Netzwerk beschrieben, und Sie erfahren, wie Sie die Funktion mit Apps in [Azure App Service](https://go.microsoft.com/fwlink/?LinkId=529714) einrichten. Mit [Azure Virtual Networks][VNETOverview] (VNETs) können Sie viele Ihrer Azure-Ressourcen in einem Netzwerk platzieren, das nicht über das Internet geroutet werden kann.  

Es gibt zwei Varianten der Nutzung von Azure App Service.

[!INCLUDE [app-service-web-vnet-types](../../includes/app-service-web-vnet-types.md)]

## <a name="enable-vnet-integration"></a>Aktivieren der VNET-Integration 

1. Navigieren Sie zur Netzwerkbenutzeroberfläche im App Service-Portal. Wählen Sie unter „VNET-Integration“ die Option *Zum Konfigurieren hier klicken* aus. 

1. Wählen Sie **VNET hinzufügen** aus.  

   ![Auswählen der VNET-Integration][1]

1. Die Dropdownliste enthält alle Resource Manager -VNETs in Ihrem Abonnement in derselben Region und darunter eine Liste aller Resource Manager-VNETs in allen anderen Regionen. Wählen Sie das VNET aus, das Sie integrieren möchten.

   ![Auswählen des VNETs][2]

   * Wenn sich das VNET in derselben Region befindet, erstellen Sie entweder ein neues Subnetz, oder wählen Sie ein leeres bereits vorhandenes Subnetz aus. 

   * Wenn Sie ein VNET in einer anderen Region auswählen möchten, müssen Sie über ein Virtual Network Gateway verfügen, für das Point-to-Site aktiviert ist.

   * Wenn die Integration in ein klassisches VNET erfolgen soll, wählen Sie anstelle der VNET-Dropdownliste die Option **Klicken Sie hier, um eine Verbindung mit einem klassischen VNET herzustellen** aus. Wählen Sie das gewünschte klassische VNET aus. Das Ziel-VNET muss bereits über ein bereitgestelltes Virtual Network Gateway verfügen, das für Point-to-Site aktiviert ist.

    ![Auswählen eines klassischen VNETs][3]
    
Während der Integration wird Ihre App neu gestartet.  Wenn die Integration abgeschlossen ist, werden Details zum VNET angezeigt, in das Sie integriert sind. 

Nachdem Ihre App in Ihr VNET integriert wurde, verwendet sie den DNS-Server, mit dem Ihr VNET konfiguriert ist, wenn es sich nicht um Azure DNS Private Zones handelt. Die VNET-Integration kann derzeit nicht mit Azure DNS Private Zones verwendet werden.

## <a name="regional-vnet-integration"></a>Regionale VNET-Integration

[!INCLUDE [app-service-web-vnet-types](../../includes/app-service-web-vnet-regional.md)]

### <a name="how-regional-vnet-integration-works"></a>Funktionsweise der regionalen VNET-Integration

Apps in App Service werden auf Workerrollen gehostet. Der Tarif „Basic“ und höhere Tarife sind dedizierte Hostingpläne, bei denen keine anderen Kundenworkloads auf den gleichen Workern ausgeführt werden. Die regionale VNET-Integration beruht auf der Einbindung virtueller Schnittstellen mit Adressen im delegierten Subnetz. Da sich die Von-Adresse in Ihrem VNET befindet, kann sie ähnlich wie eine VM im VNET auf die meisten Ressourcen zugreifen, die im oder über das VNET verfügbar sind. Die Netzwerkimplementierung unterscheidet sich von der Ausführung einer VM in Ihrem VNET. Einige Netzwerkfeatures sind daher bei der Verwendung dieser Funktion noch nicht verfügbar.

![Funktionsweise der regionalen VNET-Integration][5]

Wenn regionale VNET-Integration aktiviert ist, sendet Ihre App ausgehende Aufrufe an das Internet weiterhin über die üblichen Kanäle. Ihre App verwendet nach wie vor die ausgehenden Adressen, die im Portal für die App-Eigenschaften aufgeführt sind. Was sich für Ihre App ändert, sind Aufrufe an per Dienstendpunkt geschützte Dienste oder RFC 1918-Adressen. Diese Aufrufe werden in Ihr VNET geleitet. Wenn WEBSITE_VNET_ROUTE_ALL auf 1 festgelegt ist, kann der gesamte ausgehende Datenverkehr in Ihr VNET gesendet werden. 

Die Funktion unterstützt nur eine virtuelle Schnittstelle pro Worker,  Eine virtuelle Schnittstelle pro Worker bedeutet eine regionale VNET-Integration pro App Service Plan. Alle Apps im gleichen App Service-Plan können die gleiche VNET-Integration verwenden. Wenn eine App jedoch eine Verbindung mit einem zusätzlichen VNET herstellen muss, müssen Sie jedoch einen weiteren App Service-Plan erstellen. Die verwendete virtuelle Schnittstelle ist keine Ressource, auf die Kunden direkten Zugriff haben.

Aufgrund der Funktionsweise dieser Technologie wird der Datenverkehr in Verbindung mit der VNET-Integration nicht in Network Watcher- oder NSG-Flussprotokollen (Netzwerksicherheitsgruppe) aufgeführt.  

## <a name="gateway-required-vnet-integration"></a>VNET-Integration, die ein Gateway erfordert

Die VNET-Integration mit erforderlichem Gateway unterstützt das Herstellen einer Verbindung mit einem VNET in einer anderen Region oder mit einem klassischen VNET. VNET-Integration mit erforderlichem Gateway: 

* Eine App kann nur jeweils mit einem VNET eine Verbindung herstellen
* Bis zu fünf VNETs können in einen App Service-Plan integriert werden 
* Ermöglicht die Verwendung des gleichen VNET durch mehrere Apps, ohne dass sich dies auf die für einen App Service-Plan zulässige Gesamtanzahl von VNETs auswirkt.  Wenn Sie sechs Apps verwenden, die das gleiche VNET im gleichen App Service-Plan nutzen, zählt dies als ein verwendetes VNET. 
* Unterstützt dank der SLA auf dem Gateway eine SLA mit einer Verfügbarkeit von 99,9%
* Ermöglicht Ihren Apps die Verwendung des DNS, mit dem das VNET konfiguriert ist.
* Erfordert ein routenbasiertes Virtual Network-Gateway, das mit einem SSTP-Point-to-Site-VPN konfiguriert ist, bevor es mit einer App verbunden werden kann. 

Die von einem Gateway abhängige VNET-Integration kann in den folgenden Fällen nicht verwendet werden:

* Mit Linux-Apps
* Mit einem VNET, das mit ExpressRoute verbunden ist 
* Für den Zugriff auf durch Dienstendpunkte geschützte Ressourcen
* Mit einem Koexistenzgateway, das ExpressRoute und Point-to-Site-/Site-to-Site-VPNs unterstützt

### <a name="set-up-a-gateway-in-your-vnet"></a>Einrichten eines Gateways in Ihrem VNET ###

So erstellen Sie ein Gateway

1. [Erstellen Sie ein Gatewaysubnetz][creategatewaysubnet] in Ihrem VNET.  

1. [Erstellen Sie das VPN-Gateway][creategateway]. Wählen Sie einen routenbasierten VPN-Typ aus.

1. [Legen Sie die Point-to-Site-Adressen fest][setp2saddresses]. Wenn sich das Gateway nicht in der Basic-SKU befindet, muss IKEV2 in der Point-to-Site-Konfiguration deaktiviert und SSTP ausgewählt werden. Der Point-to Site-Adressraum muss in den RFC 1918-Adressblöcken 10.0.0.0/8, 172.16.0.0/12 und 192.168.0.0/16 liegen.

Wenn Sie das Gateway nur für die Verwendung mit der App Service-VNET-Integration erstellen, müssen Sie kein Zertifikat hochladen. Das Erstellen des Gateways kann 30 Minuten dauern. Sie können Ihre App erst mit Ihrem VNET integrieren, nachdem das Gateway bereitgestellt wurde. 

### <a name="how-gateway-required-vnet-integration-works"></a>Funktionsweise der VNET-Integration, die ein Gateway erfordert

Die von einem Gateway abhängige VNET-Integration basiert auf Point-to-Site-VPN-Technologie. Point-to-Site-VPNs beschränken den Netzwerkzugriff auf den virtuellen Computer, auf dem die App gehostet wird. Apps sind darauf beschränkt, nur Datenverkehr an das Internet über Hybrid Connections oder VNET-Integration zu senden. Wenn Ihre App über das Portal so konfiguriert wurde, dass sie die VNET-Integration mit erforderlichem Gateway verwendet, wird eine komplexe Aushandlung in Ihrem Namen verwaltet, um Zertifikate auf der Gateway- und der Anwendungsseite zu erstellen und zuzuweisen. Das Endergebnis ist, dass die Worker, die Ihre Anwendungen hosten, in der Lage sind, sich direkt mit dem virtuellen Netzwerkgateway im ausgewählten VNET zu verbinden. 

![Funktionsweise der VNET-Integration, die ein Gateway erfordert][6]

### <a name="accessing-on-premises-resources"></a>Zugriff auf lokale Ressourcen

Apps können durch Integration mit VNETs, die Site-to-Site-Verbindungen aufweisen, auf lokale Ressourcen zugreifen. Wenn Sie die von einem Gateway abhängige VNET-Integration verwenden, müssen Sie Ihre lokalen VPN-Gatewayrouten mit Ihren Point-to-Site-Adressblöcken aktualisieren. Verwenden Sie beim ersten Einrichten des Site-to-Site-VPN die Skripts für die Konfiguration. Die Routen sollten damit richtig eingerichtet werden. Wenn Sie die Point-to-Site-Adressen nach dem Erstellen des Site-to-Site-VPN hinzufügen, müssen Sie die Routen manuell aktualisieren. Die Details zur Vorgehensweise variieren je nach Gateway und sind hier nicht beschrieben. Das Border Gateway Protocol (BGP) kann nicht für eine Site-to-Site-VPN-Verbindung konfiguriert werden.

Für die regionale VNET-Integration ist keine zusätzliche Konfiguration erforderlich, um Ihr VNET und das lokale Netzwerk zu erreichen. Sie müssen Ihr VNET lediglich über ExpressRoute oder ein Site-to-Site-VPN mit dem lokalen Netzwerk verbinden. 

> [!NOTE]
> Bei der von einem Gateway abhängigen VNET-Integration wird eine App nicht in ein VNET integriert, das ein ExpressRoute-Gateway enthält. Selbst wenn das ExpressRoute-Gateway im [Koexistenzmodus][VPNERCoex] konfiguriert ist, funktioniert die VNET-Integration nicht. Wenn Sie über eine ExpressRoute-Verbindung auf Ressourcen zugreifen müssen, können Sie die regionale VNET-Integration oder eine in Ihrem VNET ausgeführte [App Service-Umgebung][ASE] verwenden. 
> 
> 

### <a name="peering"></a>Peering

Wenn Sie das Peering mit der regionalen VNET-Integration verwenden, ist keine zusätzliche Konfiguration erforderlich. 

Bei der von einem Gateway abhängigen VNET-Integration müssen Sie für das Peering einige zusätzliche Elemente konfigurieren. So konfigurieren Sie Peering für Ihre App

1. Fügen Sie eine Peeringverbindung mit dem VNET hinzu, mit dem Ihre App eine Verbindung herstellt. Aktivieren Sie beim Hinzufügen der Peeringverbindung **Zugriff auf virtuelles Netzwerk zulassen** sowie **Weitergeleiteten Datenverkehr zulassen** und **Gatewaytransit zulassen**.
1. Fügen Sie in dem VNET, das mit dem VNET, mit dem Sie verbunden sind, mittels Peering verknüpft ist, eine Peeringverbindung hinzu. Aktivieren Sie beim Hinzufügen der Peeringverbindung im Ziel-VNET **Zugriff auf virtuelles Netzwerk zulassen**, und aktivieren Sie **Weitergeleiteten Datenverkehr zulassen** und **Allow remote gateways** (Remotegateways zulassen).
1. Wechseln Sie im Portal zu „App Service-Plan > Netzwerk > VNet Integration UI“ (Benutzeroberfläche der VNET-Integration).  Wählen Sie das VNET aus, mit dem Ihre App eine Verbindung herstellt. Fügen Sie den Adressbereich des VNET, das mit dem VNET, mit dem Ihre App verbunden ist, mittels Peering verknüpft ist, im Abschnitt „Routing“ hinzu.  

## <a name="managing-vnet-integration"></a>Verwalten der VNET-Integration 

Das Verbinden und Trennen der Verbindung mit einem VNET erfolgt auf App-Ebene. Vorgänge, die sich auf die VNET-Integration über mehrere Apps auswirken können, werden auf Ebene des App Service-Plans ausgeführt. Sie können im Portal unter „App > Netzwerk > VNET-Integration“ Details zu Ihrem VNET abrufen. Ähnliche Informationen finden Sie auf der ASP-Ebene im Portal unter „ASP > Netzwerk > VNET-Integration“.

Der einzige Vorgang, den Sie in der App-Ansicht Ihrer VNET-Integration durchführen können, ist das Trennen Ihrer App von dem VNET, mit dem derzeit eine Verbindung besteht. Um Ihre App von einem VNET zu trennen, wählen Sie **Verbindung trennen** aus. Wenn Sie die Verbindung mit einem VNET trennen, wird Ihre App neu gestartet. Das Trennen der Verbindung führt nicht zu Änderungen in Ihrem VNET. Das Subnetz oder Gateway wird nicht entfernt. Wenn Sie Ihr VNET löschen möchten, müssen Sie zuerst Ihre App vom VNET trennen und die darin enthaltenen Ressourcen löschen (z. B. Gateways). 

Die Benutzeroberfläche der ASP-VNET-Integration zeigt alle VNET-Integrationen an, die von den Apps in Ihrem ASP verwendet werden. Klicken Sie auf das gewünschte VNET, um Informationen dazu anzuzeigen. Es gibt zwei Aktionen, die Sie hier für die VNET-Integration mit erforderlichem Gateway durchführen können.

* **Netzwerk synchronisieren**. Der Vorgang zum Synchronisieren des Netzwerks ist nur für die von einem Gateway abhängige VNET-Integration vorgesehen. Durch die Netzwerksynchronisierung wird sichergestellt, dass Ihre Zertifikate und Netzwerkinformationen synchronisiert sind. Wenn Sie das DNS des VNET hinzufügen oder ändern, müssen Sie eine **Netzwerksynchronisierung** durchführen. Bei diesem Vorgang werden alle Apps, die dieses VNET verwenden, neu gestartet.
* **Routen hinzufügen:** Durch das Hinzufügen von Routen wird ausgehender Datenverkehr in Ihr VNET geleitet. 

**Routing der VNET-Integration mit erforderlichem Gateway**: Die in Ihrem VNET definierten Routen werden zum Weiterleiten des Datenverkehrs aus der App in Ihr VNET verwendet. Wenn Sie weiteren ausgehenden Datenverkehr in das VNET senden möchten, können Sie diese Adressblöcke hier hinzufügen. Diese Funktion kann nur mit der VNET-Integration verwendet werden, die ein Gateway erfordert. Routingtabellen wirken sich nicht auf den App-Datenverkehr aus, wenn VNET-Integration mit erforderlichem Gateway so verwendet wird wie regionale VNET-Integration.

**Zertifikate für VNET-Integration mit erforderlichem Gateway**: Wenn die von einem Gateway abhängige VNET-Integration aktiviert ist, müssen Zertifikate ausgetauscht werden, um die Sicherheit der Verbindung zu gewährleisten. Zusammen mit den Zertifikaten werden die DNS-Konfiguration, Routen und anderen Elemente, mit denen das Netzwerk beschrieben wird, ausgetauscht.
Wenn Zertifikate oder Netzwerkinformationen geändert werden, müssen Sie auf „Netzwerk synchronisieren“ klicken. Wenn Sie auf „Netzwerk synchronisieren“ klicken, bewirkt dies einen kurzen Ausfall der Verbindung zwischen der App und dem VNET. Die App wird nicht neu gestartet, aber der Konnektivitätsverlust kann dazu führen, dass Ihre Website nicht richtig funktioniert. 

## <a name="pricing-details"></a>Preisübersicht
Bei der Funktion für die regionale VNET-Integration fallen neben den Gebühren für den App Service-Tarif keine zusätzlichen Gebühren an.

Bei der VNET-Integration, die ein Gateway erfordert, fallen drei Gebühren an:

* Gebühren für den App Service-Tarif: Ihre Apps müssen in einem App Service-Plan mit dem Tarif „Standard“, „Premium“ oder „PremiumV2“ enthalten sein. Weitere Informationen zu diesen Kosten finden Sie hier: [App Service – Preise][ASPricing] 
* Datenübertragungskosten: Für ausgehende Daten fallen keine Gebühren an, auch wenn sich das VNET im selben Rechenzentrum befindet. Diese Kosten werden unter [Datenübertragung – Preisübersicht][DataPricing] beschrieben. 
* Kosten für VPN Gateway: Für das VNET-Gateway, das für das Point-to-Site-VPN erforderlich ist, fallen Kosten an. Die Details finden Sie auf der Seite [VPN Gateway – Preise][VNETPricing].

## <a name="troubleshooting"></a>Problembehandlung

[!INCLUDE [app-service-web-vnet-troubleshooting](../../includes/app-service-web-vnet-troubleshooting.md)]

## <a name="automation"></a>Automation

Es gibt CLI-Unterstützung für die regionale VNET-Integration. Für den Zugriff auf die folgenden Befehle [installieren Sie die Azure CLI][installCLI]. 

        az webapp vnet-integration --help

        Group
            az webapp vnet-integration : Methods that list, add, and remove virtual network integrations
            from a webapp.
                This command group is in preview. It may be changed/removed in a future release.
        Commands:
            add    : Add a regional virtual network integration to a webapp.
            list   : List the virtual network integrations on a webapp.
            remove : Remove a regional virtual network integration from webapp.

        az appservice vnet-integration --help

        Group
            az appservice vnet-integration : A method that lists the virtual network integrations used in an
            appservice plan.
                This command group is in preview. It may be changed/removed in a future release.
        Commands:
            list : List the virtual network integrations used in an appservice plan.

Für die VNET-Integration mit erforderlichem Gateway können Sie App Service mithilfe von PowerShell in ein virtuelles Azure-Netzwerk integrieren. Ein ausführungsbereites Skript finden Sie unter [Verbinden einer App in Azure App Service mit einem virtuellen Azure-Netzwerk](https://gallery.technet.microsoft.com/scriptcenter/Connect-an-app-in-Azure-ab7527e3).


<!--Image references-->
[1]: ./media/web-sites-integrate-with-vnet/vnetint-app.png
[2]: ./media/web-sites-integrate-with-vnet/vnetint-addvnet.png
[3]: ./media/web-sites-integrate-with-vnet/vnetint-classic.png
[5]: ./media/web-sites-integrate-with-vnet/vnetint-regionalworks.png
[6]: ./media/web-sites-integrate-with-vnet/vnetint-gwworks.png


<!--Links-->
[VNETOverview]: https://azure.microsoft.com/documentation/articles/virtual-networks-overview/ 
[AzurePortal]: https://portal.azure.com/
[ASPricing]: https://azure.microsoft.com/pricing/details/app-service/
[VNETPricing]: https://azure.microsoft.com/pricing/details/vpn-gateway/
[DataPricing]: https://azure.microsoft.com/pricing/details/data-transfers/
[V2VNETP2S]: https://azure.microsoft.com/documentation/articles/vpn-gateway-howto-point-to-site-rm-ps/
[ILBASE]: environment/create-ilb-ase.md
[V2VNETPortal]: ../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md
[VPNERCoex]: ../expressroute/expressroute-howto-coexist-resource-manager.md
[ASE]: environment/intro.md
[creategatewaysubnet]: ../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md#creategw
[creategateway]: https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal#creategw
[setp2saddresses]: https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal#addresspool
[VNETRouteTables]: https://docs.microsoft.com/azure/virtual-network/manage-route-table/
[installCLI]: https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest/
