---
title: 'Hinzufügen und Auflisten von Rollen im Bereich von Verwaltungseinheiten: Azure Active Directory | Microsoft-Dokumentation'
description: Verwenden Sie Verwaltungseinheiten, um den Geltungsbereich von Rollenzuweisungen in Azure Active Directory einzuschränken.
services: active-directory
documentationcenter: ''
author: rolyon
manager: daveba
ms.service: active-directory
ms.topic: how-to
ms.subservice: roles
ms.workload: identity
ms.date: 11/04/2020
ms.author: rolyon
ms.reviewer: anandy
ms.custom: oldportal;it-pro;
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1fc0c4bf9f71a8fe7e8cf49b83d32ac594dbe062
ms.sourcegitcommit: 225e4b45844e845bc41d5c043587a61e6b6ce5ae
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/11/2021
ms.locfileid: "103011386"
---
# <a name="assign-scoped-roles-to-an-administrative-unit"></a>Zuweisen von bereichsbezogenen Rollen zu einer Verwaltungseinheit

Um in Azure Active Directory (Azure AD) eine präzisere administrative Steuerung zu ermöglichen, können Sie Benutzer einer Azure AD-Rolle mit einem auf einzelne oder mehrere Verwaltungseinheiten beschränkten Bereich zuweisen.

Informationen zur Vorbereitung auf den Einsatz von PowerShell und Microsoft Graph für die Verwaltung von Verwaltungseinheiten finden Sie unter [Erste Schritte](admin-units-manage.md#get-started).

## <a name="available-roles"></a>Verfügbare Rollen

Role  |  BESCHREIBUNG
----- |  -----------
Authentifizierungsadministrator  |  Hat nur in der zugewiesenen Verwaltungseinheit Zugriff zum Anzeigen, Festlegen und Zurücksetzen von Informationen zur Authentifizierungsmethode für alle Benutzer ohne Administratorrechte.
Gruppenadministrator  |  Kann nur in der zugewiesenen Verwaltungseinheit alle Aspekte von Gruppen und Gruppeneinstellungen wie Benennungs- und Ablaufrichtlinien verwalten.
Helpdeskadministrator  |  Kann nur in der zugewiesenen Verwaltungseinheit Kennwörter für Nicht-Administratoren und Helpdeskadministratoren zurücksetzen.
Lizenzadministrator  |  Kann nur in der Verwaltungseinheit Lizenzzuweisungen vornehmen, entfernen und aktualisieren.
Kennwortadministrator  |  Kann nur in der zugewiesenen Verwaltungseinheit Kennwörter für Nicht-Administratoren und Kennwortadministratoren zurücksetzen.
Benutzeradministrator  |  Kann nur in der zugewiesenen Verwaltungseinheit alle Aspekte von Benutzern und Gruppen verwalten, einschließlich der Kennwortzurücksetzung für Administratoren mit eingeschränkten Berechtigungen.

## <a name="security-principals-that-can-be-assigned-to-a-scoped-role"></a>Sicherheitsprinzipale, die einer bereichsbezogenen Rolle zugewiesen werden können

Die folgenden Sicherheitsprinzipale können einer Rolle im Bereich von Verwaltungseinheiten zugewiesen werden:

* Benutzer
* Cloudgruppen, die einer Rolle zugewiesen werden können (Vorschauversion)
* Dienstprinzipalname (Service Principal Name, SPN)

## <a name="assign-a-scoped-role"></a>Zuweisen einer bereichsbezogenen Rolle

Eine bereichsbezogene Rolle kann über das Azure-Portal, mithilfe von PowerShell oder per Microsoft Graph zugewiesen werden.

### <a name="use-the-azure-portal"></a>Verwenden des Azure-Portals

1. Navigieren Sie im Azure-Portal zu **Azure AD**.

1. Wählen Sie **Verwaltungseinheiten** und anschließend die Verwaltungseinheit aus, der Sie einen Benutzerrollenbereich zuweisen möchten. 

1. Wählen Sie im linken Bereich die Option **Rollen und Administratoren** aus, um alle verfügbaren Rollen aufzulisten.

   ![Screenshot: Bereich „Rollen und Administratoren“ zum Auswählen einer Verwaltungseinheit, deren Rollenbereich zugewiesen werden soll](./media/admin-units-assign-roles/select-role-to-scope.png)

1. Wählen Sie die zuzuweisende Rolle und anschließend **Zuweisungen hinzufügen** aus. 

1. Wählen Sie im Bereich **Zuweisungen hinzufügen** mindestens einen Benutzer aus, dem die Rolle zugewiesen werden soll.

   ![Auswählen der Rolle für den Bereich und dann Auswählen von „Zuweisungen hinzufügen“](./media/admin-units-assign-roles/select-add-assignment.png)

> [!Note]
> Informationen zum Zuweisen einer Rolle für eine Verwaltungseinheit unter Verwendung von Azure AD Privileged Identity Management (PIM) finden Sie unter [Zuweisen von Azure AD-Rollen in Privileged Identity Management](../privileged-identity-management/pim-how-to-add-role-to-user.md?tabs=new#assign-a-role-with-restricted-scope).

### <a name="use-powershell"></a>Verwenden von PowerShell

```powershell
$adminUser = Get-AzureADUser -ObjectId "Use the user's UPN, who would be an admin on this unit"
$role = Get-AzureADDirectoryRole | Where-Object -Property DisplayName -EQ -Value "User Account Administrator"
$adminUnitObj = Get-AzureADMSAdministrativeUnit -Filter "displayname eq 'The display name of the unit'"
$roleMember = New-Object -TypeName Microsoft.Open.AzureAD.Model.RoleMemberInfo
$roleMember.ObjectId = $adminUser.ObjectId
Add-AzureADMSScopedRoleMembership -ObjectId $adminUnitObj.ObjectId -RoleObjectId $role.ObjectId -RoleMemberInfo $roleMember
```

Der hervorgehobene Abschnitt kann nach Bedarf für die jeweilige Umgebung geändert werden.

### <a name="use-microsoft-graph"></a>Verwenden von Microsoft Graph

Anforderung

```http
POST /directory/administrativeUnits/{admin-unit-id}/scopedRoleMembers
```
    
Text

```http
{
  "roleId": "roleId-value",
  "roleMemberInfo": {
    "id": "id-value"
  }
}
```

## <a name="view-a-list-of-the-scoped-admins-in-an-administrative-unit"></a>Anzeigen einer Liste mit den bereichsbezogenen Administratoren in einer Verwaltungseinheit

Eine Liste mit bereichsbezogenen Administratoren kann über das Azure-Portal, mithilfe von PowerShell oder per Microsoft Graph angezeigt werden.

### <a name="use-the-azure-portal"></a>Verwenden des Azure-Portals

Alle Rollenzuweisungen, die mit einem auf Verwaltungseinheiten bezogenen Bereich erstellt wurden, können im Abschnitt [Verwaltungseinheiten von Azure AD](https://ms.portal.azure.com/?microsoft_aad_iam_adminunitprivatepreview=true&microsoft_aad_iam_rbacv2=true#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/AdminUnit) angezeigt werden. 

1. Navigieren Sie im Azure-Portal zu **Azure AD**.

1. Wählen Sie im linken Bereich die Option **Verwaltungseinheiten** und dann die Verwaltungseinheit für die Liste mit den Rollenzuweisungen aus, die Sie anzeigen möchten. 

1. Wählen Sie **Rollen und Administratoren** aus, und öffnen Sie anschließend eine Rolle, um die Zuweisungen in der Verwaltungseinheit anzuzeigen.

### <a name="use-powershell"></a>Verwenden von PowerShell

```powershell
$adminUnitObj = Get-AzureADMSAdministrativeUnit -Filter "displayname eq 'The display name of the unit'"
Get-AzureADMSScopedRoleMembership -ObjectId $adminUnitObj.ObjectId | fl *
```

Der hervorgehobene Abschnitt kann nach Bedarf für Ihre individuelle Umgebung geändert werden.

### <a name="use-microsoft-graph"></a>Verwenden von Microsoft Graph

Anforderung

```http
GET /directory/administrativeUnits/{admin-unit-id}/scopedRoleMembers
```

Body

```http
{}
```

## <a name="next-steps"></a>Nächste Schritte

- [Verwenden von Cloudgruppen zum Verwalten von Rollenzuweisungen](groups-concept.md)
- [Problembehandlung bei Rollen, die Cloudgruppen zugewiesen sind](groups-faq-troubleshooting.md)
