---
title: Aktivieren meiner Azure AD-Rollen in PIM – Azure Active Directory | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Azure AD-Rollen in Azure AD Privileged Identity Management (PIM) aktivieren.
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: how-to
ms.workload: identity
ms.subservice: pim
ms.date: 07/27/2021
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3926fb277251f7020539942c76d9c97f0ce46d75
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2021
ms.locfileid: "122339510"
---
# <a name="activate-my-azure-ad-roles-in-pim"></a>Aktivieren meiner Azure AD-Rollen in PIM

Azure Active Directory (Azure AD) Privileged Identity Management (PIM) vereinfacht die Art und Weise, in der Unternehmen den privilegierten Zugriff auf Ressourcen in Azure AD und anderen Microsoft-Onlinediensten wie Microsoft 365 oder Microsoft Intune verwalten.  

Falls Sie zu einer Administratorrolle *berechtigt* sind, müssen Sie die Rollenzuweisung *aktivieren*, wenn Sie privilegierte Aufgaben ausführen möchten. Wenn Sie beispielsweise gelegentlich Microsoft 365-Funktionen verwalten, werden Sie von den Administratoren für privilegierte Rollen Ihrer Organisation möglicherweise nicht als permanenter globaler Administrator festgelegt, da sich diese Rolle auch auf andere Dienste auswirkt. Stattdessen erteilen sie Ihnen Berechtigungen für Azure AD-Rollen (beispielsweise Exchange Online-Administrator). Sie können eine Aktivierung dieser Rolle anfordern, wenn Sie diese Berechtigungen benötigen, und verfügen damit für einen bestimmten Zeitraum über Administratorkontrolle.

Dieser Artikel richtet sich an Administratoren, die ihre Azure AD-Rolle in Privileged Identity Management aktivieren müssen.

## <a name="activate-a-role"></a>Aktivieren einer Rolle

Wenn Sie eine Azure AD-Rolle annehmen müssen, können Sie in Privileged Identity Management die Aktivierung anfordern, indem Sie die Option **Meine Rollen** öffnen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

1. Öffnen Sie **Azure AD Privileged Identity Management**. Informationen zum Hinzufügen der Privileged Identity Management-Kachel zu Ihrem Dashboard finden Sie unter [Einstieg in die Verwendung von PIM](pim-getting-started.md).

1. Wählen Sie **Meine Rollen** aus, und wählen Sie dann **Azure AD-Rollen** aus, um eine Liste der Azure AD-Rollen anzuzeigen, für die Sie berechtigt sind.

    ![Seite „Meine Rollen“ mit den Rollen, die aktiviert werden können](./media/pim-how-to-activate-role/my-roles.png)

1. Suchen Sie in der Liste **Azure AD-Rollen** die zu aktivierende Rolle.

    ![Azure AD-Rollen: Liste meiner berechtigten Rollen](./media/pim-how-to-activate-role/activate-link.png)

1. Wählen Sie **Aktivieren** aus, um den Bereich „Aktivieren“ zu öffnen.

    ![Azure AD Rollen: Aktivierungsseite mit Dauer und Bereich](./media/pim-how-to-activate-role/activate-page.png)

1. Wählen Sie **Zusätzliche Überprüfung erforderlich.** aus, und gehen Sie gemäß den Anweisungen vor, um die zusätzliche Sicherheitsüberprüfung zu durchlaufen. Sie müssen sich nur einmal pro Sitzung authentifizieren.

    ![Anzeige, die eine Sicherheitsüberprüfung wie z. B. einen PIN-Code verlangt](./media/pim-resource-roles-activate-your-roles/resources-mfa-enter-code.png)

1. Wählen Sie nach der mehrstufigen Authentifizierung die Option **Aktivieren** aus, bevor Sie den Vorgang fortsetzen.

    ![Bestätigen meiner Identität mit MFA vor der Rollenaktivierung](./media/pim-how-to-activate-role/activate-role-mfa-banner.png)

1. Wenn Sie einen reduzierten Bereich angeben möchten, wählen Sie **Bereich** aus, um den Filterbereich zu öffnen. Im Filterbereich können Sie die Azure AD-Ressourcen angeben, auf die Sie Zugriff benötigen. Es empfiehlt sich, Zugriff auf möglichst wenige benötigte Ressourcen anzufordern.

1. Falls erforderlich, geben Sie einen Startzeitpunkt für die Aktivierung an. Die Azure AD-Rolle wird dann nach dem ausgewählten Zeitpunkt aktiviert.

1. Geben Sie im Feld **Grund** den Grund für die Aktivierungsanforderung ein.

1. Wählen Sie **Aktivieren** aus.

    Wenn für die Aktivierung der [Rolle eine Genehmigung erforderlich ist](pim-resource-roles-approval-workflow.md), werden Sie über eine Benachrichtigung in der oberen rechten Ecke des Browsers darüber informiert, dass die Genehmigung der Anforderung aussteht.

    ![Ausstehende Genehmigung für die Aktivierungsanforderung](./media/pim-resource-roles-activate-your-roles/resources-my-roles-activate-notification.png)

## <a name="view-the-status-of-your-requests-for-new-version"></a>Anzeigen des Status Ihrer Anforderungen für die neue Version

Sie können den Status Ihrer ausstehenden Aktivierungsanforderungen anzeigen.

1. Öffnen Sie Azure AD Privileged Identity Management.

1. Wählen Sie **Meine Anforderungen** aus, um eine Liste mit Ihren Anforderungen von Azure AD-Rollen und Azure-Ressourcenrollen anzuzeigen.

    ![Meine Anforderungen: Azure AD-Seite mit Ihren ausstehenden Anforderungen](./media/pim-how-to-activate-role/my-requests-page.png)

1. Scrollen Sie nach rechts, um die Spalte **Anforderungsstatus** anzuzeigen.

## <a name="cancel-a-pending-request-for-new-version"></a>Abbrechen einer ausstehenden Anforderung für die neue Version

Sollten Sie die Aktivierung einer Rolle, für die eine Genehmigung erforderlich ist, nicht mehr benötigen, können Sie eine ausstehende Anforderung jederzeit abbrechen.

1. Öffnen Sie Azure AD Privileged Identity Management.

1. Wählen Sie **Meine Anforderungen** aus.

1. Wählen Sie für die Rolle, für die Sie eine ausstehende Anforderung abbrechen möchten, den Link **Abbrechen** aus.

    Durch das Auswählen von „Abbrechen“ wird die Anforderung abgebrochen. Um die Rolle erneut zu aktivieren, müssen Sie eine neue Anforderung zur Aktivierung übermitteln.

   ![Liste „Meine Anforderungen“ mit hervorgehobener Aktion „Abbrechen“](./media/pim-resource-roles-activate-your-roles/resources-my-requests-cancel.png)

## <a name="troubleshoot-for-new-version"></a>Problembehandlung bei der neuen Version

### <a name="permissions-are-not-granted-after-activating-a-role"></a>Keine Gewährung von Berechtigungen nach der Aktivierung einer Rolle

Wenn Sie in Privileged Identity Management eine Rolle aktivieren, wird die Aktivierung möglicherweise nicht sofort an alle Portale weitergegeben, für die diese privilegierte Rolle benötigt wird. In einigen Fällen kann die Web-Zwischenspeicherung im Portal auch dann dazu führen, dass die Änderung nicht sofort wirksam ist, wenn diese weitergegeben wurde. Wenn Ihre Aktivierung verzögert ist, melden Sie sich vom Portal ab, in dem Sie die Aktion durchführen möchten, und melden Sie sich neu an. Im Azure-Portal werden Sie automatisch von PIM (Privileged Identity Management) abgemeldet und wieder angemeldet.

## <a name="next-steps"></a>Nächste Schritte

- [Anzeigen des Überwachungsverlaufs für Azure AD-Rollen](pim-how-to-use-audit-log.md)
