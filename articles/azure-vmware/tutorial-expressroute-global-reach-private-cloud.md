---
title: Peering lokaler Umgebungen mit Azure VMware Solution
description: Hier erfahren Sie, wie Sie ExpressRoute Global Reach-Peering mit einer privaten Cloud in Azure VMware Solution erstellen.
ms.topic: tutorial
ms.custom: contperf-fy21q4
ms.date: 04/27/2021
ms.openlocfilehash: fb0d8bc8391c96449cdf260c543cfed1af1125f5
ms.sourcegitcommit: 516eb79d62b8dbb2c324dff2048d01ea50715aa1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2021
ms.locfileid: "108176106"
---
# <a name="peer-on-premises-environments-to-azure-vmware-solution"></a>Peering lokaler Umgebungen mit Azure VMware Solution

In diesem Schritt der Schnellstartanleitung verbinden Sie Azure VMware Solution mit Ihrer lokalen Umgebung. Über ExpressRoute Global Reach wird Ihre lokale Umgebung mit Ihrer privaten Azure VMware Solution-Cloud verbunden. Die ExpressRoute Global Reach-Verbindung wird zwischen der ExpressRoute-Leitung der privaten Cloud und einer vorhandenen ExpressRoute-Verbindung mit Ihren lokalen Umgebungen hergestellt. 


>[!NOTE]
>Sie können auch eine Verbindung per VPN herstellen, aber die Beschreibung der Vorgehensweise würde den Rahmen dieser Schnellstartanleitung sprengen.

In diesem Tutorial wird eine Verbindung hergestellt, wie im Diagramm dargestellt:

:::image type="content" source="media/pre-deployment/azure-vmware-solution-on-premises-diagram.png" alt-text="Diagramm: ExpressRoute Global Reach-Netzwerkkonnektivität für die lokale Umgebung" lightbox="media/pre-deployment/azure-vmware-solution-on-premises-diagram.png" border="false":::


## <a name="before-you-begin"></a>Voraussetzungen

Lesen Sie zunächst die Dokumentation zum [Aktivieren der Konnektivität zwischen ExpressRoute-Leitungen in verschiedenen Azure-Abonnements](../expressroute/expressroute-howto-set-global-reach-cli.md#enable-connectivity-between-expressroute-circuits-in-different-azure-subscriptions), bevor Sie die Konnektivität zwischen zwei ExpressRoute-Leitungen mit ExpressRoute Global Reach aktivieren.  

## <a name="prerequisites"></a>Voraussetzungen
- Eine separate, funktionierende ExpressRoute-Leitung für die Verbindungsherstellung zwischen lokalen Umgebungen und Azure. Dies ist _Circuit 1_ für das Peering.
- Stellen Sie sicher, dass alle Gateways mit dem Dienst des ExpressRoute-Anbieters eine ASN (Autonomous System Number, autonome Systemnummer) mit 4 Bytes unterstützen. Azure VMware Solution verwendet öffentliche ASNs mit 4 Bytes zum Ankündigen von Routen.


## <a name="create-an-expressroute-authorization-key-in-the-on-premises-expressroute-circuit"></a>Erstellen eines ExpressRoute-Autorisierungsschlüssels in der lokalen ExpressRoute-Leitung

1. Wählen Sie auf dem Blatt **ExpressRoute-Leitungen** unter „Einstellungen“ die Option **Autorisierungen** aus.

1. Geben Sie den Namen für den Autorisierungsschlüssel ein, und wählen Sie **Speichern** aus.

   :::image type="content" source="media/expressroute-global-reach/start-request-auth-key-on-premises-expressroute.png" alt-text="Auswählen von „Autorisierungen“ und Eingeben des Namens für den Autorisierungsschlüssel":::

   Nach der Erstellung wird der neue Schlüssel in der Liste der Autorisierungsschlüssel für die Leitung angezeigt.

1. Notieren Sie sich den Autorisierungsschlüssel und die ExpressRoute-ID. Diese Angaben werden im nächsten Schritt benötigt, um das Peering abzuschließen.

## <a name="peer-private-cloud-to-on-premises"></a>Peering zwischen privater Cloud und lokaler Umgebung 
Nachdem Sie einen Autorisierungsschlüssel für die ExpressRoute-Leitung der privaten Cloud erstellt haben, können Sie sie mittels Peering mit Ihrer lokalen ExpressRoute-Leitung verknüpfen. Das Peering erfolgt über die lokale ExpressRoute-Leitung im **Azure-Portal**. Sie verwenden die Ressourcen-ID (ID der ExpressRoute-Leitung) und den Autorisierungsschlüssel der ExpressRoute-Leitung Ihrer privaten Cloud dazu, das Peering abzuschließen.

1. Wählen Sie in der privaten Cloud unter „Verwalten“ die Option **Konnektivität** > **ExpressRoute Global Reach** > **Hinzufügen** aus.

    :::image type="content" source="./media/expressroute-global-reach/expressroute-global-reach-tab.png" alt-text="Screenshot: Registerkarte „ExpressRoute Global Reach“ in der privaten Azure VMware Solution-Cloud":::

1. Geben Sie die ExpressRoute-ID und den Autorisierungsschlüssel ein, die Sie im vorherigen Abschnitt erstellt haben.

   :::image type="content" source="./media/expressroute-global-reach/on-premises-cloud-connections.png" alt-text="Screenshot: Dialogfeld zum Eingeben der Verbindungsinformationen":::   

1. Klicken Sie auf **Erstellen**. Die neue Verbindung wird in der Liste „Lokale Cloudverbindungen“ angezeigt.

>[!TIP]
>Sie können eine Verbindung aus der Liste löschen oder trennen, indem Sie **Mehr** auswählen.  
>
>:::image type="content" source="./media/expressroute-global-reach/on-premises-connection-disconnect.png" alt-text="Eine lokale Verbindung trennen oder löschen":::


## <a name="verify-on-premises-network-connectivity"></a>Überprüfen der Netzwerkkonnektivität für die lokale Umgebung

Sie sollten nun auf Ihrem **lokalen Edgerouter** sehen können, wie die NSX-T-Netzwerksegmente und die Azure VMware Solution-Verwaltungssegmente per ExpressRoute verbunden sind.

>[!IMPORTANT]
>Die Umgebungen von Unternehmen unterscheiden sich, und in einigen Fällen kann es erforderlich sein, dass für diese Routen die Weitergabe zurück an das lokale Netzwerk zugelassen wird.  

## <a name="next-steps"></a>Nächste Schritte
Arbeiten Sie das nächste Tutorial durch, um zu erfahren Sie, wie Sie eine VMware HCX-Lösung für Ihre private Azure VMware Solution-Cloud bereitstellen und konfigurieren.

> [!div class="nextstepaction"]
> [Bereitstellen und Konfigurieren von VMware HCX](tutorial-deploy-vmware-hcx.md)


<!-- LINKS - external-->

<!-- LINKS - internal -->
