---
title: Variablen in Bicep
description: Informationen zum Definieren von Variablen in Bicep
author: mumian
ms.author: jgao
ms.topic: conceptual
ms.date: 06/01/2021
ms.openlocfilehash: 99c7d945f251319bf1b655ecd02730b62e3ed2e4
ms.sourcegitcommit: 7f59e3b79a12395d37d569c250285a15df7a1077
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/02/2021
ms.locfileid: "111026235"
---
# <a name="variables-in-bicep"></a>Variablen in Bicep

Dieser Artikel beschreibt, wie Sie Variablen in Ihrer Bicep-Datei definieren und verwenden. Sie verwenden Variablen, um die Entwicklung Ihrer Bicep-Dateien zu vereinfachen. Anstatt komplizierte Ausdrücke in der Bicep-Datei zu wiederholen, definieren Sie eine Variable, die den komplizierten Ausdruck enthält. Anschließend verwenden Sie diese Variable nach Bedarf in der gesamten Bicep-Datei.

Resource Manager löst Variablen vor Beginn der Bereitstellungsvorgänge auf. Jedes Vorkommen der Variablen in der Bicep-Datei wird von Resource Manager durch den aufgelösten Wert ersetzt.

## <a name="define-variable"></a>Definieren einer Variablen

Wenn Sie eine Variable definieren, geben Sie keinen [Datentyp](data-types.md) für die Variable an. Sie geben stattdessen einen Wert oder einen Vorlagenausdruck an. Der Variablentyp wird vom aufgelösten Wert abgeleitet. Im folgenden Beispiel wird eine Variable auf eine Zeichenfolge festgelegt.

```bicep
var stringVar = 'example value'
```

Sie können beim Erstellen der Variablen den Wert eines Parameters oder einer anderen Variablen verwenden.

```bicep
param inputValue string = 'deployment Parameter'

var stringVar = 'myVariable'
var concatToVar =  '${stringVar}AddToVar'
var concatToParam = '${inputValue}AddToParam'
```

Sie können [Bicep-Funktionen](bicep-functions.md) verwenden, um den Variablenwert zu erstellen. Die Funktionen [reference](bicep-functions-resource.md#reference) und [list](bicep-functions-resource.md#list) sind beim Deklarieren von Variablen gültig.

Im folgenden Beispiel wird ein Zeichenfolgenwert für ein Speicherkonto erstellt. Es werden mehrere Bicep-Funktionen verwendet, um einen Parameterwert abzurufen und zu einer eindeutigen Zeichenfolge zu verketten.

```bicep
var storageName = '${toLower(storageNamePrefix)}${uniqueString(resourceGroup().id)}'
```

## <a name="use-variable"></a>Verwenden einer Variablen

Im folgenden Beispiel wird gezeigt, wie die Variable für eine Ressourceneigenschaft verwendet wird. Durch Angabe des Variablennamens verweisen Sie auf den Wert der Variable: `storageName`.

```bicep
param rgLocation string = resourceGroup().location
param storageNamePrefix string = 'STG'

var storageName = '${toLower(storageNamePrefix)}${uniqueString(resourceGroup().id)}'

resource demoAccount 'Microsoft.Storage/storageAccounts@2021-02-01' = {
  name: storageName
  location: rgLocation
  kind: 'Storage'
  sku: {
    name: 'Standard_LRS'
    tier: 'Standard'
  }
}

output stgOutput string = storageName
```

Da Speicherkontonamen Kleinbuchstaben verwenden müssen, verwendet die Variable `storageName` die Funktion `toLower`, um den Wert `storageNamePrefix` in Kleinbuchstaben zu schreiben. Die Funktion `uniqueString` erstellt einen eindeutigen Wert aus der Ressourcengruppen-ID. Die Werte werden zu einer Zeichenfolge verkettet.

## <a name="example-template"></a>Beispielvorlage

Von der folgenden Vorlage werden keine Ressourcen bereitgestellt. Sie zeigt einige Möglichkeiten, Variablen unterschiedlicher Typen zu deklarieren.

:::code language="bicep" source="~/resourcemanager-templates/azure-resource-manager/variables.bicep":::

## <a name="configuration-variables"></a>Konfigurationsvariablen

Sie können Variablen definieren, die zugehörige Werte zum Konfigurieren einer Umgebung enthalten. Die Variable wird als ein Objekt mit den Werten definiert. Das folgende Beispiel zeigt ein Objekt, das Werte für zwei Umgebungen enthält: **test** und **prod**. Sie übergeben einen dieser Werte während der Bereitstellung.

:::code language="json" source="~/resourcemanager-templates/azure-resource-manager/variablesconfigurations.bicep":::

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu den verfügbaren Eigenschaften für Variablen finden Sie unter [Verstehen der Struktur und Syntax von Bicep-Dateien](file.md).
