---
title: Hinzufügen oder Entfernen von Azure-Rollenzuweisungen mithilfe der REST-API – Azure RBAC
description: Erfahren Sie, wie Sie den Zugriff auf Azure-Ressourcen für Benutzer, Gruppen, Dienstprinzipale und verwaltete Identitäten über die REST-API mit der rollenbasierten Zugriffssteuerung (Role-Based Access Control, RBAC) verwalten.
services: active-directory
documentationcenter: na
author: rolyon
manager: mtillman
editor: ''
ms.assetid: 1f90228a-7aac-4ea7-ad82-b57d222ab128
ms.service: role-based-access-control
ms.workload: multiple
ms.tgt_pltfrm: rest-api
ms.devlang: na
ms.topic: conceptual
ms.date: 05/06/2020
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: f9a8b35b07a4149fa2d6b9f8e6698e41f3e6870c
ms.sourcegitcommit: b396c674aa8f66597fa2dd6d6ed200dd7f409915
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2020
ms.locfileid: "82891290"
---
# <a name="add-or-remove-azure-role-assignments-using-the-rest-api"></a>Hinzufügen oder Entfernen von Azure-Rollenzuweisungen mithilfe der REST-API

[!INCLUDE [Azure RBAC definition grant access](../../includes/role-based-access-control-definition-grant.md)] In diesem Artikel wird die Zuweisung von Rollen mithilfe der REST-API beschrieben.

## <a name="prerequisites"></a>Voraussetzungen

Sie benötigen Folgendes, um Rollenzuweisungen hinzufügen oder entfernen zu können:

- `Microsoft.Authorization/roleAssignments/write`- und `Microsoft.Authorization/roleAssignments/delete`-Berechtigungen, wie z.B. [Benutzerzugriffsadministrator](built-in-roles.md#user-access-administrator) oder [Besitzer](built-in-roles.md#owner)

## <a name="add-a-role-assignment"></a>Hinzufügen einer Rollenzuweisung

In Azure RBAC fügen Sie zum Gewähren des Zugriffs eine Rollenzuweisung hinzu. Um eine Rollenzuweisung hinzuzufügen, verwenden Sie die REST-API unter [Rollenzuweisungen – Erstellen](/rest/api/authorization/roleassignments/create), und geben Sie den Sicherheitsprinzipal, die Rollendefinition und den Bereich an. Zum Aufrufen dieser API benötigen Sie Zugriff auf den Vorgang `Microsoft.Authorization/roleAssignments/write`. Von den integrierten Rollen verfügen nur [Besitzer](built-in-roles.md#owner) und [Benutzerzugriffsadministrator](built-in-roles.md#user-access-administrator) über Zugriff auf diesen Vorgang.

1. Verwenden Sie die REST API unter [Rollendefinitionen – Auflisten](/rest/api/authorization/roledefinitions/list) oder lesen Sie [Integrierte Rollen in Azure](built-in-roles.md), um den Bezeichner für die zuzuweisende Rollendefinition zu ermitteln.

1. Verwenden Sie ein GUID-Tool, um einen eindeutigen Bezeichner zu generieren, der für den Bezeichner der Rollenzuweisung verwendet wird. Das Format des Bezeichners ist: `00000000-0000-0000-0000-000000000000`

1. Beginnen Sie mit folgender Anforderung und folgendem Text:

    ```http
    PUT https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments/{roleAssignmentId}?api-version=2015-07-01
    ```

    ```json
    {
      "properties": {
        "roleDefinitionId": "/{scope}/providers/Microsoft.Authorization/roleDefinitions/{roleDefinitionId}",
        "principalId": "{principalId}"
      }
    }
    ```

1. Ersetzen Sie innerhalb des URIs *{scope}* durch den Bereich für die Rollenzuweisung.

    > [!div class="mx-tableFixed"]
    > | `Scope` | type |
    > | --- | --- |
    > | `providers/Microsoft.Management/managementGroups/{groupId1}` | Verwaltungsgruppe |
    > | `subscriptions/{subscriptionId1}` | Subscription |
    > | `subscriptions/{subscriptionId1}/resourceGroups/myresourcegroup1` | Resource group |
    > | `subscriptions/{subscriptionId1}/resourceGroups/myresourcegroup1/providers/microsoft.web/sites/mysite1` | Resource |

    Im vorherigen Beispiel ist „microsoft.web“ ein Ressourcenanbieter, der auf eine App Service-Instanz verweist. Analog dazu können Sie einen beliebigen anderen Ressourcenanbieter verwenden und den Bereich angeben. Weitere Informationen finden Sie unter [Azure-Ressourcenanbieter und -typen](../azure-resource-manager/management/resource-providers-and-types.md) und unter unterstützten [Vorgängen für Azure Resource Manager-Ressourcenanbieter](resource-provider-operations.md).  

1. Ersetzen Sie *{roleAssignmentId}* durch den GUID-Bezeichner der Rollenzuweisung.

1. Ersetzen Sie innerhalb des Anforderungstexts *{scope}* durch den Bereich für die Rollenzuweisung.

    > [!div class="mx-tableFixed"]
    > | `Scope` | type |
    > | --- | --- |
    > | `providers/Microsoft.Management/managementGroups/{groupId1}` | Verwaltungsgruppe |
    > | `subscriptions/{subscriptionId1}` | Subscription |
    > | `subscriptions/{subscriptionId1}/resourceGroups/myresourcegroup1` | Resource group |
    > | `subscriptions/{subscriptionId1}/resourceGroups/myresourcegroup1/providers/microsoft.web/sites/mysite1` | Resource |

1. Ersetzen Sie *{roleDefinitionId}* durch den Bezeichner der Rollendefinition.

1. Ersetzen Sie *{principalId}* durch den Objektbezeichner des Benutzers, der Gruppe oder des Dienstprinzipals, der bzw. die der Rolle zugewiesen wird.

Mit der folgenden Anforderung und dem folgenden Text wird einem Benutzer im Abonnementbereich die Rolle [Benutzer mit Leseberechtigung für Sicherungsfunktionen](built-in-roles.md#backup-reader) zugewiesen:

```http
PUT https://management.azure.com/subscriptions/{subscriptionId1}/providers/microsoft.authorization/roleassignments/{roleAssignmentId1}?api-version=2015-07-01
```

```json
{
  "properties": {
    "roleDefinitionId": "/subscriptions/{subscriptionId1}/providers/Microsoft.Authorization/roleDefinitions/a795c7a0-d4a2-40c1-ae25-d81f01202912",
    "principalId": "{objectId1}"
  }
}
```

Nachfolgend sehen Sie ein Beispiel für die Ausgabe:

```json
{
    "properties": {
        "roleDefinitionId": "/subscriptions/{subscriptionId1}/providers/Microsoft.Authorization/roleDefinitions/a795c7a0-d4a2-40c1-ae25-d81f01202912",
        "principalId": "{objectId1}",
        "scope": "/subscriptions/{subscriptionId1}",
        "createdOn": "2020-05-06T23:55:23.7679147Z",
        "updatedOn": "2020-05-06T23:55:23.7679147Z",
        "createdBy": null,
        "updatedBy": "{updatedByObjectId1}"
    },
    "id": "/subscriptions/{subscriptionId1}/providers/Microsoft.Authorization/roleAssignments/{roleAssignmentId1}",
    "type": "Microsoft.Authorization/roleAssignments",
    "name": "{roleAssignmentId1}"
}
```

## <a name="remove-a-role-assignment"></a>Entfernen einer Rollenzuweisung

In Azure RBAC entfernen Sie eine Rollenzuweisung, um den Zugriff zu entfernen. Zum Entfernen einer Rollenzuweisung verwenden Sie die REST-API [Rollenzuweisungen – Löschen](/rest/api/authorization/roleassignments/delete). Zum Aufrufen dieser API benötigen Sie Zugriff auf den Vorgang `Microsoft.Authorization/roleAssignments/delete`. Von den integrierten Rollen verfügen nur [Besitzer](built-in-roles.md#owner) und [Benutzerzugriffsadministrator](built-in-roles.md#user-access-administrator) über Zugriff auf diesen Vorgang.

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

## <a name="next-steps"></a>Nächste Schritte

- [Auflisten von Azure-Rollenzuweisungen mithilfe der REST-API](role-assignments-list-rest.md)
- [Bereitstellen von Ressourcen mit Resource Manager-Vorlagen und Resource Manager-REST-API](../azure-resource-manager/templates/deploy-rest.md)
- [Azure REST-API-Referenz](/rest/api/azure/)
- [Erstellen oder Aktualisieren von benutzerdefinierten Rollen in Azure über die REST-API](custom-roles-rest.md)
