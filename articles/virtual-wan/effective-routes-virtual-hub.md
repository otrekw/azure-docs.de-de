---
title: 'Anzeigen der effektiven Routen für einen virtuellen Hub: Azure Virtual WAN | Microsoft-Dokumentation'
description: Anzeigen der effektiven Routen für einen virtuellen Hub in Azure Virtual WAN
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: how-to
ms.date: 09/22/2020
ms.author: cherylmc
ms.openlocfilehash: ad487aa7492abcf02988c78ccfa2ba7cd4798249
ms.sourcegitcommit: bdd5c76457b0f0504f4f679a316b959dcfabf1ef
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90983656"
---
# <a name="view-virtual-hub-effective-routes"></a>Anzeigen effektiver Routen eines virtuellen Hubs

Sie können alle Routen Ihres Virtual WAN-Hubs im Azure-Portal anzeigen. In diesem Artikel erfahren Sie die Schritte zum Anzeigen effektiver Routen. Weitere Informationen zum Routing für virtuelle Hubs finden Sie unter [Informationen zum Routing virtueller Hubs](about-virtual-hub-routing.md).

## <a name="select-connections-or-route-tables"></a><a name="routing"></a>Auswählen von Verbindungen oder Routingtabellen

1. Navigieren Sie zu Ihrem virtuellen Hub, und wählen Sie **Routing**aus. Wählen Sie auf der Seite „Routing“ die Option **Effektive Routen** aus.
1. In der Dropdownliste können Sie **Routingtabelle** auswählen. Wenn die Option für Routingtabellen nicht angezeigt wird, ist in diesem virtuellen Hub keine benutzerdefinierte oder Standardroutingtabelle eingerichtet.

## <a name="view-output"></a><a name="output"></a>Anzeigen der Ausgabe

In der Seitenausgabe sind die folgenden Felder aufgeführt:

* **Präfix:** Adresspräfix, das der aktuellen Entität bekannt ist (vom virtuellen Hubrouter erlernt)
* **Typ des nächsten Hops:** Kann Virtual Network-Verbindung, VPN_S2S_Gateway, ExpressRouteGateway, Remotehub oder Azure Firewall sein.
* **Nächster Hop:** Dies ist der Link zur Ressourcen-ID des nächsten Hops oder zeigt einfach den On-Link an, der den aktuellen Hub beinhaltet.
* **Ursprung:** Die Ressourcen-ID der Routingquelle.
* **AS-Pfad:** Für das BGP-Attribut „AS-Pfad“ (Autonomes System) werden alle AS-Nummern aufgeführt, die durchlaufen werden müssen, um den Standort zu erreichen, an dem das Präfix für den Pfad angekündigt wird.

### <a name="example"></a><a name="example"></a>Beispiel

Die Werte in der folgenden Beispieltabelle implizieren, dass die Verbindung mit dem virtuellen Hub oder der Routingtabelle die Route für 10.2.0.0/24 (ein Branchpräfix) gelernt hat. Die Route wurde gelernt, da **Typ des nächsten Hops** ein VPN_S2S_Gateway und **Nächster Hop** eine VPN-Gatewayressourcen-ID ist. **Routenursprung** verweist auf die Ressourcen-ID des Ursprungs-VPN-Gateways bzw. der -routingtabelle oder der -verbindung. **AS-Pfad** gibt den AS-Pfad für den Branch an.

Verwenden Sie die Scrollleiste am unteren Rand der Tabelle, um den „AS-Pfad“ anzuzeigen.

| **Präfix** |  **Typ des nächsten Hops** | **Nächster Hop** |  **Routenursprung** |**AS-Pfad** |
| ---        | ---                | ---          | ---               | ---         |
| 10.2.0.0/24| VPN_S2S_Gateway |/subscriptions/`<sub id>`/resourceGroups/`<resource group name>`/providers/Microsoft.Network/vpnGateways/vpngw|/subscriptions/`<sub id>`/resourceGroups/`<resource group name>`/providers/Microsoft.Network/vpnGateways/vpngw| 20000|

**Überlegungen:**

* Wenn in der Ausgabe zum **Abrufen effektiver Routen** 0.0.0.0/0 angegeben ist, bedeutet dies, dass die Route in einer der Routingtabellen vorhanden ist. Wenn diese Route jedoch für das Internet eingerichtet wurde, ist für die Verbindung das zusätzliche Flag **"enableInternetSecurity": true** erforderlich. Die effektive Route wird nicht auf der VM-NIC angezeigt, wenn das Flag „enableInternetSecurity“ der Verbindung FALSE ist.

* Das Feld **Standardroute weitergeben** wird im Azure Virtual WAN-Portal angezeigt, wenn Sie eine Verbindung mit einem virtuellen Netzwerk, eine VPN-Verbindung oder eine ExpressRoute-Verbindung bearbeiten. Dieses Feld gibt das **enableInternetSecurity**-Flag an, das für ExpressRoute- und VPN-Verbindungen immer standardmäßig FALSE ist, für Verbindungen mit virtuellen Netzwerken jedoch TRUE.

* Wenn beim Anzeigen effektiver Routen auf einer VM-NIC der nächste Hop als „Gateway für virtuelle Netzwerke“ angezeigt wird, beinhaltet dies den virtuellen Hubrouter, wenn sich der virtuelle Computer in einer Spoke befindet, die mit einem virtuellen WAN-Hub verbunden ist.

* Die Anzeige effektiver Routen für eine Routingtabelle eines virtuellen Hubs wird nur aufgefüllt, wenn der virtuelle Hub mindestens einen Verbindungstyp (VPN/ER/VNET) aufweist.

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zu Virtual WAN finden Sie auf der Seite mit der [Übersicht über Virtual WAN](virtual-wan-about.md).
* Weitere Informationen zum Routing für virtuelle Hubs finden Sie unter [Informationen zum Routing virtueller Hubs](about-virtual-hub-routing.md).
