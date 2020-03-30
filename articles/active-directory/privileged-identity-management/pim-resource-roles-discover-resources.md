---
title: Ermitteln von Azure-Ressourcen zur Verwaltung in PIM – Azure AD | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Azure-Ressourcen zur Verwaltung in Azure AD Privileged Identity Management (PIM) ermitteln.
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: pim
ms.date: 11/08/2019
ms.author: curtand
ms.collection: M365-identity-device-management
ms.openlocfilehash: 87da43100c7494937ddc842e0f903ba3a360959e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "74022893"
---
# <a name="discover-azure-resources-to-manage-in-privileged-identity-management"></a>Ermitteln von Azure-Ressourcen zur Verwaltung in PIM

Mit Azure Active Directory (Azure AD) Privileged Identity Management (PIM) können Sie den Schutz Ihrer Azure-Ressourcen verbessern. Dies ist hilfreich für Organisationen, die Privileged Identity Management bereits zum Schutz von Azure AD-Rollen verwenden, sowie für Verwaltungsgruppen- und Abonnementbesitzer, die Produktionsressourcen schützen möchten.

Beim erstmaligen Einrichten von Privileged Identity Management für Azure-Ressourcen müssen Sie die Ressourcen ermitteln und auswählen, die mit Privileged Identity Management geschützt werden sollen. Die Anzahl der Ressourcen, die Sie mit Privileged Identity Management verwalten können, ist nicht beschränkt. Es empfiehlt sich aber, mit Ihren wichtigsten Ressourcen (für die Produktion) zu beginnen.

## <a name="discover-resources"></a>Ermitteln von Ressourcen

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

1. Öffnen Sie **Azure AD Privileged Identity Management**.

1. Wählen Sie **Azure-Ressourcen** aus.

    Falls Sie Privileged Identity Management für Azure-Ressourcen zum ersten Mal verwenden, wird der Bereich **Ressourcen ermitteln** angezeigt.

    ![Bereich „Ressourcen ermitteln“ ohne aufgeführte Ressourcen bei der ersten Verwendung](./media/pim-resource-roles-discover-resources/discover-resources-first-run.png)

    Wenn ein anderer Administrator in Ihrer Organisation bereits Azure-Ressourcen in Privileged Identity Management verwaltet, sehen Sie eine Liste der Ressourcen, die derzeit verwaltet werden.

    ![Bereich „Ressourcen ermitteln“ mit einer Auflistung der derzeit verwalteten Ressourcen](./media/pim-resource-roles-discover-resources/discover-resources.png)

1. Wählen Sie **Ressourcen ermitteln** aus, um den Ermittlungsvorgang zu starten.

    ![Bereich „Ermittlung“ mit einer Auflistung von verwaltbaren Ressourcen wie Abonnements und Verwaltungsgruppen](./media/pim-resource-roles-discover-resources/discovery-pane.png)

1. Verwenden Sie auf der Seite **Ermittlung** **Ressourcenzustandsfilter** und **Ressourcentyp auswählen**, um die Verwaltungsgruppen oder Abonnements zu filtern, für die Sie Schreibberechtigungen besitzen. An einfachsten ist es wohl, mit **Alle** zu starten.

    Sie können nur Verwaltungsgruppen oder Abonnementressourcen suchen und auswählen, um sie mithilfe von Privileged Identity Management zu verwalten. Wenn Sie eine Verwaltungsgruppe oder ein Abonnement in Privileged Identity Management verwalten, können Sie auch deren untergeordnete Ressourcen verwalten.

1. Aktivieren Sie das Kontrollkästchen neben nicht verwalteten Ressourcen, die Sie verwalten möchten.

1. Wählen Sie **Ressource verwalten** aus, um mit dem Verwalten der ausgewählten Ressourcen zu beginnen.

    > [!NOTE]
    > Sobald eine Verwaltungsgruppe oder ein Abonnement verwaltet wird, kann dies nicht mehr rückgängig gemacht werden. Dadurch wird verhindert, dass andere Ressourcenadministratoren Privileged Identity Management-Einstellungen entfernen.

    ![Bereich „Ermittlung“, in dem eine Ressource ausgewählt und die Option „Ressource verwalten“ hervorgehoben ist](./media/pim-resource-roles-discover-resources/discovery-manage-resource.png)

1. Wenn eine Meldung angezeigt wird, in der Sie das Onboarding der ausgewählten Ressource zum Verwalten bestätigen sollen, wählen Sie **Ja** aus.

    ![Meldung zur Bestätigung des Onboardings der ausgewählten Ressourcen zur Verwaltung](./media/pim-resource-roles-discover-resources/discovery-manage-resource-message.png)

## <a name="next-steps"></a>Nächste Schritte

- [Konfigurieren von Einstellungen für Azure-Ressourcenrollen in Privileged Identity Management](pim-resource-roles-configure-role-settings.md)
- [Zuweisen von Azure-Ressourcenrollen in PIM](pim-resource-roles-assign-roles.md)
