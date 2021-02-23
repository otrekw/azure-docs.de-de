---
title: 'Entfernen von Azure-Rollenzuweisungen: Azure RBAC'
description: Erfahren Sie, wie Sie den Zugriff auf Azure-Ressourcen für Benutzer, Gruppen, Dienstprinzipale oder verwaltete Identitäten mithilfe des Azure-Portals, mit Azure PowerShell, der Azure CLI oder der REST-API entfernen.
services: active-directory
author: rolyon
manager: daveba
ms.service: role-based-access-control
ms.topic: how-to
ms.workload: identity
ms.date: 02/15/2021
ms.author: rolyon
ms.openlocfilehash: 7a3e4853d6dffa7eb2c5cf80846f6f1bd6beba03
ms.sourcegitcommit: de98cb7b98eaab1b92aa6a378436d9d513494404
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/17/2021
ms.locfileid: "100561150"
---
# <a name="remove-azure-role-assignments"></a>Entfernen von Azure-Rollenzuweisungen

[Azure RBAC](../../articles/role-based-access-control/overview.md) (Role-Based Access Control, rollenbasierte Zugriffssteuerung) ist das Autorisierungssystem für die Verwaltung des Zugriffs auf Azure-Ressourcen. Um den Zugriff auf eine Azure-Ressource zu entfernen, entfernen Sie eine Rollenzuweisung. In diesem Artikel wird beschrieben, wie Rollenzuweisungen mithilfe des Azure-Portals, mit Azure PowerShell, der Azure CLI und der REST-API entfernt werden.

## <a name="prerequisites"></a>Voraussetzungen

Sie benötigen Folgendes, um Rollenzuweisungen entfernen zu können:

- `Microsoft.Authorization/roleAssignments/delete`-Berechtigungen, z. B. [Benutzerzugriffsadministrator](../../articles/role-based-access-control/built-in-roles.md#user-access-administrator) oder [Besitzer](../../articles/role-based-access-control/built-in-roles.md#owner)

## <a name="azure-portal"></a>Azure-Portal

1. Öffnen Sie das Blatt **Zugriffssteuerung (IAM)** für einen Bereich, z.B. für eine Verwaltungsgruppe, ein Abonnement, eine Ressourcengruppe oder eine Ressource, für das bzw. die Sie den Zugriff entfernen möchten.

1. Klicken Sie auf die Registerkarte **Rollenzuweisungen**, um alle Rollenzuweisungen für diesen Bereich anzuzeigen.

1. Aktivieren Sie in der Liste der Rollenzuweisungen den Sicherheitsprinzipal mit der zu entfernenden Rollenzuweisung.

   ![Zum Entfernen ausgewählte Rollenzuweisung](./media/role-assignments-remove/rg-role-assignments-select.png)

1. Klicken Sie auf **Entfernen**.

   ![Nachricht zum Entfernen der Rollenzuweisung](./media/role-assignments-remove/remove-role-assignment.png)

1. Klicken Sie in der angezeigten Meldung zum Entfernen der Rollenzuweisung auf **Ja**.

    Wenn eine Meldung angezeigt wird, dass geerbte Rollenzuweisungen nicht entfernt werden können, versuchen Sie, eine Rollenzuweisung in einem untergeordneten Bereich zu entfernen. Öffnen Sie die Zugriffssteuerung (IAM) in dem Bereich, in dem die Rolle zugewiesen wurde, und versuchen Sie es noch mal. Eine Möglichkeit zum schnellen Öffnen der Zugriffssteuerung (IAM) im richtigen Bereich besteht darin, in der Spalte **Bereich** auf den Link neben **(Geerbt)** zu klicken.

   ![Nachricht zum Entfernen der Rollenzuweisung für geerbte Rollenzuweisungen](./media/role-assignments-remove/remove-role-assignment-inherited.png)

## <a name="azure-powershell"></a>Azure PowerShell

In Azure PowerShell entfernen Sie mit [Remove-AzRoleAssignment](/powershell/module/az.resources/remove-azroleassignment) eine Rollenzuweisung.

Im folgenden Beispiel wird die Zuweisung der Rolle [Mitwirkender für virtuelle Computer](built-in-roles.md#virtual-machine-contributor) von Benutzer *patlong\@contoso.com* für die Ressourcengruppe *pharma-sales* entfernt:

```azurepowershell
PS C:\> Remove-AzRoleAssignment -SignInName patlong@contoso.com `
-RoleDefinitionName "Virtual Machine Contributor" `
-ResourceGroupName pharma-sales
```

Entfernt die Rolle [Leser](built-in-roles.md#reader) von der Gruppe *Ann Mack Team* mit der ID „22222222-2222-2222-2222-222222222222“ im Abonnementbereich.

```azurepowershell
PS C:\> Remove-AzRoleAssignment -ObjectId 22222222-2222-2222-2222-222222222222 `
-RoleDefinitionName "Reader" `
-Scope "/subscriptions/00000000-0000-0000-0000-000000000000"
```

Entfernt die Rolle [Abrechnungsleser](built-in-roles.md#billing-reader) vom Benutzer *alain\@example.com* im Verwaltungsgruppenbereich.

```azurepowershell
PS C:\> Remove-AzRoleAssignment -SignInName alain@example.com `
-RoleDefinitionName "Billing Reader" `
-Scope "/providers/Microsoft.Management/managementGroups/marketing-group"
```

Wenn Sie die folgende Fehlermeldung erhalten: „The provided information does not map to a role assignment“ (Die angegebenen Informationen stimmen mit keiner Rollenzuweisung überein), sollten Sie sicherstellen, dass auch die Parameter `-Scope` oder `-ResourceGroupName` angegeben werden. Weitere Informationen finden Sie unter [Behandeln von Problemen bei Azure RBAC](troubleshooting.md#role-assignments-with-identity-not-found).

## <a name="azure-cli"></a>Azure CLI

In der Azure CLI entfernen Sie mit [az role assignment delete](/cli/azure/role/assignment#az_role_assignment_delete) eine Rollenzuweisung.

Im folgenden Beispiel wird die Zuweisung der Rolle [Mitwirkender für virtuelle Computer](built-in-roles.md#virtual-machine-contributor) von Benutzer *patlong\@contoso.com* für die Ressourcengruppe *pharma-sales* entfernt:

```azurecli
az role assignment delete --assignee "patlong@contoso.com" \
--role "Virtual Machine Contributor" \
--resource-group "pharma-sales"
```

Entfernt die Rolle [Leser](built-in-roles.md#reader) von der Gruppe *Ann Mack Team* mit der ID „22222222-2222-2222-2222-222222222222“ im Abonnementbereich.

```azurecli
az role assignment delete --assignee "22222222-2222-2222-2222-222222222222" \
--role "Reader" \
--subscription "00000000-0000-0000-0000-000000000000"
```

Entfernt die Rolle [Abrechnungsleser](built-in-roles.md#billing-reader) vom Benutzer *alain\@example.com* im Verwaltungsgruppenbereich.

```azurecli
az role assignment delete --assignee "alain@example.com" \
--role "Billing Reader" \
--scope "/providers/Microsoft.Management/managementGroups/marketing-group"
```

## <a name="rest-api"></a>REST-API

In der REST-API entfernen Sie eine Rollenzuweisung mithilfe von [Role Assignments - Delete](/rest/api/authorization/roleassignments/delete).

1. Rufen Sie den Bezeichner der Rollenzuweisung (GUID) ab. Dieser Bezeichner wird beim ersten Erstellen der Rollenzuweisung zurückgegeben oder kann durch Auflisten der Rollenzuweisungen ermittelt werden.

1. Beginnen Sie mit der folgenden Anforderung:

    ```http
    DELETE https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments/{roleAssignmentId}?api-version=2015-07-01
    ```

1. Ersetzen Sie innerhalb des URIs *{scope}* durch den Bereich, um die Rollenzuweisung zu entfernen.

    > [!div class="mx-tableFixed"]
    > | `Scope` | type |
    > | --- | --- |
    > | `providers/Microsoft.Management/managementGroups/{groupId1}` | Verwaltungsgruppe |
    > | `subscriptions/{subscriptionId1}` | Subscription |
    > | `subscriptions/{subscriptionId1}/resourceGroups/myresourcegroup1` | Resource group |
    > | `subscriptions/{subscriptionId1}/resourceGroups/myresourcegroup1/providers/microsoft.web/sites/mysite1` | Resource |

1. Ersetzen Sie *{roleAssignmentId}* durch den GUID-Bezeichner der Rollenzuweisung.

Mit der folgenden Anforderung wird die angegebene Rollenzuweisung im Abonnementbereich entfernt:

```http
DELETE https://management.azure.com/subscriptions/{subscriptionId1}/providers/microsoft.authorization/roleassignments/{roleAssignmentId1}?api-version=2015-07-01
```

Nachfolgend sehen Sie ein Beispiel für die Ausgabe:

```json
{
    "properties": {
        "roleDefinitionId": "/subscriptions/{subscriptionId1}/providers/Microsoft.Authorization/roleDefinitions/a795c7a0-d4a2-40c1-ae25-d81f01202912",
        "principalId": "{objectId1}",
        "scope": "/subscriptions/{subscriptionId1}",
        "createdOn": "2020-05-06T23:55:24.5379478Z",
        "updatedOn": "2020-05-06T23:55:24.5379478Z",
        "createdBy": "{createdByObjectId1}",
        "updatedBy": "{updatedByObjectId1}"
    },
    "id": "/subscriptions/{subscriptionId1}/providers/Microsoft.Authorization/roleAssignments/{roleAssignmentId1}",
    "type": "Microsoft.Authorization/roleAssignments",
    "name": "{roleAssignmentId1}"
}
```

## <a name="arm-template"></a>ARM-Vorlage

Es gibt keine Möglichkeit, eine Rollenzuweisung mithilfe einer Azure Resource Manager-Vorlage (ARM-Vorlage) zu entfernen. Zum Entfernen einer Rollenzuweisung müssen Sie andere Tools verwenden, z. B. das Azure-Portal, Azure PowerShell, die Azure CLI oder die REST-API.

## <a name="next-steps"></a>Nächste Schritte

- [Auflisten von Azure-Rollenzuweisungen über das Azure-Portal](role-assignments-list-portal.md)
- [Auflisten von Azure-Rollenzuweisungen mit Azure PowerShell](role-assignments-list-powershell.md)
- [Behandeln von Problemen bei Azure RBAC](troubleshooting.md)
