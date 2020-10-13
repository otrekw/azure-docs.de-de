---
title: Hinzufügen, Entfernen und Auflisten von Gruppen in einer Verwaltungseinheit – Azure Active Directory | Microsoft-Dokumentation
description: Verwalten von Gruppen und deren Rollenberechtigungen in einer Verwaltungseinheit in Azure Active Directory
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
ms.service: active-directory
ms.topic: how-to
ms.subservice: users-groups-roles
ms.workload: identity
ms.date: 10/07/2020
ms.author: curtand
ms.reviewer: anandy
ms.custom: oldportal;it-pro;
ms.collection: M365-identity-device-management
ms.openlocfilehash: 133ea21bf7a7c1df0fccaeacce7d7a29199c033d
ms.sourcegitcommit: d2222681e14700bdd65baef97de223fa91c22c55
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/07/2020
ms.locfileid: "91827683"
---
# <a name="add-and-manage-groups-in-administrative-units-in-azure-active-directory"></a>Hinzufügen und Verwalten von Gruppen in Verwaltungseinheiten in Azure Active Directory

In Azure Active Directory (Azure AD) können Sie einer Verwaltungseinheit (VE) Gruppen hinzufügen, um einen präziseren Verwaltungsbereich steuern zu können.

Die Schritte zum Vorbereiten auf den Einsatz von PowerShell und Microsoft Graph für die Verwaltung von Verwaltungseinheiten sind unter [Erste Schritte](roles-admin-units-manage.md#get-started) beschrieben.

## <a name="add-groups-to-an-au"></a>Hinzufügen von Gruppen zu einer Verwaltungseinheit

### <a name="azure-portal"></a>Azure-Portal

Sie können Gruppen einer Verwaltungseinheit nur einzeln zuweisen. Es gibt keine Option für die Massenzuweisung von Gruppen zu einer Verwaltungseinheit. Im Portal haben Sie zwei Möglichkeiten, eine Gruppe einer Verwaltungseinheit zuzuweisen:

1. Auf der Seite **Azure AD > Gruppen**

    Öffnen Sie in Azure AD die Seite „Gruppenübersicht“, und wählen Sie die Gruppe aus, die der Verwaltungseinheit zugewiesen werden soll. Wählen Sie auf der linken Seite **Verwaltungseinheiten** aus, um die Verwaltungseinheiten aufzulisten, denen die Gruppe zugewiesen ist. Am oberen Rand finden Sie die Option „Der Verwaltungseinheit zuweisen“. Wenn Sie darauf klicken, wird ein Bereich auf der rechten Seite angezeigt, in dem Sie die Verwaltungseinheit auswählen können.

    ![Zuweisen einer einzelnen Gruppe zu einer Verwaltungseinheit](./media/roles-admin-units-add-manage-groups/assign-to-group-1.png)

1. Auf der Seite **Azure AD > Verwaltungseinheiten > Alle Gruppen**

    Öffnen Sie unter „Azure AD > Verwaltungseinheiten“ das Blatt „Alle Gruppen“. Wenn der Verwaltungseinheit bereits Gruppen zugewiesen sind, werden diese auf der rechten Seite angezeigt. Wählen Sie am oberen Rand **Hinzufügen** aus. Im eingeblendeten rechten Bereich werden die in ihrer Azure AD-Organisation verfügbaren Gruppen aufgeführt. Wählen Sie mindestens eine Gruppe aus, die den Verwaltungseinheiten zugewiesen werden soll.

    ![Auswählen einer Verwaltungseinheit und dann Auswählen von „Mitglied hinzufügen“](./media/roles-admin-units-add-manage-groups/assign-to-admin-unit.png)

### <a name="powershell"></a>PowerShell

```powershell
$administrative unitObj = Get-AzureADMSAdministrativeUnit -Filter "displayname eq 'Test administrative unit 2'"
$GroupObj = Get-AzureADGroup -Filter "displayname eq 'TestGroup'"
Add-AzureADMSAdministrativeUnitMember -ObjectId $administrative unitObj.ObjectId -RefObjectId $GroupObj.ObjectId
```

In diesem Beispiel wird das Cmdlet „Add-AzureADMSAdministrativeUnitMember“ zum Hinzufügen der Gruppe zur Verwaltungseinheit verwendet. Die Objekt-ID der Verwaltungseinheit und die Objekt-ID der hinzuzufügenden Gruppe werden als Argument verwendet. Der markierte Abschnitt kann nach Bedarf für die jeweilige Umgebung geändert werden.

### <a name="microsoft-graph"></a>Microsoft Graph

```http
Http request
POST /administrativeUnits/{Admin Unit id}/members/$ref

Request body
{
"@odata.id":"https://graph.microsoft.com/v1.0/groups/{id}"
}
```

Beispiel:

```http
{
"@odata.id":"https://graph.microsoft.com/v1.0/groups/ 871d21ab-6b4e-4d56-b257-ba27827628f3"
}
```

## <a name="list-groups-in-an-au"></a>Auflisten von Gruppen in einer Verwaltungseinheit

### <a name="azure-portal"></a>Azure-Portal

Wechseln Sie im Portal zu **Azure AD > Verwaltungseinheiten**. Wählen Sie die Verwaltungseinheit aus, deren Benutzer Sie auflisten möchten. Standardmäßig ist im linken Bereich bereits **Alle Benutzer** ausgewählt. Wählen Sie **Alle Gruppen** aus. Auf der rechten Seite finden Sie eine Liste der Gruppen, die Mitglieder der ausgewählten Verwaltungseinheit sind.

![Auflisten von Gruppen in einer Verwaltungseinheit](./media/roles-admin-units-add-manage-groups/list-groups-in-admin-units.png)

### <a name="powershell"></a>PowerShell

```powershell
$administrative unitObj = Get-AzureADMSAdministrativeUnit -Filter "displayname eq 'Test administrative unit 2'"
Get-AzureADMSAdministrativeUnitMember -ObjectId $administrative unitObj.ObjectId
```

Diese hilft Ihnen, alle Mitglieder der Verwaltungseinheit abzurufen. Wenn Sie alle Gruppen anzeigen möchten, die Mitglieder der Verwaltungseinheit sind, können Sie den folgenden Codeausschnitt verwenden:

```http
foreach ($member in (Get-AzureADMSAdministrativeUnitMember -ObjectId $administrative unitObj.ObjectId)) 
{
if($member.ObjectType -eq "Group")
{
Get-AzureADGroup -ObjectId $member.ObjectId
}
}
```

### <a name="microsoft-graph"></a>Microsoft Graph

```http
HTTP request
GET /directory/administrativeUnits/{Admin id}/members/$/microsoft.graph.group
Request body
{}
```

## <a name="list-aus-for-a-group"></a>Auflisten von Verwaltungseinheiten für eine Gruppe

### <a name="azure-portal"></a>Azure-Portal

Im Azure AD-Portal können Sie die Einzelheiten einer Gruppe öffnen, indem Sie **Gruppen** öffnen. Wählen Sie eine Gruppe aus, um das Gruppenprofil zu öffnen. Wählen Sie **Verwaltungseinheiten** aus, um alle Verwaltungseinheiten aufzulisten, bei denen die Gruppe Mitglied ist.

![Auflisten der Verwaltungseinheiten für eine Gruppe](./media/roles-admin-units-add-manage-groups/list-group-au.png)

### <a name="powershell"></a>PowerShell

```powershell
Get-AzureADMSAdministrativeUnit | where { Get-AzureADMSAdministrativeUnitMember -ObjectId $_.ObjectId | where {$_.ObjectId -eq $groupObjId} }
```

### <a name="microsoft-graph"></a>Microsoft Graph

```http
https://graph.microsoft.com/v1.0/groups/<group-id>/memberOf/$/Microsoft.Graph.AdministrativeUnit
```

## <a name="remove-a-group-from-an-au"></a>Entfernen einer Gruppe aus einer Verwaltungseinheit

### <a name="azure-portal"></a>Azure-Portal

Im Azure-Portal gibt es zwei Möglichkeiten, eine Gruppe aus einer Verwaltungseinheit zu entfernen.

- Aus einer Gruppenübersicht entfernen

  1. Öffnen Sie **Azure AD** > **Gruppen** und dann das Profil für die Gruppe, die Sie aus einer Verwaltungseinheit entfernen möchten.
  1. Wählen Sie im linken Bereich **Verwaltungseinheiten** aus, um alle Verwaltungseinheiten aufzulisten, bei denen die Gruppe Mitglied ist. Wählen Sie die Verwaltungseinheit aus, aus der Sie die Gruppe entfernen möchten, und wählen Sie dann **Aus Verwaltungseinheit entfernen** aus.

    ![Entfernen einer Gruppe aus einer Verwaltungseinheit](./media/roles-admin-units-add-manage-groups/group-au-remove.png)

- Aus einer Verwaltungseinheit entfernen

  1. Öffnen Sie **Azure AD** > **Verwaltungseinheiten**, und wählen Sie die Verwaltungseinheit aus, bei der die Gruppe Mitglied ist.
  1. Wählen Sie im linken Bereich **Gruppen** aus, um die Mitgliedergruppen aufzulisten.
  1. Wählen Sie die Gruppe aus, die aus der Verwaltungseinheit entfernt werden soll, und wählen Sie dann **Gruppen entfernen** aus.

    ![Auflisten von Gruppen in einer Verwaltungseinheit](./media/roles-admin-units-add-manage-groups/list-groups-in-admin-units.png)

### <a name="powershell"></a>PowerShell

```powershell
Remove-AzureADMSAdministrativeUnitMember -ObjectId $auId -MemberId $memberGroupObjId
```

### <a name="microsoft-graph"></a>Microsoft Graph

```http
https://graph.microsoft.com/v1.0/directory/AdministrativeUnits/<adminunit-id>/members/<group-id>/$ref
```

## <a name="next-steps"></a>Nächste Schritte

- [Zuweisen einer Rolle zu einer Verwaltungseinheit](roles-admin-units-assign-roles.md)
- [Verwalten von Benutzern in einer Verwaltungseinheit](roles-admin-units-add-manage-users.md)
