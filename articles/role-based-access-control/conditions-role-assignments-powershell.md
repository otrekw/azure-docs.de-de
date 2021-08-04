---
title: 'Hinzufügen oder Bearbeiten der Bedingungen für die Azure-Rollenzuweisung mithilfe von Azure PowerShell (Vorschau): Azure RBAC'
description: Hier erfahren Sie, wie Sie Bedingungen der attributbasierten Zugriffssteuerung (Attribute-Based Access Control, ABAC) in Azure-Rollenzuweisungen über Azure PowerShell und unter Verwendung der rollenbasierten Zugriffssteuerung von Azure (Role-Based Access Control, Azure RBAC) hinzufügen, bearbeiten, auflisten oder löschen.
services: active-directory
author: rolyon
manager: mtillman
ms.service: role-based-access-control
ms.subservice: conditions
ms.topic: how-to
ms.workload: identity
ms.date: 05/07/2021
ms.author: rolyon
ms.openlocfilehash: 0ecc87a59d6db01c5c5dc4093bb42e4ee9c190d9
ms.sourcegitcommit: 3de22db010c5efa9e11cffd44a3715723c36696a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/10/2021
ms.locfileid: "109656576"
---
# <a name="add-or-edit-azure-role-assignment-conditions-using-azure-powershell-preview"></a>Hinzufügen oder Bearbeiten der Bedingungen für die Azure-Rollenzuweisung mithilfe von Azure PowerShell (Vorschau)

> [!IMPORTANT]
> Azure ABAC und Azure-Rollenzuweisungsbedingungen befinden sich derzeit in der Vorschauphase.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar.
> Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Eine [Azure-Rollenzuweisungsbedingung](conditions-overview.md) ist eine zusätzliche Überprüfung, die Sie Ihrer Rollenzuweisung optional hinzufügen können, um die Zugriffssteuerung präziser zu gestalten. So können Sie beispielsweise eine Bedingung hinzufügen, die festlegt, dass ein Objekt über ein bestimmtes Tag verfügen muss, damit das Objekt gelesen werden kann. In diesem Artikel wird beschrieben, wie Sie Bedingungen für Ihre Rollenzuweisungen mithilfe von Azure PowerShell hinzufügen, bearbeiten, auflisten und löschen.

## <a name="prerequisites"></a>Voraussetzungen

Informationen zu den Voraussetzungen für das Hinzufügen oder Bearbeiten von Rollenzuweisungsbedingungen finden Sie unter [Voraussetzungen für Bedingungen](conditions-prerequisites.md).

## <a name="add-a-condition"></a>Bedingung hinzufügen

Verwenden Sie zum Hinzufügen einer Rollenzuweisungsbedingung den Befehl [New-AzRoleAssignment](/powershell/module/az.resources/new-azroleassignment). Der Befehl [New-AzRoleAssignment](/powershell/module/az.resources/new-azroleassignment) umfasst die folgenden Parameter bezüglich Bedingungen:

| Parameter | type | BESCHREIBUNG |
| --- | --- | --- |
| `Condition` | String | Hierbei handelt es sich um die Bedingung, unter der dem Benutzer die Berechtigung erteilt werden kann. |
| `ConditionVersion` | String | Hierbei handelt es sich um die Version der Bedingungssyntax. Muss auf 2.0 festgelegt werden. Wenn `Condition` angegeben ist, muss auch `ConditionVersion` angegeben werden. |

Im folgenden Beispiel wird gezeigt, wie Sie die Variablen initialisieren, um die Rolle [Storage-Blobdatenleser](built-in-roles.md#storage-blob-data-reader) mit einer Bedingung zuzuweisen. Die Bedingung überprüft, ob der Containername „blobs-example-container“ entspricht.

```azurepowershell
$subscriptionId = "<subscriptionId>"
$resourceGroup = "<resourceGroup>"
$roleDefinitionName = "Storage Blob Data Reader"
$roleDefinitionId = "2a2b9908-6ea1-4ae2-8e65-a410df84e7d1"
$userObjectId = "<userObjectId>"
$scope = "/subscriptions/$subscriptionId/resourceGroups/$resourceGroup"
$description = "Read access if container name equals blobs-example-container"
$condition = "((!(ActionMatches{'Microsoft.Storage/storageAccounts/blobServices/containers/blobs/read'})) OR (@Resource[Microsoft.Storage/storageAccounts/blobServices/containers:name] StringEquals 'blobs-example-container'))"
$conditionVersion = "2.0"
```

Verwenden Sie [New-AzRoleAssignment](/powershell/module/az.resources/new-azroleassignment), um die Rolle mit einer Bedingung zuzuweisen.

```azurepowershell
New-AzRoleAssignment -ObjectId $userObjectId -Scope $scope -RoleDefinitionId $roleDefinitionId -Description $description -Condition $condition -ConditionVersion $conditionVersion
```

Beispiel für die Ausgabe:

```azurepowershell
RoleAssignmentId   : /subscriptions/<subscriptionId>/resourceGroups/<resourceGroup>/providers/Microsoft.Authorization/roleAssignments/<roleAssignmentId>
Scope              : /subscriptions/<subscriptionId>/resourceGroups/<resourceGroup>
DisplayName        : User1
SignInName         : user1@contoso.com
RoleDefinitionName : Storage Blob Data Reader
RoleDefinitionId   : 2a2b9908-6ea1-4ae2-8e65-a410df84e7d1
ObjectId           : <userObjectId>
ObjectType         : User
CanDelegate        : False
Description        : Read access if container name equals blobs-example-container
ConditionVersion   : 2.0
Condition          : ((!(ActionMatches{'Microsoft.Storage/storageAccounts/blobServices/containers/blobs/read'})) OR (@Resource[Microsoft.Storage/storageAccounts/blobServices/containers:name] StringEquals 'blobs-example-container'))
```

Wenn Ihre Bedingung ein Dollarzeichen ($) enthält, müssen Sie ihm in PowerShell einen umgekehrten Schrägstrich (\`) voranstellen. In der folgenden Bedingung werden beispielsweise Dollarzeichen zum Trennen der Tagschlüsselnamen verwendet. Weitere Informationen zu Regeln für Anführungszeichen in PowerShell finden Sie unter [Informationen zu Regeln für Anführungszeichen](/powershell/module/microsoft.powershell.core/about/about_quoting_rules).

```azurepowershell
$condition = "((!(ActionMatches{'Microsoft.Storage/storageAccounts/blobServices/containers/blobs/read'} AND @Request[subOperation] ForAnyOfAnyValues:StringEqualsIgnoreCase {'Blob.Read.WithTagConditions'})) OR (@Resource[Microsoft.Storage/storageAccounts/blobServices/containers/blobs/tags:Project<`$key_case_sensitive`$>] StringEquals 'Cascade'))"
```

## <a name="edit-a-condition"></a>Bearbeiten einer Bedingung

Verwenden Sie [Set-AzRoleAssignment](/powershell/module/az.resources/set-azroleassignment), um eine vorhandene Rollenzuweisungsbedingung zu bearbeiten. Nur die Eigenschaften `Condition`, `ConditionVersion` und `Description` können bearbeitet werden. Der Parameter `-PassThru` bewirkt, dass [Set-AzRoleAssignment](/powershell/module/az.resources/set-azroleassignment) die aktualisierte Rollenzuweisung zurückgibt. Dadurch wird die Visualisierung oder Speicherung in einer Variablen zur weiteren Verwendung ermöglicht.

Es gibt zwei Möglichkeiten zum Bearbeiten einer Bedingung: Sie können das `PSRoleAssignment`-Objekt oder eine JSON-Datei verwenden.

### <a name="edit-a-condition-using-the-psroleassignment-object"></a>Bearbeiten einer Bedingung mithilfe des PSRoleAssignment-Objekts

1. Verwenden Sie [Get-AzRoleAssignment](/powershell/module/az.resources/get-azroleassignment), um die vorhandene Rollenzuweisung mit einer Bedingung als `PSRoleAssignment`-Objekt abzurufen.

    ```azurepowershell
    $testRa = Get-AzRoleAssignment -Scope $scope -RoleDefinitionName $roleDefinitionName -ObjectId $userObjectId
    ```

1. Bearbeiten Sie die Bedingung.

    ```azurepowershell
    $condition = "((!(ActionMatches{'Microsoft.Storage/storageAccounts/blobServices/containers/blobs/read'})) OR (@Resource[Microsoft.Storage/storageAccounts/blobServices/containers:name] StringEquals 'blobs-example-container' OR @Resource[Microsoft.Storage/storageAccounts/blobServices/containers:name] StringEquals 'blobs-example-container2'))"
    ```

1. Initialisieren Sie die Bedingung und Beschreibung.

    ```azurepowershell
    $testRa.Condition = $condition
    $testRa.Description = "Read access if container name equals blobs-example-container or blobs-example-container2"
    ```

1. Aktualisieren Sie mithilfe von [Set-AzRoleAssignment](/powershell/module/az.resources/set-azroleassignment) die Bedingung für die Rollenzuweisung.

    ```azurepowershell
    Set-AzRoleAssignment -InputObject $testRa -PassThru
    ```

    Beispiel für die Ausgabe:

    ```azurepowershell
    RoleAssignmentId   : /subscriptions/<subscriptionId>/resourceGroups/<resourceGroup>/providers/Microsoft.Authorization/roleAssignments/<roleAssignmentId>
    Scope              : /subscriptions/<subscriptionId>/resourceGroups/<resourceGroup>
    DisplayName        : User1
    SignInName         : user1@contoso.com
    RoleDefinitionName : Storage Blob Data Reader
    RoleDefinitionId   : 2a2b9908-6ea1-4ae2-8e65-a410df84e7d1
    ObjectId           : <userObjectId>
    ObjectType         : User
    CanDelegate        : False
    Description        : Read access if container name equals blobs-example-container or blobs-example-container2
    ConditionVersion   : 2.0
    Condition          : ((!(ActionMatches{'Microsoft.Storage/storageAccounts/blobServices/containers/blobs/read'})) OR (@Resource[Microsoft.Storage/storageAccounts/blobServices/containers:name] StringEquals 'blobs-example-container' OR @Resource[Microsoft.Storage/storageAccounts/blobServices/containers:name] StringEquals 'blobs-example-container2'))
    ```

### <a name="edit-a-condition-using-a-json-file"></a>Bearbeiten einer Bedingung mithilfe einer JSON-Datei

Zum Bearbeiten einer Bedingung können Sie auch eine JSON-Datei als Eingabe verwenden. In der folgenden JSON-Beispieldatei wurden `Condition` und `Description` geändert. Sie müssen alle Eigenschaften in der JSON-Datei angeben, um eine Bedingung zu aktualisieren.

```json
{
    "RoleDefinitionId": "2a2b9908-6ea1-4ae2-8e65-a410df84e7d1",
    "ObjectId": "<userObjectId>",
    "ObjectType": "User",
    "Scope": "/subscriptions/<subscriptionId>/resourceGroups/<resourceGroup>",
    "Condition": "((!(ActionMatches{'Microsoft.Storage/storageAccounts/blobServices/containers/blobs/read'})) OR (@Resource[Microsoft.Storage/storageAccounts/blobServices/containers:name] StringEquals 'blobs-example-container' OR @Resource[Microsoft.Storage/storageAccounts/blobServices/containers:name] StringEquals 'blobs-example-container2'))",
    "ConditionVersion": "2.0",
    "CanDelegate": false,
    "Description": "Read access if container name equals blobs-example-container or blobs-example-container2",
    "RoleAssignmentId": "/subscriptions/<subscriptionId>/resourceGroups/<resourceGroup>/providers/Microsoft.Authorization/roleAssignments/<roleAssignmentId>"
}
```

Aktualisieren Sie mithilfe von [Set-AzRoleAssignment](/powershell/module/az.resources/set-azroleassignment) die Bedingung für die Rollenzuweisung.

```azurepowershell
Set-AzRoleAssignment -InputFile "C:\path\roleassignment.json" -PassThru
```

Beispiel für die Ausgabe:

```azurepowershell
RoleAssignmentId   : /subscriptions/<subscriptionId>/resourceGroups/<resourceGroup>/providers/Microsoft.Authorization/roleAssignments/<roleAssignmentId>
Scope              : /subscriptions/<subscriptionId>/resourceGroups/<resourceGroup>
DisplayName        : User1
SignInName         : user1@contoso.com
RoleDefinitionName : Storage Blob Data Reader
RoleDefinitionId   : 2a2b9908-6ea1-4ae2-8e65-a410df84e7d1
ObjectId           : <userObjectId>
ObjectType         : User
CanDelegate        : False
Description        : Read access if container name equals blobs-example-container or blobs-example-container2
ConditionVersion   : 2.0
Condition          : ((!(ActionMatches{'Microsoft.Storage/storageAccounts/blobServices/containers/blobs/read'})) OR (@Resource[Microsoft.Storage/storageAccounts/blobServices/containers:name] StringEquals 'blobs-example-container' OR @Resource[Microsoft.Storage/storageAccounts/blobServices/containers:name] StringEquals 'blobs-example-container2'))
```

## <a name="list-a-condition"></a>Auflisten einer Bedingung

Verwenden Sie zum Auflisten einer Rollenzuweisungsbedingung den Befehl [Get-AzRoleAssignment](/powershell/module/az.resources/get-azroleassignment). Weitere Informationen finden Sie unter [Auflisten von Azure-Rollenzuweisungen mit Azure PowerShell](role-assignments-list-powershell.md).

## <a name="delete-a-condition"></a>Löschen einer Bedingung

Zum Löschen einer Rollenzuweisungsbedingung bearbeiten Sie die Rollenzuweisungsbedingung und legen sowohl die Eigenschaft `Condition` als auch `ConditionVersion` entweder auf eine leere Zeichenfolge (`""`) oder `$null` fest.

Alternativ können Sie den Befehl [Remove-AzRoleAssignment](/powershell/module/az.resources/remove-azroleassignment) verwenden, wenn Sie sowohl die Rollenzuweisung als auch die Bedingung löschen möchten. Weitere Informationen finden Sie unter [Entfernen von Azure-Rollenzuweisungen](role-assignments-remove.md).

## <a name="next-steps"></a>Nächste Schritte

- [Beispiele für Azure-Rollenzuweisungsbedingungen (Vorschau)](../storage/common/storage-auth-abac-examples.md)
- [Tutorial: Hinzufügen einer Rollenzuweisungsbedingung zum Einschränken des Zugriffs auf Blobs mithilfe von Azure PowerShell (Vorschau)](../storage/common/storage-auth-abac-powershell.md)
- [Problembehandlung: Bedingungen für die Azure-Rollenzuweisung (Vorschau)](conditions-troubleshoot.md)
