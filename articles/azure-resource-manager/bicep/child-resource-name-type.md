---
title: Untergeordnete Ressourcen in Bicep
description: Beschreibt, wie Sie den Namen und Typ für untergeordnete Ressourcen in Bicep festlegen.
author: mumian
ms.author: jgao
ms.topic: conceptual
ms.date: 06/01/2021
ms.openlocfilehash: 879c325ee64307e7c548efa4ef7ba34eb68cc896
ms.sourcegitcommit: 8000045c09d3b091314b4a73db20e99ddc825d91
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/19/2021
ms.locfileid: "122444198"
---
# <a name="set-name-and-type-for-child-resources-in-bicep"></a>Festlegen von Name und Typ für untergeordnete Ressourcen in Bicep

Untergeordnete Ressourcen sind Ressourcen, die nur im Kontext einer anderen Ressource verfügbar sind. Beispielsweise kann eine [Erweiterung des virtuellen Computers](/azure/templates/microsoft.compute/virtualmachines/extensions) nicht ohne einen [virtuellen Computer](/azure/templates/microsoft.compute/virtualmachines) vorhanden sein. Die Erweiterungsressource ist ein untergeordnetes Element des virtuellen Computers.

Jede übergeordnete Ressource akzeptiert nur bestimmte Ressourcentypen als untergeordnete Ressourcen. Der Ressourcentyp für die untergeordnete Ressource enthält den Ressourcentyp für die übergeordnete Ressource. Beispielsweise sind `Microsoft.Web/sites/config` und `Microsoft.Web/sites/extensions` beide untergeordnete Ressourcen von `Microsoft.Web/sites`. Die akzeptierten Ressourcentypen werden im [Vorlagenschema](https://github.com/Azure/azure-resource-manager-schemas) der übergeordneten Ressource angegeben.

In Bicep können Sie die untergeordnete Ressource entweder innerhalb oder außerhalb der übergeordneten Ressource angeben. Die Werte, die Sie für den Ressourcennamen und den Ressourcentyp angeben, variieren je nachdem, ob die untergeordnete Ressource innerhalb oder außerhalb der übergeordneten Ressource definiert ist.

## <a name="within-parent-resource"></a>Innerhalb der übergeordneten Ressource

Im folgenden Beispiel ist die untergeordnete Ressource in der Eigenschaft „resources“ der übergeordneten Ressource enthalten.

```bicep
resource <parent-resource-symbolic-name> '<resource-type>@<api-version>' = {
  <parent-resource-properties>

  resource <child-resource-symbolic-name> '<child-resource-type>' = {
    <child-resource-properties>
  }
}
```

Eine verschachtelte Ressourcendeklaration muss auf der obersten Ebene der Syntax der übergeordneten Ressource erscheinen. Deklarationen können beliebig tief verschachtelt werden, solange jede Ebene ein untergeordneter Typ seiner übergeordneten Ressource ist.

Wenn sie innerhalb des übergeordneten Ressourcentyps definiert sind, formatieren Sie die Werte für Typ und Name als ein einziges Segment ohne Schrägstriche. Das folgende Beispiel zeigt ein Speicherkonto mit einem Dateidienst und einer Dateifreigabe. Der Name des Dateidiensts ist auf **default** und sein Typ auf **fileServices** festgelegt. Der Name der Dateifreigabe ist auf **exampleshare** und ihr Typ auf **shares** festgelegt.

```bicep
resource storage 'Microsoft.Storage/storageAccounts@2021-02-01' = {
  name: 'examplestorage'
  location: resourceGroup().location
  kind: 'StorageV2'
  sku: {
    name: 'Standard_LRS'
  }

  resource service 'fileServices' = {
    name: 'default'

    resource share 'shares' = {
      name: 'exampleshare'
    }
  }
}
```

Die vollständigen Ressourcentypen sind immer noch `Microsoft.Storage/storageAccounts/fileServices` und `Microsoft.Storage/storageAccounts/fileServices/shares`. Sie geben `Microsoft.Storage/storageAccounts/` nicht an, weil es vom übergeordneten Ressourcentyp und der Version angenommen wird. Die geschachtelte Ressource kann fakultativ eine API-Version mit der Syntax `<segment>@<version>` deklarieren. Wenn die geschachtelte Ressource die API-Version auslässt, wird die API-Version der übergeordneten Ressource verwendet. Wenn die geschachtelte Ressource eine API-Version angibt, wird die angegebene API-Version verwendet.

Die Namen der untergeordneten Ressourcen sind auf **default** und **exampleshare** festgelegt, die vollständigen Namen enthalten jedoch die übergeordneten Namen. Sie geben weder **examplestorage** noch **default** an, da beide von der übergeordneten Ressource übernommen werden.

Eine geschachtelte Ressource kann auf Eigenschaften ihrer übergeordneten Ressource zugreifen. Andere Ressourcen, die innerhalb des Textkörpers derselben übergeordneten Ressource deklariert sind, können aufeinander verweisen, indem die symbolischen Namen verwendet werden. Eine übergeordnete Ressource greift möglicherweise nicht auf die Eigenschaften der darin enthaltenen Ressourcen zu. Dieser Versuch würde zu einer zyklischen Abhängigkeit führen.

Um auf eine geschachtelte Ressource außerhalb der übergeordneten Ressource zu verweisen, muss sie mit dem enthaltenden Ressourcennamen und dem `::`-Operator qualifiziert werden. So können Sie beispielsweise eine Eigenschaft von einer untergeordneten Ressource ausgeben:

```bicep
output childAddressPrefix string = VNet1::VNet1_Subnet1.properties.addressPrefix
```

## <a name="outside-parent-resource"></a>Außerhalb der übergeordneten Ressource

Das folgende Beispiel zeigt die untergeordnete Ressource außerhalb der übergeordneten Ressource an. So können Sie vorgehen, wenn die übergeordnete Ressource nicht in derselben Vorlage bereitgestellt wird oder wenn Sie [eine Schleife](loop-resources.md) verwenden möchten, um mehrere untergeordnete Ressourcen zu erstellen. Geben Sie die übergeordnete Eigenschaft für das untergeordnete Element an, wobei der Wert auf den symbolischen Namen des übergeordneten Elements festgelegt ist. Mit dieser Syntax müssen Sie weiterhin den vollständigen Ressourcentyp deklarieren, aber der Name der untergeordneten Ressource ist nur der Name des untergeordneten Elements.

```bicep
resource <parent-resource-symbolic-name> '<resource-type>@<api-version>' = {
  name: 'myParent'
  <parent-resource-properties>
}

resource <child-resource-symbolic-name> '<child-resource-type>@<api-version>' = {
  parent: <parent-resource-symbolic-name>
  name: 'myChild'
  <child-resource-properties>
}
```

Bei Definition außerhalb der übergeordneten Ressource formatieren Sie den Typ und Namen mit Schrägstrichen, um den übergeordneten Typ und Namen einzuschließen.

Das folgende Beispiel zeigt ein Speicherkonto, einen Dateidienst und eine Dateifreigabe, die alle auf Stammebene definiert sind.

```bicep
resource storage 'Microsoft.Storage/storageAccounts@2021-02-01' = {
  name: 'examplestorage'
  location: resourceGroup().location
  kind: 'StorageV2'
  sku: {
    name: 'Standard_LRS'
  }
}

resource service 'Microsoft.Storage/storageAccounts/fileServices@2021-02-01' = {
  name: 'default'
  parent: storage
}

resource share 'Microsoft.Storage/storageAccounts/fileServices/shares@2021-02-01' = {
  name: 'exampleshare'
  parent: service
}
```

Das Verweisen auf den symbolischen Namen der untergeordneten Ressource funktioniert genauso wie das Verweisen auf das übergeordnete Element.

## <a name="next-steps"></a>Nächste Schritte

* Informationen zum Erstellen von Bicep-Dateien finden Sie unter [Verstehen der Struktur und Syntax von Bicep-Dateien](./file.md).
* Weitere Informationen zum Format des Ressourcennamens beim Verweisen auf die Ressource finden Sie unter der [Referenzfunktion](./bicep-functions-resource.md#reference).
