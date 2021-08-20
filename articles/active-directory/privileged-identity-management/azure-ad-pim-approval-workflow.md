---
title: Genehmigen oder Ablehnen von Anforderungen für Azure AD-Rollen in PIM – Azure AD | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Anforderungen für Azure AD-Rollen in Azure AD Privileged Identity Management (PIM) genehmigen oder ablehnen.
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
editor: ''
ms.service: active-directory
ms.subservice: pim
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/03/2021
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1ffb9e53172835ddcec574802f10a4a4bbbea0e1
ms.sourcegitcommit: f3b930eeacdaebe5a5f25471bc10014a36e52e5e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/16/2021
ms.locfileid: "112233059"
---
# <a name="approve-or-deny-requests-for-azure-ad-roles-in-privileged-identity-management"></a>Genehmigen oder Ablehnen von Anforderungen für Azure AD-Rollen in PIM

Mit Azure Active Directory (Azure AD) Privileged Identity Management (PIM) können Sie Rollen konfigurieren, um die Genehmigung für die Aktivierung anzufordern, und einen oder mehrere Benutzer oder Gruppen als delegierte genehmigende Personen auswählen. Delegierte genehmigende Personen haben zum Genehmigen von Anforderungen 24 Stunden Zeit. Wenn eine Anforderung nicht innerhalb von 24 Stunden genehmigt wird, muss der berechtigte Benutzer eine neue Anforderung senden. Das 24-Stunden-Zeitfenster für die Genehmigung ist nicht konfigurierbar.

## <a name="view-pending-requests"></a>Anzeigen ausstehender Anforderungen

Als delegierte genehmigende Person erhalten Sie eine E-Mail-Benachrichtigung, wenn Ihre Genehmigung einer Anforderung für eine Azure AD-Rolle aussteht. Sie können diese ausstehenden Anforderungen in Privileged Identity Management anzeigen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

1. Öffnen Sie **Azure AD Privileged Identity Management**.

1. Wählen Sie **Anforderungen genehmigen** aus.

    ![Anforderungen genehmigen: Seite mit einer Anforderung zum Überprüfen von Azure AD-Rollen](./media/azure-ad-pim-approval-workflow/resources-approve-pane.png)

    Im Abschnitt **Anforderungen für Rollenaktivierungen** wird eine Liste der Anforderungen angezeigt, die Sie noch genehmigen müssen.

## <a name="approve-requests"></a>Genehmigen von Anforderungen

1. Suchen Sie nach der zu genehmigenden Anforderung, und wählen Sie diese aus. Daraufhin wird eine Seite zum Genehmigen oder Ablehnen von Anforderungen angezeigt.

    ![Screenshot: Seite „Anforderungen genehmigen – Azure AD-Rollen“](./media/azure-ad-pim-approval-workflow/resources-approve-pane.png)

1. Geben Sie im Feld **Begründung** die geschäftliche Begründung ein.

1. Wählen Sie **Genehmigen** aus. Sie erhalten eine Azure-Benachrichtigung über Ihre Genehmigung.

    ![Genehmigungsbenachrichtigung, dass die Anforderung genehmigt wurde](./media/pim-resource-roles-approval-workflow/resources-approve-pane.png))

## <a name="deny-requests"></a>Ablehnen von Anforderungen

1. Suchen Sie nach der Anforderung, die Sie ablehnen möchten, und wählen Sie diese aus. Daraufhin wird eine Seite zum Genehmigen oder Ablehnen von Anforderungen angezeigt.

    ![Anforderungen genehmigen: Bereich zum Genehmigen oder Ablehnen von Anforderungen mit Details und dem Feld „Begründung“](./media/pim-resource-roles-approval-workflow/resources-approve-pane.png)

1. Geben Sie im Feld **Begründung** die geschäftliche Begründung ein.

1. Wählen Sie **Ablehnen** aus. Es wird eine Benachrichtigung mit Ihrer Ablehnung angezeigt.

## <a name="workflow-notifications"></a>Workflowbenachrichtigungen

Nachfolgend finden Sie einige Informationen zu Workflowbenachrichtigungen:

- Die genehmigenden Personen werden per E-Mail benachrichtigt, wenn für die Anforderung einer Rolle die Überprüfung aussteht. E-Mail-Benachrichtigungen enthalten einen direkten Link zur Anforderung, damit diese von der genehmigenden Person genehmigt oder abgelehnt werden kann.
- Anforderungen werden von der ersten genehmigenden Person bearbeitet, die sich mit der Genehmigung oder Ablehnung befasst.
- Wenn eine genehmigende Person auf die Anforderung reagiert, werden alle genehmigenden Personen über die Aktion informiert.
- Globale Administratoren und Administratoren für privilegierte Rollen werden benachrichtigt, wenn ein genehmigter Benutzer in seiner Rolle aktiv wird.

>[!NOTE]
>Wenn ein globaler Administrator oder ein Administrator für privilegierte Rollen der Meinung ist, dass ein genehmigter Benutzer nicht aktiv sein sollte, kann er die aktive Rollenzuweisung in Privileged Identity Management entfernen. Administratoren werden nur über ausstehende Anforderungen benachrichtigt, wenn sie genehmigende Personen sind. Sie können aber ausstehende Anforderungen aller Benutzer anzeigen und abbrechen, indem sie die ausstehenden Anforderungen in Privileged Identity Management anzeigen.

## <a name="next-steps"></a>Nächste Schritte

- [E-Mail-Benachrichtigungen in Privileged Identity Management](pim-email-notifications.md)
- [Genehmigen oder Ablehnen von Anforderungen für Azure-Ressourcenrollen in PIM](pim-resource-roles-approval-workflow.md)
