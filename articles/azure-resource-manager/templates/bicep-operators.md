---
title: Bicep-Operatoren
description: Hier werden die für Azure Resource Manager-Bereitstellungen verfügbaren Bicep-Operatoren beschrieben.
ms.topic: conceptual
ms.date: 04/07/2021
ms.openlocfilehash: 4bf1005a11b1dcfea9f4b28d6bd3fa7c33e3278f
ms.sourcegitcommit: c3739cb161a6f39a9c3d1666ba5ee946e62a7ac3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/08/2021
ms.locfileid: "107211245"
---
# <a name="bicep-operators"></a>Bicep-Operatoren

In diesem Artikel werden die Bicep-Operatoren beschrieben, die verfügbar sind, wenn Sie eine Bicep-Vorlage erstellen und Ressourcen mit Azure Resource Manager bereitstellen. Operatoren werden verwendet, um Werte zu berechnen, Werte zu vergleichen oder Bedingungen auszuwerten. Es gibt drei Typen von Bicep-Operatoren: [Vergleichsoperatoren](#comparison), [logische](#logical) und [numerische](#numeric) Operatoren.

## <a name="comparison"></a>Vergleich

Die Vergleichsoperatoren vergleichen Werte und geben entweder `true` oder `false` zurück.

| Operator | Name | BESCHREIBUNG |
| ---- | ---- | ---- |
| `>=` | [Größer oder gleich](bicep-operators-comparison.md#greater-than-or-equal-) | Wertet aus, ob der erste Wert größer oder gleich dem zweiten Wert ist. |
| `>`  | [Größer als](bicep-operators-comparison.md#greater-than-) | Wertet aus, ob der erste Wert größer ist als der zweite Wert. |
| `<=` | [Kleiner oder gleich](bicep-operators-comparison.md#less-than-or-equal-) | Wertet aus, ob der erste Wert kleiner oder gleich dem zweiten Wert ist. |
| `<`  | [Kleiner als](bicep-operators-comparison.md#less-than-) | Wertet aus, ob der erste Wert kleiner ist als der zweite Wert. |
| `==` | [Ist gleich](bicep-operators-comparison.md#equals-) | Wertet aus, ob zwei Werte gleich sind. |
| `!=` | [Ungleich](bicep-operators-comparison.md#not-equal-) | Wertet aus, ob zwei Werte **nicht** gleich sind. |
| `=~` | [Gleiche Groß-/Kleinschreibung wird nicht berücksichtigt](bicep-operators-comparison.md#equal-case-insensitive-) | Ignoriert die Groß-/Kleinschreibung, um zu ermitteln, ob zwei Werte gleich sind. |
| `!~` | [Ungleiche Groß-/Kleinschreibung wird nicht berücksichtigt](bicep-operators-comparison.md#not-equal-case-insensitive-) | Ignoriert die Groß-/Kleinschreibung, um zu ermitteln, ob zwei Werte **nicht** gleich sind. |

## <a name="logical"></a>Logisch

Die logischen Operatoren werten boolesche Werte aus, geben Werte zurück, die ungleich NULL sind, oder werten einen bedingten Ausdruck aus.

| Operator | Name | BESCHREIBUNG |
| ---- | ---- | ---- |
| `&&` | [Und](bicep-operators-logical.md#and-) | Gibt `true` zurück, wenn alle Werte WAHR sind. |
| `||`| [Oder](bicep-operators-logical.md#or-) | Gibt `true` zurück, wenn einer der beiden Werte WAHR ist. |
| `!` | [Not](bicep-operators-logical.md#not-) | Negiert einen booleschen Wert. |
| `??` | [Koaleszieren (COALESCE)](bicep-operators-logical.md#coalesce-) | Gibt den ersten Wert zurück, der ungleich NULL ist. |
| `?` `:` | [Bedingter Ausdruck](bicep-operators-logical.md#conditional-expression--) | Wertet eine Bedingung auf WAHR oder FALSCH aus und gibt einen Wert zurück. |

## <a name="numeric"></a>Numeric

Die numerischen Operatoren verwenden zur Ausführung von Berechnungen ganze Zahlen und geben ganzzahlige Werte zurück.

| Operator | Name | BESCHREIBUNG |
| ---- | ---- | ---- |
| `*` | [Multiplizieren](bicep-operators-numeric.md#multiply-) | Multipliziert zwei ganze Zahlen. |
| `/` | [Dividieren](bicep-operators-numeric.md#divide-) | Dividiert eine ganze Zahl durch eine ganze Zahl. |
| `%` | [Modulo](bicep-operators-numeric.md#modulo-) | Dividiert eine ganze Zahl durch eine ganze Zahl und gibt den Rest zurück. |
| `+` | [Add (Hinzufügen)](bicep-operators-numeric.md#add-) | Addiert zwei ganze Zahlen. |
| `-` | [Subtrahieren](bicep-operators-numeric.md#subtract--) | Subtrahiert eine ganze Zahl von einer ganzen Zahl. |
| `-` | [Minus](bicep-operators-numeric.md#minus--) | Multipliziert eine ganze Zahl mit `-1`. |

> [!NOTE]
> Subtrahieren und Minus verwenden denselben Operator. Die Funktionalität ist unterschiedlich, weil Subtrahieren zwei Operanden und Minus einen Operanden verwendet.

## <a name="next-steps"></a>Nächste Schritte

- Informationen zum Erstellen einer Bicep-Datei finden Sie unter [Tutorial: Erstellen und Bereitstellen Ihrer ersten Azure Resource Manager-Bicep-Datei](bicep-tutorial-create-first-bicep.md).
- Informationen zum Beheben von Bicep-Typfehlern finden Sie unter [any-Funktion für Bicep](template-functions-any.md).
- Informationen zum Vergleichen der Syntax für Bicep und JSON finden Sie unter [Vergleichen von JSON mit Bicep für Vorlagen](compare-template-syntax.md).
- Beispiele für Bicep- und ARM-Vorlagenfunktionen finden Sie unter [ARM-Vorlagenfunktionen](template-functions.md).
