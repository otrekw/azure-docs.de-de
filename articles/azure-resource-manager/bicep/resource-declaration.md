---
title: Deklarieren von Ressourcen in Bicep
description: Hier wird beschrieben, wie Ressourcen für die Bereitstellung in Bicep deklariert werden.
author: mumian
ms.author: jgao
ms.topic: conceptual
ms.date: 06/01/2021
ms.openlocfilehash: f62c790f1cb4f0613e17d2bbb3e4fc13e39d8e39
ms.sourcegitcommit: c072eefdba1fc1f582005cdd549218863d1e149e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/10/2021
ms.locfileid: "111963469"
---
# <a name="resource-declaration-in-bicep"></a>Ressourcendeklaration in Bicep

Um eine Ressource über eine Bicep-Datei bereitzustellen, fügen Sie mithilfe des Schlüsselworts `resource` eine Ressourcendeklaration hinzu.

## <a name="set-resource-type-and-version"></a>Festlegen von Ressourcentyp und Version

Wenn Sie Ihrer Bicep-Datei eine Ressource hinzufügen, legen Sie zunächst den Ressourcentyp und die API-Version fest. Diese Werte bestimmen die anderen Eigenschaften, die für die Ressource verfügbar sind.

Im folgenden Beispiel wird gezeigt, wie der Ressourcentyp und die API-Version für ein Speicherkonto festgelegt werden. Das Beispiel zeigt nicht die vollständige Ressourcendeklaration.

```bicep
resource myStorageAccount 'Microsoft.Storage/storageAccounts@2019-06-01' = {
  ...
}
```

Sie legen einen symbolischen Namen für die Ressource fest. Im vorherigen Beispiel lautet der symbolische Name `myStorageAccount`. Sie können einen beliebigen Wert für den symbolischen Namen verwenden, aber er kann nicht mit dem Namen einer anderen Ressource, eines Parameters oder einer Variablen in der Bicep-Datei identisch sein. Der symbolische Name ist nicht mit dem Ressourcennamen identisch. Sie verwenden den symbolischen Namen, um in anderen Teilen der Bicep-Datei auf einfache Weise auf die Ressource zu verweisen.

Das `apiProfile`-Element wird von Bicep nicht unterstützt. Dieses Element ist in [ARM-Vorlagen-JSON-Dateien (Azure Resource Manager)](../templates/syntax.md) verfügbar.

## <a name="set-resource-name"></a>Festlegen des Ressourcennamens

Jede Ressource besitzt einen Namen. Achten Sie beim Festlegen des Ressourcennamens auf die [Regeln und Einschränkungen für Ressourcennamen](../management/resource-name-rules.md).

```bicep
resource stg 'Microsoft.Storage/storageAccounts@2019-06-01' = {
  name: 'examplestorage'
  ...
}
```

In der Regel legen Sie den Namen auf einen Parameter fest, damit Sie während der Bereitstellung unterschiedliche Werte übergeben können.

```bicep
@minLength(3)
@maxLength(24)
param storageAccountName string

resource stg 'Microsoft.Storage/storageAccounts@2019-06-01' = {
  name: storageAccountName
  ...
}
```

## <a name="set-location"></a>Legen Sie einen Speicherort fest

Viele Ressourcen erfordern einen Speicherort. Sie können mittels IntelliSense oder über eine [Vorlagenreferenz](/azure/templates/) ermitteln, ob die Ressource einen Speicherort benötigt. Im folgenden Beispiel wird ein Speicherortparameter hinzugefügt, der für das Speicherkonto verwendet wird.

```bicep
resource stg 'Microsoft.Storage/storageAccounts@2019-06-01' = {
  name: 'examplestorage'
  location: 'eastus'
  ...
}
```

In der Regel legen Sie den Standort auf einen Parameter fest, damit die Bereitstellung an verschiedenen Standorten erfolgen kann.

```bicep
param location string = resourceGroup().location

resource stg 'Microsoft.Storage/storageAccounts@2019-06-01' = {
  name: 'examplestorage'
  location: location
  ...
}
```

An verschiedenen Speicherorten werden unterschiedliche Ressourcentypen unterstützt. Informationen zum Abrufen der unterstützten Standorte für einen Azure-Dienst finden Sie unter [Verfügbare Produkte nach Region](https://azure.microsoft.com/global-infrastructure/services/).  Verwenden Sie Azure PowerShell oder die Azure-Befehlszeilenschnittstelle, um die unterstützten Speicherorte für einen Ressourcentyp abzurufen.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell-interactive
((Get-AzResourceProvider -ProviderNamespace Microsoft.Batch).ResourceTypes `
  | Where-Object ResourceTypeName -eq batchAccounts).Locations
```

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

```azurecli-interactive
az provider show \
  --namespace Microsoft.Batch \
  --query "resourceTypes[?resourceType=='batchAccounts'].locations | [0]" \
  --out table
```

---

## <a name="set-tags"></a>Festlegen von Tags

Sie können während der Bereitstellung Tags auf eine Ressource anwenden. Tags helfen Ihnen dabei, Ihre bereitgestellten Ressourcen logisch zu organisieren. Beispiele für die verschiedenen Methoden zum Angeben der Tags finden Sie unter [ARM-Vorlagen-Tags](../management/tag-resources.md#arm-templates).

## <a name="set-resource-specific-properties"></a>Festlegen ressourcenspezifischer Eigenschaften

Die vorstehenden Eigenschaften sind für die meisten Ressourcentypen generisch. Nachdem Sie diese Werte festgelegt haben, müssen Sie die Eigenschaften festlegen, die für den Ressourcentyp, den Sie bereitstellen, spezifisch sind.

Verwenden Sie IntelliSense oder eine [Vorlagenreferenz](/azure/templates/), um zu bestimmen, welche Eigenschaften verfügbar sind und welche erforderlich sind. Im folgenden Beispiel werden die restlichen Eigenschaften für ein Speicherkonto festgelegt.

```bicep
resource stg 'Microsoft.Storage/storageAccounts@2019-06-01' = {
  name: 'examplestorage'
  location: 'eastus'
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

## <a name="set-resource-dependencies"></a>Festlegen von Ressourcenabhängigkeiten

Beim Bereitstellen einer Ressource müssen Sie möglicherweise sicherstellen, dass andere Ressourcen bereits vorhanden sind. So benötigen Sie beispielsweise einen logischen SQL-Server, bevor Sie eine Datenbank bereitstellen. Diese Beziehung richten Sie ein, indem Sie eine Ressource als von einer anderen Ressource abhängig kennzeichnen. Es gibt zwei Möglichkeiten, die Reihenfolge der Ressourcenbereitstellung zu beeinflussen: durch die [implizite Abhängigkeit](#implicit-dependency) und die [explizite Abhängigkeit](#explicit-dependency).

Azure Resource Manager wertet die Abhängigkeiten zwischen den Ressourcen aus und stellt sie in der Reihenfolge ihrer Abhängigkeiten bereit. Wenn Ressourcen nicht voneinander abhängig sind, stellt Resource Manager sie parallel bereit. Sie müssen nur Abhängigkeiten für Ressourcen definieren, die in der gleichen Bicep-Datei bereitgestellt werden.

### <a name="implicit-dependency"></a>Implizite Abhängigkeit

Eine implizite Abhängigkeit wird erstellt, wenn eine Ressourcendeklaration auf den Bezeichner einer anderen Ressourcendeklaration in einem Ausdruck verweist. Im folgenden Beispiel wird auf *dnsZone* beispielsweise durch die zweite Ressourcendefinition verwiesen:

```bicep
resource dnsZone 'Microsoft.Network/dnszones@2018-05-01' = {
  name: 'myZone'
  location: 'global'
}

resource otherResource 'Microsoft.Example/examples@2020-06-01' = {
  name: 'exampleResource'
  properties: {
    // get read-only DNS zone property
    nameServers: dnsZone.properties.nameServers
  }
}
```

Eine geschachtelte Ressource verfügt ebenfalls über eine implizite Abhängigkeit von ihrer enthaltenden Ressource.

```bicep
resource myParent 'My.Rp/parentType@2020-01-01' = {
  name: 'myParent'
  location: 'West US'

  // depends on 'myParent' implicitly
  resource myChild 'childType' = {
    name: 'myChild'
  }
}
```

Weitere Informationen finden Sie unter [Festlegen von Name und Typ für untergeordnete Ressourcen in Bicep](./child-resource-name-type.md).

### <a name="explicit-dependency"></a>Explizite Abhängigkeit

Eine explizite Abhängigkeit wird über die Eigenschaft `dependsOn` in der Ressourcendeklaration deklariert. Die Eigenschaft akzeptiert ein Array von Ressourcenbezeichnern. Hier sehen Sie ein Beispiel für eine DNS-Zone, die explizit von einer anderen abhängig ist:

```bicep
resource dnsZone 'Microsoft.Network/dnszones@2018-05-01' = {
  name: 'myZone'
  location: 'global'
}

resource otherZone 'Microsoft.Network/dnszones@2018-05-01' = {
  name: 'myZone'
  location: 'global'
  dependsOn: [
    dnsZone
  ]
}
```

Sie können Beziehungen zwischen Ihren Ressourcen mithilfe von `dependsOn` zuordnen, dabei sollte Ihnen jedoch bewusst sein, welchen Zweck diese Zuordnung erfüllt. Um beispielsweise zu dokumentieren, wie Ressourcen miteinander verbunden sind, ist `dependsOn` nicht der richtige Ansatz. Nach der Bereitstellung können Sie nicht mehr abfragen, welche Ressourcen im `dependsOn`-Element definiert waren. Das Festlegen unnötiger Abhängigkeiten verlangsamt die Bereitstellung, da Resource Manager diese Ressourcen nicht gleichzeitig bereitstellen kann.

Explizite Abhängigkeiten sind zwar manchmal erforderlich, dies ist aber selten der Fall. In den meisten Fällen steht Ihnen ein symbolischer Verweis zur Verfügung, der die Abhängigkeit zwischen Ressourcen impliziert. Wenn Sie „dependsOn“ verwenden, sollten Sie erwägen, ob es eine Möglichkeit gibt, dieses Attribut zu entfernen.

### <a name="visualize-dependencies"></a>Visualisieren von Abhängigkeiten

Visual Studio Code stellt ein Tool zum Visualisieren der Abhängigkeiten zur Verfügung. Öffnen Sie eine Bicep-Datei in Visual Studio Code, und wählen Sie dann die Schaltfläche „Schnellansicht“ in der oberen linken Ecke aus.  Der folgende Screenshot zeigt die in der Bicep-Datei definierten Abhängigkeiten einer VM-Ressource.

:::image type="content" source="./media/resource-declaration/bicep-resource-visualizer.png" alt-text="Screenshot: Schnellansicht der Bicep-Ressourcen in Visual Studio Code":::

## <a name="reference-existing-resources"></a>Verweisen auf vorhandene Ressourcen

Sie können Verweise hinzufügen und auf Laufzeiteigenschaften von Ressourcen außerhalb der aktuellen Datei zugreifen, indem Sie das Schlüsselwort `existing` in einer Ressourcendeklaration verwenden. Dies entspricht der Verwendung der [reference()-Funktion](../templates/template-functions-resource.md#reference) in ARM-Vorlagen.

Bei Verwendung des Schlüsselworts `existing` müssen Sie den Namen (`name`) der Ressource angeben und optional auch die `scope`-Eigenschaft für den Zugriff auf eine Ressource in einem anderen Bereich festlegen. Weitere Informationen zur Verwendung der Eigenschaft „scope“ finden Sie unter [Bereitstellung von Ressourcengruppen mit Bicep-Dateien](./deploy-to-resource-group.md).

```bicep
resource stg 'Microsoft.Storage/storageAccounts@2019-06-01' existing = {
  name: 'exampleStorage'
}

output blobEndpoint string = stg.properties.primaryEndpoints.blob
```

Im vorherigen Beispiel wird das Speicherkonto nicht bereitgestellt, aber die Deklaration bietet Zugriff auf Eigenschaften für die vorhandene Ressource. Mit dem symbolischen Namen „stg“ können Sie auf Eigenschaften im Speicherkonto zugreifen.

In den folgenden Beispielen wird veranschaulicht, wie Sie die Eigenschaft `scope` angeben:

resource stg 'Microsoft.Storage/storageAccounts@2019-06-01' existing = { name: 'exampleStorage' scope: resourceGroup(mySub, myRg) }

## <a name="next-steps"></a>Nächste Schritte

- Informationen zur bedingten Bereitstellung einer Ressource finden Sie unter [Bedingte Bereitstellung in Bicep](./conditional-resource-deployment.md).