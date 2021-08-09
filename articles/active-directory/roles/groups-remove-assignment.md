---
title: Entfernen von Rollenzuweisungen für eine Gruppe in Azure Active Directory | Microsoft-Dokumentation
description: 'Vorschau: Benutzerdefinierte Azure AD-Rollen zur Delegierung der Identitätsverwaltung. Verwalten von Azure-Rollen im Azure-Portal, mit PowerShell oder über die Graph-API.'
services: active-directory
author: rolyon
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: roles
ms.topic: article
ms.date: 05/14/2021
ms.author: rolyon
ms.reviewer: vincesm
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: c56c6597e7ff2553089b62cabb84b24168b7cabf
ms.sourcegitcommit: 17345cc21e7b14e3e31cbf920f191875bf3c5914
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2021
ms.locfileid: "110085724"
---
# <a name="remove-role-assignments-from-a-group-in-azure-active-directory"></a>Entfernen von Rollenzuweisungen für eine Gruppe in Azure Active Directory

In diesem Artikel wird beschrieben, wie ein IT-Administrator Azure AD-Rollen entfernen kann, die Gruppen zugewiesen sind. Im Azure-Portal können Sie jetzt sowohl direkte als auch indirekte Rollenzuweisungen für einen Benutzer entfernen. Ist einem Benutzer durch eine Gruppenmitgliedschaft eine Rolle zugewiesen, entfernen Sie den Benutzer aus der Gruppe, um die Rollenzuweisung zu entfernen.

## <a name="prerequisites"></a>Voraussetzungen

- Eine Lizenz vom Typ Azure AD Premium P1 oder P2
- „Administrator für privilegierte Rollen“ oder „Globaler Administrator“
- AzureADPreview-Modul bei Verwendung von PowerShell
- Administratorzustimmung bei Verwendung von Graph-Tester für die Microsoft Graph-API

Weitere Informationen finden Sie unter [Voraussetzungen für die Verwendung von PowerShell oder Graph-Tester](prerequisites.md).

## <a name="azure-portal"></a>Azure-Portal

1. Melden Sie sich beim [Azure AD Admin Center](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview) an.

1. Wählen Sie **Rollen und Administratoren** > **_Rollenname_** aus.

1. Wählen Sie die Gruppe aus, für die Sie die Rollenzuweisung entfernen möchten, und wählen Sie **Zuweisung entfernen** aus.

   ![Entfernen einer Rollenzuweisung für eine ausgewählte Gruppe](./media/groups-remove-assignment/remove-assignment.png)

1. Wählen Sie **Ja** aus, wenn Sie zur Bestätigung der Aktion aufgefordert werden.

## <a name="powershell"></a>PowerShell

### <a name="create-a-group-that-can-be-assigned-to-role"></a>Erstellen einer Gruppe, die der Rolle zugewiesen werden kann

```powershell
$group = New-AzureADMSGroup -DisplayName "Contoso_Helpdesk_Administrators" -Description "This group is assigned to Helpdesk Administrator built-in role in Azure AD." -MailEnabled $true -SecurityEnabled $true -MailNickName "contosohelpdeskadministrators" -IsAssignableToRole $true
```

### <a name="get-the-role-definition-you-want-to-assign-the-group-to"></a>Abrufen der Rollendefinition, der Sie die Gruppe zuweisen möchten

```powershell
$roleDefinition = Get-AzureADMSRoleDefinition -Filter "displayName eq 'Helpdesk Administrator'"
```

### <a name="create-a-role-assignment"></a>Erstellen einer Rollenzuweisung

```powershell
$roleAssignment = New-AzureADMSRoleAssignment -ResourceScope '/' -RoleDefinitionId $roleDefinition.Id -PrincipalId $group.objectId
```

### <a name="remove-the-role-assignment"></a>Entfernen der Rollenzuweisung

```powershell
Remove-AzureAdMSRoleAssignment -Id $roleAssignment.Id 
```

## <a name="microsoft-graph-api"></a>Microsoft Graph-API

### <a name="create-a-group-that-can-be-assigned-an-azure-ad-role"></a>Erstellen einer Gruppe, die einer Azure AD-Rolle zugewiesen werden kann

```http
POST https://graph.microsoft.com/beta/groups
{
"description": "This group is assigned to Helpdesk Administrator built-in role of Azure AD",
"displayName": "Contoso_Helpdesk_Administrators",
"groupTypes": [
"Unified"
],
"mailEnabled": true,
"securityEnabled": true
"mailNickname": "contosohelpdeskadministrators",
"isAssignableToRole": true,
}
```

### <a name="get-the-role-definition"></a>Abrufen der Rollendefinition

```http
GET https://graph.microsoft.com/beta/roleManagement/directory/roleDefinitions?$filter=displayName+eq+'Helpdesk Administrator'
```

### <a name="create-the-role-assignment"></a>Erstellen der Rollenzuweisung

```http
POST https://graph.microsoft.com/beta/roleManagement/directory/roleAssignments
{
"principalId":"{object-id-of-group}",
"roleDefinitionId":"{role-definition-id}",
"directoryScopeId":"/"
}
```

### <a name="delete-role-assignment"></a>Löschen von Rollenzuweisungen

```http
DELETE https://graph.microsoft.com/beta/roleManagement/directory/roleAssignments/{role-assignment-id}
```

## <a name="next-steps"></a>Nächste Schritte

- [Verwenden von Cloudgruppen zum Verwalten von Rollenzuweisungen](groups-concept.md)
- [Problembehandlung bei Rollen, die Cloudgruppen zugewiesen sind](groups-faq-troubleshooting.md)
