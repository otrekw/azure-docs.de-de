---
title: 'Anzeigen der effektiven Routen für einen virtuellen Hub: Azure Virtual WAN | Microsoft-Dokumentation'
description: Anzeigen der effektiven Routen für einen virtuellen Hub in Azure Virtual WAN
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: how-to
ms.date: 06/29/2020
ms.author: cherylmc
ms.openlocfilehash: 20cdc55b474034480392f9dfb05b20ad25df6939
ms.sourcegitcommit: e132633b9c3a53b3ead101ea2711570e60d67b83
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/07/2020
ms.locfileid: "86037765"
---
# <a name="view-virtual-hub-effective-routes"></a>Anzeigen effektiver Routen eines virtuellen Hubs

Sie können alle Routen Ihres Virtual WAN-Hubs im Azure-Portal anzeigen. In diesem Artikel erfahren Sie die Schritte zum Anzeigen effektiver Routen. Weitere Informationen zum Routing für virtuelle Hubs finden Sie unter [Informationen zum Routing virtueller Hubs](about-virtual-hub-routing.md).

> [!NOTE]
> Im Azure-Portal werden einige dieser Features möglicherweise noch verteilt und sind bis zur Woche um den 3. August nicht verfügbar. 
>

## <a name="select-connections-or-route-tables"></a><a name="routing"></a>Auswählen von Verbindungen oder Routingtabellen

1. Navigieren Sie zu Ihrem virtuellen Hub, und wählen Sie **Routing**aus. Wählen Sie auf der Seite „Routing“ die Option **Effektive Routen** aus.
1. In der Dropdownliste können Sie **Verbindungstyp** oder eine **Routingtabelle** auswählen. Wenn die Option für Routingtabellen nicht angezeigt wird, ist in diesem virtuellen Hub keine benutzerdefinierte oder Standardroutingtabelle eingerichtet.
1. In der Dropdownliste für **Verbindungen/Routingtabellen** können Sie zwischen den folgenden Elementen auswählen:

   * VNet-Verbindung
   * VPN-Standortverbindung
   * ExpressRoute-Verbindung
   * Point-to-Site-Verbindung
   * Routingtabelle

   :::image type="content" source="./media/effective-routes-virtual-hub/routing.png" alt-text="Routing":::

## <a name="view-output"></a><a name="output"></a>Anzeigen der Ausgabe

In der Seitenausgabe sind die folgenden Felder aufgeführt:

* **Präfix:** Adresspräfix, das für die aktuelle Entität bekannt ist.
* **Typ des nächsten Hops:** Kann Virtual Network-Verbindung, VPN_S2S_Gateway, ExpressRouteGateway, Remotehub oder Azure Firewall sein.
* **Nächster Hop:** Dies ist die IP-Adresse oder einfach ein Link, der den aktuellen Hub impliziert.
* **Ursprung:** Die Ressourcen-ID der Routingquelle.
* **AS-Pfad:** Für das BGP-Attribut „AS-Pfad“ (Autonomes System) werden alle AS-Nummern aufgeführt, die durchlaufen werden müssen, um den Standort zu erreichen, an dem das Präfix für den Pfad angekündigt wird.

### <a name="example"></a><a name="example"></a>Beispiel

Die Werte in der folgenden Beispieltabelle implizieren, dass die Verbindung mit dem virtuellen Hub oder der Routingtabelle die Route für 10.2.0.0/24 (ein Branchpräfix) gelernt hat. Die Route wurde gelernt, da **Typ des nächsten Hops** ein VPN_S2S_Gateway und **Nächster Hop** eine VPN-Gatewayressourcen-ID ist. **Routenursprung** verweist auf die Ressourcen-ID des Ursprungs-VPN-Gateways bzw. der -routingtabelle oder der -verbindung. **AS-Pfad** gibt den AS-Pfad für den Branch an.

Verwenden Sie die Scrollleiste am unteren Rand der Tabelle, um den „AS-Pfad“ anzuzeigen.

| **Präfix** |  **Typ des nächsten Hops** | **Nächster Hop** |  **Routenursprung** |**AS-Pfad** |
| ---        | ---                | ---          | ---               | ---         |
| 10.2.0.0/24| VPN_S2S_Gateway |10.1.0.6, 10.1.0.7|/subscriptions/`<sub id>`/resourceGroups/`<resource group name>`/providers/Microsoft.Network/vpnGateways/vpngw| 20000|

**Überlegungen:**

* Wenn in der Ausgabe zum **Abrufen effektiver Routen** 0.0.0.0/0 angegeben ist, bedeutet dies, dass die Route in einer der Routingtabellen vorhanden ist. Wenn diese Route jedoch für das Internet eingerichtet wurde, ist für die Verbindung das zusätzliche Flag **"enableInternetSecurity": true** erforderlich. Die effektive Route wird nicht auf der VM-NIC angezeigt, wenn das Flag „enableInternetSecurity“ der Verbindung FALSE ist.

* Das Feld **Standardroute weitergeben** wird im Azure Virtual WAN-Portal angezeigt, wenn Sie eine Verbindung mit einem virtuellen Netzwerk, eine VPN-Verbindung oder eine ExpressRoute-Verbindung bearbeiten. Dieses Feld gibt das **enableInternetSecurity**-Flag an, das für ExpressRoute- und VPN-Verbindungen immer standardmäßig FALSE ist, für Verbindungen mit virtuellen Netzwerken jedoch TRUE.

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zu Virtual WAN finden Sie auf der Seite mit der [Übersicht über Virtual WAN](virtual-wan-about.md).
* Weitere Informationen zum Routing für virtuelle Hubs finden Sie unter [Informationen zum Routing virtueller Hubs](about-virtual-hub-routing.md).
