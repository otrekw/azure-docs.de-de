---
title: Bereitstellen von Ressourcen in einer Verwaltungsgruppe
description: In diesem Artikel wird beschrieben, wie Sie Ressourcen auf der Verwaltungsgruppenebene in einer Azure Resource Manager-Vorlage bereitstellen.
ms.topic: conceptual
ms.date: 11/07/2019
ms.openlocfilehash: e3661225dd69721ab223da0b44d69a592abb59bc
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2019
ms.locfileid: "75474308"
---
# <a name="create-resources-at-the-management-group-level"></a>Erstellen von Ressourcen auf der Verwaltungsgruppenebene

In der Regel stellen Sie Azure-Ressourcen für eine Ressourcengruppe in Ihrem Azure-Abonnement bereit. Sie können jedoch auch auf der Verwaltungsgruppenebene Ressourcen erstellen. Sie verwenden die Bereitstellungen auf Verwaltungsgruppenebene, um Aktionen durchzuführen, die auf dieser Ebene sinnvoll sind, etwa das Zuweisen von [rollenbasierter Zugriffssteuerung](../../role-based-access-control/overview.md) oder das Anwenden von [Richtlinien](../../governance/policy/overview.md).

Derzeit müssen Sie zum Bereitstellen von Vorlagen auf der Verwaltungsgruppenebene die REST-API verwenden.

## <a name="supported-resources"></a>Unterstützte Ressourcen

Sie können die folgenden Ressourcentypen auf der Verwaltungsgruppenebene bereitstellen:

* [Bereitstellungen](/azure/templates/microsoft.resources/deployments)
* [policyAssignments](/azure/templates/microsoft.authorization/policyassignments)
* [policyDefinitions](/azure/templates/microsoft.authorization/policydefinitions)
* [policySetDefinitions](/azure/templates/microsoft.authorization/policysetdefinitions)
* [roleAssignments](/azure/templates/microsoft.authorization/roleassignments)
* [roleDefinitions](/azure/templates/microsoft.authorization/roledefinitions)

### <a name="schema"></a>Schema

Das Schema, das Sie für Bereitstellungen auf Verwaltungsgruppenebene verwenden, unterscheidet sich von dem Schema für Ressourcengruppenbereitstellungen.

Verwenden Sie für Vorlagen Folgendes:

```json
https://schema.management.azure.com/schemas/2019-08-01/managementGroupDeploymentTemplate.json#
```

Verwenden Sie für Parameterdateien Folgendes:

```json
https://schema.management.azure.com/schemas/2019-08-01/managementGroupDeploymentParameters.json#
```

## <a name="deployment-commands"></a>Bereitstellungsbefehle

Der Befehl für Bereitstellungen auf Verwaltungsgruppenebene unterscheidet sich von dem Befehl für Ressourcengruppenbereitstellungen.

Verwenden Sie für die REST-API [Bereitstellungen – Erstellen im Verwaltungsgruppenbereich](/rest/api/resources/deployments/createorupdateatmanagementgroupscope).

## <a name="deployment-location-and-name"></a>Bereitstellungsspeicherort und -name

Für Bereitstellungen auf Verwaltungsgruppenebene müssen Sie einen Speicherort für die Bereitstellung angeben. Der Speicherort der Bereitstellung ist vom Speicherort der Ressourcen getrennt, die Sie bereitstellen. Der Bereitstellungsspeicherort gibt an, wo Bereitstellungsdaten gespeichert werden sollen.

Sie können einen Namen für die Bereitstellung angeben oder den Bereitstellungsstandardnamen verwenden. Der Standardname ist der Name der Vorlagendatei. Wenn Sie z.B. eine Vorlage mit dem Namen **azuredeploy.json** bereitstellen, wird **azuredeploy** als Standardname für die Bereitstellung erstellt.

Der Speicherort für jeden Bereitstellungsnamen ist unveränderlich. Sie können keine Bereitstellung an einem Speicherort erstellen, wenn bereits eine Bereitstellung mit demselben Namen an einem anderen Speicherort vorhanden ist. Wenn Sie den Fehlercode `InvalidDeploymentLocation` erhalten, verwenden Sie entweder einen anderen Namen oder denselben Speicherort wie bei der vorherigen Bereitstellung für diesen Namen.

## <a name="use-template-functions"></a>Verwenden von Vorlagenfunktionen

Bei Bereitstellungen auf Verwaltungsgruppenebene müssen bei der Verwendung von Vorlagenfunktionen einige wichtige Aspekte berücksichtigt werden:

* Die Funktion [resourceGroup()](template-functions-resource.md#resourcegroup) wird **nicht** unterstützt.
* Die Funktion [subscription()](template-functions-resource.md#subscription) wird **nicht** unterstützt.
* Die Funktion [resourceId()](template-functions-resource.md#resourceid) wird unterstützt. Verwenden sie diese Funktion zum Abrufen der Ressourcen-ID für Ressourcen, die in Bereitstellungen auf Verwaltungsgruppenebene verwendet werden. Rufen Sie beispielsweise die Ressourcen-ID für eine Richtliniendefinition mit `resourceId('Microsoft.Authorization/roleDefinitions/', parameters('roleDefinition'))` ab. Die Ressourcen-ID wird im Format `/providers/{resourceProviderNamespace}/{resourceType}/{resourceName}`zurückgegeben.
* Die Funktionen [reference()](template-functions-resource.md#reference) und [list()](template-functions-resource.md#list) werden unterstützt.

## <a name="create-policies"></a>Erstellen von Richtlinien

### <a name="define-policy"></a>Definieren einer Richtlinie

Im folgenden Beispiel wird veranschaulicht, wie eine Richtlinie auf der Verwaltungsgruppenebene [definiert](../../governance/policy/concepts/definition-structure.md) wird.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-08-01/managementGroupDeploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {},
    "variables": {},
    "resources": [
        {
            "type": "Microsoft.Authorization/policyDefinitions",
            "name": "locationpolicy",
            "apiVersion": "2018-05-01",
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
        }
    ]
}
```

### <a name="assign-policy"></a>Zuweisen der Richtlinie

Im folgenden Beispiel wird der Verwaltungsgruppe eine vorhandene Richtliniendefinition zugewiesen. Wenn die Richtlinie Parameter unterstützt, stellen Sie diese als Objekt bereit. Wenn die Richtlinie keine Parameter unterstützt, verwenden Sie das standardmäßige leere Objekt.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-08-01/managementGroupDeploymentTemplate.json#",
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
            "name": "[parameters('policyName')]",
            "apiVersion": "2018-03-01",
            "properties": {
                "policyDefinitionId": "[parameters('policyDefinitionID')]",
                "parameters": "[parameters('policyParameters')]"
            }
        }
    ]
}
```



## <a name="next-steps"></a>Nächste Schritte

* Informationen über das Zuweisen von Rollen finden Sie unter [Verwalten des Zugriffs auf Azure-Ressourcen mit RBAC und Azure Resource Manager-Vorlagen](../../role-based-access-control/role-assignments-template.md).
* Unter [deployASCwithWorkspaceSettings.json](https://github.com/krnese/AzureDeploy/blob/master/ARM/deployments/deployASCwithWorkspaceSettings.json) finden Sie ein Beispiel für die Bereitstellung von Arbeitsbereichseinstellungen für Azure Security Center.
* Weitere Informationen zum Erstellen von Azure-Ressourcen-Manager-Vorlagen finden Sie unter [Erstellen von Vorlagen](template-syntax.md). 
* Eine Liste der verfügbaren Funktionen in einer Vorlage finden Sie unter [Funktionen von Azure Resource Manager-Vorlagen](template-functions.md).