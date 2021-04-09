---
title: Verschieben von EA- und CSP-Abonnements für Azure VMware Solution
description: Erfahren Sie, wie Sie die private Cloud aus einem Abonnement in ein anderes verschieben. Das Verschieben kann aus verschiedenen Gründen erfolgen, wie z. B. Abrechnung.
ms.topic: how-to
ms.date: 03/15/2021
ms.openlocfilehash: 608f46dbd84d6bb899a3e7fcd1f8a63b3a5e85fb
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "103555133"
---
# <a name="move-ea-and-csp-azure-vmware-solution-subscriptions"></a>Verschieben von EA- und CSP-Abonnements für Azure VMware Solution

In diesem Artikel erfahren Sie, wie Sie die private Cloud aus einem Abonnement in ein anderes verschieben. Das Verschieben kann aus verschiedenen Gründen erfolgen, wie z. B. Abrechnung. 

>[!IMPORTANT]
>Sie müssen für das Quell- und Zielabonnement mindestens über die Berechtigung „Mitwirkender“ verfügen. Das virtuelle Netzwerk und dessen Gateway können nicht aus einem Abonnement in ein anderes verschoben werden. Darüber hinaus hat das Verschieben Ihrer Abonnements keine Auswirkungen auf Verwaltung und Workloads, z. B. auf die VMs für vCenter, NSX und Workloads.

1. Melden Sie sich beim Azure-Portal an, und wählen Sie die private Cloud aus, die Sie verschieben möchten.

1. Wählen Sie den Link **Abonnement (ändern)** aus.

   :::image type="content" source="media/private-cloud-overview-subscription-id.png" alt-text="Screenshot der Details der privaten Cloud.":::

1. Geben Sie für **Ziel** die Details des Abonnements an, und wählen Sie **Weiter** aus.

   :::image type="content" source="media/move-resources-subscription-target.png" alt-text="Screenshot der Zielressource." lightbox="media/move-resources-subscription-target.png":::

1. Bestätigen Sie die zum Verschieben ausgewählten Ressourcen, und wählen Sie **Weiter** aus. 

   :::image type="content" source="media/confirm-move-resources-subscription-target.png" alt-text="Screenshot der zu verschiebenden Ressourcen." lightbox="media/confirm-move-resources-subscription-target.png":::

1. Aktivieren Sie das Kontrollkästchen, um anzugeben, dass Sie verstanden haben, dass die zugehörigen Tools und Skripts erst dann funktionieren, nachdem Sie sie für die neuen Ressourcen-IDs aktualisiert haben. Wählen Sie **Verschieben** aus.

   :::image type="content" source="media/review-move-resources-subscription-target.png" alt-text="Screenshot mit Übersicht über die ausgewählte verschobene Ressource." lightbox="media/review-move-resources-subscription-target.png":::

   Eine Benachrichtigung wird eingeblendet, sobald das Verschieben der Ressource abgeschlossen ist. Das neue Abonnement ist in der Übersicht über die private Cloud enthalten.

   :::image type="content" source="media/moved-subscription-target.png" alt-text="Screenshot mit neuem Abonnement." lightbox="media/moved-subscription-target.png":::

