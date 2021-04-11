---
title: Erstellen eines IPSec-Tunnels zu Azure VMware Solution
description: Erfahren Sie, wie Sie einen virtuellen WAN-Hub zum Einrichten eines IPSec-Tunnels zu Azure VMware-Lösungen erstellen.
ms.topic: how-to
ms.date: 10/02/2020
ms.openlocfilehash: 21df674862b65ef6573a8a3fcfd7538b1053f04e
ms.sourcegitcommit: 4bda786435578ec7d6d94c72ca8642ce47ac628a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/16/2021
ms.locfileid: "103491839"
---
# <a name="create-an-ipsec-tunnel-into-azure-vmware-solution"></a>Erstellen eines IPSec-Tunnels zu Azure VMware Solution

In diesem Artikel werden die Schritte beschrieben, mit denen ein Site-to-Site-VPN-Tunnel (IPSec IKEv1 und IKEv2) eingerichtet wird, der im Microsoft Azure Virtual WAN-Hub endet. Wir erstellen einen Azure Virtual WAN-Hub und ein VPN-Gateway, dem eine öffentliche IP-Adresse zugeordnet ist. Anschließend erstellen wir ein Azure ExpressRoute-Gateway und richten einen Azure VMware Solution-Endpunkt ein. Außerdem wird erläutert, wie Sie eine richtlinienbasierte lokale VPN-Einrichtung ermöglichen. 

## <a name="topology"></a>Topologie

![Diagramm zur Site-to-Site-VPN-Tunnelarchitektur.](media/create-ipsec-tunnel/vpn-s2s-tunnel-architecture.png)

Der Azure Virtual-Hub enthält das Azure VMware Solution ExpressRoute-Gateway und das Site-to-Site-VPN-Gateway. Er verbindet ein lokales VPN-Gerät mit einem Azure VMware Solution-Endpunkt.

## <a name="before-you-begin"></a>Vorbereitung

Um den Site-to-Site-VPN-Tunnel zu erstellen, müssen Sie eine öffentliche IP-Adresse erstellen, die in einem lokalen VPN-Gerät endet.

## <a name="create-a-virtual-wan-hub"></a>Erstellen eines Virtual WAN-Hubs

1. Suchen Sie im Azure-Portal nach **Virtual WAN**. Wählen Sie **+ Hinzufügen** aus. Die Seite "WAN erstellen" wird geöffnet.  

2. Geben Sie die erforderlichen Felder auf der Seite **WAN erstellen** ein, und wählen Sie dann **Überprüfen + Erstellen** aus.
   
   | Feld | Wert |
   | --- | --- |
   | **Abonnement** | Dieses Feld ist bereits mit dem Abonnement ausgefüllt, das zur Ressourcengruppe gehört. |
   | **Ressourcengruppe** | Das virtuelle WAN ist eine globale Ressource und nicht auf eine bestimmte Region beschränkt.  |
   | **Ressourcengruppenstandort** | Um den Virtual WAN-Hub zu erstellen, müssen Sie einen Standort für die Ressourcengruppe festlegen.  |
   | **Name** |   |
   | **Typ** | Wählen Sie **Standard** aus, um mehr als nur den VPN-Gatewaydatenverkehr zuzulassen.  |

   :::image type="content" source="media/create-ipsec-tunnel/create-wan.png" alt-text="Der Screenshot zeigt die Seite „WAN erstellen“ im Azure-Portal.":::

3. Wählen Sie im Azure-Portal das virtuelle WAN aus, das Sie im vorherigen Schritt erstellt haben. Wählen Sie **Virtuellen Hub erstellen** aus, geben Sie die erforderlichen Felder ein, und wählen Sie dann **Weiter: Standort-zu-Standort** aus. 

   | Feld | Wert |
   | --- | --- |
   | **Region** | Aus Verwaltungsperspektive ist die Auswahl einer Region erforderlich.  |
   | **Name** |    |
   | **Privater Adressraum des Hubs** | Geben Sie das Subnetz mit `/24` ein (Minimum).  |

   :::image type="content" source="media/create-ipsec-tunnel/create-virtual-hub.png" alt-text="Der Screenshot zeigt die Seite „Virtuellen Hub erstellen“.":::

4. Definieren Sie auf der Registerkarte **Standort-zu-Standort** das Site-to-Site-Gateway, indem Sie den aggregierten Durchsatz in der Dropdown-Liste **Gateway-Skalierungseinheiten** festlegen. 

   >[!TIP]
   >Eine Skalierungseinheit = 500 MBit/s. Die Skalierungseinheiten werden aus Redundanzgründen in Paaren angegeben, die jeweils 500 MBit/s unterstützen.
  
5. Erstellen Sie auf der Registerkarte **ExpressRoute** ein ExpressRoute-Gateway. 

   >[!TIP]
   >Der Wert für eine Skalierungseinheit beträgt 2 GBit/s. 

    Die Erstellung eines Hubs dauert jeweils etwa 30 Minuten. 

## <a name="create-a-vpn-site"></a>Erstellen eines VPN-Standorts 

1. Wählen Sie im Azure-Portal in **Kürzlich verwendete Ressourcen** das virtuelle WAN aus, das Sie im vorherigen Abschnitt erstellt haben.

2. Wählen Sie in der **Übersicht** für den virtuellen Hub **Konnektivität** > **VPN (Standort-zu-Standort)** und dann **Neuen VPN-Standort erstellen** aus.

   :::image type="content" source="media/create-ipsec-tunnel/create-vpn-site-basics.png" alt-text="Der Screenshot zeigt die Übersichtsseite für den virtuellen Hub, wobei „VPN (Site-to-Site)“ und „Neue VPN-Site erstellen“ ausgewählt sind.":::  
 
3. Geben Sie auf der Registerkarte **Grundlagen** die erforderlichen Felder ein, und wählen Sie dann **Weiter: Links** aus. 

   | Feld | Wert |
   | --- | --- |
   | **Region** | Dieselbe Region, die Sie im vorherigen Abschnitt angegeben haben.  |
   | **Name** |  |
   | **Gerätehersteller** |  |
   | **Border Gateway Protocol** | Legen Sie diese Option auf **Aktivieren** fest, damit sowohl Azure VMware Solution als auch die lokalen Server ihre Routen durch den Tunnel ankündigen. Wenn diese Option deaktiviert ist, müssen die Subnetze, die angekündigt werden sollen, manuell verwaltet werden. Wenn Subnetze fehlen, kann HCX das Service Mesh nicht bilden. Weitere Informationen finden Sie unter [About BGP with Azure VPN Gateway](../vpn-gateway/vpn-gateway-bgp-overview.md) (Informationen zu BGP mit Azure VPN Gateway). |
   | **Privater Adressraum**  | Geben Sie den lokalen CIDR-Block ein.  Dieser wird verwendet, um den gesamten Datenverkehr für die lokale Umgebung über den Tunnel zu leiten.  Der CIDR-Block ist nur erforderlich, wenn Sie BGP nicht aktivieren. |
   | **Verbinden mit** |   |

4. Füllen Sie auf der Registerkarte **Links** die erforderlichen Felder aus, und wählen Sie **Überprüfen + Erstellen** aus. Durch Angeben von Verbindungs- und Anbieternamen können Sie jeweils die einzelnen Gateways unterscheiden, die irgendwann als Teil des Hubs erstellt werden. BGP und die ASN (autonome Systemnummer) müssen innerhalb Ihrer Organisation eindeutig sein.
 
## <a name="optional-defining-a-vpn-site-for-policy-based-vpn-site-to-site-tunnels"></a>(Optional) Definieren eines VPN-Standorts für richtlinienbasierte Site-to-Site-Tunnel

Dieser Abschnitt gilt nur für richtlinienbasierte VPNs. Die Einrichtung von richtlinienbasierten (oder statischen, routenbasierten) VPNs wird in den meisten Fällen durch lokale VPN-Gerätefunktionen gesteuert. Dazu müssen lokale und Azure VMware Solution-Netzwerke angegeben werden. Für Azure VMware Solution mit einem Azure Virtual WAN-Hub können Sie *nicht jedes* Netzwerk auswählen. Sie müssen stattdessen alle relevanten lokalen und Azure VMware Solution Virtual WAN-Hubbereiche angeben. Diese Hubbereiche werden verwendet, um die Verschlüsselungsdomäne für den lokalen Endpunkt des richtlinienbasierten VPN-Tunnels anzugeben. Auf der Azure VMware Solution-Seite muss lediglich der Selektor für richtlinienbasierten Datenverkehr aktiviert werden. 

1. Navigieren Sie im Azure-Portal zum Virtual WAN-Hubstandort. Klicken Sie unter **Konnektivität** auf **VPN (Site-to-Site)** .

2. Wählen Sie den Namen Ihres VPN-Standorts aus, klicken Sie ganz rechts auf die Auslassungspunkte (...) und dann auf **VPN-Verbindung mit diesem Hub bearbeiten**.
 
   :::image type="content" source="media/create-ipsec-tunnel/edit-vpn-section-to-this-hub.png" alt-text="Der Screenshot der Seite in Azure für den Standort des Virtual WAN-Hubs zeigt Auslassungspunkte, die für den Zugriff auf die VPN-Verbindung zu diesem Hub ausgewählt wurden." lightbox="media/create-ipsec-tunnel/edit-vpn-section-to-this-hub.png":::

3. Bearbeiten Sie die Verbindung zwischen dem VPN-Standort und dem Hub, und wählen Sie dann **Speichern** aus.
   - Wählen Sie für "IPSec" (Internet Protocol Security) **Benutzerdefiniert** aus.
   - **Aktivieren** Sie den Selektor für richtlinienbasierten Datenverkehr.
   - Geben Sie die Details für **IKE-Phase 1** und **IKE-Phase 2 (IPSec)** an. 
 
   :::image type="content" source="media/create-ipsec-tunnel/edit-vpn-connection.png" alt-text="Der Screenshot der Seite „VPN-Verbindung bearbeiten“."::: 
 
   Die Datenverkehrsselektoren oder Subnetze, die Teil der richtlinienbasierten Verschlüsselungsdomäne sind, müssen wie folgt angegeben werden:
    
   - Virtual WAN-Hub /24
   - Private Azure VMware Solution-Cloud /22
   - Verbundenes virtuelles Azure-Netzwerk (sofern vorhanden)

## <a name="connect-your-vpn-site-to-the-hub"></a>Herstellen einer Verbindung zwischen VPN-Standort und Hub

1. Wählen Sie den Namen Ihres VPN-Standorts aus, und klicken Sie dann auf **VPN-Standorte verbinden**. 
1. Geben Sie im Feld **Vorinstallierter Schlüssel** den Schlüssel ein, der zuvor für den lokalen Endpunkt definiert wurde. 

   >[!TIP]
   >Wenn Sie über keinen zuvor definierten Schlüssel verfügen, können Sie dieses Feld leer lassen. Es wird automatisch ein Schlüssel für Sie generiert. 
 
   >[!IMPORTANT]
   >Aktivieren Sie **Standardroute weitergeben** nur dann, wenn Sie eine Firewall im Hub bereitstellen und diese der nächste Hop für Verbindungen über diesen Tunnel ist.

1. Wählen Sie **Verbinden** aus. Auf einem Verbindungsstatusbildschirm wird der Status der Tunnelerstellung angezeigt.

2. Navigieren Sie zur Virtual WAN-Übersicht, und öffnen Sie die Seite für den VPN-Standort, um die VPN-Konfigurationsdatei für den lokalen Endpunkt herunterzuladen.  

3. Fügen Sie das Azure VMware Solution-ExpressRoute-Gateway in den Virtual WAN-Hub ein. Für diesen Schritt müssen Sie zunächst Ihre private Cloud erstellen.

   [!INCLUDE [request-authorization-key](includes/request-authorization-key.md)]

4. Verbinden Sie Azure VMware Solution und das VPN-Gateway im Virtual WAN-Hub. 
   1. Öffnen Sie im Azure-Portal das zuvor erstellte virtuelle WAN. 
   1. Wählen Sie den erstellten Virtual WAN-Hub und dann im linken Bereich **ExpressRoute** aus. 
   1. Wählen Sie **+ Autorisierungsschlüssel einlösen** aus.

      :::image type="content" source="media/create-ipsec-tunnel/redeem-authorization-key.png" alt-text="Der Screenshot der ExpressRoute-Seite für die private Cloud mit ausgewählter Option „Autorisierungsschlüssel einlösen“.":::

   1. Fügen Sie den Autorisierungsschlüssel in das Feld „Autorisierungsschlüssel“ ein.
   1. Fügen Sie die ExpressRoute-ID in das Feld **Peerleitungs-URI** ein. 
   1. Wählen Sie **Diese ExpressRoute-Leitung wird dem Hub automatisch zugeordnet** aus. 
   1. Wählen Sie **Hinzufügen** aus, um die Verbindung zu erstellen. 

5. Testen Sie Ihre Verbindung, indem Sie [ein NSX-T-Segment erstellen](./tutorial-nsx-t-network-segment.md) und eine VM im Netzwerk bereitstellen. Pingen Sie den lokalen Endpunkt und den Azure VMware Solution-Endpunkt.
