---
title: Dekompilieren von ARM-Vorlagen-JSON-Code in Bicep
description: Hier werden Befehle zum Dekompilieren von Azure Resource Manager-Vorlagen in Bicep-Dateien beschrieben.
ms.topic: conceptual
ms.date: 06/01/2021
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: b9244eb0a076785c424603dc79ba13af6c29fb03
ms.sourcegitcommit: bd65925eb409d0c516c48494c5b97960949aee05
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/06/2021
ms.locfileid: "111537036"
---
# <a name="decompiling-arm-template-json-to-bicep"></a>Dekompilieren von ARM-Vorlagen-JSON-Code in Bicep

In diesem Artikel wird beschrieben, wie Sie ARM-Vorlagen (Azure Resource Manager) in Bicep-Dateien dekompilieren. Zum Ausführen der Konvertierungsbefehle muss die [Bicep-CLI installiert](./install.md) sein.

Das Dekompilieren einer ARM-Vorlage erleichtert Ihnen den Einstieg in die Bicep-Entwicklung. Wenn Sie über eine Bibliothek mit ARM-Vorlagen verfügen und Bicep für die zukünftige Entwicklung verwenden möchten, können Sie sie in Bicep dekompilieren. Die Bicep-Datei muss jedoch möglicherweise überarbeitet werden, um bewährte Methoden für Bicep zu implementieren.

In diesem Artikel wird gezeigt, wie Sie den Befehl `decompile` in der Azure CLI ausführen. Wenn Sie nicht die Azure CLI verwenden, lassen Sie bei der Ausführung `az` am Anfang des Befehls weg. `az bicep decompile` wird beispielsweise zu ``bicep decompile``.

## <a name="decompile-from-json-to-bicep"></a>Dekompilieren von JSON in Bicep

Verwenden Sie zum Dekompilieren von ARM-Vorlagen-JSON in Bicep Folgendes:

```azurecli
az bicep decompile --file main.json
```

Der Befehl erstellt im Verzeichnis der ARM-Vorlage eine Datei mit dem Namen _main.bicep_.

> [!CAUTION]
> Bei der Dekompilierung wird versucht, die Datei zu konvertieren. Es gibt jedoch keine garantierte Zuordnung von ARM-Vorlagen-JSON-Code zu Bicep. Möglicherweise müssen Sie Warnungen und Fehler in der generierten Bicep-Datei beheben. Oder bei der Dekompilierung kann ein Fehler auftreten, wenn keine genaue Konvertierung möglich ist. Um Probleme oder ungenaue Konvertierungen zu melden, [erstellen Sie ein Problem](https://github.com/Azure/bicep/issues).

Mit den Befehlen „decompile“ und [build](bicep-cli.md#build) werden funktional gleichwertige Vorlagen erstellt. Allerdings ist die Implementierung möglicherweise nicht identisch. Wenn eine Vorlage aus JSON in Bicep und zurück in JSON konvertiert wird, weist die resultierende Vorlage wahrscheinlich eine andere Syntax als die ursprüngliche Vorlage auf. Nach der Bereitstellung führen die konvertierten Vorlagen zu denselben Ergebnissen.

## <a name="fix-conversion-issues"></a>Beheben von Konvertierungsproblemen

Angenommen, Sie verwenden die folgende ARM-Vorlage:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageAccountType": {
      "type": "string",
      "defaultValue": "Standard_LRS",
      "allowedValues": [
        "Standard_LRS",
        "Standard_GRS",
        "Standard_ZRS",
        "Premium_LRS"
      ],
      "metadata": {
        "description": "Storage Account type"
      }
    },
    "location": {
      "type": "string",
      "defaultValue": "[resourceGroup().location]",
      "metadata": {
        "description": "Location for all resources."
      }
    }
  },
  "variables": {
    "storageAccountName": "[concat('store', uniquestring(resourceGroup().id))]"
  },
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "apiVersion": "2019-06-01",
      "name": "[variables('storageAccountName')]",
      "location": "[parameters('location')]",
      "sku": {
        "name": "[parameters('storageAccountType')]"
      },
      "kind": "StorageV2",
      "properties": {}
    }
  ],
  "outputs": {
    "storageAccountName": {
      "type": "string",
      "value": "[variables('storageAccountName')]"
    }
  }
}
```

Beim Dekompilieren erhalten Sie Folgendes:

```bicep
@allowed([
  'Standard_LRS'
  'Standard_GRS'
  'Standard_ZRS'
  'Premium_LRS'
])
@description('Storage Account type')
param storageAccountType string = 'Standard_LRS'

@description('Location for all resources.')
param location string = resourceGroup().location

var storageAccountName_var = 'store${uniqueString(resourceGroup().id)}'

resource storageAccountName 'Microsoft.Storage/storageAccounts@2019-06-01' = {
  name: storageAccountName_var
  location: location
  sku: {
    name: storageAccountType
  }
  kind: 'StorageV2'
  properties: {}
}

output storageAccountName string = storageAccountName_var
```

Die dekompilierte Datei funktioniert, enthält jedoch einige Namen, die Sie ggf. ändern möchten. Die Variable `var storageAccountName_var` verfügt über eine ungewöhnliche Namenskonvention. Ändern Sie sie wie folgt:

```bicep
var uniqueStorageName = 'store${uniqueString(resourceGroup().id)}'
```

Die Ressource hat einen symbolischen Namen, den Sie möglicherweise ändern möchten. Verwenden Sie `exampleStorage` anstelle von `storageAccountName` für den symbolischen Namen.

```bicep
resource exampleStorage 'Microsoft.Storage/storageAccounts@2019-06-01' = {
```

Da Sie den Namen der Variablen für den Speicherkontonamen geändert haben, müssen Sie auch ändern, wo er verwendet wird:

```bicep
resource exampleStorage 'Microsoft.Storage/storageAccounts@2019-06-01' = {
  name: uniqueStorageName
```

Verwenden Sie in der Ausgabe Folgendes:

```bicep
output storageAccountName string = uniqueStorageName
```

Die vollständige Datei sieht wie folgt aus:

```bicep
@allowed([
  'Standard_LRS'
  'Standard_GRS'
  'Standard_ZRS'
  'Premium_LRS'
])
@description('Storage Account type')
param storageAccountType string = 'Standard_LRS'

@description('Location for all resources.')
param location string = resourceGroup().location

var uniqueStorageName = 'store${uniqueString(resourceGroup().id)}'

resource exampleStorage 'Microsoft.Storage/storageAccounts@2019-06-01' = {
  name: uniqueStorageName
  location: location
  sku: {
    name: storageAccountType
  }
  kind: 'StorageV2'
  properties: {}
}

output storageAccountName string = uniqueStorageName
```

## <a name="export-template-and-convert"></a>Exportieren der Vorlage und Konvertierung

Sie können die Vorlage für eine Ressourcengruppe exportieren und dann direkt an den Befehl `decompile` übergeben. Im folgenden Beispiel wird gezeigt, wie Sie eine exportierte Vorlage dekompilieren.

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

```azurecli
az group export --name "your_resource_group_name" > main.json
az bicep decompile --file main.json
```

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell
Export-AzResourceGroup -ResourceGroupName "your_resource_group_name" -Path ./main.json
bicep decompile main.json
```

# <a name="portal"></a>[Portal](#tab/azure-portal)

[Exportieren Sie die Vorlage](../templates/export-template-portal.md) über das Portal.

Verwenden Sie `bicep decompile <filename>` mit der heruntergeladenen Datei.

---

## <a name="side-by-side-view"></a>Parallele Ansicht

Mit dem [Bicep Playground](https://aka.ms/bicepdemo) können Sie entsprechende ARM-Vorlagen- und Bicep-Dateien nebeneinander anzeigen. Sie können eine Beispielvorlage auswählen, um beide Versionen anzuzeigen. Oder Sie wählen `Decompile` aus, um Ihre eigene ARM-Vorlage hochzuladen und die entsprechende Bicep-Datei anzuzeigen.

## <a name="next-steps"></a>Nächste Schritte

Informationen zu allen Befehlen der Bicep-Befehlszeilenschnittstelle finden Sie unter [Befehle der Bicep-Befehlszeilenschnittstelle](bicep-cli.md).
