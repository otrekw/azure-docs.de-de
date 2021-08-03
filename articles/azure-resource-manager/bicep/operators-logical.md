---
title: Logische Bicep-Operatoren
description: Beschreibt logische Bicep-Operatoren, die Bedingungen auswerten.
author: mumian
ms.author: jgao
ms.topic: conceptual
ms.date: 06/01/2021
ms.openlocfilehash: 1f904ed82f9d5b73d3b570017c005d88388bd068
ms.sourcegitcommit: 7f59e3b79a12395d37d569c250285a15df7a1077
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/02/2021
ms.locfileid: "111026087"
---
# <a name="bicep-logical-operators"></a>Logische Bicep-Operatoren

Die logischen Operatoren werten boolesche Werte aus, geben Werte ungleich NULL zurück oder werten einen bedingten Ausdruck aus. Verwenden Sie zum Ausführen der Beispiele die Azure-Befehlszeilenschnittstelle (Azure CLI) oder Azure PowerShell, um [eine Bicep-Datei bereitzustellen](./quickstart-create-bicep-use-visual-studio-code.md#deploy-the-bicep-file).

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

| Name | Typ | Wert |
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

| Name | Typ | Wert |
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

| Name | Typ | Wert |
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

| Name | Typ | Wert |
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

| Name | Typ | Wert |
| ---- | ---- | ---- |
| `outValue` | Zeichenfolge | true value |

## <a name="next-steps"></a>Nächste Schritte

- Informationen zum Erstellen einer Bicep-Datei finden Sie unter [Schnellstart: Erstellen von Bicep-Dateien mit Visual Studio Code](./quickstart-create-bicep-use-visual-studio-code.md).
- Informationen zum Beheben von Bicep-Typfehlern finden Sie unter [any-Funktion für Bicep](./bicep-functions-any.md).
- Einen Vergleich der Syntax für Bicep und JSON finden Sie unter [Vergleichen von JSON mit Bicep für Vorlagen](./compare-template-syntax.md).
- Beispiele zu Bicep-Funktionen finden Sie unter [Bicep-Funktionen](./bicep-functions.md).
