---
title: 'Schnellstart: Automatisierte VM-Bereitstellung mit Azure App Configuration'
description: In dieser Schnellstartanleitung wird veranschaulicht, wie Sie das Azure PowerShell-Modul und Azure Resource Manager-Vorlagen verwenden, um einen Azure App Configuration-Speicher bereitzustellen. Verwenden Sie die Werte im Speicher, um eine VM bereitzustellen.
author: lisaguthrie
ms.author: lcozzens
ms.date: 03/05/2020
ms.topic: quickstart
ms.service: azure-app-configuration
ms.custom:
- mvc
- subject-armqs
ms.openlocfilehash: c45f6855c33dff2790ced306fd7f049b98dd1387
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/26/2020
ms.locfileid: "79126384"
---
# <a name="quickstart-automated-vm-deployment-with-app-configuration-and-resource-manager-template"></a>Schnellstart: Automatisierte VM-Bereitstellung mit App Configuration und Resource Manager-Vorlage

Das Azure PowerShell-Modul dient zum Erstellen und Verwalten von Azure-Ressourcen mithilfe von PowerShell-Cmdlets oder -Skripts. In dieser Schnellstartanleitung wird veranschaulicht, wie Sie Azure PowerShell und Azure Resource Manager-Vorlagen verwenden, um einen Azure App Configuration-Speicher bereitzustellen. Anschließend wird beschrieben, wie Sie die Schlüsselwerte im Speicher zum Bereitstellen einer VM verwenden.

Sie nutzen die erforderliche Vorlage, um einen App Configuration-Speicher zu erstellen, und fügen dem Speicher dann Schlüsselwerte hinzu, indem Sie das Azure-Portal oder die Azure CLI verwenden. In der primären Vorlage wird auf vorhandene Schlüsselwertkonfigurationen aus einem vorhandenen Konfigurationsspeicher verwiesen. Die abgerufenen Werte werden verwendet, um Eigenschaften der Ressourcen festzulegen, die von der Vorlage erstellt werden (in diesem Beispiel eine VM).

[!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

## <a name="before-you-begin"></a>Voraussetzungen

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

* Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen.

* Für diesen Schnellstart ist das Azure PowerShell-Modul erforderlich. Führen Sie `Get-Module -ListAvailable Az` aus, um die Version zu ermitteln, die auf Ihrem lokalen Computer installiert ist. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Install and configure Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-Az-ps) (Installieren des Azure PowerShell-Moduls) Informationen dazu.

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich mit dem Befehl `Connect-AzAccount` bei Ihrem Azure-Abonnement an, und geben Sie im Popupbrowserfenster Ihre Azure-Anmeldeinformationen ein:

```azurepowershell-interactive
# Connect to your Azure account
Connect-AzAccount
```

Falls Sie mehrere Abonnements haben, führen Sie die folgenden Cmdlets aus, um das Abonnement auszuwählen, das Sie für diese Schnellstartanleitung verwenden möchten. Vergessen Sie nicht, `<your subscription name>` durch den Namen Ihres Abonnements zu ersetzen:

```azurepowershell-interactive
# List all available subscriptions.
Get-AzSubscription

# Select the Azure subscription you want to use to create the resource group and resources.
Get-AzSubscription -SubscriptionName "<your subscription name>" | Select-AzSubscription
```

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Erstellen Sie mit [New-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup) eine Azure-Ressourcengruppe. Eine Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen bereitgestellt und verwaltet werden.

```azurepowershell-interactive
$resourceGroup = "StreamAnalyticsRG"
$location = "WestUS2"
New-AzResourceGroup `
    -Name $resourceGroup `
    -Location $location
```

## <a name="deploy-an-azure-app-configuration-store"></a>Bereitstellen eines Azure App Configuration-Speichers

Bevor Sie Schlüsselwerte auf die VM anwenden können, müssen Sie über einen vorhandenen Azure App Configuration-Speicher verfügen. In diesem Abschnitt wird beschrieben, wie Sie einen Azure App Configuration-Speicher bereitstellen, indem Sie eine Azure Resource Manager-Vorlage verwenden. Wenn Sie bereits über einen App Configuration-Speicher verfügen, können Sie mit dem nächsten Abschnitt dieses Artikels fortfahren. 

1. Kopieren Sie den folgenden JSON-Code, und fügen Sie ihn in eine neue Datei mit dem Namen *prereq.azuredeploy.json* ein.

   ```json
   {
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
      "configStoreName": {
        "type": "string",
        "metadata": {
          "description": "Specifies the name of the app configuration store."
        }
      },
      "location": {
        "type": "string",
        "defaultValue": "[resourceGroup().location]",
        "metadata": {
          "description": "Specifies the Azure location where the app configuration store should be created."
        }
      },
      "skuName": {
        "type": "string",
        "defaultValue": "standard",
        "metadata": {
          "description": "Specifies the SKU of the app configuration store."
        }
      }
    },
    "resources": [
      {
        "type": "Microsoft.AppConfiguration/configurationStores",
        "name": "[parameters('configStoreName')]",
        "apiVersion": "2019-10-01",
        "location": "[parameters('location')]",
        "sku": {
          "name": "[parameters('skuName')]"
        }
      }
    ]
   }
   ```

1. Kopieren Sie den folgenden JSON-Code, und fügen Sie ihn in eine neue Datei mit dem Namen *prereq.azuredeploy.parameters.json* ein. Ersetzen Sie **GET-UNIQUE** durch einen eindeutigen Namen für Ihren Configuration-Speicher.

   ```json
   {
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
      "configStoreName": {
        "value": "GET-UNIQUE"
      }
    }
   }
   ```

1. Führen Sie in Ihrem PowerShell-Fenster den folgenden Befehl aus, um den Azure App Configuration-Speicher bereitzustellen. Vergessen Sie nicht, den Ressourcengruppennamen und die Pfade für die Vorlagendatei und die Vorlagenparameterdatei zu ersetzen.

   ```azurepowershell
   New-AzResourceGroupDeployment `
       -ResourceGroupName "<your resource group>" 
       -TemplateFile "<path to prereq.azuredeploy.json>" `
       -TemplateParameterFile "<path to prereq.azuredeploy.parameters.json>"
   ```

## <a name="add-vm-configuration-key-values"></a>Hinzufügen von Schlüsselwerten für die VM-Konfiguration

Sie können einen App Configuration-Speicher erstellen, indem Sie eine Azure Resource Manager-Vorlage verwenden, aber Sie müssen über das Azure-Portal oder per Azure CLI Schlüsselwerte hinzufügen. In dieser Schnellstartanleitung fügen Sie Schlüsselwerte über das Azure-Portal hinzu.

1. Navigieren Sie nach Abschluss der Bereitstellung im [Azure-Portal](https://portal.azure.com) zum neu erstellten App Configuration-Speicher.

1. Wählen Sie **Einstellungen** > **Zugriffsschlüssel** aus. Notieren Sie sich die schreibgeschützte Primärschlüssel-Verbindungszeichenfolge. Diese Verbindungszeichenfolge verwenden Sie später zum Konfigurieren Ihrer Anwendung für die Kommunikation mit dem erstellten App Configuration-Speicher.

1. Wählen Sie **Konfigurations-Explorer** > **Erstellen** aus, um die folgenden Schlüssel-Wert-Paare hinzuzufügen:

   |Schlüssel|Wert|
   |-|-|
   |windowsOsVersion|2019-Datacenter|
   |diskSizeGB|1023|
  
   Geben Sie unter **Beschriftung** das Wort *template* ein, aber lassen Sie **Inhaltstyp** leer.

## <a name="deploy-vm-using-stored-key-values"></a>Bereitstellen eines virtuellen Computers mit gespeicherten Schlüsselwerten

Nachdem Sie dem Speicher Schlüsselwerte hinzugefügt haben, können Sie mit einer Azure Resource Manager-Vorlage einen virtuellen Computer bereitstellen. In der Vorlage wird auf die von Ihnen erstellten Schlüssel **windowsOsVersion** und **diskSizeGB** verwiesen.

1. Kopieren Sie den folgenden JSON-Code, und fügen Sie ihn in eine neue Datei mit dem Namen *azuredeploy.json* ein, oder laden Sie die Datei von der Seite mit den [Azure-Schnellstartvorlagen](https://github.com/Azure/azure-quickstart-templates/blob/master/101-app-configuration/azuredeploy.json) herunter.

   ```json
   {
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "adminUsername": {
            "type": "string",
            "metadata": {
                "description": "Admin user name."
            }
        },
        "adminPassword": {
            "type": "securestring",
            "metadata": {
                "description": "Password for the Virtual Machine."
            }
        },
        "appConfigStoreName": {
            "type": "string",
            "metadata": {
                "description": "App configuration store name."
            }
        },
        "appConfigStoreResourceGroup": {
            "type": "string",
            "metadata": {
                "description": "Name of the resource group for the app config store."
            }
        },
        "domainNameLabel": {
            "type": "string",
            "metadata": {
                "description": "The DNS label for the public IP address. It must be lowercase. It should match the following regular expression, or it will raise an error: ^[a-z][a-z0-9-]{1,61}[a-z0-9]$."
            }
        },
        "location": {
            "type": "string",
            "defaultValue": "[resourceGroup().location]",
            "metadata": {
                "description": "Location for all resources."
            }
        },
        "vmSize": {
            "type": "string",
            "defaultValue": "Standard_D2_v3",
            "metadata": {
                "description": "Size of the VM"
            }
        },
        "vmSkuKey": {
            "type": "string",
            "metadata": {
                "description": "Name of the key in the app config store for the VM windows sku"
            }
        },
        "diskSizeKey": {
            "type": "string",
            "metadata": {
                "description": "Name of the key in the app config store for the VM disk size"
            }
        },
        "storageAccountName": {
            "type": "string",
            "metadata": {
                "description": "The name of the storage account."
            }
        }
    },
    "variables": {
        "nicName": "myVMNic",
        "addressPrefix": "10.0.0.0/16",
        "subnetName": "Subnet",
        "subnetPrefix": "10.0.0.0/24",
        "publicIPAddressName": "myPublicIP",
        "vmName": "SimpleWinVM",
        "virtualNetworkName": "MyVNET",
        "subnetRef": "[resourceId('Microsoft.Network/virtualNetworks/subnets', variables('virtualNetworkName'), variables('subnetName'))]",
        "appConfigRef": "[resourceId(parameters('appConfigStoreResourceGroup'), 'Microsoft.AppConfiguration/configurationStores', parameters('appConfigStoreName'))]",
        "windowsOSVersionParameters": {
            "key": "[parameters('vmSkuKey')]",
            "label": "template"
        },
        "diskSizeGBParameters": {
            "key": "[parameters('diskSizeKey')]",
            "label": "template"
        }
    },
    "resources": [
        {
            "type": "Microsoft.Storage/storageAccounts",
            "apiVersion": "2018-11-01",
            "name": "[parameters('storageAccountName')]",
            "location": "[parameters('location')]",
            "sku": {
                "name": "Standard_LRS"
            },
            "kind": "Storage",
            "properties": {
            }
        },
        {
            "type": "Microsoft.Network/publicIPAddresses",
            "apiVersion": "2018-11-01",
            "name": "[variables('publicIPAddressName')]",
            "location": "[parameters('location')]",
            "properties": {
                "publicIPAllocationMethod": "Dynamic",
                "dnsSettings": {
                    "domainNameLabel": "[parameters('domainNameLabel')]"
                }
            }
        },
        {
            "type": "Microsoft.Network/virtualNetworks",
            "apiVersion": "2018-11-01",
            "name": "[variables('virtualNetworkName')]",
            "location": "[parameters('location')]",
            "properties": {
                "addressSpace": {
                    "addressPrefixes": [
                        "[variables('addressPrefix')]"
                    ]
                },
                "subnets": [
                    {
                        "name": "[variables('subnetName')]",
                        "properties": {
                            "addressPrefix": "[variables('subnetPrefix')]"
                        }
                    }
                ]
            }
        },
        {
            "type": "Microsoft.Network/networkInterfaces",
            "apiVersion": "2018-11-01",
            "name": "[variables('nicName')]",
            "location": "[parameters('location')]",
            "dependsOn": [
                "[resourceId('Microsoft.Network/publicIPAddresses/', variables('publicIPAddressName'))]",
                "[resourceId('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]"
            ],
            "properties": {
                "ipConfigurations": [
                    {
                        "name": "ipconfig1",
                        "properties": {
                            "privateIPAllocationMethod": "Dynamic",
                            "publicIPAddress": {
                                "id": "[resourceId('Microsoft.Network/publicIPAddresses',variables('publicIPAddressName'))]"
                            },
                            "subnet": {
                                "id": "[variables('subnetRef')]"
                            }
                        }
                    }
                ]
            }
        },
        {
            "type": "Microsoft.Compute/virtualMachines",
            "apiVersion": "2018-10-01",
            "name": "[variables('vmName')]",
            "location": "[parameters('location')]",
            "dependsOn": [
                "[resourceId('Microsoft.Storage/storageAccounts/', parameters('storageAccountName'))]",
                "[resourceId('Microsoft.Network/networkInterfaces/', variables('nicName'))]"
            ],
            "properties": {
                "hardwareProfile": {
                    "vmSize": "[parameters('vmSize')]"
                },
                "osProfile": {
                    "computerName": "[variables('vmName')]",
                    "adminUsername": "[parameters('adminUsername')]",
                    "adminPassword": "[parameters('adminPassword')]"
                },
                "storageProfile": {
                    "imageReference": {
                        "publisher": "MicrosoftWindowsServer",
                        "offer": "WindowsServer",
                        "sku": "[listKeyValue(variables('appConfigRef'), '2019-10-01', variables('windowsOSVersionParameters')).value]",
                        "version": "latest"
                    },
                    "osDisk": {
                        "createOption": "FromImage"
                    },
                    "dataDisks": [
                        {
                            "diskSizeGB": "[listKeyValue(variables('appConfigRef'), '2019-10-01', variables('diskSizeGBParameters')).value]",
                            "lun": 0,
                            "createOption": "Empty"
                        }
                    ]
                },
                "networkProfile": {
                    "networkInterfaces": [
                        {
                            "id": "[resourceId('Microsoft.Network/networkInterfaces',variables('nicName'))]"
                        }
                    ]
                },
                "diagnosticsProfile": {
                    "bootDiagnostics": {
                        "enabled": true,
                        "storageUri": "[reference(resourceId('Microsoft.Storage/storageAccounts/', parameters('storageAccountName'))).primaryEndpoints.blob]"
                    }
                }
            }
        }
    ],
    "outputs": {
        "hostname": {
            "type": "string",
            "value": "[reference(variables('publicIPAddressName')).dnsSettings.fqdn]"
        }
    }
   }
   ```

1. Kopieren Sie den folgenden JSON-Code, und fügen Sie ihn in eine neue Datei mit dem Namen *azuredeploy.parameters.json* ein, oder laden Sie die Datei von der Seite mit den [Azure-Schnellstartvorlagen](https://github.com/Azure/azure-quickstart-templates/blob/master/101-app-configuration/azuredeploy.parameters.json) herunter.

   ```json
   {
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
      "adminPassword": {
        "value": "GEN-PASSWORD"
      },
      "appConfigStoreName":{
        "value": "GEN-APPCONFIGSTORE-NAME"
      },
      "appConfigStoreResourceGroup": {
         "value": "GEN-APPCONFIGSTORE-RESOURCEGROUP-NAME"
      },
      "vmSkuKey":{
        "value": "GEN-APPCONFIGSTORE-WINDOWSOSVERSION"
      },
      "diskSizeKey" :{
         "value": "GEN-APPCONFIGSTORE-DISKSIZEGB"
      },
      "adminUsername":{
        "value": "GEN-UNIQUE"
      },
      "storageAccountName":{
        "value": "GEN-UNIQUE"
      },
      "domainNameLabel":{
        "value": "GEN-UNIQUE"
      }
    }
   }
   ```

   Ersetzen Sie die Parameterwerte in der Vorlage durch die folgenden Werte:

   |Parameter|Wert|
   |-|-|
   |adminPassword|Ein Administratorkennwort für den virtuellen Computer.|
   |appConfigStoreName|Der Name Ihres Azure App Configuration-Speichers.|
   |appConfigStoreResourceGroup|Die Ressourcengruppe, in der Ihr App Configuration-Speicher enthalten ist.|
   |vmSkuKey|*windowsOSVersion*|
   |diskSizeKey|*diskSizeGB*|
   |adminUsername|Ein Administratorbenutzername für den virtuellen Computer.|
   |storageAccountName|Ein eindeutiger Name für ein Speicherkonto, das dem virtuellen Computer zugeordnet ist.|
   |domainNameLabel|Ein eindeutiger Domänenname.|

1. Führen Sie in Ihrem PowerShell-Fenster den folgenden Befehl aus, um den Azure App Configuration-Speicher bereitzustellen. Vergessen Sie nicht, den Ressourcengruppennamen, den Pfad der Vorlagendatei und den Pfad der Vorlagenparameterdatei zu ersetzen.

   ```azurepowershell
   New-AzResourceGroupDeployment `
       -ResourceGroupName "<your resource group>" 
       -TemplateFile "<path to prereq.azuredeploy.json>" `
       -TemplateParameterFile "<path to prereq.azuredeploy.parameters.json>"
   ```

Glückwunsch! Sie haben mithilfe von Konfigurationen, die in Azure App Configuration gespeichert sind, einen virtuellen Computer bereitgestellt.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Löschen Sie die Ressourcengruppe, den App Configuration-Speicher, den virtuellen Computer und alle zugehörigen Ressourcen, wenn Sie sie nicht mehr benötigen. Falls Sie planen, den App Configuration-Speicher oder den virtuellen Computer weiter zu nutzen, können Sie das Löschen überspringen. Wenn Sie diesen Auftrag nicht weiter verwenden möchten, löschen Sie alle in diesem Schnellstart erstellten Ressourcen, indem Sie das folgende Cmdlet ausführen:

```azurepowershell-interactive
Remove-AzResourceGroup `
  -Name $resourceGroup
```

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie einen virtuellen Computer bereitgestellt, indem Sie eine Azure Resource Manager-Vorlage und Schlüsselwerte aus Azure App Configuration verwendet haben.

Fahren Sie mit dem folgenden Artikel fort, um sich über die Erstellung anderer Anwendungen mit Azure App Configuration zu informieren:

> [!div class="nextstepaction"]
> [Schnellstart: Erstellen einer ASP.NET Core-App mit Azure App Configuration](quickstart-aspnet-core-app.md)
