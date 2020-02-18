---
title: Bereitstellen von Ressourcen in einem Abonnement
description: In diesem Artikel wird beschrieben, wie Sie eine Ressourcengruppe in einer Azure Resource Manager-Vorlage erstellen. Außerdem wird veranschaulicht, wie Sie Ressourcen für den Bereich des Azure-Abonnements bereitstellen.
ms.topic: conceptual
ms.date: 02/10/2020
ms.openlocfilehash: c53d274303a203a427a36f8f729f6b43cee44e40
ms.sourcegitcommit: 7c18afdaf67442eeb537ae3574670541e471463d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/11/2020
ms.locfileid: "77120617"
---
# <a name="create-resource-groups-and-resources-at-the-subscription-level"></a>Erstellen von Ressourcengruppen und Ressourcen auf Abonnementebene

In der Regel stellen Sie Azure-Ressourcen für eine Ressourcengruppe in Ihrem Azure-Abonnement bereit. Sie können jedoch auch auf Abonnementebene Ressourcen erstellen. Sie verwenden die Bereitstellungen auf Abonnementebene, um Aktionen durchzuführen, die auf dieser Ebene sinnvoll sind, etwa das Erstellen von Ressourcengruppen oder das Zuweisen von [rollenbasierter Zugriffssteuerung ](../../role-based-access-control/overview.md).

Um Vorlagen auf Abonnementebene bereitzustellen, verwenden Sie die Azure CLI, PowerShell oder die REST-API. Das Azure-Portal unterstützt keine Bereitstellung auf Abonnementebene.

## <a name="supported-resources"></a>Unterstützte Ressourcen

Sie können die folgenden Ressourcentypen auf der Abonnementebene bereitstellen:

* [Bereitstellungen](/azure/templates/microsoft.resources/deployments)
* [peerAsns](/azure/templates/microsoft.peering/peerasns)
* [policyAssignments](/azure/templates/microsoft.authorization/policyassignments)
* [policyDefinitions](/azure/templates/microsoft.authorization/policydefinitions)
* [policySetDefinitions](/azure/templates/microsoft.authorization/policysetdefinitions)
* [resourceGroups](/azure/templates/microsoft.resources/resourcegroups)
* [roleAssignments](/azure/templates/microsoft.authorization/roleassignments)
* [roleDefinitions](/azure/templates/microsoft.authorization/roledefinitions)

### <a name="schema"></a>Schema

Das Schema, das Sie für Bereitstellungen auf Abonnementebene verwenden, unterscheidet sich von dem Schema für Ressourcengruppenbereitstellungen.

Verwenden Sie für Vorlagen Folgendes:

```json
https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#
```

Verwenden Sie für Parameterdateien Folgendes:

```json
https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentParameters.json#
```

## <a name="deployment-commands"></a>Bereitstellungsbefehle

Die Befehle für Bereitstellungen auf Abonnementebene unterscheiden sich von den Befehlen für Ressourcengruppenbereitstellungen.

Verwenden Sie für die Azure CLI [az deployment create](/cli/azure/deployment?view=azure-cli-latest#az-deployment-create). Das folgende Beispiel stellt eine Vorlage zum Erstellen einer Ressourcengruppe bereit:

```azurecli-interactive
az deployment create \
  --name demoDeployment \
  --location centralus \
  --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/emptyRG.json \
  --parameters rgName=demoResourceGroup rgLocation=centralus
```


Verwenden Sie als PowerShell-Bereitstellungsbefehl [New-AzDeployment](/powershell/module/az.resources/new-azdeployment). Das folgende Beispiel stellt eine Vorlage zum Erstellen einer Ressourcengruppe bereit:

```azurepowershell-interactive
New-AzDeployment `
  -Name demoDeployment `
  -Location centralus `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/emptyRG.json `
  -rgName demoResourceGroup `
  -rgLocation centralus
```

Verwenden Sie für die REST-API [Bereitstellungen: Erstellen im Abonnementbereich](/rest/api/resources/deployments/createorupdateatsubscriptionscope).

## <a name="deployment-location-and-name"></a>Bereitstellungsspeicherort und -name

Für Bereitstellungen auf Abonnementebene müssen Sie einen Speicherort für die Bereitstellung angeben. Der Speicherort der Bereitstellung ist vom Speicherort der Ressourcen getrennt, die Sie bereitstellen. Der Bereitstellungsspeicherort gibt an, wo Bereitstellungsdaten gespeichert werden sollen.

Sie können einen Namen für die Bereitstellung angeben oder den Bereitstellungsstandardnamen verwenden. Der Standardname ist der Name der Vorlagendatei. Wenn Sie z.B. eine Vorlage mit dem Namen **azuredeploy.json** bereitstellen, wird **azuredeploy** als Standardname für die Bereitstellung erstellt.

Der Speicherort für jeden Bereitstellungsnamen ist unveränderlich. Sie können keine Bereitstellung an einem Speicherort erstellen, wenn bereits eine Bereitstellung mit demselben Namen an einem anderen Speicherort vorhanden ist. Wenn Sie den Fehlercode `InvalidDeploymentLocation` erhalten, verwenden Sie entweder einen anderen Namen oder denselben Speicherort wie bei der vorherigen Bereitstellung für diesen Namen.

## <a name="use-template-functions"></a>Verwenden von Vorlagenfunktionen

Bei Bereitstellungen auf Abonnementebene müssen bei der Verwendung von Vorlagenfunktionen einige wichtige Aspekte berücksichtigt werden:

* Die Funktion [resourceGroup()](template-functions-resource.md#resourcegroup) wird **nicht** unterstützt.
* Die Funktionen [reference()](template-functions-resource.md#reference) und [list()](template-functions-resource.md#list) werden unterstützt.
* Die Funktion [resourceId()](template-functions-resource.md#resourceid) wird unterstützt. Verwenden sie diese Funktion zum Abrufen der Ressourcen-ID für Ressourcen, die in Bereitstellungen auf Abonnementebene verwendet werden. Geben Sie für den Parameter der Ressourcengruppe keinen Wert an.

  Verwenden Sie beispielsweise Folgendes, um die Ressourcen-ID für eine Richtliniendefinition abzurufen:
  
  ```json
  resourceId('Microsoft.Authorization/roleDefinitions/', parameters('roleDefinition'))
  ```
  
  Die zurückgegebene Ressourcen-ID hat das folgende Format:

  ```json
  /subscriptions/{subscriptionId}/providers/{resourceProviderNamespace}/{resourceType}/{resourceName}
  ```

  Sie können auch mit der Funktion [subscriptionResourceId()](template-functions-resource.md#subscriptionresourceid) die Ressourcen-ID für eine Ressource auf der Abonnementebene abrufen.

## <a name="create-resource-groups"></a>Erstellen von Ressourcengruppe

Um eine Ressourcengruppe in einer Azure Resource Manager-Vorlage zu erstellen, definieren Sie eine Ressource [Microsoft.Resources/resourceGroups](/azure/templates/microsoft.resources/allversions) mit einem Namen und einem Speicherort für die Ressourcengruppe. Sie können eine Ressourcengruppe erstellen und Ressourcen für diese Ressourcengruppe in derselben Vorlage bereitstellen.

Mit der folgenden Vorlage wird eine leere Ressourcengruppe erstellt.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "rgName": {
      "type": "string"
    },
    "rgLocation": {
      "type": "string"
    }
  },
  "variables": {},
  "resources": [
    {
      "type": "Microsoft.Resources/resourceGroups",
      "apiVersion": "2018-05-01",
      "name": "[parameters('rgName')]",
      "location": "[parameters('rgLocation')]",
      "properties": {}
    }
  ],
  "outputs": {}
}
```

Verwenden Sie das [copy-Element](create-multiple-instances.md) mit Ressourcengruppen, um mehrere Ressourcengruppen erstellen zu können.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "rgNamePrefix": {
      "type": "string"
    },
    "rgLocation": {
      "type": "string"
    },
    "instanceCount": {
      "type": "int"
    }
  },
  "variables": {},
  "resources": [
    {
      "type": "Microsoft.Resources/resourceGroups",
      "apiVersion": "2018-05-01",
      "location": "[parameters('rgLocation')]",
      "name": "[concat(parameters('rgNamePrefix'), copyIndex())]",
      "copy": {
        "name": "rgCopy",
        "count": "[parameters('instanceCount')]"
      },
      "properties": {}
    }
  ],
  "outputs": {}
}
```

Weitere Informationen zur Ressourceniteration finden Sie unter [Bereitstellen mehrerer Instanzen einer Ressource oder Eigenschaft in Azure Resource Manager-Vorlagen](./create-multiple-instances.md) sowie unter [Tutorial: Erstellen mehrerer Ressourceninstanzen mit Resource Manager-Vorlagen](./template-tutorial-create-multiple-instances.md).

## <a name="resource-group-and-resources"></a>Ressourcengruppen und Ressourcen

Verwenden Sie zum Erstellen der Ressourcengruppe und Bereitstellen von Ressourcen für diese eine geschachtelte Vorlage. Die geschachtelte Vorlage definiert die Ressourcen, die für die Ressourcengruppe bereitgestellt werden sollen. Legen Sie die geschachtelte Vorlage vor der Ressourcenbereitstellung als von der Ressourcengruppe abhängig fest, um sicherzustellen, dass die Ressourcengruppe vorhanden ist.

Das folgende Beispiel erstellt eine Ressourcengruppe und stellt ein Speicherkonto in der Ressourcengruppe bereit.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "rgName": {
      "type": "string"
    },
    "rgLocation": {
      "type": "string"
    },
    "storagePrefix": {
      "type": "string",
      "maxLength": 11
    }
  },
  "variables": {
    "storageName": "[concat(parameters('storagePrefix'), uniqueString(subscription().id, parameters('rgName')))]"
  },
  "resources": [
    {
      "type": "Microsoft.Resources/resourceGroups",
      "apiVersion": "2018-05-01",
      "location": "[parameters('rgLocation')]",
      "name": "[parameters('rgName')]",
      "properties": {}
    },
    {
      "type": "Microsoft.Resources/deployments",
      "apiVersion": "2018-05-01",
      "name": "storageDeployment",
      "resourceGroup": "[parameters('rgName')]",
      "dependsOn": [
        "[resourceId('Microsoft.Resources/resourceGroups/', parameters('rgName'))]"
      ],
      "properties": {
        "mode": "Incremental",
        "template": {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
          "contentVersion": "1.0.0.0",
          "parameters": {},
          "variables": {},
          "resources": [
            {
              "type": "Microsoft.Storage/storageAccounts",
              "apiVersion": "2017-10-01",
              "name": "[variables('storageName')]",
              "location": "[parameters('rgLocation')]",
              "sku": {
                "name": "Standard_LRS"
              }
              "kind": "StorageV2",
            }
          ],
          "outputs": {}
        }
      }
    }
  ],
  "outputs": {}
}
```

## <a name="create-policies"></a>Erstellen von Richtlinien

### <a name="assign-policy"></a>Zuweisen der Richtlinie

Im folgenden Beispiel wird dem Abonnement eine vorhandene Richtliniendefinition zugewiesen. Wenn die Richtlinie Parameter unterstützt, stellen Sie diese als Objekt bereit. Wenn die Richtlinie keine Parameter unterstützt, verwenden Sie das standardmäßige leere Objekt.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "policyDefinitionID": {
      "type": "string"
    },
    "policyName": {
      "type": "string"
    },
    "policyParameters": {
      "type": "object",
      "defaultValue": {}
    }
  },
  "variables": {},
  "resources": [
    {
      "type": "Microsoft.Authorization/policyAssignments",
      "apiVersion": "2018-03-01",
      "name": "[parameters('policyName')]",
      "properties": {
        "scope": "[subscription().id]",
        "policyDefinitionId": "[parameters('policyDefinitionID')]",
        "parameters": "[parameters('policyParameters')]"
      }
    }
  ]
}
```

Stellen Sie diese Vorlage mit der Azure CLI wie folgt bereit:

```azurecli-interactive
# Built-in policy that accepts parameters
definition=$(az policy definition list --query "[?displayName=='Allowed locations'].id" --output tsv)

az deployment create \
  --name demoDeployment \
  --location centralus \
  --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/policyassign.json \
  --parameters policyDefinitionID=$definition policyName=setLocation policyParameters="{'listOfAllowedLocations': {'value': ['westus']} }"
```

Um diese Vorlage mit PowerShell bereitzustellen, verwenden Sie:

```azurepowershell-interactive
$definition = Get-AzPolicyDefinition | Where-Object { $_.Properties.DisplayName -eq 'Allowed locations' }

$locations = @("westus", "westus2")
$policyParams =@{listOfAllowedLocations = @{ value = $locations}}

New-AzDeployment `
  -Name policyassign `
  -Location centralus `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/policyassign.json `
  -policyDefinitionID $definition.PolicyDefinitionId `
  -policyName setLocation `
  -policyParameters $policyParams
```

### <a name="define-and-assign-policy"></a>Definieren und Zuweisen einer Richtlinie

Sie können eine Richtlinie in derselben Vorlage [definieren](../../governance/policy/concepts/definition-structure.md) und zuweisen.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {},
  "variables": {},
  "resources": [
    {
      "type": "Microsoft.Authorization/policyDefinitions",
      "apiVersion": "2018-05-01",
      "name": "locationpolicy",
      "properties": {
        "policyType": "Custom",
        "parameters": {},
        "policyRule": {
          "if": {
            "field": "location",
            "equals": "northeurope"
          },
          "then": {
            "effect": "deny"
          }
        }
      }
    },
    {
      "type": "Microsoft.Authorization/policyAssignments",
      "apiVersion": "2018-05-01",
      "name": "location-lock",
      "dependsOn": [
        "locationpolicy"
      ],
      "properties": {
        "scope": "[subscription().id]",
        "policyDefinitionId": "[resourceId('Microsoft.Authorization/policyDefinitions', 'locationpolicy')]"
      }
    }
  ]
}
```

Mit dem folgenden CLI-Befehl können Sie die Richtliniendefinition im Abonnement erstellen und auf das Abonnement anwenden:

```azurecli
az deployment create \
  --name demoDeployment \
  --location centralus \
  --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/policydefineandassign.json
```

Um diese Vorlage mit PowerShell bereitzustellen, verwenden Sie:

```azurepowershell
New-AzDeployment `
  -Name definePolicy `
  -Location centralus `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/policydefineandassign.json
```

## <a name="template-samples"></a>Vorlagenbeispiele

* Erstellen Sie eine Ressourcengruppe, sperren Sie sie, und gewähren Sie die Berechtigungen dafür. Klicken Sie [hier](https://github.com/Azure/azure-quickstart-templates/tree/master/subscription-level-deployments/create-rg-lock-role-assignment).
* Erstellen Sie eine Ressourcengruppe, eine Richtlinie und eine Richtlinienzuweisung.  Klicken Sie [hier](https://github.com/Azure/azure-docs-json-samples/blob/master/subscription-level-deployment/azuredeploy.json).

## <a name="next-steps"></a>Nächste Schritte

* Informationen über das Zuweisen von Rollen finden Sie unter [Verwalten des Zugriffs auf Azure-Ressourcen mit RBAC und Azure Resource Manager-Vorlagen](../../role-based-access-control/role-assignments-template.md).
* Unter [deployASCwithWorkspaceSettings.json](https://github.com/krnese/AzureDeploy/blob/master/ARM/deployments/deployASCwithWorkspaceSettings.json) finden Sie ein Beispiel für die Bereitstellung von Arbeitsbereichseinstellungen für Azure Security Center.
* Ähnliche Beispielvorlagen sind auf [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/subscription-level-deployments) verfügbar.
* Weitere Informationen zum Erstellen von Azure-Ressourcen-Manager-Vorlagen finden Sie unter [Erstellen von Vorlagen](template-syntax.md).
* Eine Liste der verfügbaren Funktionen in einer Vorlage finden Sie unter [Funktionen von Azure Resource Manager-Vorlagen](template-functions.md).
