---
title: Hinzufügen und Auflisten von Rollen im Bereich von Verwaltungseinheiten (Vorschau) – Azure Active Directory | Microsoft-Dokumentation
description: Verwenden Sie Verwaltungseinheiten, um den Bereich der Rollenzuweisungen in Azure Active Directory einzuschränken
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
ms.service: active-directory
ms.topic: how-to
ms.subservice: users-groups-roles
ms.workload: identity
ms.date: 04/16/2020
ms.author: curtand
ms.reviewer: anandy
ms.custom: oldportal;it-pro;
ms.collection: M365-identity-device-management
ms.openlocfilehash: 578fb481ec858e65ede49bdce2d8bc26470aa2ca
ms.sourcegitcommit: cec9676ec235ff798d2a5cad6ee45f98a421837b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85850773"
---
# <a name="assign-scoped-roles-to-an-administrative-unit"></a>Zuweisen von bereichsbezogenen Rollen zu einer Verwaltungseinheit

In Azure Active Directory (Azure AD) können Sie Benutzer einer Azure AD-Rolle zuweisen und den Geltungsbereich auf eine oder mehrere Verwaltungseinheiten (VEs) beschränken, um eine präzisere administrative Steuerung zu erhalten.

Die Schritte zum Vorbereiten auf den Einsatz von PowerShell und Microsoft Graph für die Verwaltung von Verwaltungseinheiten sind unter [Erste Schritte](roles-admin-units-manage.md#get-started) beschrieben.

## <a name="roles-available"></a>Verfügbare Rollen

Role  |  BESCHREIBUNG
----- |  -----------
Authentifizierungsadministrator  |  Hat nur in der zugewiesenen Verwaltungseinheit Zugriff zum Anzeigen, Festlegen und Zurücksetzen von Informationen zur Authentifizierungsmethode für alle Benutzer ohne Administratorrechte.
Gruppenadministrator  |  Kann nur in der zugewiesenen Verwaltungseinheit alle Aspekte von Gruppen und Gruppeneinstellungen wie Benennungs- und Ablaufrichtlinien verwalten.
Helpdeskadministrator  |  Kann nur in der zugewiesenen Verwaltungseinheit Kennwörter für Nicht-Administratoren und Helpdeskadministratoren zurücksetzen.
Lizenzadministrator  |  Kann nur in der Verwaltungseinheit Lizenzzuweisungen vornehmen, entfernen und aktualisieren.
Kennwortadministrator  |  Kann nur in der zugewiesenen Verwaltungseinheit Kennwörter für Nicht-Administratoren und Kennwortadministratoren zurücksetzen.
Benutzeradministrator  |  Kann nur in der zugewiesenen Verwaltungseinheit alle Aspekte von Benutzern und Gruppen verwalten, einschließlich der Kennwortzurücksetzung für Administratoren mit eingeschränkten Berechtigungen.

## <a name="assign-a-scoped-role"></a>Zuweisen einer bereichsbezogenen Rolle

### <a name="azure-portal"></a>Azure-Portal

Wechseln Sie im Portal zu **Azure AD > Verwaltungseinheiten**. Wählen Sie die Verwaltungseinheit aus, über die Sie die Rolle einem Benutzer zuweisen möchten. Wählen Sie im linken Bereich „Rollen und Administratoren“ aus, um alle verfügbaren Rollen aufzulisten.

![Auswählen einer Verwaltungseinheit zum Ändern des Rollenbereichs](./media/roles-admin-units-assign-roles/select-role-to-scope.png)

Wählen Sie die zuzuweisende Rolle und dann **Zuweisungen hinzufügen** aus. Dadurch wird ein Bereich auf der rechten Seite eingeblendet, in dem Sie einen oder mehrere Benutzer auswählen können, die der Rolle zugewiesen werden sollen.

![Auswählen der Rolle für den Bereich und dann Auswählen von „Zuweisungen hinzufügen“](./media/roles-admin-units-assign-roles/select-add-assignment.png)

### <a name="powershell"></a>PowerShell

```powershell
$administrative = Get-AzureADAdministrativeUnit -Filter "displayname eq 'Test administrative unit 2'"
$AdminUser = Get-AzureADUser -ObjectId 'janedoe@fabidentity.onmicrosoft.com'
$uaRoleMemberInfo = New-Object -TypeName Microsoft.Open.AzureAD.Model.RoleMemberInfo -Property @{ObjectId = $AdminUser.ObjectId}
Add-AzureADScopedRoleMembership -RoleObjectId $UserAdminRole.ObjectId -ObjectId $administrative unitObj.ObjectId -RoleMemberInfo  $uaRoleMemberInfo
```

Der markierte Abschnitt kann nach Bedarf für die jeweilige Umgebung geändert werden.

### <a name="microsoft-graph"></a>Microsoft Graph

```http
Http request
POST /administrativeUnits/{id}/scopedRoleMembers
    
Request body
{
  "roleId": "roleId-value",
  "roleMemberInfo": {
    "id": "id-value"
  }
}
```

## <a name="list-the-scoped-admins-on-an-au"></a>Auflisten der bereichsbezogenen Administratoren in einer Verwaltungseinheit

### <a name="azure-portal"></a>Azure-Portal

Alle Rollenzuweisungen, die auf Verwaltungseinheiten bezogen sind, können im Abschnitt [Verwaltungseinheiten von Azure AD](https://ms.portal.azure.com/?microsoft_aad_iam_adminunitprivatepreview=true&microsoft_aad_iam_rbacv2=true#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/AdminUnit) angezeigt werden. Wechseln Sie im Portal zu **Azure AD > Verwaltungseinheiten**. Wählen Sie die Verwaltungseinheit für die Rollenzuweisungen aus, die Sie auflisten möchten. Wählen Sie **Rollen und Administratoren** aus, und öffnen Sie eine Rolle, um die Zuweisungen in der Verwaltungseinheit anzuzeigen.

### <a name="powershell"></a>PowerShell

```powershell
$administrative unitObj = Get-AzureADAdministrativeUnit -Filter "displayname eq 'Test administrative unit 2'"
Get-AzureADScopedRoleMembership -ObjectId $administrative unitObj.ObjectId | fl *
```

Der markierte Abschnitt kann nach Bedarf für die jeweilige Umgebung geändert werden.

### <a name="microsoft-graph"></a>Microsoft Graph

```http
Http request
GET /administrativeUnits/{id}/scopedRoleMembers
Request body
{}
```

## <a name="next-steps"></a>Nächste Schritte

- [Problembehandlung und häufig gestellte Fragen im Zusammenhang mit Verwaltungseinheiten](roles-admin-units-faq-troubleshoot.md)
