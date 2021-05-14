---
title: Bicep-Vergleichsoperatoren
description: Beschreibt Bicep-Vergleichsoperatoren, die Werte vergleichen.
ms.topic: conceptual
ms.date: 04/07/2021
ms.openlocfilehash: c382ef355131d271d9f4fa4a978a807b9aac1e4f
ms.sourcegitcommit: c3739cb161a6f39a9c3d1666ba5ee946e62a7ac3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/08/2021
ms.locfileid: "107211238"
---
# <a name="bicep-comparison-operators"></a>Bicep-Vergleichsoperatoren

Die Vergleichsoperatoren vergleichen Werte und geben entweder `true` oder `false` zurück. Verwenden Sie zum Ausführen der Beispiele die Azure CLI oder Azure PowerShell, um [eine Bicep-Datei bereitzustellen](bicep-tutorial-create-first-bicep.md#deploy-bicep-file).

| Operator | Name |
| ---- | ---- |
| `>=` | [Größer oder gleich](#greater-than-or-equal-) |
| `>`  | [Größer als](#greater-than-) |
| `<=` | [Kleiner oder gleich](#less-than-or-equal-) |
| `<`  | [Kleiner als](#less-than-) |
| `==` | [Ist gleich](#equals-) |
| `!=` | [Ungleich](#not-equal-) |
| `=~` | [Gleiche Groß-/Kleinschreibung](#equal-case-insensitive-) |
| `!~` | [Ungleiche Groß-/Kleinschreibung](#not-equal-case-insensitive-) |

## <a name="greater-than-or-equal-"></a>Größer als oder gleich >=

`operand1 >= operand2`

Bewertet, ob der erste Wert größer oder gleich dem zweiten Wert ist.

### <a name="operands"></a>Operanden

| Operand | type | BESCHREIBUNG |
| ---- | ---- | ---- |
| `operand1` | Ganzzahl, Zeichenfolge | Erster Wert im Vergleich. |
| `operand2` | Ganzzahl, Zeichenfolge | Zweiter Wert im Vergleich. |

### <a name="return-value"></a>Rückgabewert

Wenn der erste Wert größer oder gleich dem zweiten Wert ist, wird `true` zurückgegeben. Andernfalls wird `false` zurückgegeben.

### <a name="example"></a>Beispiel

Ein Ganzzahlpaar und ein Zeichenfolgenpaar werden verglichen.

```bicep
param firstInt int = 10
param secondInt int = 5

param firstString string = 'A'
param secondString string = 'A'

output intGtE bool = firstInt >= secondInt
output stringGtE bool = firstString >= secondString
```

Ausgabe des Beispiels:

| Name | type | Wert |
| ---- | ---- | ---- |
| `intGtE` | boolean | true |
| `stringGtE` | boolean | true |

## <a name="greater-than-"></a>Größer als >

`operand1 > operand2`

Bewertet, ob der erste Wert größer als der zweite ist.

### <a name="operands"></a>Operanden

| Operand | type | BESCHREIBUNG |
| ---- | ---- | ---- |
| `operand1` | Ganzzahl, Zeichenfolge | Erster Wert im Vergleich. |
| `operand2` | Ganzzahl, Zeichenfolge | Zweiter Wert im Vergleich. |

### <a name="return-value"></a>Rückgabewert

Wenn der erste Wert größer als der zweite ist, wird `true` zurückgegeben. Andernfalls wird `false` zurückgegeben.

### <a name="example"></a>Beispiel

Ein Ganzzahlpaar und ein Zeichenfolgenpaar werden verglichen.

```bicep
param firstInt int = 10
param secondInt int = 5

param firstString string = 'bend'
param secondString string = 'band'

output intGt bool = firstInt > secondInt
output stringGt bool = firstString > secondString
```

Ausgabe des Beispiels:

Durch **e** in **bend** wird die erste Zeichenfolge größer.

| Name | type | Wert |
| ---- | ---- | ---- |
| `intGt` | boolean | true |
| `stringGt` | boolean | true |

## <a name="less-than-or-equal-"></a>Kleiner als oder gleich <=

`operand1 <= operand2`

Bewertet, ob der erste Wert kleiner oder gleich dem zweiten Wert ist.

### <a name="operands"></a>Operanden

| Operand | type | BESCHREIBUNG |
| ---- | ---- | ---- |
| `operand1` | Ganzzahl, Zeichenfolge | Erster Wert im Vergleich. |
| `operand2` | Ganzzahl, Zeichenfolge | Zweiter Wert im Vergleich. |

### <a name="return-value"></a>Rückgabewert

Wenn der erste Wert kleiner oder gleich dem zweiten Wert ist, wird `true` zurückgegeben. Andernfalls wird `false` zurückgegeben.

### <a name="example"></a>Beispiel

Ein Ganzzahlpaar und ein Zeichenfolgenpaar werden verglichen.

```bicep
param firstInt int = 5
param secondInt int = 10

param firstString string = 'demo'
param secondString string = 'demo'

output intLtE bool = firstInt <= secondInt
output stringLtE bool = firstString <= secondString
```

Ausgabe des Beispiels:

| Name | type | Wert |
| ---- | ---- | ---- |
| `intLtE` | boolean | true |
| `stringLtE` | boolean | true |

## <a name="less-than-"></a>Kleiner als <

`operand1 < operand2`

Bewertet, ob der erste Wert kleiner als der zweite ist.

### <a name="operands"></a>Operanden

| Operand | type | BESCHREIBUNG |
| ---- | ---- | ---- |
| `operand1` | Ganzzahl, Zeichenfolge | Erster Wert im Vergleich. |
| `operand2` | Ganzzahl, Zeichenfolge | Zweiter Wert im Vergleich. |

### <a name="return-value"></a>Rückgabewert

Wenn der erste Wert kleiner als der zweite ist, wird `true` zurückgegeben. Andernfalls wird `false` zurückgegeben.

### <a name="example"></a>Beispiel

Ein Ganzzahlpaar und ein Zeichenfolgenpaar werden verglichen.

```bicep
param firstInt int = 5
param secondInt int = 10

param firstString string = 'demo'
param secondString string = 'Demo'

output intLt bool = firstInt < secondInt
output stringLt bool = firstString < secondString
```

Ausgabe des Beispiels:

Die Zeichenfolge ist `true`, da Kleinbuchstaben kleiner als Großbuchstaben sind.

| Name | type | Wert |
| ---- | ---- | ---- |
| `intLt` | boolean | true |
| `stringLt` | boolean | true |

## <a name="equals-"></a>Gleich ==

`operand1 == operand2`

Bewertet, ob die Werte gleich sind.

### <a name="operands"></a>Operanden

| Operand | type | BESCHREIBUNG |
| ---- | ---- | ---- |
| `operand1` | string, integer, boolean, array, object | Erster Wert im Vergleich. |
| `operand2` | string, integer, boolean, array, object | Zweiter Wert im Vergleich. |

### <a name="return-value"></a>Rückgabewert

Wenn die Operanden gleich sind, wird `true` zurückgegeben. Wenn sich die Operanden unterscheiden, wird `false` zurückgegeben.

### <a name="example"></a>Beispiel

Paare aus ganzen Zahlen, Zeichenfolgen und booleschen Werten werden verglichen.

```bicep
param firstInt int = 5
param secondInt int = 5

param firstString string = 'demo'
param secondString string = 'demo'

param firstBool bool = true
param secondBool bool = true

output intEqual bool = firstInt == secondInt
output stringEqual bool = firstString == secondString
output boolEqual bool = firstBool == secondBool
```

Ausgabe des Beispiels:

| Name | type | Wert |
| ---- | ---- | ---- |
| `intEqual` | boolean | true |
| `stringEqual` | boolean | true |
| `boolEqual` | boolean | true |

## <a name="not-equal-"></a>Ungleich !=

`operand1 != operand2`

Bewertet, ob die beiden Werte **nicht** gleich sind.

### <a name="operands"></a>Operanden

| Operand | type | BESCHREIBUNG |
| ---- | ---- | ---- |
| `operand1` | string, integer, boolean, array, object | Erster Wert im Vergleich. |
| `operand2` | string, integer, boolean, array, object | Zweiter Wert im Vergleich. |

### <a name="return-value"></a>Rückgabewert

Wenn die Operanden **nicht** gleich sind, wird `true` zurückgegeben. Wenn die Operanden gleich sind, wird `false` zurückgegeben.

### <a name="example"></a>Beispiel

Paare aus ganzen Zahlen, Zeichenfolgen und booleschen Werten werden verglichen.

```bicep
param firstInt int = 10
param secondInt int = 5

param firstString string = 'demo'
param secondString string = 'test'

param firstBool bool = false
param secondBool bool = true

output intNotEqual bool = firstInt != secondInt
output stringNotEqual bool = firstString != secondString
output boolNotEqual bool = firstBool != secondBool
```

Ausgabe des Beispiels:

| Name | type | Wert |
| ---- | ---- | ---- |
| `intNotEqual` | boolean | true |
| `stringNotEqual` | boolean | true |
| `boolNotEqual` | boolean | true |

## <a name="equal-case-insensitive-"></a>Groß-/Kleinschreibung nicht berücksichtigt =~

`operand1 =~ operand2`

Ignoriert Groß- und Kleinschreibung, um festzustellen, ob die beiden Werte gleich sind.

### <a name="operands"></a>Operanden

| Operand | type | BESCHREIBUNG |
| ---- | ---- | ---- |
| `operand1`  | Zeichenfolge | Erste Zeichenfolge im Vergleich. |
| `operand2`  | Zeichenfolge | Zweite Zeichenfolge im Vergleich. |

### <a name="return-value"></a>Rückgabewert

Wenn die Zeichenfolgen gleich sind, wird `true` zurückgegeben. Andernfalls wird `false` zurückgegeben.

### <a name="example"></a>Beispiel

Vergleicht Zeichenfolgen, die sowohl Groß- als auch Kleinbuchstaben enthalten.

```bicep
param firstString string = 'demo'
param secondString string = 'DEMO'

param thirdString string = 'demo'
param fourthString string = 'TEST'

output strEqual1 bool = firstString =~ secondString
output strEqual2 bool = thirdString =~ fourthString
```

Ausgabe des Beispiels:

| Name | type | Wert |
| ---- | ---- | ---- |
| `strEqual1` | boolean | true |
| `strEqual2` | boolean | false |

## <a name="not-equal-case-insensitive-"></a>Ungleiche Groß-/Kleinschreibung wird nicht berücksichtigt !~

`operand1 !~ operand2`

Ignoriert die Groß-/Schreibung, um zu bestimmen, ob die beiden Werte **nicht** gleich sind.

### <a name="operands"></a>Operanden

| Operand | type | BESCHREIBUNG |
| ---- | ---- | ---- |
| `operand1` | Zeichenfolge | Erste Zeichenfolge im Vergleich. |
| `operand2` | Zeichenfolge | Zweite Zeichenfolge im Vergleich. |

### <a name="return-value"></a>Rückgabewert

Wenn die Zeichenfolgen **ungleich** sind, wird `true` zurückgegeben. Andernfalls wird `false` zurückgegeben.

### <a name="example"></a>Beispiel

Vergleicht Zeichenfolgen, die sowohl Groß- als auch Kleinbuchstaben enthalten.

```bicep
param firstString string = 'demo'
param secondString string = 'TEST'

param thirdString string = 'demo'
param fourthString string = 'DeMo'

output strNotEqual1 bool = firstString !~ secondString
output strEqual2 bool = thirdString !~ fourthString
```

Ausgabe des Beispiels:

| Name | type | Wert |
| ---- | ---- | ---- |
| `strNotEqual1` | boolean | true |
| `strNotEqual2` | boolean | false |

## <a name="next-steps"></a>Nächste Schritte

- Informationen zum Erstellen einer Bicep-Datei finden Sie unter [Tutorial: Erstellen und Bereitstellen Ihrer ersten Azure Resource Manager-Bicep-Datei](bicep-tutorial-create-first-bicep.md).
- Informationen zum Beheben von Bicep-Typfehlern finden Sie unter [any-Funktion für Bicep](template-functions-any.md).
- Informationen zum Vergleichen der Syntax für Bicep und JSON finden Sie unter [Vergleichen von JSON und Bicep für Vorlagen](compare-template-syntax.md).
- Beispiele für Bicep- und ARM-Vorlagenfunktionen finden Sie unter [ARM-Vorlagenfunktionen.](template-functions.md)
