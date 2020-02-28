---
title: Definieren mehrerer Instanzen einer Variablen
description: Verwenden des copy-Vorgangs in einer Azure Resource Manager-Vorlage, um sie beim Erstellen einer Variablen mehrere Male zu durchlaufen.
ms.topic: conceptual
ms.date: 02/13/2020
ms.openlocfilehash: ea4caf3553b3cd14eec194b8cef0db59499a4f4c
ms.sourcegitcommit: 5a71ec1a28da2d6ede03b3128126e0531ce4387d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/26/2020
ms.locfileid: "77622886"
---
# <a name="variable-iteration-in-azure-resource-manager-templates"></a>Variableniteration in Azure Resource Manager-Vorlagen

In diesem Artikel wird gezeigt, wie Sie mehr als einen Wert für eine Variable in Ihrer Azure Resource Manager-Vorlage erstellen können. Durch das Hinzufügen des **copy**-Elements zum Variablenabschnitt Ihrer Vorlage können Sie die Anzahl der Elemente für eine Variable während der Bereitstellung dynamisch festlegen. Außerdem vermeiden Sie so die Wiederholung von Vorlagensyntax.

Sie können „copy“ auch mit [Ressourcen](copy-resources.md), [Eigenschaften in einer Ressource](copy-properties.md) und [Ausgaben](copy-outputs.md) verwenden.

## <a name="variable-iteration"></a>Variableniteration

Das „copy“-Element weist das folgende allgemeine Format auf:

```json
"copy": [
  {
    "name": "<name-of-loop>",
    "count": <number-of-iterations>,
    "input": <values-for-the-variable>
  }
]
```

Die Eigenschaft **name** ist ein beliebiger Wert, der die Schleife identifiziert. Die Eigenschaft **count** gibt die für die Variable gewünschte Anzahl von Iterationen an.

Die Eigenschaft **input** gibt die Eigenschaften an, die Sie wiederholen möchten. Sie erstellen ein Array von Elementen, das aus dem Wert in der **input**-Eigenschaft erstellt wird. Es kann sich um eine einzelne Eigenschaft handeln (z. B. eine Zeichenfolge) oder um ein Objekt mit mehreren Eigenschaften.

Im folgenden Beispiel wird veranschaulicht, wie Sie ein Array mit Zeichenfolgenwerten erstellen:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "itemCount": {
            "type": "int",
            "defaultValue": 5
        }
     },
    "variables": {
        "copy": [
            {
                "name": "stringArray",
                "count": "[parameters('itemCount')]",
                "input": "[concat('item', copyIndex('stringArray', 1))]"
            }
        ]
    },
    "resources": [],
    "outputs": {
        "arrayResult": {
            "type": "array",
            "value": "[variables('stringArray')]"
        }
    }
}
```

Die vorangehende Vorlage gibt ein Array mit den folgenden Werten zurück:

```json
[
    "item1",
    "item2",
    "item3",
    "item4",
    "item5"
]
```

Das nächste Beispiel zeigt, wie Sie ein Array von Objekten mit drei Eigenschaften erstellen, „name“, „diskSizeGB“ und „diskIndex“.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "itemCount": {
            "type": "int",
            "defaultValue": 5
        }
    },
    "variables": {
        "copy": [
            {
                "name": "objectArray",
                "count": "[parameters('itemCount')]",
                "input": {
                    "name": "[concat('myDataDisk', copyIndex('objectArray', 1))]",
                    "diskSizeGB": "1",
                    "diskIndex": "[copyIndex('objectArray')]"
                }
            }
        ]
    },
    "resources": [],
    "outputs": {
        "arrayResult": {
            "type": "array",
            "value": "[variables('objectArray')]"
        }
    }
}
```

Das vorangehende Beispiel gibt ein Array mit den folgenden Werten zurück:

```json
[
    {
        "name": "myDataDisk1",
        "diskSizeGB": "1",
        "diskIndex": 0
    },
    {
        "name": "myDataDisk2",
        "diskSizeGB": "1",
        "diskIndex": 1
    },
    {
        "name": "myDataDisk3",
        "diskSizeGB": "1",
        "diskIndex": 2
    },
    {
        "name": "myDataDisk4",
        "diskSizeGB": "1",
        "diskIndex": 3
    },
    {
        "name": "myDataDisk5",
        "diskSizeGB": "1",
        "diskIndex": 4
    }
]
```

> [!NOTE]
> Die Variableniteration unterstützt ein Offsetargument. Der Offset muss nach dem Namen der Iteration angegeben werden. Beispiel: copyIndex('diskNames', 1). Wenn Sie keinen Offsetwert angeben, nimmt die erste Instanz den Standardwert 0 an.
>

Sie können das „copy“-Element auch innerhalb einer Variablen verwenden. Mit dem folgenden Beispiel wird ein Objekt erstellt, bei dem einer seiner Werte ein Array ist.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "itemCount": {
            "type": "int",
            "defaultValue": 5
        }
    },
    "variables": {
        "topLevelObject": {
            "sampleProperty": "sampleValue",
            "copy": [
                {
                    "name": "disks",
                    "count": "[parameters('itemCount')]",
                    "input": {
                        "name": "[concat('myDataDisk', copyIndex('disks', 1))]",
                        "diskSizeGB": "1",
                        "diskIndex": "[copyIndex('disks')]"
                    }
                }
            ]
        }
    },
    "resources": [],
    "outputs": {
        "objectResult": {
            "type": "object",
            "value": "[variables('topLevelObject')]"
        }
    }
}
```

Das vorangehende Beispiel gibt ein Objekt mit den folgenden Werten zurück:

```json
{
    "sampleProperty": "sampleValue",
    "disks": [
        {
            "name": "myDataDisk1",
            "diskSizeGB": "1",
            "diskIndex": 0
        },
        {
            "name": "myDataDisk2",
            "diskSizeGB": "1",
            "diskIndex": 1
        },
        {
            "name": "myDataDisk3",
            "diskSizeGB": "1",
            "diskIndex": 2
        },
        {
            "name": "myDataDisk4",
            "diskSizeGB": "1",
            "diskIndex": 3
        },
        {
            "name": "myDataDisk5",
            "diskSizeGB": "1",
            "diskIndex": 4
        }
    ]
}
```

Im nächsten Beispiel werden die verschiedenen Möglichkeiten gezeigt, mit denen Sie „copy“ zusammen mit Variablen verwenden können.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {},
  "variables": {
    "disk-array-on-object": {
      "copy": [
        {
          "name": "disks",
          "count": 5,
          "input": {
            "name": "[concat('myDataDisk', copyIndex('disks', 1))]",
            "diskSizeGB": "1",
            "diskIndex": "[copyIndex('disks')]"
          }
        },
        {
          "name": "diskNames",
          "count": 5,
          "input": "[concat('myDataDisk', copyIndex('diskNames', 1))]"
        }
      ]
    },
    "copy": [
      {
        "name": "top-level-object-array",
        "count": 5,
        "input": {
          "name": "[concat('myDataDisk', copyIndex('top-level-object-array', 1))]",
          "diskSizeGB": "1",
          "diskIndex": "[copyIndex('top-level-object-array')]"
        }
      },
      {
        "name": "top-level-string-array",
        "count": 5,
        "input": "[concat('myDataDisk', copyIndex('top-level-string-array', 1))]"
      },
      {
        "name": "top-level-integer-array",
        "count": 5,
        "input": "[copyIndex('top-level-integer-array')]"
      }
    ]
  },
  "resources": [],
  "outputs": {
    "exampleObject": {
      "value": "[variables('disk-array-on-object')]",
      "type": "object"
    },
    "exampleArrayOnObject": {
      "value": "[variables('disk-array-on-object').disks]",
      "type" : "array"
    },
    "exampleObjectArray": {
      "value": "[variables('top-level-object-array')]",
      "type" : "array"
    },
    "exampleStringArray": {
      "value": "[variables('top-level-string-array')]",
      "type" : "array"
    },
    "exampleIntegerArray": {
      "value": "[variables('top-level-integer-array')]",
      "type" : "array"
    }
  }
}
```

## <a name="copy-limits"></a>Einschränkungen für „copy“

Der Wert von „count“ darf 800 nicht überschreiten.

Der Wert von „count“ darf nicht negativ sein. Wenn Sie eine Vorlage mit Azure PowerShell 2.6 oder höher, Azure CLI 2.0.74 oder höher oder mit der REST-API-Version **2019-05-10** oder höher bereitstellen, können Sie „count“ auf „0“ (null) festlegen. Frühere Versionen von PowerShell, CLI und der REST-API unterstützen den Wert „0“ (null) für „count“ nicht.

## <a name="example-templates"></a>Beispielvorlagen

Die folgenden Beispiele zeigen allgemeine Szenarien für das Erstellen mehrerer Werte für eine Variable.

|Vorlage  |BESCHREIBUNG  |
|---------|---------|
|[Variablen kopieren](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/copyvariables.json) |Veranschaulicht die verschiedenen Methoden zum Durchlaufen von Variablen |
|[Mehrere Sicherheitsregeln](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/multiplesecurityrules.json) |Stellt mehrere Sicherheitsregeln in einer Netzwerksicherheitsgruppe bereit. Die Sicherheitsregeln werden aus einem Parameter generiert. Informationen zum Parameter finden Sie im Artikel zur [Datei mit mehreren NSG-Parametern](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/multiplesecurityrules.parameters.json). |

## <a name="next-steps"></a>Nächste Schritte

* Ein Tutorial, das Sie durcharbeiten können, finden Sie unter [Tutorial: Erstellen mehrerer Ressourceninstanzen mit Resource Manager-Vorlagen](template-tutorial-create-multiple-instances.md).
* Informationen zu anderen Verwendungsmöglichkeiten des „copy“-Elements finden Sie unter:
  * [Ressourceniteration in Azure Resource Manager-Vorlagen](copy-resources.md)
  * [Eigenschafteniteration in Azure Resource Manager-Vorlagen](copy-properties.md)
  * [Ausgabeniteration in Azure Resource Manager-Vorlagen](copy-outputs.md)
* Informationen zu den Abschnitten einer Vorlage finden Sie unter [Erstellen von Azure Resource Manager-Vorlagen](template-syntax.md).
* Informationen zum Bereitstellen Ihrer Vorlage finden Sie unter [Bereitstellen einer Anwendung mit einer Azure-Ressourcen-Manager-Vorlage](deploy-powershell.md).

