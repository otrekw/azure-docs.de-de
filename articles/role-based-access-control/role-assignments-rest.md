---
title: Hinzufügen oder Entfernen von Rollenzuweisungen mithilfe von Azure RBAC und der REST-API
description: Erfahren Sie, wie Sie den Zugriff auf Azure-Ressourcen für Benutzer, Gruppen, Dienstprinzipale und verwaltete Identitäten mit der rollenbasierten Zugriffssteuerung (Role-Based Access Control, RBAC) und der REST-API verwalten.
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
ms.date: 11/25/2019
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: 3c7b7dac649548b8b21cc13761009c11609c8904
ms.sourcegitcommit: 3dc1a23a7570552f0d1cc2ffdfb915ea871e257c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/15/2020
ms.locfileid: "75981033"
---
# <a name="add-or-remove-role-assignments-using-azure-rbac-and-the-rest-api"></a>Hinzufügen oder Entfernen von Rollenzuweisungen mithilfe von Azure RBAC und der REST-API

[!INCLUDE [Azure RBAC definition grant access](../../includes/role-based-access-control-definition-grant.md)] In diesem Artikel wird die Zuweisung von Rollen mithilfe der REST-API beschrieben.

## <a name="prerequisites"></a>Voraussetzungen

Sie benötigen Folgendes, um Rollenzuweisungen hinzufügen oder entfernen zu können:

- `Microsoft.Authorization/roleAssignments/write`- und `Microsoft.Authorization/roleAssignments/delete`-Berechtigungen, wie z.B. [Benutzerzugriffsadministrator](built-in-roles.md#user-access-administrator) oder [Besitzer](built-in-roles.md#owner)

## <a name="add-a-role-assignment"></a>Hinzufügen einer Rollenzuweisung

In der RBAC fügen Sie zum Gewähren des Zugriffs eine Rollenzuweisung hinzu. Um eine Rollenzuweisung hinzuzufügen, verwenden Sie die REST-API unter [Rollenzuweisungen – Erstellen](/rest/api/authorization/roleassignments/create), und geben Sie den Sicherheitsprinzipal, die Rollendefinition und den Bereich an. Zum Aufrufen dieser API benötigen Sie Zugriff auf den Vorgang `Microsoft.Authorization/roleAssignments/write`. Von den integrierten Rollen verfügen nur [Besitzer](built-in-roles.md#owner) und [Benutzerzugriffsadministrator](built-in-roles.md#user-access-administrator) über Zugriff auf diesen Vorgang.

1. Verwenden Sie die REST API unter [Rollendefinitionen – Auflisten](/rest/api/authorization/roledefinitions/list) oder lesen Sie [Integrierte Rollen in Azure](built-in-roles.md), um den Bezeichner für die zuzuweisende Rollendefinition zu ermitteln.

1. Verwenden Sie ein GUID-Tool, um einen eindeutigen Bezeichner zu generieren, der für den Bezeichner der Rollenzuweisung verwendet wird. Das Format des Bezeichners ist: `00000000-0000-0000-0000-000000000000`

1. Beginnen Sie mit folgender Anforderung und folgendem Text:

    ```http
    PUT https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments/{roleAssignmentName}?api-version=2015-07-01
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

    | `Scope` | type |
    | --- | --- |
    | `providers/Microsoft.Management/managementGroups/{groupId1}` | Verwaltungsgruppe |
    | `subscriptions/{subscriptionId1}` | Subscription |
    | `subscriptions/{subscriptionId1}/resourceGroups/myresourcegroup1` | Resource group |
    | `subscriptions/{subscriptionId1}/resourceGroups/myresourcegroup1/ providers/microsoft.web/sites/mysite1` | Resource |

1. Ersetzen Sie *{roleAssignmentName}* durch den GUID-Bezeichner der Rollenzuweisung.

1. Ersetzen Sie innerhalb des Anforderungstexts *{scope}* durch den Bereich für die Rollenzuweisung.

    | `Scope` | type |
    | --- | --- |
    | `providers/Microsoft.Management/managementGroups/{groupId1}` | Verwaltungsgruppe |
    | `subscriptions/{subscriptionId1}` | Subscription |
    | `subscriptions/{subscriptionId1}/resourceGroups/myresourcegroup1` | Resource group |
    | `subscriptions/{subscriptionId1}/resourceGroups/myresourcegroup1/ providers/microsoft.web/sites/mysite1` | Resource |

1. Ersetzen Sie *{roleDefinitionId}* durch den Bezeichner der Rollendefinition.

1. Ersetzen Sie *{principalId}* durch den Objektbezeichner des Benutzers, der Gruppe oder des Dienstprinzipals, der bzw. die der Rolle zugewiesen wird.

## <a name="remove-a-role-assignment"></a>Entfernen einer Rollenzuweisung

In RBAC entfernen Sie eine Rollenzuweisung, um den Zugriff zu entfernen. Zum Entfernen einer Rollenzuweisung verwenden Sie die REST-API [Rollenzuweisungen – Löschen](/rest/api/authorization/roleassignments/delete). Zum Aufrufen dieser API benötigen Sie Zugriff auf den Vorgang `Microsoft.Authorization/roleAssignments/delete`. Von den integrierten Rollen verfügen nur [Besitzer](built-in-roles.md#owner) und [Benutzerzugriffsadministrator](built-in-roles.md#user-access-administrator) über Zugriff auf diesen Vorgang.

1. Rufen Sie den Bezeichner der Rollenzuweisung (GUID) ab. Dieser Bezeichner wird beim ersten Erstellen der Rollenzuweisung zurückgegeben oder kann durch Auflisten der Rollenzuweisungen ermittelt werden.

1. Beginnen Sie mit der folgenden Anforderung:

    ```http
    DELETE https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments/{roleAssignmentName}?api-version=2015-07-01
    ```

1. Ersetzen Sie innerhalb des URIs *{scope}* durch den Bereich, um die Rollenzuweisung zu entfernen.

    | `Scope` | type |
    | --- | --- |
    | `providers/Microsoft.Management/managementGroups/{groupId1}` | Verwaltungsgruppe |
    | `subscriptions/{subscriptionId1}` | Subscription |
    | `subscriptions/{subscriptionId1}/resourceGroups/myresourcegroup1` | Resource group |
    | `subscriptions/{subscriptionId1}/resourceGroups/myresourcegroup1/ providers/microsoft.web/sites/mysite1` | Resource |

1. Ersetzen Sie *{roleAssignmentName}* durch den GUID-Bezeichner der Rollenzuweisung.

## <a name="next-steps"></a>Nächste Schritte

- [Auflisten von Rollenzuweisungen mithilfe von Azure RBAC und der REST-API](role-assignments-list-rest.md)
- [Bereitstellen von Ressourcen mit Resource Manager-Vorlagen und Resource Manager-REST-API](../azure-resource-manager/templates/deploy-rest.md)
- [Azure REST-API-Referenz](/rest/api/azure/)
- [Erstellen von benutzerdefinierten Rollen für Azure-Ressourcen mithilfe der REST-API](custom-roles-rest.md)
