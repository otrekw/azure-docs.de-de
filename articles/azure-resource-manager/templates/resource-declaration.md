---
title: Deklarieren von Ressourcen in Vorlagen
description: Beschreibt, wie Sie Ressourcen für die Bereitstellung in einer Azure Resource Manager-Vorlage (ARM-Vorlage) deklarieren.
ms.topic: conceptual
ms.date: 03/02/2021
ms.openlocfilehash: 13f4a08162c40cbb36173627d88a4a8202a4ed26
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "101743428"
---
# <a name="resource-declaration-in-arm-templates"></a>Ressourcendeklaration in ARM-Vorlagen

Um eine Ressource durch eine Azure Resource Manager-Vorlage (ARM-Vorlage) bereitzustellen, fügen Sie eine Ressourcendeklaration hinzu. Verwenden Sie das `resources`-Array für eine JSON-Vorlage oder das `resource`-Schlüsselwort für Bicep.

[!INCLUDE [Bicep preview](../../../includes/resource-manager-bicep-preview.md)]

## <a name="set-resource-type-and-version"></a>Festlegen von Ressourcentyp und Version

Wenn Sie Ihrer Vorlage eine Ressource hinzufügen, legen Sie zunächst den Ressourcentyp und die API-Version fest. Diese Werte bestimmen die anderen Eigenschaften, die für die Ressource verfügbar sind.

Im folgenden Beispiel wird gezeigt, wie der Ressourcentyp und die API-Version für ein Speicherkonto festgelegt werden. Das Beispiel zeigt nicht die vollständige Ressourcendeklaration.

# <a name="json"></a>[JSON](#tab/json)

```json
"resources": [
  {
    "type": "Microsoft.Storage/storageAccounts",
    "apiVersion": "2019-06-01",
    ...
  }
]
```

# <a name="bicep"></a>[Bicep](#tab/bicep)

```bicep
resource sa 'Microsoft.Storage/storageAccounts@2019-06-01' = {
  ...
}
```

---

Für Bicep legen Sie einen symbolischen Namen für die Ressource fest. Im vorherigen Beispiel lautet der symbolische Name `sa`. Sie können einen beliebigen Wert für den symbolischen Namen verwenden, aber er kann nicht mit dem Namen einer anderen Ressource, eines Parameters oder einer Variablen in der Vorlage identisch sein. Der symbolische Name ist nicht mit dem Ressourcennamen identisch. Sie verwenden den symbolischen Namen, um in anderen Teilen ihrer Vorlage auf einfache Weise auf die Ressource zu verweisen.

## <a name="set-resource-name"></a>Festlegen des Ressourcennamens

Jede Ressource besitzt einen Namen. Achten Sie beim Festlegen des Ressourcennamens auf die [Regeln und Einschränkungen für Ressourcennamen](../management/resource-name-rules.md).

# <a name="json"></a>[JSON](#tab/json)

```json
"parameters": {
  "storageAccountName": {
    "type": "string",
    "minLength": 3,
    "maxLength": 24
  }
},
"resources": [
  {
    "type": "Microsoft.Storage/storageAccounts",
    "apiVersion": "2019-06-01",
    "name": "[parameters('storageAccountName')]",
    ...
  }
]
```

# <a name="bicep"></a>[Bicep](#tab/bicep)

```bicep
@minLength(3)
@maxLength(24)
param storageAccountName string

resource sa 'Microsoft.Storage/storageAccounts@2019-06-01' = {
  name: storageAccountName
  ...
}
```

---

## <a name="set-location"></a>Legen Sie einen Speicherort fest

Viele Ressourcen erfordern einen Speicherort. Sie können mittels IntelliSense oder über eine [Vorlagenreferenz](/azure/templates/) ermitteln, ob die Ressource einen Speicherort benötigt. Im folgenden Beispiel wird ein Speicherortparameter hinzugefügt, der für das Speicherkonto verwendet wird.

# <a name="json"></a>[JSON](#tab/json)

```json
"parameters": {
  "storageAccountName": {
    "type": "string",
    "minLength": 3,
    "maxLength": 24
  },
  "location": {
    "type": "string",
    "defaultValue": "[resourceGroup().location]"
  }
},
"resources": [
  {
    "type": "Microsoft.Storage/storageAccounts",
    "apiVersion": "2019-06-01",
    "name": "[parameters('storageAccountName')]",
    "location": "[parameters('location')]",
    ...
  }
]
```

# <a name="bicep"></a>[Bicep](#tab/bicep)

```bicep
@minLength(3)
@maxLength(24)
param storageAccountName string
param location string = resourceGroup().location

resource sa 'Microsoft.Storage/storageAccounts@2019-06-01' = {
  name: storageAccountName
  location: location
  ...
}
```

---

Weitere Informationen finden Sie unter [Festlegen des Ressourcenspeicherorts in einer ARM-Vorlage](resource-location.md).

## <a name="set-tags"></a>Festlegen von Tags

Sie können während der Bereitstellung Tags auf eine Ressource anwenden. Tags helfen Ihnen dabei, Ihre bereitgestellten Ressourcen logisch zu organisieren. Beispiele für die verschiedenen Methoden zum Angeben der Tags finden Sie unter [ARM-Vorlagen-Tags](../management/tag-resources.md#arm-templates).

## <a name="set-resource-specific-properties"></a>Festlegen ressourcenspezifischer Eigenschaften

Die vorstehenden Eigenschaften sind für die meisten Ressourcentypen generisch. Nachdem Sie diese Werte festgelegt haben, müssen Sie die Eigenschaften festlegen, die für den Ressourcentyp, den Sie bereitstellen, spezifisch sind.

Verwenden Sie IntelliSense oder eine [Vorlagenreferenz](/azure/templates/), um zu bestimmen, welche Eigenschaften verfügbar sind und welche erforderlich sind. Im folgenden Beispiel werden die restlichen Eigenschaften für ein Speicherkonto festgelegt.

# <a name="json"></a>[JSON](#tab/json)

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageAccountName": {
      "type": "string",
      "minLength": 3,
      "maxLength": 24
    },
    "location": {
      "type": "string",
      "defaultValue": "[resourceGroup().location]"
    }
  },
  "functions": [],
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "apiVersion": "2019-06-01",
      "name": "[parameters('storageAccountName')]",
      "location": "[parameters('location')]",
      "sku": {
        "name": "Standard_LRS",
        "tier": "Standard"
      },
      "kind": "StorageV2",
      "properties": {
        "accessTier": "Hot"
      }
    }
  ]
}
```

# <a name="bicep"></a>[Bicep](#tab/bicep)

```bicep
@minLength(3)
@maxLength(24)
param storageAccountName string
param location string = resourceGroup().location

resource sa 'Microsoft.Storage/storageAccounts@2019-06-01' = {
  name: storageAccountName
  location: location
  sku: {
    name: 'Standard_LRS'
    tier: 'Standard'
  }
  kind: 'StorageV2'
  properties: {
    accessTier: 'Hot'
  }
}
```

---

## <a name="next-steps"></a>Nächste Schritte

* Informationen zur bedingten Bereitstellung einer Ressource finden Sie unter [Bedingte Bereitstellung in ARM-Vorlagen](conditional-resource-deployment.md).
* Informationen zum Festlegen von Abhängigkeiten finden Sie unter [Definieren der Reihenfolge für die Bereitstellung von Ressourcen in ARM-Vorlagen](define-resource-dependency.md).
