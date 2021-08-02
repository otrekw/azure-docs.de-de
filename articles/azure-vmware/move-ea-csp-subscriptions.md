---
title: Verschieben des Abonnements für Azure VMware Solution in ein anderes Abonnement
description: In diesem Artikel wird beschrieben, wie Sie Azure VMware Solution Abonnement in ein anderes Abonnement verschieben. Sie können Ihre Ressourcen aus verschiedenen Gründen verschieben, z. B. zur Abrechnung.
ms.custom: subject-moving-resources
ms.topic: how-to
ms.date: 04/26/2021
ms.openlocfilehash: 02f35efe90d4670c4ab38155f129ec0d61c1b03d
ms.sourcegitcommit: c072eefdba1fc1f582005cdd549218863d1e149e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/10/2021
ms.locfileid: "111966697"
---
# <a name="move-azure-vmware-solution-subscription-to-another-subscription"></a>Verschieben des Abonnements für Azure VMware Solution in ein anderes Abonnement

In diesem Artikel wird beschrieben, wie Sie ein Azure VMware Solution Abonnement in ein anderes Abonnement verschieben. Möglicherweise verschieben Sie Ihr Abonnement aus verschiedenen Gründen, z. B. zur Abrechnung.

## <a name="prerequisites"></a>Voraussetzungen
Sie müssen für das **Quell-** und **Zielabonnement** mindestens über die Berechtigung „Mitwirkender“ verfügen. 

>[!IMPORTANT]
>Das virtuelle Netzwerk und dessen Gateway können nicht aus einem Abonnement in ein anderes verschoben werden. Darüber hinaus hat das Verschieben Ihrer Abonnements keine Auswirkungen auf Verwaltung und Workloads, z. B. auf die VMs für vCenter, NSX und Workloads.

## <a name="prepare-and-move"></a>Vorbereiten und Verschieben 

1. Wählen Sie im Azure-Portal die private Cloud aus, die Sie verschieben möchten.

   :::image type="content" source="media/move-subscriptions/source-subscription-id.png" alt-text="Screenshot, der die Übersichtsdetails der ausgewählten privaten Cloud zeigt.":::

1. Pingen Sie an einer Eingabeaufforderung die Komponenten und Workloads, um sicherzustellen, dass sie aus demselben Abonnement pingen.  

   :::image type="content" source="media/move-subscriptions/verify-components-workloads.png" alt-text="Screenshot, der den Pingbefehl und die Ergebnisse des Pings zeigt.":::

1. Wählen Sie den Link **Abonnement (ändern)** aus.

   :::image type="content" source="media/move-subscriptions/private-cloud-overview-subscription-id.png" alt-text="Screenshot der Details der privaten Cloud."::: 

1. Geben Sie für **Ziel** die Details des Abonnements an, und wählen Sie **Weiter** aus.

   :::image type="content" source="media/move-subscriptions/move-resources-subscription-target.png" alt-text="Screenshot der Zielressource.":::

1. Bestätigen Sie die Validierung der Ressourcen, die zum Verschieben ausgewählt wurden.  Dadurch werden alle ressourcen überprüft, die verschoben werden sollen. Während der Validierung der ausgewählten Ressourcen sehen Sie **Ausstehende Validierung** für den Validierungsstatus. 

   :::image type="content" source="media/move-subscriptions/pending-move-resources-subscription-target.png" alt-text="Screenshot der zu verschiebenden Ressourcen.":::

1. Nach erfolgreicher Validierung wählen Sie **Weiter**, um die Migration Ihrer privaten Cloud zu starten.

   :::image type="content" source="media/move-subscriptions/move-resources-succeeded.png" alt-text="Screenshot, der den Validierungsstatus &quot;Erfolgreich&quot; zeigt.":::

1. Aktivieren Sie das Kontrollkästchen, um anzugeben, dass Sie verstanden haben, dass die zugehörigen Tools und Skripts erst dann funktionieren, nachdem Sie sie für die neuen Ressourcen-IDs aktualisiert haben. Wählen Sie **Verschieben** aus.

   :::image type="content" source="media/move-subscriptions/review-move-resources-subscription-target.png" alt-text="Screenshot mit der Übersicht der ausgewählten verschobenen Ressource.":::

## <a name="verify-the-move"></a>Überprüfen der Verschiebung

Eine Benachrichtigung wird eingeblendet, sobald das Verschieben der Ressource abgeschlossen ist. 

:::image type="content" source="media/move-subscriptions/notification-move-resources-subscription-target.png" alt-text="Screenshot: Benachrichtigung nach dem Verschieben der Ressourcen.":::

Das neue Abonnement ist in der Übersicht über die private Cloud enthalten.

:::image type="content" source="media/move-subscriptions/moved-subscription-target.png" alt-text="Screenshot mit neuem Abonnement.":::

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen:

- [Regionsübergreifendes Verschieben von Azure VMware Solution](move-azure-vmware-solution-across-regions.md)
- [Leitfaden zum Verschieben von Netzwerkressourcen](../azure-resource-manager/management/move-limitations/networking-move-limitations.md)
- [Anleitung zum Verschieben virtueller Computer](../azure-resource-manager/management/move-limitations/virtual-machines-move-limitations.md)
- [Anleitung zum Verschieben von App Service-Ressourcen](../azure-resource-manager/management/move-limitations/app-service-move-limitations.md)