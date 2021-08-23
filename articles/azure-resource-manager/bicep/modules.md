---
title: Bicep-Module
description: Hier wird beschrieben, wie ein Modul definiert und verwendet wird und wie Modulbereiche eingesetzt werden.
author: mumian
ms.author: jgao
ms.topic: conceptual
ms.date: 07/15/2021
ms.openlocfilehash: 5e092a0b7f27379cf9fdc488c7a56a295ce17d25
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2021
ms.locfileid: "122340334"
---
# <a name="use-bicep-modules"></a>Verwenden von Bicep-Modulen

Mithilfe von Bicep können Sie eine komplexe Lösung in Modul unterteilen. Ein Bicep-Modul enthält Ressourcen, die zusammen bereitgestellt werden. Durch Module werden komplexe Details der rohen Ressourcen-Deklaration abstrahiert, wodurch sich die Lesbarkeit verbessert. Diese Module können wiederverwendet und für andere Benutzer freigegeben werden. Bicep-Module werden in eine einzige ARM-Vorlage mit [verschachtelten Vorlagen](../templates/linked-templates.md#nested-template) für die Bereitstellung übertragen.

Ein Tutorial finden Sie unter [Erstellen Ihrer ersten Bicep-Vorlage](/learn/modules/deploy-azure-resources-by-using-bicep-templates/).

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
- **Moduldatei**: Auf Moduldateien muss mithilfe relativer Pfade verwiesen werden. Alle Pfade in Bicep müssen mithilfe des eines Schrägstrichs (/) als Verzeichnistrennzeichen angegeben werden, um eine plattformübergreifende konsistente Kompilierung sicherzustellen. Der umgekehrte Schrägstrich von Windows (\\) wird nicht unterstützt. Pfade können Leerzeichen enthalten.
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
- Die **_params_**-Eigenschaft enthält alle Parameter, die an die Moduldatei übergeben werden sollen. Diese Parameter entsprechen den in der Bicep-Datei definierten Parametern.

Module werden wie Ressourcen parallel bereitgestellt, es sei denn, sie hängen von anderen Modulen oder Ressourcenbereitstellungen ab. Weitere Informationen zu Abhängigkeiten finden Sie unter [Festlegen von Ressourcenabhängigkeiten](resource-declaration.md#set-resource-dependencies).

Zum Abrufen eines Ausgabewerts aus einem Modul rufen Sie den Eigenschaftswert mit einer Syntax wie dieser ab: `stgModule.outputs.storageEndpoint`. Dabei ist `stgModule` der Bezeichner des Moduls.

Sie können ein Modul bedingt bereitstellen. Verwenden Sie dieselbe **if**-Syntax, wie Sie sie bei einer [bedingten Bereitstellung einer Ressource](conditional-resource-deployment.md) verwenden würden.

```bicep
param deployZone bool

module dnsZone 'dnszones.bicep' = if (deployZone) {
  name: 'myZoneModule'
}
```

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

Die _scope_-Eigenschaft kann weggelassen werden, wenn der Zielbereich des Moduls und der Zielbereich des übergeordneten Moduls identisch sind. Wenn die scope-Eigenschaft nicht angegeben wird, wird das Modul im Zielbereich des übergeordneten Moduls bereitgestellt.

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

Die scope-Eigenschaft muss auf ein gültiges Bereichsobjekt festgelegt werden. Wenn Ihre Bicep-Datei eine Ressourcengruppe, ein Abonnement oder eine Verwaltungsgruppe bereitgestellt, können Sie den Bereich für ein Modul auf den symbolischen Namen für diese Ressource festlegen. Dieser Ansatz wird im vorherigen Beispiel gezeigt, in dem eine Ressourcengruppe erstellt und für den Bereich eines Moduls verwendet wird.

Sie können auch die Bereichsfunktionen verwenden, um einen gültigen Bereich zu erhalten. Dabei handelt es sich um die folgenden Funktionen:

- [Ressourcengruppe](bicep-functions-scope.md#resourcegroup)
- [Abonnement](bicep-functions-scope.md#subscription)
- [managementGroup](bicep-functions-scope.md#managementgroup)
- [tenant](bicep-functions-scope.md#tenant)

## <a name="next-steps"></a>Nächste Schritte

- Ein Tutorial finden Sie unter [Erstellen Ihrer ersten Bicep-Vorlage](/learn/modules/deploy-azure-resources-by-using-bicep-templates/).
