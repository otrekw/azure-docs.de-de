---
title: Deklarieren von Ressourcen in Vorlagen
description: Beschreibt, wie Sie Ressourcen für die Bereitstellung in einer Azure Resource Manager-Vorlage (ARM-Vorlage) deklarieren.
ms.topic: conceptual
ms.date: 05/11/2021
ms.openlocfilehash: 7d7bae8adec81aa3344c5c571f0e40556928a1a9
ms.sourcegitcommit: c072eefdba1fc1f582005cdd549218863d1e149e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/10/2021
ms.locfileid: "111960121"
---
# <a name="resource-declaration-in-arm-templates"></a>Ressourcendeklaration in ARM-Vorlagen

Um eine Ressource durch eine Azure Resource Manager-Vorlage (ARM-Vorlage) bereitzustellen, fügen Sie eine Ressourcendeklaration hinzu. Verwenden Sie das Array `resources` in einer JSON-Vorlage.

## <a name="set-resource-type-and-version"></a>Festlegen von Ressourcentyp und Version

Wenn Sie Ihrer Vorlage eine Ressource hinzufügen, legen Sie zunächst den Ressourcentyp und die API-Version fest. Diese Werte bestimmen die anderen Eigenschaften, die für die Ressource verfügbar sind.

Im folgenden Beispiel wird gezeigt, wie der Ressourcentyp und die API-Version für ein Speicherkonto festgelegt werden. Das Beispiel zeigt nicht die vollständige Ressourcendeklaration.

```json
"resources": [
  {
    "type": "Microsoft.Storage/storageAccounts",
    "apiVersion": "2019-06-01",
    ...
  }
]
```

## <a name="set-resource-name"></a>Festlegen des Ressourcennamens

Jede Ressource besitzt einen Namen. Achten Sie beim Festlegen des Ressourcennamens auf die [Regeln und Einschränkungen für Ressourcennamen](../management/resource-name-rules.md).

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

## <a name="set-location"></a>Legen Sie einen Speicherort fest

Viele Ressourcen erfordern einen Speicherort. Sie können mittels IntelliSense oder über eine [Vorlagenreferenz](/azure/templates/) ermitteln, ob die Ressource einen Speicherort benötigt. Im folgenden Beispiel wird ein Speicherortparameter hinzugefügt, der für das Speicherkonto verwendet wird.

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

Weitere Informationen finden Sie unter [Festlegen des Ressourcenspeicherorts in einer ARM-Vorlage](resource-location.md).

## <a name="set-tags"></a>Festlegen von Tags

Sie können während der Bereitstellung Tags auf eine Ressource anwenden. Tags helfen Ihnen dabei, Ihre bereitgestellten Ressourcen logisch zu organisieren. Beispiele für die verschiedenen Methoden zum Angeben der Tags finden Sie unter [ARM-Vorlagen-Tags](../management/tag-resources.md#arm-templates).

## <a name="set-resource-specific-properties"></a>Festlegen ressourcenspezifischer Eigenschaften

Die vorstehenden Eigenschaften sind für die meisten Ressourcentypen generisch. Nachdem Sie diese Werte festgelegt haben, müssen Sie die Eigenschaften festlegen, die für den Ressourcentyp, den Sie bereitstellen, spezifisch sind.

Verwenden Sie IntelliSense oder eine [Vorlagenreferenz](/azure/templates/), um zu bestimmen, welche Eigenschaften verfügbar sind und welche erforderlich sind. Im folgenden Beispiel werden die restlichen Eigenschaften für ein Speicherkonto festgelegt.

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

## <a name="next-steps"></a>Nächste Schritte

* Informationen zur bedingten Bereitstellung einer Ressource finden Sie unter [Bedingte Bereitstellung in ARM-Vorlagen](conditional-resource-deployment.md).
* Informationen zum Festlegen von Abhängigkeiten finden Sie unter [Definieren der Reihenfolge für die Bereitstellung von Ressourcen in ARM-Vorlagen](./resource-dependency.md).