---
title: Verwalten von Abonnements
description: Abonnements bestehen aus verwalteten, bestätigten Geräten und können je nach Bedarf on- oder offboarded werden.
ms.date: 3/30/2021
ms.topic: how-to
ms.openlocfilehash: 10098ec66d15b4c894106cc1d37d7ac339d508cd
ms.sourcegitcommit: 77d7639e83c6d8eb6c2ce805b6130ff9c73e5d29
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/05/2021
ms.locfileid: "106387161"
---
# <a name="manage-a-subscription"></a>Verwalten eines Abonnements

Abonnements werden auf monatlicher Basis verwaltet. Beim Onboarding eines Abonnements wird Ihnen die Nutzung dieses Abonnements bis zum Ende des Monats in Rechnung gestellt. Wenn Sie ein Abonnement offboarden, wird Ihnen entsprechend der restliche Monat in Rechnung gestellt, in dem Sie für das Abonnement das Offboarding durchgeführt haben.

## <a name="onboard-a-subscription"></a>Onboarding für ein Abonnement durchführen

Für den Einstieg in Azure Defender für IOT benötigen Sie ein Microsoft Azure-Abonnement. Wenn Sie nicht über ein Abonnement verfügen, können Sie sich für ein kostenloses Testkonto registrieren. Wenn Sie bereits Zugriff auf ein Azure-Abonnement haben, dies aber nicht aufgeführt ist, überprüfen Sie Ihre Kontodetails, und lassen Sie Ihre Berechtigungen vom Abonnementbesitzer bestätigen.

Um ein Onboarding für ein Abonnement durchzuführen:

1. Navigieren Sie zur Preisgestaltungsseite des Azure-Portals. 

   :::image type="content" source="media/how-to-manage-subscriptions/no-subscription.png" alt-text="Navigieren Sie zur Preisgestaltungsseite des Azure-Portals.":::

1. Klicken Sie auf **Onboard subscription** (Onboarding für Abonnement durchführen).

1. Wählen Sie im Fenster **Onboard-Abonnement** (Onboarding für Abonnement durchführen) Ihr Abonnement und die Anzahl der bestätigten Geräte aus den Dropdown-Menüs aus. 

   :::image type="content" source="media/how-to-manage-subscriptions/onboard-subscription.png" alt-text="Wählen Sie Ihr Abonnement aus, sowie die Anzahl der bestätigten Geräte.":::

1. Wählen Sie **Onboarding** aus.

## <a name="offboard-a-subscription"></a>Offboarding eines Abonnements

Abonnements werden auf monatlicher Basis verwaltet. Beim Offboarding eines Abonnements wird Ihnen die Nutzung dieses Abonnements bis zum Ende des Monats in Rechnung gestellt.

Deinstallieren Sie vor dem Offboarding des Abonnements alle Sensoren, die dem Abonnement zugeordnet sind. Weitere Informationen zum Löschen eines Sensors finden Sie unter [Löschen eines Sensors](how-to-manage-sensors-on-the-cloud.md#delete-a-sensor). 

Offboarding eines Abonnements:

1. Navigieren Sie zur Seite **Preise**.
1. Wählen Sie das Abonnement aus, und klicken Sie dann auf das **Löschsymbol** :::image type="icon" source="media/how-to-manage-sensors-on-the-cloud/delete-icon.png" border="false":::.
1. Aktivieren Sie im Bestätigungspopupfenster das Kontrollkästchen, um zu bestätigen, dass Sie alle dem Abonnement zugeordneten Sensoren gelöscht haben.

    :::image type="content" source="media/how-to-manage-sensors-on-the-cloud/offboard-popup.png" alt-text="Aktivieren des Kontrollkästchens und Klicken auf „Offboard“ für das Offboarding des Sensors":::

1. Klicken Sie auf die Schaltfläche **Offboard**. 

## <a name="next-steps"></a>Nächste Schritte

[Aktivieren und Einrichten der lokalen Verwaltungskonsole](how-to-activate-and-set-up-your-on-premises-management-console.md)
