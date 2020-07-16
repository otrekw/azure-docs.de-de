---
author: ccompy
ms.service: app-service-web
ms.topic: include
ms.date: 06/08/2020
ms.author: ccompy
ms.openlocfilehash: ee81b391587b994bd79e9f0950d041de70153b5c
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "84488788"
---
Die Verwendung der regionalen VNET-Integration ermöglicht der App Zugriff auf Folgendes:

* Ressourcen in einem VNET in derselben Region wie Ihre App.
* Ressourcen in VNETs, die mit dem VNET, in das Ihre App integriert ist, per Peering verbunden sind.
* Durch Dienstendpunkte geschützte Dienste.
* Ressourcen über Azure ExpressRoute-Verbindungen.
* Ressourcen in dem VNET, in das Sie integriert sind.
* Ressourcen über Verbindungen mit Peering, einschließlich von Azure ExpressRoute-Verbindungen.
* Private Endpunkte 

Wenn Sie VNET-Integration mit VNETs in derselben Region verwenden, können Sie die folgenden Azure-Netzwerkfunktionen nutzen:

* **Netzwerksicherheitsgruppen (NSGs)** : Sie können ausgehenden Datenverkehr mit einer NSG blockieren, die in Ihrem Integrationssubnetz platziert ist. Die Regeln für eingehenden Datenverkehr gelten nicht, da Sie die VNET-Integration nicht verwenden können, um eingehenden Zugriff auf Ihre App bereitzustellen.
* **Routingtabellen (UDRs)** : Sie können eine Routingtabelle im Integrationssubnetz platzieren, um ausgehenden Datenverkehr an beliebige gewünschte Ziele zu senden.

Standardmäßig leitet Ihre App nur RFC 1918-Datenverkehr in das VNET weiter. Wenn Sie den gesamten ausgehenden Datenverkehr an das VNET weiterleiten möchten, wenden Sie die App-Einstellung WEBSITE_VNET_ROUTE_ALL auf Ihre App an. So konfigurieren Sie die App-Einstellung:

1. Navigieren Sie in Ihrem App-Portal zur Benutzeroberfläche für die **Konfiguration**. Wählen Sie **Neue Anwendungseinstellung** aus.
1. Geben Sie **WEBSITE_VNET_ROUTE_ALL** im Feld **Name** ein, und geben Sie **1** im Feld **Wert** ein.

   ![Angeben der Anwendungseinstellung][4]

1. Klicken Sie auf **OK**.
1. Wählen Sie **Speichern** aus.

Wenn Sie den gesamten ausgehenden Datenverkehr in Ihr VNET weiterleiten, unterliegt dieser den NSGs und UDRs, die auf Ihr Integrationssubnetz angewandt werden. Wenn Sie den gesamten ausgehenden Datenverkehr in das VNET weiterleiten, sind Ihre ausgehenden Adressen weiterhin die in Ihren App-Eigenschaften aufgeführten ausgehenden Adressen, es sei denn, Sie stellen Routen bereit, um den Datenverkehr an andere Ziele zu senden.

Es gibt einige Einschränkungen bei der Verwendung der VNET-Integration in VNETs in derselben Region:

* Sie können nicht über Verbindungen mit globalem Peering auf Ressourcen zugreifen.
* Die Funktion ist nur bei neueren Azure App Service-Skalierungseinheiten verfügbar, die App Service-Pläne mit dem Tarif „PremiumV2“ unterstützen.
* Das Integrationssubnetz kann nur von einem App Service-Plan verwendet werden.
* Die Funktion kann nicht für Apps im Plan „App Service (isoliert)“ in einer App Service-Umgebung verwendet werden.
* Für das Feature ist ein nicht genutztes Subnetz vom Typ /27 mit 32 Adressen oder höher in einem Azure Resource Manager-VNET erforderlich.
* Die App und das VNET müssen sich in derselben Region befinden.
* Es ist nicht möglich, ein VNET mit einer integrierten App zu löschen. Entfernen Sie die Integration, bevor Sie das VNET löschen.
* Sie können nur VNETs im selben Abonnement wie die App integrieren.
* Sie können nur eine regionale VNET-Integration pro App Service-Plan verwenden. Mehrere Apps im gleichen App Service-Plan können das gleiche VNET verwenden.
* Sie können das Abonnement einer App oder eines Plans nicht ändern, solange eine App mit regionaler VNET-Integration vorhanden ist.
* Ihre App kann ohne Konfigurationsänderungen keine Adressen in Azure DNS Private Zones auflösen.

Für jede Instanz des Plans wird eine Adresse verwendet. Wenn Sie Ihre App auf fünf Instanzen skalieren, werden fünf Adressen verwendet. Da die Subnetzgröße nach der Zuweisung nicht geändert werden kann, müssen Sie ein Subnetz verwenden, das für die Aufnahme jedweder Skalierung Ihrer App groß genug ist. Die empfohlene Größe ist A /26 mit 64 Adressen. /26 mit 64 Adressen unterstützt einen App Service Premium-Plan mit 30 Instanzen. Wenn Sie einen Plan zentral hoch- oder herunterskalieren, benötigen Sie für einen kurzen Zeitraum doppelt so viele Adressen.

Wenn Ihre Apps in einem anderen Plan ein VNET erreichen sollen, das bereits mit Apps in einem anderen Plan verbunden ist, müssen Sie ein anderes Subnetz als das von der bereits vorhandenen VNET-Integration verwendete auswählen.

Das Feature wird für Windows- und Linux-Web-Apps vollständig unterstützt. Das Verhalten ist in Windows- und Linux-Apps gleich.

### <a name="service-endpoints"></a>Dienstendpunkte

Die regionale VNET-Integration ermöglicht es Ihnen, Dienstendpunkte zu verwenden. Um Dienstendpunkte mit Ihrer App zu verwenden, stellen Sie über die regionale VNET-Integration eine Verbindung mit einem ausgewählten VNET her, und konfigurieren Sie dann in dem für die Integration verwendeten Subnetz Dienstendpunkte für den Zieldienst. Wenn Sie dann über Dienstendpunkte auf einen Dienst zugreifen möchten:

1. Konfigurieren Sie die Integration des regionalen VNET mit Ihrer Web-App.
1. Wechseln Sie zum Zieldienst, und konfigurieren Sie Dienstendpunkte für das Subnetz, das für die Integration verwendet wird.

### <a name="network-security-groups"></a>Netzwerksicherheitsgruppen

Mit Netzwerksicherheitsgruppen können Sie eingehenden und ausgehenden Datenverkehr an bzw. von Ressourcen in einem VNET blockieren. Eine App mit regionaler VNET-Integration kann eine [Netzwerksicherheitsgruppe][VNETnsg] verwenden, um ausgehenden Datenverkehr an Ressourcen in Ihrem VNET oder im Internet zu blockieren. Zum Blockieren von Datenverkehr an öffentliche Adressen muss die Anwendungseinstellung WEBSITE_VNET_ROUTE_ALL auf 1 festgelegt werden. Die Regeln für eingehenden Datenverkehr in einer NSG gelten nicht für Ihre App, weil sich die VNET-Integration nur auf ausgehenden Datenverkehr von Ihrer App auswirkt.

Um den eingehenden Datenverkehr für Ihre App zu steuern, verwenden Sie das Feature für Zugriffsbeschränkungen. Eine NSG, die auf Ihr Integrationssubnetz angewendet wird, ist unabhängig von den Routen wirksam, die auf Ihr Integrationssubnetz angewendet werden. Wenn WEBSITE_VNET_ROUTE_ALL auf 1 festgelegt ist und Sie über keine Routen verfügen, die sich auf den Datenverkehr der öffentlichen Adresse in Ihrem Integrationssubnetz auswirken, unterliegt der gesamte ausgehende Datenverkehr den NSGs, die Ihrem Integrationssubnetz zugewiesen sind. Wenn WEBSITE_VNET_ROUTE_ALL nicht festgelegt ist, werden NSGs nur auf RFC1918-Datenverkehr angewendet.

### <a name="routes"></a>Routen

Sie können mithilfe von Routingtabellen ausgehenden Datenverkehr von Ihrer App an beliebige Ziele weiterleiten. Routingtabellen wirken sich standardmäßig nur auf Datenverkehr mit dem Ziel RFC1918 aus. Wenn Sie WEBSITE_VNET_ROUTE_ALL auf 1 festlegen, sind alle ausgehenden Aufrufe betroffen. Routen, die für Ihr Integrationssubnetz festgelegt werden, wirken sich nicht auf Antworten auf eingehende App-Anforderungen aus. Gängige Ziele können Firewallgeräte oder Gateways beinhalten.

Wenn Sie den gesamten ausgehenden Datenverkehr lokal weiterleiten möchten, können Sie eine Routingtabelle verwenden, um den gesamten ausgehenden Datenverkehr an das ExpressRoute-Gateway zu senden. Wenn Sie Datenverkehr nicht an ein Gateway weiterleiten, müssen Sie unbedingt Routen im externen Netzwerk festlegen, über die Antworten zurückgesendet werden.

BGP-Routen (Border Gateway Protocol) wirken sich ebenfalls auf den App-Datenverkehr aus. Wenn Sie über BGP-Routen von einem ExpressRoute-Gateway verfügen, ist der ausgehende Datenverkehr Ihrer App betroffen. BGP-Routen wirken sich standardmäßig nur auf Datenverkehr mit dem Ziel RFC1918 aus. Wenn WEBSITE_VNET_ROUTE_ALL auf 1 festgelegt ist, kann der gesamte ausgehende Datenverkehr von ihren BGP-Routen betroffen sein.

### <a name="azure-dns-private-zones"></a>Azure DNS Private Zones 

Nachdem Ihre App in Ihr VNET integriert wurde, verwendet sie den DNS-Server, mit dem Ihr VNET konfiguriert ist. Standardmäßig funktioniert Ihre App nicht mit Azure DNS Private Zones. Um mit Azure DNS Private Zones zu arbeiten, müssen Sie die folgenden App-Einstellungen hinzufügen:

1. WEBSITE_DNS_SERVER mit dem Wert „168.63.129.16“ 
1. WEBSITE_VNET_ROUTE_ALL mit dem Wert „1“

Mit diesen Einstellungen werden alle ausgehenden Aufrufe von Ihrer App an das VNET gesendet, und Ihre App kann Azure DNS Private Zones verwenden.

### <a name="private-endpoints"></a>Private Endpunkte

Wenn Sie [private Endpunkte][privateendpoints] aufrufen möchten, müssen Sie eine Integration mit Azure DNS Private Zones durchführen oder den privaten Endpunkt auf dem von der App verwendeten DNS-Server verwalten. 

<!--Image references-->
[4]: ../includes/media/web-sites-integrate-with-vnet/vnetint-appsetting.png

<!--Links-->
[VNETnsg]: https://docs.microsoft.com/azure/virtual-network/security-overview/
[privateendpoints]: https://docs.microsoft.com/azure/app-service/networking/private-endpoint
