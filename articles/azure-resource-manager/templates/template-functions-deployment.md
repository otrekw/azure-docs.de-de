---
title: Vorlagenfunktionen – Bereitstellung
description: Hier werden die Funktionen beschrieben, die in einer Azure Resource Manager-Vorlage zum Abrufen von Informationen zur Bereitstellung verwendet werden können.
ms.topic: conceptual
ms.date: 04/27/2020
ms.openlocfilehash: a52b4eae9df4ad3fdf9e481ee0a40aac48f6665b
ms.sourcegitcommit: 67bddb15f90fb7e845ca739d16ad568cbc368c06
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2020
ms.locfileid: "82203793"
---
# <a name="deployment-functions-for-arm-templates"></a>Bereitstellungsfunktionen für ARM-Vorlagen

Resource Manager stellt die folgenden Funktionen zum Abrufen von Werten im Zusammenhang mit der aktuellen Bereitstellung Ihrer ARM-Vorlage (Azure Resource Manager) bereit:

* [deployment](#deployment)
* [Umgebung](#environment)
* [parameters](#parameters)
* [variables](#variables)

Informationen zum Abrufen von Werten aus Ressourcen, Ressourcengruppen oder Abonnements finden Sie unter [Ressourcenfunktionen](template-functions-resource.md).

## <a name="deployment"></a>deployment

`deployment()`

Gibt Informationen zum aktuellen Bereitstellungsvorgang zurück.

### <a name="return-value"></a>Rückgabewert

Diese Funktion gibt das Objekt zurück, das während der Bereitstellung übergeben wird. Die Eigenschaften in dem zurückgegebenen Objekt unterscheiden sich abhängig davon, wo Sie sich befinden:

* Bereitstellen einer Vorlage, bei der es sich um eine lokale Datei handelt, oder Bereitstellen einer Vorlage, bei der es sich um eine Remotedatei handelt, auf die über einen URI zugegriffen wird.
* Bereitstellen in einer Ressourcengruppe oder Bereitstellen in einem der anderen Bereiche ([Azure-Abonnement](deploy-to-subscription.md), [Verwaltungsgruppe](deploy-to-management-group.md) oder [Mandant](deploy-to-tenant.md)).

Beim Bereitstellen einer lokalen Vorlage in einer Ressourcengruppe gibt die Funktion das folgende Format zurück:

```json
{
    "name": "",
    "properties": {
        "template": {
            "$schema": "",
            "contentVersion": "",
            "parameters": {},
            "variables": {},
            "resources": [
            ],
            "outputs": {}
        },
        "templateHash": "",
        "parameters": {},
        "mode": "",
        "provisioningState": ""
    }
}
```

Beim Bereitstellen einer Remotevorlage in einer Ressourcengruppe gibt die Funktion das folgende Format zurück:

```json
{
    "name": "",
    "properties": {
        "templateLink": {
            "uri": ""
        },
        "template": {
            "$schema": "",
            "contentVersion": "",
            "parameters": {},
            "variables": {},
            "resources": [],
            "outputs": {}
        },
        "templateHash": "",
        "parameters": {},
        "mode": "",
        "provisioningState": ""
    }
}
```

Wenn Sie in einem Azure-Abonnement, einer Verwaltungsgruppe oder einem Mandanten bereitstellen, enthält das Rückgabeobjekt eine `location`-Eigenschaft. Die location-Eigenschaft ist beim Bereitstellen einer lokalen Vorlage oder einer externen Vorlage enthalten. Das Format lautet:

```json
{
    "name": "",
    "location": "",
    "properties": {
        "template": {
            "$schema": "",
            "contentVersion": "",
            "resources": [],
            "outputs": {}
        },
        "templateHash": "",
        "parameters": {},
        "mode": "",
        "provisioningState": ""
    }
}
```

### <a name="remarks"></a>Bemerkungen

Sie können „deployment()“ verwenden, um basierend auf dem URI der übergeordneten Vorlage eine Verknüpfung mit einer anderen Vorlage zu erstellen.

```json
"variables": {  
    "sharedTemplateUrl": "[uri(deployment().properties.templateLink.uri, 'shared-resources.json')]"  
}
```  

Wenn Sie eine Vorlage aus dem Bereitstellungsverlauf im Portal erneut bereitstellen, wird die Vorlage als lokale Datei bereitgestellt. Die Eigenschaft `templateLink` wird in der Bereitstellungsfunktion nicht zurückgegeben. Falls Ihre Vorlage `templateLink` benötigt, um eine Verknüpfung mit einer anderen Vorlage zu erstellen, führen Sie die erneute Bereitstellung nicht über das Portal durch. Verwenden Sie stattdessen die Befehle, die Sie bei der ursprünglichen Bereitstellung der Vorlage verwendet haben.

### <a name="example"></a>Beispiel

Die folgende [Beispielvorlage](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/deployment.json) gibt das Bereitstellungsobjekt zurück:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "deploymentOutput": {
            "value": "[deployment()]",
            "type" : "object"
        }
    }
}
```

Im vorherigen Beispiel wird das folgende Objekt zurückgegeben:

```json
{
  "name": "deployment",
  "properties": {
    "template": {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "resources": [],
      "outputs": {
        "deploymentOutput": {
          "type": "Object",
          "value": "[deployment()]"
        }
      }
    },
    "templateHash": "13135986259522608210",
    "parameters": {},
    "mode": "Incremental",
    "provisioningState": "Accepted"
  }
}
```

## <a name="environment"></a>Environment

`environment()`

Gibt Informationen zur für die Bereitstellung verwendeten Azure-Umgebung zurück.

### <a name="return-value"></a>Rückgabewert

Diese Funktion gibt Eigenschaften für die aktuelle Azure-Umgebung zurück. Das folgende Beispiel zeigt die Eigenschaften für globales Azure. Sovereign Clouds können etwas andere Eigenschaften zurückgeben.

```json
{
  "name": "",
  "gallery": "",
  "graph": "",
  "portal": "",
  "graphAudience": "",
  "activeDirectoryDataLake": "",
  "batch": "",
  "media": "",
  "sqlManagement": "",
  "vmImageAliasDoc": "",
  "resourceManager": "",
  "authentication": {
    "loginEndpoint": "",
    "audiences": [
      "",
      ""
    ],
    "tenant": "",
    "identityProvider": ""
  },
  "suffixes": {
    "acrLoginServer": "",
    "azureDatalakeAnalyticsCatalogAndJob": "",
    "azureDatalakeStoreFileSystem": "",
    "azureFrontDoorEndpointSuffix": "",
    "keyvaultDns": "",
    "sqlServerHostname": "",
    "storage": ""
  }
}
```

### <a name="example"></a>Beispiel

Die folgende Beispielvorlage gibt das Umgebungsobjekt zurück.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "environmentOutput": {
            "value": "[environment()]",
            "type" : "object"
        }
    }
}
```

Im vorherigen Beispiel wird bei Bereitstellung in globalem Azure das folgende Objekt zurückgegeben:

```json
{
  "name": "AzureCloud",
  "gallery": "https://gallery.azure.com/",
  "graph": "https://graph.windows.net/",
  "portal": "https://portal.azure.com",
  "graphAudience": "https://graph.windows.net/",
  "activeDirectoryDataLake": "https://datalake.azure.net/",
  "batch": "https://batch.core.windows.net/",
  "media": "https://rest.media.azure.net",
  "sqlManagement": "https://management.core.windows.net:8443/",
  "vmImageAliasDoc": "https://raw.githubusercontent.com/Azure/azure-rest-api-specs/master/arm-compute/quickstart-templates/aliases.json",
  "resourceManager": "https://management.azure.com/",
  "authentication": {
    "loginEndpoint": "https://login.windows.net/",
    "audiences": [
      "https://management.core.windows.net/",
      "https://management.azure.com/"
    ],
    "tenant": "common",
    "identityProvider": "AAD"
  },
  "suffixes": {
    "acrLoginServer": ".azurecr.io",
    "azureDatalakeAnalyticsCatalogAndJob": "azuredatalakeanalytics.net",
    "azureDatalakeStoreFileSystem": "azuredatalakestore.net",
    "azureFrontDoorEndpointSuffix": "azurefd.net",
    "keyvaultDns": ".vault.azure.net",
    "sqlServerHostname": ".database.windows.net",
    "storage": "core.windows.net"
  }
}
```

## <a name="parameters"></a>parameters

`parameters(parameterName)`

Gibt einen Parameterwert zurück. Der spezifizierte Parametername muss im Parameterabschnitt der Vorlage definiert werden.

### <a name="parameters"></a>Parameter

| Parameter | Erforderlich | type | BESCHREIBUNG |
|:--- |:--- |:--- |:--- |
| parameterName |Ja |Zeichenfolge |Der Name des zurückzugebenden Parameter. |

### <a name="return-value"></a>Rückgabewert

Der Wert des angegebenen Parameters.

### <a name="remarks"></a>Bemerkungen

In der Regel verwenden Sie Parameter zum Festlegen von Ressourcenwerten. Im folgenden Beispiel wird der Name der Website mit dem Parameterwert festgelegt, der während der Bereitstellung übergeben wird.

```json
"parameters": { 
  "siteName": {
      "type": "string"
  }
},
"resources": [
   {
      "apiVersion": "2016-08-01",
      "name": "[parameters('siteName')]",
      "type": "Microsoft.Web/Sites",
      ...
   }
]
```

### <a name="example"></a>Beispiel

Die folgende [Beispielvorlage](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/parameters.json) zeigt eine vereinfachte Nutzungsweise der Parameterfunktion.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "stringParameter": {
            "type" : "string",
            "defaultValue": "option 1"
        },
        "intParameter": {
            "type": "int",
            "defaultValue": 1
        },
        "objectParameter": {
            "type": "object",
            "defaultValue": {"one": "a", "two": "b"}
        },
        "arrayParameter": {
            "type": "array",
            "defaultValue": [1, 2, 3]
        },
        "crossParameter": {
            "type": "string",
            "defaultValue": "[parameters('stringParameter')]"
        }
    },
    "variables": {},
    "resources": [],
    "outputs": {
        "stringOutput": {
            "value": "[parameters('stringParameter')]",
            "type" : "string"
        },
        "intOutput": {
            "value": "[parameters('intParameter')]",
            "type" : "int"
        },
        "objectOutput": {
            "value": "[parameters('objectParameter')]",
            "type" : "object"
        },
        "arrayOutput": {
            "value": "[parameters('arrayParameter')]",
            "type" : "array"
        },
        "crossOutput": {
            "value": "[parameters('crossParameter')]",
            "type" : "string"
        }
    }
}
```

Die Ausgabe aus dem vorherigen Beispiel mit den Standardwerten lautet:

| Name | type | Wert |
| ---- | ---- | ----- |
| stringOutput | String | option 1 |
| intOutput | Int | 1 |
| objectOutput | Object | {"one": "a", "two": "b"} |
| arrayOutput | Array | [1, 2, 3] |
| crossOutput | String | option 1 |

Weitere Informationen zur Verwendung von Parametern finden Sie unter [Parameter in Azure Resource Manager-Vorlagen](template-parameters.md).

## <a name="variables"></a>variables

`variables(variableName)`

Gibt den Wert der Variablen zurück. Der angegebene Variablenname muss im Variablenabschnitt der Vorlage definiert werden.

### <a name="parameters"></a>Parameter

| Parameter | Erforderlich | type | BESCHREIBUNG |
|:--- |:--- |:--- |:--- |
| variableName |Ja |String |Der Name der zurückzugebenden Variable. |

### <a name="return-value"></a>Rückgabewert

Der Wert der angegebenen Variablen.

### <a name="remarks"></a>Bemerkungen

In der Regel verwenden Sie Variablen, um Ihre Vorlage zu vereinfachen, indem Sie komplexe Werte nur einmal erstellen. Das folgende Beispiel erstellt einen eindeutigen Namen für ein Speicherkonto.

```json
"variables": {
    "storageName": "[concat('storage', uniqueString(resourceGroup().id))]"
},
"resources": [
    {
        "type": "Microsoft.Storage/storageAccounts",
        "name": "[variables('storageName')]",
        ...
    },
    {
        "type": "Microsoft.Compute/virtualMachines",
        "dependsOn": [
            "[variables('storageName')]"
        ],
        ...
    }
],
```

### <a name="example"></a>Beispiel

Die folgende [Beispielvorlage](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/variables.json) gibt unterschiedliche Variablenwerte zurück.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {},
    "variables": {
        "var1": "myVariable",
        "var2": [ 1,2,3,4 ],
        "var3": "[ variables('var1') ]",
        "var4": {
            "property1": "value1",
            "property2": "value2"
          }
    },
    "resources": [],
    "outputs": {
        "exampleOutput1": {
            "value": "[variables('var1')]",
            "type" : "string"
        },
        "exampleOutput2": {
            "value": "[variables('var2')]",
            "type" : "array"
        },
        "exampleOutput3": {
            "value": "[variables('var3')]",
            "type" : "string"
        },
        "exampleOutput4": {
            "value": "[variables('var4')]",
            "type" : "object"
        }
    }
}
```

Die Ausgabe aus dem vorherigen Beispiel mit den Standardwerten lautet:

| Name | type | Wert |
| ---- | ---- | ----- |
| exampleOutput1 | String | myVariable |
| exampleOutput2 | Array | [1, 2, 3, 4] |
| exampleOutput3 | String | myVariable |
| exampleOutput4 |  Object | {"property1": "value1", "property2": "value2"} |

Weitere Informationen zur Verwendung von Variablen finden Sie unter [Variablen in einer Azure Resource Manager-Vorlage](template-variables.md).

## <a name="next-steps"></a>Nächste Schritte

* Eine Beschreibung der Abschnitte in einer Azure Resource Manager-Vorlage finden Sie unter [Grundlegendes zur Struktur und Syntax von ARM-Vorlagen](template-syntax.md).
