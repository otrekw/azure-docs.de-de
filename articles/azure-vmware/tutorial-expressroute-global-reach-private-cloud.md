---
title: 'Tutorial: Zusammenfassen lokaler Umgebungen mittels Peering zu einer privaten Cloud'
description: Erfahren Sie, wie Sie ExpressRoute Global Reach-Peering mit einer privaten Cloud in einer Azure VMware Solution-Instanz erstellen.
ms.topic: tutorial
ms.date: 03/17/2021
ms.openlocfilehash: ae92bf89a08c5fade8757e3ee596c4ed4a5e6389
ms.sourcegitcommit: 4bda786435578ec7d6d94c72ca8642ce47ac628a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/16/2021
ms.locfileid: "103494157"
---
# <a name="tutorial-peer-on-premises-environments-to-a-private-cloud"></a>Tutorial: Zusammenfassen lokaler Umgebungen mittels Peering zu einer privaten Cloud

Über ExpressRoute Global Reach wird Ihre lokale Umgebung mit Ihrer privaten Azure VMware Solution-Cloud verbunden. Die ExpressRoute Global Reach-Verbindung wird zwischen der ExpressRoute-Leitung der privaten Cloud und einer vorhandenen ExpressRoute-Verbindung mit Ihren lokalen Umgebungen hergestellt. 

Für die ExpressRoute-Leitung, die Sie beim [Konfigurieren des Netzwerks für Ihre private VMware-Cloud in Azure](tutorial-configure-networking.md) verwenden, müssen Autorisierungsschlüssel erstellt und verwendet werden.  Sie haben bereits einen Autorisierungsschlüssel der ExpressRoute-Leitung verwendet, und Sie erstellen in diesem Tutorial einen zweiten Autorisierungsschlüssel, um ein Peering mit Ihrer lokalen ExpressRoute-Leitung einzurichten.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen eines zweiten Autorisierungsschlüssels für _Leitung 2_, die ExpressRoute-Leitung der privaten Cloud
> * Verwenden entweder des Azure-Portals oder der Azure CLI in einer Cloud Shell-Methode im Abonnement von _Leitung 1_, um das ExpressRoute Global Reach-Peering zwischen lokaler Umgebung und privater Cloud zu aktivieren


## <a name="before-you-begin"></a>Voraussetzungen

Lesen Sie zunächst die Dokumentation zum [Aktivieren der Konnektivität zwischen ExpressRoute-Leitungen in verschiedenen Azure-Abonnements](../expressroute/expressroute-howto-set-global-reach-cli.md#enable-connectivity-between-expressroute-circuits-in-different-azure-subscriptions), bevor Sie die Konnektivität zwischen zwei ExpressRoute-Leitungen mit ExpressRoute Global Reach aktivieren.  

## <a name="prerequisites"></a>Voraussetzungen

- Eine eingerichtete Verbindung mit einer privaten Azure VMware Solution-Cloud, deren ExpressRoute-Leitung mittels Peering mit einem ExpressRoute-Gateway in einem virtuellen Azure-Netzwerk (VNET) verbunden ist. Dies ist Circuit 2 aus den Peeringvorgängen.
- Eine separate, funktionierende ExpressRoute-Leitung für die Verbindungsherstellung zwischen lokalen Umgebungen und Azure. Dies ist Circuit 1 aus Sicht der Peeringvorgänge.
- Ein [Netzwerkadressblock](../expressroute/expressroute-routing.md#ip-addresses-used-for-peerings) vom Typ „/29“ ohne Überschneidungen für das ExpressRoute Global Reach-Peering.
- Stellen Sie sicher, dass alle Gateways mit dem Dienst des ExpressRoute-Anbieters eine ASN (Autonomous System Number, autonome Systemnummer) mit 4 Bytes unterstützen. Azure VMware Solution verwendet öffentliche ASNs mit 4 Bytes zum Ankündigen von Routen.

>[!IMPORTANT]
>Im Kontext dieser Voraussetzungen ist _Circuit 1_ Ihre lokale ExpressRoute-Leitung. Die ExpressRoute-Leitung Ihrer privaten Cloud befindet sich in einem anderen Abonnement und heißt _Circuit 2_.

## <a name="create-an-expressroute-authorization-key-in-the-on-premises-circuit"></a>Erstellen eines ExpressRoute-Autorisierungsschlüssels in der lokalen Leitung

[!INCLUDE [request-authorization-key](includes/request-authorization-key.md)]
 
## <a name="peer-private-cloud-to-on-premises-with-authorization-key"></a>Verknüpfen der privaten Cloud mit der lokalen Umgebung mittels Peering unter Verwendung des Autorisierungsschlüssels
Nachdem Sie einen Autorisierungsschlüssel für die ExpressRoute-Leitung der privaten Cloud erstellt haben, können Sie sie mittels Peering mit Ihrer lokalen ExpressRoute-Leitung verknüpfen. Das Peering erfolgt aus der Perspektive der lokalen ExpressRoute-Leitung entweder über das **Azure-Portal** oder über Verwenden der **Azure CLI in einer Cloud Shell-Umgebung**. Bei beiden Methoden verwenden Sie die Ressourcen-ID und den Autorisierungsschlüssel der ExpressRoute-Leitung Ihrer privaten Cloud dazu, das Peering abzuschließen.

### <a name="portal"></a>[Portal](#tab/azure-portal)
 
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) mit dem Abonnement an, das auch für die lokale ExpressRoute-Leitung verwendet wird.

1. Wählen Sie in der **Übersicht** der privaten Cloud unter „Verwalten“ die Option **Konnektivität** > **ExpressRoute Global Reach** > **Hinzufügen** aus.

    :::image type="content" source="./media/expressroute-global-reach/expressroute-global-reach-tab.png" alt-text="Screenshot: Registerkarte „ExpressRoute Global Reach“ in der privaten Azure VMware Solution-Cloud":::

1. Erstellen Sie eine lokale Cloudverbindung. Führen Sie einen der folgenden Schritte aus, und wählen Sie dann **Erstellen** aus:

   - Wählen Sie die **ExpressRoute-Leitung** in der Liste aus. Oder:
   - Wenn Sie über die Leitungs-ID verfügen, fügen Sie sie in das Feld ein, und geben Sie den Autorisierungsschlüssel an.

   :::image type="content" source="./media/expressroute-global-reach/on-premises-cloud-connections.png" alt-text="Eingeben der ExpressRoute-ID und des Autorisierungsschlüssels und Auswählen von „Erstellen“":::   
   
   Die neue Verbindung wird in der Liste „Lokale Cloudverbindungen“ angezeigt.

>[!TIP]
>Sie können eine Verbindung aus der Liste löschen oder trennen, indem Sie **Mehr** auswählen.  
>
> :::image type="content" source="./media/expressroute-global-reach/on-premises-connection-disconnect.png" alt-text="Eine lokale Verbindung trennen oder löschen":::

### <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

Wir haben die [CLI-Befehle](../expressroute/expressroute-howto-set-global-reach-cli.md) mit speziellen Details und Beispielen erweitert, um Sie bei beim Konfigurieren des ExpressRoute Global Reach-Peerings zwischen lokalen Umgebungen und einer privaten Azure VMware Solution-Cloud zu unterstützen.

>[!TIP]
>Zur besseren Übersichtlichkeit der Azure CLI-Befehlsausgabe wird in dieser Anleitung ggf. ein [Argument vom Typ `–query`](https://docs.microsoft.com/cli/azure/query-azure-cli) verwendet, um eine JMESPath-Abfrage auszuführen und nur die erforderlichen Ergebnisse anzuzeigen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) mit dem Abonnement an, das auch für die lokale ExpressRoute-Leitung verwendet wird. 

1. Öffnen Sie eine Cloud Shell, und verwenden Sie die standardmäßige Bash-Shell.

   :::image type="content" source="media/expressroute-global-reach/azure-portal-cloud-shell.png" alt-text="Screenshot: Cloud Shell im Azure-Portal":::

1. Erstellen Sie das Peering für Leitung 1, und übergeben Sie die Ressourcen-ID und den Autorisierungsschlüssel von Leitung 2. 

   ```azurecli-interactive
   az network express-route peering connection create -g <ResourceGroupName> --circuit-name <Circuit1Name> --peering-name AzurePrivatePeering -n <ConnectionName> --peer-circuit <Circuit2ResourceID> --address-prefix <__.__.__.__/29> --authorization-key <authorizationKey>
   ```

   :::image type="content" source="media/expressroute-global-reach/azure-command-with-results.png" alt-text="Screenshot: Befehl und Ergebnis des erfolgreichen Peerings zwischen Leitung 1 und Leitung 2":::

Sie können nun von lokalen Umgebungen aus über das ExpressRoute Global Reach-Peering eine Verbindung mit Ihrer privaten Cloud herstellen.

>[!TIP]
>Das Peering kann wie unter [Deaktivieren der Konnektivität zwischen Ihren lokalen Netzwerken](../expressroute/expressroute-howto-set-global-reach-cli.md#disable-connectivity-between-your-on-premises-networks) beschrieben gelöscht werden.


---

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie erfahren, wie Sie das ExpressRoute Global Reach-Peering zwischen lokaler Umgebung und privater Cloud aktivieren. 

Arbeiten Sie das nächste Tutorial durch, um zu erfahren Sie, wie Sie eine VMware HCX-Lösung für Ihre private Azure VMware Solution-Cloud bereitstellen und konfigurieren.

> [!div class="nextstepaction"]
> [Bereitstellen und Konfigurieren von VMware HCX](tutorial-deploy-vmware-hcx.md)


<!-- LINKS - external-->

<!-- LINKS - internal -->
