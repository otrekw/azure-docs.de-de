---
title: Erstellen einer Parameterdatei
description: Erstellen einer Parameterdatei zum Übergeben von Werten während der Bereitstellung einer Azure Resource Manager-Vorlage
ms.topic: conceptual
ms.date: 04/15/2021
ms.openlocfilehash: ddeaed94396aa662b795ae5701aa367ba13d869b
ms.sourcegitcommit: 49b2069d9bcee4ee7dd77b9f1791588fe2a23937
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2021
ms.locfileid: "107531210"
---
# <a name="create-resource-manager-parameter-file"></a>Erstellen einer Resource Manager-Parameterdatei

Anstatt Parameter als Inlinewerte in Ihrem Skript zu übergeben, können Sie eine JSON-Datei verwenden, die die Parameterwerte enthält. In diesem Artikel erfahren Sie, wie Sie eine Parameterdatei erstellen, die Sie mit einer JSON-Vorlage oder einer Bicep-Datei verwenden.

## <a name="parameter-file"></a>Parameterdatei

Eine Parameterdatei verwendet das folgende Format:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "<first-parameter-name>": {
      "value": "<first-value>"
    },
    "<second-parameter-name>": {
      "value": "<second-value>"
    }
  }
}
```

Beachten Sie, dass die Parameterdatei Parameterwerte als Nur-Text speichert. Diese Vorgehensweise funktioniert bei Werten, die nicht sensibler Art sind, z. B. bei einer SKU-Ressource. Nur-Text ist nicht für sensible Werte, z. B. Kennwörter, geeignet. Wenn Sie einen Parameter übergeben müssen, der einen vertraulichen Wert enthält, speichern Sie den Wert in einem Schlüsseltresor. Verweisen Sie dann auf den Schlüsseltresor und das Geheimnis in Ihrer Parameterdatei. Der sensible Wert wird während der Bereitstellung dann auf sichere Weise abgerufen.

Die folgende Parameterdatei enthält einen Klartextwert und einen vertraulichen Wert, der in einem Schlüsseltresor gespeichert ist.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "<first-parameter-name>": {
      "value": "<first-value>"
    },
    "<second-parameter-name>": {
      "reference": {
        "keyVault": {
          "id": "<resource-id-key-vault>"
        },
        "secretName": "<secret-name>"
      }
    }
  }
}
```

Weitere Informationen zur Verwendung von Werten aus einem Schlüsseltresor finden Sie unter [Verwenden von Azure Key Vault zum Übergeben eines sicheren Parameterwerts während der Bereitstellung](key-vault-parameter.md).

## <a name="define-parameter-values"></a>Definieren von Parameterwerten

Um zu bestimmen, wie die Parameternamen und -werte definiert werden, öffnen Sie Ihre JSON- oder Bicep-Vorlage. Sehen Sie sich den Parameterabschnitt der Vorlage an. Die folgenden Beispiele zeigen die Parameter aus JSON- und Bicep-Vorlagen.

# <a name="json"></a>[JSON](#tab/json)

```json
"parameters": {
  "storagePrefix": {
    "type": "string",
    "maxLength": 11
  },
  "storageAccountType": {
    "type": "string",
    "defaultValue": "Standard_LRS",
    "allowedValues": [
    "Standard_LRS",
    "Standard_GRS",
    "Standard_ZRS",
    "Premium_LRS"
    ]
  }
}
```

# <a name="bicep"></a>[Bicep](#tab/bicep)

```bicep
@maxLength(11)
param storagePrefix string

@allowed([
  'Standard_LRS'
  'Standard_GRS'
  'Standard_ZRS'
  'Premium_LRS'
])
param storageAccountType string = 'Standard_LRS'
```

---

In der Parameterdatei ist das erste Detail der Name der einzelnen Parameter. Der Parameternamen in der Parameterdatei müssen mit den Parameternamen in der Vorlage identisch sein.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storagePrefix": {
    },
    "storageAccountType": {
    }
  }
}
```

Beachten Sie die Parametertypskala. Die Parametertypen in ihrer Parameterdatei müssen die gleichen Typen wie Ihre Vorlage verwenden. In diesem Beispiel sind beide Parametertypen Zeichenfolgen.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storagePrefix": {
      "value": ""
    },
    "storageAccountType": {
      "value": ""
    }
  }
}
```

Überprüfen Sie die Vorlage auf Parameter mit einem Standardwert. Wenn ein Parameter über einen Standardwert verfügt, können Sie einen Wert in der Parameterdatei angeben, aber er ist nicht erforderlich. Der Wert der Parameterdatei überschreibt den Standardwert der Vorlage.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storagePrefix": {
      "value": "" // This value must be provided.
    },
    "storageAccountType": {
      "value": "" // This value is optional. Template will use default value if not provided.
    }
  }
}
```

Überprüfen Sie die zulässigen Werte der Vorlage und alle Einschränkungen, z. B. die maximale Länge. Diese Werte geben den Wertebereich an, den Sie für einen Parameter angeben können. In diesem Beispiel kann`storagePrefix` maximal 11 Zeichen enthalten und `storageAccountType` muss einen zulässigen Wert angeben.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storagePrefix": {
      "value": "storage"
    },
    "storageAccountType": {
      "value": "Standard_ZRS"
    }
  }
}
```

> [!NOTE]
> Ihre Parameterdatei kann nur Werte für Parameter enthalten, die in der Vorlage definiert sind. Wenn die Parameterdatei zusätzliche Parameter enthält, die nicht mit den Parametern in der Vorlage identisch sind, erhalten Sie eine Fehlermeldung.

## <a name="parameter-type-formats"></a>Parametertypformate

Das folgende Beispiel zeigt die Formate verschiedener Parametertypen: Zeichenfolge, ganze Zahl, Boolean, Array und Objekt.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "exampleString": {
      "value": "test string"
    },
    "exampleInt": {
      "value": 4
    },
    "exampleBool": {
      "value": true
    },
    "exampleArray": {
      "value": [
        "value 1",
        "value 2"
      ]
    },
    "exampleObject": {
      "value": {
        "property1": "value1",
        "property2": "value2"
      }
    }
  }
}
```

## <a name="deploy-template-with-parameter-file"></a>Bereitstellen der Vorlage mit einer Parameterdatei

In Azure CLI übergeben Sie eine lokale Parameterdatei mit `@` und dem Namen der Parameterdatei. Beispiel: `@storage.parameters.json`.

```azurecli
az deployment group create \
  --name ExampleDeployment \
  --resource-group ExampleGroup \
  --template-file storage.json \
  --parameters @storage.parameters.json
```

Weitere Informationen finden Sie unter [Bereitstellen von Ressourcen mit ARM-Vorlagen und der Azure CLI](./deploy-cli.md#parameters). Zum Bereitstellen von _.bicep_-Dateien benötigen Sie Version 2.20 oder höher der Azure CLI.

In Azure PowerShell übergeben Sie eine lokale Parameterdatei mithilfe des `TemplateParameterFile`-Parameters.

```azurepowershell
New-AzResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup `
  -TemplateFile C:\MyTemplates\storage.json `
  -TemplateParameterFile C:\MyTemplates\storage.parameters.json
```

Weitere Informationen finden Sie unter [Bereitstellen von Ressourcen mit ARM-Vorlagen und Azure PowerShell](./deploy-powershell.md#pass-parameter-values) Zum Bereitstellen von BICEP-Dateien benötigen Sie _Version 5.6.0 oder höher von Azure PowerShell_.

> [!NOTE]
> Es ist nicht möglich, eine Parameterdatei mit dem benutzerdefinierten Vorlagenblatt im Portal zu verwenden.

> [!TIP]
> Stellen Sie bei Verwendung des [Projekts „Azure-Ressourcengruppe“ in Visual Studio](create-visual-studio-deployment-project.md) sicher, dass die Eigenschaft **Buildvorgang** der Parameterdatei auf **Inhalt** festgelegt ist.

## <a name="file-name"></a>Dateiname

Die allgemeine Konvention für das Benennen der Parameterdatei ist das Hinzufügen von _Parametern_ zum Vorlagennamen. Wenn Ihre Vorlage beispielsweise den Namen _azuredeploy.json_ hat, ergibt sich für Ihre Parameterdatei der Name _azuredeploy.parameters.json_. Anhand dieser Namenskonvention können Sie die Verbindung zwischen der Vorlage und den Parametern erkennen.

Erstellen Sie mehr als eine Parameterdatei, wenn Sie die Bereitstellung in unterschiedlichen Umgebungen durchführen möchten. Wenn Sie die Parameterdateien benennen, identifizieren Sie deren Verwendung, z. B. Entwicklung und Produktion. Verwenden Sie beispielsweise _azuredeploy.parameters-dev.json_ und _azuredeploy.parameters-prod.json_, um Ressourcen bereitzustellen.

## <a name="parameter-precedence"></a>Parameterrangfolge

Sie können Inlineparameter und eine lokale Parameterdatei im selben Bereitstellungsvorgang verwenden. Sie können beispielsweise einige Werte in der lokalen Parameterdatei angeben und weitere Werte während der Bereitstellung inline hinzufügen. Wenn Sie Werte für einen Parameter sowohl in der lokalen Parameterdatei als auch inline bereitstellen, haben die Inlinewerte Vorrang.

Sie können eine externe Parameterdatei verwenden, indem Sie den URI der Datei bereitstellen. Bei Verwendung einer externen Parameterdatei können Sie keine anderen Werte (weder inline noch aus einer lokalen Datei) übergeben. Alle Inlineparameter werden ignoriert. Stellen Sie alle Parameterwerte in der externen Datei bereit.

## <a name="parameter-name-conflicts"></a>Parameternamenskonflikte

Wenn Ihre Vorlage einen Parameter enthält, der den gleichen Namen besitzt wie einer der Parameter des PowerShell-Befehls, zeigt PowerShell den Parameter der Vorlage mit dem Postfix `FromTemplate` an. Beispiel: Ein Parameter namens `ResourceGroupName` in Ihrer Vorlage verursacht einen Konflikt mit dem Parameter `ResourceGroupName` im Cmdlet [New-AzResourceGroupDeployment](/powershell/module/az.resources/new-azresourcegroupdeployment). Sie werden zur Eingabe eines Werts für `ResourceGroupNameFromTemplate` aufgefordert. Verwenden Sie Parameternamen, die nicht für Bereitstellungsbefehle verwendet werden, um diese Verwechslung zu vermeiden.

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zum Definieren von Parametern in einer Vorlage finden Sie unter [Parameter in ARM-Vorlagen](template-parameters.md).
- Weitere Informationen zur Verwendung von Werten aus einem Schlüsseltresor finden Sie unter [Verwenden von Azure Key Vault zum Übergeben eines sicheren Parameterwerts während der Bereitstellung](key-vault-parameter.md).
