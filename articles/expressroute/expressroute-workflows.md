---
title: 'Azure ExpressRoute: Workflows zum Konfigurieren von Verbindungen'
description: Diese Seite zeigt die Workflows zum Konfigurieren von ExpressRoute-Verbindungen und -Peerings.
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: conceptual
ms.date: 09/18/2018
ms.author: cherylmc
ms.openlocfilehash: e833e20085d7cfd8f727acb394851e96e7e19368
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "75864365"
---
# <a name="expressroute-workflows-for-circuit-provisioning-and-circuit-states"></a>ExpressRoute-Workflows für die Verbindungsbereitstellung und Verbindungszustände
Auf dieser Seite erhalten Sie einen Überblick über die Workflows zur Dienstbereitstellung und Routingkonfiguration.

![Verbindungsworkflow](./media/expressroute-workflows/expressroute-circuit-workflow.png)

In der folgenden Abbildung und in den entsprechenden Schritten wird die End-to-End-Bereitstellung einer ExpressRoute-Verbindung erläutert. 

1. Verwenden Sie PowerShell, um eine ExpressRoute-Verbindung zu konfigurieren. Ausführlichere Anweisungen finden Sie im Artikel [Erstellen von ExpressRoute-Verbindungen](expressroute-howto-circuit-classic.md) .
2. Fordern Sie Konnektivität vom Service Provider an. Dieser Prozess variiert. Wenden Sie sich an Ihren Konnektivitätsanbieter, um weitere Details zum Anfordern der Konnektivität zu erhalten.
3. Stellen Sie sicher, dass die Verbindung erfolgreich eingerichtet wurde, indem Sie den Bereitstellungszustand der ExpressRoute-Verbindung mithilfe von PowerShell überprüfen. 
4. Konfigurieren Sie Routingdomänen. Wenn Ihr Konnektivitätsanbieter die Layer 3-Konfiguration verwaltet, wird das Routing für Ihre Verbindung konfiguriert. Wenn Ihr Konnektivitätsanbieter nur Layer 2-Dienste anbietet, müssen Sie das Routing gemäß den Richtlinien auf den Seiten [Routinganforderungen](expressroute-routing.md) und [Routingkonfiguration](expressroute-howto-routing-classic.md) konfigurieren.
   
   * Privates Azure-Peering aktivieren – Aktivieren Sie dieses Peering, um eine Verbindung mit virtuellen Computern/Clouddiensten herzustellen, die in virtuellen Netzwerken bereitgestellt werden.

   * Microsoft-Peering aktivieren – Aktivieren Sie dieses Peering, um auf Microsoft-Onlinedienste wie Office 365 zuzugreifen. Über Microsoft-Peering kann auf alle Azure-PaaS-Dienste zugegriffen werden.
     
     > [!IMPORTANT]
     > Verwenden Sie für die Verbindung mit Microsoft unbedingt einen anderen Proxy/Edge als für das Internet. Wenn Sie denselben Edge für ExpressRoute und das Internet verwenden, führt das zu asymmetrischem Routing und Konnektivitätsausfällen für Ihr Netzwerk.
     > 
     > 
     
     ![Routing von Workflows](./media/expressroute-workflows/routing-workflow.png)
5. Verknüpfen von virtuellen Netzwerken mit ExpressRoute-Verbindungen – Sie können virtuelle Netzwerke mit Ihrer ExpressRoute-Verbindung verknüpfen. Führen Sie die Schritte zum [Verknüpfen von VNets](expressroute-howto-linkvnet-arm.md) mit Ihrer Verbindung aus. Diese VNets können sich im gleichen Azure-Abonnement wie die ExpressRoute-Verbindung oder aber in einem anderen Abonnement befinden.

## <a name="expressroute-circuit-provisioning-states"></a>Bereitstellungszustände von ExpressRoute-Verbindungen
Jede ExpressRoute-Verbindung hat zwei Zustände:

* Bereitstellungszustand des Service Providers
* Status

Der Status gibt den Bereitstellungszustand von Microsoft an. Diese Eigenschaft wird auf „Aktiviert“ festgelegt, wenn Sie eine ExpressRoute-Verbindung erstellen.

Der Bereitstellungszustand des Konnektivitätsanbieters gibt den Zustand aufseiten des Konnektivitätsanbieters an. Mögliche Zustände: *NichtBereitgestellt*, *Bereitstellung* oder *Bereitgestellt*. Um das Peering zu konfigurieren, muss die ExpressRoute-Verbindung den Zustand „Provisioned“ aufweisen.

### <a name="possible-states-of-an-expressroute-circuit"></a>Mögliche Zustände einer ExpressRoute-Verbindung
In diesem Abschnitt sind die möglichen Zustände einer ExpressRoute-Verbindung aufgeführt.

**Zum Zeitpunkt der Erstellung**:

Von der ExpressRoute-Verbindung werden während der Ressourcenerstellung folgende Zustände gemeldet.

    ServiceProviderProvisioningState : NotProvisioned
    Status                           : Enabled


**Der Konnektivitätsanbieter stellt die Verbindung gerade bereit**:

Von der ExpressRoute-Verbindung werden folgende Zustände gemeldet, während der Konnektivitätsanbieter an der Bereitstellung der Verbindung arbeitet.

    ServiceProviderProvisioningState : Provisioning
    Status                           : Enabled


**Der Konnektivitätsanbieter hat die Bereitstellung abgeschlossen**:

Von der ExpressRoute-Verbindung werden folgende Zustände gemeldet, sobald der Konnektivitätsanbieter die Verbindung erfolgreich bereitgestellt hat.

    ServiceProviderProvisioningState : Provisioned
    Status                           : Enabled


**Der Konnektivitätsanbieter hebt die Verbindungsbereitstellung auf**:

Wenn die Bereitstellung der ExpressRoute-Verbindung aufgehoben werden muss, werden von der Verbindung die folgenden Zustände gemeldet, sobald der Dienstanbieter die Bereitstellung vollständig aufgehoben hat.

    ServiceProviderProvisioningState : NotProvisioned
    Status                           : Enabled


Sie können sie bei Bedarf erneut aktivieren oder PowerShell-Cmdlets ausführen, um die Verbindung zu löschen.  

> [!IMPORTANT]
> Eine Verbindung kann nicht gelöscht werden, wenn „ServiceProviderProvisioningState“ den Zustand „Provisioning“ oder „Provisioned“ aufweist. Der Konnektivitätsanbieter muss die Bereitstellung der Verbindung aufheben, bevor sie gelöscht werden kann. Microsoft stellt die Verbindung weiterhin in Rechnung, bis die ExpressRoute-Verbindungsressource in Azure gelöscht wird.
> 

## <a name="routing-session-configuration-state"></a>Konfigurationszustand der Routingsitzung
Durch den BGP-Bereitstellungszustand wird gemeldet, ob die BGP-Sitzung in Microsoft Edge aktiviert wurde. Der Zustand muss aktiviert sein, um privates oder Microsoft-Peering zu verwenden.

Insbesondere beim Microsoft-Peering sollte der BGP-Sitzungszustand überprüft werden. Zusätzlich zum BGP-Bereitstellungszustand gibt es einen weiteren Zustand: *advertised public prefixes state*. Der Zustand der angekündigten öffentlichen Präfixe muss *configured* lauten, damit die BGP-Sitzung aktiv ist und das Routing durchgängig funktioniert. 

Wenn der Zustand der angekündigten öffentlichen Präfixe *validation needed* lautet, ist die BGP-Sitzung nicht aktiviert, da die angekündigten Präfixe keiner AS-Nummer in einer der Routingregistrierungen entsprachen. 

> [!IMPORTANT]
> Wenn der Zustand der angekündigten öffentlichen Präfixe *manual validation* lautet, müssen Sie ein Supportticket beim [Microsoft-Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) öffnen, nachweisen, dass Sie der Besitzer der angekündigten IP-Adressen sind, und die zugeordnete autonome Systemnummer angeben.
> 
> 

## <a name="next-steps"></a>Nächste Schritte
* Konfigurieren Sie Ihre ExpressRoute-Verbindung.
  
  * [Erstellen Sie eine ExpressRoute-Verbindung.](expressroute-howto-circuit-arm.md)
  * [Konfigurieren des Routings](expressroute-howto-routing-arm.md)
  * [Verknüpfen eines VNet mit einer ExpressRoute-Verbindung](expressroute-howto-linkvnet-arm.md)

