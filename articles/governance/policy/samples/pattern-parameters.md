---
title: 'Muster: Parameter in einer Richtliniendefinition'
description: Dieses Azure Policy-Muster enthält ein Beispiel für die Verwendung von Parametern in einer Richtliniendefinition.
ms.date: 01/31/2020
ms.topic: sample
ms.openlocfilehash: 4921bb216ef67b561bc8986cf48239e6448beafc
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/24/2020
ms.locfileid: "77170178"
---
# <a name="azure-policy-pattern-parameters"></a>Azure Policy-Muster: Parameter

Eine Richtliniendefinition kann mithilfe von [Parametern](../concepts/definition-structure.md#parameters) dynamisch gemacht werden, damit weniger Richtliniendefinitionen benötigt werden. Der Parameter wird im Rahmen der Richtlinienzuweisung definiert. Parameter verfügen über eine Reihe vordefinierter Eigenschaften, die den Parameter und dessen Verwendung beschreiben.

## <a name="sample-1-string-parameters"></a>Beispiel 1: Zeichenfolgenparameter

In dieser Richtliniendefinition werden zwei Parameter (**tagName** und **tagValue**) verwendet, um festzulegen, wonach die Richtlinienzuweisung in Ressourcen suchen soll. Mit diesem Format kann die Richtlinie für eine beliebige Kombination aus Tagname und Tagwert verwendet werden, ohne mehrere Richtliniendefinitionen verwalten zu müssen.

:::code language="json" source="~/policy-templates/patterns/pattern-parameters-1.json":::

### <a name="sample-1-explanation"></a>Beispiel 1: Erklärung

:::code language="json" source="~/policy-templates/patterns/pattern-parameters-1.json" range="8-13":::

In diesem Teil der Richtliniendefinition wird der Parameter **tagName** als Zeichenfolge (_String_) definiert und eine Beschreibung für dessen Verwendung angegeben.

Der Parameter wird dann im Block **policyRule.if** verwendet, um die Richtlinie dynamisch zu machen. Hier wird damit das auszuwertende Feld (ein Tag mit dem Wert **tagName**) definiert.

:::code language="json" source="~/policy-templates/patterns/pattern-parameters-1.json" range="22-27" highlight="3":::

## <a name="sample-2-array-parameters"></a>Beispiel 2: Arrayparameter

In dieser Richtliniendefinition wird ein einzelner Parameter (**listOfBandwidthinMbps**) verwendet, um zu überprüfen, ob die Bandbreiteneinstellung der ExpressRoute-Verbindungsressource mit einem der zulässigen Werte konfiguriert ist. Ist dies nicht der Fall, wird die Erstellung oder Aktualisierung der Ressource [verweigert](../concepts/effects.md#deny).

:::code language="json" source="~/policy-templates/patterns/pattern-parameters-2.json":::

### <a name="sample-2-explanation"></a>Beispiel 2: Erklärung

:::code language="json" source="~/policy-templates/patterns/pattern-parameters-2.json" range="6-12":::

In diesem Teil der Richtliniendefinition wird der Parameter **listOfBandwidthinMbps** als _Array_ definiert und eine Beschreibung für dessen Verwendung angegeben. Da es sich um ein _Array_ handelt, sind mehrere Werte für den Abgleich vorhanden.

Der Parameter wird dann im Block **policyRule.if** verwendet. Bei einem Parameter vom Typ _Array_ muss die [Bedingung](../concepts/definition-structure.md#conditions) **in** oder **notIn** des _Arrays_
verwendet werden.
In diesem Beispiel wird der Alias **serviceProvider.bandwidthInMbps** als einer der definierten Werte für den Abgleich verwendet.

:::code language="json" source="~/policy-templates/patterns/pattern-parameters-2.json" range="21-24" highlight="3":::

## <a name="next-steps"></a>Nächste Schritte

- Sehen Sie sich andere [Muster und integrierte Definitionen](./index.md) an.
- Lesen Sie die Informationen unter [Struktur von Azure Policy-Definitionen](../concepts/definition-structure.md).
- Lesen Sie [Grundlegendes zu Richtlinienauswirkungen](../concepts/effects.md).