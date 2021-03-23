---
title: Anfordern eines Autorisierungsschlüssels für ExpressRoute
description: Schritte zum Anfordern eines Autorisierungsschlüssels für ExpressRoute
ms.topic: include
ms.date: 03/15/2021
ms.openlocfilehash: 54a610c8b0f3f3fe9d3ebe39291bba7767007839
ms.sourcegitcommit: 4bda786435578ec7d6d94c72ca8642ce47ac628a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/16/2021
ms.locfileid: "103491837"
---
<!-- used in expressroute-global-reach-private-cloud.md and create-ipsec-tunnel.md -->

1. Wählen Sie im Azure-Portal im Abschnitt **Konnektivität** auf der Registerkarte **ExpressRoute** die Option **+ Autorisierungsschlüssel anfordern** aus. 

   :::image type="content" source="../media/expressroute-global-reach/start-request-authorization-key.png" alt-text="Screenshot: Anfordern eines ExpressRoute-Autorisierungsschlüssels" border="true" lightbox="../media/expressroute-global-reach/start-request-authorization-key.png":::

1. Geben Sie einen Namen für ihn an, und wählen Sie **Erstellen** aus. 
      
   Die Erstellung des Schlüssels kann ungefähr 30 Sekunden dauern. Nach der Erstellung wird der neue Schlüssel in der Liste der Autorisierungsschlüssel für die private Cloud angezeigt.

   :::image type="content" source="../media/expressroute-global-reach/show-global-reach-auth-key.png" alt-text="Screenshot: ExpressRoute Global Reach-Autorisierungsschlüssel":::
  
1. Notieren Sie sich den Autorisierungsschlüssel und die ExpressRoute-ID. Diese Angaben werden verwendet, um das Peering abzuschließen.  

   > [!NOTE]
   > Der Autorisierungsschlüssel wird nach einiger Zeit nicht mehr angezeigt. Kopieren Sie ihn daher, sobald er angezeigt wird.