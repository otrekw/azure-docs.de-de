---
title: Definieren mehrerer Instanzen eines Ausgabewerts
description: Verwenden des „copy“-Vorgangs in einer Azure Resource Manager-Vorlage, um das Zurückgeben eines Werts aus einer Bereitstellung mehrere Male zu durchlaufen.
ms.topic: conceptual
ms.date: 04/17/2020
ms.openlocfilehash: 0315af2f083285c4704b08fec608341b6f0b2231
ms.sourcegitcommit: eefb0f30426a138366a9d405dacdb61330df65e7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/17/2020
ms.locfileid: "81617827"
---
# <a name="output-iteration-in-arm-templates"></a>Ausgabeiteration in ARM-Vorlagen

In diesem Artikel wird gezeigt, wie Sie mehr als einen Wert für eine Ausgabe in Ihrer Azure Resource Manager-Vorlage (ARM) erstellen können. Durch das Hinzufügen des **copy**-Elements zum Variablenabschnitt Ihrer Vorlage können Sie eine Anzahl von Elementen während der Bereitstellung dynamisch zurückgeben.

Sie können „copy“ auch mit [Ressourcen](copy-resources.md), [Eigenschaften in einer Ressource](copy-properties.md) und [Variablen](copy-variables.md) verwenden.

## <a name="outputs-iteration"></a>Ausgabeniteration

Das copy-Element hat das folgende allgemeine Format:

```json
"copy": {
  "count": <number-of-iterations>,
  "input": <values-for-the-output>
}
```

Die Eigenschaft **count** gibt die gewünschte Anzahl von Iterationen für den Ausgabewert an.

Die Eigenschaft **input** gibt die Eigenschaften an, die Sie wiederholen möchten. Sie erstellen ein Array von Elementen, das aus dem Wert in der **input**-Eigenschaft erstellt wird. Es kann sich um eine einzelne Eigenschaft handeln (z. B. eine Zeichenfolge) oder um ein Objekt mit mehreren Eigenschaften.

Im folgenden Beispiel wird eine Variable Anzahl von Speicherkonten erstellt und für jedes Speicherkonto ein Endpunkt zurückgegeben:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "storageCount": {
            "type": "int",
            "defaultValue": 2
        }
    },
    "variables": {
        "baseName": "[concat('storage', uniqueString(resourceGroup().id))]"
    },
    "resources": [
        {
            "type": "Microsoft.Storage/storageAccounts",
            "apiVersion": "2019-04-01",
            "name": "[concat(copyIndex(), variables('baseName'))]",
            "location": "[resourceGroup().location]",
            "sku": {
                "name": "Standard_LRS"
            },
            "kind": "Storage",
            "properties": {},
            "copy": {
                "name": "storagecopy",
                "count": "[parameters('storageCount')]"
            }
        }
    ],
    "outputs": {
        "storageEndpoints": {
            "type": "array",
            "copy": {
                "count": "[parameters('storageCount')]",
                "input": "[reference(concat(copyIndex(), variables('baseName'))).primaryEndpoints.blob]"
            }
        }
    }
}
```

Die vorangehende Vorlage gibt ein Array mit den folgenden Werten zurück:

```json
[
    "https://0storagecfrbqnnmpeudi.blob.core.windows.net/",
    "https://1storagecfrbqnnmpeudi.blob.core.windows.net/"
]
```

Im nächsten Beispiel werden drei Eigenschaften aus den neuen Speicherkonten zurückgegeben.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "storageCount": {
            "type": "int",
            "defaultValue": 2
        }
    },
    "variables": {
        "baseName": "[concat('storage', uniqueString(resourceGroup().id))]"
    },
    "resources": [
        {
            "type": "Microsoft.Storage/storageAccounts",
            "apiVersion": "2019-04-01",
            "name": "[concat(copyIndex(), variables('baseName'))]",
            "location": "[resourceGroup().location]",
            "sku": {
                "name": "Standard_LRS"
            },
            "kind": "Storage",
            "properties": {},
            "copy": {
                "name": "storagecopy",
                "count": "[parameters('storageCount')]"
            }
        }
    ],
    "outputs": {
        "storageInfo": {
            "type": "array",
            "copy": {
                "count": "[parameters('storageCount')]",
                "input": {
                    "id": "[reference(concat(copyIndex(), variables('baseName')), '2019-04-01', 'Full').resourceId]",
                    "blobEndpoint": "[reference(concat(copyIndex(), variables('baseName'))).primaryEndpoints.blob]",
                    "status": "[reference(concat(copyIndex(), variables('baseName'))).statusOfPrimary]"
                }
            }
        }
    }
}
```

Das vorangehende Beispiel gibt ein Array mit den folgenden Werten zurück:

```json
[
    {
        "id": "Microsoft.Storage/storageAccounts/0storagecfrbqnnmpeudi",
        "blobEndpoint": "https://0storagecfrbqnnmpeudi.blob.core.windows.net/",
        "status": "available"
    },
    {
        "id": "Microsoft.Storage/storageAccounts/1storagecfrbqnnmpeudi",
        "blobEndpoint": "https://1storagecfrbqnnmpeudi.blob.core.windows.net/",
        "status": "available"
    }
]
```

## <a name="next-steps"></a>Nächste Schritte

* Ein Tutorial, das Sie durcharbeiten können, finden Sie unter [Tutorial: Erstellen mehrerer Ressourceninstanzen mit Resource Manager-Vorlagen (ARM)](template-tutorial-create-multiple-instances.md).
* Informationen zu anderen Verwendungsmöglichkeiten des „copy“-Elements finden Sie unter:
  * [Ressourceniteration in ARM-Vorlagen](copy-resources.md)
  * [Eigenschafteniteration in ARM-Vorlagen](copy-properties.md)
  * [Variableniteration in ARM-Vorlagen](copy-variables.md)
* Informationen zu den Abschnitten einer Vorlage finden Sie unter [Erstellen von ARM-Vorlagen](template-syntax.md).
* Informationen zum Bereitstellen Ihrer Vorlage finden Sie unter [Bereitstellen einer Anwendung mit einer ARM-Vorlage](deploy-powershell.md).

