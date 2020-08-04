---
title: Zusammenfassen lokaler Umgebungen mittels Peering zu einer privaten Cloud
description: In diesem AVS-Tutorial (Azure VMware Solution) wird ein ExpressRoute Global Reach-Peering mit einer privaten Cloud in einer AVS-Instanz erstellt.
ms.topic: tutorial
ms.date: 07/16/2020
ms.openlocfilehash: a9a002eab3219a0db74062570d31595bfcc0d6a3
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2020
ms.locfileid: "87093596"
---
# <a name="tutorial-peer-on-premises-environments-to-a-private-cloud"></a>Tutorial: Zusammenfassen lokaler Umgebungen mittels Peering zu einer privaten Cloud

Über ExpressRoute Global Reach wird Ihre lokale Umgebung mit Ihren privaten Clouds verbunden. Die ExpressRoute Global Reach-Verbindung wird zwischen einer ExpressRoute-Leitung einer privaten Cloud und einer vorhandenen ExpressRoute-Verbindung mit Ihren lokalen Umgebungen hergestellt.  Es gibt eine Anleitung zum Konfigurieren von ExpressRoute Global Reach per Azure CLI und per PowerShell, und wir haben die [CLI-Befehle](../expressroute/expressroute-howto-set-global-reach-cli.md) mit spezifischen Details und Beispielen erweitert, um Sie bei beim Konfigurieren des ExpressRoute Global Reach-Peerings zwischen lokalen Umgebungen und einer privaten AVS-Cloud (Azure VMware Solution) zu unterstützen.   

Lesen Sie zunächst die Dokumentation zum [Aktivieren der Konnektivität zwischen ExpressRoute-Leitungen in verschiedenen Azure-Abonnements](../expressroute/expressroute-howto-set-global-reach-cli.md#enable-connectivity-between-expressroute-circuits-in-different-azure-subscriptions), bevor Sie die Konnektivität zwischen zwei ExpressRoute-Leitungen mit ExpressRoute Global Reach aktivieren.  Für die ExpressRoute-Leitung, die Sie beim [Konfigurieren des Netzwerks zwischen Azure und privater Cloud](tutorial-configure-networking.md) verwenden, müssen Autorisierungsschlüssel erstellt und verwendet werden, wenn Sie ein Peering mit ExpressRoute-Gateways oder mit anderen ExpressRoute-Leitungen unter Verwendung von Global Reach einrichten. Sie haben bereits einen Autorisierungsschlüssel der ExpressRoute-Leitung verwendet, und Sie erstellen einen zweiten Autorisierungsschlüssel, um ein Peering mit Ihrer lokalen ExpressRoute-Leitung einzurichten.

> [!TIP]
> Im Kontext dieser Anleitung ist _Circuit 1_ Ihre lokale ExpressRoute-Leitung. Die ExpressRoute-Leitung Ihrer privaten Cloud befindet sich in einem anderen Abonnement und heißt _Circuit 2_. 

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Verwenden der vorhandenen Anweisungen zum Verwalten von ExpressRoute-Leitungen und ExpressRoute Global Reach-Peerings
> * Erstellen eines Autorisierungsschlüssels für _Circuit 2_ (ExpressRoute-Leitung der privaten Cloud)
> * Verwenden der Azure CLI in einer Cloud Shell-Instanz im Abonnement von _Circuit 1_, um das ExpressRoute Global Reach-Peering zwischen lokaler Umgebung und privater Cloud zu aktivieren

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial wird Folgendes benötigt:
- Eine private Cloud, deren ExpressRoute-Leitung mittels Peering mit einem ExpressRoute-Gateway in einem virtuellen Azure-Netzwerk (VNET) verbunden ist (_Circuit 2_ aus Sicht der Peeringprozeduren).
- Eine separate, funktionierende ExpressRoute-Leitung für die Verbindungsherstellung zwischen lokalen Umgebungen und Azure (_Circuit 1_ aus Sicht der Peeringprozeduren).
- Ein [Netzwerkadressblock](../expressroute/expressroute-routing.md#ip-addresses-used-for-peerings) vom Typ „/29“ ohne Überschneidungen für das ExpressRoute Global Reach-Peering.

## <a name="create-an-expressroute-authorization-key-in-the-avs-private-cloud"></a>Erstellen eines ExpressRoute-Autorisierungsschlüssels in der privaten AVS-Cloud

1. Navigieren Sie in der **Übersicht** der privaten Cloud unter „Verwalten“ zu **Konnektivität > ExpressRoute > Autorisierungsschlüssel anfordern**.

   :::image type="content" source="media/expressroute-global-reach/start-request-auth-key.png" alt-text="Auswählen von „Konnektivität“ > „ExpressRoute“ > „Autorisierungsschlüssel anfordern“, um eine neue Anforderung zu initiieren":::

2. Geben Sie den Namen für den Autorisierungsschlüssel ein, und wählen Sie **Erstellen** aus. 

   :::image type="content" source="media/expressroute-global-reach/create-global-reach-auth-key.png" alt-text="Klicken auf „Erstellen“, um einen neuen Autorisierungsschlüssel zu erstellen":::

   Nach der Erstellung wird der neue Schlüssel in der Liste der Autorisierungsschlüssel für die private Cloud angezeigt. 

   :::image type="content" source="media/expressroute-global-reach/show-global-reach-auth-key.png" alt-text="Vergewissern, dass der neue Autorisierungsschlüssel in der Liste der Schlüssel für die private Cloud angezeigt wird":::

3. Notieren Sie sich den Autorisierungsschlüssel und die ExpressRoute-ID zusammen mit dem Adressblock vom Typ „/29“. Diese Angaben werden im nächsten Schritt benötigt, um das Peering abzuschließen. 

## <a name="peer-private-cloud-to-on-premises-using-authorization-key"></a>Verknüpfen der privaten Cloud mit der lokalen Umgebung mittels Peering unter Verwendung des Autorisierungsschlüssels

Nachdem Sie einen Autorisierungsschlüssel für die ExpressRoute-Leitung der privaten Cloud erstellt haben, können Sie sie mittels Peering mit Ihrer lokalen ExpressRoute-Leitung verknüpfen.  Das Peering erfolgt aus Sicht der lokalen ExpressRoute-Leitung mithilfe der Azure CLI in einer Cloud Shell-Instanz und unter Verwendung der Ressourcen-ID und des Autorisierungsschlüssels der ExpressRoute-Leitung Ihrer privaten Cloud, die in den vorherigen Schritten erstellt wurde.

> [!TIP]  
> Zur besseren Übersichtlichkeit der Azure CLI-Befehlsausgabe wird in dieser Anleitung ggf. ein [Argument vom Typ `–query` verwendet, um eine JMESPath-Abfrage auszuführen und nur die erforderlichen Ergebnisse anzuzeigen](https://docs.microsoft.com/cli/azure/query-azure-cli?view=azure-cli-latest).


1. Melden Sie sich beim Azure-Portal mit dem Abonnement an, das auch für die lokale ExpressRoute-Leitung verwendet wurde, und öffnen Sie eine Cloud Shell-Instanz. Verwenden Sie die standardmäßige Bash-Shell.
 
   :::image type="content" source="media/expressroute-global-reach/open-cloud-shell.png" alt-text="Anmelden beim Azure-Portal und Öffnen einer Cloud Shell-Instanz":::
 
2. Geben Sie den Azure CLI-Befehl zum Erstellen des Peerings in die Befehlszeile ein. Verwenden Sie dabei Ihre spezifischen Informationen sowie die Ressourcen-ID, den Autorisierungsschlüssel und den CIDR-Netzwerkblock vom Typ „/29“. 

   Das folgende Beispiel zeigt den zu verwendenden Befehl sowie die Ausgabe mit der Angabe, dass das Peering erfolgreich war. Der Beispielbefehl basiert auf dem Befehl, der in Schritt 3 von [Aktivieren der Konnektivität zwischen ExpressRoute-Leitungen in verschiedenen Azure-Abonnements](../expressroute/expressroute-howto-set-global-reach-cli.md#enable-connectivity-between-expressroute-circuits-in-different-azure-subscriptions) verwendet wird.

   :::image type="content" source="media/expressroute-global-reach/azure-command-with-results.png" alt-text="Erstellen des ExpressRoute Global Reach-Peerings per Azure CLI-Befehl in einer Cloud Shell-Instanz":::
 
   Nun sollte es möglich sein, von lokalen Umgebungen aus über das ExpressRoute Global Reach-Peering eine Verbindung mit Ihrer privaten Cloud herzustellen.

> [!TIP]
> Das soeben erstellte Peering kann wie unter [Deaktivieren der Konnektivität zwischen Ihren lokalen Netzwerken](../expressroute/expressroute-howto-set-global-reach-cli.md#disable-connectivity-between-your-on-premises-networks) beschrieben wieder gelöscht werden.


## <a name="next-steps"></a>Nächste Schritte

Wenn Sie beabsichtigen, Hybrid Cloud Extension (HCX) zum Migrieren von VM-Workloads zu Ihrer privaten Cloud zu verwenden, führen Sie das Verfahren unter [Installieren von HCX für eine Azure-VMware-Lösung](hybrid-cloud-extension-installation.md) aus.


<!-- LINKS - external-->

<!-- LINKS - internal -->
