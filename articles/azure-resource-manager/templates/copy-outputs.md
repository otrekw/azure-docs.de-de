---
title: Definieren mehrerer Instanzen eines Ausgabewerts
description: Verwenden des copy-Vorgangs in einer Azure Resource Manager-Vorlage (ARM), um das Zurückgeben eines Werts aus einer Bereitstellung mehrere Male zu durchlaufen.
ms.topic: conceptual
ms.date: 04/01/2021
ms.openlocfilehash: 49050f4c0a494bbfb470b64704a09d8738a727f7
ms.sourcegitcommit: 77d7639e83c6d8eb6c2ce805b6130ff9c73e5d29
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/05/2021
ms.locfileid: "106385733"
---
# <a name="output-iteration-in-arm-templates"></a>Ausgabeiteration in ARM-Vorlagen

In diesem Artikel wird gezeigt, wie Sie mehr als einen Wert für eine Ausgabe in Ihrer Azure Resource Manager-Vorlage (ARM) erstellen können. Durch das Hinzufügen der copy-Schleife zum Ausgabenabschnitt Ihrer Vorlage können Sie eine Anzahl von Elementen während der Bereitstellung dynamisch zurückgeben.

Sie können die copy-Schleife auch mit [Ressourcen](copy-resources.md), [Eigenschaften in einer Ressource](copy-properties.md) und [Variablen](copy-variables.md) verwenden.

## <a name="syntax"></a>Syntax

# <a name="json"></a>[JSON](#tab/json)

Fügen Sie das `copy`-Element in den Ausgabeabschnitt Ihrer Vorlage ein, um eine Anzahl von Elementen zurückzugeben. Das copy-Element hat das folgende allgemeine Format:

```json
"copy": {
  "count": <number-of-iterations>,
  "input": <values-for-the-output>
}
```

Die `count`-Eigenschaft gibt die gewünschte Anzahl von Iterationen für den Ausgabewert an.

Die Eigenschaft `input` gibt die Eigenschaften an, die Sie wiederholen möchten. Erstellen Sie ein Array von Elementen, das aus dem Wert in der `input`-Eigenschaft erstellt wird. Es kann sich um eine einzelne Eigenschaft handeln (z. B. eine Zeichenfolge) oder um ein Objekt mit mehreren Eigenschaften.

# <a name="bicep"></a>[Bicep](#tab/bicep)

Mit Schleifen kann eine Reihe von Elementen während der Bereitstellung zurückgegeben werden:

- Durchlaufen eines Arrays:

  ```bicep
  output <output-name> array = [for <item> in <collection>: {
    <properties>
  }]

  ```

- Durchlaufen der Elemente eines Arrays

  ```bicep
  output <output-name> array = [for <item>, <index> in <collection>: {
    <properties>
  }]
  ```

- Verwenden des Schleifenindexes

  ```bicep
  output <output-name> array = [for <index> in range(<start>, <stop>): {
    <properties>
  }]
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

## <a name="outputs-iteration"></a>Ausgabeniteration

Im folgenden Beispiel wird eine Variable Anzahl von Speicherkonten erstellt und für jedes Speicherkonto ein Endpunkt zurückgegeben:

# <a name="json"></a>[JSON](#tab/json)

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

# <a name="bicep"></a>[Bicep](#tab/bicep)

```bicep
param storageCount int = 2

var baseName_var = 'storage${uniqueString(resourceGroup().id)}'

resource baseName 'Microsoft.Storage/storageAccounts@2019-04-01' = [for i in range(0, storageCount): {
  name: '${i}${baseName_var}'
  location: resourceGroup().location
  sku: {
    name: 'Standard_LRS'
  }
  kind: 'Storage'
  properties: {}
}]

output storageEndpoints array = [for i in range(0, storageCount): reference(${i}${baseName_var}).primaryEndpoints.blob]
```

---

Die vorangehende Vorlage gibt ein Array mit den folgenden Werten zurück:

```json
[
  "https://0storagecfrbqnnmpeudi.blob.core.windows.net/",
  "https://1storagecfrbqnnmpeudi.blob.core.windows.net/"
]
```

Im nächsten Beispiel werden drei Eigenschaften aus den neuen Speicherkonten zurückgegeben.

# <a name="json"></a>[JSON](#tab/json)

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

# <a name="bicep"></a>[Bicep](#tab/bicep)

```bicep
param storageCount int = 2

var baseName_var = 'storage${uniqueString(resourceGroup().id)}'

resource baseName 'Microsoft.Storage/storageAccounts@2019-04-01' = [for i in range(0, storageCount): {
  name: '${i}${baseName_var}'
  location: resourceGroup().location
  sku: {
    name: 'Standard_LRS'
  }
  kind: 'Storage'
  properties: {}
}]

output storageInfo array = [for i in range(0, storageCount): {
  id: reference(concat(i, baseName_var), '2019-04-01', 'Full').resourceId
  blobEndpoint: reference(concat(i, baseName_var)).primaryEndpoints.blob
  status: reference(concat(i, baseName_var)).statusOfPrimary
}]
```

---

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

- Ein entsprechendes Tutorial finden Sie unter [Tutorial: Erstellen mehrerer Ressourceninstanzen mit ARM-Vorlagen](template-tutorial-create-multiple-instances.md).
- Informationen zu anderen Verwendungsmöglichkeiten der copy-Schleife finden Sie unter:
  - [Ressourceniteration in ARM-Vorlagen](copy-resources.md)
  - [Eigenschafteniteration in ARM-Vorlagen](copy-properties.md)
  - [Variableniteration in ARM-Vorlagen](copy-variables.md)
- Weitere Informationen zu den Abschnitten in einer Vorlage finden Sie unter [Verstehen der Struktur und Syntax von ARM-Vorlagen](template-syntax.md).
- Informationen zum Bereitstellen Ihrer Vorlage finden Sie unter [Bereitstellen von Ressourcen mit ARM-Vorlagen und Azure PowerShell](deploy-powershell.md).
