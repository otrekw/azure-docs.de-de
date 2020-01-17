---
title: Zuweisen von Verzeichnisrollen zu Benutzern – Azure Active Directory | Microsoft-Dokumentation
description: Anweisungen zum Zuweisen von Administrator- und anderen Rollen zu Benutzern mithilfe von Azure Active Directory.
services: active-directory
author: msaburnley
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: conceptual
ms.date: 09/06/2018
ms.author: ajburnle
ms.reviewer: jeffsta
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2df52969ea79e5d1af132aa82c2ec1ceedb92b82
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2019
ms.locfileid: "75422895"
---
# <a name="assign-administrator-and-non-administrator-roles-to-users-with-azure-active-directory"></a>Zuweisen von Administrator- und anderen Rollen zu Benutzern mithilfe von Azure Active Directory
Wenn für einen Benutzer in Ihrer Organisation die Berechtigung zum Verwalten von Azure AD-Ressourcen (Azure Active Directory) erforderlich ist, müssen Sie dem Benutzer eine geeignete Rolle in Azure AD zuweisen – basierend auf den Aktionen, für deren Durchführung der Benutzer die Berechtigung benötigt.

Weitere Informationen zu den verfügbaren Rollen finden Sie unter [Zuweisen von Administratorrollen in Azure AD](../users-groups-roles/directory-assign-admin-roles.md). Weitere Informationen zum Hinzufügen von Benutzern finden Sie unter [Hinzufügen neuer Benutzer zu Azure Active Directory](add-users-azure-active-directory.md).

## <a name="assign-roles"></a>Zuweisen von Rollen
Eine gängige Möglichkeit zum Zuweisen von Azure AD-Rollen zu einem Benutzer ist die Nutzung der Seite **Verzeichnisrolle** für einen Benutzer.

Sie können Rollen auch zuweisen, indem Sie Privileged Identity Management (PIM) verwenden. Ausführlichere Informationen zur Verwendung von PIM finden Sie unter [Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/privileged-identity-management).

### <a name="to-assign-a-role-to-a-user"></a>So weisen Sie einem Benutzer eine Rolle zu
1. Navigieren Sie zum [Azure-Portal](https://portal.azure.com/), und melden Sie sich mit dem Konto eines globalen Administrators für das Verzeichnis an. 

2. Suchen Sie nach **Azure Active Directory**, und wählen Sie diese Option aus.

      ![Suche im Azure-Portal nach Azure Active Directory](media/active-directory-users-assign-role-azure-portal/search-azure-active-directory.png)


3. Wählen Sie **Benutzer** aus.

4. Suchen Sie nach dem Benutzer, der die Rollenzuweisung erhält, und wählen Sie ihn aus. Beispielsweise _Alain Charon_.

      ![Seite „Alle Benutzer“ – Benutzer auswählen](media/active-directory-users-assign-role-azure-portal/directory-role-select-user.png)

5. Wählen Sie auf der Seite **Alain Charon – Profil** die Option **Zugewiesene Rollen** aus.

    Die Seite **Alain Charon – Verzeichnisrolle** wird angezeigt.

6. Wählen Sie **Zuweisung hinzufügen** aus, wählen Sie die Rolle aus, die Alain Charon zugewiesen werden soll (z. B. _Anwendungsadministrator_), und wählen Sie dann **Auswählen** aus.

    ![Seite „Zugewiesene Rollen“ mit der ausgewählten Rolle](media/active-directory-users-assign-role-azure-portal/directory-role-select-role.png)

    Die Rolle „Anwendungsadministrator“ wird Alain Charon zugewiesen und auf der Seite **Alain Charon – Verzeichnisrolle** angezeigt.

## <a name="remove-a-role-assignment"></a>Entfernen einer Rollenzuweisung
Wenn Sie die Rollenzuweisung für einen Benutzer entfernen möchten, können Sie ebenfalls die Seite **Alain Charon – Verzeichnisrolle** nutzen.

### <a name="to-remove-a-role-assignment-from-a-user"></a>So entfernen Sie eine Rollenzuweisung für einen Benutzer

1. Wählen Sie **Azure Active Directory** und **Benutzer**, und suchen Sie dann nach dem Benutzer, für den die Rollenzuweisung entfernt werden soll, und wählen Sie ihn aus. Beispielsweise _Alain Charon_.

2. Wählen Sie **Zugewiesene Rollen** aus, wählen Sie **Anwendungsadministrator** aus, und wählen Sie dann **Zuweisung entfernen** aus.

    ![Seite „Zugewiesene Rollen“ mit der ausgewählten Rolle und der Option zum Entfernen](media/active-directory-users-assign-role-azure-portal/directory-role-remove-role.png)

    Die Rolle „Anwendungsadministrator“ wird für Alain Charon entfernt und auf der Seite **Alain Charon – Verzeichnisrolle** nicht mehr angezeigt.

## <a name="next-steps"></a>Nächste Schritte
- [Hinzufügen oder Löschen von Benutzern](add-users-azure-active-directory.md)

- [Hinzufügen oder Ändern von Profilinformationen](active-directory-users-profile-azure-portal.md)

- [Hinzufügen von Gastbenutzern aus einem anderen Verzeichnis](../b2b/what-is-b2b.md)

Sie können auch andere Aufgaben für die Benutzerverwaltung ausführen, z.B. Delegate zuweisen, Richtlinien verwenden und Benutzerkonten freigeben. Weitere Informationen zu anderen verfügbaren Aktionen finden Sie unter [Dokumentation zur Azure Active Directory-Benutzerverwaltung](../users-groups-roles/index.yml).


