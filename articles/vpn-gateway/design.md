---
title: Informationen zu Azure VPN Gateway-Entwurf
description: Hier erfahren Sie, wie Sie eine VPN-Gatewaytopologie für Verbindungen mit virtuellen Azure-Netzwerken entwerfen können.
services: vpn-gateway
author: cherylmc
Customer intent: As someone with a basic network background, but is new to Azure, I want to understand the capabilities of Azure VPN Gateway so that I can securely connect to my Azure virtual networks.
ms.service: vpn-gateway
ms.topic: article
ms.date: 09/03/2020
ms.author: cherylmc
ms.openlocfilehash: ecfb569f2791b674fe842e1c21cba972a8e9d2a7
ms.sourcegitcommit: bf1340bb706cf31bb002128e272b8322f37d53dd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/03/2020
ms.locfileid: "89443006"
---
# <a name="vpn-gateway-design"></a>VPN Gateway-Entwurf

Wichtig: Für VPN-Gateway-Verbindungen sind verschiedene Konfigurationen verfügbar. Sie müssen ermitteln, welche Konfiguration am besten zu Ihren Anforderungen passt. In den folgenden Abschnitten finden Sie Entwurfsinformationen und Topologiediagramme zu den folgenden VPN-Gatewayverbindungen. Orientieren Sie sich bei der Wahl einer geeigneten Verbindungstopologie an den Diagrammen und Beschreibungen. Die Diagramme zeigen die grundlegenden Topologien, aber Sie können auch komplexere Topologien erstellen, indem Sie die Diagramme als Anhaltspunkte verwenden.

## <a name="site-to-site-and-multi-site-ipsecike-vpn-tunnel"></a><a name="s2smulti"></a>Site-to-Site und Multi-Site (IPsec-/IKE-VPN-Tunnel)

### <a name="site-to-site"></a><a name="S2S"></a>Site-to-Site

Eine VPN Gateway-S2S-Verbindung (Site-to-Site) ist eine Verbindung über einen VPN-Tunnel vom Typ „IPsec/IKE“ (IKEv1 oder IKEv2). S2S-Verbindungen können für standortübergreifende Konfigurationen und Hybridkonfigurationen verwendet werden. Für S2S-Verbindungen wird ein lokales VPN-Gerät benötigt, dem eine öffentliche IP-Adresse zugewiesen ist. Informationen zum Auswählen eines VPN-Geräts finden Sie unter [Häufig gestellte Fragen zum VPN-Gateway](vpn-gateway-vpn-faq.md#s2s).

![Beispiel für Site-to-Site-Verbindung per Azure VPN Gateway](./media/design/vpngateway-site-to-site-connection-diagram.png)

VPN Gateway kann im Aktiv/Standby-Modus mit einer öffentlichen IP-Adresse oder im Aktiv/Aktiv-Modus mit zwei öffentlichen IP-Adressen konfiguriert werden. Im Aktiv/Standby-Modus ist ein IPSec-Tunnel aktiv, und der andere Tunnel befindet sich im Standbymodus. Bei dieser Konfiguration fließt der Datenverkehr durch den aktiven Tunnel, und wenn ein Problem mit diesem Tunnel auftritt, schaltet der Datenverkehr auf den Standbytunnel um. Das Einrichten von VPN Gateway im Aktiv/Aktiv-Modus wird *empfohlen*, da hier beide IPSec-Tunnel gleichzeitig aktiv sind und Daten gleichzeitig durch beide Tunnel fließen. Ein zusätzlicher Vorteil des Aktiv/Aktiv-Modus ist, dass Kunden ein höheren Durchsatz geboten wird.

### <a name="multi-site"></a><a name="Multi"></a>Multi-Site

Bei dieser Art von Verbindung handelt es sich um eine Abwandlung der Site-to-Site-Verbindung. Sie erstellen mehrere VPN-Verbindungen über Ihr Gateway für virtuelle Netzwerke, durch die in der Regel mehrere lokale Standorte verbunden werden. Bei Verwendung mehrerer Verbindungen müssen Sie den VPN-Typ „RouteBased“ verwenden (wird bei Verwendung klassischer VNets als dynamisches Gateway bezeichnet). Da jedes virtuelle Netzwerk nur über ein einzelnes VPN-Gateway verfügen kann, wird die verfügbare Bandbreite von allen über das Gateway laufenden Verbindungen gemeinsam genutzt. Diese Art von Verbindung wird häufig als Multi-Site-Verbindung bezeichnet.

![Beispiel für Multi-Site-Verbindung per Azure VPN Gateway](./media/design/vpngateway-multisite-connection-diagram.png)

### <a name="deployment-models-and-methods-for-site-to-site-and-multi-site"></a>Bereitstellungsmodelle und -methoden für Site-to-Site und Multi-Site

[!INCLUDE [site-to-site and multi-site table](../../includes/vpn-gateway-table-site-to-site-include.md)]

## <a name="point-to-site-vpn"></a><a name="P2S"></a>Point-to-Site-VPN

Mit einer P2S-VPN-Gatewayverbindung (Point-to-Site) können Sie von einem einzelnen Clientcomputer aus eine sichere Verbindung mit Ihrem virtuellen Netzwerk herstellen. Eine P2S-Verbindung wird hergestellt, indem Sie die Verbindung vom Clientcomputer aus starten. Diese Lösung ist nützlich für Telearbeiter, die an einem Remotestandort (beispielsweise zu Hause oder in einer Konferenz) eine Verbindung mit Azure-VNETs herstellen möchten. Wenn nur einige wenige Clients eine Verbindung mit einem VNET herstellen müssen, ist ein P2S-VPN (und nicht ein S2S-VPN) ebenfalls eine nützliche Lösung.

Im Gegensatz zu S2S-Verbindungen ist für P2S-Verbindungen keine lokale öffentliche IP-Adresse bzw. kein VPN-Gerät erforderlich. P2S-Verbindungen können mit S2S-Verbindungen über das gleiche VPN-Gateway verwendet werden – vorausgesetzt, alle Konfigurationsanforderungen beider Verbindungen sind kompatibel. Weitere Informationen zu Point-to-Site-Verbindungen finden Sie unter [About Point-to-Site VPN](point-to-site-about.md) (Informationen zu P2S-VPN).

![Beispiel für Point-to-Site-Verbindung per Azure VPN Gateway](./media/design/point-to-site.png)

### <a name="deployment-models-and-methods-for-p2s"></a>Bereitstellungsmodelle und -methoden für P2S

[!INCLUDE [vpn-gateway-table-site-to-site](../../includes/vpn-gateway-table-point-to-site-include.md)]

## <a name="vnet-to-vnet-connections-ipsecike-vpn-tunnel"></a><a name="V2V"></a>VNet-zu-VNet-Verbindungen (IPsec-/IKE-VPN-Tunnel)

Das Verbinden eines virtuellen Netzwerks mit einem anderen virtuellen Netzwerk (VNet-zu-VNet) ähnelt dem Verbinden eines VNet mit einem lokalen Standort. Beide Verbindungstypen verwenden ein VPN-Gateway, um einen sicheren Tunnel mit IPsec/IKE bereitzustellen. Die VNet-zu-VNet-Kommunikation kann sogar mit Multi-Site-Verbindungskonfigurationen kombiniert werden. Auf diese Weise können Sie Netzwerktopologien einrichten, die standortübergreifende Konnektivität mit Konnektivität zwischen virtuellen Netzwerken kombinieren.

Die verbundenen VNets können wie folgt angeordnet sein:

* In derselben Region oder in verschiedenen Regionen
* In demselben Abonnement oder in verschiedenen Abonnements 
* In demselben Bereitstellungsmodell oder in verschiedenen Bereitstellungsmodellen

![Beispiel für VNet-zu-VNet-Verbindung per Azure VPN Gateway](./media/design/vpngateway-vnet-to-vnet-connection-diagram.png)

### <a name="connections-between-deployment-models"></a>Verbindungen zwischen Bereitstellungsmodellen

In Azure sind zurzeit zwei Bereitstellungsmodelle verfügbar: klassisches Modell und Resource Manager-Modell. Wenn Sie Azure seit einiger Zeit verwenden, verfügen Sie wahrscheinlich über Azure-VMs und Instanzrollen, die in einem klassischen VNet ausgeführt werden. Ihre neueren VMs und Rolleninstanzen werden möglicherweise in einem in Resource Manager erstellten VNet ausgeführt. Sie können eine Verbindung zwischen den VNets erstellen, damit die Ressourcen in einem VNet direkt mit Ressourcen im anderen VNet kommunizieren können.

### <a name="vnet-peering"></a>VNet-Peering

Sofern Ihr virtuelles Netzwerk bestimmte Anforderungen erfüllt, können Sie Ihre Verbindung ggf. mithilfe von VNet-Peering erstellen. Beim VNet-Peering wird kein Gateway für das virtuelle Netzwerk verwendet. Weitere Informationen finden Sie unter [VNet-Peering](../virtual-network/virtual-network-peering-overview.md).

### <a name="deployment-models-and-methods-for-vnet-to-vnet"></a>Bereitstellungsmodelle und -methoden für VNet-zu-VNet

[!INCLUDE [vpn-gateway-table-vnet-to-vnet](../../includes/vpn-gateway-table-vnet-to-vnet-include.md)]

## <a name="expressroute-private-connection"></a><a name="ExpressRoute"></a>ExpressRoute (private Verbindung)

Mit ExpressRoute können Sie Ihre lokalen Netzwerke über eine private Verbindung, die von einem Konnektivitätsanbieter bereitgestellt wird, auf die Microsoft Cloud ausdehnen. Mit ExpressRoute können Sie Verbindungen mit Microsoft-Clouddiensten herstellen, z. B. Microsoft Azure, Office 365 und CRM Online. Die Konnektivität kann über ein Any-to-Any-Netzwerk (IP VPN), ein Point-to-Point-Ethernet-Netzwerk oder eine virtuelle Querverbindung über einen Konnektivitätsanbieter in einer Co-Location-Einrichtung bereitgestellt werden.

ExpressRoute-Verbindungen verlaufen nicht über das öffentliche Internet. Auf diese Weise können ExpressRoute-Verbindungen eine höhere Sicherheit, größere Zuverlässigkeit und schnellere Geschwindigkeit bei geringerer Latenz als herkömmliche Verbindungen über das Internet bieten.

Bei ExpressRoute-Verbindungen ist ein Gateway für virtuelle Netzwerke Teil der erforderlichen Konfiguration. Bei einer ExpressRoute-Verbindung wird das Gateway für virtuelle Netzwerke nicht mit dem Gatewaytyp „Vpn“, sondern mit „ExpressRoute“ konfiguriert. Datenverkehr, der über eine ExpressRoute-Verbindung übertragen wird, ist standardmäßig nicht verschlüsselt. Es besteht jedoch die Möglichkeit, eine Lösung zu erstellen, die es ermöglicht, verschlüsselten Datenverkehr über eine ExpressRoute-Verbindung zu senden. Weitere Informationen über ExpressRoute finden Sie unter [ExpressRoute – Technische Übersicht](../expressroute/expressroute-introduction.md).

## <a name="site-to-site-and-expressroute-coexisting-connections"></a><a name="coexisting"></a>Parallel bestehende Site-to-Site- und ExpressRoute-Verbindungen

ExpressRoute ist eine direkte, private Verbindung aus Ihrem WAN mit Microsoft Services (einschließlich Azure), die nicht über das öffentliche Internet verläuft. Site-to-Site-VPN-Datenverkehr wird verschlüsselt über das öffentliche Internet gesendet. Die Möglichkeit, Site-to-Site-VPN- und ExpressRoute-Verbindungen für dasselbe virtuelle Netzwerk zu konfigurieren, hat mehrere Vorteile.

Sie können eine Site-to-Site-VPN-Verbindung als sicheren Failoverpfad für ExpressRoute konfigurieren oder für die Verbindung mit Websites nutzen, die nicht Teil Ihres Netzwerks, aber über ExpressRoute verbunden sind. Diese Konfiguration erfordert zwei Gateways für das gleiche virtuelle Netzwerk: eins mit dem Gatewaytyp „Vpn“ und eins mit dem Gatewaytyp „ExpressRoute“.

![Beispiel für gleichzeitig bestehende ExpressRoute- und VPN Gateway-Verbindungen](./media/design/expressroute-vpngateway-coexisting-connections-diagram.png)

### <a name="deployment-models-and-methods-for-s2s-and-expressroute-coexist"></a>Koexistenz von Bereitstellungsmodellen und -methoden für S2S und ExpressRoute

[!INCLUDE [vpn-gateway-table-coexist](../../includes/vpn-gateway-table-coexist-include.md)]

## <a name="highly-available-connections"></a><a name="highly-available"></a>Hoch verfügbare Verbindungen

Informationen zum Planen und Entwerfen von hoch verfügbaren Verbindungen finden Sie unter [Hoch verfügbare Verbindungen](vpn-gateway-highlyavailable.md).

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen finden Sie unter [Häufig gestellte Fragen zum VPN-Gateway](vpn-gateway-vpn-faq.md).

* Erfahren Sie mehr über [VPN Gateway-Konfigurationseinstellungen](vpn-gateway-about-vpn-gateway-settings.md).

* Überlegungen zum Border Gateway Protocol für VPN Gateways finden Sie unter [Informationen zu BGP](vpn-gateway-bgp-overview.md).

* Sehen Sie sich die [Abonnements und Diensteinschränkungen](../azure-resource-manager/management/azure-subscription-service-limits.md#networking-limits) an.

* Erfahren Sie mehr über die anderen zentralen [Netzwerkfunktionen](../networking/networking-overview.md) von Azure.
