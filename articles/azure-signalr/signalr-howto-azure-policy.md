---
title: Konformität mithilfe von Azure Policy
description: In diesem Artikel erfahren Sie, wie Sie integrierte Richtlinien in Azure Policy zuweisen, um die Konformität Ihrer Azure SignalR Service-Ressourcen zu überwachen.
author: JialinXin
ms.service: signalr
ms.topic: conceptual
ms.date: 06/17/2020
ms.author: jixin
ms.openlocfilehash: 018033d3a6123948191a7261f5a1ee2ae526e25a
ms.sourcegitcommit: 58d3b3314df4ba3cabd4d4a6016b22fa5264f05a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/02/2020
ms.locfileid: "89295017"
---
# <a name="audit-compliance-of-azure-signalr-service-resources-using-azure-policy"></a>Überwachen der Konformität von Azure SignalR Service-Ressourcen mithilfe von Azure Policy

[Azure Policy](../governance/policy/overview.md) ist ein Dienst in Azure, mit dem Sie Richtlinien erstellen, zuweisen und verwalten können. Mit diesen Richtlinien werden unterschiedliche Regeln und Auswirkungen für Ihre Ressourcen erzwungen, damit diese stets mit Ihren Unternehmensstandards und Vereinbarungen zum Servicelevel konform bleiben.

In diesem Artikel werden integrierte Richtlinien (Vorschauversion) für Azure SignalR Service vorgestellt. Mit diesen Richtlinien können Sie die Konformität neuer und vorhandener SignalR-Ressourcen überwachen.

Die Verwendung von Azure Policy ist gebührenfrei.

## <a name="built-in-policy-definitions"></a>Integrierte Richtliniendefinitionen

Für Azure SignalR Service gelten die folgenden Definitionen für integrierte Richtlinien:

[!INCLUDE [azure-policy-reference-policies-signalr](../../includes/policy/reference/bycat/policies-signalr.md)]

## <a name="assign-policy-definitions"></a>Zuweisen von Richtliniendefinitionen

* Weisen Sie Richtliniendefinitionen über das [Azure-Portal](../governance/policy/assign-policy-portal.md), die [Azure CLI](../governance/policy/assign-policy-azurecli.md), eine [Resource Manager-Vorlage](../governance/policy/assign-policy-template.md) oder die Azure Policy-SDKs zu.
* Beschränken Sie eine Richtlinienzuweisung auf eine Ressourcengruppe, ein Abonnement oder eine [Azure-Verwaltungsgruppe](../governance/management-groups/overview.md). SignalR-Richtlinienzuweisungen gelten für vorhandene und neue SignalR-Ressourcen innerhalb des Gültigkeitsbereichs.
* Sie können die [Richtliniendurchsetzung](../governance/policy/concepts/assignment-structure.md#enforcement-mode) jederzeit aktivieren oder deaktivieren.

> [!NOTE]
> Nach dem Zuweisen oder Aktualisieren einer Richtlinie dauert es einen Moment, bis die Zuweisung auf die Ressourcen innerhalb des festgelegten Bereichs angewendet wird. Lesen Sie dazu auch die Informationen zu [Richtlinienauswertungsauslösern](../governance/policy/how-to/get-compliance-data.md#evaluation-triggers).

## <a name="review-policy-compliance"></a>Überprüfen der Richtlinienkonformität

Greifen Sie über das Azure-Portal, die Azure-Befehlszeilentools oder die Azure Policy-SDKs auf die Informationen zur Konformität zu, die von Ihren Richtlinienzuweisungen generiert werden. Einzelheiten finden Sie unter [Abrufen von Compliancedaten von Azure-Ressourcen](../governance/policy/how-to/get-compliance-data.md).

Wenn eine Ressource nicht konform ist, kann das viele mögliche Ursachen haben. Wie Sie die Ursache bestimmen oder die verantwortliche Änderung finden, ist unter [Bestimmen der Nichtkonformität](../governance/policy/how-to/determine-non-compliance.md) beschrieben.

### <a name="policy-compliance-in-the-portal"></a>Richtlinienkonformität im Portal:

1. Wählen Sie **Alle Dienste** aus, und suchen Sie nach **Richtlinie**.
1. Wählen Sie **Compliance** aus.
1. Verwenden Sie die Filter, um die Konformitätszustände einzuschränken oder nach Richtlinien zu suchen.
   
    [ ![Richtlinienkonformität im Portal](./media/signalr-howto-azure-policy/azure-policy-compliance.png) ](./media/signalr-howto-azure-policy/azure-policy-compliance.png#lightbox)
2. Wählen Sie eine Richtlinie aus, um die Zusammenfassung der Konformitätsdetails und -ereignisse zu überprüfen. Wählen Sie anschließend bei Bedarf eine bestimmte SignalR-Ressource aus, deren Konformität überprüft werden soll.

### <a name="policy-compliance-in-the-azure-cli"></a>Richtlinienkonformität in der Azure CLI

Konformitätsdaten können auch über die Azure CLI abgerufen werden. Verwenden Sie beispielsweise den Befehl [az policy assignment list](/cli/azure/policy/assignment#az-policy-assignment-list) in der CLI, um die Richtlinien-IDs der Azure SignalR Service-Richtlinien abzurufen, die angewendet werden:

```azurecli
az policy assignment list --query "[?contains(displayName,'SignalR')].{name:displayName, ID:id}" --output table
```

Beispielausgabe:

```
Name                                                                                   ID
-------------------------------------------------------------------------------------  --------------------------------------------------------------------------------------------------------------------------------
[Preview]: Azure SignalR Service should use private links  /subscriptions/<subscriptionId>/resourceGroups/<resourceGroup>/providers/Microsoft.Authorization/policyAssignments/<assignmentId>
```

Führen Sie dann [az policy state list](/cli/azure/policy/state#az-policy-state-list) aus, um den Konformitätszustand aller Ressourcen einer bestimmten Ressourcengruppe im JSON-Format zurückzugeben:

```azurecli
az policy state list --g <resourceGroup>
```

Mit [az policy state list](/cli/azure/policy/state#az-policy-state-list) können Sie auch den Konformitätszustand einer bestimmten SignalR-Ressource im JSON-Format zurückgeben:

```azurecli
az policy state list \
 --resource /subscriptions/<subscriptionId>/resourceGroups/<resourceGroup>/providers/Microsoft.SignalRService/SignalR/<resourceName> \
 --namespace Microsoft.SignalRService \
 --resource-group <resourceGroup>
```

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zu [Definitionen](../governance/policy/concepts/definition-structure.md) und [Auswirkungen](../governance/policy/concepts/effects.md) in Azure Policy

* Erstellen einer [benutzerdefinierten Richtliniendefinition](../governance/policy/tutorials/create-custom-policy-definition.md)

* Weitere Informationen zu [Governancefunktionen](../governance/index.yml) in Azure


<!-- LINKS - External -->
[terms-of-use]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/