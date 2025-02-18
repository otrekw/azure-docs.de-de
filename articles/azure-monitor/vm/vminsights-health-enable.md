---
title: Aktivieren der Gastintegrität in VM Insights (Vorschau)
description: In diesem Artikel wird beschrieben, wie Sie das Feature „Gastintegrität“ von VM Insights in Ihrem Abonnement aktivieren und das Onboarding für VMs durchführen.
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 04/05/2021
ms.custom: references_regions, devx-track-azurepowershell
ms.openlocfilehash: f38172e82b61ce07863614857ab7ef096683f34a
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2021
ms.locfileid: "122346120"
---
# <a name="enable-vm-insights-guest-health-preview"></a>Aktivieren der Gastintegrität in VM Insights (Vorschau)
Mit dem Feature „Gastintegrität“ von VM Insights können Sie Informationen zur Integrität einer VM anzeigen. Dies wird anhand von verschiedenen Leistungsmessungen definiert, für die in regelmäßigen Abständen Stichproben genommen werden. In diesem Artikel wird beschrieben, wie Sie dieses Feature in Ihrem Abonnement aktivieren und die Gastüberwachung für die einzelnen virtuellen Computer aktivieren.

## <a name="current-limitations"></a>Aktuelle Einschränkungen
Das Feature „Gastintegrität“ von VM Insights verfügt in der öffentlichen Vorschauphase über die folgenden Einschränkungen:

- Aktuell werden nur virtuelle Azure-Computer unterstützt. Azure Arc für Server wird derzeit nicht unterstützt.
- Netzwerkproxys werden derzeit nicht unterstützt.


## <a name="supported-operating-systems"></a>Unterstützte Betriebssysteme
Auf dem virtuellen Computer muss eines der folgenden Betriebssysteme ausgeführt werden: 

  - CentOS 7.5, 7.6, 7.7, 7.8, 7.9
  - RedHat 7.5, 7.6, 7.7, 7.8, 7.9
  - Ubuntu 16.04 LTS, Ubuntu 18.04 LTS
  - Windows Server 2012 oder höher

## <a name="supported-regions"></a>Unterstützte Regionen

Der virtuelle Computer muss sich in einer der folgenden Regionen befinden:

- Australien, Mitte
- Australien (Osten)
- Australien, Südosten
- Brasilien Süd
- Brasilien, Südosten
- Kanada, Mitte
- Indien, Mitte
- USA (Mitte)
- Asien, Osten
- East US
- USA (Ost) 2
- USA, Osten 2 (EUAP)
- Frankreich, Mitte
- Deutschland, Westen-Mitte
- Japan, Osten
- Japan, Westen
- Korea, Mitte
- USA Nord Mitte
- Nordeuropa
- Norwegen, Osten
- USA Süd Mitte
- Südafrika, Norden
- Asien, Südosten
- Schweiz, Norden
- Schweiz, Westen
- Vereinigte Arabische Emirate, Norden
- UK, Süden
- UK, Westen
- USA, Westen-Mitte
- Europa, Westen
- USA (Westen)
- USA, Westen 2


Der Log Analytics-Arbeitsbereich muss sich in einer der folgenden Regionen befinden:

- Australien, Mitte
- Australien (Osten)
- Australien, Südosten
- Kanada, Mitte
- Indien, Mitte
- USA (Mitte)
- Asien, Osten
- East US
- USA (Ost) 2
- USA, Osten 2 (EUAP)
- Frankreich, Mitte
- Japan, Osten
- USA Nord Mitte
- Nordeuropa
- USA Süd Mitte
- Asien, Südosten
- Schweiz, Norden
- UK, Süden
- Region „Europa, Westen“
- USA (Westen)
- USA, Westen 2

## <a name="prerequisites"></a>Voraussetzungen

- Für die VM muss ein Onboarding in VM Insights durchgeführt werden.
- Der Benutzer, der die Schritte für das Onboarding ausführt, muss mindestens über die Zugriffsebene „Mitwirkender“ für das Abonnement verfügen, unter dem sich der virtuelle Computer und die Datensammlungsregel befinden.
- Erforderliche Azure-Ressourcenanbieter müssen wie im folgenden Abschnitt beschrieben registriert werden.

## <a name="register-required-azure-resource-providers"></a>Registrieren von erforderlichen Azure-Ressourcenanbietern
Die folgenden Azure-Ressourcenanbieter müssen für Ihr Abonnement registriert werden, damit das Feature „Gastintegrität“ von VM Insights aktiviert werden kann. 

- Microsoft.WorkloadMonitor
- Microsoft.Insights

Sie können eine der verfügbaren Methoden zum Registrieren eines Ressourcenanbieters verwenden, die unter [Azure-Ressourcenanbieter und -typen](../../azure-resource-manager/management/resource-providers-and-types.md) beschrieben sind. Sie können auch den folgenden Beispielbefehl mit ARMClient, Postman oder einer anderen Methode verwenden, um Azure Resource Manager mit Authentifizierung aufzurufen:

```
POST https://management.azure.com/subscriptions/[subscriptionId]/providers/Microsoft.WorkloadMonitor/register?api-version=2019-10-01
POST https://management.azure.com/subscriptions/[subscriptionId]/providers/Microsoft.Insights/register?api-version=2019-10-01
```


## <a name="enable-a-virtual-machine-using-the-azure-portal"></a>Aktivieren eines virtuellen Computers über das Azure-Portal
Wenn Sie „Gastintegrität“ für einen virtuellen Computer im Azure-Portal aktivieren, wird die gesamte erforderliche Konfiguration für Sie durchgeführt. Dies umfasst auch die Erstellung der benötigten Datensammlungsregel, die Installation der Gastintegrität-Erweiterung auf dem virtuellen Computer und die Erstellung einer Zuordnung zur Datensammlungsregel.

Klicken Sie in der Ansicht **Los geht‘s** in VM Insights auf den Link neben der Upgradenachricht für eine VM und dann auf **Upgrade**. Sie können auch mehrere virtuelle Computer auswählen, um den Upgradevorgang dafür zusammen durchzuführen.

![Aktivieren des Integritätsfeatures auf dem virtuellen Computer](media/vminsights-health-enable/enable-agent.png)


## <a name="enable-a-virtual-machine-using-resource-manager-template"></a>Aktivieren eines virtuellen Computers per Resource Manager-Vorlage
Es sind drei Schritte erforderlich, um virtuelle Computer per Azure Resource Manager zu aktivieren.

- Erstellen einer Datensammlungsregel
- Installieren der Erweiterung für die Gastintegrität auf jedem virtuellen Computer
- Erstellen einer Zuordnung zwischen dem virtuellen Computer und der Datensammlungsregel

### <a name="create-data-collection-rule-dcr"></a>Erstellen einer Datensammlungsregel

> [!NOTE]
> Wenn Sie einen virtuellen Computer über das Azure-Portal aktivieren, wird die hier beschriebene Datensammlungsregel für Sie erstellt. In diesem Fall müssen Sie diesen Schritt nicht ausführen.

Die Konfiguration der Monitore des VM Insights-Features „Gastintegrität“ wird in den [Datensammlungsregeln](../agents/data-collection-rule-overview.md) gespeichert. Für jeden virtuellen Computer mit der Erweiterung für die Gastintegrität wird eine Zuordnung dieser Regel benötigt.

> [!NOTE]
> Sie können weitere Datensammlungsregeln erstellen, um die Standardkonfiguration von Monitoren zu ändern. Dies wird unter [Konfigurieren der Überwachung für das VM Insights-Feature „Gastintegrität“ (Vorschau)](vminsights-health-configure.md) beschrieben.

Für die Vorlage sind Werte für die folgenden Parameter erforderlich:

- **defaultHealthDataCollectionRuleName**: Behalten Sie den Standardnamen bei, der in der Vorlage definiert ist.
- **destinationWorkspaceResourceId**: Die Ressourcen-ID des Log Analytics-Arbeitsbereichs, der für die Datensammlung für die virtuellen Computer verwendet wird.
- **dataCollectionRuleLocation**: Die Region der Datensammlungsregel. Sie muss mit der Region des Log Analytics-Arbeitsbereichs übereinstimmen.


Stellen Sie die Vorlage mit einer [beliebigen Bereitstellungsmethode für Resource Manager-Vorlagen](../../azure-resource-manager/templates/deploy-powershell.md) bereit. Mit den folgenden Befehlen können Sie die Vorlage und die Parameterdatei per PowerShell oder mit der Azure CLI bereitstellen.

# <a name="powershell"></a>[PowerShell](#tab/powershell)

```powershell
New-AzResourceGroupDeployment -Name GuestHealthDataCollectionRule -ResourceGroupName my-resource-group -TemplateFile Health.DataCollectionRule.template.json -TemplateParameterFile Health.DataCollectionRule.template.parameters.json
```

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

```azurecli
az deployment group create --name GuestHealthDataCollectionRule --resource-group my-resource-group --template-file Health.DataCollectionRule.template.json --parameters Health.DataCollectionRule.template.parameters.json
```

---

Mit der in der Resource Manager-Vorlage unten definierten Datensammlungsregel werden alle Monitore für die virtuellen Computer mit der Erweiterung für die Gastintegrität aktiviert. Sie muss Datenquellen für jeden von den Monitoren verwendeten Leistungsindikator enthalten.

```json
{
  "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "defaultHealthDataCollectionRuleName": {
      "type": "string",
      "metadata": {
        "description": "Specifies the name of the data collection rule to create."
      },
      "defaultValue": "Microsoft-VMInsights-Health"
    },
    "destinationWorkspaceResourceId": {
      "type": "string",
      "metadata": {
        "description": "Specifies the Azure resource ID of the Log Analytics workspace to use to store virtual machine health data."
      }
    },
    "dataCollectionRuleLocation": {
      "type": "string",
      "metadata": {
        "description": "The location code in which the data collection rule should be deployed. Examples: eastus, westeurope, etc"
      }
    }
  },
  "resources": [
    {
      "type": "Microsoft.Insights/dataCollectionRules",
      "name": "[parameters('defaultHealthDataCollectionRuleName')]",
      "location": "[parameters('dataCollectionRuleLocation')]",
      "apiVersion": "2019-11-01-preview",
      "properties": {
        "description": "Data collection rule for VM Insights health.",
        "dataSources": {
          "performanceCounters": [
              {
                  "name": "VMHealthPerfCounters",
                  "streams": [ "Microsoft-Perf" ],
                  "scheduledTransferPeriod": "PT1M",
                  "samplingFrequencyInSeconds": 60,
                  "counterSpecifiers": [
                      "\\LogicalDisk(*)\\% Free Space",
                      "\\Memory\\Available Bytes",
                      "\\Processor(_Total)\\% Processor Time"
                  ]
              }
          ],
          "extensions": [
            {
              "name": "Microsoft-VMInsights-Health",
              "streams": [
                "Microsoft-HealthStateChange"
              ],
              "extensionName": "HealthExtension",
              "extensionSettings": {
                "schemaVersion": "1.0",
                "contentVersion": "",
                "healthRuleOverrides": [
                  {
                    "scopes": [ "*" ],
                    "monitors": ["root"],
                    "alertConfiguration": {
                      "isEnabled": true
                    }
                  }
                ]
              },
              "inputDataSources": [
                  "VMHealthPerfCounters"
              ]

            }
          ]
        },
        "destinations": {
          "logAnalytics": [
            {
              "workspaceResourceId": "[parameters('destinationWorkspaceResourceId')]",
              "name": "Microsoft-HealthStateChange-Dest"
            }
          ]
        },                  
        "dataFlows": [
          {
            "streams": [
              "Microsoft-HealthStateChange"
            ],
            "destinations": [
              "Microsoft-HealthStateChange-Dest"
            ]
          }
        ]
      }
    }
  ]
}
```

### <a name="sample-parameter-file"></a>Beispiel für Parameterdatei

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
      "destinationWorkspaceResourceId": {
        "value": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups/my-resource-group/providers/microsoft.operationalinsights/workspaces/my-workspace"
      },
      "dataCollectionRuleLocation": {
        "value": "eastus"
      }
  }
}
```



### <a name="install-guest-health-extension-and-associate-with-data-collection-rule"></a>Installieren der Erweiterung für die Gastintegrität und Zuordnen zur Datensammlungsregel
Verwenden Sie die folgende Resource Manager-Vorlage, um einen virtuellen Computer für die Gastintegrität zu aktivieren. Hiermit wird die Erweiterung für die Gastintegrität installiert und die Zuordnung zur Datensammlungsregel durchgeführt. Sie können diese Vorlage mit einer [beliebigen Bereitstellungsmethode für Resource Manager-Vorlagen](../../azure-resource-manager/templates/deploy-powershell.md) bereitstellen.


Verwenden Sie beispielsweise die folgenden Befehle, um die Vorlagen- und Parameterdatei für eine Ressourcengruppe mithilfe von PowerShell oder der Azure CLI bereitzustellen.


# <a name="powershell"></a>[PowerShell](#tab/powershell)

```powershell
New-AzResourceGroupDeployment -Name GuestHealthDeployment -ResourceGroupName my-resource-group -TemplateFile azure-monitor-deploy.json -TemplateParameterFile azure-monitor-deploy.parameters.json
```

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

```azurecli
az deployment group create --name GuestHealthDeployment --resource-group my-resource-group --template-file Health.VirtualMachine.template.json --parameters Health.VirtualMachine.template.parameters.json
```

---

### <a name="template-file"></a>Vorlagendatei

``` json
{
  "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "virtualMachineName": {
      "type": "string",
      "metadata": {
        "description": "Specifies the name of the virtual machine."
      }
    },
    "virtualMachineLocation": {
      "type": "string",
      "metadata": {
        "description": "The location code of the virtual machine region (location). Examples: eastus, westeurope, etc"
      }
    },
    "virtualMachineOsType": {
      "type": "string",
      "metadata": {
        "description": "Specifies operating system type of the target virtual machine."
      },
      "allowedValues": ["windows", "linux"]
    },
    "dataCollectionRuleAssociationName": {
      "type": "string",
      "metadata": {
        "description": "Specifies the name of the data collection rule association to create."
      },
      "defaultValue": "VM-Health-Dcr-Association"
    },
    "healthDataCollectionRuleResourceId": {
      "type": "string",
      "metadata": {
        "description": "Specifies resource id of Azure Monitor Data Collection Rule for virtual machine health data."
      }
    }
  },
  "variables": {
    "healthExtensionProperties": {
      "windows": {
        "publisher": "Microsoft.Azure.Monitor.VirtualMachines.GuestHealth",
        "type": "GuestHealthWindowsAgent",
        "typeHandlerVersion": "1.0",
        "autoUpgradeMinorVersion": true
      },
      "linux": {
        "publisher": "Microsoft.Azure.Monitor.VirtualMachines.GuestHealth",
        "type": "GuestHealthLinuxAgent",
        "typeHandlerVersion": "1.0",
        "autoUpgradeMinorVersion": true
      }
    },
    "azureMonitorAgentExtensionProperties": {
      "windows": {
        "publisher": "Microsoft.Azure.Monitor", 
        "type": "AzureMonitorWindowsAgent", 
        "typeHandlerVersion": "1.0", 
        "autoUpgradeMinorVersion": true 
      },
      "linux": {
        "publisher": "Microsoft.Azure.Monitor", 
        "type": "AzureMonitorLinuxAgent", 
        "typeHandlerVersion": "1.0", 
        "autoUpgradeMinorVersion": true 
      }
    }
  },
  "resources": [
    {
      "type": "Microsoft.Compute/virtualMachines",
      "name": "[parameters('virtualMachineName')]",
      "location": "[parameters('virtualMachineLocation')]",
      "apiVersion": "2017-03-30",
      "identity": {
        "type": "SystemAssigned"
      }
    },
    {
      "type": "Microsoft.Compute/virtualMachines/providers/dataCollectionRuleAssociations",
      "name": "[concat(parameters('virtualMachineName'), '/Microsoft.Insights/', parameters('dataCollectionRuleAssociationName'))]",
      "apiVersion": "2019-11-01-preview",
      "properties": {
        "description": "Association of data collection rule for VM Insights Health.",
        "dataCollectionRuleId": "[parameters('healthDataCollectionRuleResourceId')]"
      }
    },
    { 
      "type": "Microsoft.Compute/virtualMachines/extensions", 
      "apiVersion": "2019-12-01", 
      "name": "[concat(parameters('virtualMachineName'), '/', variables('azureMonitorAgentExtensionProperties')[parameters('virtualMachineOsType')].type)]",
      "location": "[parameters('virtualMachineLocation')]",
      "dependsOn": [
        "[resourceId('Microsoft.Compute/virtualMachines/providers/dataCollectionRuleAssociations', parameters('virtualMachineName'), 'Microsoft.Insights', parameters('dataCollectionRuleAssociationName'))]"
      ], 
      "properties": "[variables('azureMonitorAgentExtensionProperties')[parameters('virtualMachineOsType')]]"
    },
    {
      "type": "Microsoft.Compute/virtualMachines/extensions",
      "name": "[concat(parameters('virtualMachineName'), '/', variables('healthExtensionProperties')[parameters('virtualMachineOsType')].type)]",
      "location": "[parameters('virtualMachineLocation')]",
      "apiVersion": "2018-06-01",
      "dependsOn": [ 
        "[resourceId('Microsoft.Compute/virtualMachines/extensions', parameters('virtualMachineName'), variables('azureMonitorAgentExtensionProperties')[parameters('virtualMachineOsType')].type)]",
        "[resourceId('Microsoft.Compute/virtualMachines/providers/dataCollectionRuleAssociations', parameters('virtualMachineName'), 'Microsoft.Insights', parameters('dataCollectionRuleAssociationName'))]"
      ],    
      "properties": "[variables('healthExtensionProperties')[parameters('virtualMachineOsType')]]"
    }               
  ]
}
```

### <a name="sample-parameter-file"></a>Beispiel für Parameterdatei

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
      "virtualMachineName": {
        "value": "myvm"
      },
      "virtualMachineLocation": {
        "value": "eastus"
      },
      "virtualMachineOsType": {
        "value": "linux"
      },
      "healthDataCollectionRuleResourceId": {
        "value": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/my-resource-group/providers/Microsoft.Insights/dataCollectionRules/Microsoft-VMInsights-Health"
      }
  }
}
```

## <a name="next-steps"></a>Nächste Schritte

- [Anpassen von Monitoren für VM Insights](vminsights-health-configure.md)
