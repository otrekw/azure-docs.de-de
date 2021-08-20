---
title: Anfordern eines Autorisierungsschlüssels für ExpressRoute
description: Schritte zum Anfordern eines Autorisierungsschlüssels für ExpressRoute
ms.topic: include
ms.date: 03/15/2021
author: shortpatti
ms.author: v-patsho
ms.service: azure-vmware
ms.openlocfilehash: 5f4c436a8559a4b579a3a0c9bbfbe79bcb50764e
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2021
ms.locfileid: "121729045"
---
<!-- used in tutorial-expressroute-global-reach-private-cloud.md and create-ipsec-tunnel.md -->

1. Navigieren Sie im Azure-Portal zur privaten Azure VMware Solution-Cloud. Wählen Sie **Verwalten** > **Konnektivität** > **ExpressRoute** und anschließend **+ Autorisierungsschlüssel anfordern** aus.

   :::image type="content" source="../media/expressroute-global-reach/start-request-authorization-key.png" alt-text="Screenshot: Anfordern eines ExpressRoute-Autorisierungsschlüssels" border="true" lightbox="../media/expressroute-global-reach/start-request-authorization-key.png":::

1. Geben Sie einen Namen für ihn an, und wählen Sie **Erstellen** aus.

   Die Erstellung des Schlüssels kann ungefähr 30 Sekunden dauern. Nach der Erstellung wird der neue Schlüssel in der Liste der Autorisierungsschlüssel für die private Cloud angezeigt.

   :::image type="content" source="../media/expressroute-global-reach/show-global-reach-auth-key.png" alt-text="Screenshot: ExpressRoute Global Reach-Autorisierungsschlüssel" lightbox="../media/expressroute-global-reach/show-global-reach-auth-key.png":::
  
1. Kopieren Sie den Autorisierungsschlüssel und die ExpressRoute-ID. Diese Angaben werden zum Abschließen des Peerings benötigt. Der Autorisierungsschlüssel wird nach einiger Zeit nicht mehr angezeigt. Kopieren Sie ihn daher, sobald er angezeigt wird.

