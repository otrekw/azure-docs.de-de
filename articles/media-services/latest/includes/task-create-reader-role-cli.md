---
author: IngridAtMicrosoft
ms.service: media-services
ms.topic: include
ms.date: 08/17/2020
ms.author: inhenkel
ms.custom: CLI, devx-track-azurecli
ms.openlocfilehash: a79851e48e40b3c282d18c06e851f6c92d2391be
ms.sourcegitcommit: 17345cc21e7b14e3e31cbf920f191875bf3c5914
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2021
ms.locfileid: "110103217"
---
<!-- ### Create a Reader role -->

Der folgende Befehl erstellt eine Leser-Rolle. 

Ändern Sie `assignee` in `principalId`. Der Befehl setzt voraus, dass Sie bereits eine Ressourcengruppe und ein Speicherkonto erstellt haben.  Verwenden Sie `your-resource-group-name` und `your-storage-account-name` als Teil des `scope`-Werts, wie im folgenden Befehl gezeigt:

```azurecli-interactive
az role assignment create --assignee 00000000-0000-0000-000000000000 --role "Reader" --scope "/subscriptions/00000000-0000-0000-000000000000/resourceGroups/your-resource-group-name/providers/Microsoft.Storage/storageAccounts/your-storage-account-name"
```

JSON-Beispielantwort:

```json
{
  "canDelegate": null,
  "condition": null,
  "conditionVersion": null,
  "description": null,
  "id": "/subscriptions/00000000-0000-0000-000000000000/resourceGroups/your-resource-group-name/providers/Microsoft.Storage/storageAccounts/your-storage-account-name/providers/Microsoft.Authorization/roleAssignments/00000000-0000-0000-000000000000",
  "name": "00000000-0000-0000-000000000000",
  "principalId": "00000000-0000-0000-000000000000",
  "principalType": "Reader",
  "resourceGroup": "your-resource-group-name",
  "roleDefinitionId": "/subscriptions/00000000-0000-0000-000000000000/providers/Microsoft.Authorization/roleDefinitions/00000000-0000-0000-000000000000",
  "scope": "/subscriptions/00000000-0000-0000-000000000000/resourceGroups/your-resource-group-name/providers/Microsoft.Storage/storageAccounts/your-storage-account-name",
  "type": "Microsoft.Authorization/roleAssignments"
}

```
