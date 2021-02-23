---
title: 'Tutorial: Zusammenfassen lokaler Umgebungen mittels Peering zu einer privaten Cloud'
description: Erfahren Sie, wie Sie ExpressRoute Global Reach-Peering mit einer privaten Cloud in einer Azure VMware Solution-Instanz erstellen.
ms.topic: tutorial
ms.date: 01/27/2021
ms.openlocfilehash: 04be94849f1cd78ff915ca271845b283c8cfc64c
ms.sourcegitcommit: de98cb7b98eaab1b92aa6a378436d9d513494404
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/17/2021
ms.locfileid: "100558795"
---
# <a name="tutorial-peer-on-premises-environments-to-a-private-cloud"></a>Tutorial: Zusammenfassen lokaler Umgebungen mittels Peering zu einer privaten Cloud

Über ExpressRoute Global Reach wird Ihre lokale Umgebung mit Ihrer privaten Azure VMware Solution-Cloud verbunden. Die ExpressRoute Global Reach-Verbindung wird zwischen der ExpressRoute-Leitung der privaten Cloud und einer vorhandenen ExpressRoute-Verbindung mit Ihren lokalen Umgebungen hergestellt. 

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Aktivieren des ExpressRoute Global Reach-Peerings zwischen lokaler Umgebung und privater Cloud mithilfe des Azure-Portals


## <a name="before-you-begin"></a>Voraussetzungen

Lesen Sie zunächst die Dokumentation zum [Aktivieren der Konnektivität zwischen ExpressRoute-Leitungen in verschiedenen Azure-Abonnements](../expressroute/expressroute-howto-set-global-reach-cli.md#enable-connectivity-between-expressroute-circuits-in-different-azure-subscriptions), bevor Sie die Konnektivität zwischen zwei ExpressRoute-Leitungen mit ExpressRoute Global Reach aktivieren.  


## <a name="prerequisites"></a>Voraussetzungen

- Eine separate, funktionierende ExpressRoute-Leitung für die Verbindungsherstellung zwischen lokalen Umgebungen und Azure
- Stellen Sie sicher, dass alle Gateways mit dem Dienst des ExpressRoute-Anbieters eine ASN (Autonomous System Number, autonome Systemnummer) mit 4 Bytes unterstützen. Azure VMware Solution verwendet öffentliche ASNs mit 4 Bytes zum Ankündigen von Routen.


## <a name="create-an-expressroute-authorization-key-in-the-on-premises-expressroute-circuit"></a>Erstellen eines ExpressRoute-Autorisierungsschlüssels in der lokalen ExpressRoute-Leitung

1. Wählen Sie auf dem Blatt **ExpressRoute-Leitungen** unter „Einstellungen“ die Option **Autorisierungen** aus.

2. Geben Sie den Namen für den Autorisierungsschlüssel ein, und wählen Sie **Speichern** aus.

    :::image type="content" source="media/expressroute-global-reach/start-request-auth-key-on-premises-expressroute.png" alt-text="Auswählen von „Autorisierungen“ und Eingeben des Namens für den Autorisierungsschlüssel":::
  
     Nach der Erstellung wird der neue Schlüssel in der Liste der Autorisierungsschlüssel für die Leitung angezeigt.
 
 4. Notieren Sie sich den Autorisierungsschlüssel und die ExpressRoute-ID. Diese Angaben werden im nächsten Schritt benötigt, um das Peering abzuschließen.
 
 
 ## <a name="peer-private-cloud-to-on-premises"></a>Peering zwischen privater Cloud und lokaler Umgebung

1. Wählen Sie in der **Übersicht** der privaten Cloud unter „Verwalten“ die Option **Konnektivität > ExpressRoute Global Reach > Hinzufügen** aus.

    :::image type="content" source="./media/expressroute-global-reach/expressroute-global-reach-tab.png" alt-text="Wählen Sie im Menü die Option „Konnektivität“ aus, wählen Sie die Registerkarte „ExpressRoute Global Reach“ und dann „Hinzufügen“ aus.":::

2. Geben Sie die ExpressRoute-ID und den Autorisierungsschlüssel ein, die Sie im vorherigen Abschnitt erstellt haben.

    :::image type="content" source="./media/expressroute-global-reach/on-premises-cloud-connections.png" alt-text="Eingeben der ExpressRoute-ID und des Autorisierungsschlüssels und Auswählen von „Erstellen“":::

3. Klicken Sie auf **Erstellen**. Die neue Verbindung wird in der Liste „Lokale Cloudverbindungen“ angezeigt.  

>[!TIP]
>Sie können eine Verbindung aus der Liste löschen oder trennen, indem Sie **Mehr** auswählen.  
>
> :::image type="content" source="./media/expressroute-global-reach/on-premises-connection-disconnect.png" alt-text="Eine lokale Verbindung trennen oder löschen":::


## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie erfahren, wie Sie das ExpressRoute Global Reach-Peering zwischen lokaler Umgebung und privater Cloud aktivieren. 

Arbeiten Sie das nächste Tutorial durch, um zu erfahren Sie, wie Sie eine VMware HCX-Lösung für Ihre private Azure VMware Solution-Cloud bereitstellen und konfigurieren.

> [!div class="nextstepaction"]
> [Bereitstellen und Konfigurieren von VMware HCX](tutorial-deploy-vmware-hcx.md)


<!-- LINKS - external-->

<!-- LINKS - internal -->
