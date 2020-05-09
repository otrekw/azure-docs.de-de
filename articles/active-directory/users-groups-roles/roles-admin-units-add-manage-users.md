---
title: Hinzufügen, Entfernen und Auflisten von Benutzern in Verwaltungseinheiten (Vorschau) – Azure Active Directory | Microsoft-Dokumentation
description: Verwalten von Benutzern und deren Rollenberechtigungen in einer Verwaltungseinheit in Azure Active Directory
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
ms.service: active-directory
ms.topic: article
ms.subservice: users-groups-roles
ms.workload: identity
ms.date: 04/16/2020
ms.author: curtand
ms.reviewer: anandy
ms.custom: oldportal;it-pro;
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9c2c5c083115440e1e4da203f39f2b32734458c3
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2020
ms.locfileid: "81684969"
---
# <a name="add-and-manage-users-in-an-administrative-unit-in-azure-active-directory"></a>Hinzufügen und Verwalten von Benutzern in einer Verwaltungseinheit in Azure Active Directory

In Azure Active Directory (Azure AD) können Sie einer Verwaltungseinheit Benutzer hinzufügen, um einen präziseren Verwaltungsbereich steuern zu können.

Die Schritte zum Vorbereiten auf den Einsatz von PowerShell und Microsoft Graph für die Verwaltung von Verwaltungseinheiten sind unter [Erste Schritte](roles-admin-units-manage.md#get-started) beschrieben.

## <a name="add-users-to-an-au"></a>Hinzufügen von Benutzern zu einer Verwaltungseinheit

### <a name="azure-portal"></a>Azure-Portal

Es gibt zwei Möglichkeiten, Benutzer einer Verwaltungseinheit zuzuweisen.

1. Individuelle Zuweisung

    1. Sie können im Portal zu Azure AD wechseln und „Benutzer“ auswählen. Wählen Sie dann den Benutzer aus, der einer Verwaltungseinheit zugewiesen werden soll. Anschließend können Sie im linken Bereich die Verwaltungseinheiten auswählen. Der Benutzer kann einer oder mehreren Verwaltungseinheiten zugewiesen werden. Dazu klicken Sie auf „Der Verwaltungseinheit zuweisen“ und wählen die Verwaltungseinheiten aus, denen der Benutzer zugewiesen werden soll.

       ![Auswählen von „Hinzufügen“ und Eingeben eines Namens für die Verwaltungseinheit](./media/roles-admin-units-add-manage-users/assign-users-individually.png)

    1. Sie können im Portal zu Azure AD wechseln und im linken Bereich „Verwaltungseinheiten“ auswählen. Wählen Sie dann die Verwaltungseinheit aus, der die Benutzer zugewiesen werden sollen. Wählen Sie im linken Bereich „Alle Benutzer“ aus, und wählen Sie dann „Mitglied hinzufügen“ aus. Sie können dann den Vorgang fortsetzen und im rechten Bereich einen oder mehrere Benutzer auswählen, die der Verwaltungseinheit zugewiesen werden sollen.

        ![Auswählen einer Verwaltungseinheit und Auswählen von „Mitglied hinzufügen“](./media/roles-admin-units-add-manage-users/assign-to-admin-unit.png)

1. Massenzuweisung

    Wechseln Sie im Portal zu „Azure AD“, und wählen Sie „Verwaltungseinheiten“ aus. Wählen Sie die Verwaltungseinheit aus, der Benutzer hinzugefügt werden sollen. Klicken Sie zum Fortfahren auf „Alle Benutzer > Mitglieder aus CSV-Datei hinzufügen“. Sie können dann die CSV-Vorlage herunterladen und die Datei bearbeiten. Das Format ist einfach. In jeder Zeile muss jeweils ein Benutzerprinzipalname (UPN) hinzugefügt werden. Nachdem Sie die Datei fertiggestellt haben, speichern Sie sie an einem geeigneten Speicherort. Sie wird dann in Schritt 3 hochgeladen, wie in der Momentaufnahme hervorgehoben.

    ![Massenzuweisung von Benutzern zu einer Verwaltungseinheit](./media/roles-admin-units-add-manage-users/bulk-assign-to-admin-unit.png)

### <a name="powershell"></a>PowerShell

    $administrativeunitObj = Get-AzureADAdministrativeUnit -Filter "displayname eq 'Test administrative unit 2'"
    $UserObj = Get-AzureADUser -Filter "UserPrincipalName eq 'billjohn@fabidentity.onmicrosoft.com'"
    Add-AzureADAdministrativeUnitMember -ObjectId $administrativeunitObj.ObjectId -RefObjectId $UserObj.ObjectId

In diesem Beispiel wird das Cmdlet „Add-AzureADAdministrativeUnitMember“ zum Hinzufügen des Benutzers zur Verwaltungseinheit verwendet. Die Objekt-ID der Verwaltungseinheit, der der Benutzer hinzugefügt werden soll, und die Objekt-ID des hinzuzufügenden Benutzers werden als Argument verwendet. Der markierte Abschnitt kann nach Bedarf für die jeweilige Umgebung geändert werden.

### <a name="microsoft-graph"></a>Microsoft Graph

    Http request
    POST /administrativeUnits/{Admin Unit id}/members/$ref
    Request body
    {
      "@odata.id":"https://graph.microsoft.com/beta/users/{id}"
    }

Beispiel:

    {
      "@odata.id":"https://graph.microsoft.com/beta/users/johndoe@fabidentity.com"
    }

## <a name="list-administrative-units-for-a-user"></a>Auflisten der Verwaltungseinheiten für einen Benutzer

### <a name="azure-portal"></a>Azure-Portal

Im Azure-Portal können Sie das Profil eines Benutzers öffnen, indem Sie zu „Azure AD > Benutzer“ wechseln. Klicken Sie auf den Benutzer, um das Profil des Benutzers zu öffnen.

![Öffnen eines Benutzerprofils in Azure Active Directory](./media/roles-admin-units-add-manage-users/user-profile-admin-units.png)

Wählen Sie im linken Bereich **Verwaltungseinheiten** aus, um die Liste der Verwaltungseinheiten anzuzeigen, denen der Benutzer zugewiesen wurde.

![Auflisten der Verwaltungseinheiten für einen Benutzer](./media/roles-admin-units-add-manage-users/list-user-admin-units.png)

### <a name="powershell"></a>PowerShell

    Get-AzureADAdministrativeUnit | where { Get-AzureADAdministrativeUnitMember -ObjectId $_.ObjectId | where {$_.ObjectId -eq $userObjId} }

### <a name="microsoft-graph"></a>Microsoft Graph

    https://graph.microsoft.com/beta/users//memberOf/$/Microsoft.Graph.AdministrativeUnit

## <a name="remove-a-single-user-from-an-au"></a>Entfernen eines einzelnen Benutzers aus einer Verwaltungseinheit

### <a name="azure-portal"></a>Azure-Portal

Es gibt zwei Möglichkeiten, einen Benutzer aus einer Verwaltungseinheit zu entfernen. Im Azure-Portal können Sie das Profil eines Benutzers öffnen, indem Sie zu **Azure AD** > **Benutzer** wechseln. Wählen Sie den Benutzer aus, um dessen Profil zu öffnen. Wählen Sie die Verwaltungseinheit aus, aus der Sie den Benutzer entfernen möchten, und wählen Sie **Aus Verwaltungseinheit entfernen** aus.

![Entfernen eines Benutzers aus einer Verwaltungseinheit über das Benutzerprofil](./media/roles-admin-units-add-manage-users/user-remove-admin-units.png)

Sie können auch in **Azure AD** > **Verwaltungseinheiten** einen Benutzer entfernen, indem Sie die Verwaltungseinheit auswählen, aus der Sie Benutzer entfernen möchten. Wählen Sie zuerst den Benutzer und dann **Mitglied entfernen** aus.
  
![Entfernen eines Benutzers über die Ebene der Verwaltungseinheit](./media/roles-admin-units-add-manage-users/admin-units-remove-user.png)

### <a name="powershell"></a>PowerShell

    Remove-AzureADAdministrativeUnitMember -ObjectId $auId -MemberId $memberUserObjId

### <a name="microsoft-graph"></a>Microsoft Graph

   https://graph.microsoft.com/beta/administrativeUnits/<adminunit-id>/members/<user-id>/ $ref

## <a name="bulk-remove-more-than-one-user"></a>Massenentfernung von mehreren Benutzern

Sie können zu „Azure AD > Verwaltungseinheiten“ wechseln und die Verwaltungseinheit auswählen, aus der Sie Benutzer entfernen möchten. Klicken Sie auf „Massenentfernung von Mitgliedern“. Laden Sie die CSV-Vorlage herunter, um die Liste der zu entfernenden Benutzer bereitzustellen.

Bearbeiten Sie die heruntergeladene CSV-Vorlage mit den relevanten Benutzereinträgen. Entfernen Sie auf keinen Fall die beiden ersten Zeilen der Tabelle. Fügen Sie in jeder Zeile einen Benutzerprinzipalnamen (UPN) hinzu.

![Bearbeiten der CSV-Datei für die Massenentfernung von Benutzern aus Verwaltungseinheiten](./media/roles-admin-units-add-manage-users/bulk-user-entries.png)

Nachdem Sie die Einträge in der Datei gespeichert haben, laden Sie die Datei hoch. Wählen Sie **Senden** aus.

![Hochladen (Senden) der Datei für die Massenentfernung](./media/roles-admin-units-add-manage-users/bulk-user-remove.png)

## <a name="next-steps"></a>Nächste Schritte

- [Zuweisen einer Rolle zu einer Verwaltungseinheit](roles-admin-units-assign-roles.md)
- [Zuweisen von Gruppen zu einer Verwaltungseinheit](roles-admin-units-add-manage-groups.md)
