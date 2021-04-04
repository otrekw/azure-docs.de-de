---
title: Vorlagensyntax und -ausdrücke
description: Beschreibt die deklarative JSON-Syntax für Azure Resource Manager-Vorlagen (ARM-Vorlagen).
ms.topic: conceptual
ms.date: 03/17/2020
ms.openlocfilehash: 44a386ed849771dfba717c8d1414e64422d0c7bd
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "97797042"
---
# <a name="syntax-and-expressions-in-arm-templates"></a>Syntax und Ausdrücke in ARM-Vorlagen

Die grundlegende Syntax der Azure Resource Manager-Vorlage (ARM-Vorlage) ist in JSON-Format (JavaScript Object Notation). Allerdings können Sie Ausdrücke verwenden, um die JSON-Werte zu erweitern, die in der Vorlage zur Verfügung stehen.  Ausdrücke beginnen und enden mit Klammern, entsprechend `[` und `]`. Der Wert des Ausdrucks wird bei der Bereitstellung der Vorlage ausgewertet. Ein Ausdruck kann eine Zeichenfolge, eine ganze Zahl, einen booleschen Wert, ein Array oder ein Objekt zurückgeben.

Ein Vorlagenausdruck darf aus maximal 24.576 Zeichen bestehen.

## <a name="use-functions"></a>Verwenden von Funktionen

Azure Resource Manager bietet [Funktionen](template-functions.md), die Sie in einer Vorlage verwenden können. Das folgende Beispiel zeigt einen Ausdruck, der eine Funktion im Standardwert eines Parameters verwendet:

```json
"parameters": {
  "location": {
    "type": "string",
    "defaultValue": "[resourceGroup().location]"
  }
},
```

Innerhalb des Ausdrucks ruft die Syntax `resourceGroup()` eine der Funktionen auf, die Resource Manager für die Verwendung innerhalb einer Vorlage bereitstellt. Hier ist dies die Funktion [resourceGroup](template-functions-resource.md#resourcegroup). Genau wie in JavaScript haben Funktionsaufrufe das Format `functionName(arg1,arg2,arg3)`. Die Syntax `.location` ruft eine Eigenschaft des Objekts ab, das von dieser Funktion zurückgegeben wird.

Bei Vorlagenfunktionen und ihren Parametern wird Groß-und Kleinschreibung nicht unterschieden. Beispielsweise löst Resource Manager `variables('var1')` und `VARIABLES('VAR1')` identisch auf. Bei der Auswertung bleibt die Groß-/Kleinschreibung erhalten, sofern diese nicht ausdrücklich durch die Funktion geändert wird (z. B. mit `toUpper` oder `toLower`). Für spezielle Ressourcentypen gelten möglicherweise Vorgaben zur Groß-/Kleinschreibung, die sich davon unterscheiden, wie Funktionen ausgewertet werden.

Verwenden Sie einfache Anführungszeichen, um einen Zeichenfolgenwert als Parameter an eine Funktion zu übergeben.

```json
"name": "[concat('storage', uniqueString(resourceGroup().id))]"
```

Die meisten Funktionen funktionieren identisch, unabhängig davon, ob sie in einer Ressourcengruppe, einem Abonnement, einer Verwaltungsgruppe oder einem Mandanten bereitgestellt werden. Die folgenden Funktionen haben Einschränkungen auf der Grundlage des Bereichs:

* [resourceGroup](template-functions-resource.md#resourcegroup): Kann nur in Bereitstellungen in einer Ressourcengruppe verwendet werden.
* [resourceId](template-functions-resource.md#resourceid): Kann in jedem Bereich verwendet werden, aber die gültigen Parameter ändern sich je nach Bereich.
* [subscription](template-functions-resource.md#subscription): Kann nur in Bereitstellungen in einer Ressourcengruppe oder einem Abonnement verwendet werden.

## <a name="escape-characters"></a>Escape-Zeichen

Wenn eine Literalzeichenfolge mit einer öffnenden eckigen Klammer `[` beginnen und einer schließenden eckigen Klammer `]` enden, aber nicht als Ausdruck interpretiert werden soll, fügen Sie am Anfang der Zeichenfolge eine zusätzliche eckige Klammer ein: `[[`. Beispielsweise wird folgende Variable:

```json
"demoVar1": "[[test value]"
```

in `[test value]` aufgelöst.

Wenn die Literalzeichenfolge jedoch nicht mit einer eckigen Klammer endet, dürfen Sie die erste eckige Klammer nicht mit einem Escapezeichen versehen. Beispielsweise wird folgende Variable:

```json
"demoVar2": "[test] value"
```

in `[test] value` aufgelöst.

Verwenden Sie den umgekehrten Schrägstrich, um doppelte Anführungszeichen in einem Ausdruck mit einem Escapezeichen zu versehen, wie beim Hinzufügen eines JSON-Objekts.

```json
"tags": {
    "CostCenter": "{\"Dept\":\"Finance\",\"Environment\":\"Production\"}"
},
```

Beim Übergeben von Parameterwerten hängt die Verwendung von Escapezeichen davon ab, wo der Parameterwert angegeben wird. Wenn Sie in der Vorlage einen Standardwert festlegen, benötigen Sie die zusätzliche linke eckige Klammer.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "demoParam1":{
            "type": "string",
            "defaultValue": "[[test value]"
        }
    },
    "resources": [],
    "outputs": {
        "exampleOutput": {
            "type": "string",
            "value": "[parameters('demoParam1')]"
        }
    }
}
```

Wenn Sie den Standardwert verwenden, gibt die Vorlage `[test value]` zurück.

Wenn Sie jedoch einen Parameterwert über die Befehlszeile übergeben, werden die Zeichen buchstäblich interpretiert. Das Bereitstellen der vorherigen Vorlage mit:

```azurepowershell
New-AzResourceGroupDeployment -ResourceGroupName demoGroup -TemplateFile azuredeploy.json -demoParam1 "[[test value]"
```

Gibt `[[test value]` zurück. Verwenden Sie stattdessen:

```azurepowershell
New-AzResourceGroupDeployment -ResourceGroupName demoGroup -TemplateFile azuredeploy.json -demoParam1 "[test value]"
```

Die gleiche Formatierung gilt auch, wenn Werte aus einer Parameterdatei übergeben werden. Die Zeichen werden buchstäblich interpretiert. Bei Verwendung mit der vorherigen Vorlage gibt die folgende Parameterdatei `[test value]` zurück:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "demoParam1": {
            "value": "[test value]"
        }
   }
}
```

## <a name="null-values"></a>NULL-Werte

Zur Festlegung einer Eigenschaft auf NULL, können Sie `null` oder `[json('null')]` verwenden. Die [json-Funktion](template-functions-object.md#json) gibt ein leeres Objekt zurück, wenn Sie `null` als Parameter angeben. In beiden Fällen wird dies von Resource Manager-Vorlagen so behandelt, als ob die Eigenschaft nicht vorhanden ist.

```json
"stringValue": null,
"objectValue": "[json('null')]"
```

## <a name="next-steps"></a>Nächste Schritte

* Eine vollständige Liste der Funktionen in einer Vorlage finden Sie unter [Funktionen von ARM-Vorlagen](template-functions.md).
* Weitere Informationen zu Vorlagendateien finden Sie unter [Verstehen der Struktur und Syntax von ARM-Vorlagen](template-syntax.md).
