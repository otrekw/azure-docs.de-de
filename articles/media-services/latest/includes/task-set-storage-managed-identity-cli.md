---
author: IngridAtMicrosoft
ms.service: media-services
ms.topic: include
ms.date: 08/17/2020
ms.author: inhenkel
ms.custom: CLI, devx-track-azurecli
ms.openlocfilehash: 8b4fd9356810b6ef44ea3401ac87c12c4f58b3d8
ms.sourcegitcommit: 17345cc21e7b14e3e31cbf920f191875bf3c5914
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2021
ms.locfileid: "110103212"
---
<!-- ### Give a Media Services Managed identity access to a storage account. -->

Der folgende Befehl gewährt einer verwalteten Media Services-Identität Zugriff auf ein Speicherkonto.

Ändern Sie `your-resource-group-name` im folgenden Befehl in den Namen der Ressourcengruppe und `your-media-services-account-name` in den Namen des Media Services-Kontos, mit dem Sie arbeiten möchten:

```azurecli-interactive
az ams account storage set-authentication --storage-auth ManagedIdentity --resource-group <your-resource-group_name> --account-name <your-media-services-account-name>

```

JSON-Beispielantwort:

```json
{
  "encryption": {
    "keyVaultProperties": null,
    "type": "SystemKey"
  },
  "id": "/subscriptions/00000000-0000-0000-00000000/resourceGroups/your-resource-group-name/providers/Microsoft.Media/mediaservices/your-storage-account-name",
  "identity": null,
  "location": "West US 2",
  "mediaServiceId": "00000000-0000-0000-00000000",
  "name": "your-media-services-account",
  "resourceGroup": "your-resource-group-name",
  "storageAccounts": [
    {
      "id": "/subscriptions/2b461b25-f7b4-4a22-90cc-d640a14b5471/resourceGroups/your-resource-group-name/providers/Microsoft.Storage/storageAccounts/your-storage-account-name",
      "resourceGroup": "your-resource-group-name",
      "type": "Primary"
    }
  ],
  "storageAuthentication": "ManagedIdentity",
  "systemData": {
    "createdAt": "2021-05-17T19:15:00.8850297Z",
    "createdBy": "you@example.com",
    "createdByType": "User",
    "lastModifiedAt": "2021-05-17T21:23:11.3863627Z",
    "lastModifiedBy": "you@example.com",
    "lastModifiedByType": "User"
  },
  "tags": null,
  "type": "Microsoft.Media/mediaservices"
}

```
