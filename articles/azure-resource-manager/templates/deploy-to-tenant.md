---
title: Bereitstellen von Ressourcen für einen Mandanten
description: Hier erfahren Sie, wie Sie Ressourcen im Mandantenbereich in einer Azure Resource Manager-Vorlage bereitstellen.
ms.topic: conceptual
ms.date: 03/16/2020
ms.openlocfilehash: fcdfc5b1c4333a0d7eeec80a09ad85579a1f8b77
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "79460261"
---
# <a name="create-resources-at-the-tenant-level"></a>Erstellen von Ressourcen auf der Mandantenebene

Im Zuge der Entwicklung Ihrer Organisation müssen unter Umständen [Richtlinien](../../governance/policy/overview.md) oder [rollenbasierte Zugriffssteuerungen](../../role-based-access-control/overview.md) innerhalb Ihres Azure AD-Mandanten definiert und zugewiesen werden. Mit Vorlagen auf der Mandantenebene können Sie Richtlinien deklarativ anwenden und Rollen global zuweisen.

## <a name="supported-resources"></a>Unterstützte Ressourcen

Folgende Ressourcentypen können auf der Mandantenebene bereitgestellt werden:

* [Bereitstellungen:](/azure/templates/microsoft.resources/deployments) Für geschachtelte Vorlagen, die für die Bereitstellung in Verwaltungsgruppen oder Abonnements verwendet werden.
* [policyAssignments](/azure/templates/microsoft.authorization/policyassignments)
* [policyDefinitions](/azure/templates/microsoft.authorization/policydefinitions)
* [policySetDefinitions](/azure/templates/microsoft.authorization/policysetdefinitions)
* [roleAssignments](/azure/templates/microsoft.authorization/roleassignments)
* [roleDefinitions](/azure/templates/microsoft.authorization/roledefinitions)

### <a name="schema"></a>Schema

Das Schema, das Sie für Bereitstellungen auf der Mandantenebene verwenden, unterscheidet sich von dem Schema für Ressourcengruppenbereitstellungen.

Verwenden Sie für Vorlagen Folgendes:

```json
https://schema.management.azure.com/schemas/2019-08-01/tenantDeploymentTemplate.json#
```

Das Schema für eine Parameterdatei ist für alle Bereitstellungsbereiche identisch. Verwenden Sie für Parameterdateien Folgendes:

```json
https://schema.management.azure.com/schemas/2019-04-01/deploymentParameters.json#
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

Verwenden Sie bei der Azure CLI [az deployment tenant create](/cli/azure/deployment/tenant?view=azure-cli-latest#az-deployment-tenant-create):

```azurecli-interactive
az deployment tenant create \
  --name demoTenantDeployment \
  --location WestUS \
  --template-uri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/tenant-level-deployments/new-mg/azuredeploy.json"
```

Verwenden Sie für Azure PowerShell den Befehl [New-AzTenantDeployment](/powershell/module/az.resources/new-aztenantdeployment).

```azurepowershell-interactive
New-AzTenantDeployment `
  -Name demoTenantDeployment `
  -Location "West US" `
  -TemplateUri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/tenant-level-deployments/new-mg/azuredeploy.json"
```

Verwenden Sie für die REST-API [Bereitstellungen: Erstellen oder Aktualisieren im Mandantenbereich](/rest/api/resources/deployments/createorupdateattenantscope).

## <a name="deployment-location-and-name"></a>Bereitstellungsspeicherort und -name

Für Bereitstellungen auf der Mandantenebene müssen Sie einen Speicherort für die Bereitstellung angeben. Der Speicherort der Bereitstellung ist vom Speicherort der Ressourcen getrennt, die Sie bereitstellen. Der Bereitstellungsspeicherort gibt an, wo Bereitstellungsdaten gespeichert werden sollen.

Sie können einen Namen für die Bereitstellung angeben oder den Bereitstellungsstandardnamen verwenden. Der Standardname ist der Name der Vorlagendatei. Wenn Sie z.B. eine Vorlage mit dem Namen **azuredeploy.json** bereitstellen, wird **azuredeploy** als Standardname für die Bereitstellung erstellt.

Der Speicherort für jeden Bereitstellungsnamen ist unveränderlich. Sie können keine Bereitstellung an einem Speicherort erstellen, wenn bereits eine Bereitstellung mit demselben Namen an einem anderen Speicherort vorhanden ist. Wenn Sie den Fehlercode `InvalidDeploymentLocation` erhalten, verwenden Sie entweder einen anderen Namen oder denselben Speicherort wie bei der vorherigen Bereitstellung für diesen Namen.

## <a name="use-template-functions"></a>Verwenden von Vorlagenfunktionen

Bei Mandantenbereitstellungen müssen bei der Verwendung von Vorlagenfunktionen einige wichtige Aspekte berücksichtigt werden:

* Die Funktion [resourceGroup()](template-functions-resource.md#resourcegroup) wird **nicht** unterstützt.
* Die Funktion [subscription()](template-functions-resource.md#subscription) wird **nicht** unterstützt.
* Die Funktionen [reference()](template-functions-resource.md#reference) und [list()](template-functions-resource.md#list) werden unterstützt.
* Verwenden Sie die Funktion [tenantResourceId()](template-functions-resource.md#tenantresourceid), um die Ressourcen-ID für Ressourcen abzurufen, die auf der Mandantenebene bereitgestellt werden.

  Verwenden Sie beispielsweise Folgendes, um die Ressourcen-ID für eine Richtliniendefinition abzurufen:
  
  ```json
  tenantResourceId('Microsoft.Authorization/policyDefinitions/', parameters('policyDefinition'))
  ```
  
  Die zurückgegebene Ressourcen-ID hat das folgende Format:
  
  ```json
  /providers/{resourceProviderNamespace}/{resourceType}/{resourceName}
  ```

## <a name="create-management-group"></a>Erstellen einer Verwaltungsgruppe

Die [folgende Vorlage](https://github.com/Azure/azure-quickstart-templates/tree/master/tenant-level-deployments/new-mg) dient zum Erstellen einer Verwaltungsgruppe:

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

Die [folgende Vorlage](https://github.com/Azure/azure-quickstart-templates/tree/master/tenant-level-deployments/tenant-role-assignment) dient zum Zuweisen einer Rolle im Mandantenbereich:

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

* Informationen über das Zuweisen von Rollen finden Sie unter [Verwalten des Zugriffs auf Azure-Ressourcen mit RBAC und Azure Resource Manager-Vorlagen](../../role-based-access-control/role-assignments-template.md).
* Vorlagen können auch auf der [Abonnementebene](deploy-to-subscription.md) oder auf der [Verwaltungsgruppenebene](deploy-to-management-group.md) bereitgestellt werden.
