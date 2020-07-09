---
title: Hinzufügen und Entfernen von Verwaltungseinheiten (Vorschau) – Azure Active Directory | Microsoft-Dokumentation
description: Verwenden Sie Verwaltungseinheiten, um den Geltungsbereich der Rollenberechtigungen in Azure Active Directory einzuschränken.
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
ms.openlocfilehash: 977a90419c142e576fcf484562875d12c8dad451
ms.sourcegitcommit: cec9676ec235ff798d2a5cad6ee45f98a421837b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85851773"
---
# <a name="manage-administrative-units-in-azure-active-directory"></a>Verwalten von Verwaltungseinheiten in Azure Active Directory

Um eine präzisere administrative Steuerung in Azure Active Directory (Azure AD) zu ermöglichen, können Sie Benutzer einer Azure AD-Rolle mit einem auf eine oder mehrere Verwaltungseinheiten beschränkten Bereich zuweisen.

## <a name="get-started"></a>Erste Schritte

1. Um Abfragen aus den folgenden Anweisungen über den [Graph-Tester](https://aka.ms/ge) auszuführen, gehen Sie folgendermaßen vor:

    a. Wechseln Sie im Azure-Portal zu Azure AD. Wählen Sie in der Liste der Anwendungen **Graph-Tester** und dann **Administratoreinwilligung für Graph-Tester erteilen** aus.

    ![Screenshot mit Link zu „Administratoreinwilligung erteilen“](./media/roles-admin-units-manage/select-graph-explorer.png)

    b. Wählen Sie im Graph-Tester die **Betaversion** aus.

    ![Screenshot mit ausgewählter Betaversion](./media/roles-admin-units-manage/select-beta-version.png)

1. Verwenden Sie die Vorschauversion von Azure AD PowerShell.

## <a name="add-an-administrative-unit"></a>Hinzufügen einer Verwaltungseinheit

### <a name="use-the-azure-portal"></a>Verwenden des Azure-Portals

1. Wechseln Sie im Azure-Portal zu Azure AD, und wählen Sie im Bereich links **Verwaltungseinheiten** aus.

    ![Screenshot mit dem Link zu „Verwaltungseinheiten (Vorschau)“ in Azure AD](./media/roles-admin-units-manage/nav-to-admin-units.png)

1. Wählen Sie **Hinzufügen** aus, und geben Sie den Namen der Verwaltungseinheit ein. Fügen Sie optional eine Beschreibung der Verwaltungseinheit hinzu.

    ![Screenshot der Schaltfläche „Hinzufügen“ und des Textfelds zum Eingeben des Namens der Verwaltungseinheit](./media/roles-admin-units-manage/add-new-admin-unit.png)

1. Wählen Sie **Hinzufügen** aus, um die Verwaltungseinheit abzuschließen.

### <a name="use-powershell"></a>Verwenden von PowerShell

Installieren Sie Azure AD PowerShell (Vorschau), bevor Sie versuchen, die folgenden Befehle auszuführen:

```powershell
Connect-AzureAD
New-AzureADAdministrativeUnit -Description "West Coast region" -DisplayName "West Coast"
```

Sie können die Werte in den Anführungszeichen nach Bedarf ändern.

### <a name="use-microsoft-graph"></a>Verwenden von Microsoft Graph

```http
Http Request
POST /administrativeUnits
Request body
{
  "displayName": "North America Operations",
  "description": "North America Operations administration"
}
```

## <a name="remove-an-administrative-unit"></a>Entfernen einer Verwaltungseinheit

In Azure AD können Sie eine Verwaltungseinheit entfernen, die nicht mehr als Bereichseinheit für Administratorrollen benötigt wird.

### <a name="use-the-azure-portal"></a>Verwenden des Azure-Portals

1. Wechseln Sie im Azure-Portal zu **Azure AD** > **Verwaltungseinheiten**. 
1. Wählen Sie die zu löschende Verwaltungseinheit und dann **Löschen** aus. 
1. Klicken Sie auf **Ja**, um zu bestätigen, dass Sie die Verwaltungseinheit löschen möchten. Die Verwaltungseinheit wird gelöscht.

![Screenshot der Schaltfläche „Löschen“ und des Bestätigungsfensters für eine Verwaltungseinheit](./media/roles-admin-units-manage/select-admin-unit-to-delete.png)

### <a name="use-powershell"></a>Verwenden von PowerShell

```powershell
$delau = Get-AzureADAdministrativeUnit -Filter "displayname eq 'DeleteMe Admin Unit'"
Remove-AzureADAdministrativeUnit -ObjectId $delau.ObjectId
```

Sie können die Werte in den Anführungszeichen so ändern, wie sie für die jeweilige Umgebung benötigt werden.

### <a name="use-the-graph-api"></a>Verwenden der Graph-API

```http
HTTP request
DELETE /administrativeUnits/{Admin id}
Request body
{}
```

## <a name="next-steps"></a>Nächste Schritte

* [Verwalten von Benutzern in einer Verwaltungseinheit](roles-admin-units-add-manage-users.md)
* [Verwalten von Gruppen in einer Verwaltungseinheit](roles-admin-units-add-manage-groups.md)
