---
title: Erstellen eines Automation-Kontos mithilfe einer Azure Resource Manager-Vorlage | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie Sie mit einer Azure Resource Manager-Vorlage ein Azure Automation-Konto erstellen.
ms.service: automation
ms.subservice: update-management
ms.topic: conceptual
author: mgoedtel
ms.author: magoedte
ms.date: 05/22/2020
ms.openlocfilehash: 1418b26a2a498c43ff61f42b2761c59cbca5d0f4
ms.sourcegitcommit: 0b80a5802343ea769a91f91a8cdbdf1b67a932d3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/25/2020
ms.locfileid: "83837143"
---
# <a name="create-an-automation-account-using-an-azure-resource-manager-template"></a>Erstellen eines Automation-Kontos mithilfe einer Azure Resource Manager-Vorlage

Sie können mit einer [Azure Resource Manager-Vorlage](../azure-resource-manager/templates/template-syntax.md) ein Azure Automation-Konto in Ihrer Ressourcengruppe erstellen. Dieser Artikel enthält eine Beispielvorlage mit folgenden Funktionen:

* Automatisieren der Erstellung eines Azure Monitor Log Analytics-Arbeitsbereichs
* Automatisieren der Erstellung eines Azure Automation-Kontos
* Verknüpfen des Automation-Kontos mit dem Log Analytics-Arbeitsbereich

Die Vorlage automatisiert nicht die Aktivierung von virtuellen Computern innerhalb oder außerhalb von Azure. 

>[!NOTE]
>Die Erstellung des ausführenden Automation-Kontos wird bei Verwendung einer Azure Resource Manager-Vorlage nicht unterstützt. Informationen zum Erstellen eines ausführenden Kontos über das Portal oder mit PowerShell finden Sie unter [Verwalten von ausführenden Azure Automation-Konten](manage-runas-account.md).

Nachdem Sie diese Schritte ausgeführt haben, müssen Sie [Diagnoseeinstellungen konfigurieren](automation-manage-send-joblogs-log-analytics.md), damit Ihr Automation-Konto Auftragsstatus und Auftragsdatenströme von Runbooks an den verknüpften Log Analytics Arbeitsbereich sendet. 

## <a name="api-versions"></a>API-Versionen

Die folgende Tabelle enthält die API-Versionen für die Ressourcen, die in diesem Beispiel verwendet werden.

| Resource | Ressourcentyp | API-Version |
|:---|:---|:---|
| Arbeitsbereich | workspaces | 2017-03-15-preview |
| Automation-Konto | automation | 2015-10-31 | 

## <a name="before-you-use-the-template"></a>Vor der Verwendung der Vorlage

Wenn Sie PowerShell lokal installieren und verwenden möchten, müssen Sie für diesen Artikel das Az-Modul für Azure PowerShell verwenden. Führen Sie `Get-Module -ListAvailable Az` aus, um die Version zu ermitteln. Sollte ein Upgrade erforderlich sein, lesen Sie [Installieren des Azure PowerShell-Moduls](/powershell/azure/install-az-ps). Wenn Sie PowerShell lokal ausführen, müssen Sie auch `Connect-AzAccount` ausführen, um eine Verbindung mit Azure herzustellen. In PowerShell wird für die Bereitstellung [New-AzResourceGroupDeployment](/powershell/module/az.resources/new-azresourcegroupdeployment) verwendet.

Wenn Sie die Azure-Befehlszeilenschnittstelle (CLI) lokal installieren und verwenden möchten, müssen Sie für diesen Artikel mindestens die Version 2.1.0 ausführen. Führen Sie `az --version` aus, um die Version zu ermitteln. Installations- und Upgradeinformationen finden Sie bei Bedarf unter [Installieren von Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest). Bei der Azure-Befehlszeilenschnittstelle wird für diese Bereitstellung [az group deployment create](https://docs.microsoft.com/cli/azure/group/deployment?view=azure-cli-latest#az-group-deployment-create) verwendet. 

Die JSON-Vorlage ist so konfiguriert, dass Sie Folgendes von Ihnen anfordert:

* Den Namen des Arbeitsbereichs
* Die Region, in der der Arbeitsbereich erstellt werden soll
* Der Namen des Automation-Kontos.
* Die Region, in der das Konto erstellt werden soll

Die folgenden Parameter in der Vorlage werden mit einem Standardwert für den Log Analytics-Arbeitsbereich festgelegt:

* *sku:* Als Standardwert wird der neue, im Preismodell von April 2018 veröffentlichte Tarif pro GB verwendet.
* *dataRetention* ist standardmäßig auf 30 Tage festgelegt.
* *capacityReservationLevel* ist standardmäßig auf 100 GB festgelegt.

>[!WARNING]
>Wenn Sie einen Log Analytics-Arbeitsbereich in einem Abonnement mit dem Preismodell von April 2018 erstellen oder konfigurieren möchten, ist *PerGB2018* als einziger gültiger Log Analytics-Tarif verfügbar.
>

Die JSON-Vorlage gibt einen Standardwerte für die anderen Parameter an, die wahrscheinlich als Standardkonfiguration in Ihrer Umgebung verwendet werden. Für den gemeinsamen Zugriff in Ihrer Organisation können Sie die Vorlage in einem Azure Storage-Konto speichern. Weitere Informationen zum Arbeiten mit Vorlagen finden Sie unter [Bereitstellen von Ressourcen mit Azure Resource Manager-Vorlagen und der Azure-Befehlszeilenschnittstelle](../azure-resource-manager/templates/deploy-cli.md).

Wenn Sie mit Azure Automation und Azure Monitor noch nicht vertraut sind, sollten Sie unbedingt die folgenden Konfigurationsdetails kennen. Sie können Ihnen helfen, Fehler zu vermeiden, wenn Sie versuchen, einen mit Ihrem neuen Automation-Konto verknüpften Log Analytics-Arbeitsbereich zu erstellen, zu konfigurieren und zu verwenden. 

* Lesen Sie die [zusätzlichen Details](../azure-monitor/platform/template-workspace-configuration.md#create-a-log-analytics-workspace), um die Optionen für die Arbeitsbereichskonfiguration vollständig zu verstehen, z. B. Zugriffssteuerungsmodus, Tarif, Datenaufbewahrung und Kapazitätsreservierungsstufe.

* Überprüfen Sie die [Arbeitsbereichszuordnungen](how-to/region-mappings.md), um die unterstützten Regionen inline oder in einer Parameterdatei anzugeben. Es werden nur bestimmte Regionen zum Verknüpfen mit einem Log Analytics-Arbeitsbereich und einem Automation-Konto in Ihrem Abonnement unterstützt.

* Wenn Sie noch nicht mit Azure Monitor-Protokollen vertraut sind und noch keinen Arbeitsbereich bereitgestellt haben, sollten Sie die [Entwurfsanleitungen für Arbeitsbereiche](../azure-monitor/platform/design-logs-deployment.md) lesen. Sie helfen Ihnen, etwas über die Zugriffssteuerung zu erfahren und die Implementierungsstrategien für den Entwurf zu verstehen, die für Ihre Organisation empfohlen werden.

## <a name="deploy-the-template"></a>Bereitstellen der Vorlage

1. Kopieren Sie die folgende JSON-Syntax, und fügen Sie sie in Ihre Datei ein:

    ```json
    {
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "workspaceName": {
            "type": "string",
            "metadata": {
                "description": "Workspace name"
            }
        },
        "sku": {
            "type": "string",
            "allowedValues": [
                "pergb2018",
                "Free",
                "Standalone",
                "PerNode",
                "Standard",
                "Premium"
            ],
            "defaultValue": "pergb2018",
            "metadata": {
                "description": "Pricing tier: perGB2018 or legacy tiers (Free, Standalone, PerNode, Standard or Premium), which are not available to all customers."
            }
        },
        "dataRetention": {
            "type": "int",
            "defaultValue": 30,
            "minValue": 7,
            "maxValue": 730,
            "metadata": {
                "description": "Number of days of retention. Workspaces in the legacy Free pricing tier can have only 7 days."
            }
        },
        "immediatePurgeDataOn30Days": {
            "type": "bool",
            "defaultValue": "[bool('false')]",
            "metadata": {
                "description": "If set to true when changing retention to 30 days, older data will be immediately deleted. Use this with extreme caution. This applies only when retention is being set to 30 days."
            }
        },
        "location": {
            "type": "string",
            "metadata": {
                "description": "Specifies the location in which to create the workspace."
            }
        },
        "automationAccountName": {
            "type": "string",
            "metadata": {
                "description": "Automation account name"
            }
        },
        "automationAccountLocation": {
            "type": "string",
            "metadata": {
                "description": "Specify the location in which to create the Automation account."
            }
        },
        "sampleGraphicalRunbookName": {
                "type": "String",
                "defaultValue": "AzureAutomationTutorial"
            },
            "sampleGraphicalRunbookDescription": {
                "type": "String",
                "defaultValue": " An example runbook that gets all the Resource Manager resources by using the Run As account (service principal)."
            },
            "sampleGraphicalRunbookContentUri": {
                "type": "String",
                "defaultValue": "https://eus2oaasibizamarketprod1.blob.core.windows.net/marketplace-runbooks/AzureAutomationTutorial.graphrunbook"
            },
            "samplePowerShellRunbookName": {
                "type": "String",
                "defaultValue": "AzureAutomationTutorialScript"
            },
            "samplePowerShellRunbookDescription": {
                "type": "String",
                "defaultValue": " An example runbook that gets all the Resource Manager resources by using the Run As account (service principal)."
            },
            "samplePowerShellRunbookContentUri": {
                "type": "String",
                "defaultValue": "https://eus2oaasibizamarketprod1.blob.core.windows.net/marketplace-runbooks/AzureAutomationTutorial.ps1"
            },
            "samplePython2RunbookName": {
                "type": "String",
                "defaultValue": "AzureAutomationTutorialPython2"
            },
            "samplePython2RunbookDescription": {
                "type": "String",
                "defaultValue": " An example runbook that gets all the Resource Manager resources by using the Run As account (service principal)."
            },
            "samplePython2RunbookContentUri": {
                "type": "String",
                "defaultValue": "https://eus2oaasibizamarketprod1.blob.core.windows.net/marketplace-runbooks/AzureAutomationTutorialPython2.py"
            }
    },
    "resources": [
        {
        "type": "Microsoft.OperationalInsights/workspaces",
            "name": "[parameters('workspaceName')]",
            "apiVersion": "2017-03-15-preview",
            "location": "[parameters('location')]",
            "properties": {
                "sku": {
                    "Name": "[parameters('sku')]",
                    "name": "CapacityReservation",
                    "capacityReservationLevel": 100
                },
                "retentionInDays": "[parameters('dataRetention')]",
                "features": {
                    "searchVersion": 1,
                    "legacy": 0,
                    "enableLogAccessUsingOnlyResourcePermissions": true
                }
            },
        "resources": [
        {
            "type": "Microsoft.Automation/automationAccounts",
            "apiVersion": "2015-01-01-preview",
            "name": "[parameters('automationAccountName')]",
            "location": "[parameters('automationAccountLocation')]",
            "dependsOn": [
                 "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]",
            ],
            "tags": {},
            "properties": {
                "sku": {
                    "name": "Basic"
                }
            },
            "resources": [
                    {
                        "type": "runbooks",
                        "apiVersion": "2015-01-01-preview",
                        "name": "[parameters('sampleGraphicalRunbookName')]",
                        "location": "[parameters('automationAccountLocation')]",
                        "dependsOn": [
                            "[concat('Microsoft.Automation/automationAccounts/', parameters('automationAccountName'))]"
                        ],
                        "tags": {},
                        "properties": {
                            "runbookType": "GraphPowerShell",
                            "logProgress": "false",
                            "logVerbose": "false",
                            "description": "[parameters('sampleGraphicalRunbookDescription')]",
                            "publishContentLink": {
                                "uri": "[parameters('sampleGraphicalRunbookContentUri')]",
                                "version": "1.0.0.0"
                            }
                        }
                    },
                    {
                        "type": "runbooks",
                        "apiVersion": "2015-01-01-preview",
                        "name": "[parameters('samplePowerShellRunbookName')]",
                        "location": "[parameters('automationAccountLocation')]",
                        "dependsOn": [
                            "[concat('Microsoft.Automation/automationAccounts/', parameters('automationAccountName'))]"
                        ],
                        "tags": {},
                        "properties": {
                            "runbookType": "PowerShell",
                            "logProgress": "false",
                            "logVerbose": "false",
                            "description": "[parameters('samplePowerShellRunbookDescription')]",
                            "publishContentLink": {
                                "uri": "[parameters('samplePowerShellRunbookContentUri')]",
                                "version": "1.0.0.0"
                            }
                        }
                    },
                    {
                        "type": "runbooks",
                        "apiVersion": "2015-01-01-preview",
                        "name": "[parameters('samplePython2RunbookName')]",
                        "location": "[parameters('automationAccountLocation')]",
                        "dependsOn": [
                            "[concat('Microsoft.Automation/automationAccounts/', parameters('automationAccountName'))]"
                        ],
                        "tags": {},
                        "properties": {
                            "runbookType": "Python2",
                            "logProgress": "false",
                            "logVerbose": "false",
                            "description": "[parameters('samplePython2RunbookDescription')]",
                            "publishContentLink": {
                                "uri": "[parameters('samplePython2RunbookContentUri')]",
                                "version": "1.0.0.0"
                            }
                        }
                    }
                ]
        },
        {
            "apiVersion": "2015-11-01-preview",
            "type": "Microsoft.OperationalInsights/workspaces/linkedServices",
            "name": "[concat(parameters('workspaceName'), '/' , 'Automation')]",
            "location": "[resourceGroup().location]",
            "dependsOn": [
                "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]",
                "[concat('Microsoft.Automation/automationAccounts/', parameters('automationAccountName'))]"
            ],
            "properties": {
                "resourceId": "[resourceId('Microsoft.Automation/automationAccounts/', parameters('automationAccountName'))]"
            }
        }
       ]
      }
     ]
    }
    ```

2. Bearbeiten Sie die Vorlage entsprechend Ihren Anforderungen. Erstellen Sie ggf. eine [Resource Manager-Parameterdatei](../azure-resource-manager/templates/parameter-files.md), anstatt Parameter als Inlinewerte zu übergeben.

3. Speichern Sie diese Datei unter dem Namen „deployAzAutomationAccttemplate.json“ in einem lokalen Ordner.

4. Die Vorlage kann nun bereitgestellt werden. Sie können dazu entweder PowerShell oder Azure CLI verwenden. Wenn Sie zur Eingabe des Namens eines Arbeitsbereichs oder Automation-Kontos aufgefordert werden, geben Sie einen Namen an, der in allen Ihren Azure-Abonnements global eindeutig ist.

    **PowerShell**

    ```powershell
    New-AzResourceGroupDeployment -Name <deployment-name> -ResourceGroupName <resource-group-name> -TemplateFile deployAzAutomationAccttemplate.json
    ```

    **Azure-Befehlszeilenschnittstelle**

    ```cli
    az group deployment create --resource-group <my-resource-group> --name <my-deployment-name> --template-file deployAzAutomationAccttemplate.json
    ```

    Die Bereitstellung kann einige Minuten dauern. Wenn sie abgeschlossen ist, wird eine Meldung ähnlich der folgenden mit dem Ergebnis angezeigt.

    ![Beispielergebnis nach abgeschlossener Bereitstellung](media/automation-create-account-template/template-output.png)

## <a name="next-steps"></a>Nächste Schritte

Informationen zum Weiterleiten von Auftragsstatus und Auftragsdatenströmen Ihrer Runbooks an den verknüpften Log Analytics-Arbeitsbereich finden Sie unter [Weiterleiten von Azure Automation-Auftragsdaten an Azure Monitor-Protokolle](automation-manage-send-joblogs-log-analytics.md). Dadurch werden die Diagnoseeinstellungen des Automation-Kontos mithilfe von Azure PowerShell-Befehlen konfiguriert, um die Integration für das Senden von Protokollen zur Analyse an den Arbeitsbereich abzuschließen. 
