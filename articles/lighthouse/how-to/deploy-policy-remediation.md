---
title: Bereitstellen einer Richtlinie, die gewartet werden kann
description: Erfahren Sie, wie Sie einen Kunden für delegierte Azure-Ressourcenverwaltung integrieren, sodass Sie von Ihrem eigenen Mandanten aus auf dessen Ressourcen zugreifen und sie verwalten können.
ms.date: 10/11/2019
ms.topic: conceptual
ms.openlocfilehash: b625e9e3c96866cfbc655a55b770c9ac07a626bd
ms.sourcegitcommit: 7d8158fcdcc25107dfda98a355bf4ee6343c0f5c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/09/2020
ms.locfileid: "80985166"
---
# <a name="deploy-a-policy-that-can-be-remediated-within-a-delegated-subscription"></a>Bereitstellen einer Richtlinie, die innerhalb eines delegierten Abonnements gewartet werden kann

Mit [Azure Lighthouse](../overview.md) können Dienstanbieter Richtliniendefinitionen innerhalb eines delegierten Abonnements erstellen und bearbeiten. Wenn Sie allerdings Richtlinien mit einem [Wartungstask](../../governance/policy/how-to/remediate-resources.md) (sprich: Richtlinien mit [deployIfNotExists](../../governance/policy/concepts/effects.md#deployifnotexists)- oder [modify](../../governance/policy/concepts/effects.md#modify)-Effekt) bereitstellen möchten, müssen Sie eine [verwaltete Identität](../../active-directory/managed-identities-azure-resources/overview.md) im Kundenmandanten erstellen. Diese verwaltete Identität kann von Azure Policy verwendet werden, um die Vorlage innerhalb der Richtlinie bereitzustellen. Für dieses Szenario sind bestimmte Schritte erforderlich – sowohl beim Onboarding des Kunden für die delegierte Azure-Ressourcenverwaltung als auch bei der Bereitstellung der eigentlichen Richtlinie.

## <a name="create-a-user-who-can-assign-roles-to-a-managed-identity-in-the-customer-tenant"></a>Erstellen eines Benutzers, der einer verwalteten Identität im Kundenmandanten Rollen zuweisen kann

Im Rahmen des Kunden-Onboardings für die delegierte Azure-Ressourcenverwaltung verwenden Sie eine [Azure Resource Manager-Vorlage](onboard-customer.md#create-an-azure-resource-manager-template) sowie eine Parameterdatei, die die Benutzer, Benutzergruppen und Dienstprinzipale in Ihrem Verwaltungsmandanten definiert, der auf die delegierten Ressourcen im Kundenmandanten zugreifen kann. In Ihrer Parameterdatei wird jedem dieser Benutzer (**principalId**) eine [integrierte Rolle](../../role-based-access-control/built-in-roles.md) (**roleDefinitionId**) zugewiesen, die die Zugriffsebene definiert.

Damit von einer Prinzipal-ID (**principalId**) eine verwaltete Identität im Kundenmandanten erstellt werden kann, muss die zugehörige Rollendefinitions-ID (**roleDefinitionId**) auf **Benutzerzugriffsadministrator** festgelegt werden. Diese Rolle wird zwar nicht allgemein unterstützt, kann aber in diesem speziellen Szenario verwendet werden. Benutzer mit dieser Berechtigung können verwalteten Identitäten spezifische integrierte Rollen zuweisen. Diese Rollen werden in der Eigenschaft **delegatedRoleDefinitionIds** definiert. Sie können hier jede beliebige integrierte Rolle einschließen (mit Ausnahme von „Benutzerzugriffsadministrator“ und „Besitzer“).

Nach Abschluss des Kunden-Onboardings kann die in dieser Autorisierung erstellte Prinzipal-ID (**principalId**) verwalteten Identitäten im Kundenmandanten diese integrierten Rollen zuweisen. Sie verfügen jedoch über keine der anderen Berechtigungen, die normalerweise der Rolle „Benutzerzugriffsadministrator“ zugeordnet sind.

Das folgende Beispiel zeigt eine Prinzipal-ID (**principalId**), die über die Rolle „Benutzerzugriffsadministrator“ verfügt. Dieser Benutzer kann verwalteten Identitäten im Kundenmandanten zwei integrierte Rollen zuweisen: „Mitwirkender“ und „Log Analytics-Mitwirkender“.

```json
{
    "principalId": "3kl47fff-5655-4779-b726-2cf02b05c7c4",
    "principalIdDisplayName": "Policy Automation Account",
    "roleDefinitionId": "18d7d88d-d35e-4fb5-a5c3-7773c20a72d9",
    "delegatedRoleDefinitionIds": [
         "b24988ac-6180-42a0-ab88-20f7382dd24c",
         "92aaf0da-9dab-42b6-94a3-d43ce8d16293"
    ]
}
```

## <a name="deploy-policies-that-can-be-remediated"></a>Bereitstellen von Richtlinien, die gewartet werden können

Nachdem Sie gemäß der obigen Beschreibung den Benutzer mit den erforderlichen Berechtigungen erstellt haben, kann dieser Richtlinien im Kundenmandanten bereitstellen, die Wartungstasks verwenden.

Nehmen wir beispielsweise an, Sie möchten Diagnosen für Azure Key Vault-Ressourcen im Kundenmandanten aktivieren, wie in [diesem Beispiel](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/templates/policy-enforce-keyvault-monitoring) veranschaulicht. In diesem Fall würde ein Benutzer im Verwaltungsmandanten mit entsprechenden Berechtigungen (wie oben beschrieben) eine [Azure Resource Manager-Vorlage](https://github.com/Azure/Azure-Lighthouse-samples/blob/master/templates/policy-enforce-keyvault-monitoring/enforceAzureMonitoredKeyVault.json) bereitstellen.

Hinweis: Die Richtlinienzuweisung zur Verwendung mit einem delegierten Abonnement muss aktuell über APIs erstellt werden (nicht über das Azure-Portal). Dabei muss die API-Version (**apiVersion**) auf **2019-04-01-preview** festgelegt werden, die die neue Eigenschaft **delegatedManagedIdentityResourceId** beinhaltet. Diese Eigenschaft ermöglicht es Ihnen, eine verwaltete Identität einzuschließen, die sich im Kundenmandanten befindet (in einem Abonnement oder in einer Ressourcengruppe, das bzw. die mittels Onboarding in die delegierte Azure-Ressourcenverwaltung integriert wurde).

Das folgende Beispiel zeigt eine Rollenzuweisung mit **delegatedManagedIdentityResourceId**:

```json
"type": "Microsoft.Authorization/roleAssignments",
            "apiVersion": "2019-04-01-preview",
            "name": "[parameters('rbacGuid')]",
            "dependsOn": [
                "[variables('policyAssignment')]"
            ],
            "properties": {
                "roleDefinitionId": "[concat(subscription().id, '/providers/Microsoft.Authorization/roleDefinitions/', variables('rbacContributor'))]",
                "principalType": "ServicePrincipal",
                "delegatedManagedIdentityResourceId": "[concat(subscription().id, '/providers/Microsoft.Authorization/policyAssignments/', variables('policyAssignment'))]",
                "principalId": "[toLower(reference(concat('/providers/Microsoft.Authorization/policyAssignments/', variables('policyAssignment')), '2018-05-01', 'Full' ).identity.principalId)]"
            }
```

> [!TIP]
> In einem [ähnlichen Beispiel](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/templates/policy-add-or-replace-tag) wird gezeigt, wie eine Richtlinie zugewiesen wird, die unter Verwendung des modify-Effekts einem delegierten Abonnement ein Tag hinzufügt oder ein Tag daraus entfernt.

## <a name="next-steps"></a>Nächste Schritte

- Informieren Sie sich über [Azure Policy](../../governance/policy/index.yml).
- Informieren Sie sich über [verwaltete Identitäten für Azure-Ressourcen](../../active-directory/managed-identities-azure-resources/overview.md).
