---
title: 'Azure RBAC: Hinzufügen und Bearbeiten der Bedingungen für die Azure-Rollenzuweisung mithilfe der Azure CLI (Vorschau)'
description: Hier erfahren Sie, wie Sie Bedingungen der attributbasierten Zugriffssteuerung (Attribute-Based Access Control, ABAC) in Azure-Rollenzuweisungen über die Azure CLI und unter Verwendung der rollenbasierten Zugriffssteuerung von Azure (Role-Based Access Control, Azure RBAC) hinzufügen, auflisten, anzeigen oder löschen.
services: active-directory
author: rolyon
manager: mtillman
ms.service: role-based-access-control
ms.subservice: conditions
ms.topic: how-to
ms.workload: identity
ms.date: 05/07/2021
ms.author: rolyon
ms.openlocfilehash: c7baafbd34e9af488fbbee6237625aa469dff12c
ms.sourcegitcommit: 3de22db010c5efa9e11cffd44a3715723c36696a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/10/2021
ms.locfileid: "109656630"
---
# <a name="add-or-edit-azure-role-assignment-conditions-using-azure-cli-preview"></a>Hinzufügen und Bearbeiten der Bedingungen für die Azure-Rollenzuweisung mithilfe der Azure CLI (Vorschau)

> [!IMPORTANT]
> Azure ABAC und Azure-Rollenzuweisungsbedingungen befinden sich derzeit in der Vorschauphase.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar.
> Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Eine [Azure-Rollenzuweisungsbedingung](conditions-overview.md) ist eine zusätzliche Überprüfung, die Sie Ihrer Rollenzuweisung optional hinzufügen können, um die Zugriffssteuerung präziser zu gestalten. So können Sie beispielsweise eine Bedingung hinzufügen, die festlegt, dass ein Objekt über ein bestimmtes Tag verfügen muss, damit das Objekt gelesen werden kann. In diesem Artikel wird beschrieben, wie Sie Bedingungen mithilfe der Azure CLI für Ihre Rollenzuweisungen hinzufügen, bearbeiten, auflisten und löschen können.

## <a name="prerequisites"></a>Voraussetzungen

Informationen zu den Voraussetzungen für das Hinzufügen oder Bearbeiten von Rollenzuweisungsbedingungen finden Sie unter [Voraussetzungen für Bedingungen](conditions-prerequisites.md).

## <a name="add-a-condition"></a>Bedingung hinzufügen

Sie verwenden [az role assignment create](/cli/azure/role/assignment#az_role_assignment_create), um eine Rollenzuweisungsbedingung hinzuzufügen. Der Befehl [az role assignment create](/cli/azure/role/assignment#az_role_assignment_create) umfasst die folgenden Parameter bezüglich Bedingungen.

| Parameter | type | BESCHREIBUNG |
| --- | --- | --- |
| `condition` | String | Hierbei handelt es sich um die Bedingung, unter der dem Benutzer die Berechtigung erteilt werden kann. |
| `condition-version` | String | Hierbei handelt es sich um die Version der Bedingungssyntax. Wenn `--condition` ohne `--condition-version` angegeben wird, wird die Standardversion 2.0 festgelegt. |

Im folgenden Beispiel wird gezeigt, wie Sie die Rolle [Storage-Blobdatenleser](built-in-roles.md#storage-blob-data-reader) mit einer Bedingung zuweisen. Die Bedingung überprüft, ob der Containername „blobs-example-container“ entspricht.

```azurecli
az role assignment create --role "Storage Blob Data Reader" --assignee "user1@contoso.com" --resource-group {resourceGroup} \
--description "Read access if container name equals blobs-example-container" \
--condition "((!(ActionMatches{'Microsoft.Storage/storageAccounts/blobServices/containers/blobs/read'})) OR (@Resource[Microsoft.Storage/storageAccounts/blobServices/containers:name] StringEquals 'blobs-example-container'))" \
--condition-version "2.0"
```

Nachfolgend sehen Sie ein Beispiel für die Ausgabe:

```azurecli
{
    "canDelegate": null,
    "condition": "((!(ActionMatches{'Microsoft.Storage/storageAccounts/blobServices/containers/blobs/read'})) OR (@Resource[Microsoft.Storage/storageAccounts/blobServices/containers:name] StringEquals 'blobs-example-container'))",
    "conditionVersion": "2.0",
    "description": "Read access if container name equals blobs-example-container",
    "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroup}/providers/Microsoft.Authorization/roleAssignments/{roleAssignmentId}",
    "name": "{roleAssignmentId}",
    "principalId": "{userObjectId}",
    "principalType": "User",
    "resourceGroup": "{resourceGroup}",
    "roleDefinitionId": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/2a2b9908-6ea1-4ae2-8e65-a410df84e7d1",
    "scope": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroup}",
    "type": "Microsoft.Authorization/roleAssignments"
}
```

## <a name="edit-a-condition"></a>Bearbeiten einer Bedingung

Verwenden Sie [az role assignment update](/cli/azure/role/assignment#az_role_assignment_update) und eine JSON-Datei als Eingabe, um eine vorhandene Rollenzuweisungsbedingung zu bearbeiten. Das folgende Beispiel zeigt eine JSON-Beispieldatei, in der Bedingung und Beschreibung aktualisiert werden. Nur die Eigenschaften `condition`, `conditionVersion` und `description` können bearbeitet werden. Sie müssen alle Eigenschaften angeben, um die Bedingung für die Rollenzuweisung zu aktualisieren.

```json
{
    "canDelegate": null,
    "condition": "((!(ActionMatches{'Microsoft.Storage/storageAccounts/blobServices/containers/blobs/read'})) OR (@Resource[Microsoft.Storage/storageAccounts/blobServices/containers:name] StringEquals 'blobs-example-container' OR @Resource[Microsoft.Storage/storageAccounts/blobServices/containers:name] StringEquals 'blobs-example-container2'))",
    "conditionVersion": "2.0",
    "description": "Read access if container name equals blobs-example-container or blobs-example-container2",
    "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroup}/providers/Microsoft.Authorization/roleAssignments/{roleAssignmentId}",
    "name": "{roleAssignmentId}",
    "principalId": "{userObjectId}",
    "principalType": "User",
    "resourceGroup": "{resourceGroup}",
    "roleDefinitionId": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/2a2b9908-6ea1-4ae2-8e65-a410df84e7d1",
    "scope": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroup}",
    "type": "Microsoft.Authorization/roleAssignments"
}
```

Aktualisieren Sie mithilfe von [az role assignment update](/cli/azure/role/assignment#az_role_assignment_update) die Bedingung für die Rollenzuweisung.

```azurecli
az role assignment update --role-assignment "./path/roleassignment.json"
```

Nachfolgend sehen Sie ein Beispiel für die Ausgabe:

```azurecli
{
    "canDelegate": null,
    "condition": "((!(ActionMatches{'Microsoft.Storage/storageAccounts/blobServices/containers/blobs/read'})) OR (@Resource[Microsoft.Storage/storageAccounts/blobServices/containers:name] StringEquals 'blobs-example-container' OR @Resource[Microsoft.Storage/storageAccounts/blobServices/containers:name] StringEquals 'blobs-example-container2'))",
    "conditionVersion": "2.0",
    "description": "Read access if container name equals blobs-example-container or blobs-example-container2",
    "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroup}/providers/Microsoft.Authorization/roleAssignments/{roleAssignmentId}",
    "name": "{roleAssignmentId}",
    "principalId": "{userObjectId}",
    "principalType": "User",
    "resourceGroup": "{resourceGroup}",
    "roleDefinitionId": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/2a2b9908-6ea1-4ae2-8e65-a410df84e7d1",
    "scope": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroup}",
    "type": "Microsoft.Authorization/roleAssignments"
}
```

## <a name="list-a-condition"></a>Auflisten einer Bedingung

Sie verwenden [az role assignment list](/cli/azure/role/assignment#az_role_assignment_list), um eine Rollenzuweisungsbedingung aufzulisten. Weitere Informationen finden Sie unter [Auflisten von Azure-Rollenzuweisungen mithilfe der Azure-Befehlszeilenschnittstelle](role-assignments-list-cli.md).

## <a name="delete-a-condition"></a>Löschen einer Bedingung

Zum Löschen einer Rollenzuweisungsbedingung bearbeiten Sie die Rollenzuweisungsbedingung und legen sowohl die Eigenschaft `condition` als auch `condition-version` entweder auf eine leere Zeichenfolge (`""`) oder `null` fest.

Alternativ können Sie den Befehl [az role assignment delete](/cli/azure/role/assignment#az_role_assignment_delete) verwenden, wenn Sie sowohl die Rollenzuweisung als auch die Bedingung löschen möchten. Weitere Informationen finden Sie unter [Entfernen von Azure-Rollenzuweisungen](role-assignments-remove.md).

## <a name="next-steps"></a>Nächste Schritte

- [Beispiele für Azure-Rollenzuweisungsbedingungen (Vorschau)](../storage/common/storage-auth-abac-examples.md)
- [Tutorial: Hinzufügen einer Rollenzuweisungsbedingung zum Einschränken des Zugriffs auf Blobs mithilfe der Azure CLI (Vorschau)](../storage/common/storage-auth-abac-cli.md)
- [Problembehandlung: Bedingungen für die Azure-Rollenzuweisung (Vorschau)](conditions-troubleshoot.md)
