---
title: Bicep-Operatoren
description: Hier werden die für Azure Resource Manager-Bereitstellungen verfügbaren Bicep-Operatoren beschrieben.
author: mumian
ms.author: jgao
ms.topic: conceptual
ms.date: 06/01/2021
ms.openlocfilehash: 154a42d1bcdc78eee63241286e8f65ab7777bc6b
ms.sourcegitcommit: 7f59e3b79a12395d37d569c250285a15df7a1077
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/02/2021
ms.locfileid: "111026449"
---
# <a name="bicep-operators"></a>Bicep-Operatoren

In diesem Artikel werden die Bicep-Operatoren beschrieben, die verfügbar sind, wenn Sie eine Bicep-Vorlage erstellen und Ressourcen mit Azure Resource Manager bereitstellen. Operatoren werden verwendet, um Werte zu berechnen, Werte zu vergleichen oder Bedingungen auszuwerten. Es gibt drei Typen von Bicep-Operatoren:

- [Vergleich](#comparison)
- [Logisch](#logical)
- [numeric](#numeric)

Wenn Sie einen Ausdruck zwischen `(` und `)` einschließen, können Sie die Standardrangfolge des Bicep-Operators überschreiben. Beispielsweise wertet der Ausdruck x + y / z zuerst die Division und dann die Addition aus. Der Ausdruck (x + y) / z wertet jedoch die Addition zuerst und die Division danach aus.

Informationen zum Zugreifen auf eine Ressource über den Operator `::` finden Sie unter [Festlegen von Name und Typ für untergeordnete Ressourcen in Bicep](child-resource-name-type.md).

## <a name="comparison"></a>Vergleich

Die Vergleichsoperatoren vergleichen Werte und geben entweder `true` oder `false` zurück.

| Operator | Name | Beschreibung |
| ---- | ---- | ---- |
| `>=` | [Größer oder gleich](./operators-comparison.md#greater-than-or-equal-) | Wertet aus, ob der erste Wert größer oder gleich dem zweiten Wert ist. |
| `>`  | [Größer als](./operators-comparison.md#greater-than-) | Wertet aus, ob der erste Wert größer ist als der zweite Wert. |
| `<=` | [Kleiner oder gleich](./operators-comparison.md#less-than-or-equal-) | Wertet aus, ob der erste Wert kleiner oder gleich dem zweiten Wert ist. |
| `<`  | [Kleiner als](./operators-comparison.md#less-than-) | Wertet aus, ob der erste Wert kleiner ist als der zweite Wert. |
| `==` | [Ist gleich](./operators-comparison.md#equals-) | Wertet aus, ob zwei Werte gleich sind. |
| `!=` | [Ungleich](./operators-comparison.md#not-equal-) | Wertet aus, ob zwei Werte **nicht** gleich sind. |
| `=~` | [Gleiche Groß-/Kleinschreibung wird nicht berücksichtigt](./operators-comparison.md#equal-case-insensitive-) | Ignoriert die Groß-/Kleinschreibung, um zu ermitteln, ob zwei Werte gleich sind. |
| `!~` | [Ungleiche Groß-/Kleinschreibung wird nicht berücksichtigt](./operators-comparison.md#not-equal-case-insensitive-) | Ignoriert die Groß-/Kleinschreibung, um zu ermitteln, ob zwei Werte **nicht** gleich sind. |

## <a name="logical"></a>Logisch

Die logischen Operatoren werten boolesche Werte aus, geben Werte zurück, die ungleich NULL sind, oder werten einen bedingten Ausdruck aus.

| Operator | Name | Beschreibung |
| ---- | ---- | ---- |
| `&&` | [Und](./operators-logical.md#and-) | Gibt `true` zurück, wenn alle Werte WAHR sind. |
| `||`| [Oder](./operators-logical.md#or-) | Gibt `true` zurück, wenn einer der beiden Werte WAHR ist. |
| `!` | [Not](./operators-logical.md#not-) | Negiert einen booleschen Wert. |
| `??` | [Koaleszieren (COALESCE)](./operators-logical.md#coalesce-) | Gibt den ersten Wert zurück, der ungleich NULL ist. |
| `?` `:` | [Bedingter Ausdruck](./operators-logical.md#conditional-expression--) | Wertet eine Bedingung auf WAHR oder FALSCH aus und gibt einen Wert zurück. |

## <a name="numeric"></a>Numeric

Die numerischen Operatoren verwenden zur Ausführung von Berechnungen ganze Zahlen und geben ganzzahlige Werte zurück.

| Operator | Name | Beschreibung |
| ---- | ---- | ---- |
| `*` | [Multiplizieren](./operators-numeric.md#multiply-) | Multipliziert zwei ganze Zahlen. |
| `/` | [Dividieren](./operators-numeric.md#divide-) | Dividiert eine ganze Zahl durch eine ganze Zahl. |
| `%` | [Modulo](./operators-numeric.md#modulo-) | Dividiert eine ganze Zahl durch eine ganze Zahl und gibt den Rest zurück. |
| `+` | [Add (Hinzufügen)](./operators-numeric.md#add-) | Addiert zwei ganze Zahlen. |
| `-` | [Subtrahieren](./operators-numeric.md#subtract--) | Subtrahiert eine ganze Zahl von einer ganzen Zahl. |
| `-` | [Minus](./operators-numeric.md#minus--) | Multipliziert eine ganze Zahl mit `-1`. |

> [!NOTE]
> Subtrahieren und Minus verwenden denselben Operator. Die Funktionalität ist unterschiedlich, weil Subtrahieren zwei Operanden und Minus einen Operanden verwendet.

## <a name="next-steps"></a>Nächste Schritte

- Informationen zum Erstellen einer Bicep-Datei finden Sie unter [Schnellstart: Erstellen von Bicep-Dateien mit Visual Studio Code](./quickstart-create-bicep-use-visual-studio-code.md).
- Informationen zum Beheben von Bicep-Typfehlern finden Sie unter [any-Funktion für Bicep](./bicep-functions-any.md).
- Einen Vergleich der Syntax für Bicep und JSON finden Sie unter [Vergleichen von JSON mit Bicep für Vorlagen](./compare-template-syntax.md).
- Beispiele zu Bicep-Funktionen finden Sie unter [Bicep-Funktionen](./bicep-functions.md).
