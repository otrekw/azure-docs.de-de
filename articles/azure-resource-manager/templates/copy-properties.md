---
title: Definieren mehrerer Instanzen einer Eigenschaft
description: Erfahren Sie, wie Sie den copy-Vorgang in einer Azure Resource Manager-Vorlage (ARM) verwenden, um sie beim Erstellen einer Eigenschaft für eine Ressource mehrmals zu durchlaufen.
ms.topic: conceptual
ms.date: 04/01/2021
ms.openlocfilehash: 94bc153a49f80694ab9b2d5b04fdf57e8a12e8c8
ms.sourcegitcommit: 77d7639e83c6d8eb6c2ce805b6130ff9c73e5d29
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/05/2021
ms.locfileid: "106385750"
---
# <a name="property-iteration-in-arm-templates"></a>Eigenschafteniteration in ARM-Vorlagen

In diesem Artikel erfahren Sie, wie Sie in Ihrer Azure Resource Manager-Vorlage (ARM-Vorlage) mehrere Instanzen einer Eigenschaft erstellen. Durch Hinzufügen der copy-Schleife zum Eigenschaftenabschnitt einer Ressource in Ihrer Vorlage kann die Anzahl von Elementen für eine Eigenschaft während der Bereitstellung dynamisch festgelegt werden. Außerdem vermeiden Sie so die Wiederholung von Vorlagensyntax.

Sie können die copy-Schleife nur mit Ressourcen der obersten Ebene verwenden, auch wenn Sie eine copy-Schleife auf eine Eigenschaft anwenden. Weitere Informationen zum Ändern einer untergeordneten Ressource in eine Ressource der obersten Ebene finden Sie unter [Iterationen für eine untergeordnete Ressource](copy-resources.md#iteration-for-a-child-resource).

Die copy-Schleife kann auch mit [Ressourcen](copy-resources.md), [Variablen](copy-variables.md) und [Ausgaben](copy-outputs.md) verwendet werden.

## <a name="syntax"></a>Syntax

# <a name="json"></a>[JSON](#tab/json)

Fügen Sie das `copy`-Element im Ressourcenabschnitt Ihrer Vorlage ein, um die Anzahl der Elemente für eine Eigenschaft festzulegen. Das copy-Element hat das folgende allgemeine Format:

```json
"copy": [
  {
    "name": "<name-of-property>",
    "count": <number-of-iterations>,
    "input": <values-for-the-property>
  }
]
```

Geben Sie für `name` den Namen der Ressourceneigenschaft an, die Sie erstellen möchten.

Die Eigenschaft `count` gibt die gewünschte Anzahl von Iterationen für die Eigenschaft an.

Die Eigenschaft `input` gibt die Eigenschaften an, die Sie wiederholen möchten. Erstellen Sie ein Array von Elementen, das aus dem Wert in der `input`-Eigenschaft erstellt wird.

# <a name="bicep"></a>[Bicep](#tab/bicep)

Mit Schleifen können mehrere Eigenschaften deklariert werden durch:

- Durchlaufen eines Arrays:

  ```bicep
  <property-name>: [for <item> in <collection>: {
    <properties>
  }
  ```

- Durchlaufen der Elemente eines Arrays

  ```bicep
  <property-name>: [for (<item>, <index>) in <collection>: {
    <properties>
  }
  ```

- Verwenden des Schleifenindexes

  ```bicep
  <property-name>: [for <index> in range(<start>, <stop>): {
    <properties>
  }
  ```

---

## <a name="copy-limits"></a>Einschränkungen für „copy“

Der Wert von „count“ darf 800 nicht überschreiten.

Der Wert von „count“ darf nicht negativ sein. Er kann Null sein, wenn Sie die Vorlage mit einer neueren Version von Azure CLI, PowerShell oder der REST-API bereitstellen. Insbesondere müssen Sie Folgendes verwenden:

- Azure PowerShell **2.6** oder höher
- Azure CLI **2.0.74** oder höher
- REST-API-Version **2019-05-10** oder höher
- [Verknüpfte Bereitstellungen](linked-templates.md) müssen API-Version **2019-05-10** oder höher für den Bereitstellungsressourcentyp verwenden.

Frühere Versionen von PowerShell, CLI und der REST-API unterstützen den Wert „0“ (null) für „count“ nicht.

## <a name="property-iteration"></a>Iteration von Eigenschaften

Im folgenden Beispiel wird veranschaulicht, wie die copy-Schleife auf die `dataDisks`-Eigenschaft auf einer VM angewandt wird:

# <a name="json"></a>[JSON](#tab/json)

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "numberOfDataDisks": {
      "type": "int",
      "minValue": 0,
      "maxValue": 16,
      "defaultValue": 3,
      "metadata": {
        "description": "The number of dataDisks to create."
      }
    },
    ...
  },
  "resources": [
    {
      "type": "Microsoft.Compute/virtualMachines",
      "apiVersion": "2020-06-01",
      ...
      "properties": {
        "storageProfile": {
          ...
          "copy": [
            {
              "name": "dataDisks",
              "count": "[parameters('numberOfDataDisks')]",
              "input": {
                "lun": "[copyIndex('dataDisks')]",
                "createOption": "Empty",
                "diskSizeGB": 1023
              }
            }
          ]
        }
        ...
      }
    }
  ]
}
```

Beachten Sie Folgendes: Bei Verwendung von `copyIndex` in einer Eigenschaften-Iteration müssen Sie den Namen der Iteration angeben. Von der Eigenschafteniteration wird auch ein Offsetargument unterstützt. Der Offset muss nach dem Namen der Iteration angegeben werden, z. B. mit `copyIndex('dataDisks', 1)`.

Die bereitgestellte Vorlage sieht wie folgt aus:

```json
{
  "name": "examplevm",
  "type": "Microsoft.Compute/virtualMachines",
  "apiVersion": "2020-06-01",
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

Der „copy“-Vorgang ist besonders bei Verwendung von Arrays hilfreich, weil Sie jedes Element im Array durchlaufen können. Verwenden Sie die Funktion `length` für das Array, um die Anzahl von Iterationen anzugeben, und `copyIndex`, um den aktuellen Index im Array abzurufen.

Die folgende Beispielvorlage erstellt eine Failovergruppe für Datenbanken, die als Array übergeben werden.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "primaryServerName": {
      "type": "string"
    },
    "secondaryServerName": {
      "type": "string"
    },
    "databaseNames": {
      "type": "array",
      "defaultValue": [
        "mydb1",
        "mydb2",
        "mydb3"
      ]
    }
  },
  "variables": {
    "failoverName": "[concat(parameters('primaryServerName'),'/', parameters('primaryServerName'),'failovergroups')]"
  },
  "resources": [
    {
      "type": "Microsoft.Sql/servers/failoverGroups",
      "apiVersion": "2015-05-01-preview",
      "name": "[variables('failoverName')]",
      "properties": {
        "readWriteEndpoint": {
          "failoverPolicy": "Automatic",
          "failoverWithDataLossGracePeriodMinutes": 60
        },
        "readOnlyEndpoint": {
          "failoverPolicy": "Disabled"
        },
        "partnerServers": [
          {
            "id": "[resourceId('Microsoft.Sql/servers', parameters('secondaryServerName'))]"
          }
        ],
        "copy": [
          {
            "name": "databases",
            "count": "[length(parameters('databaseNames'))]",
            "input": "[resourceId('Microsoft.Sql/servers/databases', parameters('primaryServerName'), parameters('databaseNames')[copyIndex('databases')])]"
          }
        ]
      }
    }
  ],
  "outputs": {
  }
}
```

Das `copy`-Element ist ein Array, sodass Sie mehrere Eigenschaften für die Ressource angeben können.

```json
{
  "type": "Microsoft.Network/loadBalancers",
  "apiVersion": "2017-10-01",
  "name": "exampleLB",
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

# <a name="bicep"></a>[Bicep](#tab/bicep)

```bicep
@minValue(0)
@maxValue(16)
@description('The number of dataDisks to be returned in the output array.')
param numberOfDataDisks int = 16

resource vmName 'Microsoft.Compute/virtualMachines@2020-06-01' = {
  ...
  properties: {
    storageProfile: {
      ...
      dataDisks: [for i in range(0, numberOfDataDisks): {
        lun: i
        createOption: 'Empty'
        diskSizeGB: 1023
      }]
    }
    ...
  }
}
```

Die bereitgestellte Vorlage sieht wie folgt aus:

```json
{
  "name": "examplevm",
  "type": "Microsoft.Compute/virtualMachines",
  "apiVersion": "2020-06-01",
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

---

Sie können die Ressourcen- und die Eigenschaften-Iteration zusammen verwenden. Verweisen Sie anhand des Namens auf die Eigenschaften-Iteration.

# <a name="json"></a>[JSON](#tab/json)

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

# <a name="bicep"></a>[Bicep](#tab/bicep)

```bicep
resource vnetname_resource 'Microsoft.Network/virtualNetworks@2018-04-01' = [for i in range(0, 2): {
  name: concat(vnetname, i)
  location: resourceGroup().location
  properties: {
    addressSpace: {
      addressPrefixes: [
        addressPrefix
      ]
    }
    subnets: [for j in range(0, 2): {
      name: 'subnet-${j}'
      properties: {
        addressPrefix: subnetAddressPrefix[j]
      }
    }]
  }
}]
```

---

## <a name="example-templates"></a>Beispielvorlagen

Das folgende Beispiel zeigt ein gängiges Szenario für die Erstellung mehrerer Werte für eine Eigenschaft:

|Vorlage  |BESCHREIBUNG  |
|---------|---------|
|[VM-Bereitstellung mit einer variablen Anzahl von Datenträgern](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-windows-copy-datadisks) |Stellt mehrere Datenträger mit einem virtuellen Computer bereit. |

## <a name="next-steps"></a>Nächste Schritte

- Ein entsprechendes Tutorial finden Sie unter [Tutorial: Erstellen mehrerer Ressourceninstanzen mit ARM-Vorlagen](template-tutorial-create-multiple-instances.md).
- Informationen zu anderen Verwendungsmöglichkeiten der copy-Schleife finden Sie unter:
  - [Ressourceniteration in ARM-Vorlagen](copy-resources.md)
  - [Variableniteration in ARM-Vorlagen](copy-variables.md)
  - [Ausgabeiteration in ARM-Vorlagen](copy-outputs.md)
- Weitere Informationen zu den Abschnitten in einer Vorlage finden Sie unter [Verstehen der Struktur und Syntax von ARM-Vorlagen](template-syntax.md).
- Informationen zum Bereitstellen Ihrer Vorlage finden Sie unter [Bereitstellen von Ressourcen mit ARM-Vorlagen und Azure PowerShell](deploy-powershell.md).
