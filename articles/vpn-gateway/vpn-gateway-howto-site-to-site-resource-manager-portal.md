---
title: 'Verbinden eines lokalen Netzwerks mit einem virtuellen Azure-Netzwerk: Site-to-Site-VPN: Portal'
description: Erstellen einer IPsec-Site-to-Site-VPN-Gateway-Verbindung zwischen Ihrem lokalen Netzwerk und einem virtuellen Azure-Netzwerk über das öffentliche Internet im Portal.
services: vpn-gateway
titleSuffix: Azure VPN Gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: how-to
ms.date: 10/22/2020
ms.author: cherylmc
ms.openlocfilehash: b8c12048283cb8cc4220cb325762b272cde4a68f
ms.sourcegitcommit: d767156543e16e816fc8a0c3777f033d649ffd3c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/26/2020
ms.locfileid: "92541314"
---
# <a name="create-a-site-to-site-connection-in-the-azure-portal"></a>Erstellen einer Site-to-Site-Verbindung im Azure-Portal

In diesem Artikel wird beschrieben, wie Sie das Azure-Portal zum Erstellen einer Site-to-Site-VPN-Gateway-Verbindung zwischen Ihrem lokalen Netzwerk und dem VNET verwenden. Die Schritte in diesem Artikel gelten für das Resource Manager-Bereitstellungsmodell. Sie können diese Konfiguration auch mit einem anderen Bereitstellungstool oder -modell erstellen. Wählen Sie hierzu in der folgenden Liste eine andere Option:

> [!div class="op_single_selector"]
> * [Azure portal](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
> * [PowerShell](vpn-gateway-create-site-to-site-rm-powershell.md)
> * [BEFEHLSZEILENSCHNITTSTELLE (CLI)](vpn-gateway-howto-site-to-site-resource-manager-cli.md)
> * [Azure-Portal (klassisch)](vpn-gateway-howto-site-to-site-classic-portal.md)
>

Eine Site-to-Site-VPN-Gateway-Verbindung wird verwendet, um Ihr lokales Netzwerk über einen IPsec/IKE-VPN-Tunnel (IKEv1 oder IKEv2) mit einem virtuellen Azure-Netzwerk zu verbinden. Für diese Art von Verbindung wird ein lokales VPN-Gerät benötigt, dem eine extern zugängliche, öffentliche IP-Adresse zugewiesen ist. Weitere Informationen zu VPN-Gateways finden Sie unter [Informationen zu VPN Gateway](vpn-gateway-about-vpngateways.md).

:::image type="content" source="./media/vpn-gateway-howto-site-to-site-resource-manager-portal/site-to-site-diagram.png" alt-text="Diagramm für die standortübergreifende Site-to-Site-VPN Gateway-Verbindung":::

## <a name="prerequisites"></a>Voraussetzungen

Vergewissern Sie sich vor Beginn der Konfiguration, dass die folgenden Voraussetzungen erfüllt sind bzw. Folgendes vorhanden ist:

* Ein Azure-Konto mit einem aktiven Abonnement. Sollten Sie über keine Organisation verfügen, [können Sie kostenlos eine erstellen](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).
* Achten Sie darauf, dass Sie ein kompatibles VPN-Gerät nutzen (und über eine Person verfügen, die es konfigurieren kann). Weitere Informationen zu kompatiblen VPN-Geräten und zur Gerätekonfiguration finden Sie unter [Informationen zu VPN-Geräten](vpn-gateway-about-vpn-devices.md).
* Vergewissern Sie sich, dass Sie über eine externe öffentliche IPv4-Adresse für Ihr VPN-Gerät verfügen.
* Falls Sie nicht mit den IP-Adressbereichen in Ihrer lokalen Netzwerkkonfiguration vertraut sind, wenden Sie sich an eine Person, die Ihnen diese Informationen zur Verfügung stellen kann. Beim Erstellen dieser Konfiguration müssen Sie die Präfixe für die IP-Adressbereiche angeben, die Azure an Ihren lokalen Standort weiterleitet. Kein Subnetz Ihres lokalen Netzwerks darf sich mit den Subnetzen des virtuellen Netzwerks überschneiden, mit dem Sie eine Verbindung herstellen möchten.

## <a name="create-a-virtual-network"></a><a name="CreatVNet"></a>Erstellen eines virtuellen Netzwerks

Erstellen Sie ein virtuelles Netzwerk (VNET) mit den folgenden Werten:

* **Ressourcengruppe:** TestRG1
* **Name:** VNet1
* **Region:** (USA) USA, Osten
* **IPv4-Adressraum** : 10.1.0.0/16
* **Subnetzname:** FrontEnd
* **Subnetzadressraum** : 10.1.0.0/24

[!INCLUDE [About cross-premises addresses](../../includes/vpn-gateway-cross-premises.md)]

[!INCLUDE [Create a virtual network](../../includes/vpn-gateway-basic-vnet-rm-portal-include.md)]

## <a name="create-a-vpn-gateway"></a><a name="VNetGateway"></a>Erstellen eines VPN-Gateways

In diesem Schritt erstellen Sie das virtuelle Netzwerkgateway für Ihr VNet. Häufig kann die Erstellung eines Gateways je nach ausgewählter Gateway-SKU mindestens 45 Minuten dauern.

### <a name="about-the-gateway-subnet"></a>Informationen zum Gatewaysubnetz

[!INCLUDE [About gateway subnets](../../includes/vpn-gateway-about-gwsubnet-portal-include.md)]

### <a name="create-the-gateway"></a>Erstellen des Gateways

Erstellen Sie ein VPN-Gateway mit den folgenden Werten:

* **Name:** VNet1GW
* **Region:** East US
* **Gatewaytyp** : VPN
* **VPN-Typ:** routenbasiert
* **SKU** : VpnGw1
* **Generation** : Generation1
* **Virtuelles Netzwerk:** VNet1
* **Adressbereich für Gatewaysubnetz** : 10.1.255.0/27
* **Öffentliche IP-Adresse** : Neu erstellen
* **Öffentliche IP-Adresse:** VNet1GWpip
* **Aktiv/Aktiv-Modus aktivieren** : Disabled
* **BGP konfigurieren** : Disabled

[!INCLUDE [Create a vpn gateway](../../includes/vpn-gateway-add-gw-rm-portal-include.md)]

[!INCLUDE [NSG warning](../../includes/vpn-gateway-no-nsg-include.md)]

## <a name="create-a-local-network-gateway"></a><a name="LocalNetworkGateway"></a>Erstellen eines Gateways für das lokale Netzwerk

Das lokale Netzwerkgateway ist ein spezielles Objekt, das Ihrem lokalen Standort für Routingzwecke entspricht. Sie geben dem Standort einen Namen, über den Azure darauf verweisen kann, und geben dann die IP-Adresse des lokalen VPN-Geräts an, mit dem Sie eine Verbindung herstellen. Außerdem geben Sie die IP-Adresspräfixe an, die über das VPN-Gateway an das VPN-Gerät weitergeleitet werden. Die von Ihnen angegebenen Adresspräfixe befinden sich in Ihrem lokalen Netzwerk. Wenn bei dem lokalen Netzwerk Änderungen vorgenommen werden oder Sie die öffentliche IP-Adresse des VPN-Geräts ändern müssen, können Sie dies Werte später bequem aktualisieren.

Erstellen Sie ein lokales Netzwerkgateway mit den folgenden Werten:

* **Name:** Site1
* **Ressourcengruppe:** TestRG1
* **Standort:** East US

[!INCLUDE [Add a local network gateway](../../includes/vpn-gateway-add-local-network-gateway-portal-include.md)]

## <a name="configure-your-vpn-device"></a><a name="VPNDevice"></a>Konfigurieren des VPN-Geräts

Für Site-to-Site-Verbindungen mit einem lokalen Netzwerk ist ein VPN-Gerät erforderlich. In diesem Schritt konfigurieren Sie Ihr VPN-Gerät. Beim Konfigurieren des VPN-Geräts benötigen Sie die folgenden Werte:

* Einen gemeinsam verwendeten Schlüssel. Dies ist derselbe gemeinsame Schlüssel, den Sie beim Erstellen Ihrer Site-to-Site-VPN-Verbindung angeben. In unseren Beispielen verwenden wir einen einfachen gemeinsamen Schlüssel. Es wird empfohlen, einen komplexeren Schlüssel zu generieren.
* Die öffentliche IP-Adresse Ihres Gateways für virtuelle Netzwerke. Sie können die öffentliche IP-Adresse mit dem Azure-Portal, mit PowerShell oder mit der CLI anzeigen. Im Azure-Portal ermitteln Sie die öffentliche IP-Adresse Ihres VPN-Gateways, indem Sie zu **Gateways für virtuelle Netzwerke** navigieren und auf den Namen Ihres Gateways klicken.

[!INCLUDE [Configure a VPN device](../../includes/vpn-gateway-configure-vpn-device-include.md)]

## <a name="create-a-vpn-connection"></a><a name="CreateConnection"></a>Erstellen einer VPN-Verbindung

Erstellen Sie die Site-to-Site-VPN-Verbindung zwischen dem Gateway Ihres virtuellen Netzwerks und Ihrem lokalen VPN-Gerät.

Erstellen Sie eine Verbindung mit den folgenden Werten:

* **Name des Gateways für das lokale Netzwerk** : Site1
* **Verbindungsname** : VNet1toSite1
* **Gemeinsam verwendeter Schlüssel:** In diesem Beispiel verwenden wir „abc123“. Sie können jedoch einen beliebigen, mit Ihrer VPN-Hardware kompatiblen Wert verwenden. Wichtig ist, dass die Werte auf beiden Seiten der Verbindung übereinstimmen.

[!INCLUDE [Add a site-to-site connection](../../includes/vpn-gateway-add-site-to-site-connection-portal-include.md)]

## <a name="verify-the-vpn-connection"></a><a name="VerifyConnection"></a>Überprüfen der VPN-Verbindung

[!INCLUDE [Verify the connection](../../includes/vpn-gateway-verify-connection-portal-include.md)]

## <a name="how-to-connect-to-a-virtual-machine"></a><a name="connectVM"></a>Herstellen einer Verbindung mit einem virtuellen Computer

[!INCLUDE [Connect to a VM](../../includes/vpn-gateway-connect-vm.md)]

## <a name="how-to-reset-a-vpn-gateway"></a><a name="reset"></a>Zurücksetzen einer VPN Gateway-Instanz

Das Zurücksetzen von Azure VPN Gateway-Instanzen ist nützlich, wenn die standortübergreifende VPN-Verbindung bei mindestens einem Site-to-Site-VPN-Tunnel unterbrochen ist. In diesem Fall funktionieren Ihre lokalen VPN-Geräte ordnungsgemäß, können jedoch keine IPsec-Tunnelverbindungen mit Azure VPN Gateway-Instanzen herstellen. Eine entsprechende Anleitung finden Sie unter [Zurücksetzen einer VPN Gateway-Instanz](reset-gateway.md).

## <a name="how-to-change-a-gateway-sku-resize-a-gateway"></a><a name="resize"></a>Ändern einer Gateway-SKU (Ändern der Gatewaygröße)

Eine Anleitung zum Ändern einer Gateway-SKU finden Sie unter [Gateway-SKUs](vpn-gateway-about-vpn-gateway-settings.md#gwsku).

## <a name="how-to-add-an-additional-connection-to-a-vpn-gateway"></a><a name="addconnect"></a>Hinzufügen zusätzlicher Verbindungen zu einem VPN-Gateway

Sie können weitere Verbindungen hinzufügen. Voraussetzung dafür ist, dass sich Adressbereiche zwischen Verbindungen nicht überlappen.

1. Um eine zusätzliche Verbindung hinzuzufügen, navigieren Sie zum VPN-Gateway, und klicken Sie auf **Verbindungen** , um die Seite „Verbindungen“ zu öffnen.
1. Klicken Sie auf **+ Hinzufügen** , um die Verbindung hinzuzufügen. Passen Sie den Verbindungstyp an, um VNET-to-VNET (falls Sie eine Verbindung mit einem anderen VNET-Gateway herstellen) oder Site-to-Site widerzuspiegeln.
1. Wenn Sie eine Verbindung über Site-to-Site herstellen und noch kein lokales Netzwerkgateway für die Website erstellt haben, mit der Sie eine Verbindung herstellen möchten, können Sie ein neues erstellen.
1. Geben Sie den gemeinsam verwendeten Schlüssel ein, den Sie verwenden möchten, und klicken Sie auf **OK** , um die Verbindung zu erstellen.

## <a name="next-steps"></a>Nächste Schritte

* Informationen zu BGP finden Sie in der [Übersicht über BGP](vpn-gateway-bgp-overview.md) und unter [Konfigurieren von BGP](vpn-gateway-bgp-resource-manager-ps.md).
* Weitere Informationen zur Tunnelerzwingung finden Sie unter [Konfigurieren der Tunnelerzwingung mit dem Azure Resource Manager-Bereitstellungsmodell](vpn-gateway-forced-tunneling-rm.md).
* Informationen zu hochverfügbaren Aktiv/Aktiv-Verbindungen finden Sie unter [Standortübergreifende Verbindungen und VNET-zu-VNET-Verbindungen mit hoher Verfügbarkeit](vpn-gateway-highlyavailable.md).
* Informationen zum Beschränken von Netzwerkdatenverkehr auf Ressourcen in einem virtuellen Netzwerk finden Sie unter [Netzwerksicherheit](../virtual-network/security-overview.md).
* Informationen zur Weiterleitung von Datenverkehr zwischen Azure, lokalen Speicherorten und Internetressourcen finden Sie unter [Routing von Datenverkehr für virtuelle Netzwerke](../virtual-network/virtual-networks-udr-overview.md).
* Informationen zum Erstellen einer Site-to-Site-VPN-Verbindung mithilfe einer Azure Resource Manager-Vorlage finden Sie unter [Create a Site-to-Site VPN Connection](https://azure.microsoft.com/resources/templates/101-site-to-site-vpn-create/) (Erstellen einer Site-to-Site-VPN-Verbindung).
* Informationen zum Erstellen einer VNET-zu-VNET-VPN-Verbindung mithilfe einer Azure Resource Manager-Vorlage finden Sie unter [Deploy HBase geo replication](https://azure.microsoft.com/resources/templates/101-hdinsight-hbase-replication-geo/) (Bereitstellen der HBase-Georeplikation).
