---
title: Speicherort der Vorlagenressource
description: Beschreibt, wie Sie den Speicherort von Ressourcen in einer Azure Resource Manager-Vorlage festlegen.
ms.topic: conceptual
ms.date: 09/04/2019
ms.openlocfilehash: 24d278df8f71fecfaec4f0fa3a84172bf1db942b
ms.sourcegitcommit: 5bbe87cf121bf99184cc9840c7a07385f0d128ae
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/16/2020
ms.locfileid: "76122405"
---
# <a name="set-resource-location-in-resource-manager-template"></a>Festlegen des Ressourcenspeicherorts in Resource Manager-Vorlagen

Beim Bereitstellen einer Vorlage müssen Sie einen Speicherort für jede Ressource angeben. Der Speicherort muss nicht mit dem Speicherort für die Ressourcengruppe identisch sein.

## <a name="get-available-locations"></a>Abrufen der verfügbaren Speicherorte

An verschiedenen Speicherorten werden unterschiedliche Ressourcentypen unterstützt. Verwenden Sie Azure PowerShell oder die Azure-Befehlszeilenschnittstelle, um die unterstützten Speicherorte für einen Ressourcentyp abzurufen.

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell-interactive
((Get-AzResourceProvider -ProviderNamespace Microsoft.Batch).ResourceTypes `
  | Where-Object ResourceTypeName -eq batchAccounts).Locations
```

# <a name="azure-clitabazure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

```azurecli-interactive
az provider show \
  --namespace Microsoft.Batch \
  --query "resourceTypes[?resourceType=='batchAccounts'].locations | [0]" \
  --out table
```

---

## <a name="use-location-parameter"></a>Verwenden des location-Parameters

Verwenden Sie einen Parameter, um den Speicherort für Ressourcen anzugeben, um beim Bereitstellen der Vorlage mehr Flexibilität zu haben. Der Standardwert dieses Parameters lautet `resourceGroup().location`.

Das folgende Beispiel zeigt ein Speicherkonto, das an einem als Parameter angegebenen Speicherort bereitgestellt wird:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageAccountType": {
      "type": "string",
      "defaultValue": "Standard_LRS",
      "allowedValues": [
        "Standard_LRS",
        "Standard_GRS",
        "Standard_ZRS",
        "Premium_LRS"
      ],
      "metadata": {
        "description": "Storage Account type"
      }
    },
    "location": {
      "type": "string",
      "defaultValue": "[resourceGroup().location]",
      "metadata": {
        "description": "Location for all resources."
      }
    }
  },
  "variables": {
    "storageAccountName": "[concat('storage', uniquestring(resourceGroup().id))]"
  },
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "apiVersion": "2018-07-01",
      "name": "[variables('storageAccountName')]",
      "location": "[parameters('location')]",
      "sku": {
        "name": "[parameters('storageAccountType')]"
      },
      "kind": "StorageV2",
      "properties": {}
    }
  ],
  "outputs": {
    "storageAccountName": {
      "type": "string",
      "value": "[variables('storageAccountName')]"
    }
  }
}
```

## <a name="next-steps"></a>Nächste Schritte

* Unter [Funktionen von Azure-Ressourcen-Manager-Vorlagen](template-functions.md)finden Sie eine vollständige Liste der Vorlagenfunktionen.
* Weitere Informationen zu Vorlagendateien finden Sie unter [Verstehen der Struktur und Syntax von Azure Resource Manager-Vorlagen](template-syntax.md).
