---
title: Auflisten von Rollenzuweisungen mithilfe von Azure RBAC und der REST-API
description: Erfahren Sie, wie Sie mit der rollenbasierten Zugriffssteuerung (Role-Based Access Control, RBAC) in Azure und der REST-API ermitteln können, auf welche Ressourcen Benutzer, Gruppen, Dienstprinzipale und verwaltete Identitäten Zugriff haben.
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
ms.openlocfilehash: e20edcb5e2406c216711a2e0f696ef06e19fe21e
ms.sourcegitcommit: c69c8c5c783db26c19e885f10b94d77ad625d8b4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/03/2019
ms.locfileid: "74709882"
---
# <a name="list-role-assignments-using-azure-rbac-and-the-rest-api"></a>Auflisten von Rollenzuweisungen mithilfe von Azure RBAC und der REST-API

[!INCLUDE [Azure RBAC definition list access](../../includes/role-based-access-control-definition-list.md)] In diesem Artikel wird beschrieben, wie Sie mit der REST-API Rollenzuweisungen auflisten.

## <a name="list-role-assignments"></a>Auflisten der Rollenzuweisungen

Zum Auflisten des Zugriffs in RBAC führen Sie die Rollenzuweisungen auf. Um Rollenzuweisungen aufzulisten, verwenden Sie eine der REST-APIs unter [Rollenzuweisungen – Auflisten](/rest/api/authorization/roleassignments/list). Um die Ergebnisse einzugrenzen, geben Sie einen Bereich und einen optionalen Filter an.

1. Beginnen Sie mit der folgenden Anforderung:

    ```http
    GET https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments?api-version=2015-07-01&$filter={filter}
    ```

1. Ersetzen Sie innerhalb des URIs *{scope}* durch den Bereich, für den die Rollen aufgelistet werden sollen.

    | `Scope` | type |
    | --- | --- |
    | `providers/Microsoft.Management/managementGroups/{groupId1}` | Verwaltungsgruppe |
    | `subscriptions/{subscriptionId1}` | Subscription |
    | `subscriptions/{subscriptionId1}/resourceGroups/myresourcegroup1` | Resource group |
    | `subscriptions/{subscriptionId1}/resourceGroups/myresourcegroup1/ providers/Microsoft.Web/sites/mysite1` | Resource |

    Im vorherigen Beispiel ist „microsoft.web“ ein Ressourcenanbieter, der auf eine App Service-Instanz verweist. Analog dazu können Sie einen beliebigen anderen Ressourcenanbieter verwenden und den Bereich angeben. Weitere Informationen finden Sie unter [Azure-Ressourcenanbieter und -typen](../azure-resource-manager/resource-manager-supported-services.md) und unter unterstützten [Vorgängen für Azure Resource Manager-Ressourcenanbieter](resource-provider-operations.md).  
     
1. Ersetzen Sie *{filter}* durch die Bedingung, die zum Filtern der Liste mit den Rollenzuweisungen angewendet werden soll.

    | Filter | BESCHREIBUNG |
    | --- | --- |
    | `$filter=atScope()` | Listet nur die Rollenzuweisungen für den angegebenen Bereich auf, ohne die Rollenzuweisungen der Unterbereiche. |
    | `$filter=principalId%20eq%20'{objectId}'` | Listet die Rollenzuweisungen für bestimmte Benutzer, Gruppen oder Dienstprinzipale auf. |
    | `$filter=assignedTo('{objectId}')` | Listet die Rollenzuweisungen für bestimmte Benutzer oder Dienstprinzipale auf. Wenn der Benutzer Mitglied einer Gruppe ist, die über eine Rollenzuweisung verfügt, wird diese Rollenzuweisung ebenfalls aufgeführt. Dieser Filter ist für Gruppen transitiv, das heißt: Wenn der Benutzer Mitglied einer Gruppe und diese Gruppe wiederum Mitglied einer anderen Gruppe ist, die über eine Rollenzuweisung verfügt, wird diese Rollenzuweisung ebenfalls aufgeführt. Dieser Filter akzeptiert nur eine Objekt-ID für einen Benutzer oder einen Dienstprinzipal. Für eine Gruppe kann keine Objekt-ID übergeben werden. |

## <a name="next-steps"></a>Nächste Schritte

- [Hinzufügen oder Entfernen von Rollenzuweisungen mithilfe von Azure RBAC und der REST-API](role-assignments-rest.md)
- [Azure REST-API-Referenz](/rest/api/azure/)
