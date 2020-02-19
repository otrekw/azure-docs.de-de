---
title: 'Muster: Bereitstellen von Ressourcen mit einer Richtliniendefinition'
description: Dieses Azure Policy-Muster enthält ein Beispiel für die Bereitstellung von Ressourcen mit einer Richtliniendefinition.
ms.date: 01/31/2020
ms.topic: sample
ms.openlocfilehash: a8b6528afbd21c7c667e48965574c9b48c403654
ms.sourcegitcommit: bdf31d87bddd04382effbc36e0c465235d7a2947
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/12/2020
ms.locfileid: "77169988"
---
# <a name="azure-policy-pattern-deploy-resources"></a>Azure Policy-Muster: Bereitstellen von Ressourcen

Die Auswirkung [deployIfNotExists](../concepts/effects.md#deployifnotexists) ermöglicht das Bereitstellen einer [Azure Resource Manager-Vorlage](../../../azure-resource-manager/templates/overview.md) beim Erstellen oder Aktualisieren einer nicht kompatiblen Ressource. Dieser Ansatz ist ggf. der Auswirkung [deny](../concepts/effects.md#deny) vorzuziehen, da er die Fortsetzung der Ressourcenerstellung ermöglicht, aber dafür sorgt, dass die erforderlichen Änderungen vorgenommen werden, um die Ressourcen kompatibel zu machen.

## <a name="sample-policy-definition"></a>Beispielrichtliniendefinition

In dieser Richtliniendefinition wird der Operator **field** (Feld) verwendet, um den Typ (`type`) der erstellten oder aktualisierten Ressource auszuwerten. Wenn es sich bei der Ressource um eine Ressource vom Typ _Microsoft.Network/virtualNetworks_ handelt, sucht die Richtlinie am Ort der neuen oder aktualisierten Ressource nach einer Network Watcher-Ressource. Sollte keine passende Network Watcher-Ressource gefunden werden, wird die Resource Manager-Vorlage bereitgestellt, um die fehlende Ressource zu erstellen.

:::code language="json" source="~/policy-templates/patterns/pattern-deploy-resources.json":::

### <a name="explanation"></a>Erklärung

#### <a name="existencecondition"></a>existenceCondition

:::code language="json" source="~/policy-templates/patterns/pattern-deploy-resources.json" range="18-23":::

Durch den Block **properties.policyRule.then.details** wird Azure Policy mitgeteilt, wonach im Zusammenhang mit der erstellten oder aktualisierten Ressource im Block **properties.policyRule.if** gesucht werden soll. In diesem Beispiel muss eine Network Watcher-Ressource in der Ressourcengruppe **networkWatcherRG** vorhanden sein, und das Feld (**field**) `location` muss dem Ort der neuen oder aktualisierten Ressource entsprechen. Die Funktion `field()` ermöglicht **existenceCondition** den Zugriff auf Eigenschaften der neuen oder aktualisierten Ressource – in diesem Fall: auf die Eigenschaft `location`.

#### <a name="roledefinitionids"></a>roleDefinitionIds

:::code language="json" source="~/policy-templates/patterns/pattern-deploy-resources.json" range="24-26":::

Durch die Eigenschaft **roleDefinitionIds** vom Typ _Array_ im Block **properties.policyRule.then.details** wird der Richtliniendefinition mitgeteilt, welche Rechte die verwaltete Identität zum Bereitstellen der enthaltenen Resource Manager-Vorlage benötigt. Diese Eigenschaft muss mit Rollen festgelegt werden, die über die für die Vorlagenbereitstellung benötigten Berechtigungen verfügen. Dabei muss allerdings das Prinzip der geringsten Rechte beachtet werden, und es dürfen nur die erforderlichen Vorgänge enthalten sein.

#### <a name="deployment-template"></a>Bereitstellungsvorlage

Der Bereitstellungsteil (**deployment**) der Richtliniendefinition enthält einen Eigenschaftenblock (**properties**), in dem drei Kernkomponenten definiert werden:

- Modus (**mode**): Diese Eigenschaft dient zum Festlegen des [Bereitstellungsmodus](../../../azure-resource-manager/templates/deployment-modes.md) der Vorlage.

- Vorlage (**template**): Diese Eigenschaft enthält die eigentliche Vorlage. In diesem Beispiel wird mithilfe des Vorlagenparameters **location** der Ort der neuen Network Watcher-Ressource festgelegt.

  :::code language="json" source="~/policy-templates/patterns/pattern-deploy-resources.json" range="30-44":::
  
- Parameter (**parameters**): Diese Eigenschaft dient zum Definieren von Parametern für die Vorlage (**template**). Die Parameternamen müssen den Definitionen in **template** entsprechen. In diesem Beispiel heißt der abzugleichende Parameter **location**. Der Wert von **location** verwendet erneut die Funktion `field()`, um den Wert der ausgewerteten Ressource zu erhalten. Hierbei handelt es sich um das virtuelle Netzwerk im Block **policyRule.if**.

  :::code language="json" source="~/policy-templates/patterns/pattern-deploy-resources.json" range="45-49":::

## <a name="next-steps"></a>Nächste Schritte

- Sehen Sie sich andere [Muster und integrierte Definitionen](./index.md) an.
- Lesen Sie die Informationen unter [Struktur von Azure Policy-Definitionen](../concepts/definition-structure.md).
- Lesen Sie [Grundlegendes zu Richtlinienauswirkungen](../concepts/effects.md).