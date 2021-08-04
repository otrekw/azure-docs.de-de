---
title: 'Format und Syntax von Azure-Rollenzuweisungsbedingungen (Vorschau): Azure RBAC'
description: Hier erhalten Sie einen Überblick über das Format und die Syntax von Azure-Rollenzuweisungsbedingungen für die attributbasierte Zugriffssteuerung in Azure (Azure ABAC).
services: active-directory
author: rolyon
manager: mtillman
ms.service: role-based-access-control
ms.subservice: conditions
ms.topic: conceptual
ms.workload: identity
ms.date: 05/06/2021
ms.author: rolyon
ms.openlocfilehash: b271da3cf8e591df8557133abcff248a737645e5
ms.sourcegitcommit: 1fbd591a67e6422edb6de8fc901ac7063172f49e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2021
ms.locfileid: "109489656"
---
# <a name="azure-role-assignment-condition-format-and-syntax-preview"></a>Format und Syntax von Azure-Rollenzuweisungsbedingungen (Vorschau)

> [!IMPORTANT]
> Azure ABAC und Azure-Rollenzuweisungsbedingungen befinden sich derzeit in der Vorschauphase.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar.
> Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Eine Bedingung ist eine zusätzliche Überprüfung, die Sie Ihrer Rollenzuweisung optional hinzufügen können, um eine präzisere Zugriffssteuerung zu ermöglichen. So können Sie beispielsweise eine Bedingung hinzufügen, die festlegt, dass ein Objekt über ein bestimmtes Tag verfügen muss, damit das Objekt gelesen werden kann. In diesem Artikel werden das Format und die Syntax von Rollenzuweisungsbedingungen beschrieben.

## <a name="condition-format"></a>Bedingungsformat

Um Rollenzuweisungsbedingungen besser zu verstehen, ist es hilfreich, sich das Format anzusehen.

### <a name="simple-condition"></a>Einfache Bedingung

Die einfachste Bedingung besteht aus einer zielorientierten Aktion und einem Ausdruck. Eine Aktion ist ein Vorgang, den ein Benutzer für einen Ressourcentyp ausführen kann. Ein Ausdruck ist eine Anweisung, deren Auswertung „true“ oder „false“ ergibt. Dadurch wird bestimmt, ob die Aktion ausgeführt werden darf.

Nachfolgend ist das Format einer einfachen Bedingung gezeigt:

![Format einer einfachen Bedingung mit einer einzelnen Aktion und einem einzelnen Ausdruck](./media/conditions-format/format-simple.png)

Die folgende Bedingung verfügt über die Aktion „Blob lesen“. Der Ausdruck überprüft, ob der Containername „blobs-example-container“ lautet.

![Beispiel für eine einfache Bedingung mit einer Aktion zum Lesen von Blobs und einem Ausdruck für den Containernamen](./media/conditions-format/format-simple-example.png)

![Diagramm: Lesezugriff auf Blobs mit einem bestimmten Containernamen](./media/conditions-format/format-simple-example-diagram.png)

### <a name="how-a-condition-is-evaluated"></a>Auswertung einer Bedingung

Wenn ein Benutzer versucht, in der Rollenzuweisung eine andere Aktion als `<action>` auszuführen, ergeben die Auswertung von `!(ActionMatches)` und die Auswertung der gesamten Bedingung „true“, damit die Aktion ausgeführt werden kann.

Wenn ein Benutzer versucht, `<action>` in der Rollenzuweisung auszuführen, ergibt die Auswertung von `!(ActionMatches)` „false“, sodass der Ausdruck ausgewertet wird. Ergibt die Auswertung des Ausdrucks „true“, ergibt die Auswertung der gesamten Bedingung „true“, damit `<action>` ausgeführt werden kann. Andernfalls darf `<action>` nicht ausgeführt werden.

Der folgende Pseudocode zeigt eine andere Möglichkeit zum Lesen dieser Bedingung:

```
if a user tries to perform an action in the role assignment that does not match <action>
{
    Allow action to be performed
}
else
{
    if <attribute> <operator> <value> is true
    {
        Allow <action> to be performed
    }
    else
    {
        Do not allow <action> to be performed
    }
}
```

### <a name="suboperations"></a>Untervorgänge

Einige Aktionen verfügen über Untervorgänge. Die Datenaktion „Blob lesen“ verfügt beispielsweise über den Untervorgang „Inhalt aus einem Blob mit Tagbedingungen lesen“. Bedingungen mit Untervorgängen haben das folgende Format:

![Format für eine Aktion mit einem Untervorgang](./media/conditions-format/format-suboperation.png)

### <a name="multiple-actions"></a>Mehrere Aktionen

Eine Bedingung kann mehrere Aktionen enthalten, die Sie zulassen möchten, wenn die Bedingung „true“ lautet. Wenn Sie mehrere Aktionen für eine einzelne Bedingung auswählen, stehen für Ihre Bedingung möglicherweise weniger Attribute zur Auswahl, weil die Attribute für alle ausgewählten Aktionen verfügbar sein müssen.

![Format zum Zulassen mehrerer Aktionen, wenn die Bedingung „true“ lautet](./media/conditions-format/format-multiple-actions.png)

### <a name="multiple-expressions"></a>Mehrere Ausdrücke

Eine Bedingung kann mehrere Ausdrücke enthalten. Je nach Operator können Attribute anhand mehrerer Werte überprüft werden.

![Format für mehrere Ausdrücke mit booleschen Operatoren und mehreren Werten](./media/conditions-format/format-multiple-expressions.png)

### <a name="multiple-conditions"></a>Mehrere Bedingungen

Sie können Bedingungen auch kombinieren, um mehrere Aktionen einzubeziehen.

![Format für mehrere Bedingungen mit booleschem Operator](./media/conditions-format/format-multiple-conditions.png)

## <a name="condition-syntax"></a>Bedingungssyntax

Im Folgenden wird die Syntax für eine Rollenzuweisungsbedingung veranschaulicht:

```
(
    (
        !(ActionMatches{'<action>'} AND @Request[subOperation] ForAnyOfAnyValues:StringEqualsIgnoreCase {'<subOperation>'})
        AND
        !(ActionMatches{'<action>'} AND @Request[subOperation] ForAnyOfAnyValues:StringEqualsIgnoreCase {'<subOperation>'})
        AND
        ...
    )
    OR
    (
        <attribute> <operator> {<value, <value>, ...}
        AND | OR
        <attribute> <operator> {<value>, <value>, ...}
        AND | OR
        ...
    )
)
AND
(
    (
        !(ActionMatches{'<action>'} AND @Request[subOperation] ForAnyOfAnyValues:StringEqualsIgnoreCase {'<subOperation>'})
        AND
        !(ActionMatches{'<action>'} AND @Request[subOperation] ForAnyOfAnyValues:StringEqualsIgnoreCase {'<subOperation>'})
        AND
        ...
    )
    OR
    (
        <attribute> <operator> {<value, <value>, ...}
        AND | OR
        <attribute> <operator> {<value>, <value>, ...}
        AND | OR
        ...
    )
)
AND
...
```

## <a name="actions"></a>Aktionen

Derzeit können Bedingungen zu integrierten oder benutzerdefinierten Rollenzuweisungen hinzugefügt werden, die über Aktionen für Speicherblobdaten verfügen. Dies umfasst die folgenden integrierten Rollen:

- [Mitwirkender an Speicherblobdaten](built-in-roles.md#storage-blob-data-contributor)
- [Besitzer von Speicherblobdaten](built-in-roles.md#storage-blob-data-owner)
- [Leser von Speicherblobdaten](built-in-roles.md#storage-blob-data-reader)

Eine Liste der Speicherblobaktionen, die Sie in Bedingungen verwenden können, finden Sie unter [Aktionen und Attribute für Azure-Rollenzuweisungsbedingungen in Azure Storage (Vorschau)](../storage/common/storage-auth-abac-attributes.md).

## <a name="attributes"></a>Attribute

Abhängig von den ausgewählten Aktionen befindet sich das Attribut unter Umständen an verschiedenen Orten. Wenn Sie mehrere Aktionen für eine einzelne Bedingung auswählen, stehen für Ihre Bedingung möglicherweise weniger Attribute zur Auswahl, weil die Attribute für alle ausgewählten Aktionen verfügbar sein müssen. Zum Angeben eines Attributs müssen Sie die Quelle als Präfix aufnehmen.

> [!div class="mx-tableFixed"]
> | Attributquelle | BESCHREIBUNG | Code |
> | --- | --- | --- |
> | Ressource | Gibt an, dass sich das Attribut in der Ressource (z. B. der Containername) befindet. | `@Resource` |
> | Anforderung | Gibt an, dass das Attribut Teil der Aktionsanforderung ist (z. B. das Festlegen des Blobindextags). | `@Request` |

Eine Liste der Speicherblobattribute, die Sie in Bedingungen verwenden können, finden Sie unter [Aktionen und Attribute für Azure-Rollenzuweisungsbedingungen in Azure Storage (Vorschau)](../storage/common/storage-auth-abac-attributes.md).

## <a name="operators"></a>Operatoren

In der folgenden Tabelle sind die Operatoren aufgeführt, die zum Erstellen von Bedingungen verfügbar sind:

| Category | Operator | BESCHREIBUNG |
| --- | --- | --- |
| Logischer Vergleich |`AND`<br/>`&&` | and-Operator |
|  | `OR`<br/>`||` | or-Operator |
|  | `NOT`<br/>`!` | not- oder Negationsoperator |
| Zeichenfolgenvergleich | `StringEquals`<br/>`StringEqualsIgnoreCase` | Beim Abgleich wird die Groß-/Kleinschreibung beachtet (oder die Groß-/Kleinschreibung nicht beachtet). Die Werte müssen genau mit der Zeichenfolge übereinstimmen. |
|  | `StringNotEquals`<br/>`StringNotEqualsIgnoreCase` | Negation des Operators `StringEquals` (oder `StringEqualsIgnoreCase`) |
|  | `StringStartsWith`<br/>`StringStartsWithIgnoreCase` | Beim Abgleich wird die Groß-/Kleinschreibung beachtet (oder die Groß-/Kleinschreibung nicht beachtet). Die Werte beginnen mit der Zeichenfolge. |
|  | `StringNotStartsWith`<br/>`StringNotStartsWithIgnoreCase` | Negation des Operators `StringStartsWith` (oder `StringStartsWithIgnoreCase`) |
|  | `StringLike`<br/>`StringLikeIgnoreCase` | Beim Abgleich wird die Groß-/Kleinschreibung beachtet (oder die Groß-/Kleinschreibung nicht beachtet). Die Werte können an beliebiger Stelle in der Zeichenfolge einen Platzhalter für eine mehrstellige Übereinstimmung (`*`) oder einen Platzhalter für eine einstellige Übereinstimmung (`?`) enthalten. Bei Bedarf können diese Zeichen mit einem umgekehrten Schrägstrich als Escapezeichen (`\*` bzw. `\?`) versehen werden. |
|  | `StringNotLike`<br/>`StringNotLikeIgnoreCase` | Negation des Operators `StringLike` (oder `StringLikeIgnoreCase`) |
| Numerischer Vergleich | `NumericEquals`<br/>`NumericNotEquals`<br/>`NumericLessThan`<br/>`NumericLessThanEquals`<br/>`NumericGreaterThan`<br/>`NumericGreaterThanEquals` | Derzeit werden nur ganze Zahlen unterstützt. |
| Funktionen auf höherer Ebene | `ActionMatches` | Überprüft, ob der Action[ID]-Wert dem angegebenen Aktionsmuster entspricht. Dieser Operator entspricht der Aktionsabgleichslogik, die das SDK verwendet, wenn eine Aktion mit einem Aktionsmuster innerhalb einer Berechtigung verglichen wird. |
| Produktübergreifender Vergleich | `ForAnyOfAnyValues:StringEquals`<br/>`ForAnyOfAnyValues:StringEqualsIgnoreCase`<br/>`ForAnyOfAnyValues:StringNotEquals`<br/>`ForAnyOfAnyValues:StringNotEqualsIgnoreCase`<br/>`ForAnyOfAnyValues:StringLike`<br/>`ForAnyOfAnyValues:StringLikeIgnoreCase`<br/>`ForAnyOfAnyValues:StringNotLike`<br/>`ForAnyOfAnyValues:StringNotLikeIgnoreCase`<br/>`ForAnyOfAnyValues:NumericEquals`<br/>`ForAnyOfAnyValues:NumericNotEquals`<br/>`ForAnyOfAnyValues:NumericGreaterThan`<br/>`ForAnyOfAnyValues:NumericGreaterThanEquals`<br/>`ForAnyOfAnyValues:NumericLessThan`<br/>`ForAnyOfAnyValues:NumericLessThanEquals` | Wenn mindestens ein Wert auf der linken Seite mindestens einem Wert auf der rechten Seite entspricht, ergibt die Auswertung des Ausdrucks „true“. Format: `ForAnyOfAnyValues:<BooleanFunction>`. Unterstützt mehrere Zeichenfolgen und Zahlen. |
|  | `ForAllOfAnyValues:StringEquals`<br/>`ForAllOfAnyValues:StringEqualsIgnoreCase`<br/>`ForAllOfAnyValues:StringNotEquals`<br/>`ForAllOfAnyValues:StringNotEqualsIgnoreCase`<br/>`ForAllOfAnyValues:StringLike`<br/>`ForAllOfAnyValues:StringLikeIgnoreCase`<br/>`ForAllOfAnyValues:StringNotLike`<br/>`ForAllOfAnyValues:StringNotLikeIgnoreCase`<br/>`ForAllOfAnyValues:NumericEquals`<br/>`ForAllOfAnyValues:NumericNotEquals`<br/>`ForAllOfAnyValues:NumericGreaterThan`<br/>`ForAllOfAnyValues:NumericGreaterThanEquals`<br/>`ForAllOfAnyValues:NumericLessThan`<br/>`ForAllOfAnyValues:NumericLessThanEquals` | Wenn jeder Wert auf der linken Seite mindestens einem Wert auf der rechten Seite entspricht, ergibt die Auswertung des Ausdrucks „true“. Format: `ForAllOfAnyValues:<BooleanFunction>`. Unterstützt mehrere Zeichenfolgen und Zahlen. |
|  | `ForAnyOfAllValues:StringEquals`<br/>`ForAnyOfAllValues:StringEqualsIgnoreCase`<br/>`ForAnyOfAllValues:StringNotEquals`<br/>`ForAnyOfAllValues:StringNotEqualsIgnoreCase`<br/>`ForAnyOfAllValues:StringLike`<br/>`ForAnyOfAllValues:StringLikeIgnoreCase`<br/>`ForAnyOfAllValues:StringNotLike`<br/>`ForAnyOfAllValues:StringNotLikeIgnoreCase`<br/>`ForAnyOfAllValues:NumericEquals`<br/>`ForAnyOfAllValues:NumericNotEquals`<br/>`ForAnyOfAllValues:NumericGreaterThan`<br/>`ForAnyOfAllValues:NumericGreaterThanEquals`<br/>`ForAnyOfAllValues:NumericLessThan`<br/>`ForAnyOfAllValues:NumericLessThanEquals` | Wenn mindestens ein Wert auf der linken Seite jedem Wert auf der rechten Seite entspricht, ergibt die Auswertung des Ausdrucks „true“. Format: `ForAnyOfAllValues:<BooleanFunction>`. Unterstützt mehrere Zeichenfolgen und Zahlen. |
|  | `ForAllOfAllValues:StringEquals`<br/>`ForAllOfAllValues:StringEqualsIgnoreCase`<br/>`ForAllOfAllValues:StringNotEquals`<br/>`ForAllOfAllValues:StringNotEqualsIgnoreCase`<br/>`ForAllOfAllValues:StringLike`<br/>`ForAllOfAllValues:StringLikeIgnoreCase`<br/>`ForAllOfAllValues:StringNotLike`<br/>`ForAllOfAllValues:StringNotLikeIgnoreCase`<br/>`ForAllOfAllValues:NumericEquals`<br/>`ForAllOfAllValues:NumericNotEquals`<br/>`ForAllOfAllValues:NumericGreaterThan`<br/>`ForAllOfAllValues:NumericGreaterThanEquals`<br/>`ForAllOfAllValues:NumericLessThan`<br/>`ForAllOfAllValues:NumericLessThanEquals` | Wenn jeder Wert auf der linken Seite jedem Wert auf der rechten Seite entspricht, ergibt die Auswertung des Ausdrucks „true“. Format: `ForAllOfAllValues:<BooleanFunction>`. Unterstützt mehrere Zeichenfolgen und Zahlen. |

## <a name="operator-examples"></a>Beispiele für Operatoren

> [!div class="mx-tableFixed"]
> | Beispiel | Ergebnis |
> | --- | --- |
> | `ActionMatches{'Microsoft.Authorization/roleAssignments/*'}` | Wenn die überprüfte Aktion „Microsoft.Authorization/roleAssignments/write“ entspricht, wird „true“ ausgegeben. |
> | `ActionMatches{'Microsoft.Authorization/roleDefinitions/*'}` | Wenn die überprüfte Aktion „Microsoft.Authorization/roleAssignments/write“ entspricht, wird „false“ ausgegeben. |
> | `Resource[name1] StringLike 'a*c?'` | Wenn „Resource[name1]“ der Zeichenfolge „abcd“ entspricht, wird „true“ ausgegeben. |
> | `Resource[name1] StringLike 'A*C?'` | Wenn „Resource[name1]“ der Zeichenfolge „abcd“ entspricht, wird „false“ ausgegeben. |
> | `Resource[name1] StringLike 'a*c'` | Wenn „Resource[name1]“ der Zeichenfolge „abcd“ entspricht, wird „false“ ausgegeben. |
> | `{'red', 'blue'} ForAnyOfAnyValues:StringEquals {'blue', 'green'}` | true |
> | `{'red', 'blue'} ForAnyOfAnyValues:StringEquals {'orange', 'green'}` | false |
> | `{'red', 'blue'} ForAllOfAnyValues:StringEquals {'orange', 'red', 'blue'}` | true |
> | `{'red', 'blue'} ForAllOfAnyValues:StringEquals {'red', 'green'}` | false |
> | `{10, 20} ForAnyOfAllValues:NumericLessThan {15, 18}` | true |
> | `{10, 20} ForAllOfAllValues:NumericLessThan {5, 15, 18}` | false |
> | `{10, 20} ForAllOfAllValues:NumericLessThan {25, 30}` | true |
> | `{10, 20} ForAllOfAllValues:NumericLessThan {15, 25, 30}` | false |

## <a name="special-characters"></a>Sonderzeichen

| Zeichen | BESCHREIBUNG |
| --- | --- |
| `*` | Ein Sternchen (*) steht für einen Platzhalter für eine mehrstellige Übereinstimmung und kann mit `Like`-Operatoren verwendet werden. Bei Bedarf können Sie ein Sternchen mit einem umgekehrten Schrägstrich als Escapezeichen (`\*`) versehen. |
| `?` | Ein Fragezeichen (?) steht für einen Platzhalter für eine einstellige Übereinstimmung und kann mit `Like`-Operatoren verwendet werden. Bei Bedarf können Sie ein Fragezeichen mit einem umgekehrten Schrägstrich als Escapezeichen (`\?`) versehen. |
| `$` | Ein Dollarzeichen ($) dient zum Trennen von Tagschlüsseln. Wenn in Azure PowerShell eine Zeichenfolge in doppelten Anführungszeichen (") ein Dollarzeichen enthält, müssen Sie ihr ein Graviszeichen (\`) voranstellen. Beispiel: ``tags:Project<`$key_case_sensitive`$>``. | 

## <a name="grouping-and-precedence"></a>Gruppierung und Rangfolge

Sie verwenden Klammern – `()` –, um Ausdrücke zu gruppieren und die Rangfolge in einer Bedingung zu definieren. Wenn Sie über drei oder mehr Ausdrücke für eine zielorientierte Aktion verfügen, müssen Sie Klammern hinzufügen, um die Reihenfolge zu definieren, in der die Ausdrücke ausgewertet werden. Ausdrücke in Klammern haben eine höhere Rangfolge. Angenommen, Sie verfügen über den folgenden Ausdruck:

```
a AND b OR c
```

Sie müssen wie folgt Klammern hinzufügen:

```
(a AND b) OR c
```

```
a AND (b OR c)
```

## <a name="next-steps"></a>Nächste Schritte

- [Beispiele für Azure-Rollenzuweisungsbedingungen (Vorschau)](../storage/common/storage-auth-abac-examples.md)
- [Aktionen und Attribute für Azure-Rollenzuweisungsbedingungen in Azure Storage (Vorschau)](../storage/common/storage-auth-abac-attributes.md)
