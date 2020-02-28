---
title: Definieren mehrerer Instanzen einer Eigenschaft
description: Hier erfahren Sie, wie Sie den Kopiervorgang in einer Azure Resource Manager-Vorlage verwenden, um sie beim Erstellen einer Eigenschaft für eine Ressource mehrmals zu durchlaufen.
ms.topic: conceptual
ms.date: 02/13/2020
ms.openlocfilehash: b759389cd1065c399658bd8d0c1ddd263054697c
ms.sourcegitcommit: 5a71ec1a28da2d6ede03b3128126e0531ce4387d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/26/2020
ms.locfileid: "77622856"
---
# <a name="property-iteration-in-azure-resource-manager-templates"></a>Eigenschafteniteration in Azure Resource Manager-Vorlagen

In diesem Artikel erfahren Sie, wie Sie in Ihrer Azure Resource Manager-Vorlage mehrere Instanzen einer Eigenschaft erstellen. Durch Hinzufügen des Elements **copy** zum Eigenschaftenabschnitt einer Ressource in Ihrer Vorlage kann die Anzahl von Elementen für eine Eigenschaft während der Bereitstellung dynamisch festgelegt werden. Außerdem vermeiden Sie so die Wiederholung von Vorlagensyntax.

„copy“ kann auch mit [Ressourcen](copy-resources.md), [Variablen](copy-variables.md) und [Ausgaben](copy-outputs.md) verwendet werden.

## <a name="property-iteration"></a>Iteration von Eigenschaften

Das copy-Element hat das folgende allgemeine Format:

```json
"copy": [
  {
    "name": "<name-of-loop>",
    "count": <number-of-iterations>,
    "input": <values-for-the-property>
  }
]
```

Geben Sie für **name** den Namen der Ressourceneigenschaft an, die Sie erstellen möchten. Die Eigenschaft **count** gibt die gewünschte Anzahl von Iterationen für die Eigenschaft an.

Die Eigenschaft **input** gibt die Eigenschaften an, die Sie wiederholen möchten. Sie erstellen ein Array von Elementen, das auf der Grundlage des Werts in der Eigenschaft **input** erstellt wird.

Im folgenden Beispiel wird veranschaulicht, wie `copy` auf die dataDisks-Eigenschaft auf einem virtuellen Computer angewendet wird:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "numberOfDataDisks": {
      "type": "int",
      "minValue": 0,
      "maxValue": 16,
      "defaultValue": 16,
      "metadata": {
        "description": "The number of dataDisks to create."
      }
    },
    ...
  },
  "resources": [
    {
      "type": "Microsoft.Compute/virtualMachines",
      "apiVersion": "2017-03-30",
      ...
      "properties": {
        "storageProfile": {
          ...
          "copy": [
            {
              "name": "dataDisks",
              "count": "[parameters('numberOfDataDisks')]",
              "input": {
                "diskSizeGB": 1023,
                "lun": "[copyIndex('dataDisks')]",
                "createOption": "Empty"
              }
            }
          ]
        }
      }
    }
  ]
}
```

Beachten Sie Folgendes: Bei Verwendung von `copyIndex` in einer Eigenschaften-Iteration müssen Sie den Namen der Iteration angeben.

> [!NOTE]
> Von der Eigenschafteniteration wird auch ein Offsetargument unterstützt. Der Offset muss nach dem Namen der Iteration angegeben werden. Beispiel: copyIndex('dataDisks', 1).
>

Ressourcen-Manager erweitert das `copy`-Array während der Bereitstellung. Der Name des Arrays wird zum Namen der Eigenschaft. Die Eingabewerte werden zu den Eigenschaften des Objekts. Die bereitgestellte Vorlage sieht wie folgt aus:

```json
{
  "name": "examplevm",
  "type": "Microsoft.Compute/virtualMachines",
  "apiVersion": "2017-03-30",
  "properties": {
    "storageProfile": {
      "dataDisks": [
        {
          "lun": 0,
          "createOption": "Empty",
          "diskSizeGB": 1023
        },
        {
          "lun": 1,
          "createOption": "Empty",
          "diskSizeGB": 1023
        },
        {
          "lun": 2,
          "createOption": "Empty",
          "diskSizeGB": 1023
        }
      ],
      ...
```

Das Kopierelement ist ein Array, sodass Sie mehrere Eigenschaften für die Ressource angeben können.

```json
{
  "type": "Microsoft.Network/loadBalancers",
  "apiVersion": "2017-10-01",
  "name": "examleLB",
  "properties": {
    "copy": [
      {
        "name": "loadBalancingRules",
        "count": "[length(parameters('loadBalancingRules'))]",
        "input": {
          ...
        }
      },
      {
        "name": "probes",
        "count": "[length(parameters('loadBalancingRules'))]",
        "input": {
          ...
        }
      }
    ]
  }
}
```

Sie können die Ressourcen- und die Eigenschaften-Iteration zusammen verwenden. Verweisen Sie anhand des Namens auf die Eigenschaften-Iteration.

```json
{
  "type": "Microsoft.Network/virtualNetworks",
  "apiVersion": "2018-04-01",
  "name": "[concat(parameters('vnetname'), copyIndex())]",
  "copy":{
    "count": 2,
    "name": "vnetloop"
  },
  "location": "[resourceGroup().location]",
  "properties": {
    "addressSpace": {
      "addressPrefixes": [
        "[parameters('addressPrefix')]"
      ]
    },
    "copy": [
      {
        "name": "subnets",
        "count": 2,
        "input": {
          "name": "[concat('subnet-', copyIndex('subnets'))]",
          "properties": {
            "addressPrefix": "[variables('subnetAddressPrefix')[copyIndex('subnets')]]"
          }
        }
      }
    ]
  }
}
```

## <a name="copy-limits"></a>Einschränkungen für „copy“

Der Wert von „count“ darf 800 nicht überschreiten.

Der Wert von „count“ darf nicht negativ sein. Wenn Sie eine Vorlage mit Azure PowerShell 2.6 oder höher, Azure CLI 2.0.74 oder höher oder mit der REST-API-Version **2019-05-10** oder höher bereitstellen, können Sie „count“ auf „0“ (null) festlegen. Frühere Versionen von PowerShell, CLI und der REST-API unterstützen den Wert „0“ (null) für „count“ nicht.

## <a name="example-templates"></a>Beispielvorlagen

Das folgende Beispiel zeigt ein gängiges Szenario für die Erstellung mehrerer Werte für eine Eigenschaft:

|Vorlage  |Beschreibung  |
|---------|---------|
|[VM-Bereitstellung mit einer variablen Anzahl von Datenträgern](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-windows-copy-datadisks) |Stellt mehrere Datenträger mit einem virtuellen Computer bereit. |

## <a name="next-steps"></a>Nächste Schritte

* Ein Tutorial, das Sie durcharbeiten können, finden Sie unter [Tutorial: Erstellen mehrerer Ressourceninstanzen mit Resource Manager-Vorlagen](template-tutorial-create-multiple-instances.md).
* Informationen zu anderen Verwendungsmöglichkeiten des „copy“-Elements finden Sie unter:
  * [Ressourceniteration in Azure Resource Manager-Vorlagen](copy-resources.md)
  * [Variableniteration in Azure Resource Manager-Vorlagen](copy-variables.md)
  * [Ausgabeniteration in Azure Resource Manager-Vorlagen](copy-outputs.md)
* Informationen zu den Abschnitten einer Vorlage finden Sie unter [Erstellen von Azure Resource Manager-Vorlagen](template-syntax.md).
* Informationen zum Bereitstellen Ihrer Vorlage finden Sie unter [Bereitstellen einer Anwendung mit einer Azure-Ressourcen-Manager-Vorlage](deploy-powershell.md).

