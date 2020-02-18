---
title: Vorlagensyntax und -ausdrücke
description: Beschreibt die deklarative JSON-Syntax für Azure Resource Manager-Vorlagen.
ms.topic: conceptual
ms.date: 02/10/2020
ms.openlocfilehash: 42649d4b04b03de32b82335fce68401192de75a3
ms.sourcegitcommit: 7c18afdaf67442eeb537ae3574670541e471463d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/11/2020
ms.locfileid: "77120602"
---
# <a name="syntax-and-expressions-in-azure-resource-manager-templates"></a>Syntax und Ausdrücke in Azure Resource Manager-Vorlagen

Die grundlegende Syntax der Vorlage ist JSON. Allerdings können Sie Ausdrücke verwenden, um die JSON-Werte zu erweitern, die in der Vorlage zur Verfügung stehen.  Ausdrücke beginnen und enden mit Klammern, entsprechend `[` und `]`. Der Wert des Ausdrucks wird bei der Bereitstellung der Vorlage ausgewertet. Ein Ausdruck kann eine Zeichenfolge, eine ganze Zahl, einen booleschen Wert, ein Array oder ein Objekt zurückgeben.

Ein Vorlagenausdruck darf aus maximal 24.576 Zeichen bestehen.

Für Ausdrücke wird „json('null')“ und für Eigenschaften ein Literalwert von „null“ unterstützt. In beiden Fällen wird dies von Resource Manager-Vorlagen so behandelt, als ob die Eigenschaft nicht vorhanden ist.

## <a name="use-functions"></a>Verwenden von Funktionen

Das folgende Beispiel zeigt einen Ausdruck im Standardwert eines Parameters:

```json
"parameters": {
  "location": {
    "type": "string",
    "defaultValue": "[resourceGroup().location]"
  }
},
```

Innerhalb des Ausdrucks ruft die Syntax `resourceGroup()` eine der Funktionen auf, die Resource Manager für die Verwendung innerhalb einer Vorlage bereitstellt. Hier ist dies die Funktion [resourceGroup](template-functions-resource.md#resourcegroup). Genau wie in JavaScript haben Funktionsaufrufe das Format `functionName(arg1,arg2,arg3)`. Die Syntax `.location` ruft eine Eigenschaft des Objekts ab, das von dieser Funktion zurückgegeben wird.

Bei Vorlagenfunktionen und ihren Parametern wird Groß-und Kleinschreibung nicht unterschieden. Der Resource Manager löst beispielsweise **variables('var1')** und **VARIABLES('VAR1')** identisch auf. Bei der Auswertung wird die Groß-/Kleinschreibung beibehalten, sofern diese nicht ausdrücklich durch die Funktion geändert wird (z.B. mit „toUpper“ oder „toLower“). Für spezielle Ressourcentypen gelten möglicherweise Vorgaben zur Groß-/Kleinschreibung, die sich davon unterscheiden, wie Funktionen ausgewertet werden.

Verwenden Sie einfache Anführungszeichen, um einen Zeichenfolgenwert als Parameter an eine Funktion zu übergeben.

```json
"name": "[concat('storage', uniqueString(resourceGroup().id))]"
```

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

## <a name="next-steps"></a>Nächste Schritte

* Unter [Funktionen von Azure-Ressourcen-Manager-Vorlagen](template-functions.md)finden Sie eine vollständige Liste der Vorlagenfunktionen.
* Weitere Informationen zu Vorlagendateien finden Sie unter [Verstehen der Struktur und Syntax von Azure Resource Manager-Vorlagen](template-syntax.md).
