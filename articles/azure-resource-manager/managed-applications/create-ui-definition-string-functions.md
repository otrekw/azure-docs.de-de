---
title: Erstellen von Benutzeroberflächendefinitions-Zeichenfolgenfunktionen
description: Hier werden die Zeichenfolgenfunktionen beschrieben, mit denen Benutzeroberflächendefinitionen für verwaltete Azure-Anwendungen erstellt werden.
author: tfitzmac
ms.topic: conceptual
ms.date: 07/13/2020
ms.author: tomfitz
ms.openlocfilehash: c662948542c36cd93f889ca045ee245c15c7bb11
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2020
ms.locfileid: "87094438"
---
# <a name="createuidefinition-string-functions"></a>CreateUiDefinition-Zeichenfolgenfunktionen

Diese Funktionen können mit JSON-Zeichenfolgen verwendet werden.

## <a name="concat"></a>concat

Verkettet eine oder mehrere Zeichenfolgen.

Wenn der Ausgabewert von `element1` beispielsweise `"Contoso"` lautet, gibt dieses Beispiel die Zeichenfolge `"Demo Contoso app"` zurück:

```json
"[concat('Demo ', steps('step1').element1, ' app')]"
```

## <a name="endswith"></a>endsWith

Bestimmt, ob eine Zeichenfolge mit einem Wert endet.

Im folgenden Beispiel wird „true“ zurückgegeben.

```json
"[endsWith('tuvwxyz', 'xyz')]"
```

## <a name="guid"></a>guid

Generiert eine global eindeutige Zeichenfolge (GUID).

Im folgenden Beispiel wird ein Wert wie `"c7bc8bdc-7252-4a82-ba53-7c468679a511"` zurückgegeben:

```json
"[guid()]"
```

## <a name="indexof"></a>indexOf

Gibt die erste Position eines Werts innerhalb einer Zeichenfolge zurück oder „-1“, wenn er nicht gefunden wurde.

Im folgenden Beispiel wird 2 zurückgegeben.

```json
"[indexOf('abcdef', 'cd')]"
```

## <a name="lastindexof"></a>lastIndexOf

Gibt die letzte Position eines Werts in einer Zeichenfolge zurück oder „-1“, wenn er nicht gefunden wurde.

Im folgenden Beispiel wird 3 zurückgegeben.

```json
"[lastIndexOf('test', 't')]"
```

## <a name="replace"></a>replace

Gibt eine Zeichenfolge zurück, in der alle Vorkommen der angegebenen Zeichenfolge in der aktuellen Zeichenfolge durch eine andere Zeichenfolge ersetzt werden.

Das folgende Beispiel gibt `"Contoso.com web app"`zurück:

```json
"[replace('Contoso.net web app', '.net', '.com')]"
```

## <a name="startswith"></a>startsWith

Stellt fest, ob eine Zeichenfolge mit einem bestimmten Wert beginnt.

Im folgenden Beispiel wird „true“ zurückgegeben.

```json
"[startsWith('abcdefg', 'ab')]"
```

## <a name="substring"></a>substring

Gibt die Teilzeichenfolge der angegebenen Zeichenfolge zurück. Die Teilzeichenfolge beginnt am angegebenen Index und weist die angegebene Länge auf.

Das folgende Beispiel gibt `"web"`zurück:

```json
"[substring('Contoso.com web app', 12, 3)]"
```

## <a name="tolower"></a>toLower

Gibt eine in Kleinbuchstaben konvertierte Zeichenfolge zurück.

Das folgende Beispiel gibt `"contoso"`zurück:

```json
"[toLower('CONTOSO')]"
```

## <a name="toupper"></a>toUpper

Gibt eine in Großbuchstaben konvertierte Zeichenfolge zurück.

Das folgende Beispiel gibt `"CONTOSO"`zurück:

```json
"[toUpper('contoso')]"
```

## <a name="next-steps"></a>Nächste Schritte

* Eine Einführung in Azure Resource Manager finden Sie unter [Übersicht über Azure Resource Manager](../management/overview.md).

