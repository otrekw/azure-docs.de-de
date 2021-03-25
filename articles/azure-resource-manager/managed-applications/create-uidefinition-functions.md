---
title: Erstellen von Benutzeroberflächen-Definitionsfunktionen
description: Hier werden die Funktionen beschrieben, mit denen Benutzeroberflächendefinitionen für verwaltete Azure-Anwendungen erstellt werden.
author: tfitzmac
ms.topic: conceptual
ms.date: 07/13/2020
ms.author: tomfitz
ms.openlocfilehash: e4255f0d42e28a72ad55d9b7f81d0dc49b2950cb
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "87040994"
---
# <a name="createuidefinition-functions"></a>CreateUiDefinition-Funktionen

Dieser Artikel bietet eine Übersicht der unterstützten Funktionen für CreateUiDefinition.

## <a name="function-syntax"></a>Funktionssyntax

Setzen Sie den Aufruf in eckige Klammern, um eine Funktion zu verwenden. Beispiel:

```json
"[function()]"
```

Auf Zeichenfolgen und andere Funktionen kann als Parameter für eine Funktion verwiesen werden, Zeichenfolgen müssen jedoch in einfache Anführungszeichen gesetzt werden. Beispiel:

```json
"[fn1(fn2(), 'demo text')]"
```

Falls zutreffend, können Sie auf die Eigenschaften der Ausgabe einer Funktion mit dem Punktoperator verweisen. Beispiel:

```json
"[func().prop1]"
```

## <a name="collection-functions"></a>Auflistungsfunktionen

Diese Funktionen können mit Auflistungen wie JSON-Zeichenfolgen, Arrays und Objekten verwendet werden.

* [contains](create-ui-definition-collection-functions.md#contains)
* [empty](create-ui-definition-collection-functions.md#empty)
* [filter](create-ui-definition-collection-functions.md#filter)
* [first](create-ui-definition-collection-functions.md#first)
* [last](create-ui-definition-collection-functions.md#last)
* [length](create-ui-definition-collection-functions.md#length)
* [map](create-ui-definition-collection-functions.md#map)
* [skip](create-ui-definition-collection-functions.md#skip)
* [split](create-ui-definition-collection-functions.md#split)
* [take](create-ui-definition-collection-functions.md#take)

## <a name="conversion-functions"></a>Konvertierungsfunktionen

Diese Funktionen können verwendet werden, um Werte zwischen JSON-Datentypen und Codierungen zu konvertieren.

* [bool](create-ui-definition-conversion-functions.md#bool)
* [decodeBase64](create-ui-definition-conversion-functions.md#decodebase64)
* [decodeUriComponent](create-ui-definition-conversion-functions.md#decodeuricomponent)
* [encodeBase64](create-ui-definition-conversion-functions.md#encodebase64)
* [encodeUriComponent](create-ui-definition-conversion-functions.md#encodeuricomponent)
* [float](create-ui-definition-conversion-functions.md#float)
* [int](create-ui-definition-conversion-functions.md#int)
* [parse](create-ui-definition-conversion-functions.md#parse)
* [string](create-ui-definition-conversion-functions.md#string)

## <a name="date-functions"></a>Datumsfunktionen

* [addHours](create-ui-definition-date-functions.md#addhours)
* [addMinutes](create-ui-definition-date-functions.md#addminutes)
* [addSeconds](create-ui-definition-date-functions.md#addseconds)
* [utcNow](create-ui-definition-date-functions.md#utcnow)

## <a name="logical-functions"></a>Logische Funktionen

Diese Funktionen können in bedingten Ausdrücken verwendet werden. Einige Funktionen unterstützen möglicherweise nicht alle JSON-Datentypen.

* [and](create-ui-definition-logical-functions.md#and)
* [coalesce](create-ui-definition-logical-functions.md#coalesce)
* [equals](create-ui-definition-logical-functions.md#equals)
* [greater](create-ui-definition-logical-functions.md#greater)
* [greaterOrEquals](create-ui-definition-logical-functions.md#greaterorequals)
* [if](create-ui-definition-logical-functions.md#if)
* [less](create-ui-definition-logical-functions.md#less)
* [lessOrEquals](create-ui-definition-logical-functions.md#lessorequals)
* [not](create-ui-definition-logical-functions.md#not)
* [or](create-ui-definition-logical-functions.md#or)

## <a name="math-functions"></a>Mathematische Funktionen

* [add](create-ui-definition-math-functions.md#add)
* [ceil](create-ui-definition-math-functions.md#ceil)
* [div](create-ui-definition-math-functions.md#div)
* [floor](create-ui-definition-math-functions.md#floor)
* [max](create-ui-definition-math-functions.md#max)
* [min](create-ui-definition-math-functions.md#min)
* [mod](create-ui-definition-math-functions.md#mod)
* [mul](create-ui-definition-math-functions.md#mul)
* [rand](create-ui-definition-math-functions.md#rand)
* [range](create-ui-definition-math-functions.md#range)
* [sub](create-ui-definition-math-functions.md#sub)

## <a name="referencing-functions"></a>Verweisfunktionen

* [basics](create-ui-definition-referencing-functions.md#basics)
* [location](create-ui-definition-referencing-functions.md#location)
* [Ressourcengruppe](create-ui-definition-referencing-functions.md#resourcegroup)
* [steps](create-ui-definition-referencing-functions.md#steps)
* [Abonnement](create-ui-definition-referencing-functions.md#subscription)

## <a name="string-functions"></a>Zeichenfolgenfunktionen

* [concat](create-ui-definition-string-functions.md#concat)
* [endsWith](create-ui-definition-string-functions.md#endswith)
* [guid](create-ui-definition-string-functions.md#guid)
* [indexOf](create-ui-definition-string-functions.md#indexof)
* [lastIndexOf](create-ui-definition-string-functions.md#lastindexof)
* [replace](create-ui-definition-string-functions.md#replace)
* [startsWith](create-ui-definition-string-functions.md#startswith)
* [substring](create-ui-definition-string-functions.md#substring)
* [toLower](create-ui-definition-string-functions.md#tolower)
* [toUpper](create-ui-definition-string-functions.md#toupper)

## <a name="next-steps"></a>Nächste Schritte

* Eine Einführung in Azure Resource Manager finden Sie unter [Übersicht über Azure Resource Manager](../management/overview.md).
