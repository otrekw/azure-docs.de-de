---
title: 'Muster: Der Operator „count“ in einer Richtliniendefinition'
description: Dieses Azure Policy-Muster enthält ein Beispiel für die Verwendung des Operators „count“ in einer Richtliniendefinition.
ms.date: 03/31/2021
ms.topic: sample
ms.openlocfilehash: dc2914028887ae5a91e3379e2a94ddbc57a7cef3
ms.sourcegitcommit: 99fc6ced979d780f773d73ec01bf651d18e89b93
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/31/2021
ms.locfileid: "106093451"
---
# <a name="azure-policy-pattern-the-count-operator"></a>Azure Policy-Muster: Der Operator „count“

Mit dem Operator [count](../concepts/definition-structure.md#count) werden Mitglieder eines Alias vom Typ \[\*\] ausgewertet.

## <a name="sample-policy-definition"></a>Beispielrichtliniendefinition

Mit der folgenden Richtliniendefinition wird eine [Überwachung](../concepts/effects.md#audit) von Netzwerksicherheitsgruppen durchgeführt, die so konfiguriert sind, dass eingehender RDP-Datenverkehr (Remotedesktopprotokoll) zugelassen wird.

:::code language="json" source="~/policy-templates/patterns/pattern-count-operator.json":::

### <a name="explanation"></a>Erklärung

Die Kernkomponenten des Operators **count** sind _field_, _where_ und die Bedingung. Sie sind jeweils im folgenden Codeausschnitt hervorgehoben.

- _field_ dient zum Angeben des [Alias](../concepts/definition-structure.md#aliases), dessen Mitglieder ausgewertet werden sollen. In diesem Beispiel wird das Alias-_Array_ **securityRules\[\*\]** der Netzwerksicherheitsgruppe betrachtet.
- Mithilfe von _where_ wird unter Verwendung der Richtliniensprache definiert, welche Mitglieder des _Arrays_ die Kriterien erfüllen. In diesem Beispiel werden mithilfe eines logischen Operators vom Typ **allOf** drei verschiedene Bedingungsauswertungen der Eigenschaften des Alias-_Arrays_ gruppiert: _direction_, _access_ und _destinationPortRange_.
- Die count-Bedingung in diesem Beispiel lautet **greater**. „count“ wird als „true“ ausgewertet, wenn mindestens ein Mitglied des Alias-_Arrays_ der _where_-Klausel entspricht.

:::code language="json" source="~/policy-templates/patterns/pattern-count-operator.json" range="12-32" highlight="3,4,20":::

## <a name="next-steps"></a>Nächste Schritte

- Sehen Sie sich andere [Muster und integrierte Definitionen](./index.md) an.
- Lesen Sie die Informationen unter [Struktur von Azure Policy-Definitionen](../concepts/definition-structure.md).
- Lesen Sie [Grundlegendes zu Richtlinienauswirkungen](../concepts/effects.md).