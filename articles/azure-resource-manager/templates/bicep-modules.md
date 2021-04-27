---
title: Bicep-Module
description: Hier wird beschrieben, wie ein Modul definiert und verwendet wird und wie Modulbereiche eingesetzt werden.
ms.topic: conceptual
ms.date: 03/30/2021
ms.openlocfilehash: 6c325bbbe265e13241119761373985ca4552b158
ms.sourcegitcommit: f5448fe5b24c67e24aea769e1ab438a465dfe037
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "105967879"
---
# <a name="use-bicep-modules-preview"></a>Verwenden Sie Bicep-Module (Vorschau)

Mithilfe von Bicep können Sie eine komplexe Lösung in Modul unterteilen. Ein Bicep-Modul enthält Ressourcen, die zusammen bereitgestellt werden. Durch Module werden komplexe Details der rohen Ressourcen-Deklaration abstrahiert, wodurch sich die Lesbarkeit verbessert. Diese Module können wiederverwendet und für andere Benutzer freigegeben werden. In Kombination mit [Vorlagenspezifikationen](./template-specs.md) bieten sie Möglichkeiten für die Modularität und Wiederverwendung von Code. Bicep-Module werden in eine einzige ARM-Vorlage mit [verschachtelten Vorlagen](./linked-templates.md#nested-template) für die Bereitstellung übertragen. In Bicep wird [_dependsOn_](./template-syntax.md#resources) automatisch bearbeitet.

Ein Tutorial hierzu finden Sie unter [Tutorial: Hinzufügen von Bicep-Modulen](./bicep-tutorial-add-modules.md).

## <a name="define-modules"></a>Definieren von Modulen

Jede Bicep-Datei kann als Modul verwendet werden. Ein Modul stellt nur Parameter und Ausgaben als Vertrag für andere Bicep-Dateien bereit. Sowohl Parameter als auch Ausgaben sind optional.

Die folgende Bicep-Datei kann zum Erstellen eines Speicherkontos bereitgestellt oder als Modul verwendet werden.  Im nächsten Abschnitt erfahren Sie, wie Module verwendet werden:

```bicep
@minLength(3)
@maxLength(11)
param storagePrefix string

@allowed([
  'Standard_LRS'
  'Standard_GRS'
  'Standard_RAGRS'
  'Standard_ZRS'
  'Premium_LRS'
  'Premium_ZRS'
  'Standard_GZRS'
  'Standard_RAGZRS'
])
param storageSKU string = 'Standard_LRS'
param location string

var uniqueStorageName = '${storagePrefix}${uniqueString(resourceGroup().id)}'

resource stg 'Microsoft.Storage/storageAccounts@2019-04-01' = {
  name: uniqueStorageName
  location: location
  sku: {
    name: storageSKU
  }
  kind: 'StorageV2'
  properties: {
    supportsHttpsTrafficOnly: true
  }
}

output storageEndpoint object = stg.properties.primaryEndpoints
```

Die Ausgabe wird verwendet, um Werte an die übergeordneten Bicep-Dateien zu übergeben.

## <a name="consume-modules"></a>Verwenden von Modulen

Verwenden Sie das Schlüsselwort _module_, um ein Modul zu verwenden. Mit der folgenden Bicep-Datei wird die in der referenzierten Moduldatei definierte Ressource bereitgestellt:

```bicep
@minLength(3)
@maxLength(11)
param namePrefix string
param location string = resourceGroup().location

module stgModule './storageAccount.bicep' = {
  name: 'storageDeploy'
  params: {
    storagePrefix: namePrefix
    location: location
  }
}

output storageEndpoint object = stgModule.outputs.storageEndpoint
```

- **module**: Schlüsselwort.
- **symbolischer Name** (stgModule): Bezeichner für das Modul.
- **Moduldatei**: Der Pfad zum Modul in diesem Beispiel wird unter Verwendung eines relativen Pfads (./storageAccount.bicep) angegeben. Alle Pfade in Bicep müssen mithilfe des eines Schrägstrichs (/) als Verzeichnistrennzeichen angegeben werden, um eine plattformübergreifende konsistente Kompilierung sicherzustellen. Der umgekehrte Schrägstrich von Windows (\\) wird nicht unterstützt.
- Die Eigenschaft **_name_** (storageDeploy) ist bei der Verwendung eines Moduls erforderlich. Wenn mit Bicep die Vorlage IL generiert wird, wird dieses Feld als Name der Ressource für geschachtelte Bereitstellung verwendet, die für das Modul generiert wird:

    ```json
    ...
    ...
    "resources": [
      {
        "type": "Microsoft.Resources/deployments",
        "apiVersion": "2020-10-01",
        "name": "storageDeploy",
        "properties": {
          ...
        }
      }
    ]
    ...
    ```

Zum Abrufen eines Ausgabewerts aus einem Modul rufen Sie den Eigenschaftswert mit einer Syntax wie dieser ab: `stgModule.outputs.storageEndpoint`. Dabei ist `stgModule` der Bezeichner des Moduls.

## <a name="configure-module-scopes"></a>Konfigurieren von Modulbereichen

Beim Deklarieren eines Moduls können Sie eine _scope_-Eigenschaft bereitstellen, um den Bereich festzulegen, in dem das Modul bereitgestellt werden soll:

```bicep
module stgModule './storageAccount.bicep' = {
  name: 'storageDeploy'
  scope: resourceGroup('someOtherRg') // pass in a scope to a different resourceGroup
  params: {
    storagePrefix: namePrefix
    location: location
  }
}
```

Die _scope_-Eigenschaft kann weggelassen werden, wenn der Zielbereich des Moduls und der Zielbereich des übergeordneten Moduls identisch sind. Wenn die „scope“-Eigenschaft nicht angegeben wird, wird das Modul im Zielbereich des übergeordneten Moduls bereitgestellt.

Anhand der folgenden Bicep-Datei wird deutlich, wie eine Ressourcengruppe erstellt und ein Modul in der Ressourcengruppe bereitgestellt wird:

```bicep
// set the target scope for this file
targetScope = 'subscription'

@minLength(3)
@maxLength(11)
param namePrefix string

param location string = deployment().location

var resourceGroupName = '${namePrefix}rg'
resource myResourceGroup 'Microsoft.Resources/resourceGroups@2020-01-01' = {
  name: resourceGroupName
  location: location
  scope: subscription()
}

module stgModule './storageAccount.bicep' = {
  name: 'storageDeploy'
  scope: myResourceGroup
  params: {
    storagePrefix: namePrefix
    location: location
  }
}

output storageEndpoint object = stgModule.outputs.storageEndpoint
```

## <a name="next-steps"></a>Nächste Schritte

- Ein Tutorial hierzu finden Sie unter [Tutorial: Hinzufügen von Bicep-Modulen](./bicep-tutorial-add-modules.md).
