---
title: Hinzufügen, Entfernen und Auflisten von Benutzern in einer Verwaltungseinheit – Azure Active Directory | Microsoft-Dokumentation
description: Verwalten von Benutzern und deren Rollenberechtigungen in einer Verwaltungseinheit in Azure Active Directory
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
ms.service: active-directory
ms.topic: how-to
ms.subservice: users-groups-roles
ms.workload: identity
ms.date: 09/22/2020
ms.author: curtand
ms.reviewer: anandy
ms.custom: oldportal;it-pro;
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0d29f4ef5806eb8ed9385696dea78f4ae0992b93
ms.sourcegitcommit: d2222681e14700bdd65baef97de223fa91c22c55
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/07/2020
ms.locfileid: "91818155"
---
# <a name="add-and-manage-users-in-an-administrative-unit-in-azure-active-directory"></a>Hinzufügen und Verwalten von Benutzern in einer Verwaltungseinheit in Azure Active Directory

In Azure Active Directory (Azure AD) können Sie einer Verwaltungseinheit Benutzer hinzufügen, um einen präziseren Verwaltungsbereich steuern zu können.

Die Schritte zum Vorbereiten auf den Einsatz von PowerShell und Microsoft Graph für die Verwaltung von Verwaltungseinheiten sind unter [Erste Schritte](roles-admin-units-manage.md#get-started) beschrieben.

## <a name="add-users-to-an-au"></a>Hinzufügen von Benutzern zu einer Verwaltungseinheit

### <a name="azure-portal"></a>Azure-Portal

Sie können Benutzer Verwaltungseinheiten einzeln oder in einem Massenvorgang zuweisen.

- Einzelne Zuweisung aus einem Benutzerprofil

   1. Melden Sie sich beim [Azure AD Admin Center](https://portal.azure.com) mit Berechtigungen „Administrator für privilegierte Rollen“ an.
   1. Wählen Sie **Benutzer** und dann den Benutzer aus, der einer administrativen Einheit zugewiesen werden soll, um dessen Profil zu öffnen.
   1. Wählen Sie **Verwaltungseinheiten** aus. Der Benutzer kann einer oder mehreren Verwaltungseinheiten zugewiesen werden. Wählen Sie dazu **Der Verwaltungseinheit zuweisen** und dann die Verwaltungseinheiten aus, denen der Benutzer zugewiesen werden soll.

       ![Auswählen von „Hinzufügen“ und Eingeben eines Namens für die Verwaltungseinheit](./media/roles-admin-units-add-manage-users/assign-users-individually.png)

- Einzelne Zuweisung aus einer Verwaltungseinheit

   1. Melden Sie sich beim [Azure AD Admin Center](https://portal.azure.com) mit Berechtigungen „Administrator für privilegierte Rollen“ an.
   1. Wählen Sie **Verwaltungseinheiten** und dann die Verwaltungseinheit aus, der die Benutzer zugewiesen werden sollen.
   1. Wählen Sie **Alle Benutzer** und dann **Mitglied hinzufügen** aus, um im Bereich **Mitglied hinzufügen** mindestens einen Benutzer auszuwählen, der der Verwaltungseinheit zugewiesen werden soll.

        ![Auswählen einer Verwaltungseinheit und dann Auswählen von „Mitglied hinzufügen“](./media/roles-admin-units-add-manage-users/assign-to-admin-unit.png)

- Massenzuweisung

   1. Melden Sie sich beim [Azure AD Admin Center](https://portal.azure.com) mit Berechtigungen „Administrator für privilegierte Rollen“ an.
   1. Wählen Sie **Verwaltungseinheiten** aus.
   1. Wählen Sie die Verwaltungseinheit aus, der Benutzer hinzugefügt werden sollen.
   1. Öffnen Sie **Alle Benutzer** > **Mitglieder aus CSV-Datei hinzufügen**. Anschließend können Sie die CSV-Vorlage herunterladen und die Datei bearbeiten. Das Format ist einfach. In jeder Zeile muss jeweils ein Benutzerprinzipalname hinzugefügt werden. Sobald die Datei bereit ist, speichern Sie sie an einem geeigneten Speicherort, und laden Sie sie dann als Teil dieses Schritts hoch.

    ![Massenzuweisung von Benutzern zu einer Verwaltungseinheit](./media/roles-admin-units-add-manage-users/bulk-assign-to-admin-unit.png)

### <a name="powershell"></a>PowerShell

```powershell
$administrativeunitObj = Get-AzureADMSAdministrativeUnit -Filter "displayname eq 'Test administrative unit 2'"
$UserObj = Get-AzureADUser -Filter "UserPrincipalName eq 'billjohn@fabidentity.onmicrosoft.com'"
Add-AzureADMSAdministrativeUnitMember -Id $administrativeunitObj.ObjectId -RefObjectId $UserObj.ObjectId
```

In diesem Beispiel wird das Cmdlet „Add-AzureADAdministrativeUnitMember“ zum Hinzufügen des Benutzers zur Verwaltungseinheit verwendet. Die Objekt-ID der Verwaltungseinheit, der der Benutzer hinzugefügt werden soll, und die Objekt-ID des hinzuzufügenden Benutzers werden als Argument verwendet. Der markierte Abschnitt kann nach Bedarf für die jeweilige Umgebung geändert werden.

### <a name="microsoft-graph"></a>Microsoft Graph

```http
Http request
POST /administrativeUnits/{Admin Unit id}/members/$ref
Request body
{
  "@odata.id":"https://graph.microsoft.com/v1.0/users/{id}"
}
```

Beispiel:

```http
{
  "@odata.id":"https://graph.microsoft.com/v1.0/users/johndoe@fabidentity.com"
}
```

## <a name="list-administrative-units-for-a-user"></a>Auflisten der Verwaltungseinheiten für einen Benutzer

### <a name="azure-portal"></a>Azure-Portal

Im Azure-Portal können Sie das Profil eines Benutzers öffnen durch:

1. Öffnen von **Azure AD** > **Benutzer**.

1. Wählen Sie den Benutzer aus, um dessen Profil zu öffnen.

1. Wählen Sie **Verwaltungseinheiten** aus, um die Liste der Verwaltungseinheiten anzuzeigen, denen der Benutzer zugewiesen wurde.

   ![Auflisten der Verwaltungseinheiten für einen Benutzer](./media/roles-admin-units-add-manage-users/list-user-admin-units.png)

### <a name="powershell"></a>PowerShell

```powershell
Get-AzureADMSAdministrativeUnit | where { Get-AzureADMSAdministrativeUnitMember -Id $_.ObjectId | where {$_.RefObjectId -eq $userObjId} }
```
Hinweis: „Get-AzureADAdministrativeUnitMember“gibt standardmäßig nur 100 Mitglieder zurück. Sie können „-All $true“ hinzufügen, um weitere Mitglieder abzurufen.

### <a name="microsoft-graph"></a>Microsoft Graph

```http
https://graph.microsoft.com/v1.0/users/{id}/memberOf/$/Microsoft.Graph.AdministrativeUnit
```

## <a name="remove-a-single-user-from-an-au"></a>Entfernen eines einzelnen Benutzers aus einer Verwaltungseinheit

### <a name="azure-portal"></a>Azure-Portal

Es gibt zwei Möglichkeiten, einen Benutzer aus einer Verwaltungseinheit zu entfernen. Im Azure-Portal können Sie das Profil eines Benutzers öffnen, indem Sie zu **Azure AD** > **Benutzer** wechseln. Wählen Sie den Benutzer aus, um dessen Profil zu öffnen. Wählen Sie die Verwaltungseinheit aus, aus der Sie den Benutzer entfernen möchten, und wählen Sie **Aus Verwaltungseinheit entfernen** aus.

![Entfernen eines Benutzers aus einer Verwaltungseinheit über das Benutzerprofil](./media/roles-admin-units-add-manage-users/user-remove-admin-units.png)

Sie können auch in **Azure AD** > **Verwaltungseinheiten** einen Benutzer entfernen, indem Sie die Verwaltungseinheit auswählen, aus der Sie Benutzer entfernen möchten. Wählen Sie zuerst den Benutzer und dann **Mitglied entfernen** aus.
  
![Entfernen eines Benutzers über die Ebene der Verwaltungseinheit](./media/roles-admin-units-add-manage-users/admin-units-remove-user.png)

### <a name="powershell"></a>PowerShell

```powershell
Remove-AzureADMSAdministrativeUnitMember -Id $auId -MemberId $memberUserObjId
```

### <a name="microsoft-graph"></a>Microsoft Graph

   https://graph.microsoft.com/v1.0/directory/administrativeUnits/{adminunit-id}/members/{user-id}/ $ref

## <a name="bulk-remove-more-than-one-user"></a>Massenentfernung von mehreren Benutzern

Sie können zu „Azure AD > Verwaltungseinheiten“ wechseln und die Verwaltungseinheit auswählen, aus der Sie Benutzer entfernen möchten. Klicken Sie auf „Massenentfernung von Mitgliedern“. Laden Sie die CSV-Vorlage herunter, um die Liste der zu entfernenden Benutzer bereitzustellen.

Bearbeiten Sie die heruntergeladene CSV-Vorlage mit den relevanten Benutzereinträgen. Entfernen Sie auf keinen Fall die beiden ersten Zeilen der Tabelle. Fügen Sie in jeder Zeile einen Benutzerprinzipalnamen (UPN) hinzu.

![Bearbeiten der CSV-Datei für die Massenentfernung von Benutzern aus Verwaltungseinheiten](./media/roles-admin-units-add-manage-users/bulk-user-entries.png)

Nachdem Sie die Einträge in der Datei gespeichert haben, laden Sie die Datei hoch. Wählen Sie **Senden** aus.

![Hochladen (Senden) der Datei für die Massenentfernung](./media/roles-admin-units-add-manage-users/bulk-user-remove.png)

## <a name="next-steps"></a>Nächste Schritte

- [Zuweisen einer Rolle zu einer Verwaltungseinheit](roles-admin-units-assign-roles.md)
- [Zuweisen von Gruppen zu einer Verwaltungseinheit](roles-admin-units-add-manage-groups.md)
