---
author: IngridAtMicrosoft
ms.service: media-services
ms.topic: include
ms.date: 08/17/2020
ms.author: inhenkel
ms.custom: CLI
ms.openlocfilehash: f1b77e4cee809d7ddb7bbbf7de6c5ac483b150e3
ms.sourcegitcommit: 17345cc21e7b14e3e31cbf920f191875bf3c5914
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2021
ms.locfileid: "110060124"
---
<!-- Create a resource group -->

Verwenden Sie den folgenden Befehl, um eine Ressourcengruppe zu erstellen. Ändern Sie `your-resource-group-name` in den gewünschten Namen für Ihre Ressourcengruppe.

Ändern Sie `your-region` in die geografische Region, in der die Medien- und Metadaten-Datensätze für Ihr Media Services-Konto gespeichert werden sollen. Dieser Bereich wird zum Verarbeiten und Streamen Ihrer Medien verwendet.

```azurecli-interactive
az group create --name <your-resource-group-name> --location <your-region>
```

JSON-Beispielantwort:

```json
{
  "id": "/subscriptions/00000000-0000-0000-000000000000/resourceGroups/your-resource-group-name",
  "location": "your-region",
  "managedBy": null,
  "name": "your-resource-group-name",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "tags": null,
  "type": "Microsoft.Resources/resourceGroups"
}
```
