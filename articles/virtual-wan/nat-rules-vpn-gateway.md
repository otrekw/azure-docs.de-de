---
title: Konfigurieren von VPN-NAT-Regeln für Ihr Gateway
titleSuffix: Azure Virtual WAN
description: Informationen zum Konfigurieren von NAT-Regeln für Ihr VWAN-VPN-Gateway
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: how-to
ms.date: 03/05/2021
ms.author: cherylmc
ms.openlocfilehash: 6fbee31f015953bd7e65648ea273e3ca84686115
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "102431215"
---
# <a name="configure-nat-rules-for-your-virtual-wan-vpn-gateway---preview"></a>Konfigurieren von NAT-Regeln für Ihr Virtual WAN-VPN-Gateway (Vorschau)

> [!IMPORTANT]
> NAT-Regeln sind zurzeit als öffentliche Vorschau verfügbar.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar.
> Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Sie können Ihr Virtual WAN-VPN-Gateway mit statischen 1:1-NAT-Regeln konfigurieren. Eine NAT-Regel bietet einen Mechanismus zum Einrichten einer 1:1-Übersetzung von IP-Adressen. NAT kann verwendet werden, um zwei IP-Netzwerke mit nicht kompatiblen oder überlappenden IP-Adressen miteinander zu verbinden. Hierbei sind Branches, die überlappende IP-Adressen besitzen und auf Azure-VNET-Ressourcen zugreifen möchten, ein typisches Szenario.

Diese Konfiguration verwendet eine Flusstabelle zum Weiterleiten von Datenverkehr von einer externen IP-Adresse (Host-IP) an eine interne IP-Adresse, die einem Endpunkt in einem virtuellen Netzwerk (virtueller Computer, Computer, Container usw.) zugeordnet ist.

   :::image type="content" source="./media/nat-rules-vpn-gateway/diagram.png" alt-text="Diagramm mit der Architektur":::

## <a name="configure-nat-rules"></a><a name="rules"></a>Konfigurieren von NAT-Regeln

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

### <a name="configuration-considerations"></a><a name="considerations"></a>Überlegungen zur Konfiguration

* Die Subnetzgröße für die interne und die externe Zuordnung muss für die statische 1:1-NAT identisch sein.
* Passen Sie unbedingt den VPN-Standort im Azure-Portal an, um **ExternalMapping**-Präfixe im Feld „Privater Adressraum“ hinzuzufügen. Derzeit müssen Standorte, für die BGP aktiviert ist, sicherstellen, dass der lokale BGP-Ankündiger (BGP-Geräteeinstellungen) einen Eintrag für die externen Zuordnungspräfixe enthält.

## <a name="examples-and-verification"></a><a name="examples"></a>Beispiele und Überprüfung

### <a name="ingress-mode-nat"></a>Eingangsmodus-NAT

Die Regeln der Eingangsmodus-NAT werden auf Pakete angewendet, die über das Site-to-Site-VPN-Gateway von Virtual WAN in Azure eingehen. In diesem Szenario möchten Sie zwei Site-to-Site-VPN-Branches mit Azure verbinden. VPN-Standort 1 stellt eine Verbindung über Link 1 her, und VPN-Standort 2 stellt eine Verbindung über Link 2 her. Jeder Standort verfügt über den Adressraum 192.169.1.0/24.

Das folgende Diagramm zeigt das projizierte Endergebnis:

:::image type="content" source="./media/nat-rules-vpn-gateway/ingress.png" alt-text="Diagramm, das die Eingangsmodus-NAT anzeigt":::

1. Geben Sie eine NAT-Regel an.

   Geben Sie eine NAT-Regel an, um sicherzustellen, dass das Site-to-Site-VPN-Gateway in der Lage ist, zwischen den beiden Branches mit überlappenden Adressräumen (z. B. 192.168.1.0/24) zu unterscheiden. In diesem Beispiel konzentrieren wir uns auf Link 1 für VPN-Standort 1.

   Die folgende NAT-Regel kann eingerichtet und mit Link 1 eines der Branches verknüpft werden. Da es sich um eine statische NAT-Regel handelt, enthalten die Adressräume von „InternalMapping“ und „ExternalMapping“ die gleiche Anzahl von IP-Adressen.

   * **Name:** IngressRule01
   * **Typ:** Statisch
   * **Modus:** IngressSnat
   * **InternalMapping:** 192.168.1.0/24
   * **ExternalMapping:** 10.1.1.0/24
   * **Linkverbindung:** Link 1

1. Kündigen Sie das richtige „ExternalMapping“ an.

   Stellen Sie in diesem Schritt sicher, dass Ihr Site-to-Site-VPN-Gateway dem Rest Ihrer Azure-Ressourcen den richtigen ExternalMapping-Adressraum ankündigt. Je nachdem, ob BGP aktiviert ist, gibt es unterschiedliche Anweisungen.

   **Beispiel 1: BGP ist aktiviert**

   * Stellen Sie sicher, dass der lokale BGP-Sprecher, der sich am VPN-Standort 1 befindet, so konfiguriert ist, dass er den Adressraum 10.1.1.0/24 angekündigt.
   * Während dieser Vorschau müssen Standorte, für die BGP aktiviert ist, sicherstellen, dass die lokale BGP-Ankündigung (BGP-Geräteeinstellungen) einen Eintrag für die externen Zuordnungspräfixe enthält.

   **Beispiel 2: BGP ist nicht aktiviert.**

   * Navigieren Sie zu der Ressource „Virtueller Hub“, die das Site-to-Site-VPN-Gateway enthält. Wählen Sie auf der Seite des virtuellen Hubs unter **Konnektivität** die Option **VPN (Site-to-Site)** aus.
   * Wählen Sie den VPN-Standort aus, der über Link 1 mit dem Virtual WAN-Hub verbunden ist. Wählen Sie **Standort bearbeiten** aus, und geben Sie 10.1.1.0/24 als privaten Adressraum für den VPN-Standort ein.

     :::image type="content" source="./media/nat-rules-vpn-gateway/edit-site.png" alt-text="Screenshot der Seite „VPN-Standort bearbeiten“":::

### <a name="packet-flow"></a>Paketfluss

In diesem Beispiel möchte ein lokales Gerät ein virtuelles Spoke-Netzwerk erreichen. Der Paketfluss sieht wie folgt aus, wobei die NAT-Übersetzungen in Fettformatierung angezeigt sind.

1. Der Datenverkehr vom lokalen Standort wird initiiert.
   * Quell-IP-Adresse: **192.168.1.1**
   * Ziel-IP-Adresse: 30.0.0.1
1. Der Datenverkehr gelangt in das Site-to-Site-Gateway und wird mithilfe der NAT-Regel übersetzt und dann an das Spoke-Netzwerk gesendet.
   * Quell-IP-Adresse: **10.1.1.1**
   * Ziel-IP-Adresse: 30.0.0.1
1. Die Antwort des Spoke-Netzwerks wird initiiert.
   * Quell-IP-Adresse: 30.0.0.1
   * Ziel-IP-Adresse: **10.1.1.1**
1. Der Datenverkehr gelangt in das Site-to-Site-VPN-Gateway, die Übersetzung wird umgekehrt und an den lokalen Standort gesendet.
   * Quell-IP-Adresse: 30.0.0.1
   * Ziel-IP-Adresse: **192.168.1.1**

### <a name="verification-checks"></a>Überprüfungen

In diesem Abschnitt werden Überprüfungen gezeigt, mit denen Sie sicherstellen können, dass Ihre Konfiguration ordnungsgemäß eingerichtet ist.

#### <a name="validate-defaultroutetable-rules-and-routes"></a>Überprüfen von DefaultRouteTable, Regeln und Routen

Branches in Virtual WAN werden **DefaultRouteTable** zugeordnet, was bedeutet, dass alle Branchverbindungen Informationen über Routen erhalten, die in „DefaultRouteTable“ ausgefüllt sind. Die NAT-Regel wird mit dem externen Zuordnungspräfix in den effektiven Routen von „DefaultRouteTable“ angezeigt.

Beispiel:

* **Präfix:** 10.1.1.0/24  
* **Typ des nächsten Hops:** VPN_S2S_Gateway
* **Nächster Hop:** VPN_S2S_Gateway Resource

#### <a name="validate-address-prefixes"></a>Überprüfen von Adresspräfixen

Dieses Beispiel gilt für Ressourcen in virtuellen Netzwerken, die „DefaultRouteTable“ zugeordnet sind.

Die **Effektiven Routen** auf den Netzwerkschnittstellenkarten (NIC) eines beliebigen virtuellen Computers, der sich in einem mit dem virtuellen WAN-Hub verbundenen virtuellen Spoke-Netzwerk befindet, sollten auch die Adresspräfixe der NAT-Regeln **ExternalMapping** enthalten.

#### <a name="validate-bgp-advertisements"></a>Überprüfen von BGP-Ankündigungen

Wenn Sie BGP für die VPN-Standortverbindung konfiguriert haben, überprüfen Sie den lokalen BGP-Sprecher, um sicherzustellen, dass er einen Eintrag für die externen Zuordnungspräfixe ankündigt.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Site-to-Site-Konfigurationen finden Sie unter [Konfigurieren einer Site-to-Site-Verbindung für ein Virtual WAN](virtual-wan-site-to-site-portal.md).
