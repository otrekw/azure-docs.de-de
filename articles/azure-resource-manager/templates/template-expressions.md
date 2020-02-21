---
title: Vorlagensyntax und -ausdrücke
description: Beschreibt die deklarative JSON-Syntax für Azure Resource Manager-Vorlagen.
ms.topic: conceptual
ms.date: 02/13/2020
ms.openlocfilehash: 7bca3125f80225d2180734f483194a63e39d9cf5
ms.sourcegitcommit: 2823677304c10763c21bcb047df90f86339e476a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/14/2020
ms.locfileid: "77207399"
---
# <a name="syntax-and-expressions-in-azure-resource-manager-templates"></a>Syntax und Ausdrücke in Azure Resource Manager-Vorlagen

Die grundlegende Syntax der Vorlage ist JSON. Allerdings können Sie Ausdrücke verwenden, um die JSON-Werte zu erweitern, die in der Vorlage zur Verfügung stehen.  Ausdrücke beginnen und enden mit Klammern, entsprechend `[` und `]`. Der Wert des Ausdrucks wird bei der Bereitstellung der Vorlage ausgewertet. Ein Ausdruck kann eine Zeichenfolge, eine ganze Zahl, einen booleschen Wert, ein Array oder ein Objekt zurückgeben.

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

Bei Vorlagenfunktionen und ihren Parametern wird Groß-und Kleinschreibung nicht unterschieden. Der Resource Manager löst beispielsweise **variables('var1')** und **VARIABLES('VAR1')** identisch auf. Bei der Auswertung wird die Groß-/Kleinschreibung beibehalten, sofern diese nicht ausdrücklich durch die Funktion geändert wird (z.B. mit „toUpper“ oder „toLower“). Für spezielle Ressourcentypen gelten möglicherweise Vorgaben zur Groß-/Kleinschreibung, die sich davon unterscheiden, wie Funktionen ausgewertet werden.

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

## <a name="null-values"></a>NULL-Werte

Um eine Eigenschaft auf Null festzulegen, können Sie **null** oder **[json('null')]** verwenden. Die [json-Funktion](template-functions-array.md#json) gibt ein leeres Objekt zurück, wenn Sie `null` als Parameter angeben. In beiden Fällen wird dies von Resource Manager-Vorlagen so behandelt, als ob die Eigenschaft nicht vorhanden ist.

```json
"stringValue": null,
"objectValue": "[json('null')]"
```

## <a name="next-steps"></a>Nächste Schritte

* Unter [Funktionen von Azure-Ressourcen-Manager-Vorlagen](template-functions.md)finden Sie eine vollständige Liste der Vorlagenfunktionen.
* Weitere Informationen zu Vorlagendateien finden Sie unter [Verstehen der Struktur und Syntax von Azure Resource Manager-Vorlagen](template-syntax.md).
