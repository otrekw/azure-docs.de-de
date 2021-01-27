---
title: Anzeigen der zugewiesenen Rollen einer Gruppe in Azure Active Directory | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie die einer Gruppe zugewiesenen Rollen im Azure AD Admin Center anzeigen können. Das Anzeigen von Gruppen und zugewiesenen Rollen gehört zu den Standardbenutzerberechtigungen.
services: active-directory
author: rolyon
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: roles
ms.topic: article
ms.date: 11/05/2020
ms.author: rolyon
ms.reviewer: vincesm
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1a1939be42126606fdae261e60c890c71374c894
ms.sourcegitcommit: 6272bc01d8bdb833d43c56375bab1841a9c380a5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/23/2021
ms.locfileid: "98741824"
---
# <a name="view-roles-assigned-to-a-group-in-azure-active-directory"></a>Anzeigen der zugewiesenen Rollen einer Gruppe in Azure Active Directory

In diesem Abschnitt wird beschrieben, wie die einer Gruppe zugewiesenen Rollen mithilfe von Azure AD Admin Center angezeigt werden können. Das Anzeigen von Gruppen und zugewiesenen Rollen gehört zu den Standardbenutzerberechtigungen.

1. Melden Sie sich mit beliebigen Anmeldeinformationen (nicht zwingend als Administrator) bei [Azure AD Admin Center](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview) an.

1. Wählen Sie die Gruppe aus, an der Sie interessiert sind.

1. Wählen Sie **Zugewiesene Rollen** aus. Damit werden alle Azure AD-Rollen angezeigt, die dieser Gruppe zugewiesen sind.

   ![Anzeigen aller einer ausgewählten Gruppe zugewiesenen Rollen](./media/groups-view-assignments/view-assignments.png)

## <a name="using-powershell"></a>PowerShell

### <a name="get-object-id-of-the-group"></a>Abrufen der Objekt-ID der Gruppe

```powershell
Get-AzureADMSGroup -SearchString “Contoso_Helpdesk_Administrators”
```

### <a name="view-role-assignment-to-a-group"></a>Anzeigen der Rollenzuweisung einer Gruppe

```powershell
Get-AzureADMSRoleAssignment -Filter "principalId eq '<object id of group>" 
```

## <a name="using-microsoft-graph-api"></a>Verwenden der Microsoft Graph-API

### <a name="get-object-id-of-the-group"></a>Abrufen der Objekt-ID der Gruppe

```powershell
GET https://graph.microsoft.com/beta/groups?$filter displayName eq ‘Contoso_Helpdesk_Administrator’ 
```

### <a name="get-role-assignments-to-a-group"></a>Abrufen der Rollenzuweisungen einer Gruppe

```powershell
GET https://graph.microsoft.com/beta/roleManagement/directory/roleAssignments?$filter=principalId eq
```

## <a name="next-steps"></a>Nächste Schritte

- [Verwenden von Cloudgruppen zum Verwalten von Rollenzuweisungen](groups-concept.md)
- [Problembehandlung bei Rollen, die Cloudgruppen zugewiesen sind](groups-faq-troubleshooting.md)
