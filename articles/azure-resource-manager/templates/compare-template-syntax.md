---
title: Vergleichen der Syntax für Azure Resource Manager-Vorlagen in JSON und Bicep
description: Hier werden mit JSON und Bicep entwickelte Azure Resource Manager-Vorlagen verglichen, und es wird die Konvertierung zwischen den Sprachen veranschaulicht.
ms.topic: conceptual
ms.date: 03/12/2021
ms.openlocfilehash: 225e52e9534a77a01502b762f043a4f34df19caa
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "103461793"
---
# <a name="comparing-json-and-bicep-for-templates"></a>Vergleichen von JSON- und Bicep für Vorlagen

In diesem Artikel wird die Bicep-Syntax mit der JSON-Syntax für Azure Resource Manager-Vorlagen (ARM-Vorlagen) verglichen. In den meisten Fällen bietet Bicep eine Syntax, die weniger ausführlich ist als die entsprechende in JSON.

Wenn Sie mit JSON zum Entwickeln von ARM-Vorlagen vertraut sind, erfahren Sie in der folgenden Tabelle mehr über die entsprechende Syntax für Bicep.

## <a name="expressions"></a>Ausdrücke

So erstellen Sie einen Ausdruck

```bicep
func()
```

```json
"[func()]"
```

## <a name="parameters"></a>Parameter

So deklarieren Sie einen Parameter mit einem Standardwert

```bicep
param demoParam string = 'Contoso'
```

```json
"parameters": {
  "demoParam": {
    "type": "string",
    "defaultValue": "Contoso"
  }
}
```

So rufen Sie einen Parameterwert ab

```bicep
demoParam
```

```json
[parameters('demoParam'))]
```

## <a name="variables"></a>Variablen

So deklarieren Sie eine Variable

```bicep
var demoVar = 'example value'
```

```json
"variables": {
  "demoVar": "example value"
},
```

So rufen Sie den Wert einer Variablen ab

```bicep
demoVar
```

```json
[variables('demoVar'))]
```

## <a name="strings"></a>Zeichenfolgen

So verketten Sie Zeichenfolgen

```bicep
'${namePrefix}-vm'
```

```json
[concat(parameters('namePrefix'), '-vm')]
```

## <a name="logical-operators"></a>Logische Operatoren

So geben Sie logisches **UND** zurück

```bicep
isMonday && isNovember
```

```json
[and(parameter('isMonday'), parameter('isNovember'))]
```

So legen Sie einen Wert bedingt fest

```bicep
isMonday ? 'valueIfTrue' : 'valueIfFalse'
```

```json
[if(parameters('isMonday'), 'valueIfTrue', 'valueIfFalse')]
```

## <a name="deployment-scope"></a>Bereitstellungsumfang

So legen Sie den Zielbereich der Bereitstellung fest

```bicep
targetScope = 'subscription'
```

```json
"$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#"
```

## <a name="resources"></a>Ressourcen

So deklarieren Sie eine Ressource

```bicep
resource vm 'Microsoft.Compute/virtualMachines@2020-06-01' = {
  ...
}
```

```json
"resources": [ 
  { 
    "type": "Microsoft.Compute/virtualMachines", 
    "apiVersion": "2020-06-01", 
    ... 
  } 
]
```

So stellen Sie bedingt eine Ressource bereit

```bicep
resource vm 'Microsoft.Compute/virtualMachines@2020-06-01' = if(deployVM) {
  ...
}
```

```json
"resources": [ 
  {
    "condition": "[parameters('deployVM')]",
    "type": "Microsoft.Compute/virtualMachines", 
    "apiVersion": "2020-06-01", 
    ... 
  } 
]
```

So legen Sie eine Ressourceneigenschaft fest

```bicep
sku: '2016-Datacenter'
```

```json
"sku": "2016-Datacenter",
```

So rufen Sie die ID der Ressource in der Vorlage ab

```bicep
nic1.id
```

```json
[resourceId('Microsoft.Network/networkInterfaces', variables('nic1Name'))]
```

## <a name="loops"></a>Schleifen

So durchlaufen Sie Elemente in einem Array oder einer Anzahl

```bicep
[for storageName in storageAccounts: {
  ...
}]
```

```json
"copy": {
  "name": "storagecopy",
  "count": "[length(parameters('storageAccounts'))]"
},
...
```

## <a name="resource-dependencies"></a>Ressourcenabhängigkeiten

So legen Sie Abhängigkeiten zwischen Ressourcen fest

Setzen Sie in Bicep entweder auf die automatische Erkennung von Abhängigkeiten, oder legen Sie Abhängigkeit manuell fest.

```bicep
dependsOn: [ stg ]
```

```json
"dependsOn": ["[resourceId('Microsoft.Storage/storageAccounts', 'parameters('storageAccountName'))]"]
```

## <a name="reference-resources"></a>Referenzressourcen

So rufen Sie eine Eigenschaft aus einer Ressource in der Vorlage ab

```bicep
diagsAccount.properties.primaryEndpoints.blob
```

```json
[reference(resourceId('Microsoft.Storage/storageAccounts', variables('diagStorageAccountName'))).primaryEndpoints.blob]
```

So rufen Sie eine Eigenschaft einer vorhandenen Ressource ab, die nicht in der Vorlage bereitgestellt ist

```bicep
resource stg 'Microsoft.Storage/storageAccounts@2019-06-01' existing = {
  name: storageAccountName
}

// use later in template as often as needed
stg.properties.primaryEndpoints.blob
```

```json
// required every time the property is needed
"[reference(resourceId('Microsoft.Storage/storageAccounts/', parameters('storageAccountName')), '2019-06-01').primaryEndpoints.blob]"
```

## <a name="outputs"></a>Ausgaben

So geben Sie eine Eigenschaft aus einer Ressource in der Vorlage aus

```bicep
output hostname string = publicIP.properties.dnsSettings.fqdn
```

```json
"outputs": {
  "hostname": {
    "type": "string",
    "value": "[reference(resourceId('Microsoft.Network/publicIPAddresses', variables('publicIPAddressName'))).dnsSettings.fqdn]"
  },
}
```

## <a name="code-reuse"></a>Wiederverwendung von Code

So teilen Sie eine Lösung in mehrere Dateien auf

* Verwenden Sie für Bicep [Module](bicep-tutorial-add-modules.md).
* Verwenden Sie für JSON [verknüpfte Vorlagen](linked-templates.md).

## <a name="recommendations"></a>Empfehlungen

* Vermeiden Sie nach Möglichkeit die Verwendung der Funktionen [reference](template-functions-resource.md#reference) und [resourceId](template-functions-resource.md#resourceid) in Ihrer Bicep-Datei. Wenn Sie in derselben Bicep-Bereitstellung auf eine Ressource verweisen, verwenden Sie stattdessen den Ressourcenbezeichner. Wenn Sie z. B. eine Ressource in Ihrer Bicep-Datei mit `stg` als Ressourcenbezeichner bereitgestellt haben, verwenden Sie eine Syntax wie `stg.id` oder `stg.properties.primaryEndpoints.blob`, um Eigenschaftswerte abzurufen. Wenn Sie den Ressourcenbezeichner verwenden, erstellen Sie eine implizite Abhängigkeit zwischen Ressourcen. Sie müssen die Abhängigkeit nicht explizit mit der dependsOn-Eigenschaft festlegen.
* Wenn die Ressource nicht in der Bicep-Datei bereitgestellt ist, können Sie mit dem Schlüsselwort **existing** dennoch einen symbolischen Verweis auf die Ressource abrufen.
* Verwenden Sie konsistente Schreibweisen für Bezeichner. Wenn Sie nicht sicher sind, welche Schreibweise Sie verwenden sollen, versuchen Sie es mit gemischter Groß-/Kleinschreibung (CamelCase). Beispiel: `param myCamelCasedParameter string`.
* Fügen Sie einem Parameter nur dann eine Beschreibung hinzu, wenn sie dem Benutzer wichtige Informationen vermittelt. Sie können für einige Informationen `//`-Kommentare verwenden.

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zu Bicep finden Sie im [Tutorial zu Bicep](./bicep-tutorial-create-first-bicep.md).
* Informationen zum Umstellen von Vorlagen zwischen den Sprachen finden Sie unter [Umstellen von ARM-Vorlagen zwischen JSON und BICEP](bicep-decompile.md).
