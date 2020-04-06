---
title: Auflisten von Rollendefinitionen in Azure RBAC mit Azure-Portal, Azure PowerShell, Azure CLI oder REST-API | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie integrierte und benutzerdefinierte Rollen in Azure RBAC mit Azure-Portal, Azure PowerShell, Azure CLI oder REST-API auflisten.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.assetid: 8078f366-a2c4-4fbb-a44b-fc39fd89df81
ms.service: role-based-access-control
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/19/2020
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: aa888eedc81ceb3188f801e273c70722207bf512
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "80062987"
---
# <a name="list-role-definitions-in-azure-rbac"></a>Auflisten von Rollendefinitionen in Azure RBAC

Eine Rollendefinition ist eine Sammlung von ausführbaren Berechtigungen wie etwa Lesen, Schreiben und Löschen. Sie wird in der Regel einfach als „Rolle“ bezeichnet. [Die rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC) in Azure](overview.md) umfasst über 120 [integrierte Rollen](built-in-roles.md), oder Sie können eigene benutzerdefinierte Rollen erstellen. In diesem Artikel wird beschrieben, wie Sie die integrierten und benutzerdefinierten Rollen auflisten, die Sie verwenden können, um Zugriff auf Azure-Ressourcen zu gewähren.

Eine Liste mit Administratorrollen für Azure Active Directory finden Sie unter [Berechtigungen der Administratorrolle in Azure Active Directory](../active-directory/users-groups-roles/directory-assign-admin-roles.md).

## <a name="azure-portal"></a>Azure-Portal

### <a name="list-all-roles"></a>Auflisten aller Rollen

Führen Sie diese Schritte aus, um alle Rollen im Azure-Portal aufzulisten.

1. Klicken Sie im Azure-Portal auf **Alle Dienste**, und wählen Sie anschließend einen Bereich aus. Sie können beispielsweise **Verwaltungsgruppen**, **Abonnements**, **Ressourcengruppen** oder eine Ressource auswählen.

1. Klicken Sie auf die gewünschte Ressource.

1. Klicken Sie auf **Zugriffssteuerung (IAM)** .

1. Klicken Sie auf die Registerkarte **Rollen**, um eine Liste aller integrierten und benutzerdefinierten Rollen anzuzeigen.

   Sie können die Anzahl von Benutzern und Gruppen anzeigen, die jeder Rolle für den aktuellen Bereich zugewiesen sind.

   ![Rollenliste](./media/role-definitions-list/roles-list.png)

## <a name="azure-powershell"></a>Azure PowerShell

### <a name="list-all-roles"></a>Auflisten aller Rollen

Verwenden Sie [Get-AzRoleDefinition](/powershell/module/az.resources/get-azroledefinition), um alle Rollen in Azure PowerShell aufzulisten.

```azurepowershell
Get-AzRoleDefinition | FT Name, Description
```

```Example
AcrImageSigner                                    acr image signer
AcrQuarantineReader                               acr quarantine data reader
AcrQuarantineWriter                               acr quarantine data writer
API Management Service Contributor                Can manage service and the APIs
API Management Service Operator Role              Can manage service but not the APIs
API Management Service Reader Role                Read-only access to service and APIs
Application Insights Component Contributor        Can manage Application Insights components
Application Insights Snapshot Debugger            Gives user permission to use Application Insights Snapshot Debugge...
Automation Job Operator                           Create and Manage Jobs using Automation Runbooks.
Automation Operator                               Automation Operators are able to start, stop, suspend, and resume ...
...
```

### <a name="list-a-role-definition"></a>Auflisten einer Rollendefinition

Verwenden Sie zum Auflisten der Details einer bestimmte Rolle [Get-AzRoleDefinition](/powershell/module/az.resources/get-azroledefinition).

```azurepowershell
Get-AzRoleDefinition <role_name>
```

```Example
PS C:\> Get-AzRoleDefinition "Contributor"

Name             : Contributor
Id               : b24988ac-6180-42a0-ab88-20f7382dd24c
IsCustom         : False
Description      : Lets you manage everything except access to resources.
Actions          : {*}
NotActions       : {Microsoft.Authorization/*/Delete, Microsoft.Authorization/*/Write,
                   Microsoft.Authorization/elevateAccess/Action}
DataActions      : {}
NotDataActions   : {}
AssignableScopes : {/}
```

### <a name="list-a-role-definition-in-json-format"></a>Auflisten einer Rollendefinition im JSON-Format

Verwenden Sie [Get-AzRoleDefinition](/powershell/module/az.resources/get-azroledefinition), um eine Rolle im JSON-Format aufzulisten.

```azurepowershell
Get-AzRoleDefinition <role_name> | ConvertTo-Json
```

```Example
PS C:\> Get-AzRoleDefinition "Contributor" | ConvertTo-Json

{
  "Name": "Contributor",
  "Id": "b24988ac-6180-42a0-ab88-20f7382dd24c",
  "IsCustom": false,
  "Description": "Lets you manage everything except access to resources.",
  "Actions": [
    "*"
  ],
  "NotActions": [
    "Microsoft.Authorization/*/Delete",
    "Microsoft.Authorization/*/Write",
    "Microsoft.Authorization/elevateAccess/Action",
    "Microsoft.Blueprint/blueprintAssignments/write",
    "Microsoft.Blueprint/blueprintAssignments/delete"
  ],
  "DataActions": [],
  "NotDataActions": [],
  "AssignableScopes": [
    "/"
  ]
}
```

### <a name="list-permissions-of-a-role-definition"></a>Auflisten der Berechtigungen einer Rollendefinition

Verwenden Sie [Get-AzRoleDefinition](/powershell/module/az.resources/get-azroledefinition), um Aktionen für eine bestimmte Rolle aufzulisten.

```azurepowershell
Get-AzRoleDefinition <role_name> | FL Actions, NotActions
```

```Example
PS C:\> Get-AzRoleDefinition "Contributor" | FL Actions, NotActions

Actions    : {*}
NotActions : {Microsoft.Authorization/*/Delete, Microsoft.Authorization/*/Write,
             Microsoft.Authorization/elevateAccess/Action,
             Microsoft.Blueprint/blueprintAssignments/write...}
```

```azurepowershell
(Get-AzRoleDefinition <role_name>).Actions
```

```Example
PS C:\> (Get-AzRoleDefinition "Virtual Machine Contributor").Actions

Microsoft.Authorization/*/read
Microsoft.Compute/availabilitySets/*
Microsoft.Compute/locations/*
Microsoft.Compute/virtualMachines/*
Microsoft.Compute/virtualMachineScaleSets/*
Microsoft.DevTestLab/schedules/*
Microsoft.Insights/alertRules/*
Microsoft.Network/applicationGateways/backendAddressPools/join/action
Microsoft.Network/loadBalancers/backendAddressPools/join/action
...
```

## <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle

### <a name="list-all-roles"></a>Auflisten aller Rollen

Verwenden Sie [az role definition list](/cli/azure/role/definition#az-role-definition-list), um alle Rollen in der Azure CLI aufzulisten.

```azurecli
az role definition list
```

Im folgenden Beispiel werden der Name und die Beschreibung aller verfügbaren Rollendefinitionen aufgelistet:

```azurecli
az role definition list --output json | jq '.[] | {"roleName":.roleName, "description":.description}'
```

```Output
{
  "roleName": "API Management Service Contributor",
  "description": "Can manage service and the APIs"
}
{
  "roleName": "API Management Service Operator Role",
  "description": "Can manage service but not the APIs"
}
{
  "roleName": "API Management Service Reader Role",
  "description": "Read-only access to service and APIs"
}

...
```

Im folgenden Beispiel werden alle integrierten Rollen aufgelistet.

```azurecli
az role definition list --custom-role-only false --output json | jq '.[] | {"roleName":.roleName, "description":.description, "roleType":.roleType}'
```

```Output
{
  "roleName": "API Management Service Contributor",
  "description": "Can manage service and the APIs",
  "roleType": "BuiltInRole"
}
{
  "roleName": "API Management Service Operator Role",
  "description": "Can manage service but not the APIs",
  "roleType": "BuiltInRole"
}
{
  "roleName": "API Management Service Reader Role",
  "description": "Read-only access to service and APIs",
  "roleType": "BuiltInRole"
}

...
```

### <a name="list-a-role-definition"></a>Auflisten einer Rollendefinition

Verwenden Sie [az role definition list](/cli/azure/role/definition#az-role-definition-list), um Details einer Rolle aufzulisten.

```azurecli
az role definition list --name <role_name>
```

Im folgenden Beispiel wird die Rollendefinition für *Contributor* (Mitwirkender) aufgelistet:

```azurecli
az role definition list --name "Contributor"
```

```Output
[
  {
    "additionalProperties": {},
    "assignableScopes": [
      "/"
    ],
    "description": "Lets you manage everything except access to resources.",
    "id": "/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Authorization/roleDefinitions/b24988ac-6180-42a0-ab88-20f7382dd24c",
    "name": "b24988ac-6180-42a0-ab88-20f7382dd24c",
    "permissions": [
      {
        "actions": [
          "*"
        ],
        "additionalProperties": {},
        "dataActions": [],
        "notActions": [
          "Microsoft.Authorization/*/Delete",
          "Microsoft.Authorization/*/Write",
          "Microsoft.Authorization/elevateAccess/Action"
        ],
        "notDataActions": []
      }
    ],
    "roleName": "Contributor",
    "roleType": "BuiltInRole",
    "type": "Microsoft.Authorization/roleDefinitions"
  }
]
```

### <a name="list-permissions-of-a-role-definition"></a>Auflisten der Berechtigungen einer Rollendefinition

Im folgenden Beispiel werden nur *actions* und *notActions* der Rolle *Contributor* aufgelistet.

```azurecli
az role definition list --name "Contributor" --output json | jq '.[] | {"actions":.permissions[0].actions, "notActions":.permissions[0].notActions}'
```

```Output
{
  "actions": [
    "*"
  ],
  "notActions": [
    "Microsoft.Authorization/*/Delete",
    "Microsoft.Authorization/*/Write",
    "Microsoft.Authorization/elevateAccess/Action"
  ]
}
```

Im folgenden Beispiel werden nur die Aktionen der Rolle *Virtual Machine Contributor* aufgelistet.

```azurecli
az role definition list --name "Virtual Machine Contributor" --output json | jq '.[] | .permissions[0].actions'
```

```Output
[
  "Microsoft.Authorization/*/read",
  "Microsoft.Compute/availabilitySets/*",
  "Microsoft.Compute/locations/*",
  "Microsoft.Compute/virtualMachines/*",
  "Microsoft.Compute/virtualMachineScaleSets/*",
  "Microsoft.Insights/alertRules/*",
  "Microsoft.Network/applicationGateways/backendAddressPools/join/action",
  "Microsoft.Network/loadBalancers/backendAddressPools/join/action",

  ...

  "Microsoft.Storage/storageAccounts/listKeys/action",
  "Microsoft.Storage/storageAccounts/read"
]
```

## <a name="rest-api"></a>REST-API

### <a name="list-role-definitions"></a>Auflisten der Rollendefinitionen

Verwenden Sie zum Auflisten von Rollendefinitionen die [Rollendefinitionen – Liste](/rest/api/authorization/roledefinitions/list)-REST-API. Um die Ergebnisse einzugrenzen, geben Sie einen Bereich und einen optionalen Filter an.

1. Beginnen Sie mit der folgenden Anforderung:

    ```http
    GET https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions?$filter={$filter}&api-version=2015-07-01
    ```

1. Ersetzen Sie innerhalb des URIs *{scope}* durch den Bereich, für den die Rollendefinitionen aufgelistet werden sollen.

    > [!div class="mx-tableFixed"]
    > | `Scope` | type |
    > | --- | --- |
    > | `providers/Microsoft.Management/managementGroups/{groupId1}` | Verwaltungsgruppe |
    > | `subscriptions/{subscriptionId1}` | Subscription |
    > | `subscriptions/{subscriptionId1}/resourceGroups/myresourcegroup1` | Resource group |
    > | `subscriptions/{subscriptionId1}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1` | Resource |

    Im vorherigen Beispiel ist „microsoft.web“ ein Ressourcenanbieter, der auf eine App Service-Instanz verweist. Analog dazu können Sie einen beliebigen anderen Ressourcenanbieter verwenden und den Bereich angeben. Weitere Informationen finden Sie unter [Azure-Ressourcenanbieter und -typen](../azure-resource-manager/management/resource-providers-and-types.md) und unter unterstützten [Vorgängen für Azure Resource Manager-Ressourcenanbieter](resource-provider-operations.md).  
     
1. Ersetzen Sie *{filter}* durch die Bedingung, die zum Filtern der Liste mit den Rollendefinitionen angewendet werden soll.

    > [!div class="mx-tableFixed"]
    > | Filtern | BESCHREIBUNG |
    > | --- | --- |
    > | `$filter=atScopeAndBelow()` | Listet Rollendefinitionen für den angegebenen Bereich und alle Unterbereiche auf. |
    > | `$filter=type+eq+'{type}'` | Listet Rollendefinitionen des angegebenen Typs auf. Der Typ der Rolle kann `CustomRole` oder `BuiltInRole`sein. |

### <a name="list-a-role-definition"></a>Auflisten einer Rollendefinition

Um die Details einer bestimmten Rolle aufzulisten, verwenden Sie die REST-API [Rollendefinitionen – Abrufen](/rest/api/authorization/roledefinitions/get) oder [Rollendefinitionen – Abrufen nach ID](/rest/api/authorization/roledefinitions/getbyid).

1. Beginnen Sie mit der folgenden Anforderung:

    ```http
    GET https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{roleDefinitionId}?api-version=2015-07-01
    ```

    Für eine Rollendefinition auf Verzeichnisebene können Sie diese Anforderung verwenden:

    ```http
    GET https://management.azure.com/providers/Microsoft.Authorization/roleDefinitions/{roleDefinitionId}?api-version=2015-07-01
    ```

1. Ersetzen Sie innerhalb des URIs *{scope}* durch den Bereich, für den die Rollendefinition aufgelistet werden soll.

    > [!div class="mx-tableFixed"]
    > | `Scope` | type |
    > | --- | --- |
    > | `providers/Microsoft.Management/managementGroups/{groupId1}` | Verwaltungsgruppe |
    > | `subscriptions/{subscriptionId1}` | Subscription |
    > | `subscriptions/{subscriptionId1}/resourceGroups/myresourcegroup1` | Resource group |
    > | `subscriptions/{subscriptionId1}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1` | Resource |
     
1. Ersetzen Sie *{roleDefinitionId}* durch den Bezeichner der Rollendefinition.

## <a name="next-steps"></a>Nächste Schritte

- [Integrierte Rollen für die rollenbasierte Zugriffssteuerung in Azure](built-in-roles.md)
- [Benutzerdefinierte Rollen für Azure-Ressourcen](custom-roles.md)
- [Auflisten von Rollenzuweisungen mithilfe von Azure RBAC und dem Azure-Portal](role-assignments-list-portal.md)
- [Hinzufügen oder Entfernen von Rollenzuweisungen mithilfe von Azure RBAC und dem Azure-Portal](role-assignments-portal.md)
