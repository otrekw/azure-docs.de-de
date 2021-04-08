---
title: 'Erstellen einer Verbindung zwischen VNETs: klassisch: Azure-Portal'
description: Stellen Sie mithilfe von PowerShell und dem Azure-Portal Verbindungen zwischen virtuellen Azure-Netzwerken her.
services: vpn-gateway
titleSuffix: Azure VPN Gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: how-to
ms.date: 10/15/2020
ms.author: cherylmc
ms.openlocfilehash: 0d81e0474d898ffee7f128c0bcea61f077c3d758
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "92103219"
---
# <a name="configure-a-vnet-to-vnet-connection-classic"></a>Konfigurieren einer VNet-zu-VNet-Verbindung (klassisch)

Dieser Artikel hilft Ihnen beim Erstellen einer VPN-Gatewayverbindung zwischen virtuellen Netzwerken. Die virtuellen Netzwerke können sich in derselben oder in unterschiedlichen Regionen befinden und aus demselben oder unterschiedlichen Abonnements stammen.

:::image type="content" source="./media/vpn-gateway-howto-vnet-vnet-portal-classic/v2vclassic.png" alt-text="Abbildung der klassischen VNET-zu-VNET-Architektur":::

[!INCLUDE [deployment models](../../includes/vpn-gateway-classic-deployment-model-include.md)]

Die Schritte in diesem Artikel gelten für das klassische Bereitstellungsmodell und Azure-Portal. Sie können diese Konfiguration auch mit einem anderen Bereitstellungstool oder -modell erstellen. Wählen Sie hierzu in der folgenden Liste eine andere Option:

> [!div class="op_single_selector"]
> * [Klassisch](vpn-gateway-howto-vnet-vnet-portal-classic.md)
> * [Ressourcen-Manager](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md)
> * [Verbinden von VNETs mit verschiedenen Bereitstellungsmodellen](vpn-gateway-connect-different-deployment-models-portal.md)
>
>

## <a name="about-vnet-to-vnet-connections"></a>Über VNet-zu-VNet-Verbindungen

Beim klassischen Bereitstellungsmodell ähnelt die Verbindung eines virtuellen Netzwerks mit einem anderen virtuellen Netzwerk (VNet-zu-VNet) mittels eines VPN-Gateways der Verbindung eines virtuellen Netzwerks mit einem lokalen Standort. Beide Verbindungstypen verwenden ein VPN-Gateway, um einen sicheren Tunnel mit IPsec/IKE bereitzustellen.

Die VNets, die Sie verbinden, können sich in verschiedenen Abonnements und Regionen befinden. Sie können die VNet-zu-VNet-Kommunikation mit Konfigurationen für mehrere Standorte kombinieren. Auf diese Weise können Sie Netzwerktopologien einrichten, die standortübergreifende Konnektivität mit Konnektivität zwischen virtuellen Netzwerken kombinieren.

:::image type="content" source="./media/vpn-gateway-howto-vnet-vnet-portal-classic/aboutconnections.png" alt-text="Abbildung von Verbindungen":::

### <a name="why-connect-virtual-networks"></a><a name="why"></a>Gründe für Verbindungen zwischen virtuellen Netzwerken

Aus den folgenden Gründen empfiehlt sich das Herstellen von Verbindungen zwischen virtuellen Netzwerken:

* **Regionsübergreifende Georedundanz und Geopräsenz**

  * Sie können Ihre eigene Georeplikation oder -synchronisierung mit sicheren Verbindungen einrichten, ohne Endpunkte mit Internetzugriff verwenden zu müssen.
  * Mit dem Azure Load Balancer und Clustertechnologie von Microsoft oder Drittanbietern können Sie Workloads mit hoher Verfügbarkeit mit Georedundanz über mehrere Azure-Regionen hinweg einrichten. Ein wichtiges Beispiel ist die Einrichtung von SQL Always On mit Verfügbarkeitsgruppen, die sich über mehrere Azure-Regionen erstrecken.
* **Regionale Anwendungen mit mehreren Ebenen mit starker Isolierungsgrenze**

  * Innerhalb der gleichen Region können Sie Anwendungen mit mehreren Ebenen mit mehreren verbundenen VNets einrichten, die starke Isolierung und sichere Kommunikation zwischen den Ebenen aufweisen.
* **Abonnementübergreifende Kommunikation zwischen Organisationen in Azure**

  * Wenn Sie über mehrere Azure-Abonnements verfügen, können Sie Workloads aus verschiedenen Abonnements sicher zwischen virtuellen Netzwerken verbinden.
  * Für Unternehmen und Dienstanbieter können Sie die organisationsübergreifende Kommunikation mit sicherer VPN-Technologie in Azure aktivieren.

Weitere Informationen zu VNet-zu-VNet-Verbindungen finden Sie am Ende dieses Artikels unter [Informationen zu VNet-zu-VNet-Verbindungen](#faq).

## <a name="prerequisites"></a>Voraussetzungen

Wir verwenden für die meisten Schritte das Portal. Sie müssen jedoch PowerShell verwenden, um die Verbindungen zwischen den VNets herzustellen. Sie können die Verbindungen nicht mithilfe des Azure-Portals erstellen, da der gemeinsam genutzte Schlüssel im Portal nicht angegeben werden kann. [!INCLUDE [vpn-gateway-classic-powershell](../../includes/vpn-gateway-powershell-classic-locally.md)]

## <a name="planning"></a><a name="planning"></a>Planung

Eine wichtige Entscheidung betrifft die Bereiche, die Sie zum Konfigurieren Ihrer virtuellen Netzwerke verwenden. Bei dieser Konfiguration müssen Sie sicherstellen, dass sich die VNet-Bereiche weder untereinander überlappen, noch mit lokalen Netzwerken, mit denen sie eine Verbindung herstellen.

### <a name="vnets"></a><a name="vnet"></a>VNETs

Für diese Übung verwenden Sie die folgenden Beispielwerte:

**Werte für TestVNet1**

Name: TestVNet1<br>
Adressraum: 10.11.0.0/16, 10.12.0.0/16 (optional)<br>
Subnetzname: Standard<br>
Subnetzadressbereich: 10.11.0.0/24<br>
Ressourcengruppe: ClassicRG<br>
Standort: USA, Osten<br>
GatewaySubnet: 10.11.1.0/27

**Werte für TestVNet4**

Name: TestVNet4<br>
Adressraum: 10.41.0.0/16, 10.42.0.0/16 (optional)<br>
Subnetzname: Standard<br>
Subnetzadressbereich: 10.41.0.0/24<br>
Ressourcengruppe: ClassicRG<br>
Standort: USA, Westen<br>
GatewaySubnet: 10.41.1.0/27

### <a name="connections"></a><a name="plan"></a>Verbindungen

Die folgende Tabelle zeigt ein Beispiel, wie Sie Ihre VNETs verbinden. Verwenden Sie die aufgeführten Bereiche nur als Richtlinie. Notieren Sie sich die Bereiche für Ihre virtuellen Netzwerke. Sie benötigen diese Informationen in den späteren Schritten.

In diesem Beispiel stellt „TestVNet1“ eine Verbindung mit einem lokalen Netzwerkstandort her, den Sie mit dem Namen „VNet4Local“ erstellt haben. Die Einstellungen für VNet4Local enthalten die Adresspräfixe für TestVNet4.
Der lokale Standort für jedes VNET ist das andere VNET. Die folgenden Beispielwerte werden für diese Konfiguration verwendet:

**Beispiel**

| Virtual Network | Adressraum | Standort | Verbindung mit dem lokalen Netzwerkstandort |
|:--- |:--- |:--- |:--- |
| TestVNet1 |TestVNet1<br>(10.11.0.0/16)<br>(10.12.0.0/16) |East US |SiteVNet4<br>(10.41.0.0/16)<br>(10.42.0.0/16) |
| TestVNet4 |TestVNet4<br>(10.41.0.0/16)<br>(10.42.0.0/16) |USA (Westen) |SiteVNet1<br>(10.11.0.0/16)<br>(10.12.0.0/16) |

## <a name="create-virtual-networks"></a><a name="vnetvalues"></a>Erstellen virtueller Netzwerke

In diesem Schritt erstellen Sie mit „TestVNet1“ und „TestVNet4“ zwei klassische virtuelle Netzwerke. Wenn Sie diesen Artikel als Übung nutzen, verwenden Sie die [Beispielwerte](#vnet).

**Berücksichtigen Sie bei der Erstellung der VNETs die folgenden Einstellungen:**

* **Adressräume von Virtual Network** – Geben Sie auf der Seite „Adressräume von Virtual Network“ den Adressbereich an, der für das virtuelle Netzwerk verwendet werden soll. Dies sind die dynamischen IP-Adressen, die den VMs und anderen Rolleninstanzen zugewiesen werden, die Sie für dieses virtuelle Netzwerk bereitstellen.<br>Die Adressräume, die Sie auswählen, dürfen nicht die Adressräume von anderen VNETs oder lokalen Standorten überlappen, zu denen dieses VNET eine Verbindung herstellt.

* **Standort** – Wenn Sie ein virtuelles Netzwerk erstellen, ordnen Sie dieses einem Azure-Standort (einer Region) zu. Wenn Sie z. B. wünschen, dass Ihre virtuellen Computer, die Sie für Ihr virtuelles Netzwerk bereitgestellt haben, physisch in "USA West" vorhanden sind, wählen Sie diesen Standort aus. Sie können den Ihrem virtuellen Netzwerk zugeordnete Standort nach dem Erstellen nicht mehr ändern.

**Nach der Erstellung der VNETs können Sie die folgenden Einstellungen hinzufügen:**

* **Adressraum** – Für diese Konfiguration ist kein zusätzlicher Adressraum erforderlich, Sie können jedoch nach der Erstellung des VNET zusätzliche Adressräume hinzufügen.

* **Subnetze** – Für diese Konfiguration sind keine zusätzlichen Subnetze erforderlich, aber eventuell sollten Sie Ihre VMs in einem Subnetz, das von Ihren anderen Rolleninstanzen getrennt ist, verwalten.

* **DNS-Server** – Geben Sie den DNS-Servernamen und die IP-Adresse ein. Mit dieser Einstellung wird kein DNS-Server erstellt. Sie bietet die Möglichkeit, den DNS-Server anzugeben, den Sie zur Namensauflösung für dieses virtuelle Netzwerk verwenden möchten.

### <a name="to-create-a-classic-virtual-network"></a>Erstellen eines klassischen virtuellen Netzwerks

[!INCLUDE [basic classic vnet](../../includes/vpn-gateway-vnet-classic.md)]

[!INCLUDE [basic classic DNS](../../includes/vpn-gateway-dns-classic.md)]

## <a name="configure-sites-and-gateways"></a><a name="localsite"></a>Konfigurieren von Standorten und Gateways

Azure verwendet die für die einzelnen lokalen Netzwerkstandorte angegebenen Einstellungen, um zu bestimmen, wie Datenverkehr zwischen den VNets weitergeleitet wird. Jedes VNet muss auf das entsprechende lokale Netzwerk verweisen, an das Sie Datenverkehr weiterleiten möchten. Sie legen den Namen fest, den Sie zum Verweisen auf die einzelnen lokalen Netzwerkstandorte verwenden möchten. Es wird empfohlen, einen aussagekräftigen Namen zu verwenden.

Beispielsweise stellt TestVNet1 eine Verbindung mit einem lokalen Netzwerkstandort her, den Sie mit dem Namen „VNet4Local“ erstellt haben. Die Einstellungen für VNet4Local enthalten die Adresspräfixe für TestVNet4.

Beachten Sie, dass der lokale Standort für jedes VNET das andere VNET ist.

| Virtual Network | Adressraum | Standort | Verbindung mit dem lokalen Netzwerkstandort |
|:--- |:--- |:--- |:--- |
| TestVNet1 |TestVNet1<br>(10.11.0.0/16)<br>(10.12.0.0/16) |East US |SiteVNet4<br>(10.41.0.0/16)<br>(10.42.0.0/16) |
| TestVNet4 |TestVNet4<br>(10.41.0.0/16)<br>(10.42.0.0/16) |USA (Westen) |SiteVNet1<br>(10.11.0.0/16)<br>(10.12.0.0/16) |

### <a name="to-configure-a-site"></a><a name="site"></a>Konfigurieren eines Standorts

Mit dem lokalen Standort ist in der Regel Ihr lokaler Standort gemeint. Er enthält die IP-Adresse des VPN-Geräts, mit dem Sie eine Verbindung herstellen, und die IP-Adressbereiche, die über das VPN-Gateway an das VPN-Gerät weitergeleitet werden.

1. Wählen Sie auf der Seite für Ihr VNET unter **Einstellungen** die Option **Site-to-Site-Verbindungen** aus.
1. Wählen Sie auf der Seite „Site-to-Site-Verbindungen“ **+ Hinzufügen** aus.
1. Lassen Sie auf der Seite **VPN-Verbindung und Gateway konfigurieren** für **Verbindungstyp** die Option **Standort-zu-Standort** ausgewählt.

   * **IP-Adresse des VPN-Gateways:** Dies ist die öffentliche IP-Adresse des VPN-Geräts für Ihr lokales Netzwerk. Für diese Übung können Sie eine Dummyadresse einfügen, da Sie noch nicht über die IP-Adresse für das VPN-Gateway für den anderen Standort verfügen (z. B. 5.4.3.2). Nachdem Sie das Gateway für das andere VNET konfiguriert haben, können Sie diesen Wert später anpassen.

   * **Clientadressraum**: Listen Sie die IP-Adressbereiche auf, die über dieses Gateway an das andere VNET weitergeleitet werden sollen. Sie können mehrere Adressraumbereiche hinzufügen. Stellen Sie sicher, dass sich die hier angegebenen Bereiche nicht mit den Bereichen anderer Netzwerke, mit denen für Ihr virtuelles Netzwerk Verbindungen hergestellt werden, oder mit den Adressbereichen des virtuellen Netzwerks selbst überlappen.
1. Wählen Sie unten auf der Seite NICHT die Option „Überprüfen + erstellen“ aus. Wählen Sie stattdessen **Weiter: Gateway >** aus.

### <a name="to-configure-a-virtual-network-gateway"></a><a name="sku"></a>Konfigurieren eines Gateways für virtuelle Netzwerke

1. Wählen Sie auf der Seite **Gateway** die folgenden Werte aus:

   * **Size:** Hierbei handelt es sich um die Gateway-SKU, die Sie bei der Erstellung Ihres Gateways für virtuelle Netzwerke verwenden. Klassische VPN-Gateways verwenden die alten Gateway-SKUs. Weitere Informationen zu den alten Gateway-SKUs finden Sie unter [Arbeiten mit SKUs für virtuelle Netzwerkgateways (alte SKUs)](vpn-gateway-about-skus-legacy.md). Für diese Übung können Sie **Standard** auswählen.

   * **Routingtyp**: Wählen Sie den Routingtyp für Ihr Gateway aus. Dies wird auch als VPN-Typ bezeichnet. Es ist wichtig, den richtigen Typ auszuwählen, weil es nicht möglich ist, das Gateway in einen anderen Typ zu konvertieren. Ihr VPN-Gerät muss mit dem Routingtyp kompatibel sein, den Sie auswählen. Weitere Informationen zum Routingtyp finden Sie unter [Informationen zu VPN Gateway-Einstellungen](vpn-gateway-about-vpn-gateway-settings.md#vpntype). Es kann sein, dass in Artikeln die VPN-Typen „RouteBased“ und „PolicyBased“ vorkommen. „Dynamisch“ entspricht „RouteBased“, und „Statisch“ entspricht „PolicyBased“. Wählen Sie für diese Konfiguration **Dynamisch** aus.

   * **Gatewaysubnetz:** Die Größe des von Ihnen angegebenen Gatewaysubnetzes richtet sich nach der VPN-Gatewaykonfiguration, die Sie erstellen möchten. Obwohl es möglich ist, ein Gatewaysubnetz mit einer Größe von nur /29 zu erstellen, wird /27 oder /28 empfohlen. Dadurch wird ein größeres Subnetz erstellt, das mehr Adressen enthält. Die Verwendung eines größeren Gatewaysubnetzes ermöglicht die Vergabe einer ausreichenden Zahl von IP-Adressen für potenzielle zukünftige Konfigurationen.

1. Wählen Sie unten auf der Seite die Option **Überprüfen + erstellen** aus, um die Einstellungen zu überprüfen. Klicken Sie auf **Erstellen**, um die Bereitstellung durchzuführen. Abhängig von der ausgewählten SKU kann die Erstellung eines Gateways für virtuelle Netzwerke bis zu 45 Minuten dauern.
1. Sie können mit dem nächsten Schritt fortfahren, während dieses Gateway erstellt wird.

### <a name="configure-testvnet4-settings"></a>Konfigurieren von TestVNet4-Einstellungen

Wiederholen Sie die Schritte zum [Erstellen eines Standorts und Gateways](#localsite), um „TestVNet4“ zu konfigurieren. Ersetzen Sie die Werte bei Bedarf. Wenn Sie diese Schritte als Übung durchführen, verwenden Sie die [Beispielwerte](#planning).

## <a name="update-local-sites"></a><a name="updatelocal"></a>Aktualisieren lokaler Standorte

Nachdem die Gateways für Ihre virtuellen Netzwerke für beide VNETs erstellt wurden, müssen Sie die Eigenschaften für die Einstellung **IP-Adresse des VPN-Gateways** der lokalen Standorte anpassen.

|VNet-Name|Verbundener Standort|Gateway-IP-Adresse|
|:--- |:--- |:--- |
|TestVNet1|VNet4Local|IP-Adresse des VPN-Gateways für TestVNet4|
|TestVNet4|VNet1Local|IP-Adresse des VPN-Gateways für TestVNet1|

### <a name="part-1---get-the-virtual-network-gateway-public-ip-address"></a>Teil 1 – Abrufen der öffentlichen IP-Adresse des Gateways für virtuelle Netzwerke

1. Navigieren Sie zu Ihrem VNET, indem Sie zur **Ressourcengruppe** wechseln und das virtuelle Netzwerk auswählen.
1. Suchen Sie auf der Seite für Ihr virtuelles Netzwerk im Bereich **Zusammenfassung** auf der rechten Seite nach **Gateway-IP-Adresse**, und kopieren Sie diese in die Zwischenablage.

### <a name="part-2---modify-the-local-site-properties"></a>Teil 2 – Ändern der Eigenschaften lokaler Standorte

1. Wählen Sie unter „Site-to-Site-Verbindungen“ die Verbindung aus. Beispiel: SiteVNet4.
1. Klicken Sie auf der Seite **Eigenschaften** für die Site-to-Site-Verbindung auf **Lokalen Standort bearbeiten**.
1. Fügen Sie im Feld **IP-Adresse des VPN-Gateways** die IP-Adresse des VPN-Gateways ein, die Sie im vorherigen Abschnitt kopiert haben.
1. Klicken Sie auf **OK**.
1. Das Feld wird im System aktualisiert. Sie können diese Methode auch verwenden, um zusätzliche IP-Adressen hinzuzufügen, die Sie an diesen Standort weiterleiten möchten.

### <a name="part-3---repeat-steps-for-the-other-vnet"></a>Teil 3: Wiederholen der Schritte für das andere VNET

Wiederholen Sie die Schritte für „TestVNet4“.

## <a name="retrieve-configuration-values"></a><a name="getvalues"></a>Abrufen von Konfigurationswerten

[!INCLUDE [retrieve values](../../includes/vpn-gateway-values-classic.md)]

## <a name="create-connections"></a><a name="createconnections"></a>Erstellen von Verbindungen

Nach Abschluss aller zuvor beschriebenen Schritte können Sie die vorinstallierten IPSec/IKE-Schlüssel festlegen und die Verbindung erstellen. Bei dieser Gruppe von Schritten wird auf PowerShell zurückgegriffen. Im Azure-Portal können keine VNet-to-VNet-Verbindungen für das klassische Bereitstellungsmodell konfiguriert werden, da der gemeinsam verwendete Schlüssel nicht im Portal angegeben werden kann.

Beachten Sie in den folgenden Beispielen, dass der Wert für „SharedKey“ identisch ist. Dieser Wert muss immer übereinstimmen. Achten Sie darauf, dass die Werte in diesen Beispielen durch die genauen Namen Ihrer VNETs und lokalen Netzwerkstandorte ersetzt werden.

1. Erstellen Sie die Verbindung zwischen TestVNet1 und TestVNet4. Stellen Sie sicher, dass Sie die Werte ändern.

   ```powershell
   Set-AzureVNetGatewayKey -VNetName 'Group ClassicRG TestVNet1' `
   -LocalNetworkSiteName 'value for _VNet4Local' -SharedKey A1b2C3D4
   ```
2. Erstellen Sie die Verbindung zwischen TestVNet4 und TestVNet1.

   ```powershell
   Set-AzureVNetGatewayKey -VNetName 'Group ClassicRG TestVNet4' `
   -LocalNetworkSiteName 'value for _VNet1Local' -SharedKey A1b2C3D4
   ```
3. Warten Sie, bis die Verbindung initialisiert wird. Sobald das Gateway initialisiert wurde, wechselt der Status zu „Erfolgreich“.

   ```
   Error          :
   HttpStatusCode : OK
   Id             :
   Status         : Successful
   RequestId      :
   StatusCode     : OK
   ```

## <a name="faq-and-considerations"></a><a name="faq"></a>FAQ und Überlegungen

Diese Überlegungen gelten für klassische virtuelle Netzwerke und klassische Gateways für virtuelle Netzwerke.

* Die virtuellen Netzwerke können sich im gleichen Abonnement oder in verschiedenen Abonnements befinden.
* Die virtuellen Netzwerke können sich in der gleichen Azure-Region oder in verschiedenen Azure-Regionen (Standorte) befinden.
* Ein Clouddienst oder Endpunkt mit Lastenausgleich darf auch dann nicht mehrere virtuelle Netzwerke umfassen, wenn diese verbunden sind.
* Für das Verbinden mehrerer virtueller Netzwerke sind keine VPN-Geräte erforderlich.
* VNet-zu-VNet unterstützt das Verbinden virtueller Azure-Netzwerke. Nicht unterstützt werden hingegen Verbindungen virtueller Computer oder Clouddienste, die nicht in einem virtuellen Netzwerk bereitgestellt wurden.
* Für VNet-zu-VNet sind Gateways mit dynamischem Routing erforderlich. Azure-Gateways mit statischem Routing werden nicht unterstützt.
* Virtuelle Netzwerkverbindungen können simultan mit VPNs mit mehreren Standorten verwendet werden. Maximal 10 VPN-Tunnel können für ein VPN-Gateway des virtuellen Netzwerks verwendet werden, das Verbindungen mit anderen virtuellen Netzwerken oder lokalen Standorten herstellt.
* Die Adressräume der virtuellen Netzwerke und der lokalen Netzwerkstandorte dürfen sich nicht überschneiden. Wenn sich Adressbereiche überschneiden, tritt beim Erstellen eines virtuellen Netzwerks oder beim Hochladen von NETCFG-Konfigurationsdateien ein Fehler auf.
* Redundante Tunnel zwischen einem Paar virtueller Netzwerke werden nicht unterstützt.
* Alle VPN-Tunnel des VNet (einschließlich P2S-VPNs) verwenden die verfügbare Bandbreite für das VPN-Gateway und die gleiche SLA für die Verfügbarkeit des VPN-Gateways in Azure gemeinsam.
* VNet-zu-VNet-Datenverkehr wird über den Azure-Backbone übertragen.

## <a name="next-steps"></a>Nächste Schritte

Überprüfen Sie Ihre Verbindungen. Weitere Informationen finden Sie unter [Überprüfen einer VPN-Gatewayverbindung](vpn-gateway-verify-connection-resource-manager.md).
