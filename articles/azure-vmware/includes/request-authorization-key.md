---
title: Anfordern eines Autorisierungsschlüssels für ExpressRoute
description: Schritte zum Anfordern eines Autorisierungsschlüssels für ExpressRoute
ms.topic: include
ms.date: 03/15/2021
ms.openlocfilehash: 44ca81e25dda61ab32ea3455a1f228e134952582
ms.sourcegitcommit: ad921e1cde8fb973f39c31d0b3f7f3c77495600f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/25/2021
ms.locfileid: "107945561"
---
<!-- used in tutorial-expressroute-global-reach-private-cloud.md and create-ipsec-tunnel.md -->

1. Navigieren Sie im Azure-Portal zur privaten Azure VMware Solution-Cloud. Wählen Sie **Verwalten** > **Konnektivität** > **ExpressRoute** und anschließend **+ Autorisierungsschlüssel anfordern** aus.

   :::image type="content" source="../media/expressroute-global-reach/start-request-authorization-key.png" alt-text="Screenshot: Anfordern eines ExpressRoute-Autorisierungsschlüssels" border="true" lightbox="../media/expressroute-global-reach/start-request-authorization-key.png":::

1. Geben Sie einen Namen für ihn an, und wählen Sie **Erstellen** aus.

   Die Erstellung des Schlüssels kann ungefähr 30 Sekunden dauern. Nach der Erstellung wird der neue Schlüssel in der Liste der Autorisierungsschlüssel für die private Cloud angezeigt.

   :::image type="content" source="../media/expressroute-global-reach/show-global-reach-auth-key.png" alt-text="Screenshot: ExpressRoute Global Reach-Autorisierungsschlüssel":::
  
1. Kopieren Sie den Autorisierungsschlüssel und die ExpressRoute-ID. Diese Angaben werden zum Abschließen des Peerings benötigt. Der Autorisierungsschlüssel wird nach einiger Zeit nicht mehr angezeigt. Kopieren Sie ihn daher, sobald er angezeigt wird.

