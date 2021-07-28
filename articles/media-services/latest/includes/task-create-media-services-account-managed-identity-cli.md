---
author: IngridAtMicrosoft
ms.service: media-services
ms.topic: include
ms.date: 08/17/2020
ms.author: inhenkel
ms.custom: CLI, devx-track-azurecli
ms.openlocfilehash: 72dc27bf1600d298d41a034b081b216d3d77c94c
ms.sourcegitcommit: 17345cc21e7b14e3e31cbf920f191875bf3c5914
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2021
ms.locfileid: "110103180"
---
<!--Create a media services account -->

Der folgende Azure CLI-Befehl erstellt ein neues Media Services-Konto. Ersetzen Sie die Werte `your-media-services-account-name`, `your-storage-account-name` und `your-resource-group-name` durch die Namen, die Sie verwenden möchten. Der Befehl setzt voraus, dass Sie bereits eine Ressourcengruppe und ein Speicherkonto erstellt haben. 

Durch ihn erhält das Media Services-Konto eine systemseitig zugewiesene verwaltete Identität mit dem Flag `--mi-system-assigned`.

```azurecli-interactive

az ams account create --name <your-media-services-account-name> --resource-group <your-resource-group-name> --mi-system-assigned --storage-account <your-storage-account-name>

```

JSON-Beispielantwort:

```json
{
  "encryption": {
    "keyVaultProperties": null,
    "type": "SystemKey"
  },
  "id": "/subscriptions/00000000-0000-0000-0000-00000000/resourceGroups/your-resource-group/providers/Microsoft.Media/mediaservices/your-media-services-account-name",
  "identity": {
    "principalId": "00000000-0000-0000-0000-00000000",
    "tenantId": "00000000-0000-0000-0000-00000000",
    "type": "SystemAssigned"
  },
  "location": "your-region",
  "mediaServiceId": "00000000-0000-0000-0000-00000000",
  "name": "your-media-services-account-name",
  "resourceGroup": "your-resource-group",
  "storageAccounts": [
    {
      "id": "/subscriptions/00000000-0000-0000-0000-00000000/resourceGroups/mediatest1rg/providers/Microsoft.Storage/storageAccounts/your-storage-account-name",
      "resourceGroup": "your-resource-group",
      "type": "Primary"
    }
  ],
  "storageAuthentication": "System",
  "systemData": {
    "createdAt": "2021-05-14T21:25:12.3492071Z",
    "createdBy": "you@example.com",
    "createdByType": "User",
    "lastModifiedAt": "2021-05-14T21:25:12.3492071Z",
    "lastModifiedBy": "you@example.com",
    "lastModifiedByType": "User"
  },
  "tags": null,
  "type": "Microsoft.Media/mediaservices"
}
```
