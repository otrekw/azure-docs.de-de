---
title: Auflisten von Ablehnungszuweisungen für Azure-Ressourcen mithilfe der REST-API
description: Hier erfahren Sie, wie Sie Ablehnungszuweisungen für Benutzer, Gruppen und Anwendungen mithilfe der rollenbasierten Zugriffssteuerung (Role-Based Access Control, RBAC) für Azure-Ressourcen und der REST-API auflisten.
services: active-directory
documentationcenter: na
author: rolyon
manager: mtillman
editor: ''
ms.assetid: ''
ms.service: role-based-access-control
ms.workload: multiple
ms.tgt_pltfrm: rest-api
ms.devlang: na
ms.topic: conceptual
ms.date: 03/19/2020
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: 0f648405a3d71bf27c64dacbb3fd78f3e9801137
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "80063019"
---
# <a name="list-deny-assignments-for-azure-resources-using-the-rest-api"></a>Auflisten von Ablehnungszuweisungen für Azure-Ressourcen mithilfe der REST-API

[Ablehnungszuweisungen](deny-assignments.md) blockieren Aktionen von Azure-Ressourcen für bestimmte Benutzer, selbst wenn diesen durch eine Rollenzuweisung Zugriff erteilt wurde. In diesem Artikel wird beschrieben, wie Sie mit der REST-API Ablehnungszuweisungen auflisten.

> [!NOTE]
> Sie können Ihre eigenen Ablehnungszuweisungen nicht direkt erstellen. Weitere Informationen dazu, wie Ablehnungszuweisungen erstellt werden, finden Sie unter [Ablehnungszuweisungen](deny-assignments.md).

## <a name="prerequisites"></a>Voraussetzungen

Um Informationen zu einer Ablehnungszuweisung abzurufen, müssen Sie über Folgendes verfügen:

- `Microsoft.Authorization/denyAssignments/read`-Berechtigung, die in den meisten [integrierten Rollen für Azure-Ressourcen](built-in-roles.md) enthalten ist.

## <a name="list-a-single-deny-assignment"></a>Auflisten einer einzelnen Ablehnungszuweisung

1. Beginnen Sie mit der folgenden Anforderung:

    ```http
    GET https://management.azure.com/{scope}/providers/Microsoft.Authorization/denyAssignments/{deny-assignment-id}?api-version=2018-07-01-preview
    ```

1. Ersetzen Sie innerhalb des URI *{scope}* durch den Bereich, für den die Ablehnungszuweisungen aufgelistet werden sollen.

    > [!div class="mx-tableFixed"]
    > | `Scope` | type |
    > | --- | --- |
    > | `subscriptions/{subscriptionId}` | Subscription |
    > | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1` | Resource group |
    > | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1` | Resource |

1. Ersetzen Sie *{deny-assignment-id}* durch den Bezeichner für die Ablehnungszuweisung, die Sie abrufen möchten.

## <a name="list-multiple-deny-assignments"></a>Auflisten von mehreren Ablehnungszuweisungen

1. Beginnen Sie mit einer der folgenden Anforderungen:

    ```http
    GET https://management.azure.com/{scope}/providers/Microsoft.Authorization/denyAssignments?api-version=2018-07-01-preview
    ```

    Mit optionalen Parametern:

    ```http
    GET https://management.azure.com/{scope}/providers/Microsoft.Authorization/denyAssignments?api-version=2018-07-01-preview&$filter={filter}
    ```

1. Ersetzen Sie innerhalb des URI *{scope}* durch den Bereich, für den die Ablehnungszuweisungen aufgelistet werden sollen.

    > [!div class="mx-tableFixed"]
    > | `Scope` | type |
    > | --- | --- |
    > | `subscriptions/{subscriptionId}` | Subscription |
    > | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1` | Resource group |
    > | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1` | Resource |

1. Ersetzen Sie *{filter}* durch die Bedingung, die zum Filtern der Liste mit den Ablehnungszuweisungen angewendet werden soll.

    > [!div class="mx-tableFixed"]
    > | Filtern | BESCHREIBUNG |
    > | --- | --- |
    > | (Kein Filter) | Alle Ablehnungszuweisungen des angegebenen Bereichs (auch ober- und unterhalb) werden aufgelistet. |
    > | `$filter=atScope()` | Nur die Ablehnungszuweisungen für den angegebenen Bereich und oberhalb davon werden aufgelistet. Hierin sind nicht die Ablehnungszuweisungen von Unterbereichen enthalten. |
    > | `$filter=assignedTo('{objectId}')` | Ablehnungszuweisungen für den angegebenen Benutzer oder Dienstprinzipal werden aufgelistet.<br/>Wenn der Benutzer Mitglied einer Gruppe ist, die über eine Ablehnungszuweisung verfügt, wird diese Ablehnungszuweisung ebenfalls aufgeführt. Dieser Filter ist für Gruppen transitiv, das heißt: Wenn der Benutzer Mitglied einer Gruppe und diese Gruppe wiederum Mitglied einer anderen Gruppe ist, die über eine Ablehnungszuweisung verfügt, wird diese Ablehnungszuweisung ebenfalls aufgeführt.<br/>Dieser Filter akzeptiert nur eine Objekt-ID für einen Benutzer oder einen Dienstprinzipal. Für eine Gruppe kann keine Objekt-ID übergeben werden. |
    > | `$filter=atScope()+and+assignedTo('{objectId}')` | Listet die Ablehnungszuweisungen für bestimmte Benutzer oder Dienstprinzipale sowie im angegebenen Umfang auf. |
    > | `$filter=denyAssignmentName+eq+'{deny-assignment-name}'` | Ablehnungszuweisungen mit dem angegebenen Namen werden aufgelistet. |
    > | `$filter=principalId+eq+'{objectId}'` | Ablehnungszuweisungen für den angegebenen Benutzer, die angegebene Gruppe oder den Dienstprinzipal werden aufgelistet. |

## <a name="list-deny-assignments-at-the-root-scope-"></a>Auflisten von Ablehnungszuweisungen im Stammbereich (/)

1. Erhöhen Sie Ihre Zugriffsrechte wie unter [Erhöhen der Zugriffsrechte für einen globalen Administrator in Azure Active Directory](elevate-access-global-admin.md) beschrieben.

1. Verwenden Sie die folgende Anforderung:

    ```http
    GET https://management.azure.com/providers/Microsoft.Authorization/denyAssignments?api-version=2018-07-01-preview&$filter={filter}
    ```

1. Ersetzen Sie *{filter}* durch die Bedingung, die zum Filtern der Liste mit den Ablehnungszuweisungen angewendet werden soll. Ein Filter ist erforderlich.

    > [!div class="mx-tableFixed"]
    > | Filtern | BESCHREIBUNG |
    > | --- | --- |
    > | `$filter=atScope()` | Nur Ablehnungszuweisungen für den Stammbereich werden aufgelistet. Hierin sind nicht die Ablehnungszuweisungen von Unterbereichen enthalten. |
    > | `$filter=denyAssignmentName+eq+'{deny-assignment-name}'` | Ablehnungszuweisungen mit dem angegebenen Namen werden aufgelistet. |

1. Entfernen Sie die erhöhten Zugriffsrechte.

## <a name="next-steps"></a>Nächste Schritte

- [Verstehen von Ablehnungszuweisungen für Azure-Ressourcen](deny-assignments.md)
- [Erhöhen der Zugriffsrechte für einen globalen Administrator in Azure Active Directory](elevate-access-global-admin.md)
- [Azure REST-API-Referenz](/rest/api/azure/)
