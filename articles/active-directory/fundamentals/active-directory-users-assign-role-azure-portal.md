---
title: Zuweisen von Azure AD-Rollen zu Benutzern – Azure Active Directory | Microsoft-Dokumentation
description: Anweisungen zum Zuweisen von Administrator- und anderen Rollen zu Benutzern mithilfe von Azure Active Directory.
services: active-directory
author: ajburnle
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: how-to
ms.date: 08/31/2020
ms.author: ajburnle
ms.reviewer: jeffsta
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: fe0c7eec5c023fcba3152ad6329d318210a0c2ab
ms.sourcegitcommit: 28c5fdc3828316f45f7c20fc4de4b2c05a1c5548
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/22/2020
ms.locfileid: "92370932"
---
# <a name="assign-administrator-and-non-administrator-roles-to-users-with-azure-active-directory"></a>Zuweisen von Administrator- und anderen Rollen zu Benutzern mithilfe von Azure Active Directory

Wenn ein Benutzer in Azure Active Directory (Azure AD) die Berechtigung zum Verwalten von Azure AD-Ressourcen benötigt, müssen Sie ihm einer Rolle zuweisen, die die erforderlichen Berechtigungen bereitstellt. Informationen dazu, mit welchen Rollen Azure-Ressourcen und mit welchen Rollen Azure AD-Ressourcen verwaltet werden, finden Sie unter [Administratorrollen für klassische Abonnements, Azure-Rollen und Azure AD-Rollen](../../role-based-access-control/rbac-and-directory-admin-roles.md).

Weitere Informationen zu den verfügbaren Azure AD-Rollen finden Sie unter [Zuweisen von Administratorrollen in Azure Active Directory](../roles/permissions-reference.md). Informationen zum Hinzufügen von Benutzern finden Sie unter [Hinzufügen oder Löschen von Benutzern in Azure Active Directory](add-users-azure-active-directory.md).

## <a name="assign-roles"></a>Zuweisen von Rollen

Eine gängige Methode zum Zuweisen von Azure AD-Rollen zu einem Benutzer ist die Nutzung der Seite **Zugewiesene Rollen** für einen Benutzer. Sie können auch die Benutzerberechtigung zur Just-In-Time-Erhöhung für eine Rolle mit Privileged Identity Management (PIM) konfigurieren. Weitere Informationen zur Verwendung von PIM finden Sie unter [Privileged Identity Management](../privileged-identity-management/index.yml).

> [!Note]
> Wenn Sie über einen Azure AD Premium P2-Lizenzplan verfügen und PIM bereits nutzen, werden alle Rollenverwaltungsaufgaben in [Privileged Identity Management](../roles/manage-roles-portal.md) ausgeführt. Diese Funktion ist zurzeit auf das Zuweisen jeweils nur einer Rolle beschränkt. Sie können derzeit nicht mehrere Rollen auswählen und einem Benutzer gleichzeitig zuweisen.
>
> ![In PIM verwaltete Azure AD-Rollen für Benutzer, die PIM bereits nutzen und über eine Premium P2-Lizenz verfügen](./media/active-directory-users-assign-role-azure-portal/pim-manages-roles-for-p2.png)

## <a name="assign-a-role-to-a-user"></a>Zuweisen einer Rolle zu einem Benutzer

1. Navigieren Sie zum [Azure-Portal](https://portal.azure.com/), und melden Sie sich mit dem Konto eines globalen Administrators für das Verzeichnis an.

2. Suchen Sie nach **Azure Active Directory** , und wählen Sie diese Option aus.

      ![Suche im Azure-Portal nach Azure Active Directory](media/active-directory-users-assign-role-azure-portal/search-azure-active-directory.png)

3. Wählen Sie **Benutzer** aus.

4. Suchen Sie nach dem Benutzer, der die Rollenzuweisung erhält, und wählen Sie ihn aus. Beispielsweise _Alain Charon_ .

      ![Seite „Alle Benutzer“ – Benutzer auswählen](media/active-directory-users-assign-role-azure-portal/directory-role-select-user.png)

5. Wählen Sie auf der Seite **Alain Charon – Profil** die Option **Zugewiesene Rollen** aus.

    Die Seite **Alain Charon – Administratorrollen** wird angezeigt.

6. Wählen Sie **Zuweisung hinzufügen** und die Rolle aus, die Alain Charon zugewiesen werden soll (z. B. _Anwendungsadministrator_ ), und wählen Sie dann **Auswählen** aus.

    ![Seite „Zugewiesene Rollen“ mit der ausgewählten Rolle](media/active-directory-users-assign-role-azure-portal/directory-role-select-role.png)

    Die Rolle „Anwendungsadministrator“ wird Alain Charon zugewiesen und auf der Seite **Alain Charon – Administratorrollen** angezeigt.

## <a name="remove-a-role-assignment"></a>Entfernen einer Rollenzuweisung

Wenn Sie die Rollenzuweisung für einen Benutzer entfernen müssen, können Sie ebenfalls die Seite **Alain Charon – Administratorrollen** nutzen.

### <a name="to-remove-a-role-assignment-from-a-user"></a>So entfernen Sie eine Rollenzuweisung für einen Benutzer

1. Wählen Sie **Azure Active Directory** und **Benutzer** , und suchen Sie dann nach dem Benutzer, für den die Rollenzuweisung entfernt werden soll, und wählen Sie ihn aus. Beispielsweise _Alain Charon_ .

2. Wählen Sie **Zugewiesene Rollen** aus, wählen Sie **Anwendungsadministrator** aus, und wählen Sie dann **Zuweisung entfernen** aus.

    ![Seite „Zugewiesene Rollen“ mit der ausgewählten Rolle und der Option zum Entfernen](media/active-directory-users-assign-role-azure-portal/directory-role-remove-role.png)

    Die Rolle „Anwendungsadministrator“ wird für Alain Charon entfernt und nicht mehr auf der Seite **Alain Charon – Administratorrollen** angezeigt.

## <a name="next-steps"></a>Nächste Schritte

- [Hinzufügen oder Löschen von Benutzern](add-users-azure-active-directory.md)

- [Hinzufügen oder Ändern von Profilinformationen](active-directory-users-profile-azure-portal.md)

- [Hinzufügen von Gastbenutzern aus einem anderen Verzeichnis](../external-identities/what-is-b2b.md)

Informationen zu anderen Aufgaben der Benutzerverwaltung finden Sie unter [Dokumentation zur Azure Active Directory-Benutzerverwaltung](../enterprise-users/index.yml).