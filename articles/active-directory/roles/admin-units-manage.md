---
title: Hinzufügen und Entfernen von Verwaltungseinheiten – Azure Active Directory | Microsoft-Dokumentation
description: Verwenden Sie Verwaltungseinheiten, um den Geltungsbereich der Rollenberechtigungen in Azure Active Directory einzuschränken.
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
ms.openlocfilehash: 6fad9356d3379e76aa259d67711d18f14a4e266f
ms.sourcegitcommit: 3b5cb7fb84a427aee5b15fb96b89ec213a6536c2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/14/2021
ms.locfileid: "107505274"
---
# <a name="manage-administrative-units-in-azure-active-directory"></a>Verwalten von Verwaltungseinheiten in Azure Active Directory

Um eine präzisere administrative Steuerung in Azure Active Directory (Azure AD) zu ermöglichen, können Sie Benutzer einer Azure AD-Rolle mit einem auf eine oder mehrere Verwaltungseinheiten beschränkten Bereich zuweisen.

## <a name="get-started"></a>Erste Schritte

1. Um Abfragen aus den folgenden Anweisungen über den [Graph-Tester](https://aka.ms/ge) auszuführen, gehen Sie folgendermaßen vor:

    a. Wechseln Sie im Azure-Portal zu Azure AD. 
    
    b. Wählen Sie in der Anwendungsliste **Graph-Tester** aus.
    
    c. Wählen Sie im Bereich **Berechtigungen** die Option **Grant admin consent for Graph explorer** (Administratoreinwilligung für Graph-Tester erteilen) aus.

    ![Screenshot: Link „Grant admin consent for Graph explorer“ (Administratoreinwilligung für Graph-Tester erteilen)](./media/admin-units-manage/select-graph-explorer.png)


1. Verwenden Sie [Azure AD PowerShell](https://www.powershellgallery.com/packages/AzureAD/).

## <a name="add-an-administrative-unit"></a>Hinzufügen einer Verwaltungseinheit

Sie können eine Verwaltungseinheit entweder über das Azure-Portal oder mithilfe von PowerShell hinzufügen.

### <a name="use-the-azure-portal"></a>Verwenden des Azure-Portals

1. Wechseln Sie im Azure-Portal zu Azure AD. Anschließend können Sie im linken Bereich **Verwaltungseinheiten** auswählen.

    ![Screenshot: Link „Verwaltungseinheiten“ in Azure AD](./media/admin-units-manage/nav-to-admin-units.png)

1. Wählen Sie oben im Bereich die Schaltfläche **Hinzufügen** aus, und geben Sie dann im Feld **Name** den Namen der Verwaltungseinheit ein. Fügen Sie optional eine Beschreibung der Verwaltungseinheit hinzu.

    ![Screenshot: Schaltfläche „Hinzufügen“ und Feld „Name“ zum Eingeben des Namens der Verwaltungseinheit](./media/admin-units-manage/add-new-admin-unit.png)

1. Wählen Sie die blaue Schaltfläche **Hinzufügen** aus, um die Verwaltungseinheit abzuschließen.

### <a name="use-powershell"></a>Verwenden von PowerShell

Installieren Sie [Azure AD PowerShell](https://www.powershellgallery.com/packages/AzureAD/), bevor Sie versuchen, die folgenden Befehle auszuführen:

```powershell
Connect-AzureAD
New-AzureADMSAdministrativeUnit -Description "West Coast region" -DisplayName "West Coast"
```

Sie können die Werte in den Anführungszeichen nach Bedarf ändern.

### <a name="use-microsoft-graph"></a>Verwenden von Microsoft Graph

Anforderung

```http
POST /administrativeUnits
```

Text

```http
{
  "displayName": "North America Operations",
  "description": "North America Operations administration"
}
```

## <a name="remove-an-administrative-unit"></a>Entfernen einer Verwaltungseinheit

In Azure AD können Sie eine Verwaltungseinheit entfernen, die nicht mehr als Bereichseinheit für Administratorrollen benötigt wird.

### <a name="use-the-azure-portal"></a>Verwenden des Azure-Portals

1. Navigieren Sie im Azure-Portal zu **Azure AD**, und wählen Sie **Verwaltungseinheiten** aus. 
1. Wählen Sie die zu löschende Verwaltungseinheit und dann **Löschen** aus. 
1. Klicken Sie auf **Ja**, um zu bestätigen, dass Sie die Verwaltungseinheit löschen möchten. Die Verwaltungseinheit wird gelöscht.

![Screenshot: Schaltfläche „Löschen“ und Bestätigungsfenster für eine Verwaltungseinheit](./media/admin-units-manage/select-admin-unit-to-delete.png)

### <a name="use-powershell"></a>Verwenden von PowerShell

```powershell
$adminUnitObj = Get-AzureADMSAdministrativeUnit -Filter "displayname eq 'DeleteMe Admin Unit'"
Remove-AzureADMSAdministrativeUnit -Id $adminUnitObj.Id
```

Sie können die Werte in den Anführungszeichen so ändern, wie sie für die jeweilige Umgebung benötigt werden.

### <a name="use-the-graph-api"></a>Verwenden der Graph-API

Anforderung

```http
DELETE /administrativeUnits/{admin-unit-id}
```

Body

```http
{}
```

## <a name="next-steps"></a>Nächste Schritte

* [Verwalten von Benutzern in einer Verwaltungseinheit](admin-units-add-manage-users.md)
* [Verwalten von Gruppen in einer Verwaltungseinheit](admin-units-add-manage-groups.md)
