---
title: Erstellen von Diagnoseeinstellungen in Azure mithilfe einer Resource Manager-Vorlage
description: Erstellen Sie Diagnoseeinstellungen mithilfe einer Resource Manager-Vorlage, um Protokolle der Azure-Plattform an Azure Monitor-Protokolle, Azure Storage oder Azure Event Hubs weiterzuleiten.
author: bwren
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 12/13/2019
ms.author: bwren
ms.subservice: ''
ms.openlocfilehash: 2a171ae89e8314684eddf29f78b9b09bc52f9c9b
ms.sourcegitcommit: 3dc1a23a7570552f0d1cc2ffdfb915ea871e257c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/15/2020
ms.locfileid: "75977551"
---
# <a name="create-diagnostic-setting-in-azure-using-a-resource-manager-template"></a>Erstellen von Diagnoseeinstellungen in Azure mithilfe einer Resource Manager-Vorlage
[Diagnoseeinstellungen](diagnostic-settings.md) in Azure Monitor bezeichnen den Ort, an den die von Azure-Ressourcen erfassten [Plattformprotokolle](platform-logs-overview.md) gesendet werden sollen, sowie die Azure-Plattform, von der sie abhängen. Dieser Artikel enthält Details und Beispiele für die Verwendung einer [Azure Resource Manager-Vorlage](../../azure-resource-manager/templates/template-syntax.md) zum Erstellen und Konfigurieren von Diagnoseeinstellungen, um Plattformprotokolle an verschiedenen Zielen zu erfassen.

> [!NOTE]
> Da das [Erstellen einer Diagnoseeinstellung](diagnostic-settings.md) für das Azure-Aktivitätsprotokoll nicht mithilfe von PowerShell oder der CLI durchgeführt werden kann, wie es bei Diagnoseeinstellungen für andere Azure-Ressourcen der Fall ist, erstellen Sie anhand der Informationen in diesem Artikel eine Resource Manager-Vorlage für das Aktivitätsprotokoll und stellen dann die Vorlage mithilfe von PowerShell oder der CLI bereit.

## <a name="deployment-methods"></a>Bereitstellungsmethoden
Sie können Resource Manager-Vorlagen mithilfe einer gültigen Methode, einschließlich PowerShell und CLI, bereitstellen. Diagnoseeinstellungen für das Aktivitätsprotokoll müssen in einem Abonnement mit `az deployment create` für die CLI oder `New-AzDeployment` für PowerShell bereitgestellt werden. Diagnoseeinstellungen für Ressourcenprotokolle müssen in einer Ressourcengruppe mit `az group deployment create` für die CLI oder `New-AzResourceGroupDeployment` für PowerShell bereitgestellt werden.

Ausführliche Informationen finden Sie unter [Bereitstellen von Ressourcen mit Azure Resource Manager-Vorlagen und Azure PowerShell](../../azure-resource-manager/templates/deploy-powershell.md) und [Bereitstellen von Ressourcen mit Azure Resource Manager-Vorlagen und Azure-CLI](../../azure-resource-manager/templates/deploy-cli.md). 





## <a name="resource-logs"></a>Ressourcenprotokolle
Für Ressourcenprotokolle fügen Sie der Vorlage eine Ressource vom Typ `<resource namespace>/providers/diagnosticSettings` hinzu. Der Eigenschaftenabschnitt verwendet das unter [Diagnoseeinstellungen – Erstellen oder Aktualisieren](https://docs.microsoft.com/rest/api/monitor/diagnosticsettings/createorupdate) beschriebene Format. Geben Sie im Abschnitt `logs` ein `category`-Element für jede der Kategorien an, die für die zu erfassende Ressource gültig sind. Fügen Sie die `metrics`-Eigenschaft hinzu, um Ressourcenmetriken an denselben Ziele zu erfassen, falls die [Ressource Metriken unterstützt](metrics-supported.md).

Es folgt eine Vorlage, mit der eine Ressourcenprotokollkategorie für eine bestimmte Ressource in einem Log Analytics-Arbeitsbereich, einem Speicherkonto und einem Event Hub erfasst wird.

```json
"resources": [
  {
    "type": "/<resource namespace>/providers/diagnosticSettings",
    "name": "[concat(parameters('resourceName'),'/microsoft.insights/', parameters('settingName'))]",
    "dependsOn": [
      "[<resource Id for which resource logs will be enabled>]"
    ],
    "apiVersion": "2017-05-01-preview",
    "properties": {
      "name": "[parameters('settingName')]",
      "storageAccountId": "[resourceId('Microsoft.Storage/storageAccounts', parameters('storageAccountName'))]",
      "eventHubAuthorizationRuleId": "[parameters('eventHubAuthorizationRuleId')]",
      "eventHubName": "[parameters('eventHubName')]",
      "workspaceId": "[parameters('workspaceId')]",
      "logs": [
        {
          "category": "<category name>",
          "enabled": true
        }
      ],
      "metrics": [
        {
          "category": "AllMetrics",
          "enabled": true
        }
      ]
    }
  }
]
```



### <a name="example"></a>Beispiel
Es folgt ein Beispiel, bei dem eine Diagnoseeinstellung für eine Autoskalierungseinstellung erstellt wird, die das Streamen von Ressourcenprotokollen an einen Event Hub, in ein Speicherkonto und in einen Log Analytics-Arbeitsbereich ermöglicht.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "autoscaleSettingName": {
            "type": "string",
            "metadata": {
                "description": "The name of the autoscale setting"
            }
        },
        "settingName": {
            "type": "string",
            "metadata": {
                "description": "The name of the diagnostic setting"
            }
        },
        "workspaceId": {
            "type": "string",
            "metadata": {
                "description": "ResourceIDl of the Log Analytics workspace in which resource logs should be saved."
            }
        },
        "storageAccountId": {
            "type": "string",
            "metadata": {
              "description": "ResourceID of the Storage Account in which resource logs should be saved."
            }
        },
        "eventHubAuthorizationRuleId": {
            "type": "string",
            "metadata": {
              "description": "Resource ID of the event hub authorization rule for the Event Hubs namespace in which the event hub should be created or streamed to."
            }
        },
        "eventHubName": {
            "type": "string",
            "metadata": {
                "description": "Optional. Name of the event hub within the namespace to which logs are streamed. Without this, an event hub is created for each log category."
            }
        }
    },
    "variables": {},
    "resources": [
    {
      "type": "microsoft.insights/autoscalesettings/providers/diagnosticSettings",
      "apiVersion": "2017-05-01-preview",
      "name": "[concat(parameters('autoscaleSettingName'),'/microsoft.insights/', parameters('settingName'))]",
      "dependsOn": [
        "[resourceId('Microsoft.Insights/autoscalesettings', parameters('autoscaleSettingName'))]"
      ],
      "properties": {
        "workspaceId": "[parameters('workspaceId')]",
        "storageAccountId": "[parameters('storageAccountId')]",
        "eventHubAuthorizationRuleId": "[parameters('eventHubAuthorizationRuleId')]",
        "eventHubName": "[parameters('eventHubName')]",
        "logs": [
          {
            "category": "AutoscaleScaleActions",
            "enabled": true
          },
          {
            "category": "AutoscaleEvaluations",
            "enabled": true
          }
        ]
      }
    }
  ]
}
```

## <a name="activity-log"></a>Aktivitätsprotokoll
Für das Azure-Aktivitätsprotokoll fügen Sie eine Ressource vom Typ `Microsoft.Insights/diagnosticSettings` hinzu. Die verfügbaren Kategorien sind unter [Kategorien im Aktivitätsprotokoll](activity-log-view.md#categories-in-the-activity-log) aufgelistet. Es folgt eine Vorlage, mit der alle Aktivitätsprotokollkategorien in einem Log Analytics-Arbeitsbereich, einem Speicherkonto und einem Event Hub erfasst werden.


```json
{
    "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "settingName": {
            "type": "string",
            "metadata": {
                "description": "The name of the diagnostic setting"
            }
        },
        "workspaceId": {
            "type": "string",
            "metadata": {
                "description": "ResourceID of the Log Analytics workspace in which resource logs should be saved."
            }
        },
        "storageAccountId": {
            "type": "string",
            "metadata": {
              "description": "ResourceID of the Storage Account in which resource logs should be saved."
            }
        },
        "eventHubAuthorizationRuleId": {
            "type": "string",
            "metadata": {
              "description": "Resource ID of the event hub authorization rule for the Event Hubs namespace in which the event hub should be created or streamed to."
            }
        },
        "eventHubName": {
            "type": "string",
            "metadata": {
                "description": "Optional. Name of the event hub within the namespace to which logs are streamed. Without this, an event hub is created for each log category."
            }
        }
    },
    "variables": {},
    "resources": [
        {
            "type": "Microsoft.Insights/diagnosticSettings",
            "apiVersion": "2017-05-01-preview",
            "name": "[parameters('settingName')]",
            "location": "global",
            "properties": {
                "workspaceId": "[parameters('workspaceId')]",
                "storageAccountId": "[parameters('storageAccountId')]",
                "eventHubAuthorizationRuleId": "[parameters('eventHubAuthorizationRuleId')]",
                "eventHubName": "[parameters('eventHubName')]",
                "logs": [
                    {
                        "category": "Administrative",
                        "enabled": true
                    },
                    {
                        "category": "Security",
                        "enabled": true
                    },
                    {
                        "category": "ServiceHealth",
                        "enabled": true
                    },
                    {
                        "category": "Alert",
                        "enabled": true
                    },
                    {
                        "category": "Recommendation",
                        "enabled": true
                    },
                    {
                        "category": "Policy",
                        "enabled": true
                    },
                    {
                        "category": "Autoscale",
                        "enabled": true
                    },
                    {
                        "category": "ResourceHealth",
                        "enabled": true
                    }
                ]
            }
        }
    ]
}
```


## <a name="next-steps"></a>Nächste Schritte
* Informieren Sie sich ausführlicher über [Plattformprotokolle in Azure](platform-logs-overview.md).
* Erfahren Sie mehr über [Diagnoseeinstellungen](diagnostic-settings.md).
