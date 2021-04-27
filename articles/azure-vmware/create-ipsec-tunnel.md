---
title: Erstellen eines IPSec-Tunnels zu Azure VMware Solution
description: Erfahren Sie, wie Sie einen VPN-Standort-zu-Standort-Tunnel (IPSec ikev1 and IKEv2) in Azure VMware-Lösungen einrichten.
ms.topic: how-to
ms.date: 03/23/2021
ms.openlocfilehash: 280ffdd3fec77208d5b49c8e624b7b22bca1daaf
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "105026999"
---
# <a name="create-an-ipsec-tunnel-into-azure-vmware-solution"></a>Erstellen eines IPSec-Tunnels zu Azure VMware Solution

In diesem Artikel werden die Schritte beschrieben, mit denen ein Site-to-Site-VPN-Tunnel (IPSec IKEv1 und IKEv2) eingerichtet wird, der im Microsoft Azure Virtual WAN-Hub endet. Der Azure Virtual-Hub enthält das Azure VMware Solution ExpressRoute-Gateway und das VPN-Standort-zu-Standort Gateway. Er verbindet ein lokales VPN-Gerät mit einem Azure VMware Solution-Endpunkt.

:::image type="content" source="media/create-ipsec-tunnel/vpn-s2s-tunnel-architecture.png" alt-text="Diagramm zur Site-to-Site-VPN-Tunnelarchitektur." border="false":::

In dieser Anleitung werden Sie:
- Einen Azure Virtual WAN-Hub und ein VPN-Gateways mit einer damit verbundenen öffentlichen IP-Adresse erstellen. 
- Einen Azure ExpressRoute-Gateway erstellen und einen Azure VMware Solution-Endpunkt einrichten. 
- Eine richtlinienbasierte VPN-Einrichtung vor Ort aktivieren. 

## <a name="prerequisites"></a>Voraussetzungen
Sie müssen über eine öffentliche IP-Adresse verfügen, die auf einem lokalen VPN-Gerät endet.

## <a name="step-1-create-an-azure-virtual-wan"></a>Schritt 1: Erstellen Sie einen Azure Virtual WAN

[!INCLUDE [Create a virtual WAN](../../includes/virtual-wan-create-vwan-include.md)]

## <a name="step-2-create-a-virtual-wan-hub-and-gateway"></a>Schritt 2: Einen virtuellen WAN-Hub und -Gateway erstellen

>[!TIP]
>Sie können auch ein [Gateway in einem bestehenden Hub erstellen](../virtual-wan/virtual-wan-expressroute-portal.md#existinghub).

1. Wählen Sie das virtuelle WAN aus, das Sie im vorherigen Schritt erstellt haben.

1. Wählen Sie **Virtuellen Hub erstellen** aus, geben Sie die erforderlichen Felder ein, und wählen Sie dann: **Weiter: Standort zu Standort**. 

   Geben Sie das Subnetz mit `/24` ein (Minimum).

   :::image type="content" source="media/create-ipsec-tunnel/create-virtual-hub.png" alt-text="Der Screenshot zeigt die Seite „Virtuellen Hub erstellen“.":::

4. Definieren Sie auf der Registerkarte **Standort-zu-Standort** das Site-to-Site-Gateway, indem Sie den aggregierten Durchsatz in der Dropdown-Liste **Gateway-Skalierungseinheiten** festlegen. 

   >[!TIP]
   >Die Skaleneinheiten sind zur Redundanz paarweise angeordnet und unterstützen jeweils 500 Mbit/s (eine Skaleneinheit = 500 Mbit/s). 
  
   :::image type="content" source="../../includes/media/virtual-wan-tutorial-hub-include/site-to-site.png" alt-text="Screenshot, das die Standort-zu-Standort-Details anzeigt.":::

5. Wählen Sie die **ExpressRoute** Registerkarte, erstellen Sie eine ExpressRoute-Gateway. 

   :::image type="content" source="../../includes/media/virtual-wan-tutorial-er-hub-include/hub2.png" alt-text="Screenshot der ExpressRoute-Einstellungen.":::

   >[!TIP]
   >Der Wert für eine Skalierungseinheit beträgt 2 GBit/s. 

    Die Erstellung eines Hubs dauert jeweils etwa 30 Minuten. 

## <a name="step-3-create-a-site-to-site-vpn"></a>Schritt 3: Erstellen eines Site-to-Site-VPN

1. Öffnen Sie im Azure-Portal das zuvor erstellte virtuelle WAN.

2. Wählen Sie in der **Übersicht** des virtuellen Hubs die VPN- **Konnektivität** >  **(Standort-zu-Standort)**  >  **aus. Erstellen Sie einen neuen VPN-Standort**.

   :::image type="content" source="media/create-ipsec-tunnel/create-vpn-site-basics.png" alt-text="Der Screenshot zeigt die Übersichtsseite für den virtuellen Hub, wobei „VPN (Site-to-Site)“ und „Neue VPN-Site erstellen“ ausgewählt sind.":::  
 
3. Geben Sie auf der Registerkarte **Grundeinstellungen** die erforderlichen Informationen ein. 

   :::image type="content" source="media/create-ipsec-tunnel/create-vpn-site-basics2.png" alt-text="Screenshot der Registerkarte Grundeinstellungen für die neue VPN-Website.":::  

   1. Stellen Sie den **Border Gateway Protocol** auf **aktivieren** ein.  Wenn es aktiviert ist, stellt es sicher, dass sowohl Azure VMware Solution als auch die lokalen Server ihre Routen über den Tunnel veröffentlichen. Wenn diese Option deaktiviert ist, müssen die Subnetze, die angekündigt werden sollen, manuell verwaltet werden. Wenn Subnetze fehlen, kann HCX das Service Mesh nicht bilden. Weitere Informationen finden Sie unter [About BGP with Azure VPN Gateway](../vpn-gateway/vpn-gateway-bgp-overview.md) (Informationen zu BGP mit Azure VPN Gateway).
   
   1. Geben Sie für den **privaten Adressbereich** den lokalen CIDR-Block ein. Dieser wird verwendet, um den gesamten Datenverkehr für die lokale Umgebung über den Tunnel zu leiten. Der CIDR-Block ist nur erforderlich, wenn Sie BGP nicht aktivieren.

1. Wählen Sie **Weiter: Links** aus, und füllen Sie die erforderlichen Felder aus. Durch Angeben von Verbindungs- und Anbieternamen können Sie jeweils die einzelnen Gateways unterscheiden, die irgendwann als Teil des Hubs erstellt werden. BGP und die ASN (autonome Systemnummer) müssen innerhalb Ihrer Organisation eindeutig sein.

   :::image type="content" source="media/create-ipsec-tunnel/create-vpn-site-links.png" alt-text="Screenshot das die Benutzerdetails anzeigt.":::

1. Klicken Sie auf **Überprüfen + erstellen**. 

1. Navigieren Sie zum gewünschten virtuellen Hub, und deaktivieren Sie **Hubzuordnung**, um Ihren VPN-Standort mit dem Hub zu verbinden.
 
   :::image type="content" source="../../includes/media/virtual-wan-tutorial-site-include/connect.png" alt-text="Screenshot, der den Bereich „Verbundene Sites&quot; für den Virtual HUB anzeigt, bereit für den Pre-shared Key und die zugehörigen Einstellungen.":::   

## <a name="step-4-optional-create-policy-based-vpn-site-to-site-tunnels"></a>Schritt 4. (Fakultativ) Erstellen von Richtlinien-basierten VPN-Standort-zu-Standort-Tunneln

>[!IMPORTANT]
>Dies ist ein optionaler Schritt, der nur für richtlinienbasierte VPNs gilt. 

Für richtlinienbasierte VPN-Setups müssen lokale und Azure VMware-Solution-Netzwerke angegeben werden, einschließlich der Hub-Bereiche.  Diese Hubbereiche werden verwendet, um die Verschlüsselungsdomäne für den lokalen Endpunkt des richtlinienbasierten VPN-Tunnels anzugeben.  Auf der Azure VMware Solution-Seite muss lediglich der Selektor für richtlinienbasierten Datenverkehr aktiviert werden. 

1. Navigieren Sie im Azure-Portal zum Virtual WAN-Hubstandort. Klicken Sie unter **Konnektivität** auf **VPN (Site-to-Site)** .

2. Wählen Sie den Namen Ihres VPN-Standorts aus, klicken Sie ganz rechts auf die Auslassungspunkte (...) und dann auf **VPN-Verbindung mit diesem Hub bearbeiten**.
 
   :::image type="content" source="media/create-ipsec-tunnel/edit-vpn-section-to-this-hub.png" alt-text="Der Screenshot der Seite in Azure für den Standort des Virtual WAN-Hubs zeigt Auslassungspunkte, die für den Zugriff auf die VPN-Verbindung zu diesem Hub ausgewählt wurden." lightbox="media/create-ipsec-tunnel/edit-vpn-section-to-this-hub.png":::

3. Bearbeiten Sie die Verbindung zwischen dem VPN-Standort und dem Hub, und wählen Sie dann **Speichern** aus.
   - Wählen Sie für "IPSec" (Internet Protocol Security) **Benutzerdefiniert** aus.
   - **Aktivieren** Sie den Selektor für richtlinienbasierten Datenverkehr.
   - Geben Sie die Details für **IKE-Phase 1** und **IKE-Phase 2 (IPSec)** an. 
 
   :::image type="content" source="media/create-ipsec-tunnel/edit-vpn-connection.png" alt-text="Der Screenshot der Seite „VPN-Verbindung bearbeiten“."::: 
 
   Die Datenverkehrsselektoren oder Subnetze, die Teil der richtlinienbasierten Verschlüsselungsdomäne sind, müssen wie folgt angegeben werden:
    
   - Virtueller WAN-Hub`/24`
   - Private Azure VMware Solution-Cloud`/22`
   - Verbundenes virtuelles Azure-Netzwerk (sofern vorhanden)

## <a name="step-5-connect-your-vpn-site-to-the-hub"></a>Schritt 5: Herstellen einer Verbindung zwischen VPN-Standort und Hub

1. Wählen Sie den Namen Ihres VPN-Standorts aus, und klicken Sie dann auf **VPN-Standorte verbinden**. 

1. Geben Sie im Feld **Vorinstallierter Schlüssel** den Schlüssel ein, der zuvor für den lokalen Endpunkt definiert wurde. 

   >[!TIP]
   >Wenn Sie über keinen zuvor definierten Schlüssel verfügen, können Sie dieses Feld leer lassen. Es wird automatisch ein Schlüssel für Sie generiert. 

   :::image type="content" source="../../includes/media/virtual-wan-tutorial-connect-vpn-site-include/connect.png" alt-text="Screenshot, der den Bereich &quot;Verbundene Sites&quot; für Virtual HUB zeigt, bereit für einen Pre-Shared Key und zugehörigen Einstellungen. "::: 

1. Wenn Sie eine Firewall im Hub einsetzen und dieser der nächste Hop ist, setzen Sie die Option **Propagate Default Route** auf **Enable**. 

   Wenn diese Option aktiviert ist, propagiert der Virtual WAN Hub eine Verbindung nur dann, wenn der Hub die Standardroute bereits bei der Einrichtung einer Firewall im Hub gelernt hat oder wenn eine andere angeschlossene Site das Forced Tunneling aktiviert hat. Der Ursprung der Standardroute liegt nicht im Virtual WAN-Hub.  

1. Wählen Sie **Verbinden**. Nach ein paar Minuten zeigt die Seite den Verbindungs- und Konnektivitätsstatus an.

   :::image type="content" source="../../includes/media/virtual-wan-tutorial-connect-vpn-site-include/status.png" alt-text="Screenshot, dass die Standort-zu-Standort-Verbindung und Konnektivitätsstatus anzeigt." lightbox="../../includes/media/virtual-wan-tutorial-connect-vpn-site-include/status.png":::

1. [Laden Sie die VPN-Konfigurationsdatei](../virtual-wan/virtual-wan-site-to-site-portal.md#device) für den lokalen Endpunkt herunter.  

3. Fügen Sie das Azure VMware Solution-ExpressRoute-Gateway in den Virtual WAN-Hub ein. 

   >[!IMPORTANT]
   >Vor dem Patchen der Plattform muss zunächst ein Private Cloud erstellt werden. 

   [!INCLUDE [request-authorization-key](includes/request-authorization-key.md)]

4. Verbinden Sie Azure VMware Solution und das VPN-Gateway im Virtual WAN-Hub. Sie verwenden den Autorisierungs-Schlüssel und die expressroute-ID (Peer Circuit URI) aus dem vorherigen Schritt.

   1. Wählen Sie das expressroute-Gateway, und wählen Sie dann **Autorisierungsschlüssel einlösen** aus.

      :::image type="content" source="media/create-ipsec-tunnel/redeem-authorization-key.png" alt-text="Der Screenshot der ExpressRoute-Seite für die private Cloud mit ausgewählter Option „Autorisierungsschlüssel einlösen“.":::

   1. Fügen Sie den Autorisierungsschlüssel in das Feld **Autorisierungsschlüssel** ein.
   1. Fügen Sie die ExpressRoute-ID in das Feld **Peer Circuit URI** ein. 
   1. Wählen Sie den Checkbox **Diesen ExpressRoute-Kreis automatisch dem Hub zuordnen** aus. 
   1. Wählen Sie **Hinzufügen** aus, um die Verbindung zu erstellen. 

5. Testen Sie Ihre Verbindung, indem Sie [ein NSX-T-Segment erstellen](./tutorial-nsx-t-network-segment.md) und eine VM im Netzwerk bereitstellen. Pingen Sie den lokalen Endpunkt und den Azure VMware Solution-Endpunkt.
