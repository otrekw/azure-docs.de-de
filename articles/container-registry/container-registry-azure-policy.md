---
title: Konformität mithilfe von Azure Policy
description: Zuweisen integrierter Richtlinien in Azure Policy, um die Konformität Ihrer Azure-Containerregistrierungen zu überwachen
ms.topic: article
ms.date: 03/01/2021
ms.openlocfilehash: 0fed0c4132043e1eaed7e634e1f45b27f7c6e933
ms.sourcegitcommit: 225e4b45844e845bc41d5c043587a61e6b6ce5ae
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/11/2021
ms.locfileid: "103014297"
---
# <a name="audit-compliance-of-azure-container-registries-using-azure-policy"></a>Überwachen der Konformität von Azure-Containerregistrierungen mithilfe von Azure Policy

[Azure Policy](../governance/policy/overview.md) ist ein Dienst in Azure, mit dem Sie Richtlinien erstellen, zuweisen und verwalten können. Mit diesen Richtlinien werden unterschiedliche Regeln und Auswirkungen für Ihre Ressourcen erzwungen, damit diese stets mit Ihren Unternehmensstandards und Vereinbarungen zum Servicelevel konform bleiben.

In diesem Artikel werden integrierte Richtlinien für Azure Container Registry vorgestellt. Verwenden Sie diese Richtlinien, um die Konformität neuer und vorhandener Registrierungen zu überwachen.

Die Verwendung von Azure Policy ist gebührenfrei.

## <a name="built-in-policy-definitions"></a>Integrierte Richtliniendefinitionen

Für Azure Container Registry gelten die folgenden integrierten Richtliniendefinitionen:

[!INCLUDE [azure-policy-reference-rp-containerreg](../../includes/policy/reference/byrp/microsoft.containerregistry.md)]

## <a name="assign-policies"></a>Zuweisen von Richtlinien

* Weisen Sie Richtlinien über das [Azure-Portal](../governance/policy/assign-policy-portal.md), die [Azure CLI](../governance/policy/assign-policy-azurecli.md), eine [Resource Manager-Vorlage](../governance/policy/assign-policy-template.md) oder die Azure Policy-SDKs zu.
* Beschränken Sie eine Richtlinienzuweisung auf eine Ressourcengruppe, ein Abonnement oder eine [Azure-Verwaltungsgruppe](../governance/management-groups/overview.md). Zuweisungen von Containerregistrierungsrichtlinien gelten für vorhandene und neue Containerregistrierungen innerhalb des festgelegten Bereichs.
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

    ![Richtlinienkonformität im Portal](./media/container-registry-azure-policy/azure-policy-compliance.png)
    
1. Wählen Sie eine Richtlinie aus, um die Zusammenfassung der Konformitätsdetails und -ereignisse zu überprüfen. Wählen Sie gegebenenfalls eine bestimmte Registrierung aus, deren Konformität überprüft werden soll.

### <a name="policy-compliance-in-the-azure-cli"></a>Richtlinienkonformität in der Azure CLI

Konformitätsdaten können auch über die Azure CLI abgerufen werden. Verwenden Sie beispielsweise den Befehl [az policy assignment list](/cli/azure/policy/assignment#az-policy-assignment-list) in der CLI, um die Richtlinien-IDs der Azure Container Registry-Richtlinien abzurufen, die angewendet werden:

```azurecli
az policy assignment list --query "[?contains(displayName,'Container Registries')].{name:displayName, ID:id}" --output table
```

Beispielausgabe:

```
Name                                                                                   ID
-------------------------------------------------------------------------------------  --------------------------------------------------------------------------------------------------------------------------------
Container Registries should not allow unrestricted network access           /subscriptions/<subscriptionID>/providers/Microsoft.Authorization/policyAssignments/b4faf132dc344b84ba68a441
Container Registries should be encrypted with a Customer-Managed Key (CMK)  /subscriptions/<subscriptionID>/providers/Microsoft.Authorization/policyAssignments/cce1ed4f38a147ad994ab60a
```

Führen Sie dann [az policy state list](/cli/azure/policy/state#az-policy-state-list) aus, um den Konformitätszustand für alle Ressourcen einer bestimmten Richtlinien-ID im JSON-Format zurückzugeben:

```azurecli
az policy state list \
  --resource <policyID>
```

Sie können [az policy state list](/cli/azure/policy/state#az-policy-state-list) auch ausführen, um den Konformitätszustand einer bestimmten Registrierungsressource (z.B. *myregistry*) im JSON-Format zurückzugeben:

```azurecli
az policy state list \
 --resource myregistry \
 --namespace Microsoft.ContainerRegistry \
 --resource-type registries \
 --resource-group myresourcegroup
```

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zu [Definitionen](../governance/policy/concepts/definition-structure.md) und [Auswirkungen](../governance/policy/concepts/effects.md) in Verbindung mit Azure Policy.

* Erstellen Sie eine [benutzerdefinierte Richtliniendefinition](../governance/policy/tutorials/create-custom-policy-definition.md).

* Weitere Informationen zu [Governancefunktionen](../governance/index.yml) in Azure.
