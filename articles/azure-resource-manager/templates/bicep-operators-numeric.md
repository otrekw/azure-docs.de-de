---
title: Numerische Operatoren für Bicep
description: Hier werden numerische Operatoren für Bicep beschrieben, die Werte berechnen.
ms.topic: conceptual
ms.date: 04/07/2021
ms.openlocfilehash: f2c7f722a3f13648c94b69039d31e5095a3256f7
ms.sourcegitcommit: c3739cb161a6f39a9c3d1666ba5ee946e62a7ac3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/08/2021
ms.locfileid: "107211237"
---
# <a name="bicep-numeric-operators"></a>Numerische Operatoren für Bicep

Die numerischen Operatoren verwenden zur Ausführung von Berechnungen ganze Zahlen und geben ganzzahlige Werte zurück. Verwenden Sie zum Ausführen der Beispiele Azure CLI oder Azure PowerShell, um [eine Bicep-Datei bereitzustellen](bicep-tutorial-create-first-bicep.md#deploy-bicep-file).

| Operator | Name |
| ---- | ---- |
| `*` | [Multiplizieren](#multiply-) |
| `/` | [Dividieren](#divide-) |
| `%` | [Modulo](#modulo-) |
| `+` | [Add (Hinzufügen)](#add-) |
| `-` | [Subtrahieren](#subtract--) |
| `-` | [Minus](#minus--) |

> [!NOTE]
> Subtrahieren und Minus verwenden denselben Operator. Die Funktionalität ist unterschiedlich, weil Subtrahieren zwei Operanden und Minus einen Operanden verwendet.

## <a name="multiply-"></a>Multiplizieren *

`operand1 * operand2`

Multipliziert zwei ganze Zahlen.

### <a name="operands"></a>Operanden

| Operand | type | BESCHREIBUNG |
| ---- | ---- | ---- |
| `operand1`  | integer | Eine zu multiplizierende Zahl. |
| `operand2`  | integer  | Der Multiplikator der Zahl. |

### <a name="return-value"></a>Rückgabewert

Die Multiplikation gibt das Produkt als ganze Zahl zurück.

### <a name="example"></a>Beispiel

Zwei ganze Zahlen werden multipliziert und geben das Produkt zurück.

```bicep
param firstInt int = 5
param secondInt int = 2

output product int = firstInt * secondInt
```

Ausgabe des Beispiels:

| Name | type | Wert |
| ---- | ---- | ---- |
| `product` | integer | 10 |

## <a name="divide-"></a>Dividieren /

`operand1 / operand2`

Dividiert eine ganze Zahl durch eine ganze Zahl.

### <a name="operands"></a>Operanden

| Operand | type | BESCHREIBUNG |
| ---- | ---- | ---- |
| `operand1` | integer | Ganze Zahl, die geteilt wird. |
| `operand2` | integer | Ganze Zahl, die für die Division verwendet wird. Darf nicht Null (0) sein. |

### <a name="return-value"></a>Rückgabewert

Die Division gibt den Quotienten als ganze Zahl zurück.

### <a name="example"></a>Beispiel

Zwei ganze Zahlen werden geteilt und geben den Quotienten zurück.

```bicep
param firstInt int = 10
param secondInt int = 2

output quotient int = firstInt / secondInt
```

Ausgabe des Beispiels:

| Name | type | Wert |
| ---- | ---- | ---- |
| `quotient` | integer | 5 |

## <a name="modulo-"></a>Modulo %

`operand1 % operand2`

Dividiert eine ganze Zahl durch eine ganze Zahl und gibt den Rest zurück.

### <a name="operands"></a>Operanden

| Operand | type | BESCHREIBUNG |
| ---- | ---- | ---- |
| `operand1`  | integer  | Die ganze Zahl, die geteilt wird. |
| `operand2`  | integer  | Die für die Division verwendete ganze Zahl. Darf nicht null (0) sein. |

### <a name="return-value"></a>Rückgabewert

Der Rest wird als ganze Zahl zurückgegeben. Wenn die Division keinen Rest ergibt, wird 0 zurückgegeben.

### <a name="example"></a>Beispiel

Zwei Ganzzahlpaare werden geteilt und geben die Reste zurück.

```bicep
param firstInt int = 10
param secondInt int = 3

param thirdInt int = 8
param fourthInt int = 4

output remainder int = firstInt % secondInt
output zeroRemainder int = thirdInt % fourthInt
```

Ausgabe des Beispiels:

| Name | type | Wert |
| ---- | ---- | ---- |
| `remainder` | integer | 1 |
| `zeroRemainder` | integer | 0 |

## <a name="add-"></a>Addieren +

`operand1 + operand2`

Addiert zwei ganze Zahlen.

### <a name="operands"></a>Operanden

| Operand | type | BESCHREIBUNG |
| ---- | ---- | ---- |
| `operand1` | integer | Eine zu addierende Zahl. |
| `operand2` | integer | Zahl, die zu einer Zahl addiert wird. |

### <a name="return-value"></a>Rückgabewert

Die Addition gibt die Summe als ganze Zahl zurück.

### <a name="example"></a>Beispiel

Zwei ganze Zahlen werden addiert und geben die Summe zurück.

```bicep
param firstInt int = 10
param secondInt int = 2

output sum int = firstInt + secondInt
```

Ausgabe des Beispiels:

| Name | type | Wert |
| ---- | ---- | ---- |
| `sum` | integer | 12 |

## <a name="subtract--"></a>Subtrahieren -

`operand1 - operand2`

Subtrahiert eine ganze Zahl von einer ganzen Zahl.

### <a name="operands"></a>Operanden

| Operand | type | BESCHREIBUNG |
| ---- | ---- | ---- |
| `operand1` | integer | Größere Zahl, von der subtrahiert wird. |
| `operand2` | integer | Zahl, die von der größeren Zahl subtrahiert wird. |

### <a name="return-value"></a>Rückgabewert

Die Subtraktion gibt die Differenz als ganze Zahl zurück.

### <a name="example"></a>Beispiel

Eine ganze Zahl wird subtrahiert und gibt die Differenz zurück.

```bicep
param firstInt int = 10
param secondInt int = 4

output difference int = firstInt - secondInt
```

Ausgabe des Beispiels:

| Name | type | Wert |
| ---- | ---- | ---- |
| `difference` | integer | 6 |

## <a name="minus--"></a>Minus -

`-integerValue`

Multipliziert eine ganze Zahl mit `-1`.

### <a name="operand"></a>Operand

| Operand | type | BESCHREIBUNG |
| ---- | ---- | ---- |
| `integerValue` | integer | Ganze Zahl, die mit `-1` multipliziert wird. |

### <a name="return-value"></a>Rückgabewert

Eine ganze Zahl wird mit `-1` multipliziert. Eine positive ganze Zahl gibt eine negative ganze Zahl zurück, und eine negative ganze Zahl gibt eine positive ganze Zahl zurück. Die Werte können mit Klammern umschlossen werden.

### <a name="example"></a>Beispiel

```bicep
param posInt int = 10
param negInt int = -20

output startedPositive int = -posInt
output startedNegative int = -(negInt)
```

Ausgabe des Beispiels:

| Name | type | Wert |
| ---- | ---- | ---- |
| `startedPositive` | integer | -10 |
| `startedNegative` | integer | 20 |

## <a name="next-steps"></a>Nächste Schritte

- Informationen zum Erstellen einer Bicep-Datei finden Sie unter [Tutorial: Erstellen und Bereitstellen Ihrer ersten Azure Resource Manager-Bicep-Datei](bicep-tutorial-create-first-bicep.md).
- Informationen zum Beheben von Bicep-Typfehlern finden Sie unter [any-Funktion für Bicep](template-functions-any.md).
- Informationen zum Vergleichen der Syntax für Bicep und JSON finden Sie unter [Vergleichen von JSON mit Bicep für Vorlagen](compare-template-syntax.md).
- Beispiele für Bicep- und ARM-Vorlagenfunktionen finden Sie unter [ARM-Vorlagenfunktionen](template-functions.md).
