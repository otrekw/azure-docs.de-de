---
title: Untergeordnete Ressourcen in Vorlagen
description: Beschreibt, wie Sie den Namen und Typ für untergeordnete Ressourcen in einer Azure Resource Manager-Vorlage (ARM-Vorlage) festlegen.
ms.topic: conceptual
ms.date: 12/21/2020
ms.openlocfilehash: a950d72751b829c0a2aa3ba5ca27316a0544d9cc
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "97963911"
---
# <a name="set-name-and-type-for-child-resources"></a>Festlegen von Name und Typ für untergeordnete Ressourcen

Untergeordnete Ressourcen sind Ressourcen, die nur im Kontext einer anderen Ressource verfügbar sind. Beispielsweise kann eine [Erweiterung des virtuellen Computers](/azure/templates/microsoft.compute/virtualmachines/extensions) nicht ohne einen [virtuellen Computer](/azure/templates/microsoft.compute/virtualmachines) vorhanden sein. Die Erweiterungsressource ist ein untergeordnetes Element des virtuellen Computers.

Jede übergeordnete Ressource akzeptiert nur bestimmte Ressourcentypen als untergeordnete Ressourcen. Der Ressourcentyp für die untergeordnete Ressource enthält den Ressourcentyp für die übergeordnete Ressource. Beispielsweise sind `Microsoft.Web/sites/config` und `Microsoft.Web/sites/extensions` beide untergeordnete Ressourcen von `Microsoft.Web/sites`. Die akzeptierten Ressourcentypen werden im [Vorlagenschema](https://github.com/Azure/azure-resource-manager-schemas) der übergeordneten Ressource angegeben.

In einer Azure Resource Manager-Vorlage (ARM-Vorlage) können Sie die untergeordnete Ressource entweder innerhalb oder außerhalb der übergeordneten Ressource angeben. Im folgenden Beispiel ist die untergeordnete Ressource in der Eigenschaft „resources“ der übergeordneten Ressource enthalten.

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

Im nächsten Beispiel ist die untergeordnete Ressource außerhalb der übergeordneten Ressource angegeben. So können Sie vorgehen, wenn die übergeordnete Ressource nicht in derselben Vorlage bereitgestellt wird oder wenn Sie mithilfe von [copy](copy-resources.md) mehrere untergeordnete Ressourcen erstellen möchten.

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

Welche Werte Sie für den Ressourcennamen und Typ angeben, richtet sich danach, ob die untergeordnete Ressource innerhalb oder außerhalb der übergeordneten Ressource definiert ist.

## <a name="within-parent-resource"></a>Innerhalb der übergeordneten Ressource

Bei Definition innerhalb des übergeordneten Ressourcentyps formatieren Sie die Werte für Typ und Name als ein einzelnes Wort ohne Schrägstriche.

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
        "location": "[parameters('location')]",
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

## <a name="outside-parent-resource"></a>Außerhalb der übergeordneten Ressource

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
    "location": "[parameters('location')]",
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

## <a name="next-steps"></a>Nächste Schritte

* Informationen zum Erstellen von ARM-Vorlagen finden Sie unter [Verstehen der Struktur und Syntax von ARM-Vorlagen](template-syntax.md).
* Weitere Informationen zum Format des Ressourcennamens beim Verweisen auf die Ressource finden Sie unter der [Referenzfunktion](template-functions-resource.md#reference).
