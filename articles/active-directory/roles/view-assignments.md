---
title: Auflisten von Azure AD-Rollenzuweisungen
description: Sie können jetzt Mitglieder einer Azure Active Directory-Administratorrolle im Azure Active Directory Admin Center anzeigen und verwalten.
services: active-directory
author: rolyon
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: roles
ms.topic: how-to
ms.date: 05/14/2021
ms.author: rolyon
ms.reviewer: vincesm
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: f8fe57b5b6df03e135bf77e9064e14f376c57ac9
ms.sourcegitcommit: 7f59e3b79a12395d37d569c250285a15df7a1077
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/02/2021
ms.locfileid: "110796356"
---
# <a name="list-azure-ad-role-assignments"></a>Auflisten von Azure AD-Rollenzuweisungen

In diesem Artikel wird beschrieben, wie Sie Rollen auflisten, die Sie in Azure Active Directory (Azure AD) zugewiesen haben. In Azure Active Directory (Azure AD) können Rollen im organisationsweiten Bereich oder mit einem Einzelanwendungsbereich zugewiesen werden.

- Rollenzuweisungen im organisationsweiten Bereich werden der Liste der Rollenzuweisungen einzelner Anwendungen hinzugefügt und werden dort angezeigt.
- Rollenzuweisungen im Einzelanwendungsbereich werden der Liste der Zuweisungen im organisationsweiten Bereich nicht hinzugefügt und dort nicht angezeigt.

## <a name="prerequisites"></a>Voraussetzungen

- AzureADPreview-Modul bei Verwendung von PowerShell
- Administratorzustimmung bei Verwendung von Graph-Tester für die Microsoft Graph-API

Weitere Informationen finden Sie unter [Voraussetzungen für die Verwendung von PowerShell oder Graph-Tester](prerequisites.md).

## <a name="list-role-assignments-in-the-azure-portal"></a>Auflisten von Rollenzuweisungen im Azure-Portal

In diesem Verfahren wird beschrieben, wie organisationsweit geltende Rollenzuweisungen aufgelistet werden.

1. Melden Sie sich beim [Azure AD Admin Center](https://aad.portal.azure.com) an.
1. Wählen Sie **Azure Active Directory** > **Rollen und Administratoren** und anschließend eine Rolle aus, um sie zu öffnen und ihre Eigenschaften anzuzeigen.
1. Wählen Sie **Zuweisungen** aus, um die Rollenzuweisungen aufzulisten.

    ![Auflisten von Rollenzuweisungen und Berechtigungen beim Öffnen einer Rolle in der Liste](./media/view-assignments/role-assignments.png)

## <a name="list-my-role-assignments"></a>Auflisten der eigenen Rollenzuweisungen

Sie können auch Ihre eigenen Berechtigungen ohne großen Aufwand auflisten. Wählen Sie auf der Seite **Rollen und Administratoren** die Option **Ihre Rolle**, um die Rollen anzuzeigen, die Ihnen derzeit zugewiesen sind.

## <a name="download-role-assignments"></a>Herunterladen von Rollenzuweisungen

Um alle Zuweisungen für eine bestimmte Rolle herunterzuladen, wählen Sie auf der Seite **Rollen und Administratoren** eine Rolle aus, und klicken Sie dann auf **Rollenzuweisungen herunterladen**. Es wird eine CSV-Datei heruntergeladen, die Zuweisungen in allen Bereichen für diese Rolle auflistet.

![Herunterladen sämtlicher Zuweisungen für eine Rolle](./media/view-assignments/download-role-assignments.png)

## <a name="list-role-assignments-using-powershell"></a>Auflisten von Rollenzuweisungen mithilfe von PowerShell

In diesem Abschnitt wird das Anzeigen von Zuweisungen einer Rolle mit einem organisationsweiten Bereich beschrieben. Dieser Artikel verwendet das [Azure Active Directory PowerShell Version 2](/powershell/module/azuread/#directory_roles)-Modul. Zum Anzeigen von Zuweisungen im Einzelanwendungsbereich mithilfe von PowerShell können Sie die Cmdlets in [Zuweisen benutzerdefinierter Rollen mit Ressourcengeltungsbereich unter Verwendung von PowerShell in Azure Active Directory](custom-assign-powershell.md) verwenden.

Beispiel für das Auflisten der Rollenzuweisungen.

``` PowerShell
# Fetch list of all directory roles with object ID
Get-AzureADDirectoryRole

# Fetch a specific directory role by ID
$role = Get-AzureADDirectoryRole -ObjectId "5b3fe201-fa8b-4144-b6f1-875829ff7543"

# Fetch role membership for a role
Get-AzureADDirectoryRoleMember -ObjectId $role.ObjectId | Get-AzureADUser
```

## <a name="list-role-assignments-using-the-microsoft-graph-api"></a>Auflisten von Rollenzuweisungen mithilfe der Microsoft Graph-API

In diesem Abschnitt wird beschrieben, wie organisationsweit geltende Rollenzuweisungen aufgelistet werden.  Zum Auflisten von Zuweisungen im Einzelanwendungsbereich mithilfe der Graph-API können Sie die Vorgänge in [Zuweisen von benutzerdefinierten Administratorrollen mithilfe der Graph-API in Azure Active Directory](custom-assign-graph.md) verwenden.

HTTP-Anforderung zum Abrufen einer Rollenzuweisung für eine bestimmte Rollendefinition

GET

``` HTTP
https://graph.microsoft.com/beta/roleManagement/directory/roleAssignments&$filter=roleDefinitionId eq ‘<object-id-or-template-id-of-role-definition>’
```

Antwort

``` HTTP
HTTP/1.1 200 OK
{
    "id":"CtRxNqwabEKgwaOCHr2CGJIiSDKQoTVJrLE9etXyrY0-1"
    "principalId":"ab2e1023-bddc-4038-9ac1-ad4843e7e539",
    "roleDefinitionId":"3671d40a-1aac-426c-a0c1-a3821ebd8218",
    "resourceScopes":["/"]
}
```

## <a name="list-role-assignments-with-single-application-scope"></a>Auflisten von Rollenzuweisungen im Einzelanwendungsbereich

In diesem Abschnitt wird beschrieben, wie Rollenzuweisungen im Einzelanwendungsbereich aufgelistet werden. Dieses Feature ist zurzeit als öffentliche Preview verfügbar.

1. Melden Sie sich beim [Azure AD Admin Center](https://aad.portal.azure.com) an.
1. Wählen Sie **App-Registrierungen** und dann die gewünschte App-Registrierung aus, um ihre Eigenschaften anzuzeigen. Möglicherweise müssen Sie die Option **Alle Anwendungen** auswählen, um die vollständige Liste der App-Registrierungen in ihrer Azure AD-Organisation anzuzeigen.

    ![Erstellen oder Bearbeiten von App-Registrierungen auf der Seite „App-Registrierungen“](./media/view-assignments/app-reg-all-apps.png)

1. Wählen Sie in der App-Registrierung **Rollen und Administratoren** und anschließend eine Rolle aus, um ihre Eigenschaften anzuzeigen.

    ![Auflisten der Rollenzuweisungen einer App-Registrierung auf der Seite „App-Registrierungen“](./media/view-assignments/app-reg-assignments.png)

1. Wählen Sie **Zuweisungen** aus, um die Rollenzuweisungen aufzulisten. Wenn Sie die Seite „Zuweisungen“ in der App-Registrierung öffnen, werden die Rollenzuweisungen angezeigt, die auf diese Azure AD-Ressource beschränkt sind.

    ![Auflisten der Rollenzuweisungen einer App-Registrierung über die Eigenschaften einer App-Registrierung](./media/view-assignments/app-reg-assignments-2.png)

## <a name="next-steps"></a>Nächste Schritte

* Im [Forum für Azure AD-Administratorrollen](https://feedback.azure.com/forums/169401-azure-active-directory?category_id=166032) können Sie sich gerne mit uns in Verbindung setzen.
* Weitere Informationen zu Rollenberechtigungen finden Sie unter [Integrierte Rollen in Azure AD](permissions-reference.md).
* Informationen zu Standardbenutzerberechtigungen finden Sie unter [Vergleich von Standardbenutzerberechtigungen für Gäste und Mitglieder](../fundamentals/users-default-permissions.md).
