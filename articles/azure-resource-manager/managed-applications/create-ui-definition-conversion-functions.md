---
title: Erstellen von Benutzeroberflächendefinitions-Konvertierungsfunktionen
description: Beschreibt die Funktionen, die beim Konvertieren von Werten zwischen Datentypen und Codierungen verwendet werden können.
author: tfitzmac
ms.topic: conceptual
ms.date: 07/13/2020
ms.author: tomfitz
ms.openlocfilehash: b69cd35b27b343da08727b4c4ee9b4fd025e1df7
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "87094445"
---
# <a name="createuidefinition-conversion-functions"></a>CreateUiDefinition-Konvertierungsfunktionen

Diese Funktionen können verwendet werden, um Werte zwischen JSON-Datentypen und Codierungen zu konvertieren.

## <a name="bool"></a>bool

Konvertiert den Parameter in einen booleschen Wert. Diese Funktion unterstützt Parameter vom Typ „Zahl“, „Zeichenfolge“ und „Boolesch“. Ähnlich wie boolesche Werte in JavaScript geben alle Werte außer `0` oder `'false'` `true` zurück.

Das folgende Beispiel gibt `true`zurück:

```json
"[bool(1)]"
```

Das folgende Beispiel gibt `false`zurück:

```json
"[bool(0)]"
```

Das folgende Beispiel gibt `true`zurück:

```json
"[bool(true)]"
```

Das folgende Beispiel gibt `true`zurück:

```json
"[bool('true')]"
```

## <a name="decodebase64"></a>decodeBase64

Decodiert den Parameter aus einer Base64-codierten Zeichenfolge. Diese Funktion unterstützt nur Parameter vom Typ „Zeichenfolge“.

Das folgende Beispiel gibt `"Contoso"`zurück:

```json
"[decodeBase64('Q29udG9zbw==')]"
```

## <a name="decodeuricomponent"></a>decodeUriComponent

Decodiert den Parameter aus einer URL-codierten Zeichenfolge. Diese Funktion unterstützt nur Parameter vom Typ „Zeichenfolge“.

Das folgende Beispiel gibt `"https://portal.azure.com/"`zurück:

```json
"[decodeUriComponent('https%3A%2F%2Fportal.azure.com%2F')]"
```

## <a name="encodebase64"></a>encodeBase64

Codiert den Parameter in eine Base64-codierte Zeichenfolge. Diese Funktion unterstützt nur Parameter vom Typ „Zeichenfolge“.

Das folgende Beispiel gibt `"Q29udG9zbw=="`zurück:

```json
"[encodeBase64('Contoso')]"
```

## <a name="encodeuricomponent"></a>encodeUriComponent

Codiert den Parameter in eine URL-codierte Zeichenfolge. Diese Funktion unterstützt nur Parameter vom Typ „Zeichenfolge“.

Das folgende Beispiel gibt `"https%3A%2F%2Fportal.azure.com%2F"`zurück:

```json
"[encodeUriComponent('https://portal.azure.com/')]"
```

## <a name="float"></a>float

Konvertiert den Parameter in eine Gleitkommazahl. Diese Funktion unterstützt Parameter vom Typ „Zahl“ und „Zeichenfolge“.

Das folgende Beispiel gibt `1.0`zurück:

```json
"[float('1.0')]"
```

Das folgende Beispiel gibt `2.9`zurück:

```json
"[float(2.9)]"
```

## <a name="int"></a>INT

Konvertiert den Parameter in eine ganze Zahl. Diese Funktion unterstützt Parameter vom Typ „Zahl“ und „Zeichenfolge“.

Das folgende Beispiel gibt `1`zurück:

```json
"[int('1')]"
```

Das folgende Beispiel gibt `2`zurück:

```json
"[int(2.9)]"
```

## <a name="parse"></a>parse

Konvertiert den Parameter in einen systemeigenen Typ. Das heißt, diese Funktion ist das Gegenteil von `string()`. Diese Funktion unterstützt nur Parameter vom Typ „Zeichenfolge“.

Das folgende Beispiel gibt `1`zurück:

```json
"[parse('1')]"
```

Das folgende Beispiel gibt `true`zurück:

```json
"[parse('true')]"
```

Das folgende Beispiel gibt `[1,2,3]`zurück:

```json
"[parse('[1,2,3]')]"
```

Das folgende Beispiel gibt `{"type":"webapp"}`zurück:

```json
"[parse('{\"type\":\"webapp\"}')]"
```

## <a name="string"></a>Zeichenfolge

Konvertiert den Parameter in eine Zeichenfolge. Diese Funktion unterstützt Parameter aller JSON-Datentypen.

Das folgende Beispiel gibt `"1"`zurück:

```json
"[string(1)]"
```

Das folgende Beispiel gibt `"2.9"`zurück:

```json
"[string(2.9)]"
```

Das folgende Beispiel gibt `"[1,2,3]"`zurück:

```json
"[string([1,2,3])]"
```

Das folgende Beispiel gibt `"{"type":"webapp"}"`zurück:

```json
"[string({\"type\":\"webapp\"})]"
```

## <a name="next-steps"></a>Nächste Schritte

* Eine Einführung in Azure Resource Manager finden Sie unter [Übersicht über Azure Resource Manager](../management/overview.md).
