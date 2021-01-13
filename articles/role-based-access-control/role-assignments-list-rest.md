---
title: Auflisten von Azure-Rollenzuweisungen mithilfe der REST-API – Azure RBAC
description: Erfahren Sie, wie Sie mithilfe der REST-API und der rollenbasierten Zugriffssteuerung in Azure (Azure RBAC) ermitteln können, auf welche Ressourcen Benutzer, Gruppen, Dienstprinzipale und verwaltete Identitäten Zugriff haben.
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
ms.topic: how-to
ms.date: 05/06/2020
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: b717d84c8fedf4a87c0b81752b14914ece185250
ms.sourcegitcommit: 1bdcaca5978c3a4929cccbc8dc42fc0c93ca7b30
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/13/2020
ms.locfileid: "97369207"
---
# <a name="list-azure-role-assignments-using-the-rest-api"></a>Auflisten von Azure-Rollenzuweisungen mithilfe der REST-API

[!INCLUDE [Azure RBAC definition list access](../../includes/role-based-access-control-definition-list.md)] In diesem Artikel wird beschrieben, wie Sie mit der REST-API Rollenzuweisungen auflisten.

> [!NOTE]
> Wenn Ihre Organisation Verwaltungsfunktionen an einen Dienstanbieter ausgelagert hat, der die [delegierte Azure-Ressourcenverwaltung](../lighthouse/concepts/azure-delegated-resource-management.md) verwendet, werden die von diesem Dienstanbieter autorisierten Rollenzuweisungen hier nicht angezeigt.

## <a name="list-role-assignments"></a>Auflisten der Rollenzuweisungen

Zum Auflisten des Zugriffs in Azure RBAC führen Sie die Rollenzuweisungen auf. Um Rollenzuweisungen aufzulisten, verwenden Sie eine der REST-APIs unter [Rollenzuweisungen – Auflisten](/rest/api/authorization/roleassignments/list). Um die Ergebnisse einzugrenzen, geben Sie einen Bereich und einen optionalen Filter an.

1. Beginnen Sie mit der folgenden Anforderung:

    ```http
    GET https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments?api-version=2015-07-01&$filter={filter}
    ```

1. Ersetzen Sie innerhalb des URIs *{scope}* durch den Bereich, für den die Rollen aufgelistet werden sollen.

    > [!div class="mx-tableFixed"]
    > | `Scope` | type |
    > | --- | --- |
    > | `providers/Microsoft.Management/managementGroups/{groupId1}` | Verwaltungsgruppe |
    > | `subscriptions/{subscriptionId1}` | Subscription |
    > | `subscriptions/{subscriptionId1}/resourceGroups/myresourcegroup1` | Resource group |
    > | `subscriptions/{subscriptionId1}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1` | Resource |

    Im vorherigen Beispiel ist „microsoft.web“ ein Ressourcenanbieter, der auf eine App Service-Instanz verweist. Analog dazu können Sie einen beliebigen anderen Ressourcenanbieter verwenden und den Bereich angeben. Weitere Informationen finden Sie unter [Azure-Ressourcenanbieter und -typen](../azure-resource-manager/management/resource-providers-and-types.md) und unter unterstützten [Vorgängen für Azure-Ressourcenanbieter](resource-provider-operations.md).  
     
1. Ersetzen Sie *{filter}* durch die Bedingung, die zum Filtern der Liste mit den Rollenzuweisungen angewendet werden soll.

    > [!div class="mx-tableFixed"]
    > | Filtern | BESCHREIBUNG |
    > | --- | --- |
    > | `$filter=atScope()` | Listet nur die Rollenzuweisungen für den angegebenen Bereich auf, ohne die Rollenzuweisungen der Unterbereiche. |
    > | `$filter=assignedTo('{objectId}')` | Listet die Rollenzuweisungen für bestimmte Benutzer oder Dienstprinzipale auf.<br/>Wenn der Benutzer Mitglied einer Gruppe ist, die über eine Rollenzuweisung verfügt, wird diese Rollenzuweisung ebenfalls aufgeführt. Dieser Filter ist für Gruppen transitiv, das heißt: Wenn der Benutzer Mitglied einer Gruppe und diese Gruppe wiederum Mitglied einer anderen Gruppe ist, die über eine Rollenzuweisung verfügt, wird diese Rollenzuweisung ebenfalls aufgeführt.<br/>Dieser Filter akzeptiert nur eine Objekt-ID für einen Benutzer oder einen Dienstprinzipal. Für eine Gruppe kann keine Objekt-ID übergeben werden. |
    > | `$filter=atScope()+and+assignedTo('{objectId}')` | Listet die Rollenzuweisungen für bestimmte Benutzer oder Dienstprinzipale sowie im angegebenen Umfang auf. |
    > | `$filter=principalId+eq+'{objectId}'` | Listet die Rollenzuweisungen für bestimmte Benutzer, Gruppen oder Dienstprinzipale auf. |

Mit der folgenden Anforderung werden alle Rollenzuweisungen für den angegebenen Benutzer im Abonnementbereich angezeigt:

```http
GET https://management.azure.com/subscriptions/{subscriptionId1}/providers/Microsoft.Authorization/roleAssignments?api-version=2015-07-01&$filter=atScope()+and+assignedTo('{objectId1}')
```

Nachfolgend sehen Sie ein Beispiel für die Ausgabe:

```json
{
    "value": [
        {
            "properties": {
                "roleDefinitionId": "/subscriptions/{subscriptionId1}/providers/Microsoft.Authorization/roleDefinitions/2a2b9908-6ea1-4ae2-8e65-a410df84e7d1",
                "principalId": "{objectId1}",
                "scope": "/subscriptions/{subscriptionId1}",
                "createdOn": "2019-01-15T21:08:45.4904312Z",
                "updatedOn": "2019-01-15T21:08:45.4904312Z",
                "createdBy": "{createdByObjectId1}",
                "updatedBy": "{updatedByObjectId1}"
            },
            "id": "/subscriptions/{subscriptionId1}/providers/Microsoft.Authorization/roleAssignments/{roleAssignmentId1}",
            "type": "Microsoft.Authorization/roleAssignments",
            "name": "{roleAssignmentId1}"
        }
    ]
}
```

## <a name="next-steps"></a>Nächste Schritte

- [Hinzufügen oder Entfernen von Azure-Rollenzuweisungen mithilfe der REST-API](role-assignments-rest.md)
- [Azure REST-API-Referenz](/rest/api/azure/)
