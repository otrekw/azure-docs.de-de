---
title: Variablen in Vorlagen
description: Beschreibt, wie Variablen in einer Azure Resource Manager-Vorlage (ARM-Vorlage) und einer Bicep-Datei definiert werden.
ms.topic: conceptual
ms.date: 02/12/2021
ms.openlocfilehash: cafd42112e5d296cb73f88e292a66ca2203f3810
ms.sourcegitcommit: d4734bc680ea221ea80fdea67859d6d32241aefc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/14/2021
ms.locfileid: "100364459"
---
# <a name="variables-in-arm-templates"></a>Variablen in ARM-Vorlagen

Dieser Artikel beschreibt, wie Sie Variablen in Ihrer Azure Resource Manager-Vorlage (ARM-Vorlage) oder Bicep-Datei definieren und verwenden. Mit Variablen vereinfachen Sie Ihre Vorlage. Anstatt komplizierte Ausdrücke innerhalb der Vorlage zu wiederholen, definieren Sie eine Variable, die den komplizierten Ausdruck enthält. Anschließend verwenden Sie diese Variable wie gewünscht in der gesamten Vorlage.

Resource Manager löst Variablen vor Beginn der Bereitstellungsvorgänge auf. Jedes Vorkommen der Variablen in der Vorlage wird von Resource Manager durch den aufgelösten Wert ersetzt.

[!INCLUDE [Bicep preview](../../../includes/resource-manager-bicep-preview.md)]

## <a name="define-variable"></a>Definieren einer Variablen

Wenn Sie eine Variable definieren, geben Sie keinen [Datentyp](template-syntax.md#data-types) für die Variable an. Sie geben stattdessen einen Wert oder einen Vorlagenausdruck an. Der Variablentyp wird vom aufgelösten Wert abgeleitet. Im folgenden Beispiel wird eine Variable auf eine Zeichenfolge festgelegt.

# <a name="json"></a>[JSON](#tab/json)

```json
"variables": {
  "stringVar": "example value"
},
```

# <a name="bicep"></a>[Bicep](#tab/bicep)

```bicep
var stringVar = 'example value'
```

---

Sie können beim Erstellen der Variablen den Wert eines Parameters oder einer anderen Variablen verwenden.

# <a name="json"></a>[JSON](#tab/json)

```json
"parameters": {
  "inputValue": {
    "defaultValue": "deployment parameter",
    "type": "string"
  }
},
"variables": {
  "stringVar": "myVariable",
  "concatToVar": "[concat(variables('stringVar'), '-addtovar') ]",
  "concatToParam": "[concat(parameters('inputValue'), '-addtoparam')]"
}
```

# <a name="bicep"></a>[Bicep](#tab/bicep)

```bicep
param inputValue string = 'deployment parameter'

var stringVar = 'myVariable'
var concatToVar =  '${stringVar}-addtovar'
var concatToParam = '${inputValue}-addtoparam'
```

---

Sie können [Vorlagenfunktionen](template-functions.md) verwenden, um den Variablenwert zu erstellen.

Bei JSON-Vorlagen können Sie in der Variablendeklaration nicht die [reference](template-functions-resource.md#reference)-Funktion oder eine der [list](template-functions-resource.md#list)-Funktionen verwenden. Diese Funktionen rufen den Laufzeitstatus einer Ressource ab und können nicht vor der Bereitstellung ausgeführt werden, wenn Variablen aufgelöst werden.

Die Funktionen „reference“ und „list“ sind beim Deklarieren einer Variable in einer Bicep-Datei gültig.

Im folgenden Beispiel wird ein Zeichenfolgenwert für ein Speicherkonto erstellt. Es werden mehrere Vorlagenfunktionen verwendet, um einen Parameterwert abzurufen und zu einer eindeutigen Zeichenfolge zu verketten.

# <a name="json"></a>[JSON](#tab/json)

```json
"variables": {
  "storageName": "[concat(toLower(parameters('storageNamePrefix')), uniqueString(resourceGroup().id))]"
},
```

# <a name="bicep"></a>[Bicep](#tab/bicep)

```bicep
var storageName = '${toLower(storageNamePrefix)}${uniqueString(resourceGroup().id)}'
```

---

## <a name="use-variable"></a>Verwenden einer Variablen

Im folgenden Beispiel wird gezeigt, wie die Variable für eine Ressourceneigenschaft verwendet wird.

# <a name="json"></a>[JSON](#tab/json)

In einer JSON-Vorlage verweisen Sie mithilfe der [variables](template-functions-deployment.md#variables)-Funktion auf den Wert für die Variable.

```json
"resources": [
  {
    "type": "Microsoft.Storage/storageAccounts",
    "name": "[variables('storageName')]",
    ...
  }
]
```

# <a name="bicep"></a>[Bicep](#tab/bicep)

In einer Bicep-Datei verweisen Sie über die Angabe des Variablennamens auf den Wert für die Variable.

```bicep
resource demoAccount 'Microsoft.Storage/storageAccounts@2019-06-01' = {
  name: storageName
```

---

## <a name="example-template"></a>Beispielvorlage

Von der folgenden Vorlage werden keine Ressourcen bereitgestellt. Sie zeigt einige Möglichkeiten, Variablen unterschiedlicher Typen zu deklarieren.

# <a name="json"></a>[JSON](#tab/json)

:::code language="json" source="~/resourcemanager-templates/azure-resource-manager/variables.json":::

# <a name="bicep"></a>[Bicep](#tab/bicep)

Bicep unterstützt derzeit keine Schleifen.

:::code language="bicep" source="~/resourcemanager-templates/azure-resource-manager/variables.bicep":::

---

## <a name="configuration-variables"></a>Konfigurationsvariablen

Sie können Variablen definieren, die zugehörige Werte zum Konfigurieren einer Umgebung enthalten. Die Variable wird als ein Objekt mit den Werten definiert. Das folgende Beispiel zeigt ein Objekt, das Werte für zwei Umgebungen enthält: **test** und **prod**. Sie übergeben einen dieser Werte während der Bereitstellung.

# <a name="json"></a>[JSON](#tab/json)

:::code language="json" source="~/resourcemanager-templates/azure-resource-manager/variablesconfigurations.json":::

# <a name="bicep"></a>[Bicep](#tab/bicep)

:::code language="json" source="~/resourcemanager-templates/azure-resource-manager/variablesconfigurations.bicep":::

---

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zu den verfügbaren Eigenschaften für Variablen finden Sie unter [Verstehen der Struktur und Syntax von ARM-Vorlagen](template-syntax.md).
* Empfehlungen zum Erstellen von Variablen finden Sie unter [Bewährte Methoden: Variablen](template-best-practices.md#variables).
* Eine Beispielvorlage, mit der Sicherheitsregeln einer Netzwerksicherheitsgruppe zugewiesen werden, finden Sie unter [Netzwerksicherheitsregeln](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/multiplesecurityrules.json) und [Parameterdatei](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/multiplesecurityrules.parameters.json).
