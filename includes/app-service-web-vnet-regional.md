---
author: ccompy
ms.service: app-service-web
ms.topic: include
ms.date: 02/27/2020
ms.author: ccompy
ms.openlocfilehash: c731e2a7d4da1a66aabb50b335d526fbb6a0a302
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "78671528"
---
Die Verwendung der regionalen VNET-Integration ermöglicht der App Zugriff auf Folgendes:

* Ressourcen im VNET in derselben Region, in die sie integriert sind. 
* Ressourcen in VNETs, die über Peering mit Ihrem VNET verbunden sind und sich in derselben Region befinden.
* Durch Dienstendpunkte geschützte Dienste.
* Ressourcen über ExpressRoute-Verbindungen.
* Ressourcen im VNET, mit dem Sie verbunden sind.
* Ressourcen über Verbindungen mit Peering, einschließlich ExpressRoute-Verbindungen.
* Private Endpunkte. 

Wenn Sie VNET-Integration mit VNETs in der gleichen Region verwenden, können Sie die folgenden Azure-Netzwerkfunktionen nutzen:

* Netzwerksicherheitsgruppen (NSGs): Sie können ausgehenden Datenverkehr mit einer Netzwerksicherheitsgruppe blockieren, die sich in Ihrem Integrationssubnetz befindet. Die Regeln für eingehenden Datenverkehr gelten nicht, da Sie die VNET-Integration nicht verwenden können, um eingehenden Zugriff auf Ihre App bereitzustellen.
* Routingtabellen (UDRs): Sie können eine Routingtabelle im Integrationssubnetz platzieren, um ausgehenden Datenverkehr an beliebige gewünschte Ziele zu senden. 

Standardmäßig leitet Ihre App nur RFC1918-Datenverkehr in das VNET weiter. Wenn Sie den gesamten ausgehenden Datenverkehr an das VNET weiterleiten möchten, wenden Sie die App-Einstellung WEBSITE_VNET_ROUTE_ALL auf Ihre App an. So konfigurieren Sie die App-Einstellung:

1. Navigieren Sie in Ihrem App-Portal zur Konfigurationsbenutzeroberfläche. Wählen Sie **Neue Anwendungseinstellung** aus.
1. Geben Sie im Feld „Name“ **WEBSITE_VNET_ROUTE_ALL** und im Feld „Wert“ den Wert **1** ein.

   ![Angeben der Anwendungseinstellung][4]

1. Klicken Sie auf **OK**.
1. Wählen Sie **Speichern** aus.

Wenn Sie den gesamten ausgehenden Datenverkehr in Ihr VNET weiterleiten, unterliegt der den NSGs und UDRs, die auf Ihr Integrationssubnetz angewendet werden. Wenn Sie den gesamten ausgehenden Datenverkehr in das VNET weiterleiten, sind Ihre ausgehenden Adressen weiterhin die ausgehenden Adressen, die in ihren App-Eigenschaften aufgeführt werden, es sei denn, Sie stellen Routen bereit, um den Datenverkehr an andere Ziele zu senden. 

Es gibt einige Einschränkungen bei der Verwendung der VNET-Integration in VNETs in derselben Region:

* Sie können nicht über Verbindungen mit globalem Peering auf Ressourcen zugreifen.
* Die Funktion ist nur bei neueren App Service-Skalierungseinheiten verfügbar, die App Service-Pläne mit dem Tarif „PremiumV2“ unterstützen.
* Das Integrationssubnetz kann nur von einem App Service-Plan verwendet werden.
* Die Funktion kann nicht für Apps im Plan „App Service (isoliert)“ in einer App Service-Umgebung verwendet werden.
* Für das Feature ist ein nicht genutztes Subnetz vom Typ /27 mit 32 Adressen oder höher in einem Resource Manager-VNET erforderlich.
* Die App und das VNET müssen sich in der gleichen Region befinden.
* Ein VNET mit einer integrierten App kann nicht gelöscht werden. Entfernen Sie die Integration, bevor Sie das VNET löschen. 
* Sie können eine Integration nur in VNETs im selben Abonnement wie die App ausführen.
* Sie können nur eine regionale VNET-Integration pro App Service-Plan verwenden. Mehrere Apps im gleichen App Service-Plan können das gleiche VNET verwenden. 
* Sie können das Abonnement einer App oder eines Plans nicht ändern, solange eine App mit regionaler VNET-Integration vorhanden ist.

Für jede Instanz des Plans wird eine Adresse verwendet. Wenn Sie Ihre App auf fünf Instanzen skalieren, werden fünf Adressen verwendet. Da die Subnetzgröße nach der Zuweisung nicht geändert werden kann, müssen Sie ein Subnetz verwenden, das für die Aufnahme jedweder Skalierung Ihrer App groß genug ist. Die empfohlene Größe ist A /26 mit 64 Adressen. /26 mit 64 Adressen unterstützt einen App Service Premium-Plan mit 30 Instanzen. Wenn Sie einen Plan zentral hoch- oder herunterskalieren, benötigen Sie für einen kurzen Zeitraum doppelt so viele Adressen. 

Wenn Ihre Apps in einem anderen Plan ein VNET erreichen sollen, das bereits mit Apps in einem anderen Plan verbunden ist, müssen Sie ein anderes Subnetz als das von der bereits vorhandenen VNET-Integration verwendete auswählen.  

Dieses Feature befindet sich für Linux in der Vorschauphase. Die Linux-Form der Funktion unterstützt nur Aufrufe von RFC 1918-Adressen (10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16).

### <a name="web--function-app-for-containers"></a>Web-/Funktions-App für Container

Wenn Sie Ihre App unter Linux mit den integrierten Images hosten, funktioniert die regionale VNET-Integration ohne weitere Änderungen. Wenn Sie eine Web-/Funktions-App für Container verwenden, müssen Sie Ihr Docker-Image so ändern, dass die VNET-Integration verwendet wird. Verwenden Sie in Ihrem Docker-Image die Umgebungsvariable PORT als Hauptlauschport des Webservers, anstatt eine hartcodierte Portnummer zu verwenden. Die Umgebungsvariable PORT wird von der Plattform automatisch beim Start des Containers festgelegt. Wenn Sie SSH verwenden, muss der SSH-Daemon so konfiguriert sein, dass er auf den Port mit der Nummer lauscht, die in der SSH_PORT-Umgebungsvariablen angegeben ist, wenn regionale VNET-Integration verwendet wird.  Die VNET-Integration mit erforderlichem Gateway unter Linux wird nicht unterstützt. 

### <a name="service-endpoints"></a>Dienstendpunkte

Die regionale VNET-Integration ermöglicht es Ihnen, Dienstendpunkte zu verwenden.  Um Dienstendpunkte mit Ihrer App zu verwenden, stellen Sie über die regionale VNET-Integration eine Verbindung mit einem ausgewählten VNET her, und konfigurieren Sie dann Dienstendpunkte in dem für die Integration verwendeten Subnetz. 

### <a name="network-security-groups"></a>Netzwerksicherheitsgruppen

Mit Netzwerk Sicherheitsgruppen können Sie eingehenden und ausgehenden Datenverkehr an Ressourcen in einem VNET blockieren. Eine App mit regionaler VNET-Integration kann eine [Netzwerksicherheitsgruppe][VNETnsg] verwenden, um ausgehenden Datenverkehr an Ressourcen in Ihrem VNET oder im Internet zu blockieren. Zum Blockieren von Datenverkehr an öffentliche Adressen muss die Anwendungseinstellung WEBSITE_VNET_ROUTE_ALL auf 1 festgelegt werden. Die Regeln für eingehenden Datenverkehr in einer NSG gelten nicht für Ihre App, weil sich die VNET-Integration nur auf ausgehenden Datenverkehr von Ihrer App auswirkt. Um den eingehenden Datenverkehr für Ihre App zu steuern, verwenden Sie das Feature für Zugriffsbeschränkungen. Eine NSG, die auf Ihr Integrationssubnetz angewendet wird, ist unabhängig von den Routen, die auf Ihr Integrationssubnetz angewendet werden, wirksam. Wenn WEBSITE_VNET_ROUTE_ALL auf 1 festgelegt wurde und Sie über keine Routen verfügen, die sich auf den Datenverkehr der öffentlichen Adresse in Ihrem Integrationssubnetz auswirken, unterliegt der gesamte ausgehende Datenverkehr den NSGs, die Ihrem Integrationssubnetz zugewiesen sind. Wenn WEBSITE_VNET_ROUTE_ALL nicht festgelegt wird, werden NSGs nur auf RFC1918-Datenverkehr angewendet.

### <a name="routes"></a>Routen

Routingtabellen ermöglichen das Weiterleiten von ausgehendem Datenverkehr von Ihrer App an beliebige Ziele. Routingtabellen wirken sich standardmäßig nur auf Datenverkehr mit dem Ziel RFC1918 aus.  Wenn Sie WEBSITE_VNET_ROUTE_ALL auf 1 festlegen, sind alle ausgehenden Aufrufe betroffen. Routen, die für Ihr Integrationssubnetz festgelegt werden, wirken sich nicht auf Antworten auf eingehende App-Anforderungen aus. Gängige Ziele können Firewallgeräte oder Gateways beinhalten. Wenn Sie den gesamten ausgehenden Datenverkehr lokal weiterleiten möchten, können Sie eine Routingtabelle verwenden, um den gesamten ausgehenden Datenverkehr an das ExpressRoute-Gateway zu senden. Wenn Sie Datenverkehr nicht an ein Gateway weiterleiten, müssen Sie unbedingt Routen im externen Netzwerk festlegen, über die Antworten zurückgesendet werden.

BGP-Routen (Border Gateway Protocol) wirken sich ebenfalls auf den App-Datenverkehr aus. Wenn Sie über BGP-Routen von einem ExpressRoute-Gateway verfügen, ist der ausgehende Datenverkehr Ihrer App betroffen. BGP-Routen wirken sich standardmäßig nur auf Datenverkehr mit dem Ziel RFC1918 aus. Wenn WEBSITE_VNET_ROUTE_ALL auf 1 festgelegt ist, kann der gesamte ausgehende Datenverkehr von ihren BGP-Routen betroffen sein. 


<!--Image references-->
[4]: ../includes/media/web-sites-integrate-with-vnet/vnetint-appsetting.png

<!--Links-->
[VNETnsg]: https://docs.microsoft.com/azure/virtual-network/security-overview/