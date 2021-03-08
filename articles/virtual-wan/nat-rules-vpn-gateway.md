---
title: Konfigurieren von VPN-NAT-Regeln für Ihr Gateway
titleSuffix: Azure Virtual WAN
description: Informationen zum Konfigurieren von NAT-Regeln für Ihr VWAN-VPN-Gateway
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: how-to
ms.date: 02/17/2021
ms.author: cherylmc
ms.openlocfilehash: a31b3718eb1baa32aef39474383924efe8cf93b6
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/03/2021
ms.locfileid: "101743482"
---
# <a name="configure-nat-rules-for-your-virtual-wan-vpn-gateway---preview"></a>Konfigurieren von NAT-Regeln für Ihr Virtual WAN-VPN-Gateway (Vorschau)

> [!IMPORTANT]
> NAT-Regeln sind zurzeit als öffentliche Vorschau verfügbar.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar.
> Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Sie können Ihr Virtual WAN-VPN-Gateway mit statischen 1:1-NAT-Regeln konfigurieren. Eine NAT-Regel bietet einen Mechanismus zum Einrichten einer 1:1-Übersetzung von IP-Adressen. NAT kann verwendet werden, um zwei IP-Netzwerke mit nicht kompatiblen oder überlappenden IP-Adressen miteinander zu verbinden. Hierbei sind Branches, die überlappende IP-Adressen besitzen und auf Azure-VNET-Ressourcen zugreifen möchten, ein typisches Szenario.

Diese Konfiguration verwendet eine Flusstabelle zum Weiterleiten von Datenverkehr von einer externen IP-Adresse (Host-IP) an eine interne IP-Adresse, die einem Endpunkt in einem virtuellen Netzwerk (virtueller Computer, Computer, Container usw.) zugeordnet ist.

   :::image type="content" source="./media/nat-rules-vpn-gateway/diagram.png" alt-text="Diagramm mit der Architektur":::

## <a name="configure-and-view-rules"></a><a name="view"></a>Konfigurieren und Anzeigen von Regeln

Sie können NAT-Regeln für Ihre VPN-Gatewayeinstellungen jederzeit konfigurieren und anzeigen.

   :::image type="content" source="./media/nat-rules-vpn-gateway/edit-rules.png" alt-text="Screenshot mit den Regeln für die Bearbeitung":::

1. Navigieren Sie zu Ihrem virtuellen Hub.
1. Wählen Sie **VPN (Site-to-Site)** aus.
1. Wählen Sie **NAT-Regeln (Bearbeiten)** aus.
1. Sie können auf der Seite **Edit NAT Rule** (NAT-Regel bearbeiten) eine NAT-Regel mit den folgenden Werten **hinzufügen/bearbeiten/löschen**:

   * **Name:** Ein eindeutiger Name für Ihre NAT-Regel
   * **Type:** (Typ) Statisch. Eine statische 1:1-NAT-Verbindung stellt eine 1:1-Beziehung zwischen einer internen Adresse und einer externen Adresse her.
   * **Mode:** (Modus) IngressSnat oder EgressSnat.  
      * Der IngressSnat-Modus (auch als eingehende Quellen-NAT bezeichnet) gilt für Datenverkehr, der in das Site-to-Site-VPN-Gateway des Azure-Hubs wechselt.
      * Der EgressSnat-Modus (auch als ausgehende Quellen-NAT bezeichnet) gilt für Datenverkehr, der das Site-to-Site-VPN-Gateway des Azure-Hubs verlässt.
   * **InternalMapping:** (Interne Zuordnung) Ein Adresspräfixbereich der Quell-IPs im internen Netzwerk, dem einer Gruppe externer IPs zugeordnet wird, also in anderen Worten, Ihrem Präfixbereich vor der NAT-Adresse.
   * **ExternalMapping:** (Externe Zuordnung) Ein Adresspräfixbereich der Ziel-IPs im externen Netzwerk, dem Quell-IPs zugeordnet werden, also in anderen Worten, Ihrem Präfixbereich nach der NAT-Adresse.
   * **Link Connection:** (Linkverbindung) Die Verbindungsressource, die einen VPN-Standort virtuell mit dem Site-to-Site-VPN-Gateway des Azure-Hubs verknüpft.

### <a name="configuration-considerations"></a>Überlegungen zur Konfiguration

* Die Subnetzgröße für die interne und die externe Zuordnung muss für die statische 1:1-NAT identisch sein.
* Passen Sie unbedingt den VPN-Standort im Azure-Portal an, um **ExternalMapping**-Präfixe im Feld „Privater Adressraum“ hinzuzufügen. Derzeit müssen Standorte, für die BGP aktiviert ist, sicherstellen, dass der lokale BGP-Ankündiger (BGP-Geräteeinstellungen) einen Eintrag für die externen Zuordnungspräfixe enthält.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Site-to-Site-Konfigurationen finden Sie unter [Konfigurieren einer Site-to-Site-Verbindung für ein Virtual WAN](virtual-wan-site-to-site-portal.md).
