---
title: Erstellen einer Parameterdatei
description: Erstellen einer Parameterdatei zum Übergeben von Werten während der Bereitstellung einer Azure Resource Manager-Vorlage
ms.topic: conceptual
ms.date: 09/01/2020
ms.openlocfilehash: 2b6d942b21594fa608127bb8f403e72295671005
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "89276642"
---
# <a name="create-resource-manager-parameter-file"></a>Erstellen einer Resource Manager-Parameterdatei

Anstatt Parameter als Inlinewerte in Ihrem Skript zu übergeben, ist es wohl einfacher, eine JSON-Datei zu verwenden, die die Parameterwerte enthält. In diesem Artikel wird veranschaulicht, wie Sie die Parameterdatei erstellen.

## <a name="parameter-file"></a>Parameterdatei

Die Parameterdatei hat das folgende Format:

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

Beachten Sie, dass die Parameterwerte in der Parameterdatei als Klartext gespeichert werden. Diese Vorgehensweise funktioniert bei Werten, die nicht sensibler Art sind, z. B. beim Angeben der SKU für eine Ressource. Sie ist nicht für sensible Werte, z. B. Kennwörter, geeignet. Falls Sie einen sensiblen Wert als Parameter übergeben müssen, sollten Sie den Wert in einem Schlüsseltresor speichern und in Ihrer Parameterdatei auf den Schlüsseltresor verweisen. Der sensible Wert wird während der Bereitstellung dann auf sichere Weise abgerufen.

Die folgende Parameterdatei enthält einen Klartextwert und einen Wert, der in einem Schlüsseltresor gespeichert ist.

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

Öffnen Sie die Vorlage, die Sie bereitstellen, um zu ermitteln, wie Sie die Parameterwerte definieren. Sehen Sie sich den Parameterabschnitt der Vorlage an. Im folgenden Beispiel sind die Parameter aus einer Vorlage dargestellt.

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

Das erste Detail ist der Name der einzelnen Parameter. Die Werte in Ihrer Parameterdatei müssen mit den Namen übereinstimmen.

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

Achten Sie auf den Typ des Parameters. Die Werte in Ihrer Parameterdatei müssen die gleichen Typen aufweisen. Für diese Vorlage können Sie beide Parameter als Zeichenfolgen angeben.

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

Suchen Sie als Nächstes nach einem Standardwert. Wenn ein Parameter über einen Standardwert verfügt, können Sie einen Wert angeben, aber dies ist nicht zwingend erforderlich.

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

Sehen Sie sich abschließend die zulässigen Werte und die Einschränkungen an, z. B. die maximale Länge. Hieraus können Sie den Bereich der Werte ablesen, den Sie für den Parameter angeben können.

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

Ihre Parameterdatei kann nur Werte für Parameter enthalten, die in der Vorlage definiert sind. Wenn die Parameterdatei zusätzliche Parameter enthält, die nicht mit den Parametern in der Vorlage identisch sind, erhalten Sie eine Fehlermeldung.

## <a name="parameter-type-formats"></a>Parametertypformate

Im folgenden Beispiel sind die Formate der unterschiedlichen Parametertypen dargestellt.

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

Um eine lokale Parameterdatei mit Azure CLI zu übergeben, verwenden Sie @ und den Namen der Parameterdatei.

```azurecli
az deployment group create \
  --name ExampleDeployment \
  --resource-group ExampleGroup \
  --template-file storage.json \
  --parameters @storage.parameters.json
```

Weitere Informationen finden Sie unter [Bereitstellen von Ressourcen mit ARM-Vorlagen und der Azure CLI](./deploy-cli.md#parameters).

Um eine lokale Parameterdatei mit Azure PowerShell zu übergeben, verwenden Sie den `TemplateParameterFile`-Parameter.

```azurepowershell
New-AzResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup `
  -TemplateFile c:\MyTemplates\azuredeploy.json `
  -TemplateParameterFile c:\MyTemplates\storage.parameters.json
```

Weitere Informationen finden Sie unter [Bereitstellen von Ressourcen mit ARM-Vorlagen und Azure PowerShell](./deploy-powershell.md#pass-parameter-values).

> [!NOTE]
> Es ist nicht möglich, eine Parameterdatei mit dem benutzerdefinierten Vorlagenblatt im Portal zu verwenden.

## <a name="file-name"></a>Dateiname

Die allgemeine Konvention für das Benennen der Parameterdatei ist das Hinzufügen von **.parameters** zum Vorlagennamen. Wenn Ihre Vorlage beispielsweise den Namen **azuredeploy.json** hat, ergibt sich für Ihre Parameterdatei der Name **azuredeploy.parameters.json**. Anhand dieser Namenskonvention können Sie die Verbindung zwischen der Vorlage und den Parametern erkennen.

Erstellen Sie mehr als eine Parameterdatei, wenn Sie die Bereitstellung in unterschiedlichen Umgebungen durchführen möchten. Fügen Sie beim Benennen der Parameterdatei Informationen zu ihrem Verwendungszweck hinzu. Verwenden Sie beispielsweise **azuredeploy.parameters-dev.json** und **azuredeploy.parameters-prod.json**.

## <a name="parameter-precedence"></a>Parameterrangfolge

Sie können Inlineparameter und eine lokale Parameterdatei im selben Bereitstellungsvorgang verwenden. Sie können beispielsweise einige Werte in der lokalen Parameterdatei angeben und weitere Werte während der Bereitstellung inline hinzufügen. Wenn Sie Werte für einen Parameter sowohl in der lokalen Parameterdatei als auch inline bereitstellen, haben die Inlinewerte Vorrang.

Sie können eine externe Parameterdatei verwenden, indem Sie den URI der Datei bereitstellen. Bei Verwendung einer externen Parameterdatei können Sie keine anderen Werte (weder inline noch aus einer lokalen Datei) übergeben. Alle Inlineparameter werden ignoriert. Stellen Sie alle Parameterwerte in der externen Datei bereit.

## <a name="parameter-name-conflicts"></a>Parameternamenskonflikte

Wenn Ihre Vorlage einen Parameter enthält, der den gleichen Namen besitzt wie einer der Parameter des PowerShell-Befehls, zeigt PowerShell den Parameter der Vorlage mit dem Postfix **FromTemplate** an. Beispiel: Ein Parameter namens **ResourceGroupName** in Ihrer Vorlage verursacht einen Konflikt mit dem Parameter **ResourceGroupName** im Cmdlet [New-AzResourceGroupDeployment](/powershell/module/az.resources/new-azresourcegroupdeployment). Sie werden zur Eingabe eines Werts für **ResourceGroupNameFromTemplate** aufgefordert. Sie können diese Verwirrung vermeiden, indem Sie Parameternamen verwenden, die nicht für Bereitstellungsbefehle verwendet werden.


## <a name="next-steps"></a>Nächste Schritte

- Um zu verstehen, wie Parameter in der Vorlage definiert werden, lesen Sie [Parameter in Azure Resource Manager-Vorlagen](template-parameters.md).
- Weitere Informationen zur Verwendung von Werten aus einem Schlüsseltresor finden Sie unter [Verwenden von Azure Key Vault zum Übergeben eines sicheren Parameterwerts während der Bereitstellung](key-vault-parameter.md).
- Weitere Informationen zu Parametern finden Sie unter [Parameter in Azure Resource Manager-Vorlagen](template-parameters.md).
