---
title: Verwaltung von VM-Erweiterungen mit Azure Arc-fähigen Servern
description: Mit Azure Arc-fähigen Servern kann die Bereitstellung von Erweiterungen für virtuelle Computer verwaltet werden, die Konfigurations- und Automatisierungsaufgaben nach der Bereitstellung für nicht in Azure gehostete VMs bereitstellen.
ms.date: 09/23/2020
ms.topic: conceptual
ms.openlocfilehash: 1c3d50f407f4412a14201dfe669334dbb083d323
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2020
ms.locfileid: "91329073"
---
# <a name="virtual-machine-extension-management-with-azure-arc-enabled-servers"></a>Verwaltung von Erweiterungen für virtuelle Computer mit Azure Arc-fähigen Servern

Erweiterungen für virtuelle Computer sind kleine Anwendungen, die Konfigurations- und Automatisierungsaufgaben auf virtuellen Azure-Computern nach der Bereitstellung ermöglichen. Wenn z.B. Software auf einem virtuellen Computer (virtual machine, VM) installiert werden muss, Virenschutz oder die Ausführung eines Skripts erforderlich ist, kann eine VM-Erweiterung verwendet werden.

Mit Azure Arc-fähigen Servern können Sie Azure-VM-Erweiterungen für nicht unter Azure gehostete virtuelle Windows- und Linux-Computer bereitstellen, um die Verwaltung Ihrer hybriden Computer vor Ort, in Edge-Umgebungen und in anderen Cloudumgebungen über ihren gesamten Lebenszyklus zu vereinfachen.

## <a name="key-benefits"></a>Hauptvorteile

Die Unterstützung von VM-Erweiterungen durch Azure Arc-fähige Server bietet die folgenden Hauptvorteile:

* Verwendung von [Azure Automation State Configuration](../../automation/automation-dsc-overview.md), um Konfigurationen zentral zu speichern und den gewünschten Zustand hybrid verbundener Computer mithilfe der DSC-VM-Erweiterung zu verwalten.

* Sammeln von Protokolldaten für die Analyse mit [Protokollen in Azure Monitor](../../azure-monitor/platform/data-platform-logs.md), die durch die Log Analytics-Agent-VM-Erweiterung unterstützt werden. Dies ist nützlich, um komplexe Datenanalysen über Daten aus einer Vielzahl von Quellen hinweg auszuführen.

* Mit [Azure Monitor für VMs](../../azure-monitor/insights/vminsights-overview.md) werden die Leistung Ihrer Windows- und Linux-VMs analysiert sowie ihre Prozesse und Abhängigkeiten von anderen Ressourcen und externen Prozessen überwacht. Dies wird durch Aktivieren sowohl der Log Analytics-Agent- als auch der Dependency-Agent-VM-Erweiterung erreicht.

* Herunterladen und Ausführen von Skripts auf hybrid verbundenen Computern mithilfe der Erweiterung für benutzerdefinierte Skripts. Diese Erweiterung ist hilfreich bei der Konfiguration nach der Bereitstellung, bei der Softwareinstallation oder bei anderen Konfigurations- oder Verwaltungsaufgaben.

## <a name="availability"></a>Verfügbarkeit

Die Funktionalität der VM-Erweiterungen ist nur in den [unterstützten Regionen](overview.md#supported-regions) aus der Liste verfügbar. Achten Sie darauf, das Onboarding Ihres Computers in einer dieser Regionen durchzuführen.

## <a name="extensions"></a>Erweiterungen

In dieser Version unterstützen wir die folgenden VM-Erweiterungen auf Windows- und Linux-Computern.

|Durchwahl |OS |Herausgeber |Zusätzliche Informationen |
|----------|---|----------|-----------------------|
|CustomScriptExtension |Windows |Microsoft.Compute |[Erweiterung für benutzerdefinierte Windows-Skripts](../../virtual-machines/extensions/custom-script-windows.md)|
|DSC |Windows |Microsoft.PowerShell|[Windows PowerShell DSC-Erweiterung](../../virtual-machines/extensions/dsc-windows.md)|
|Log Analytics-Agent |Windows |Microsoft.EnterpriseCloud.Monitoring |[Log Analytics-Erweiterung für virtuelle Computer für Windows](../../virtual-machines/extensions/oms-windows.md)|
|Microsoft Dependency-Agent | Windows |Microsoft.Compute | [VM-Erweiterung für den Dependency-Agent für Windows](../../virtual-machines/extensions/agent-dependency-windows.md)|
|CustomScript|Linux |Microsoft.Azure.Extension |[Erweiterung für benutzerdefinierte Linux-Skripts, Version 2](../../virtual-machines/extensions/custom-script-linux.md) |
|DSC |Linux |Microsoft.OSTCExtensions |[PowerShell DSC.-Erweiterung für Linux](../../virtual-machines/extensions/dsc-linux.md) |
|Log Analytics-Agent |Linux |Microsoft.EnterpriseCloud.Monitoring |[Log Analytics VM-Erweiterung für Linux](../../virtual-machines/extensions/oms-linux.md) |
|Microsoft Dependency-Agent | Linux |Microsoft.Compute | [VM-Erweiterung für den Dependency-Agent für Linux](../../virtual-machines/extensions/agent-dependency-linux.md) |

VM-Erweiterungen können auf Hybridservern, die von Azure Arc-fähigen Servern verwaltet werden, mit Azure Resource Manager-Vorlagen, über das Azure-Portal oder in Azure PowerShell ausgeführt werden.

Weitere Informationen zum Azure Connected Machine-Agent-Paket und Details zur Erweiterungs-Agent-Komponente finden Sie in der [Agent-Übersicht](agent-overview.md#agent-component-details).

## <a name="prerequisites"></a>Voraussetzungen

Diese Funktion hängt von den folgenden Azure-Ressourcenanbietern in Ihrem Abonnement ab:

* **Microsoft.HybridCompute**
* **Microsoft.GuestConfiguration**

Wenn diese noch nicht registriert sind, führen Sie die Schritte unter [Registrieren von Azure-Ressourcenanbietern](agent-overview.md#register-azure-resource-providers) aus.

Für die Log Analytics-Agent-VM-Erweiterung für Linux muss Python 2.x auf dem Zielcomputer installiert sein.

### <a name="connected-machine-agent"></a>Connected Machine-Agent

Überprüfen Sie, ob Ihr Computer mit den [unterstützten Versionen](agent-overview.md#supported-operating-systems) des Windows- und Linux-Betriebssystems für den Azure Connected Machine-Agent übereinstimmt.

Die Mindestversion des Connected Machine-Agents, die mit dieser Funktion unter Windows und Linux unterstütz wird, ist Version 1.0.

Informationen für das Upgrade Ihres Computers auf die erforderliche Agent-Version finden Sie unter [Agent aktualisieren](manage-agent.md#upgrading-agent).

## <a name="enable-extensions-from-the-portal"></a>Aktivieren von Erweiterungen über das Portal

VM-Erweiterungen können über das Azure-Portal auf Ihren mit Arc für Server verwalteten Computer angewendet werden.

1. Navigieren Sie in Ihrem Browser zum [Azure-Portal](https://portal.azure.com).

2. Navigieren Sie im Portal zu **Server - Azure Arc**, und wählen Sie in der Liste Ihren Hybridcomputer aus.

3. Wählen Sie **Erweiterungen** und dann **Hinzufügen** aus. Wählen Sie die Erweiterung aus, die Sie aus der Liste verfügbarer Erweiterungen erhalten, und befolgen Sie die Anweisungen im Assistenten. In diesem Beispiel stellen wir die Log Analytics-VM-Erweiterung bereit.

    ![Auswählen der VM-Erweiterung für den ausgewählten Computer](./media/manage-vm-extensions/add-vm-extensions.png)

    Im folgenden Beispiel wird die Installation der Log Analytics-VM-Erweiterung über das Azure-Portal gezeigt:

    ![Installieren der Log Analytics VM-Erweiterung](./media/manage-vm-extensions/mma-extension-config.png)

    Um die Installation abzuschließen, müssen Sie die Arbeitsbereichs-ID und den Primärschlüssel angeben. Wenn Sie mit dem Ermitteln dieser Informationen nicht vertraut sind, lesen Sie [Abrufen von Arbeitsbereichs-ID und Schlüssel](../../azure-monitor/platform/log-analytics-agent.md#workspace-id-and-key).

4. Nach dem Bestätigen der bereitgestellten erforderlichen Informationen wählen Sie **Erstellen** aus. Es wird eine Zusammenfassung der Bereitstellung angezeigt, und Sie können den Status der Bereitstellung überprüfen.

>[!NOTE]
>Zwar können mehrere Erweiterungen in einem Batch zusammengefasst und verarbeitet werden, die Installation erfolgt jedoch nacheinander. Sobald die Installation der ersten Erweiterung abgeschlossen ist, wird die Installation der nächsten Erweiterung versucht.

## <a name="azure-resource-manager-templates"></a>Azure-Ressourcen-Manager-Vorlagen

VM-Erweiterungen können einer Azure Resource Manager-Vorlage hinzugefügt und mit der Bereitstellung der Vorlage ausgeführt werden. Mit den von Azure Arc-fähigen Servern unterstützten VM-Erweiterungen können Sie die unterstützte VM-Erweiterung mithilfe von Azure PowerShell auf Linux- oder Windows-Computern bereitstellen. Jedes Beispiel unten beinhaltet eine Vorlagendatei und eine Parameterdatei mit Beispielwerten für die Vorlage.

>[!NOTE]
>Zwar können mehrere Erweiterungen in einem Batch zusammengefasst und verarbeitet werden, die Installation erfolgt jedoch nacheinander. Sobald die Installation der ersten Erweiterung abgeschlossen ist, wird die Installation der nächsten Erweiterung versucht.

### <a name="deploy-the-log-analytics-vm-extension"></a>Bereitstellen der Log Analytics-VM-Erweiterung

Um den Log Analytics-Agent komfortabel bereitzustellen, steht das folgende Beispiel zum Installieren des Agents wahlweise unter Windows oder Linux zur Verfügung.

#### <a name="template-file-for-linux"></a>Vorlagendatei für Linux

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "vmName": {
            "type": "string"
        },
        "location": {
            "type": "string"
        },
        "workspaceId": {
            "type": "string"
        },
        "workspaceKey": {
            "type": "string"
        }
    },
    "resources": [
        {
            "name": "[concat(parameters('vmName'),'/OMSAgentForLinux')]",
            "type": "Microsoft.HybridCompute/machines/extensions",
            "location": "[parameters('location')]",
            "apiVersion": "2019-08-02-preview",
            "properties": {
                "publisher": "Microsoft.EnterpriseCloud.Monitoring",
                "type": "OmsAgentForLinux",
                "settings": {
                    "workspaceId": "[parameters('workspaceId')]"
                },
                "protectedSettings": {
                    "workspaceKey": "[parameters('workspaceKey')]"
                }
            }
        }
    ]
}
```

#### <a name="template-file-for-windows"></a>Vorlagendatei für Windows

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "vmName": {
            "type": "string"
        },
        "location": {
            "type": "string"
        },
        "workspaceId": {
            "type": "string"
        },
        "workspaceKey": {
            "type": "string"
        }
    },
    "resources": [
        {
            "name": "[concat(parameters('vmName'),'/MicrosoftMonitoringAgent')]",
            "type": "Microsoft.HybridCompute/machines/extensions",
            "location": "[parameters('location')]",
            "apiVersion": "2019-08-02-preview",
            "properties": {
                "publisher": "Microsoft.EnterpriseCloud.Monitoring",
                "type": "MicrosoftMonitoringAgent",
                "autoUpgradeMinorVersion": true,
                "settings": {
                    "workspaceId": "[parameters('workspaceId')]"
                },
                "protectedSettings": {
                    "workspaceKey": "[parameters('workspaceKey')]"
                }
            }
        }
    ]
}
```

#### <a name="parameter-file"></a>Parameterdatei

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "vmName": {
            "value": "<vmName>"
        },
        "location": {
            "value": "<region>"
        },
        "workspaceId": {
            "value": "<MyWorkspaceID>"
        },
        "workspaceKey": {
            "value": "<MyWorkspaceKey>"
        }
    }
}
```

Speichern Sie die Vorlagen- und Parameterdateien auf dem Datenträger, und bearbeiten Sie die Parameterdatei mit den passenden Werten für Ihre Bereitstellung. Anschließend können Sie mit dem folgenden Befehl die Erweiterung auf allen verbundenen Computern innerhalb einer Ressourcengruppe installieren. Der Befehl verwendet den *TemplateFile*-Parameter zum Angeben der Vorlage und den *TemplateParameterFile*-Parameter zum Angeben einer Datei, die Parameter und Parameterwerte enthält.

```powershell
New-AzResourceGroupDeployment -ResourceGroupName "ContosoEngineering" -TemplateFile "D:\Azure\Templates\LogAnalyticsAgentWin.json" -TemplateParameterFile "D:\Azure\Templates\LogAnalyticsAgentWinParms.json"
```

### <a name="deploy-the-custom-script-extension"></a>Bereitstellen der Erweiterung für benutzerdefinierte Skripts

Zum Verwenden der Erweiterung für benutzerdefinierte Skripts steht das folgende Beispiel für die Ausführung unter Windows oder Linux zur Verfügung. Wenn Sie mit der Erweiterung für benutzerdefinierte Skripts nicht vertraut sind, lesen Sie [Erweiterung für benutzerdefinierte Skripts für Windows](../../virtual-machines/extensions/custom-script-windows.md) oder [Erweiterung für benutzerdefinierte Skripts für Linux](../../virtual-machines/extensions/custom-script-linux.md). Es gibt eine Reihe abweichender Merkmale, die Sie kennen sollten, wenn Sie diese Erweiterung für Hybridcomputer verwenden:

* Die Liste der unterstützten Betriebssysteme für die Azure VM-Erweiterung für benutzerdefinierte Skripts trifft auf Azure Arc-fähige Server nicht zu. Die Liste der unterstützten Betriebssysteme für Arc-fähige Server finden Sie [hier](agent-overview.md#supported-operating-systems).

* Konfigurationsdetails, die Azure-VM-Skalierungsgruppen oder klassische VMs betreffen, sind nicht anwendbar.

* Wenn Ihre Computer ein Skript extern herunterladen müssen und nur über einen Proxyserver Daten austauschen können, müssen Sie den [Connected Machine-Agent so konfigurieren](manage-agent.md#update-or-remove-proxy-settings), dass er die Umgebungsvariable des Proxyservers festlegt.

In der Konfiguration der benutzerdefinierten Skripterweiterung werden Aspekte wie der Skriptspeicherort und der auszuführende Befehl angegeben. Diese Konfiguration ist in einer Azure Resource Manager-Vorlage angegeben, die unten sowohl für Linux- als auch für Windows-Hybridcomputer zur Verfügung steht.

#### <a name="template-file-for-linux"></a>Vorlagendatei für Linux

```json
{
  "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "vmName": {
      "type": "string"
    },
    "location": {
      "type": "string"
    },
    "fileUris": {
      "type": "array"
    },
    "commandToExecute": {
      "type": "securestring"
    }
  },
  "resources": [
    {
      "name": "[concat(parameters('vmName'),'/CustomScript')]",
      "type": "Microsoft.HybridCompute/machines/extensions",
      "location": "[parameters('location')]",
      "apiVersion": "2019-08-02-preview",
      "properties": {
        "publisher": "Microsoft.Azure.Extensions",
        "type": "CustomScript",
        "autoUpgradeMinorVersion": true,
        "settings": {},
        "protectedSettings": {
          "commandToExecute": "[parameters('commandToExecute')]",
          "fileUris": "[parameters('fileUris')]"
        }
      }
    }
  ]
}
```

#### <a name="template-file-for-windows"></a>Vorlagendatei für Windows

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "vmName": {
            "type": "string"
        },
        "location": {
            "type": "string"
        },
        "fileUris": {
            "type": "string"
        },
        "arguments": {
            "type": "securestring",
            "defaultValue": " "
        }
    },
    "variables": {
        "UriFileNamePieces": "[split(parameters('fileUris'), '/')]",
        "firstFileNameString": "[variables('UriFileNamePieces')[sub(length(variables('UriFileNamePieces')), 1)]]",
        "firstFileNameBreakString": "[split(variables('firstFileNameString'), '?')]",
        "firstFileName": "[variables('firstFileNameBreakString')[0]]"
    },
    "resources": [
        {
            "name": "[concat(parameters('vmName'),'/CustomScriptExtension')]",
            "type": "Microsoft.HybridCompute/machines/extensions",
            "location": "[parameters('location')]",
            "apiVersion": "2019-08-02-preview",
            "properties": {
                "publisher": "Microsoft.Compute",
                "type": "CustomScriptExtension",
                "autoUpgradeMinorVersion": true,
                "settings": {
                    "fileUris": "[split(parameters('fileUris'), ' ')]"
                },
                "protectedSettings": {
                    "commandToExecute": "[concat ('powershell -ExecutionPolicy Unrestricted -File ', variables('firstFileName'), ' ', parameters('arguments'))]"
                }
            }
        }
    ]
}
```

#### <a name="parameter-file"></a>Parameterdatei

```json
{
  "$schema": "https://schema.management.azure.com/schemas/0.1.2-preview/CreateUIDefinition.MultiVm.json#",
  "handler": "Microsoft.Azure.CreateUIDef",
  "version": "0.1.2-preview",
  "parameters": {
    "basics": [
      {}
    ],
    "steps": [
      {
        "name": "customScriptExt",
        "label": "Add Custom Script Extension",
        "elements": [
          {
            "name": "fileUris",
            "type": "Microsoft.Common.FileUpload",
            "label": "Script files",
            "toolTip": "The script files that will be downloaded to the virtual machine.",
            "constraints": {
              "required": false
            },
            "options": {
              "multiple": true,
              "uploadMode": "url"
            },
            "visible": true
          },
          {
            "name": "commandToExecute",
            "type": "Microsoft.Common.TextBox",
            "label": "Command",
            "defaultValue": "sh script.sh",
            "toolTip": "The command to execute, for example: sh script.sh",
            "constraints": {
              "required": true
            },
            "visible": true
          }
        ]
      }
    ],
    "outputs": {
      "vmName": "[vmName()]",
      "location": "[location()]",
      "fileUris": "[steps('customScriptExt').fileUris]",
      "commandToExecute": "[steps('customScriptExt').commandToExecute]"
    }
  }
}
```

### <a name="deploy-the-powershell-dsc-extension"></a>Bereitstellen der PowerShell DSC-Erweiterung

Zum Verwenden der PowerShell DSC-Erweiterung steht das folgende Beispiel für die Ausführung unter Windows oder Linux zur Verfügung. Wenn Sie mit der PowerShell DSC-Erweiterung nicht vertraut sind, informieren Sie sich in der [Übersicht zum DSC-Erweiterungshandler](../../virtual-machines/extensions/dsc-overview.md). Es gibt eine Reihe abweichender Merkmale, die Sie kennen sollten, wenn Sie diese Erweiterung für Hybridcomputer verwenden:

* Die Liste der unterstützten Betriebssysteme für die Azure VM PowerShell DSC-Erweiterung trifft auf Azure Arc-fähige Server nicht zu. Die Liste der unterstützten Betriebssysteme für Arc-fähige Server finden Sie [hier](agent-overview.md#supported-operating-systems).

* Wenn Ihre Computer ein Skript extern herunterladen müssen und nur über einen Proxyserver Daten austauschen können, müssen Sie den [Connected Machine-Agent so konfigurieren](manage-agent.md#update-or-remove-proxy-settings), dass er die Umgebungsvariable des Proxyservers festlegt.

#### <a name="template-file-for-linux"></a>Vorlagendatei für Linux

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "vmName": {
            "type": "string",
            "metadata": {
                "description": "Name of the vm, will be used as DNS Name for the Public IP used to access the Virtual Machine."
            }
        },
        "location": {
            "type": "string",
            "metadata": {
                "description": "Location for all resources."
            }
        },
        "mode": {
            "type": "string",
            "defaultValue": "Push",
            "metadata": {
                "description": "The functional mode, push MOF configuration (Push), distribute MOF configuration (Pull), install custom DSC module (Install)"
            },
            "allowedValues": [
                "Push",
                "Pull",
                "Install",
                "Register"
            ]
        },
        "fileUri": {
            "type": "string",
            "defaultValue": "",
            "metadata": {
                "description": "The uri of the MOF file/Meta MOF file/resource ZIP file"
            }
        },
        "registrationUrl": {
            "type": "string",
            "defaultValue": "",
            "metadata": {
                "description": "The URL of the Azure Automation account"
            }
        },
        "registrationKey": {
            "type": "string",
            "defaultValue": "",
            "metadata": {
                "description": "The access key of the Azure Automation account"
            }
        }
    },
    "resources": [
        {
            "name": "[concat(parameters('vmName'),'/DSCForLinux')]",
            "type": "Microsoft.HybridCompute/machines/extensions",
            "location": "[parameters('location')]",
            "apiVersion": "2019-08-02-preview",
            "properties": {
                "publisher": "Microsoft.OSTCExtensions",
                "type": "DSCForLinux",
                "settings": {
                    "Mode": "[parameters('mode')]",
                    "FileUri": "[parameters('fileUri')]"
                },
                "protectedSettings": {
                    "RegistrationUrl": "[parameters('registrationUrl')]",
                    "RegistrationKey": "[parameters('registrationKey')]"
                }
            }
        }
    ]
}
```

#### <a name="template-file-for-windows"></a>Vorlagendatei für Windows

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "vmName": {
            "type": "string"
        },
        "location": {
            "type": "string"
        },
        "modulesUrl": {
            "type": "string"
        },
        "configurationFunction": {
            "type": "string"
        },
        "properties": {
            "type": "string",
            "defaultValue": ""
        },
        "dataBlobUri": {
            "type": "string",
            "defaultValue": ""
        },
        "wmfVersion": {
            "type": "string",
            "defaultValue": "latest",
            "allowedValues": [
                "4.0",
                "5.0",
                "5.1",
                "latest"
            ]
        },
        "privacy": {
            "type": "string",
            "defaultValue": ""
        },
        "autoUpdate": {
            "type": "bool",
            "defaultValue": false
        }
    },
    "resources": [
        {
            "name": "[concat(parameters('vmName'),'/Microsoft.Powershell.DSC')]",
            "type": "Microsoft.HybridCompute/machines/extensions",
            "location": "[parameters('location')]",
            "apiVersion": "2019-08-02-preview",
            "properties": {
                "publisher": "Microsoft.Powershell",
                "type": "DSC",
                "autoUpgradeMinorVersion": "[parameters('autoUpdate')]",
                "settings": {
                    "ModulesUrl": "[parameters('modulesUrl')]",
                    "ConfigurationFunction": "[parameters('configurationFunction')]",
                    "Properties": "[parameters('properties')]",
                    "WmfVersion": "[parameters('wmfVersion')]",
                    "Privacy": {
                        "DataCollection": "[parameters('privacy')]"
                    }
                },
                "protectedSettings": {
                    "DataBlobUri": "[parameters('dataBlobUri')]"
                }
            }
        }
    ]
}
```

#### <a name="parameter-file"></a>Parameterdatei

```json
{
  "$schema": "https://schema.management.azure.com/schemas/0.1.2-preview/CreateUIDefinition.MultiVm.json#",
  "handler": "Microsoft.Azure.CreateUIDef",
  "version": "0.1.2-preview",
  "parameters": {
    "basics": [
      {}
    ],
    "steps": [
      {
        "name": "dscExtension",
        "label": "Add DSC Extension",
        "elements": [
          {
            "name": "Mode",
            "type": "Microsoft.Common.OptionsGroup",
            "label": "Mode",
            "defaultValue": 0,
            "toolTip": "The functional mode, push MOF configuration (Push), distribute MOF configuration (Pull), install custom DSC module (Install)",
            "constraints": {
              "allowedValues": [
                {
                  "label": "Push",
                  "value": "Push"
                },
                {
                  "label": "Pull",
                  "value": "Pull"
                },
                {
                  "label": "Install",
                  "value": "Install"
                },
                {
                  "label": "Register",
                  "value": "Register"
                }
              ]
            },
            "visible": true
          },
          {
            "name": "FileUri",
            "type": "Microsoft.Common.FileUpload",
            "label": "File URI",
            "toolTip": "The uri of the MOF file/Meta MOF file/resource ZIP file",
            "constraints": {
              "required": false,
              "accept": ".psd1"
            },
            "options": {
              "multiple": false,
              "uploadMode": "url",
              "openMode": "binary",
              "encoding": "UTF-8"
            }
          },
          {
            "name": "RegistrationUrl",
            "type": "Microsoft.Common.TextBox",
            "label": "Registration URL",
            "toolTip": "The URL of the Azure Automation account",
            "constraints": {
              "required": false
            }
          },
          {
            "name": "RegistrationKey",
            "type": "Microsoft.Common.TextBox",
            "label": "Registration key",
            "toolTip": "The access key of the Azure Automation account",
            "constraints": {
              "required": false
            }
          }
        ]
      }
    ],
    "outputs": {
      "vmName": "[vmName()]",
      "location": "[location()]",
      "mode": "[steps('dscExtension').Mode]",
      "fileUri": "[steps('dscExtension').FileUri]",
      "registrationUrl": "[steps('dscExtension').RegistrationUrl]",
      "registrationKey": "[steps('dscExtension').RegistrationKey]"
    }
  }
}
```

### <a name="dependency-agent"></a>Abhängigkeits-Agent

Zum Verwenden der Azure Monitor Dependency-Agent-Erweiterung steht das folgende Beisiel für Windows und Linux zur Verfügung. Wenn Sie mit dem Dependency-Agent nicht vertraut sind, informieren Sie sich in der [Übersicht zu Azure Monitor-Agents](../../azure-monitor/platform/agents-overview.md#dependency-agent).

#### <a name="template-file-for-linux"></a>Vorlagendatei für Linux

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
  "parameters": {
    "vmName": {
      "type": "string",
      "metadata": {
        "description": "The name of existing Linux machine."
      }
    }
  },
  "variables": {
    "vmExtensionsApiVersion": "2017-03-30"
  },
  "resources": [
    {
      "type": "Microsoft.HybridCompute/machines/extensions",
      "name": "[concat(parameters('vmName'),'/DAExtension')]",
      "apiVersion": "[variables('vmExtensionsApiVersion')]",
      "location": "[resourceGroup().location]",
      "dependsOn": [
      ],
      "properties": {
        "publisher": "Microsoft.Azure.Monitoring.DependencyAgent",
        "type": "DependencyAgentLinux",
        "typeHandlerVersion": "9.5",
        "autoUpgradeMinorVersion": true
      }
    }
  ],
    "outputs": {
    }
}
```

#### <a name="template-file-for-windows"></a>Vorlagendatei für Windows

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
  "parameters": {
    "vmName": {
      "type": "string",
      "metadata": {
        "description": "The name of existing Windows machine."
      }
    }
  },
  "variables": {
    "vmExtensionsApiVersion": "2017-03-30"
  },
  "resources": [
    {
      "type": "Microsoft.HybridCompute/machines/extensions",
      "name": "[concat(parameters('vmName'),'/DAExtension')]",
      "apiVersion": "[variables('vmExtensionsApiVersion')]",
      "location": "[resourceGroup().location]",
      "dependsOn": [
      ],
      "properties": {
        "publisher": "Microsoft.Azure.Monitoring.DependencyAgent",
        "type": "DependencyAgentWindows",
        "typeHandlerVersion": "9.5",
        "autoUpgradeMinorVersion": true
      }
    }
  ],
    "outputs": {
    }
}
```

## <a name="uninstall-extension"></a>Deinstallieren der Erweiterung

Das Entfernen mindestens einer Erweiterung von einem Arc-fähigen Server kann nur über das Azure-Portal durchgeführt werden. Führen Sie dann die folgenden Schritte aus, um eine Erweiterung zu entfernen.

1. Navigieren Sie in Ihrem Browser zum [Azure-Portal](https://portal.azure.com).

2. Navigieren Sie im Portal zu **Server - Azure Arc**, und wählen Sie in der Liste Ihren Hybridcomputer aus.

3. Wählen Sie **Erweiterungen** aus, und wählen Sie dann eine Erweiterung aus der Liste der installierten Erweiterungen aus.

4. Wählen Sie **Deinstallieren** aus. Wenn Sie zur Bestätigung aufgefordert werden, wählen Sie **Ja** aus, um fortzufahren.

## <a name="next-steps"></a>Nächste Schritte

* Informationen zur Problembehandlung finden Sie im [Problembehandlungs-Handbuch für VM-Erweiterungen](troubleshoot-vm-extensions.md).

* Erfahren Sie, wie Sie Ihren Computer mithilfe von [Azure Policy](../../governance/policy/overview.md) verwalten, wie z. B. bei der VM-[Gastkonfiguration](../../governance/policy/concepts/guest-configuration.md), dem Überprüfen, ob der Computer dem erwarteten Log Analytics-Arbeitsbereich Bericht erstattet, beim Aktivieren der Überwachung mit [Azure Monitor mit VMs](../../azure-monitor/insights/vminsights-enable-policy.md) und vieles mehr.

* Weitere Informationen zum [Log Analytics-Agent](../../azure-monitor/platform/log-analytics-agent.md). Der Log Analytics-Agent für Windows und Linux ist erforderlich, wenn Sie Daten zur Betriebssystem- und Workloadüberwachung erfassen, diese mithilfe von Automation Runbooks oder Funktionen wie Updateverwaltung oder andere Azure-Dienste wie [Azure Security Center](../../security-center/security-center-intro.md) nutzen möchten.