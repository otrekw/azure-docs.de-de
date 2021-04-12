---
title: Einhaltung gesetzlicher Bestimmungen in Initiativendefinitionen
description: In diesem Artikel wird beschrieben, wie Sie mit einer Initiativendefinition Richtlinien nach gesetzlichen Bereichen (wie z. B. Zugriffssteuerung, Konfigurationsverwaltung usw.) gruppieren können.
ms.date: 03/31/2021
ms.topic: conceptual
ms.openlocfilehash: 08173cd94e0841fa7a18fd55d53aa97281724ede
ms.sourcegitcommit: 99fc6ced979d780f773d73ec01bf651d18e89b93
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/31/2021
ms.locfileid: "106094063"
---
# <a name="regulatory-compliance-in-azure-policy"></a>Einhaltung gesetzlicher Bestimmungen in Azure Policy

Die Funktion „Einhaltung gesetzlicher Bestimmungen“ in Azure Policy stellt integrierte Initiativendefinitionen bereit, um eine Liste der **Steuerungen** und **Compliancebereiche** basierend auf dem jeweiligen Zuständigkeitsbereich (_Kunde_, _Microsoft_, _Gemeinsam_) anzuzeigen.
Für Steuerungen im Zuständigkeitsbereich von Microsoft werden zusätzliche Details zu den Überwachungsergebnissen basierend auf Nachweisen von Drittanbietern sowie Implementierungsdetails zur Einhaltung der Bestimmungen bereitgestellt.
Von Microsoft verwaltete Steuerungen sind vom `type` [statisch](./definition-structure.md#type).

> [!NOTE]
> Die Funktion „Einhaltung gesetzlicher Bestimmungen“ ist als Vorschauversion verfügbar. Bei aktualisierten integrierten Funktionen werden die Steuerungen der Initiativen dem entsprechenden Konformitätsstandard zugeordnet. Vorhandene Initiativen zum Konformitätsstandard werden derzeit aktualisiert, sodass sie künftig die Einhaltung gesetzlicher Bestimmungen unterstützen.

## <a name="regulatory-compliance-defined"></a>Definition der Funktion „Einhaltung gesetzlicher Bestimmungen“

Die Einhaltung gesetzlicher Bestimmungen basiert auf der Komponente [Gruppierung](./initiative-definition-structure.md#policy-definition-groups) einer Initiativendefinition. Bei integrierten Funktionen definiert jede Gruppierung in einer Initiativendefinition einen Namen (**Steuerung**) und eine Kategorie (**Compliancebereich**) und stellt einen Verweis auf das Objekt [policyMetadata](./initiative-definition-structure.md#metadata-objects) bereit, das Informationen zu der jeweiligen **Steuerung** enthält. Bei einer Initiativendefinition der Funktion „Einhaltung gesetzlicher Bestimmungen“ muss die `category`-Eigenschaft auf **Einhaltung gesetzlicher Bestimmungen** festgelegt sein. Mit der Standardeinstellung von Initiativendefinition unterstützen Initiativen zur Einhaltung gesetzlicher Bestimmungen [Parameter](./initiative-definition-structure.md#parameters) zum Erstellen dynamischer Zuweisungen.

Kunden können eigene Initiativen zur Einhaltung gesetzlicher Bestimmungen erstellen. Diese Definitionen können neu erstellt oder aus vorhandenen integrierten Definitionen kopiert werden. Wenn Sie eine integrierte Initiativendefinition der Funktion „Einhaltung gesetzlicher Bestimmungen“ als Verweis verwenden, wird empfohlen, die Quelle der Definitionen im [GitHub-Repository von Azure Policy](https://github.com/Azure/azure-policy/tree/master/built-in-policies/policySetDefinitions/Regulatory%20Compliance) zu überwachen.

Informationen zum Verknüpfen einer benutzerdefinierten Initiative der Funktion „Einhaltung gesetzlicher Bestimmungen“ mit Ihrem Azure Security Center-Dashboard finden Sie unter [Azure Security Center: Verwenden von benutzerdefinierten Sicherheitsrichtlinien](../../../security-center/custom-security-policies.md).

## <a name="regulatory-compliance-in-portal"></a>Einhaltung gesetzlicher Bestimmungen im Portal

Wurde eine Initiativendefinition mithilfe von [Gruppen](./initiative-definition-structure.md#policy-definition-groups) erstellt, enthält die Detailseite **Compliance** für diese Initiative im Portal weitere Informationen. 

Der Seite wird eine neue Registerkarte mit den Namen **Steuerungen** hinzugefügt. Es kann nach **Compliancebereichen** gefiltert werden, und Richtliniendefinitionen werden nach dem `title`-Feld des **policyMetadata**-Objekts gruppiert. Jede Zeile stellt eine **Steuerung** mit dem jeweiligen Konformitätszustand dar, ihrem **Compliancebereich**, Informationen zur Zuständigkeit sowie der Anzahl der nicht konformen und konformen Richtliniendefinitionen dieser **Steuerung**.

:::image type="content" source="../media/regulatory-compliance/regulatory-compliance-overview.png" alt-text="Screenshot: Übersicht über die Einhaltung gesetzlicher Bestimmungen für die integrierte NIST SP 800-53 R4-Definition mit konformen und nicht konformen Steuerungen":::

Wenn Sie auf eine **Steuerung** klicken, wird eine Seite mit Details zur jeweiligen Steuerung geöffnet. Die **Übersichtsseite** enthält Informationen zu `description` und `requirements`. Auf der Registerkarte **Richtlinien** finden Sie die einzelnen Richtliniendefinitionen der Initiative, die zu dieser **Steuerung** gehören. Die Registerkarte **Ressourcenkonformität** bietet eine genaue Ansicht der einzelnen Ressourcen, die anhand einer Mitgliedsrichtlinie der aktuell angezeigten **Steuerung** ausgewertet werden.

> [!NOTE]
> Der Auswertungstyp **Von Microsoft verwaltet** stellt einen [statischen](./definition-structure.md#type) `type` von Richtliniendefinitionen dar.

:::image type="content" source="../media/regulatory-compliance/regulatory-compliance-policies.png" alt-text="Screenshot: Details zur Einhaltung gesetzlicher Bestimmungen für die Steuerung vom Typ „Schutz von Grenzen“ der integrierten NIST SP 800-53 R4-Definition":::

Wenn Sie von dieser **Steuerungsseite** zur Registerkarte **Ressourcenkonformität** wechseln, werden alle Ressourcen angezeigt, die in den Richtliniendefinitionen dieser **Steuerung** enthalten sind. Sie können nach Name oder ID, Konformitätszustand, Ressourcentyp sowie Speicherort filtern.

:::image type="content" source="../media/regulatory-compliance/regulatory-compliance-resources.png" alt-text="Screenshot: Details zur Ressourcenkonformität für die Steuerung vom Typ „Schutz von Grenzen“ der integrierten NIST SP 800-53 R4-Definition":::

## <a name="regulatory-compliance-in-sdk"></a>Einhaltung gesetzlicher Bestimmungen im SDK

Ist die Einhaltung gesetzlicher Bestimmungen für eine Initiativendefinition aktiviert, geben die Datensätze des Auswertungsscans, Ereignisse und SDKs für Richtlinienzustände jeweils weitere Eigenschaften zurück. Diese zusätzlichen Eigenschaften werden nach Konformitätszustand gruppiert und enthalten Informationen zur Anzahl der Gruppen in den einzelnen Zuständen.

Das folgende Codebeispiel zeigt die zusätzlichen Ergebnisse eines `summarize`-Aufrufs:

```json
"policyGroupDetails": [{
        "complianceState": "noncompliant",
        "count": 4
    },
    {
        "complianceState": "compliant",
        "count": 76
    }
]
```

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen finden Sie unter [Struktur der Initiativendefinition](./initiative-definition-structure.md).
- Sehen Sie sich die Beispiele unter [Azure Policy-Beispiele](../samples/index.md) an.
- Lesen Sie [Grundlegendes zu Richtlinienauswirkungen](./effects.md).
- Erfahren Sie, wie Sie [nicht konforme Ressourcen korrigieren](../how-to/remediate-resources.md) können.
