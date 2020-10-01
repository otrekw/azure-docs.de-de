---
title: 'Konfigurieren von BGP für VPN Gateway: Portal'
titleSuffix: Azure VPN Gateway
description: In diesem Artikel werden die Schritte zum Konfigurieren von BGP mit Azure VPN Gateway über PowerShell beschrieben.
services: vpn-gateway
author: yushwang
ms.service: vpn-gateway
ms.topic: how-to
ms.date: 09/18/2020
ms.author: yushwang
ms.openlocfilehash: f52d684d1e6ef63fdf4287c610608061f30395f8
ms.sourcegitcommit: bdd5c76457b0f0504f4f679a316b959dcfabf1ef
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90992932"
---
# <a name="how-to-configure-bgp-on-azure-vpn-gateways"></a>Konfigurieren von BGP für Azure-VPN-Gateways

Dieser Artikel beschreibt die Schritte zum Aktivieren von BGP für eine standortübergreifende Site-to-Site-VPN-Verbindung (S2S) und eine VNET-zu-VNET-Verbindung über das Azure-Portal.

## <a name="about-bgp"></a><a name="about"></a>Informationen zu BGP

BGP ist das standardmäßige Routingprotokoll, mit dem im Internet üblicherweise Routing- und Erreichbarkeitsinformationen zwischen mehren Netzwerken ausgetauscht werden. Azure-VPN-Gateways und Ihre lokalen VPN-Geräte (sogenannte BGP-Peers oder Nachbarn) können über BGP Routen austauschen, die beide Gateways über die Verfügbarkeit und Erreichbarkeit der Präfixe informieren, die die beteiligten Gateways oder Router durchlaufen. BGP ermöglicht auch Transitrouting zwischen mehreren Netzwerken. Hierzu werden von einem BGP-Gateway ermittelte Routen eines BGP-Peers an alle anderen BGP-Peers weitergegeben.

Weitere Informationen zu den Vorteilen von BGP sowie zu den technischen Anforderungen und den Überlegungen zur Verwendung von BGP finden Sie unter [Übersicht über BGP mit Azure VPN Gateways](vpn-gateway-bgp-overview.md).

## <a name="getting-started"></a>Erste Schritte

Jeder Teil dieses Artikels hilft Ihnen beim Erstellen eines grundlegenden Bausteins zum Aktivieren von BGP in Ihrer Netzwerkverbindung. Wenn Sie alle drei Teile abgeschlossen haben, erstellen Sie die Topologie wie in Diagramm 1 gezeigt.

**Diagramm 1**

:::image type="content" source="./media/bgp-howto/bgp-crosspremises-v2v.png" alt-text="Diagramm der Netzwerkarchitektur und -einstellungen" border="false":::

Zum Erstellen eines komplexeren Multihop-Übertragungsnetzwerk, das Ihren Anforderungen entspricht, können Sie auch Teile miteinander kombinieren.

### <a name="prerequisites"></a>Voraussetzungen

Stellen Sie sicher, dass Sie über ein Azure-Abonnement verfügen. Wenn Sie noch kein Azure-Abonnement besitzen, können Sie Ihre [MSDN-Abonnentenvorteile](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) aktivieren oder sich für ein [kostenloses Konto](https://azure.microsoft.com/pricing/free-trial/) registrieren.

## <a name="part-1-configure-bgp-on-the-virtual-network-gateway"></a><a name ="config"></a>Teil 1: Konfigurieren von BGP für das Gateway des virtuellen Netzwerks

In diesem Abschnitt erstellen und konfigurieren Sie ein virtuelles Netzwerk sowie ein Gateway für das virtuelle Netzwerk mit BGP-Parametern und rufen die Azure-BGP-Peer-IP-Adresse ab. In Diagramm 2 werden die Konfigurationseinstellungen für die Schritte in diesem Abschnitt gezeigt.

**Diagramm 2**

:::image type="content" source="./media/bgp-howto/bgp-gateway.png" alt-text="Diagramm der Netzwerkarchitektur und -einstellungen" border="false":::

### <a name="1-create-and-configure-testvnet1"></a>1. Erstellen und Konfigurieren von „TestVNet1“

In diesem Schritt erstellen und konfigurieren Sie „TestVNet1“. Führen Sie die Schritte im [Tutorial zum Erstellen eines Gateways](vpn-gateway-tutorial-create-gateway-powershell.md) aus, um Ihr virtuelles Azure-Netzwerk und Ihr VPN-Gateway zu erstellen und zu konfigurieren. Verwenden Sie die Referenzeinstellungen in den nachstehenden Screenshots.

* Virtuelles Netzwerk:

   :::image type="content" source="./media/bgp-howto/testvnet-1.png" alt-text="Diagramm der Netzwerkarchitektur und -einstellungen":::

* Subnetze:

   :::image type="content" source="./media/bgp-howto/testvnet-1-subnets.png" alt-text="Diagramm der Netzwerkarchitektur und -einstellungen":::

### <a name="2-create-the-vpn-gateway-for-testvnet1-with-bgp-parameters"></a>2. Erstellen des VPN-Gateways für „TestVNet1“ mit BGP-Parametern

In diesem Schritt erstellen Sie ein VPN-Gateway mit den zugehörigen BGP-Parametern.

1. Navigieren Sie im Azure-Portal zur Ressource **Gateway für virtuelle Netzwerke** aus dem Marketplace, und wählen Sie **Erstellen** aus.

1. Geben Sie die Parameter wie nachstehend gezeigt ein:

   :::image type="content" source="./media/bgp-howto/create-gateway-1.png" alt-text="Diagramm der Netzwerkarchitektur und -einstellungen":::

1. Konfigurieren Sie im hervorgehobenen Abschnitt **Configure BGP** (BGP konfigurieren) der Seite die folgenden Einstellungen:

   :::image type="content" source="./media/bgp-howto/create-gateway-1-bgp.png" alt-text="Diagramm der Netzwerkarchitektur und -einstellungen"::: (BGP konfigurieren)

   * Wählen Sie **Configure BGP (BGP konfigurieren)**  - **Aktiviert** aus, um den Abschnitt für die BGP-Konfiguration anzuzeigen.

   * Geben Sie Ihre ASN (Autonomous System Number) ein.

   * Das Feld **Azure APIPA BGP IP address** (Azure-APIPA-BGP-IP-Adresse) ist optional. Wenn Ihre lokalen VPN-Geräte die APIPA-Adresse für BGP verwenden, müssen Sie eine Adresse aus dem für Azure reservierten APIPA-Adressbereich für das VPN von **169.254.21.0-** bis **169.254.22.255** auswählen. In diesem Beispiel wird 169.254.21.11 verwendet.

   * Wenn Sie ein Aktiv/Aktiv-VPN-Gateway erstellen, wird im BGP-Abschnitt eine zusätzliche **zweite benutzerdefinierte Azure-APIPA-BGP-IP-Adresse** angezeigt. Geben Sie eine andere Adresse aus dem zulässigen APIPA-Bereich (**169.254.21.0** bis **169.254.22.255**) an.

   > [!IMPORTANT]
   >
   > * Standardmäßig weist Azure automatisch eine private IP-Adresse aus dem Präfixbereich des Gatewaysubnetzes als Azure-BGP-IP-Adresse für das Azure-VPN-Gateway zu. Die benutzerdefinierte Azure-APIPA-BGP-Adresse ist erforderlich, wenn Ihre lokalen VPN-Geräte eine APIPA-Adresse (169.254.0.1 bis 169.254.255.254) als BGP-IP-Adresse verwenden. Azure VPN Gateway wählt die benutzerdefinierte APIPA-Adresse aus, wenn die zugehörige lokale Netzwerkgateway-Ressource (lokales Netzwerk) eine APIPA-Adresse als BGP-Peer-IP-Adresse aufweist. Wenn das lokale Netzwerkgateway eine reguläre IP-Adresse (nicht APIPA) verwendet, wird Azure VPN Gateway auf die private IP-Adresse aus dem Bereich des Gatewaysubnetzes zurückgesetzt.
   >
   > * Die APIPA-BGP-Adressen dürfen sich zwischen den lokalen VPN-Geräten und allen verbundenen Azure-VPN-Gateways nicht überschneiden.
   >

1. Wählen Sie zum Ausführen der Validierung **Bewerten + erstellen** aus. Wählen Sie nach Abschluss der Validierung **Erstellen** aus, um das VPN-Gateway bereitzustellen. Die gesamte Erstellung und Bereitstellung eines Gateways kann bis zu 45 Minuten dauern. Der Bereitstellungsstatus wird auf der Übersichtsseite für Ihr Gateway angezeigt.

### <a name="3-obtain-the-azure-bgp-peer-ip-addresses"></a>3. Abrufen der Azure-BGP-Peer-IP-Adressen

Nachdem das Gateway erstellt wurde, können Sie die BGP-Peer-IP-Adressen des Azure-VPN-Gateways abrufen. Diese Adressen sind zum Konfigurieren Ihrer lokalen VPN-Geräte für das Einrichten von BGP-Sitzungen mit dem Azure-VPN-Gateway erforderlich.

1. Navigieren Sie zur Ressource des Gateways für virtuelle Netzwerke, und wählen Sie die Seite **Konfiguration** aus, um die BGP-Konfigurationsinformationen anzuzeigen, wie im folgenden Screenshot gezeigt. Auf dieser Seite können Sie alle BGP-Konfigurationsinformationen für Ihr Azure-VPN-Gateway anzeigen: ASN, öffentliche IP-Adresse und die entsprechenden BGP-Peer-IP-Adressen auf Azure-Seite (Standard und APIPA).

   :::image type="content" source="./media/bgp-howto/vnet-1-gw-bgp.png" alt-text="Diagramm der Netzwerkarchitektur und -einstellungen":::

1. Auf der Seite **Konfiguration** können Sie die folgenden Konfigurationsänderungen vornehmen:

   * Sie können bei Bedarf die ASN oder die APIPA-BGP-IP-Adresse aktualisieren.
   * Wenn Sie über ein Aktiv/Aktiv-VPN-Gateway verfügen, werden auf dieser Seite die öffentliche IP-Adresse, die Standard- und die APIPA-BGP-IP-Adressen der zweiten Azure VPN Gateway-Instanz angezeigt.

1. Wenn Sie Änderungen vorgenommen haben, wählen Sie **Speichern** aus, um die Änderungen an Ihrem Azure-VPN-Gateway zu übertragen.

## <a name="part-2-configure-bgp-on-cross-premises-s2s-connections"></a><a name ="crosspremises"></a>Teil 2: Konfigurieren von BGP für standortübergreifende Site-to-Site-Verbindungen

Um eine standortübergreifende Verbindung herzustellen, müssen Sie ein *Gateway des lokalen Netzwerks* für das lokale VPN-Gerät und eine *Verbindung* erstellen, um das VPN-Gateway mit dem Gateway des lokalen Netzwerks zu verbinden, wie unter [Einrichten einer Standort-zu-Standort-Verbindung](vpn-gateway-howto-site-to-site-resource-manager-portal.md) erläutert. Dieser Artikel enthält die zusätzlichen Eigenschaften, die zum Festlegen der BGP-Konfigurationsparameter erforderlich sind.

**Diagramm 3**

:::image type="content" source="./media/bgp-howto/bgp-crosspremises.png" alt-text="Diagramm der Netzwerkarchitektur und -einstellungen" border="false":::

### <a name="1-configure-bgp-on-the-local-network-gateway"></a>1. Konfigurieren von BGP für das Gateway des lokalen Netzwerks

In diesem Schritt konfigurieren Sie BGP für das Gateway des lokalen Netzwerks. Der folgende Screenshot dient als Beispiel. Der Screenshot zeigt das Gateway des lokalen Netzwerks (Site5) mit den in Diagramm 3 angegebenen Parametern.

:::image type="content" source="./media/bgp-howto/create-local-bgp.png" alt-text="Diagramm der Netzwerkarchitektur und -einstellungen":::

#### <a name="important-configuration-considerations"></a>Wichtige Überlegungen zur Konfiguration

* Die ASN und die BGP-Peer-IP-Adresse müssen mit Ihrer lokalen VPN-Routerkonfiguration übereinstimmen.
* Den **Adressraum** können Sie nur dann leer lassen, wenn Sie BGP für die Verbindung mit diesem Netzwerk verwenden. Azure VPN Gateway fügt intern eine Route Ihrer BGP-Peer-IP-Adresse zum entsprechenden IPsec-Tunnel hinzu. Wenn Sie BGP zwischen dem Azure-VPN-Gateway und diesem Netzwerk **NICHT** verwenden, **müssen** Sie eine Liste gültiger Adresspräfixe für den **Adressraum** bereitstellen.
* Bei Bedarf können Sie optional eine **APIPA-IP-Adresse** (169.254.x.x) als lokale BGP-Peer-IP-Adresse verwenden. Sie müssen jedoch auch eine APIPA-IP-Adresse angeben, wie weiter oben in diesem Artikel für Ihr Azure-VPN-Gateway beschrieben. Andernfalls kann die BGP-Sitzung für diese Verbindung nicht eingerichtet werden.
* Sie können die BGP-Konfigurationsinformationen beim Erstellen des Gateways des lokalen Netzwerks eingeben, oder Sie können die BGP-Konfiguration auf der Seite **Konfiguration** der Ressource für das Gateway des lokalen Netzwerks hinzufügen oder ändern.

**Beispiel**

In diesem Beispiel wird eine APIPA-Adresse (169.254.100.1) als lokale BGP-Peer-IP-Adresse verwendet:

:::image type="content" source="./media/bgp-howto/local-apipa.png" alt-text="Diagramm der Netzwerkarchitektur und -einstellungen":::

### <a name="2-configure-a-s2s-connection-with-bgp-enabled"></a>2. Konfigurieren einer Site-to-Site-Verbindung mit aktiviertem BGP

In diesem Schritt erstellen Sie eine neue Verbindung, bei der BGP aktiviert ist. Wenn Sie bereits über eine Verbindung verfügen und BGP aktivieren möchten, können Sie [eine vorhandene Verbindung aktualisieren](#update).

#### <a name="to-create-a-connection-with-bgp-enabled"></a>So erstellen Sie eine Verbindung mit aktiviertem BGP

Wenn Sie eine neue Verbindung mit aktiviertem BGP erstellen möchten, geben Sie auf der Seite **Verbindung hinzufügen** Ihre Werte ein, und aktivieren Sie dann die Option **BGP aktivieren**, um BGP für diese Verbindung zu aktivieren. Wählen Sie **OK** aus, um die Verbindung zu erstellen.

:::image type="content" source="./media/bgp-howto/ipsec-connection-bgp.png" alt-text="Diagramm der Netzwerkarchitektur und -einstellungen":::

#### <a name="to-update-an-existing-connection"></a><a name ="update"></a>So aktualisieren Sie eine vorhandene Verbindung

Wenn Sie die BGP-Option für eine Verbindung ändern möchten, navigieren Sie zur Seite **Konfiguration** der Verbindungsressource, und schalten Sie die Option **BGP** um, wie im folgenden Beispiel hervorgehoben. Wählen Sie **Speichern** aus, um die Änderungen zu speichern.

:::image type="content" source="./media/bgp-howto/update-bgp.png" alt-text="Diagramm der Netzwerkarchitektur und -einstellungen":::

## <a name="part-3-configure-bgp-on-vnet-to-vnet-connections"></a><a name ="v2v"></a>Teil 3: Konfigurieren von BGP für VNET-zu-VNET-Verbindungen

Die Schritte zum Aktivieren oder Deaktivieren von BGP für eine VNET-zu-VNET-Verbindung entsprechen den S2S-Schritten in [Teil 2](#crosspremises). Sie können BGP beim Erstellen der Verbindung aktivieren oder die Konfiguration für eine vorhandene VNET-zu-VNET-Verbindung aktualisieren.

>[!NOTE] 
>Bei einer VNET-zu-VNET-Verbindung ohne BGP wird die Kommunikation auf die beiden verbundenen VNETs beschränkt. Aktivieren Sie BGP, um das Transitrouting für andere S2S- oder VNET-zu-VNET-Verbindungen dieser beiden VNETs zuzulassen.
>

Zur Veranschaulichung gilt nach **Diagramm 4**: Wenn BGP zwischen TestVNet2 und TestVNet1 deaktiviert wäre, würde TestVNet2 die Routen für das lokale Netzwerk Site5 nicht erlernen und könnte daher nicht mit Site5 kommunizieren. Nachdem Sie BGP entsprechend Diagramm 4 aktiviert haben, können alle drei Netzwerke über IPsec- und VNET-zu-VNET-Verbindungen kommunizieren.

**Diagramm 4**

:::image type="content" source="./media/bgp-howto/bgp-crosspremises-v2v.png" alt-text="Diagramm der Netzwerkarchitektur und -einstellungen" border="false":::

## <a name="next-steps"></a>Nächste Schritte

Sobald die Verbindung hergestellt ist, können Sie Ihren virtuellen Netzwerken virtuelle Computer hinzufügen. Für diese Schritte finden Sie Informationen unter [Erstellen eines virtuellen Computers](../virtual-machines/windows/quick-create-portal.md) .
