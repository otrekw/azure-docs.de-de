---
title: 'Azure RBAC: Hinzufügen und Bearbeiten der Bedingungen für die Azure-Rollenzuweisung mithilfe der REST-API (Vorschau)'
description: Hier erfahren Sie, wie Sie Bedingungen der attributbasierten Zugriffssteuerung (Attribute-Based Access Control, ABAC) in Azure-Rollenzuweisungen über die REST-API und unter Verwendung der rollenbasierten Zugriffssteuerung von Azure (Role-Based Access Control, Azure RBAC) hinzufügen, auflisten, anzeigen oder löschen.
services: active-directory
author: rolyon
manager: mtillman
ms.service: role-based-access-control
ms.subservice: conditions
ms.topic: how-to
ms.workload: identity
ms.date: 05/07/2021
ms.author: rolyon
ms.openlocfilehash: 4a929fd99f10e66ea07dacc0302dd6d0bba83a96
ms.sourcegitcommit: 3de22db010c5efa9e11cffd44a3715723c36696a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/10/2021
ms.locfileid: "109656558"
---
# <a name="add-or-edit-azure-role-assignment-conditions-using-the-rest-api-preview"></a>Hinzufügen und Bearbeiten der Bedingungen für die Azure-Rollenzuweisung mithilfe der REST-API (Vorschau)

> [!IMPORTANT]
> Die attributbasierte Zugriffssteuerung von Azure und Azure-Rollenzuweisungsbedingungen befinden sich derzeit in der Vorschauphase.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar.
> Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Eine [Azure-Rollenzuweisungsbedingung](conditions-overview.md) ist eine zusätzliche Überprüfung, die Sie Ihrer Rollenzuweisung optional hinzufügen können, um die Zugriffssteuerung präziser zu gestalten. So können Sie beispielsweise eine Bedingung hinzufügen, die festlegt, dass ein Objekt über ein bestimmtes Tag verfügen muss, damit das Objekt gelesen werden kann. In diesem Artikel wird beschrieben, wie Sie Bedingungen mithilfe der REST-API für Ihre Rollenzuweisungen hinzufügen, bearbeiten, auflisten und löschen können.

## <a name="prerequisites"></a>Voraussetzungen

Informationen zu den Voraussetzungen für das Hinzufügen oder Bearbeiten von Rollenzuweisungsbedingungen finden Sie unter [Voraussetzungen für Bedingungen](conditions-prerequisites.md).

## <a name="add-a-condition"></a>Bedingung hinzufügen

Verwenden Sie den Vorgang [Role Assignments – Create](/rest/api/authorization/roleassignments/create) (Rollenzuweisungen: Erstellen) REST-API, um eine Rollenzuweisungsbedingung hinzuzufügen. Legen Sie `api-version` auf `2020-03-01-preview` oder höher fest. Wenn Sie die Eigenschaft `description` für Rollenzuweisungen verwenden möchten, müssen Sie `2020-04-01-preview` oder höher festlegen.  Der Vorgang [Role Assignments – Create](/rest/api/authorization/roleassignments/create) umfasst die folgenden Parameter bezüglich Bedingungen.

| Parameter | type | BESCHREIBUNG |
| --- | --- | --- |
| `condition` | String | Hierbei handelt es sich um die Bedingung, unter der dem Benutzer die Berechtigung erteilt werden kann. |
| `conditionVersion` | String | Hierbei handelt es sich um die Version der Bedingungssyntax. Wenn `condition` ohne `conditionVersion` angegeben wird, wird die Standardversion 2.0 festgelegt. |

Verwenden Sie die folgende Anforderung und den folgenden Text:

```http
PUT https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments/{roleAssignmentId}?api-version=2020-04-01-preview
```

```json
{
    "properties": {
        "roleDefinitionId": "/{scope}/providers/Microsoft.Authorization/roleDefinitions/{roleDefinitionId}",
        "principalId": "{principalId}",
        "condition": "{condition}",
        "conditionVersion": "2.0",
        "description": "{description}"
    }
}
```

Im folgenden Beispiel wird gezeigt, wie Sie die Rolle [Storage-Blobdatenleser](built-in-roles.md#storage-blob-data-reader) mit einer Bedingung zuweisen. Die Bedingung überprüft, ob der Containername „blobs-example-container“ entspricht.

```http
PUT https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Authorization/roleAssignments/{roleAssignmentId}?api-version=2020-04-01-preview
```

```json
{
    "properties": {
        "roleDefinitionId": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Authorization/roleDefinitions/2a2b9908-6ea1-4ae2-8e65-a410df84e7d1",
        "principalId": "{principalId}",
        "condition": "((!(ActionMatches{'Microsoft.Storage/storageAccounts/blobServices/containers/blobs/read'})) OR (@Resource[Microsoft.Storage/storageAccounts/blobServices/containers:name] StringEquals 'blobs-example-container'))",
        "conditionVersion": "2.0",
        "description": "Read access if container name equals blobs-example-container"
    }
}
```

Nachfolgend sehen Sie ein Beispiel für die Ausgabe:

```json
{
    "properties": {
        "roleDefinitionId": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Authorization/roleDefinitions/2a2b9908-6ea1-4ae2-8e65-a410df84e7d1",
        "principalId": "{principalId}",
        "principalType": "User",
        "scope": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}",
        "condition": "((!(ActionMatches{'Microsoft.Storage/storageAccounts/blobServices/containers/blobs/read'})) OR (@Resource[Microsoft.Storage/storageAccounts/blobServices/containers:name] StringEquals 'blobs-example-container'))",
        "conditionVersion": "2.0",
        "createdOn": "2021-04-20T06:20:44.0205560Z",
        "updatedOn": "2021-04-20T06:20:44.2955371Z",
        "createdBy": null,
        "updatedBy": "{updatedById}",
        "delegatedManagedIdentityResourceId": null,
        "description": "Read access if container name equals blobs-example-container"
    },
    "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Authorization/roleAssignments/{roleAssignmentId}",
    "type": "Microsoft.Authorization/roleAssignments",
    "name": "{roleAssignmentId}"
}
```

## <a name="edit-a-condition"></a>Bearbeiten einer Bedingung

Zum Bearbeiten einer vorhandenen Rollenzuweisungsbedingung verwenden Sie denselben REST-API-Vorgang ([Role Assignments – Create](/rest/api/authorization/roleassignments/create)), den Sie zum Hinzufügen der Rollenzuweisungsbedingung verwendet haben. Im folgenden JSON-Beispielcode wurden `condition` und `description` geändert. Nur die Eigenschaften `condition`, `conditionVersion` und `description` können bearbeitet werden. Sie müssen die anderen Eigenschaften entsprechend der vorhandenen Rollenzuweisung angeben.

```http
PUT https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Authorization/roleAssignments/{roleAssignmentId}?api-version=2020-04-01-preview
```

```json
{
    "properties": {
        "roleDefinitionId": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Authorization/roleDefinitions/2a2b9908-6ea1-4ae2-8e65-a410df84e7d1",
        "principalId": "{principalId}",
        "condition": "((!(ActionMatches{'Microsoft.Storage/storageAccounts/blobServices/containers/blobs/read'})) OR (@Resource[Microsoft.Storage/storageAccounts/blobServices/containers:name] StringEquals 'blobs-example-container' OR @Resource[Microsoft.Storage/storageAccounts/blobServices/containers:name] StringEquals 'blobs-example-container2'))",
        "conditionVersion": "2.0",
        "description": "Read access if container name equals blobs-example-container or blobs-example-container2"
    }
}
```


## <a name="list-a-condition"></a>Auflisten einer Bedingung

Verwenden Sie den REST-API-Vorgang [Role Assignments – List](/rest/api/authorization/roleassignments/list) (Rollenzuweisungen: Auflisten), um eine Rollenzuweisungsbedingung aufzulisten. Legen Sie `api-version` auf `2020-03-01-preview` oder höher fest. Wenn Sie die Eigenschaft `description` für Rollenzuweisungen verwenden möchten, müssen Sie `2020-04-01-preview` oder höher festlegen. Weitere Informationen finden Sie unter [Auflisten von Azure-Rollenzuweisungen mithilfe der REST-API](role-assignments-list-rest.md).

## <a name="delete-a-condition"></a>Löschen einer Bedingung

Zum Löschen einer Rollenzuweisungsbedingung bearbeiten Sie die Rollenzuweisungsbedingung und legen sowohl für die Bedingung als auch für die Bedingungsversion entweder eine leere Zeichenfolge oder den Wert NULL fest.

Alternativ können Sie den REST-API-Vorgang [Role Assignments – Delete](/rest/api/authorization/roleassignments/delete) (Rollenzuweisungen: Löschen) verwenden, wenn Sie sowohl die Rollenzuweisung als auch die Bedingung löschen möchten. Weitere Informationen finden Sie unter [Entfernen von Azure-Rollenzuweisungen](role-assignments-remove.md).

## <a name="next-steps"></a>Nächste Schritte

- [Beispiele für Azure-Rollenzuweisungsbedingungen (Vorschau)](../storage/common/storage-auth-abac-examples.md)
- [Tutorial: Hinzufügen einer Rollenzuweisungsbedingung zum Einschränken des Zugriffs auf Blobs mit dem Azure-Portal (Vorschau)](../storage/common/storage-auth-abac-portal.md)
- [Problembehandlung für Azure-Rollenzuweisungsbedingungen (Vorschau)](conditions-troubleshoot.md)
