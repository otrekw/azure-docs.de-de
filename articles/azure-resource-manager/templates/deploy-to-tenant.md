---
title: Bereitstellen von Ressourcen für einen Mandanten
description: Hier erfahren Sie, wie Sie Ressourcen im Mandantenbereich in einer Azure Resource Manager-Vorlage bereitstellen.
ms.topic: conceptual
ms.date: 10/22/2020
ms.openlocfilehash: 854ccbd43509b6c0b5a04357844c78c32b7e6396
ms.sourcegitcommit: 4cb89d880be26a2a4531fedcc59317471fe729cd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/27/2020
ms.locfileid: "92668692"
---
# <a name="tenant-deployments-with-arm-templates"></a>Mandantenbereitstellungen mit ARM-Vorlagen

Im Zuge der Entwicklung Ihrer Organisation müssen unter Umständen [Richtlinien](../../governance/policy/overview.md) oder die [rollenbasierte Zugriffssteuerung in Azure (Azure Role-Based Access Control, Azure RBAC)](../../role-based-access-control/overview.md) für Ihren Azure AD-Mandanten definiert und zugewiesen werden. Mit Vorlagen auf der Mandantenebene können Sie Richtlinien deklarativ anwenden und Rollen global zuweisen.

## <a name="supported-resources"></a>Unterstützte Ressourcen

Nicht alle Ressourcentypen können auf Mandantenebene bereitgestellt werden. Im folgenden Abschnitt werden die unterstützten Ressourcentypen aufgelistet.

Verwenden Sie für Azure-Richtlinien:

* [policyAssignments](/azure/templates/microsoft.authorization/policyassignments)
* [policyDefinitions](/azure/templates/microsoft.authorization/policydefinitions)
* [policySetDefinitions](/azure/templates/microsoft.authorization/policysetdefinitions)

Verwenden Sie für rollenbasierte Zugriffssteuerung von Azure (Azure RBAC) Folgendes:

* [roleAssignments](/azure/templates/microsoft.authorization/roleassignments)

Verwenden Sie für geschachtelte Vorlagen, die om Verwaltungsgruppen, Abonnements oder Ressourcengruppen bereitstellen:

* [Bereitstellungen](/azure/templates/microsoft.resources/deployments)

Verwenden Sie zum Erstellen von Verwaltungsgruppen:

* [managementGroups](/azure/templates/microsoft.management/managementgroups)

Verwenden Sie zum Verwalten von Kosten:

* [billingProfiles](/azure/templates/microsoft.billing/billingaccounts/billingprofiles)
* [instructions](/azure/templates/microsoft.billing/billingaccounts/billingprofiles/instructions)
* [invoiceSections](/azure/templates/microsoft.billing/billingaccounts/billingprofiles/invoicesections)

## <a name="schema"></a>Schema

Das Schema, das Sie für Bereitstellungen auf der Mandantenebene verwenden, unterscheidet sich von dem Schema für Ressourcengruppenbereitstellungen.

Verwenden Sie für Vorlagen Folgendes:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-08-01/tenantDeploymentTemplate.json#",
    ...
}
```

Das Schema für eine Parameterdatei ist für alle Bereitstellungsbereiche identisch. Verwenden Sie für Parameterdateien Folgendes:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentParameters.json#",
    ...
}
```

## <a name="required-access"></a>Erforderlicher Zugriff

Der Prinzipal, der die Vorlage bereitstellt, muss über Berechtigungen zum Erstellen von Ressourcen im Mandantenbereich verfügen. Der Prinzipal muss über zum Ausführen der Bereitstellungsaktionen (`Microsoft.Resources/deployments/*`) sowie zum Erstellen der in der Vorlage definierten Ressourcen berechtigt sein. Soll also beispielsweise eine Verwaltungsgruppe erstellt werden, muss der Prinzipal im Mandantenbereich über die Berechtigung „Mitwirkender“ verfügen. Zum Erstellen von Rollenzuweisungen muss der Prinzipal über die Berechtigung „Besitzer“ verfügen.

Der globale Administrator für die Azure Active Directory-Instanz ist nicht automatisch zum Zuweisen von Rollen berechtigt. Um Vorlagenbereitstellungen im Mandantenbereich zu ermöglichen, muss der globale Administrator folgende Schritte ausführen:

1. Erhöhen der Kontozugriffsrechte, damit der globale Administrator Rollen zuweisen kann. Weitere Informationen finden Sie unter [Erhöhen der Zugriffsrechte zum Verwalten aller Azure-Abonnements und Verwaltungsgruppen](../../role-based-access-control/elevate-access-global-admin.md).

1. Zuweisen der Rolle „Besitzer“ oder „Mitwirkender“ für den Prinzipal, der die Vorlagen bereitstellen muss:

   ```azurepowershell-interactive
   New-AzRoleAssignment -SignInName "[userId]" -Scope "/" -RoleDefinitionName "Owner"
   ```

   ```azurecli-interactive
   az role assignment create --assignee "[userId]" --scope "/" --role "Owner"
   ```

Der Prinzipal verfügt nun über die erforderlichen Berechtigungen, um die Vorlage bereitzustellen.

## <a name="deployment-commands"></a>Bereitstellungsbefehle

Die Befehle für Mandantenbereitstellungen unterscheiden sich von den Befehlen für Ressourcengruppenbereitstellungen.

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

Verwenden Sie bei der Azure CLI [az deployment tenant create](/cli/azure/deployment/tenant#az-deployment-tenant-create):

```azurecli-interactive
az deployment tenant create \
  --name demoTenantDeployment \
  --location WestUS \
  --template-uri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/tenant-deployments/new-mg/azuredeploy.json"
```

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Verwenden Sie für Azure PowerShell den Befehl [New-AzTenantDeployment](/powershell/module/az.resources/new-aztenantdeployment).

```azurepowershell-interactive
New-AzTenantDeployment `
  -Name demoTenantDeployment `
  -Location "West US" `
  -TemplateUri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/tenant-deployments/new-mg/azuredeploy.json"
```

---

Ausführlichere Informationen über Bereitstellungsbefehle und -optionen für die Bereitstellung von ARM-Vorlagen finden Sie in den folgenden Artikeln:

* [Bereitstellen von Ressourcen mit ARM-Vorlagen und dem Azure-Portal](deploy-portal.md)
* [Bereitstellen von Ressourcen mit ARM-Vorlagen und der Azure CLI](deploy-cli.md)
* [Bereitstellen von Ressourcen mit ARM-Vorlagen und Azure PowerShell](deploy-powershell.md)
* [Bereitstellen von Ressourcen mit ARM-Vorlagen und der Resource Manager-REST-API](deploy-rest.md)
* [Verwenden einer Bereitstellungsschaltfläche zum Bereitstellen von Vorlagen aus einem GitHub-Repository](deploy-to-azure-button.md)
* [Bereitstellen von ARM-Vorlagen über Cloud Shell](deploy-cloud-shell.md)

## <a name="deployment-scopes"></a>Bereitstellungsbereiche

Bei der Bereitstellung in einer Verwaltungsgruppe können Sie Ressourcen an folgenden Orten bereitstellen:

* im Mandanten
* in Verwaltungsgruppen innerhalb des Mandanten
* subscriptions
* in Ressourcengruppen (über zwei geschachtelte Bereitstellungen)
* [Erweiterungsressourcen](scope-extension-resources.md) können auf Ressourcen angewendet werden.

Der Benutzer, der die Vorlage bereitstellt, muss Zugriff auf den angegebenen Bereich besitzen.

In diesem Abschnitt wird das Festlegen verschiedener Bereiche veranschaulicht. Sie können diese verschiedenen Bereiche in einer Vorlage kombinieren.

### <a name="scope-to-tenant"></a>Bereich: Mandant

Ressourcen, die im Ressourcenabschnitt der Vorlage definiert sind, werden auf den Mandanten angewendet.

:::code language="json" source="~/resourcemanager-templates/azure-resource-manager/scope/default-tenant.json" highlight="5":::

### <a name="scope-to-management-group"></a>Bereich: Verwaltungsgruppe

Um eine Verwaltungsgruppe innerhalb des Mandanten als Ziel zu verwenden, fügen Sie eine geschachtelte Bereitstellung hinzu, und geben Sie die `scope`-Eigenschaft an.

:::code language="json" source="~/resourcemanager-templates/azure-resource-manager/scope/tenant-to-mg.json" highlight="10,17,22":::

### <a name="scope-to-subscription"></a>Bereich: Abonnement

Sie können auch Abonnements innerhalb des Mandanten als Bereitstellungsziel verwenden. Der Benutzer, der die Vorlage bereitstellt, muss Zugriff auf den angegebenen Bereich besitzen.

Verwenden Sie eine geschachtelte Bereitstellung und die Eigenschaft `subscriptionId`, um ein Abonnement im Mandanten als Bereitstellungsziel zu verwenden.

:::code language="json" source="~/resourcemanager-templates/azure-resource-manager/scope/tenant-to-subscription.json" highlight="10,18":::

## <a name="deployment-location-and-name"></a>Bereitstellungsspeicherort und -name

Für Bereitstellungen auf der Mandantenebene müssen Sie einen Speicherort für die Bereitstellung angeben. Der Speicherort der Bereitstellung ist vom Speicherort der Ressourcen getrennt, die Sie bereitstellen. Der Bereitstellungsspeicherort gibt an, wo Bereitstellungsdaten gespeichert werden sollen.

Sie können einen Namen für die Bereitstellung angeben oder den Bereitstellungsstandardnamen verwenden. Der Standardname ist der Name der Vorlagendatei. Wenn Sie z.B. eine Vorlage mit dem Namen **azuredeploy.json** bereitstellen, wird **azuredeploy** als Standardname für die Bereitstellung erstellt.

Der Speicherort für jeden Bereitstellungsnamen ist unveränderlich. Sie können keine Bereitstellung an einem Speicherort erstellen, wenn bereits eine Bereitstellung mit demselben Namen an einem anderen Speicherort vorhanden ist. Wenn Sie den Fehlercode `InvalidDeploymentLocation` erhalten, verwenden Sie entweder einen anderen Namen oder denselben Speicherort wie bei der vorherigen Bereitstellung für diesen Namen.

## <a name="create-management-group"></a>Erstellen einer Verwaltungsgruppe

Die [folgende Vorlage](https://github.com/Azure/azure-quickstart-templates/tree/master/tenant-deployments/new-mg) dient zum Erstellen einer Verwaltungsgruppe:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-08-01/tenantDeploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "mgName": {
      "type": "string",
      "defaultValue": "[concat('mg-', uniqueString(newGuid()))]"
    }
  },
  "resources": [
    {
      "type": "Microsoft.Management/managementGroups",
      "apiVersion": "2019-11-01",
      "name": "[parameters('mgName')]",
      "properties": {
      }
    }
  ]
}
```

## <a name="assign-role"></a>Zuweisen einer Rolle

Die [folgende Vorlage](https://github.com/Azure/azure-quickstart-templates/tree/master/tenant-deployments/tenant-role-assignment) dient zum Zuweisen einer Rolle im Mandantenbereich:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-08-01/tenantDeploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "principalId": {
      "type": "string",
      "metadata": {
        "description": "principalId if the user that will be given contributor access to the resourceGroup"
      }
    },
    "roleDefinitionId": {
      "type": "string",
      "defaultValue": "8e3af657-a8ff-443c-a75c-2fe8c4bcb635",
      "metadata": {
        "description": "roleDefinition for the assignment - default is owner"
      }
    }
  },
  "variables": {
    // This creates an idempotent guid for the role assignment
    "roleAssignmentName": "[guid('/', parameters('principalId'), parameters('roleDefinitionId'))]"
  },
  "resources": [
    {
      "name": "[variables('roleAssignmentName')]",
      "type": "Microsoft.Authorization/roleAssignments",
      "apiVersion": "2019-04-01-preview",
      "properties": {
        "roleDefinitionId": "[tenantResourceId('Microsoft.Authorization/roleDefinitions', parameters('roleDefinitionId'))]",
        "principalId": "[parameters('principalId')]",
        "scope": "/"
      }
    }
  ]
}
```

## <a name="next-steps"></a>Nächste Schritte

* Informationen zum Zuweisen von Rollen finden Sie unter [Hinzufügen von Azure-Rollenzuweisungen mithilfe von Azure Resource Manager-Vorlagen](../../role-based-access-control/role-assignments-template.md).
* Vorlagen können auch auf der [Abonnementebene](deploy-to-subscription.md) oder auf der [Verwaltungsgruppenebene](deploy-to-management-group.md) bereitgestellt werden.
