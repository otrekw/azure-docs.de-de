---
title: 'Muster: Logische Operatoren in einer Richtliniendefinition'
description: Dieses Azure Policy-Muster enthält Beispiele für die Verwendung der logischen Operatoren in einer Richtliniendefinition.
ms.date: 01/31/2020
ms.topic: sample
ms.openlocfilehash: 8e57efaea81848c6b2d0188dbf3f91e06ed74c67
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/24/2020
ms.locfileid: "77170238"
---
# <a name="azure-policy-pattern-logical-operators"></a>Azure Policy-Muster: Logische Operatoren

Eine Richtliniendefinition kann mehrere Bedingungsanweisungen enthalten. In manchen Szenarien müssen alle Anweisungen „true“ sein, in anderen nur einige. Zur Unterstützung dieser Anforderungen verfügt die Sprache über [logische Operatoren](../concepts/definition-structure.md#logical-operators) für **not** (nicht), **allOf** (alle) und **anyOf** (beliebige). Sie sind optional und können geschachtelt werden, um komplexe Szenarien zu erstellen.

## <a name="sample-1-one-logical-operator"></a>Beispiel 1: Einzelner logischer Operator

Mit der folgenden Richtliniendefinition werden Cosmos DB-Konten ausgewertet, um zu ermitteln, ob automatische Failover und mehrere Schreibstandorte konfiguriert sind. Falls nicht, wird [audit](../concepts/effects.md#audit) ausgelöst und ein Protokolleintrag erstellt, wenn die nicht konforme Ressource erstellt oder aktualisiert wird.

:::code language="json" source="~/policy-templates/patterns/pattern-logical-operators-1.json":::

### <a name="sample-1-explanation"></a>Beispiel 1: Erklärung

:::code language="json" source="~/policy-templates/patterns/pattern-logical-operators-1.json" range="6-22" highlight="3":::

Im Block **policyRule.if** wird ein einzelner logischer Operator vom Typ **allOf** verwendet, um sicherzustellen, dass alle drei Bedingungen erfüllt sind.
Die Auswirkung **audit** wird nur ausgelöst, wenn alle diese Bedingungen als „true“ ausgewertet werden.

## <a name="sample-2-multiple-logical-operators"></a>Beispiel 2: Mehrere logische Operatoren

Mit der folgenden Richtliniendefinition wird das Benennungsmuster von Ressourcen ausgewertet. Entspricht eine Ressource nicht dem Muster, wird Sie [abgelehnt](../concepts/effects.md#deny).

:::code language="json" source="~/policy-templates/patterns/pattern-logical-operators-2.json":::

### <a name="sample-2-explanation"></a>Beispiel 2: Erklärung

:::code language="json" source="~/policy-templates/patterns/pattern-logical-operators-2.json" range="7-21" highlight="2,3,9":::

Der Block **policyRule.if** in diesem Beispiel enthält ebenfalls einen einzelnen logischen Operator vom Typ **allOf**, die Bedingungen sind jedoch jeweils von dem logischen Operator **not** umschlossen. Die Bedingung innerhalb des logischen Operators **not** wird zuerst ausgewertet. Danach wird der logische Operator **not** ausgewertet, um zu ermitteln, ob die gesamte Klausel „true“ oder „false“ ist. Werden beide logischen Operatoren vom Typ **not** als „true“ ausgewertet, wird die Richtlinienauswirkung ausgelöst.

## <a name="next-steps"></a>Nächste Schritte

- Sehen Sie sich andere [Muster und integrierte Definitionen](./index.md) an.
- Lesen Sie die Informationen unter [Struktur von Azure Policy-Definitionen](../concepts/definition-structure.md).
- Lesen Sie [Grundlegendes zu Richtlinienauswirkungen](../concepts/effects.md).