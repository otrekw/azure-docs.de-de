---
title: Erstellen von Benutzeroberflächendefinitions-Sammlungsfunktionen
description: Beschreibt die Funktionen, die beim Arbeiten mit Sammlungen wie Arrays oder Objekten verwendet werden können.
author: tfitzmac
ms.topic: conceptual
ms.date: 07/13/2020
ms.author: tomfitz
ms.openlocfilehash: 2a075c5c99f457681cd49e75014487bf9cca263c
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "87094446"
---
# <a name="createuidefinition-collection-functions"></a>CreateUiDefinition-Sammlungsfunktionen

Diese Funktionen können mit Auflistungen wie JSON-Zeichenfolgen, Arrays und Objekten verwendet werden.

## <a name="contains"></a>contains

Gibt `true` zurück, wenn eine Zeichenfolge die angegebene Teilzeichenfolge enthält, ein Array den angegebenen Wert enthält oder ein Objekt den angegebenen Schlüssel enthält.

### <a name="example-string-contains"></a>Beispiel: Zeichenfolge enthält

Das folgende Beispiel gibt `true`zurück:

```json
"[contains('webapp', 'web')]"
```

### <a name="example-array-contains"></a>Beispiel: Array enthält

Angenommen `element1` gibt `[1, 2, 3]` zurück. Das folgende Beispiel gibt `false`zurück:

```json
"[contains(steps('demoStep').element1, 4)]"
```

### <a name="example-object-contains"></a>Beispiel: Objekt enthält

Angenommen `element1` gibt Folgendes zurück:

```json
{
  "key1": "Linux",
  "key2": "Windows"
}
```

Das folgende Beispiel gibt `true`zurück:

```json
"[contains(steps('demoStep').element1, 'key1')]"
```

## <a name="empty"></a>empty

Gibt `true` zurück, wenn die Zeichenfolge, das Array oder das Objekt null oder leer ist.

### <a name="example-string-empty"></a>Beispiel: Zeichenfolge leer

Das folgende Beispiel gibt `true`zurück:

```json
"[empty('')]"
```

### <a name="example-array-empty"></a>Beispiel: Array leer

Angenommen `element1` gibt `[1, 2, 3]` zurück. Das folgende Beispiel gibt `false`zurück:

```json
"[empty(steps('demoStep').element1)]"
```

### <a name="example-object-empty"></a>Beispiel: Objekt leer

Angenommen `element1` gibt Folgendes zurück:

```json
{
  "key1": "Linux",
  "key2": "Windows"
}
```

Das folgende Beispiel gibt `false`zurück:

```json
"[empty(steps('demoStep').element1)]"
```

### <a name="example-null-and-undefined"></a>Beispiel: Null und nicht definiert

Angenommen `element1` ist `null` oder nicht definiert. Das folgende Beispiel gibt `true`zurück:

```json
"[empty(steps('demoStep').element1)]"
```

## <a name="filter"></a>filter

Gibt nach dem Anwenden der Filterlogik, die als Lambdafunktion bereitgestellt wird, ein neues Array zurück. Der erste Parameter ist das Array, das zum Filtern verwendet werden soll. Der zweite Parameter ist die Lambdafunktion, die die Filterlogik angibt.

Im folgenden Beispiel wird das Array `[ { "name": "abc" } ]` zurückgegeben.

```json
"[filter(parse('[{\"name\":\"abc\"},{\"name\":\"xyz\"}]'), (item) => contains(item.name, 'abc'))]"
```

## <a name="first"></a>first

Gibt das erste Zeichen der angegebenen Zeichenfolge, den ersten Wert des angegebenen Arrays oder den ersten Schlüssel und Wert des angegebenen Objekts zurück.

### <a name="example-string-first"></a>Beispiel: Zeichenfolge zuerst

Das folgende Beispiel gibt `"c"`zurück:

```json
"[first('contoso')]"
```

### <a name="example-array-first"></a>Beispiel: Array zuerst

Angenommen `element1` gibt `[1, 2, 3]` zurück. Das folgende Beispiel gibt `1`zurück:

```json
"[first(steps('demoStep').element1)]"
```

#### <a name="example-object-first"></a>Beispiel: Objekt zuerst

Angenommen `element1` gibt Folgendes zurück:

```json
{
  "key1": "Linux",
  "key2": "Windows"
}
```

Das folgende Beispiel gibt `{"key1": "Linux"}`zurück:

```json
"[first(steps('demoStep').element1)]"
```

## <a name="last"></a>last

Gibt das letzte Zeichen der angegebenen Zeichenfolge, den letzten Wert des angegebenen Arrays oder den letzten Schlüssel und Wert des angegebenen Objekts zurück.

### <a name="example-string-last"></a>Beispiel: Zeichenfolge zuletzt

Das folgende Beispiel gibt `"o"`zurück:

```json
"[last('contoso')]"
```

### <a name="example-array-last"></a>Beispiel: Array zuletzt

Angenommen `element1` gibt `[1, 2, 3]` zurück. Das folgende Beispiel gibt `3`zurück:

```json
"[last(steps('demoStep').element1)]"
```

### <a name="example-object-last"></a>Beispiel: Objekt zuletzt

Angenommen `element1` gibt Folgendes zurück:

```json
{
  "key1": "Linux",
  "key2": "Windows"
}
```

Das folgende Beispiel gibt `{"key2": "Windows"}`zurück:

```json
"[last(steps('demoStep').element1)]"
```

## <a name="length"></a>length

Gibt die Anzahl von Zeichen in einer Zeichenfolge, die Anzahl von Werten in einem Array oder die Anzahl von Schlüsseln in einem Objekt zurück.

### <a name="example-string-length"></a>Beispiel: Zeichenfolgenlänge

Das folgende Beispiel gibt `7`zurück:

```json
"[length('Contoso')]"
```

### <a name="example-array-length"></a>Beispiel: Arraylänge

Angenommen `element1` gibt `[1, 2, 3]` zurück. Das folgende Beispiel gibt `3`zurück:

```json
"[length(steps('demoStep').element1)]"
```

### <a name="example-object-length"></a>Beispiel: Objektlänge

Angenommen `element1` gibt Folgendes zurück:

```json
{
  "key1": "Linux",
  "key2": "Windows"
}
```

Das folgende Beispiel gibt `2`zurück:

```json
"[length(steps('demoStep').element1)]"
```

## <a name="map"></a>map

Gibt nach dem Aufrufen einer Lambdafunktion für ein bereitgestelltes Array ein neues Array zurück. Der erste Parameter ist das Array, das für die Lambdafunktion verwendet werden soll. Der zweite Parameter ist die Lambdafunktion.

Im folgenden Beispiel wird ein neues Array mit jedem Wert verdoppelt zurückgegeben. Das Ergebnis ist `[2, 4, 6]`.

```json
"[map(parse('[1, 2, 3]'), (item) => mul(2, item))]"
```

Im folgenden Beispiel wird ein neues Array `["abc", "xyz"]` zurückgegeben.

```json
"[map(parse('[{\"name\":\"abc\"},{\"name\":\"xyz\"}]'), (item) => item.name)]"
```

## <a name="skip"></a>skip

Umgeht eine angegebenen Anzahl von Elementen in einer Auflistung und gibt dann die übrigen Elemente zurück.

### <a name="example-string-skip"></a>Beispiel: Zeichenfolge überspringen

Das folgende Beispiel gibt `"app"`zurück:

```json
"[skip('webapp', 3)]"
```

### <a name="example-array-skip"></a>Beispiel: Array überspringen

Angenommen `element1` gibt `[1, 2, 3]` zurück. Das folgende Beispiel gibt `[3]`zurück:

```json
"[skip(steps('demoStep').element1, 2)]"
```

### <a name="example-object-skip"></a>Beispiel: Objekt überspringen

Angenommen `element1` gibt Folgendes zurück:

```json
{
  "key1": "Linux",
  "key2": "Windows"
}
```
Das folgende Beispiel gibt `{"key2": "Windows"}`zurück:

```json
"[skip(steps('demoStep').element1, 1)]"
```

## <a name="split"></a>split

Gibt ein Array mit Zeichenfolgen zurück, das die Teilzeichenfolgen der Eingabe, getrennt durch das Trennzeichen, enthält.

Im folgenden Beispiel wird das Array `[ "555", "867", "5309" ]` zurückgegeben.

```json
"[split('555-867-5309', '-')]"
```

## <a name="take"></a>take

Gibt eine angegebene Anzahl von zusammenhängenden Zeichen vom Anfang der Zeichenfolge, eine angegebene Anzahl von zusammenhängenden Werten vom Anfang des Arrays oder eine angegebene Anzahl von zusammenhängenden Schlüsseln und Werten vom Anfang des Objekts zurück.

### <a name="example-string-take"></a>Beispiel: Zeichenfolge nehmen

Das folgende Beispiel gibt `"web"`zurück:

```json
"[take('webapp', 3)]"
```

### <a name="example-array-take"></a>Beispiel: Array nehmen

Angenommen `element1` gibt `[1, 2, 3]` zurück. Das folgende Beispiel gibt `[1, 2]`zurück:

```json
"[take(steps('demoStep').element1, 2)]"
```

### <a name="example-object-take"></a>Beispiel: Objekt nehmen

Angenommen `element1` gibt Folgendes zurück:

```json
{
  "key1": "Linux",
  "key2": "Windows"
}
```

Das folgende Beispiel gibt `{"key1": "Linux"}`zurück:

```json
"[take(steps('demoStep').element1, 1)]"
```

## <a name="next-steps"></a>Nächste Schritte

* Eine Einführung in Azure Resource Manager finden Sie unter [Übersicht über Azure Resource Manager](../management/overview.md).
