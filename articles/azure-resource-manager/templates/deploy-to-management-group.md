---
title: Bereitstellen von Ressourcen in einer Verwaltungsgruppe
description: In diesem Artikel wird beschrieben, wie Sie Ressourcen auf der Verwaltungsgruppenebene in einer Azure Resource Manager-Vorlage bereitstellen.
ms.topic: conceptual
ms.date: 09/15/2020
ms.openlocfilehash: 2325e9f5a03f7451492c9b9b8e929df95ddc3852
ms.sourcegitcommit: 80b9c8ef63cc75b226db5513ad81368b8ab28a28
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/16/2020
ms.locfileid: "90605225"
---
# <a name="create-resources-at-the-management-group-level"></a>Erstellen von Ressourcen auf der Verwaltungsgruppenebene

Wenn Ihre Organisation sich weiterentwickelt, können Sie eine Azure Resource Manager-Vorlage (ARM-Vorlage) bereitstellen, um Ressourcen auf Verwaltungsgruppenebene zu erstellen. Beispielsweise müssen Sie möglicherweise [Richtlinien](../../governance/policy/overview.md) oder die [rollenbasierte Zugriffssteuerung von Azure (Azure Role-Based Access Control, Azure-RBAC)](../../role-based-access-control/overview.md) für eine Verwaltungsgruppe definieren und zuweisen. Mit Vorlagen auf Verwaltungsgruppenebene können Sie Richtlinien deklarativ anwenden und Rollen auf Verwaltungsgruppenebene zuweisen.

## <a name="supported-resources"></a>Unterstützte Ressourcen

Nicht alle Ressourcentypen können auf Mandantengruppenebene bereitgestellt werden. Im folgenden Abschnitt werden die unterstützten Ressourcentypen aufgelistet.

Verwenden Sie für Azure Blueprints:

* [artifacts](/azure/templates/microsoft.blueprint/blueprints/artifacts)
* [blueprints](/azure/templates/microsoft.blueprint/blueprints)
* [blueprintAssignments](/azure/templates/microsoft.blueprint/blueprintassignments)
* [versions](/azure/templates/microsoft.blueprint/blueprints/versions)

Verwenden Sie für Azure-Richtlinien:

* [policyAssignments](/azure/templates/microsoft.authorization/policyassignments)
* [policyDefinitions](/azure/templates/microsoft.authorization/policydefinitions)
* [policySetDefinitions](/azure/templates/microsoft.authorization/policysetdefinitions)
* [remediations](/azure/templates/microsoft.policyinsights/remediations)

Verwenden Sie für rollenbasierte Zugriffssteuerung:

* [roleAssignments](/azure/templates/microsoft.authorization/roleassignments)
* [roleDefinitions](/azure/templates/microsoft.authorization/roledefinitions)

Verwenden Sie für geschachtelte Vorlagen, die in Abonnements oder Ressourcengruppen bereitstellen:

* [Bereitstellungen](/azure/templates/microsoft.resources/deployments)

Verwenden Sie für die Verwaltung Ihrer Ressourcen:

* [Tags](/azure/templates/microsoft.resources/tags)

### <a name="schema"></a>Schema

Das Schema, das Sie für Bereitstellungen auf Verwaltungsgruppenebene verwenden, unterscheidet sich von dem Schema für Ressourcengruppenbereitstellungen.

Verwenden Sie für Vorlagen Folgendes:

```json
https://schema.management.azure.com/schemas/2019-08-01/managementGroupDeploymentTemplate.json#
```

Das Schema für eine Parameterdatei ist für alle Bereitstellungsbereiche identisch. Verwenden Sie für Parameterdateien Folgendes:

```json
https://schema.management.azure.com/schemas/2019-04-01/deploymentParameters.json#
```

## <a name="deployment-commands"></a>Bereitstellungsbefehle

Die Befehle für Verwaltungsgruppenbereitstellungen unterscheiden sich von den Befehlen für Ressourcengruppenbereitstellungen.

Verwenden Sie bei der Azure CLI [az deployment mg create](/cli/azure/deployment/mg#az-deployment-mg-create):

```azurecli-interactive
az deployment mg create \
  --name demoMGDeployment \
  --location WestUS \
  --management-group-id myMG \
  --template-uri "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/management-level-deployment/azuredeploy.json"
```

Verwenden Sie für Azure PowerShell [New-AzManagementGroupDeployment](/powershell/module/az.resources/new-azmanagementgroupdeployment).

```azurepowershell-interactive
New-AzManagementGroupDeployment `
  -Name demoMGDeployment `
  -Location "West US" `
  -ManagementGroupId "myMG" `
  -TemplateUri "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/management-level-deployment/azuredeploy.json"
```

Verwenden Sie für die REST-API [Bereitstellungen – Erstellen im Verwaltungsgruppenbereich](/rest/api/resources/deployments/createorupdateatmanagementgroupscope).

## <a name="deployment-location-and-name"></a>Bereitstellungsspeicherort und -name

Für Bereitstellungen auf Verwaltungsgruppenebene müssen Sie einen Speicherort für die Bereitstellung angeben. Der Speicherort der Bereitstellung ist vom Speicherort der Ressourcen getrennt, die Sie bereitstellen. Der Bereitstellungsspeicherort gibt an, wo Bereitstellungsdaten gespeichert werden sollen.

Sie können einen Namen für die Bereitstellung angeben oder den Bereitstellungsstandardnamen verwenden. Der Standardname ist der Name der Vorlagendatei. Wenn Sie z.B. eine Vorlage mit dem Namen **azuredeploy.json** bereitstellen, wird **azuredeploy** als Standardname für die Bereitstellung erstellt.

Der Speicherort für jeden Bereitstellungsnamen ist unveränderlich. Sie können keine Bereitstellung an einem Speicherort erstellen, wenn bereits eine Bereitstellung mit demselben Namen an einem anderen Speicherort vorhanden ist. Wenn Sie den Fehlercode `InvalidDeploymentLocation` erhalten, verwenden Sie entweder einen anderen Namen oder denselben Speicherort wie bei der vorherigen Bereitstellung für diesen Namen.

## <a name="deployment-scopes"></a>Bereitstellungsbereiche

Wenn Sie in einer Verwaltungsgruppe bereitstellen, können Sie auf die im Bereitstellungsbefehl angegebene Verwaltungsgruppe oder auf andere Verwaltungsgruppen im Mandanten abzielen. Sie können auch auf Abonnements oder Ressourcengruppen innerhalb einer Verwaltungsgruppe abzielen. Der Benutzer, der die Vorlage bereitstellt, muss Zugriff auf den angegebenen Bereich besitzen.

Ressourcen, die im Ressourcenabschnitt der Vorlage definiert sind, werden vom Bereitstellungsbefehl auf die Verwaltungsgruppe angewendet.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-08-01/managementGroupDeploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
        management-group-level-resources
    ],
    "outputs": {}
}
```

Um eine andere Verwaltungsgruppe als Ziel zu verwenden, fügen Sie eine geschachtelte Bereitstellung hinzu, und geben Sie die `scope`-Eigenschaft an.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-08-01/managementGroupDeploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "mgName": {
            "type": "string"
        }
    },
    "variables": {
        "mgId": "[concat('Microsoft.Management/managementGroups/', parameters('mgName'))]"
    },
    "resources": [
        {
            "type": "Microsoft.Resources/deployments",
            "apiVersion": "2019-10-01",
            "name": "nestedDeployment",
            "scope": "[variables('mgId')]",
            "location": "eastus",
            "properties": {
                "mode": "Incremental",
                "template": {
                    nested-template-with-resources-in-different-mg
                }
            }
        }
    ],
    "outputs": {}
}
```

Um ein Abonnement innerhalb der Verwaltungsgruppe als Ziel zu verwenden, verwenden Sie eine geschachtelte Bereitstellung und die `subscriptionId`-Eigenschaft. Um eine Ressourcengruppe innerhalb dieses Abonnements als Ziel zu verwenden, fügen Sie eine weitere geschachtelte Bereitstellung hinzu sowie die `resourceGroup`-Eigenschaft.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-08-01/managementGroupDeploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "resources": [
    {
      "type": "Microsoft.Resources/deployments",
      "apiVersion": "2020-06-01",
      "name": "nestedSub",
      "location": "westus2",
      "subscriptionId": "00000000-0000-0000-0000-000000000000",
      "properties": {
        "mode": "Incremental",
        "template": {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
          "contentVersion": "1.0.0.0",
          "resources": [
            {
              "type": "Microsoft.Resources/deployments",
              "apiVersion": "2020-06-01",
              "name": "nestedRG",
              "resourceGroup": "rg2",
              "properties": {
                "mode": "Incremental",
                "template": {
                  nested-template-with-resources-in-resource-group
                }
              }
            }
          ]
        }
      }
    }
  ]
}
```

Informationen zur Verwendung einer Verwaltungsgruppenbereitstellung zum Erstellen einer Ressourcengruppe in einem Abonnement und zum Bereitstellen eines Speicherkontos für diese Ressourcengruppe finden Sie unter [Bereitstellen in Abonnements und Ressourcengruppen](#deploy-to-subscription-and-resource-group).

## <a name="use-template-functions"></a>Verwenden von Vorlagenfunktionen

Bei Bereitstellungen auf Verwaltungsgruppenebene müssen bei der Verwendung von Vorlagenfunktionen einige wichtige Aspekte berücksichtigt werden:

* Die Funktion [resourceGroup()](template-functions-resource.md#resourcegroup) wird **nicht** unterstützt.
* Die Funktion [subscription()](template-functions-resource.md#subscription) wird **nicht** unterstützt.
* Die Funktionen [reference()](template-functions-resource.md#reference) und [list()](template-functions-resource.md#list) werden unterstützt.
* Verwenden Sie die [resourceId()](template-functions-resource.md#resourceid)-Funktion nicht für Ressourcen, die in der Verwaltungsgruppe bereitgestellt werden.

  Verwenden Sie stattdessen die [extensionResourceId()](template-functions-resource.md#extensionresourceid)-Funktion für Ressourcen, die als Erweiterungen der Verwaltungsgruppe implementiert werden. Benutzerdefinierte Richtliniendefinitionen, die für die Verwaltungsgruppe bereitgestellt werden, sind Erweiterungen der Verwaltungsgruppe.

  Verwenden Sie den folgenden Code, um die Ressourcen-ID für eine benutzerdefinierte Richtliniendefinition auf Verwaltungsgruppenebene zu erhalten:
  
  ```json
  "policyDefinitionId": "[extensionResourceId(variables('mgScope'), 'Microsoft.Authorization/policyDefinitions', parameters('policyDefinitionID'))]"
  ```

  Verwenden Sie die Funktion [tenantResourceId](template-functions-resource.md#tenantresourceid) für Mandantenressourcen, die in der Verwaltungsgruppe verfügbar sind. Integrierte Richtliniendefinitionen sind Ressourcen auf Mandantenebene.

  Verwenden Sie beispielsweise den folgenden Code, um die Ressourcen-ID für eine integrierte Richtliniendefinition abzurufen:
  
  ```json
  "policyDefinitionId": "[tenantResourceId('Microsoft.Authorization/policyDefinitions', parameters('policyDefinitionID'))]"
  ```

## <a name="azure-policy"></a>Azure Policy

Im folgenden Beispiel wird veranschaulicht, wie eine Richtlinie auf der Verwaltungsgruppenebene [definiert](../../governance/policy/concepts/definition-structure.md) und zugewiesen wird.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-08-01/managementGroupDeploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "targetMG": {
            "type": "string",
            "metadata": {
                "description": "Target Management Group"
            }
        },
        "allowedLocations": {
            "type": "array",
            "defaultValue": [
                "australiaeast",
                "australiasoutheast",
                "australiacentral"
            ],
            "metadata": {
                "description": "An array of the allowed locations, all other locations will be denied by the created policy."
            }
        }
    },
    "variables": {
        "mgScope": "[tenantResourceId('Microsoft.Management/managementGroups', parameters('targetMG'))]",
        "policyDefinition": "LocationRestriction"
    },
    "resources": [
        {
            "type": "Microsoft.Authorization/policyDefinitions",
            "name": "[variables('policyDefinition')]",
            "apiVersion": "2019-09-01",
            "properties": {
                "policyType": "Custom",
                "mode": "All",
                "parameters": {
                },
                "policyRule": {
                    "if": {
                        "not": {
                            "field": "location",
                            "in": "[parameters('allowedLocations')]"
                        }
                    },
                    "then": {
                        "effect": "deny"
                    }
                }
            }
        },
        {
            "type": "Microsoft.Authorization/policyAssignments",
            "name": "location-lock",
            "apiVersion": "2019-09-01",
            "dependsOn": [
                "[variables('policyDefinition')]"
            ],
            "properties": {
                "scope": "[variables('mgScope')]",
                "policyDefinitionId": "[extensionResourceId(variables('mgScope'), 'Microsoft.Authorization/policyDefinitions', variables('policyDefinition'))]"
            }
        }
    ]
}
```

## <a name="deploy-to-subscription-and-resource-group"></a>Bereitstellen in Abonnements und Ressourcengruppen

Aus einer Bereitstellung auf Verwaltungsgruppenebene können Sie ein Abonnement innerhalb der Verwaltungsgruppe als Ziel verwenden. Das folgende Beispiel erstellt eine Ressourcengruppe innerhalb eines Abonnements und stellt in dieser Ressourcengruppe ein Speicherkonto bereit.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-08-01/managementGroupDeploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "nestedsubId": {
      "type": "string"
    },
    "nestedRG": {
      "type": "string"
    },
    "storageAccountName": {
      "type": "string"
    },
    "nestedLocation": {
      "type": "string"
    }
  },
  "resources": [
    {
      "type": "Microsoft.Resources/deployments",
      "apiVersion": "2020-06-01",
      "name": "nestedSub",
      "location": "[parameters('nestedLocation')]",
      "subscriptionId": "[parameters('nestedSubId')]",
      "properties": {
        "mode": "Incremental",
        "template": {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
          "contentVersion": "1.0.0.0",
          "parameters": {
          },
          "variables": {
          },
          "resources": [
            {
              "type": "Microsoft.Resources/resourceGroups",
              "apiVersion": "2020-06-01",
              "name": "[parameters('nestedRG')]",
              "location": "[parameters('nestedLocation')]",
            },
            {
              "type": "Microsoft.Resources/deployments",
              "apiVersion": "2020-06-01",
              "name": "nestedSubRG",
              "resourceGroup": "[parameters('nestedRG')]",
              "dependsOn": [
                "[parameters('nestedRG')]"
              ],
              "properties": {
                "mode": "Incremental",
                "template": {
                  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
                  "contentVersion": "1.0.0.0",
                  "resources": [
                    {
                      "type": "Microsoft.Storage/storageAccounts",
                      "apiVersion": "2019-04-01",
                      "name": "[parameters('storageAccountName')]",
                      "location": "[parameters('nestedLocation')]",
                      "sku": {
                        "name": "Standard_LRS"
                      }
                    }
                  ]
                }
              }
            }
          ]
        }
      }
    }
  ]
}
```

## <a name="next-steps"></a>Nächste Schritte

* Informationen zum Zuweisen von Rollen finden Sie unter [Hinzufügen von Azure-Rollenzuweisungen mithilfe von Azure Resource Manager-Vorlagen](../../role-based-access-control/role-assignments-template.md).
* Unter [deployASCwithWorkspaceSettings.json](https://github.com/krnese/AzureDeploy/blob/master/ARM/deployments/deployASCwithWorkspaceSettings.json) finden Sie ein Beispiel für die Bereitstellung von Arbeitsbereichseinstellungen für Azure Security Center.
* Sie können Vorlagen auch auf [Abonnementebene](deploy-to-subscription.md) und [Mandantenebene](deploy-to-tenant.md) bereitstellen.
