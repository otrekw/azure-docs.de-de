---
title: Untergeordnete Ressourcen in Vorlagen
description: Beschreibt, wie Sie den Namen und Typ für untergeordnete Ressourcen in einer Azure Resource Manager-Vorlage (ARM-Vorlage) festlegen.
ms.topic: conceptual
ms.date: 04/23/2021
ms.openlocfilehash: 1928a94fbfefc694091a3f09ea577e8250540bf4
ms.sourcegitcommit: ad921e1cde8fb973f39c31d0b3f7f3c77495600f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/25/2021
ms.locfileid: "107949522"
---
# <a name="set-name-and-type-for-child-resources"></a>Festlegen von Name und Typ für untergeordnete Ressourcen

Untergeordnete Ressourcen sind Ressourcen, die nur im Kontext einer anderen Ressource verfügbar sind. Beispielsweise kann eine [Erweiterung des virtuellen Computers](/azure/templates/microsoft.compute/virtualmachines/extensions) nicht ohne einen [virtuellen Computer](/azure/templates/microsoft.compute/virtualmachines) vorhanden sein. Die Erweiterungsressource ist ein untergeordnetes Element des virtuellen Computers.

Jede übergeordnete Ressource akzeptiert nur bestimmte Ressourcentypen als untergeordnete Ressourcen. Der Ressourcentyp für die untergeordnete Ressource enthält den Ressourcentyp für die übergeordnete Ressource. Beispielsweise sind `Microsoft.Web/sites/config` und `Microsoft.Web/sites/extensions` beide untergeordnete Ressourcen von `Microsoft.Web/sites`. Die akzeptierten Ressourcentypen werden im [Vorlagenschema](https://github.com/Azure/azure-resource-manager-schemas) der übergeordneten Ressource angegeben.

[!INCLUDE [Bicep preview](../../../includes/resource-manager-bicep-preview.md)]

In einer Azure Resource Manager-Vorlage (ARM-Vorlage) können Sie die untergeordnete Ressource entweder innerhalb oder außerhalb der übergeordneten Ressource angeben. Die Werte, die Sie für den Ressourcennamen und den Ressourcentyp angeben, variieren je nachdem, ob die untergeordnete Ressource innerhalb oder außerhalb der übergeordneten Ressource definiert ist.

## <a name="within-parent-resource"></a>Innerhalb der übergeordneten Ressource

Im folgenden Beispiel ist die untergeordnete Ressource in der Eigenschaft „resources“ der übergeordneten Ressource enthalten.

# <a name="json"></a>[JSON](#tab/json)

```json
"resources": [
  {
    <parent-resource>
    "resources": [
      <child-resource>
    ]
  }
]
```

Untergeordnete Ressourcen können nur mit fünf Ebenen definiert werden.

Wenn sie innerhalb des übergeordneten Ressourcentyps definiert sind, formatieren Sie die Werte für Typ und Name als ein einziges Segment ohne Schrägstriche.

```json
"type": "{child-resource-type}",
"name": "{child-resource-name}",
```

Das folgende Beispiel zeigt ein virtuelles Netzwerk mit einem Subnetz. Beachten Sie, dass das Subnetz im Ressourcenarray für das virtuelle Netzwerk enthalten ist. Der Name ist auf **Subnet1** und der Typ auf **subnets** festgelegt. Die untergeordnete Ressource ist als von der übergeordneten Ressource abhängig gekennzeichnet, da die übergeordnete Ressource vorhanden sein muss, bevor die untergeordnete Ressource bereitgestellt werden kann.

```json
"resources": [
  {
    "type": "Microsoft.Network/virtualNetworks",
    "apiVersion": "2018-10-01",
    "name": "VNet1",
    "location": "[parameters('location')]",
    "properties": {
      "addressSpace": {
        "addressPrefixes": [
          "10.0.0.0/16"
        ]
      }
    },
    "resources": [
      {
        "type": "subnets",
        "apiVersion": "2018-10-01",
        "name": "Subnet1",
        "dependsOn": [
          "VNet1"
        ],
        "properties": {
          "addressPrefix": "10.0.0.0/24"
        }
      }
    ]
  }
]
```

Der vollständige Ressourcentyp lautet immer noch `Microsoft.Network/virtualNetworks/subnets`. `Microsoft.Network/virtualNetworks/` wird nicht angegeben, da dieser Teil vom übergeordneten Ressourcentyp übernommen wird.

Der Name der untergeordneten Ressource ist auf **Subnet1** festgelegt, der vollständige Name enthält jedoch auch den Namen der übergeordneten Ressource. **VNet1** wird nicht angegeben, da dieser Teil von der übergeordneten Ressource übernommen wird.

# <a name="bicep"></a>[Bicep](#tab/bicep)

```bicep
resource <parent-resource-symbolic-name> '<resource-type>@<api-version>' = {
  <parent-resource-properties>

  resource <child-resource-symbolic-name> '<child-resource-type>' = {
    <child-resource-properties>
  }
}
```

Eine verschachtelte Ressourcendeklaration muss auf der obersten Ebene der Syntax der übergeordneten Ressource erscheinen. Deklarationen können beliebig tief verschachtelt werden, solange jede Ebene ein untergeordneter Typ seiner übergeordneten Ressource ist.

Wenn sie innerhalb des übergeordneten Ressourcentyps definiert sind, formatieren Sie die Werte für Typ und Name als ein einziges Segment ohne Schrägstriche. Das folgende Beispiel zeigt ein virtuelles Netzwerk mit einem Subnetz. Beachten Sie, dass das Subnetz im Ressourcenarray für das virtuelle Netzwerk enthalten ist. Der Name ist auf **Subnet1** und der Typ auf **subnets** festgelegt.

```bicep
param location string

resource VNet1 'Microsoft.Network/virtualNetworks@2018-10-01' = {
  name: 'VNet1'
  location: location
  properties: {
    addressSpace: {
      addressPrefixes: [
        '10.0.0.0/16'
      ]
    }
  }

  resource VNet1_Subnet1 'subnets' = {
    name: 'Subnet1'
    properties: {
      addressPrefix: '10.0.0.0/24'
    }
  }
}
```

Der vollständige Ressourcentyp lautet immer noch `Microsoft.Network/virtualNetworks/subnets`. Sie geben `Microsoft.Network/virtualNetworks/` nicht an, weil es vom übergeordneten Ressourcentyp und der Version angenommen wird. Die geschachtelte Ressource kann fakultativ eine API-Version mit der Syntax `<segment>@<version>` deklarieren. Wenn die geschachtelte Ressource die API-Version auslässt, wird die API-Version der übergeordneten Ressource verwendet. Wenn die geschachtelte Ressource eine API-Version angibt, wird die angegebene API-Version verwendet.

Der Name der untergeordneten Ressource ist auf **Subnet1** festgelegt, der vollständige Name enthält jedoch auch den Namen der übergeordneten Ressource. VNet1 wird nicht angegeben, da dieser Teil von der übergeordneten Ressource übernommen wird.

Um auf den symbolischen Namen der untergeordneten Ressource zuzugreifen, müssen Sie den `::`-Operator verwenden. So können Sie beispielsweise eine Eigenschaft von einer untergeordneten Ressource ausgeben:

```bicep
output childAddressPrefix string = VNet1::VNet1_Subnet1.properties.addressPrefix
```

Eine geschachtelte Ressource kann auf Eigenschaften ihrer übergeordneten Ressource zugreifen. Andere Ressourcen, die innerhalb des Textkörpers derselben übergeordneten Ressource deklariert sind, können aufeinander verweisen, und es gelten die typischen Regeln zu zyklischen Abhängigkeiten. Eine übergeordnete Ressource greift möglicherweise nicht auf die Eigenschaften der darin enthaltenen Ressourcen zu. Dies würde zu einer zyklischen Abhängigkeit führen.

---

## <a name="outside-parent-resource"></a>Außerhalb der übergeordneten Ressource

Das folgende Beispiel zeigt die untergeordnete Ressource außerhalb der übergeordneten Ressource an. So können Sie vorgehen, wenn die übergeordnete Ressource nicht in derselben Vorlage bereitgestellt wird oder wenn Sie mithilfe von [copy](copy-resources.md) mehrere untergeordnete Ressourcen erstellen möchten.

# <a name="json"></a>[JSON](#tab/json)

```json
"resources": [
  {
    <parent-resource>
  },
  {
    <child-resource>
  }
]
```

Bei Definition außerhalb der übergeordneten Ressource formatieren Sie den Typ und Namen mit Schrägstrichen, um den übergeordneten Typ und Namen einzuschließen.

```json
"type": "{resource-provider-namespace}/{parent-resource-type}/{child-resource-type}",
"name": "{parent-resource-name}/{child-resource-name}",
```

Das folgende Beispiel zeigt ein virtuelles Netzwerk und ein Subnetz, die beide auf Stammebene definiert sind. Beachten Sie, dass das Subnetz nicht im Ressourcenarray für das virtuelle Netzwerk enthalten ist. Der Name ist auf **VNet1/Subnet1** und der Typ auf `Microsoft.Network/virtualNetworks/subnets` festgelegt. Die untergeordnete Ressource ist als von der übergeordneten Ressource abhängig gekennzeichnet, da die übergeordnete Ressource vorhanden sein muss, bevor die untergeordnete Ressource bereitgestellt werden kann.

```json
"resources": [
  {
    "type": "Microsoft.Network/virtualNetworks",
    "apiVersion": "2018-10-01",
    "name": "VNet1",
    "location": "[parameters('location')]",
    "properties": {
      "addressSpace": {
        "addressPrefixes": [
          "10.0.0.0/16"
        ]
      }
    }
  },
  {
    "type": "Microsoft.Network/virtualNetworks/subnets",
    "apiVersion": "2018-10-01",
    "name": "VNet1/Subnet1",
    "dependsOn": [
      "VNet1"
    ],
    "properties": {
      "addressPrefix": "10.0.0.0/24"
    }
  }
]
```

# <a name="bicep"></a>[Bicep](#tab/bicep)

```bicep
resource <parent-resource-symbolic-name> '<resource-type>@<api-version>' = {
  <parent-resource-properties>
}

resource <child-resource-symbolic-name> '<child-resource-type>@<api-version>' = {
  <child-resource-properties>
}
```

Bei Definition außerhalb der übergeordneten Ressource formatieren Sie den Typ und Namen mit Schrägstrichen, um den übergeordneten Typ und Namen einzuschließen.

Das folgende Beispiel zeigt ein virtuelles Netzwerk und ein Subnetz, die beide auf Stammebene definiert sind. Beachten Sie, dass das Subnetz nicht im Ressourcenarray für das virtuelle Netzwerk enthalten ist. Der Name ist auf **VNet1/Subnet1** und der Typ auf `Microsoft.Network/virtualNetworks/subnets` festgelegt. Die untergeordnete Ressource ist als von der übergeordneten Ressource abhängig gekennzeichnet, da die übergeordnete Ressource vorhanden sein muss, bevor die untergeordnete Ressource bereitgestellt werden kann.

```bicep
param location string

resource VNet1 'Microsoft.Network/virtualNetworks@2018-10-01' = {
  name: 'VNet1'
  location: location
  properties: {
    addressSpace: {
      addressPrefixes: [
        '10.0.0.0/16'
      ]
    }
  }
}

resource VNet1_Subnet1 'Microsoft.Network/virtualNetworks/subnets@2018-10-01' = {
  name: '${VNet1.name}/Subnet1'
  properties: {
    addressPrefix: '10.0.0.0/24'
  }
}
```

---

## <a name="next-steps"></a>Nächste Schritte

* Informationen zum Erstellen von ARM-Vorlagen finden Sie unter [Verstehen der Struktur und Syntax von ARM-Vorlagen](template-syntax.md).
* Weitere Informationen zum Format des Ressourcennamens beim Verweisen auf die Ressource finden Sie unter der [Referenzfunktion](template-functions-resource.md#reference).
