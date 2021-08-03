---
title: Variablen in Vorlagen
description: Beschreibt, wie Variablen in einer Azure Resource Manager-Vorlage (ARM-Vorlage) definiert werden.
ms.topic: conceptual
ms.date: 05/14/2021
ms.openlocfilehash: 02b80e4ee9cc6392353d6229d6437feb5a1a41ec
ms.sourcegitcommit: c072eefdba1fc1f582005cdd549218863d1e149e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/10/2021
ms.locfileid: "111959310"
---
# <a name="variables-in-arm-templates"></a>Variablen in ARM-Vorlagen

Dieser Artikel beschreibt, wie Sie Variablen in Ihrer Azure Resource Manager-Vorlage (ARM-Vorlage) definieren und verwenden. Mit Variablen vereinfachen Sie Ihre Vorlage. Anstatt komplizierte Ausdrücke innerhalb der Vorlage zu wiederholen, definieren Sie eine Variable, die den komplizierten Ausdruck enthält. Anschließend verwenden Sie diese Variable wie gewünscht in der gesamten Vorlage.

Resource Manager löst Variablen vor Beginn der Bereitstellungsvorgänge auf. Jedes Vorkommen der Variablen in der Vorlage wird von Resource Manager durch den aufgelösten Wert ersetzt.

## <a name="define-variable"></a>Definieren einer Variablen

Wenn Sie eine Variable definieren, geben Sie keinen [Datentyp](data-types.md) für die Variable an. Sie geben stattdessen einen Wert oder einen Vorlagenausdruck an. Der Variablentyp wird vom aufgelösten Wert abgeleitet. Im folgenden Beispiel wird eine Variable auf eine Zeichenfolge festgelegt.

```json
"variables": {
  "stringVar": "example value"
},
```

Sie können Sie den Wert aus einem Parameter oder einer anderen Variablen verwenden, um die Variable zu erstellen.

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

Sie können [Vorlagenfunktionen](template-functions.md) verwenden, um den Variablenwert zu erstellen.

Im folgenden Beispiel wird ein Zeichenfolgenwert für ein Speicherkonto erstellt. Es werden mehrere Vorlagenfunktionen verwendet, um einen Parameterwert abzurufen und zu einer eindeutigen Zeichenfolge zu verketten.

```json
"variables": {
  "storageName": "[concat(toLower(parameters('storageNamePrefix')), uniqueString(resourceGroup().id))]"
},
```

In der Variablendeklaration kann weder die [reference](template-functions-resource.md#reference)-Funktion noch eine der [list](template-functions-resource.md#list)-Funktionen verwendet werden. Diese Funktionen rufen den Laufzeitstatus einer Ressource ab und können nicht vor der Bereitstellung ausgeführt werden, wenn Variablen aufgelöst werden.

## <a name="use-variable"></a>Verwenden einer Variablen

Im folgenden Beispiel wird gezeigt, wie die Variable für eine Ressourceneigenschaft verwendet wird.

Verwenden Sie die [variables](template-functions-deployment.md#variables)-Funktion, um auf den Wert für die Variable zu verweisen.

```json
"variables": {
  "storageName": "[concat(toLower(parameters('storageNamePrefix')), uniqueString(resourceGroup().id))]"
},
"resources": [
  {
    "type": "Microsoft.Storage/storageAccounts",
    "name": "[variables('storageName')]",
    ...
  }
]
```

## <a name="example-template"></a>Beispielvorlage

Von der folgenden Vorlage werden keine Ressourcen bereitgestellt. Sie zeigt einige Möglichkeiten, Variablen unterschiedlicher Typen zu deklarieren.

:::code language="json" source="~/resourcemanager-templates/azure-resource-manager/variables.json":::

## <a name="configuration-variables"></a>Konfigurationsvariablen

Sie können Variablen definieren, die zugehörige Werte zum Konfigurieren einer Umgebung enthalten. Die Variable wird als ein Objekt mit den Werten definiert. Das folgende Beispiel zeigt ein Objekt, das Werte für zwei Umgebungen enthält: **test** und **prod**. Sie übergeben einen dieser Werte während der Bereitstellung.

:::code language="json" source="~/resourcemanager-templates/azure-resource-manager/variablesconfigurations.json":::

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zu den verfügbaren Eigenschaften für Variablen finden Sie unter [Verstehen der Struktur und Syntax von ARM-Vorlagen](./syntax.md).
* Empfehlungen zum Erstellen von Variablen finden Sie unter [Bewährte Methoden: Variablen](./best-practices.md#variables).
* Eine Beispielvorlage, mit der Sicherheitsregeln einer Netzwerksicherheitsgruppe zugewiesen werden, finden Sie unter [Netzwerksicherheitsregeln](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/multiplesecurityrules.json) und [Parameterdatei](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/multiplesecurityrules.parameters.json).