---
title: Verwenden von Azure Resource Manager-Vorlagen für das Onboarding der Updateverwaltung | Microsoft-Dokumentation
description: Sie können eine Azure Resource Manager-Vorlage verwenden, um das Onboarding der Azure Automation-Updateverwaltungslösung durchzuführen.
ms.service: automation
ms.subservice: update-management
ms.topic: conceptual
author: mgoedtel
ms.author: magoedte
ms.date: 03/30/2020
ms.openlocfilehash: e69f3d7350d0da9f364983eae0935532b576bd76
ms.sourcegitcommit: 27bbda320225c2c2a43ac370b604432679a6a7c0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/31/2020
ms.locfileid: "80411473"
---
# <a name="onboard-update-management-solution-using-azure-resource-manager-template"></a>Onboarding der Updateverwaltungslösung mithilfe einer Azure Resource Manager-Vorlage

Sie können [Azure Resource Manager-Vorlagen](../azure-resource-manager/templates/template-syntax.md) verwenden, um die Azure Automation-Updateverwaltungslösung in Ihrer Ressourcengruppe zu aktivieren. Dieser Artikel enthält eine Beispielvorlage, die Folgendes automatisiert:

* Erstellen eines Azure Monitor Log Analytics-Arbeitsbereichs
* Erstellen eines Azure Automation-Kontos.
* Verknüpft das Automation-Konto mit dem Log Analytics-Arbeitsbereich, falls nicht bereits erfolgt.
* Onboarding der Azure Automation-Updateverwaltungslösung

Die Vorlage automatisiert nicht das Onboarding von Azure- oder Nicht-Azure-VMs.

Wenn Sie in Ihrem Abonnement bereits einen Log Analytics-Arbeitsbereich und ein Automation-Konto in einer unterstützten Region bereitgestellt haben, sind diese nicht verknüpft, und in dem Arbeitsbereich ist die Updateverwaltungslösung nicht bereits bereitgestellt. Durch die Verwendung dieser Vorlage wird die Verknüpfung erfolgreich erstellt und die Updateverwaltungslösung bereitgestellt. 

## <a name="api-versions"></a>API-Versionen

Die folgende Tabelle enthält die API-Versionen für die Ressourcen, die in diesem Beispiel verwendet werden.

| Resource | Ressourcentyp | API-Version |
|:---|:---|:---|
| Arbeitsbereich | workspaces | 2017-03-15-preview |
| Automation-Konto | automation | 2015-10-31 | 
| Lösung | solutions | 2015-11-01-preview |

## <a name="before-using-the-template"></a>Vor dem Verwenden der Vorlage

Wenn Sie PowerShell lokal installieren und verwenden möchten, müssen Sie für diesen Artikel das Az-Modul für Azure PowerShell verwenden. Führen Sie `Get-Module -ListAvailable Az` aus, um die Version zu ermitteln. Sollte ein Upgrade erforderlich sein, lesen Sie [Installieren des Azure PowerShell-Moduls](/powershell/azure/install-az-ps). Wenn Sie PowerShell lokal ausführen, müssen Sie auch `Connect-AzAccount` ausführen, um eine Verbindung mit Azure herzustellen. Bei der Azure PowerShell verwendet die Bereitstellung [New-AzResourceGroupDeployment](/powershell/module/az.resources/new-azresourcegroupdeployment).

Wenn Sie die Befehlszeilenschnittstelle (CLI) lokal installieren und verwenden möchten, müssen Sie für diesen Artikel mindestens Version 2.1.0 der Azure-Befehlszeilenschnittstelle ausführen. Führen Sie `az --version` aus, um die Version zu ermitteln. Informationen zum Durchführen einer Installation oder eines Upgrades finden Sie bei Bedarf unter [Installieren der Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest). Bei der Azure CLI verwendet diese Bereitstellung [az group deployment create](https://docs.microsoft.com/cli/azure/group/deployment?view=azure-cli-latest#az-group-deployment-create). 

Die JSON-Vorlage ist so konfiguriert, dass Sie Folgendes von Ihnen anfordert:

* Den Namen des Arbeitsbereichs.
* Die Region, in der der den Arbeitsbereich erstellt werden soll.
* Den Namen des Automation-Kontos.
* Die Region, in der das Konto erstellt werden soll.

Die JSON-Vorlage gibt einen Standardwerte für die anderen Parameter an, die wahrscheinlich als Standardkonfiguration in Ihrer Umgebung verwendet werden. Für den gemeinsamen Zugriff in Ihrer Organisation können Sie die Vorlage in einem Azure Storage-Konto speichern. Weitere Informationen zum Arbeiten mit Vorlagen finden Sie unter [Bereitstellen von Ressourcen mit Azure Resource Manager-Vorlagen und der Azure-Befehlszeilenschnittstelle](../azure-resource-manager/templates/deploy-cli.md).

Die folgenden Parameter in der Vorlage werden mit einem Standardwert für den Log Analytics-Arbeitsbereich festgelegt:

* SKU: Als Standardwert wird der neue, im Preismodell von April 2018 veröffentlichte Tarif pro GB verwendet.
* Datenaufbewahrung: Als Standardwert werden dreißig Tage verwendet.
* Kapazitätsreservierung: Standardwerte bis zu 100 GB

>[!WARNING]
>Wenn Sie einen Log Analytics-Arbeitsbereich in einem Abonnement mit dem neuen Preismodell von April 2018 erstellen oder konfigurieren, ist **PerGB2018** als einziger gültiger Log Analytics-Tarif verfügbar.
>

>[!NOTE]
>Vor der Verwendung dieser Vorlage lesen Sie die [zusätzlichen Details](../azure-monitor/platform/template-workspace-configuration.md#create-a-log-analytics-workspace), die Optionen für die Arbeitsbereichskonfiguration vollständig zu verstehen, z. B. Zugriffssteuerungmodus, Tarif, Aufbewahrung und Kapazitätsreservierungsstufe. Wenn Sie noch keine Erfahrungen mit Azure Monitor-Protokollen noch keinen Arbeitsbereich bereitgestellt haben, sollten Sie die Anleitung zum [Arbeitsbereichsdesign](../azure-monitor/platform/design-logs-deployment.md) lesen, um Informationen zur Zugriffssteuerung zu erhalten und ein Verständnis für die Designimplementierungsstrategien zu erhalten, die wir für Ihre Organisation empfehlen.

## <a name="deploy-template"></a>Bereitstellen der Vorlage

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
                "description": "Pricing tier: perGB2018 or legacy tiers (Free, Standalone, PerNode, Standard or Premium) which are not available to all customers."
            }
        },
        "dataRetention": {
            "type": "int",
            "defaultValue": 30,
            "minValue": 7,
            "maxValue": 730,
            "metadata": {
                "description": "Number of days of retention. Workspaces in the legacy Free pricing tier can only have 7 days."
            }
        },
        "immediatePurgeDataOn30Days": {
            "type": "bool",
            "defaultValue": "[bool('false')]",
            "metadata": {
                "description": "If set to true when changing retention to 30 days, older data will be immediately deleted. Use this with extreme caution. This only applies when retention is being set to 30 days."
            }
        },
        "location": {
            "type": "string",
            "allowedValues": [
                "australiacentral",
                "australiaeast",
                "australiasoutheast",
                "brazilsouth",
                "canadacentral",
                "centralindia",
                "centralus",
                "eastasia",
                "eastus",
                "eastus2",
                "francecentral",
                "japaneast",
                "koreacentral",
                "northcentralus",
                "northeurope",
                "southafricanorth",
                "southcentralus",
                "southeastasia",
                "uksouth",
                "ukwest",
                "westcentralus",
                "westeurope",
                "westus",
                "westus2"
            ],
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
        }
    },
    "variables": {
        "Updates": {
            "name": "[concat('Updates', '(', parameters('workspaceName'), ')')]",
            "galleryName": "Updates"
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
                    "apiVersion": "2015-11-01-preview",
                    "location": "[resourceGroup().location]",
                    "name": "[variables('Updates').name]",
                    "type": "Microsoft.OperationsManagement/solutions",
                    "id": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.OperationsManagement/solutions/', variables('Updates').name)]",
                    "dependsOn": [
                        "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
                    ],
                    "properties": {
                        "workspaceResourceId": "[resourceId('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
                    },
                    "plan": {
                        "name": "[variables('Updates').name]",
                        "publisher": "Microsoft",
                        "promotionCode": "",
                        "product": "[concat('OMSGallery/', variables('Updates').galleryName)]"
                    }
                }
            ]
        },
        {
            "type": "Microsoft.Automation/automationAccounts",
            "apiVersion": "2015-01-01-preview",
            "name": "[parameters('automationAccountName')]",
            "location": "[parameters('automationAccountLocation')]",
            "dependsOn": [],
            "tags": {},
            "properties": {
                "sku": {
                    "name": "Basic"
                }
            },
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
    ```

2. Bearbeiten Sie die Vorlage entsprechend Ihren Anforderungen. Erstellen Sie ggf. eine [Resource Manager-Parameterdatei](../azure-resource-manager/templates/parameter-files.md), anstatt Parameter als Inlinewerte zu übergeben.

3. Speichern Sie diese Datei unter dem Namen „deployUMSolutiontemplate.json“ in einem lokalen Ordner.

4. Nun können Sie die Vorlage bereitstellen. Sie können dazu entweder PowerShell oder Azure CLI verwenden. Wenn Sie zur Eingabe des Namens eines Arbeitsbereichs oder Automation-Kontos aufgefordert werden, geben Sie einen Namen an, der in allen Azure-Abonnements global eindeutig ist.

    **PowerShell**

    ```powershell
    New-AzResourceGroupDeployment -Name <deployment-name> -ResourceGroupName <resource-group-name> -TemplateFile deployUMSolutiontemplate.json
    ```

    **Azure-Befehlszeilenschnittstelle**

    ```cli
    az group deployment create --resource-group <my-resource-group> --name <my-deployment-name> --template-file deployUMSolutiontemplate.json
    ```

    Die Bereitstellung kann einige Minuten dauern. Wenn sie abgeschlossen ist, wird eine Meldung ähnlich der folgenden mit dem Ergebnis angezeigt:

    ![Beispielergebnis nach abgeschlossener Bereitstellung](media/automation-update-management-deploy-template/template-output.png)

## <a name="next-steps"></a>Nächste Schritte

Jetzt, da Sie die Updateverwaltungslösung bereitgestellt haben, können Sie VMs für die Verwaltung aktivieren, Updatebewertungen überprüfen und Updates bereitstellen, um sie in Übereinstimmung mit den Vorschriften zu bringen.

- Aus Ihrem [Azure Automation-Konto](automation-onboard-solutions-from-automation-account.md) für einen oder mehrere Azure-Computer und manuell für Nicht-Azure-Computer.

- Für einen einzelnen virtuellen Azure-Computer auf der Seite für virtuelle Computer im Azure-Portal. Dieses Szenario steht für virtuelle Computer unter [Linux](../virtual-machines/linux/tutorial-config-management.md#enable-update-management) oder [Windows](../virtual-machines/windows/tutorial-config-management.md#enable-update-management) zur Verfügung.

- Für [mehrere virtuelle Azure-Computer](manage-update-multi.md), indem Sie sie auf der Seite **Virtuelle Computer** im Azure-Portal auswählen. 