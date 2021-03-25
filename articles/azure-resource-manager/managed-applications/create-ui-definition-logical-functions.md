---
title: Erstellen von logischen Benutzeroberflächendefinitions-Funktionen
description: Beschreibt die Funktionen für die Durchführung logischer Operationen.
author: tfitzmac
ms.topic: conceptual
ms.date: 07/13/2020
ms.author: tomfitz
ms.openlocfilehash: 00d2f0eeb5d353c8ebd7ad30f6866f890d6cb42e
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "87094442"
---
# <a name="createuidefinition-logical-functions"></a>Logische CreateUiDefinition-Funktionen

Diese Funktionen können in bedingten Ausdrücken verwendet werden. Einige Funktionen unterstützen möglicherweise nicht alle JSON-Datentypen.

## <a name="and"></a>and

Gibt `true` zurück, wenn alle Parameter zu `true` ausgewertet werden. Diese Funktion unterstützt zwei oder mehr Parameter vom Typ „Boolesch“.

Das folgende Beispiel gibt `true`zurück:

```json
"[and(equals(0, 0), equals('web', 'web'), less(1, 2))]"
```

Das folgende Beispiel gibt `false`zurück:

```json
"[and(equals(0, 0), greater(1, 2))]"
```

## <a name="coalesce"></a>coalesce

Gibt den Wert des ersten Parameters ungleich NULL zurück. Diese Funktion unterstützt alle JSON-Datentypen.

Angenommen `element1` und `element2` sind nicht definiert. Das folgende Beispiel gibt `"Contoso"`zurück:

```json
"[coalesce(steps('demoStep').element1, steps('demoStep').element2, 'Contoso')]"
```

Diese Funktion ist besonders nützlich im Kontext eines optionalen Aufrufs, der aufgrund einer Benutzeraktion nach dem Laden der Seite auftritt. Ein Beispiel hierfür ist, wenn die Einschränkungen, die für ein Feld in der Benutzeroberfläche festgelegt wurden, vom aktuell ausgewählten Wert eines anderen, **anfänglich nicht sichtbaren** Felds abhängen. In diesem Fall kann `coalesce()` verwendet werden, um zu ermöglichen, dass die Funktion zur Seitenladezeit syntaktisch gültig ist, während gleichzeitig der gewünschte Effekt erzielt wird, wenn der Benutzer mit dem Feld interagiert.

Beachten Sie dieses `DropDown`, das es dem Benutzer ermöglicht, aus mehreren verschiedenen Datenbanktypen auszuwählen:

```
{
    "name": "databaseType",
    "type": "Microsoft.Common.DropDown",
    "label": "Choose database type",
    "toolTip": "Choose database type",
    "defaultValue": "Oracle Database",
    "visible": "[bool(steps('section_database').connectToDatabase)]"
    "constraints": {
        "allowedValues": [
            {
                "label": "Azure Database for PostgreSQL",
                "value": "postgresql"
            },
            {
                "label": "Oracle Database",
                "value": "oracle"
            },
            {
                "label": "Azure SQL",
                "value": "sqlserver"
            }
        ],
        "required": true
    },
```

Um die Aktion eines anderen Felds bedingt an den aktuell ausgewählten Wert für dieses Felds zu binden, verwenden Sie `coalesce()`, wie hier gezeigt:

```
"regex": "[concat('^jdbc:', coalesce(steps('section_database').databaseConnectionInfo.databaseType, ''), '.*$')]",
```

Dies ist notwendig, da der `databaseType` anfänglich nicht sichtbar ist und daher keinen Wert besitzt. Dies bewirkt, dass der gesamte Ausdruck nicht ordnungsgemäß ausgewertet wird.

## <a name="equals"></a>equals

Gibt `true` zurück, wenn beide Parameter denselben Typ und Wert aufweisen. Diese Funktion unterstützt alle JSON-Datentypen.

Das folgende Beispiel gibt `true`zurück:

```json
"[equals(0, 0)]"
```

Das folgende Beispiel gibt `true`zurück:

```json
"[equals('web', 'web')]"
```

Das folgende Beispiel gibt `false`zurück:

```json
"[equals('abc', ['a', 'b', 'c'])]"
```

## <a name="greater"></a>greater

Gibt `true` zurück, wenn der erste Parameter streng größer als der zweite Parameter ist. Diese Funktion unterstützt nur Parameter vom Typ „Zahl“ und „Zeichenfolge“.

Das folgende Beispiel gibt `false`zurück:

```json
"[greater(1, 2)]"
```

Das folgende Beispiel gibt `true`zurück:

```json
"[greater('9', '10')]"
```

## <a name="greaterorequals"></a>greaterOrEquals

Gibt `true` zurück, wenn der erste Parameter im Vergleich zum zweiten Parameter größer oder gleich groß ist. Diese Funktion unterstützt nur Parameter vom Typ „Zahl“ und „Zeichenfolge“.

Das folgende Beispiel gibt `true`zurück:

```json
"[greaterOrEquals(2, 2)]"
```

## <a name="if"></a>if

Gibt einen Wert abhängig davon zurück, ob eine Bedingung zutrifft oder nicht. Der erste Parameter ist die zu testende Bedingung. Der zweite Parameter ist der Wert für die Rückgabe, wenn die Bedingung „true“ ist. Der dritte Parameter ist der Wert für die Rückgabe, wenn die Bedingung „false“ ist.

Im folgenden Beispiel wird `yes` zurückgegeben.

```json
"[if(equals(42, mul(6, 7)), 'yes', 'no')]"
```

## <a name="less"></a>less

Gibt `true` zurück, wenn der erste Parameter streng kleiner als der zweite Parameter ist. Diese Funktion unterstützt nur Parameter vom Typ „Zahl“ und „Zeichenfolge“.

Das folgende Beispiel gibt `true`zurück:

```json
"[less(1, 2)]"
```

Das folgende Beispiel gibt `false`zurück:

```json
"[less('9', '10')]"
```

## <a name="lessorequals"></a>lessOrEquals

Gibt `true` zurück, wenn der erste Parameter im Vergleich zum zweiten Parameter kleiner oder gleich groß ist. Diese Funktion unterstützt nur Parameter vom Typ „Zahl“ und „Zeichenfolge“.

Das folgende Beispiel gibt `true`zurück:

```json
"[lessOrEquals(2, 2)]"
```

## <a name="not"></a>not

Gibt `true` zurück, wenn der Parameter zu `false` ausgewertet wird. Diese Funktion unterstützt nur Parameter vom Typ „Boolesch“.

Das folgende Beispiel gibt `true`zurück:

```json
"[not(false)]"
```

Das folgende Beispiel gibt `false`zurück:

```json
"[not(equals(0, 0))]"
```

## <a name="or"></a>oder

Gibt `true` zurück, wenn mindestens einer der Parameter zu `true` ausgewertet wird. Diese Funktion unterstützt zwei oder mehr Parameter vom Typ „Boolesch“.

Das folgende Beispiel gibt `true`zurück:

```json
"[or(equals(0, 0), equals('web', 'web'), less(1, 2))]"
```

Das folgende Beispiel gibt `true`zurück:

```json
"[or(equals(0, 0), greater(1, 2))]"
```

## <a name="next-steps"></a>Nächste Schritte

* Eine Einführung in Azure Resource Manager finden Sie unter [Übersicht über Azure Resource Manager](../management/overview.md).
