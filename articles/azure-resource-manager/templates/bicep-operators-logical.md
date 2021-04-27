---
title: Logische Bicep-Operatoren
description: Beschreibt logische Bicep-Operatoren, die Bedingungen auswerten.
ms.topic: conceptual
ms.date: 04/07/2021
ms.openlocfilehash: 1eecbe589ecf08e32246dd97d137c60fa7b5078f
ms.sourcegitcommit: c3739cb161a6f39a9c3d1666ba5ee946e62a7ac3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/08/2021
ms.locfileid: "107211246"
---
# <a name="bicep-logical-operators"></a>Logische Bicep-Operatoren

Die logischen Operatoren werten boolesche Werte aus, geben Werte ungleich NULL zurück oder werten einen bedingten Ausdruck aus. Verwenden Sie zum Ausführen der Beispiele die Azure-Befehlszeilenschnittstelle (Azure CLI) oder Azure PowerShell, um [eine Bicep-Datei bereitzustellen](bicep-tutorial-create-first-bicep.md#deploy-bicep-file).

| Operator | Name |
| ---- | ---- |
| `&&` | [Und](#and-) |
| `||` | [Oder](#or-) |
| `!` | [Not](#not-) |
| `??` | [Zusammenfügen](#coalesce-) |
| `?` `:` | [Bedingter Ausdruck](#conditional-expression--) |

## <a name="and-"></a>Und &&

`operand1 && operand2`

Ermittelt, ob beide Werte wahr (TRUE) sind.

### <a name="operands"></a>Operanden

| Operand | type | BESCHREIBUNG |
| ---- | ---- | ---- |
| `operand1` | boolean | Der erste Wert, für den überprüft werden soll, ob er wahr ist. |
| `operand2` | boolean | Der zweite Wert, für den überprüft werden soll, ob er wahr ist. |
| Weitere Operanden | boolean | Es können weitere Operanden eingeschlossen werden. |

### <a name="return-value"></a>Rückgabewert

`True`, wenn beide Werte wahr sind, andernfalls wird `false` zurückgegeben.

### <a name="example"></a>Beispiel

Wertet einen Satz von Parameterwerten und einen Satz von Ausdrücken aus.

```bicep
param operand1 bool = true
param operand2 bool = true

output andResultParm bool = operand1 && operand2
output andResultExp bool = bool(10 >= 10) && bool(5 > 2)
```

Ausgabe des Beispiels:

| Name | type | Wert |
| ---- | ---- | ---- |
| `andResultParm` | boolean | true |
| `andResultExp` | boolean | true |

## <a name="or-"></a>Oder ||

`operand1 || operand2`

Ermittelt, ob einer der beiden Werte wahr (TRUE) ist.

### <a name="operands"></a>Operanden

| Operand | type | BESCHREIBUNG |
| ---- | ---- | ---- |
| `operand1` | boolean | Der erste Wert, für den überprüft werden soll, ob er wahr ist. |
| `operand2` | boolean | Der zweite Wert, für den überprüft werden soll, ob er wahr ist. |
| Weitere Operanden | boolean | Es können weitere Operanden eingeschlossen werden. |

### <a name="return-value"></a>Rückgabewert

`True`, wenn einer der Werte wahr ist, andernfalls wird `false` zurückgegeben.

### <a name="example"></a>Beispiel

Wertet einen Satz von Parameterwerten und einen Satz von Ausdrücken aus.

```bicep
param operand1 bool = true
param operand2 bool = false

output orResultParm bool = operand1 || operand2
output orResultExp bool = bool(10 >= 10) || bool(5 < 2)
```

Ausgabe des Beispiels:

| Name | type | Wert |
| ---- | ---- | ---- |
| `orResultParm` | boolean | true |
| `orResultExp` | boolean | true |

## <a name="not-"></a>Nicht !

`!boolValue`

Negiert einen booleschen Wert.

### <a name="operand"></a>Operand

| Operand | type | BESCHREIBUNG |
| ---- | ---- | ---- |
| `boolValue` | boolean | Boolescher Wert, der negiert wird. |

### <a name="return-value"></a>Rückgabewert

Negiert den Anfangswert und gibt einen booleschen Wert zurück. Wenn der Anfangswert `true` ist, wird `false` zurückgegeben.

### <a name="example"></a>Beispiel

Der `not`-Operator negiert einen Wert. Die Werte können in Klammern eingeschlossen werden.

```bicep
param initTrue bool = true
param initFalse bool = false

output startedTrue bool = !(initTrue)
output startedFalse bool = !initFalse
```

Ausgabe des Beispiels:

| Name | type | Wert |
| ---- | ---- | ---- |
| `startedTrue` | boolean | false |
| `startedFalse` | boolean | true |

## <a name="coalesce-"></a>Zusammenfügen ??

`operand1 ?? operand2`

Gibt den ersten Wert der Operanden zurück, der ungleich NULL ist.

### <a name="operands"></a>Operanden

| Operand | type | BESCHREIBUNG |
| ---- | ---- | ---- |
| `operand1` | Zeichenfolge, ganze Zahl, boolescher Wert, Objekt, Array | Der Wert, für den getestet werden soll, ob er `null` ist. |
| `operand2` | Zeichenfolge, ganze Zahl, boolescher Wert, Objekt, Array | Der Wert, für den getestet werden soll, ob er `null` ist. |
| Weitere Operanden | Zeichenfolge, ganze Zahl, boolescher Wert, Objekt, Array | Der Wert, für den getestet werden soll, ob er `null` ist. |

### <a name="return-value"></a>Rückgabewert

Gibt den ersten Wert zurück, der ungleich NULL ist. Für leere Zeichenfolgen, Arrays und Objekte wird der Wert \<empty> zurückgegeben, da sie nicht `null` sind.

### <a name="example"></a>Beispiel

Die Ausgabeanweisungen geben die Werte zurück, die ungleich NULL sind. Der Ausgabetyp muss mit dem Typ im Vergleich übereinstimmen. Andernfalls wird ein Fehler generiert.

```bicep
param myObject object = {
  'isnull1': null
  'isnull2': null
  'string': 'demoString'
  'emptystr': ''
  'integer': 10
  }

output nonNullStr string = myObject.isnull1 ?? myObject.string ?? myObject.isnull2
output nonNullInt int = myObject.isnull1 ?? myObject.integer ?? myObject.isnull2
output nonNullEmpty string = myObject.isnull1 ?? myObject.emptystr ?? myObject.string ?? myObject.isnull2
```

Ausgabe des Beispiels:

| Name | type | Wert |
| ---- | ---- | ---- |
| `nonNullStr` | Zeichenfolge | demoString |
| `nonNullInt` | INT | 10 |
| `nonNullEmpty` | Zeichenfolge | \<empty> |

## <a name="conditional-expression--"></a>Bedingter Ausdruck ? :

`condition ? true-value : false-value`

Wertet eine Bedingung aus und gibt einen Wert zurück, der angibt, ob die Bedingung wahr (TRUE) oder nicht wahr (FALSE) ist.

### <a name="operands"></a>Operanden

| Operand | type | BESCHREIBUNG |
| ---- | ---- | ---- |
| `condition` | boolean | Bedingung, die als wahr oder falsch ausgewertet werden soll. |
| `true-value` | Zeichenfolge, ganze Zahl, boolescher Wert, Objekt, Array | Wert, wenn die Bedingung wahr ist. |
| `false-value` | Zeichenfolge, ganze Zahl, boolescher Wert, Objekt, Array | Wert, wenn die Bedingung nicht wahr ist. |

### <a name="example"></a>Beispiel

In diesem Beispiel wird der Anfangswert eines Parameters ausgewertet und ein Wert zurückgegeben, der angibt, ob die Bedingung wahr oder nicht wahr ist.

```bicep
param initValue bool = true

output outValue string = initValue ? 'true value' : 'false value'
```

Ausgabe des Beispiels:

| Name | type | Wert |
| ---- | ---- | ---- |
| `outValue` | Zeichenfolge | true value |

## <a name="next-steps"></a>Nächste Schritte

- Informationen zum Erstellen einer Bicep-Datei finden Sie unter [Tutorial: Erstellen und Bereitstellen Ihrer ersten Azure Resource Manager-Bicep-Datei](bicep-tutorial-create-first-bicep.md).
- Informationen zum Beheben von Bicep-Typfehlern finden Sie unter [any-Funktion für Bicep](template-functions-any.md).
- Einen Vergleich der Syntax für Bicep und JSON finden Sie unter [Vergleichen von JSON mit Bicep für Vorlagen](compare-template-syntax.md).
- Beispiele für Bicep- und ARM-Vorlagenfunktionen finden Sie unter [ARM-Vorlagenfunktionen](template-functions.md).
