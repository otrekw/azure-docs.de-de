---
title: Häufige Fragen zu VMs im Azure Marketplace
description: Häufige Fragen, die sich beim Erstellen eines virtuellen Computers im Azure Marketplace ergeben.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: guide
author: kriti-ms
ms.author: krsh
ms.date: 03/10/2021
ms.openlocfilehash: 2975d1f1558bc7f9e4a12c18882e43a163b97982
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "104593423"
---
# <a name="common-questions-about-vm-in-azure-marketplace"></a>Häufige Fragen zu VMs im Azure Marketplace

Diese häufig gestellten Fragen (FAQ) beziehen sich auf allgemeine Probleme, die beim Erstellen eines VM (Virtual Machine)-Angebots im Azure Marketplace auftreten können.

## <a name="how-do-i-configure-a-virtual-private-network-vpn-to-work-with-my-vms"></a>Wie kann ich ein virtuelles privates Netzwerk (VPN) für VMs konfigurieren?

Bei Verwendung des Azure Resource Manager-Bereitstellungsmodells haben Sie drei Optionen:

- [Erstellen eines routenbasierten VPN-Gateways mit dem Azure-Portal](../vpn-gateway/tutorial-create-gateway-portal.md)
- [Erstellen eines routenbasierten VPN-Gateways mit Azure PowerShell](../vpn-gateway/create-routebased-vpn-gateway-powershell.md)
- [Erstellen eines routenbasierten VPN-Gateways mithilfe von CLI](../vpn-gateway/create-routebased-vpn-gateway-cli.md)

## <a name="what-are-microsoft-support-policies-for-running-microsoft-server-software-on-azure-based-vms"></a>Was sind Microsoft-Supportrichtlinien für die Ausführung von Microsoft-Serversoftware auf Azure-basierten VMs?

Ausführliche Informationen finden Sie unter [Microsoft-Server-Software-Support für virtuelle Maschinen von Microsoft Azure](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines).

## <a name="in-a-vm-how-do-i-manage-the-custom-script-extension-in-the-startup-task"></a>Wie kann ich auf einem virtuellen Computer die benutzerdefinierte Skripterweiterung in der Startaufgabe verwalten?

Informationen dazu, wie Sie die benutzerdefinierte Skripterweiterung mit dem Azure PowerShell-Modul verwenden, zu Azure Resource Manager-Vorlagen und zu Problembehandlungsschritten für Windows-Systeme finden Sie unter [CustomScript-Erweiterung für Windows](../virtual-machines/extensions/custom-script-windows.md).

## <a name="are-32-bit-applications-or-services-supported-in-azure-marketplace"></a>Werden 32-Bit-Anwendungen oder -Dienste für Azure Marketplace unterstützt?

Nein. Bei allen unterstützten Betriebssystemen und Standarddiensten für Azure-VMs handelt es sich um 64-Bit-Versionen. Die meisten 64-Bit-Betriebssysteme unterstützen aus Gründen der Abwärtskompatibilität zwar 32-Bit-Versionen von Anwendungen, aber die Nutzung von 32-Bit-Anwendungen für Ihre VM-Lösung wird nicht unterstützt, und wir raten daher dringend von der Nutzung ab. Erstellen Sie Ihre Anwendung als 64-Bit-Projekt neu.

Weitere Informationen und Beispiele finden Sie in diesen Artikeln:

- [Running 32-bit applications](/windows/desktop/WinProg64/running-32-bit-applications) (Ausführen von 32-Bit-Anwendungen)
- [Unterstützung für 32-Bit-Betriebssysteme auf virtuellen Azure-Computern](https://support.microsoft.com/help/4021388/support-for-32-bit-operating-systems-in-azure-virtual-machines)
- [Microsoft-Server-Software-Support für virtuelle Maschinen von Microsoft Azure](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines)

## <a name="error-vhd-is-already-registered-with-image-repository-as-the-resource"></a>Error: Die virtuelle Festplatte ist im Imagerepository bereits als Ressource registriert.

Bei jeder versuchten Erstellung eines Images aus virtuellen Festplatten erhalte ich in Azure PowerShell eine Fehlermeldung mit dem Hinweis, dass die virtuelle Festplatte im Imagerepository bereits als Ressource registriert ist. Ich habe vorher weder ein Image erstellt, noch habe ich ein Image mit diesem Namen in Azure gefunden. Wie löse ich dieses Problem?

Dieses Problem tritt normalerweise auf, wenn Sie eine VM aus einer VHD erstellt haben, die mit einer Sperre versehen ist. Stellen Sie sicher, dass über diese virtuelle Festplatte kein virtueller Computer zugeordnet wird, und wiederholen Sie anschließend den Vorgang. Öffnen Sie ein Supportticket, falls dieses Problem weiterhin besteht. Weitere Informationen finden Sie unter [Support für das Programm „Kommerzieller Marketplace“ im Partner Center](support.md).

## <a name="how-do-i-create-a-vm-from-a-generalized-vhd"></a>Wie erstelle ich eine VM aus einer generalisierten virtuellen Festplatte (VHD)?

### <a name="prepare-an-azure-resource-manager-template"></a>Vorbereiten einer Azure Resource Manager-Vorlage

In diesem Abschnitt wird beschrieben, wie Sie ein vom Benutzer bereitgestelltes VM-Image erstellen und bereitstellen. Dazu können Sie Betriebssystem- und Datenträger-VHD-Images von einer in Azure bereitgestellten virtuellen Festplatte (VHD) bereitstellen. Mit diesen Schritten wird die VM mithilfe einer generalisierten VHD bereitgestellt.

1. Melden Sie sich beim Azure-Portal an.
2. Laden Sie Ihre generalisierten Betriebssystem- und Datenträger-VHDs in Ihr Azure-Speicherkonto hoch.
3. Wählen Sie auf der Startseite Ressource erstellen aus, suchen Sie nach „Vorlagenbereitstellung“, und wählen Sie Erstellen aus.
4. Wählen Sie Eigene Vorlage im Editor erstellen aus.

   :::image type="content" source="media/vm/template-deployment.png" alt-text="Auswahl einer Vorlage":::

5. Fügen Sie die folgende JSON-Vorlage in den Editor ein, und wählen Sie Speichern aus.
 ```json
  {
       "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
       "contentVersion": "1.0.0.0",
       "parameters": {
           "userStorageAccountName": {
               "type": "String"
           },
           "userStorageContainerName": {
               "defaultValue": "vhds",
               "type": "String"
           },
           "dnsNameForPublicIP": {
               "type": "String"
           },
           "adminUserName": {
               "defaultValue": "isv",
               "type": "String"
           },
           "adminPassword": {
               "defaultValue": "",
               "type": "SecureString"
           },
           "osType": {
               "defaultValue": "windows",
               "allowedValues": [
                   "windows",
                   "linux"
               ],
               "type": "String"
           },
           "subscriptionId": {
               "type": "String"
           },
           "location": {
               "type": "String"
           },
           "vmSize": {
               "type": "String"
           },
           "publicIPAddressName": {
               "type": "String"
           },
           "vmName": {
               "type": "String"
           },
           "virtualNetworkName": {
               "type": "String"
           },
           "nicName": {
               "type": "String"
           },
           "vhdUrl": {
               "type": "String",
               "metadata": {
                   "description": "VHD Url..."
               }
           }
       },
       "variables": {
           "addressPrefix": "10.0.0.0/16",
           "subnet1Name": "Subnet-1",
           "subnet2Name": "Subnet-2",
           "subnet1Prefix": "10.0.0.0/24",
           "subnet2Prefix": "10.0.1.0/24",
           "publicIPAddressType": "Dynamic",
           "vnetID": "[resourceId('Microsoft.Network/virtualNetworks',parameters('virtualNetworkName'))]",
           "subnet1Ref": "[concat(variables('vnetID'),'/subnets/',variables('subnet1Name'))]",
           "hostDNSNameScriptArgument": "[concat(parameters('dnsNameForPublicIP'),'.',parameters('location'),'.cloudapp.azure.com')]",
           "osDiskVhdName": "[concat('http://',parameters('userStorageAccountName'),'.blob.core.windows.net/',parameters('userStorageContainerName'),'/',parameters('vmName'),'osDisk.vhd')]"
       },
       "resources": [
           {
               "type": "Microsoft.Network/publicIPAddresses",
               "apiVersion": "2015-06-15",
               "name": "[parameters('publicIPAddressName')]",
               "location": "[parameters('location')]",
               "properties": {
                   "publicIPAllocationMethod": "[variables('publicIPAddressType')]",
                   "dnsSettings": {
                       "domainNameLabel": "[parameters('dnsNameForPublicIP')]"
                   }
               }
           },
           {
               "type": "Microsoft.Network/virtualNetworks",
               "apiVersion": "2015-06-15",
               "name": "[parameters('virtualNetworkName')]",
               "location": "[parameters('location')]",
               "properties": {
                   "addressSpace": {
                       "addressPrefixes": [
                           "[variables('addressPrefix')]"
                       ]
                   },
                   "subnets": [
                       {
                           "name": "[variables('subnet1Name')]",
                           "properties": {
                               "addressPrefix": "[variables('subnet1Prefix')]"
                           }
                       },
                       {
                           "name": "[variables('subnet2Name')]",
                           "properties": {
                               "addressPrefix": "[variables('subnet2Prefix')]"
                           }
                       }
                   ]
               }
           },
           {
               "type": "Microsoft.Network/networkInterfaces",
               "apiVersion": "2015-06-15",
               "name": "[parameters('nicName')]",
               "location": "[parameters('location')]",
               "dependsOn": [
                   "[concat('Microsoft.Network/publicIPAddresses/', parameters('publicIPAddressName'))]",
                   "[concat('Microsoft.Network/virtualNetworks/', parameters('virtualNetworkName'))]"
               ],
               "properties": {
                   "ipConfigurations": [
                       {
                           "name": "ipconfig1",
                           "properties": {
                               "privateIPAllocationMethod": "Dynamic",
                               "publicIPAddress": {
                                   "id": "[resourceId('Microsoft.Network/publicIPAddresses',parameters('publicIPAddressName'))]"
                               },
                               "subnet": {
                                   "id": "[variables('subnet1Ref')]"
                               }
                           }
                       }
                   ]
               }
           },
           {
               "type": "Microsoft.Compute/virtualMachines",
               "apiVersion": "2015-06-15",
               "name": "[parameters('vmName')]",
               "location": "[parameters('location')]",
               "dependsOn": [
                   "[concat('Microsoft.Network/networkInterfaces/', parameters('nicName'))]"
               ],
               "properties": {
                   "hardwareProfile": {
                       "vmSize": "[parameters('vmSize')]"
                   },
                   "osProfile": {
                       "computername": "[parameters('vmName')]",
                       "adminUsername": "[parameters('adminUsername')]",
                       "adminPassword": "[parameters('adminPassword')]"
                   },
                   "storageProfile": {
                       "osDisk": {
                           "name": "[concat(parameters('vmName'),'-osDisk')]",
                           "osType": "[parameters('osType')]",
                           "caching": "ReadWrite",
                           "image": {
                               "uri": "[parameters('vhdUrl')]"
                           },
                           "vhd": {
                               "uri": "[variables('osDiskVhdName')]"
                           },
                           "createOption": "FromImage"
                       }
                   },
                   "networkProfile": {
                       "networkInterfaces": [
                           {
                               "id": "[resourceId('Microsoft.Network/networkInterfaces',parameters('nicName'))]"
                           }
                       ]
                   }
               }
           }
       ]
   }
   ```


6. Geben Sie die Parameterwerte für die angezeigten Eigenschaftenseiten für die benutzerdefinierte Bereitstellung ein.

 **TABELLE 1**

| **ResourceGroupName** | **Vorhandener Name der Azure-Ressourcengruppe. Verwenden Sie in der Regel dieselbe RG wie Ihr Key Vault.** |
| --- | --- |
| TemplateFile | Vollständiger Pfadname der Datei „VHDtoImage.json“. |
| userStorageAccountName | Name des Speicherkontos. |
| dnsNameForPublicIP | DNS-Name für die öffentliche IP-Adresse. Muss in Kleinbuchstaben angegeben werden. |
| subscriptionId | Die Azure-Abonnement-ID. |
| Standort | Standardmäßiger geografischer Azure-Standort der Ressourcengruppe. |
| vmName | Der Name des virtuellen Computers. |
| vhdUrl | Die Webadresse der virtuellen Festplatte. |
| vmSize | Die Größe der VM-Instanz. |
| publicIPAddressName | Der Name der öffentlichen IP-Adresse. |
| virtualNetworkName | Der Name des virtuellen Netzwerks. |
| nicName | Der Name der Netzwerkschnittstellenkarte für das virtuelle Netzwerk. |
| adminUserName | Der Benutzername des Administratorkontos. |
| adminPassword | Das Administratorkennwort. |
|

7. Nachdem Sie diese Werte angegeben haben, wählen Sie Kaufen aus.

Azure startet die Bereitstellung. Im angegebenen Speicherkontopfad wird eine neue VM mit der angegebenen nicht verwalteten VHD erstellt. Sie können den Fortschritt im Azure-Portal nachverfolgen, indem Sie auf der linken Seite Virtuelle Computer auswählen. Nachdem die VM erstellt wurde, ändert sich der Status von „Wird gestartet“ in „Wird ausgeführt“.

Verwenden Sie für die Bereitstellung von VMs der 2. Generation diese Vorlage:
 ```json
 {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
          "userStorageAccountName": {
              "type": "String"
          },
          "userStorageContainerName": {
              "defaultValue": "vhds",
              "type": "String"
          },
          "dnsNameForPublicIP": {
              "type": "String"
          },
          "adminUserName": {
              "defaultValue": "isv",
              "type": "String"
          },
          "adminPassword": {
              "defaultValue": "",
              "type": "SecureString"
          },
          "osType": {
              "defaultValue": "windows",
              "allowedValues": [
                  "windows",
                  "linux"
              ],
              "type": "String"
          },
          "subscriptionId": {
              "type": "String"
          },
          "location": {
              "type": "String"
          },
          "vmSize": {
              "type": "String"
          },
          "publicIPAddressName": {
              "type": "String"
          },
          "vmName": {
              "type": "String"
          },
          "virtualNetworkName": {
              "type": "String"
          },
          "nicName": {
              "type": "String"
          },
          "vhdUrl": {
              "type": "String",
              "metadata": {
                  "description": "VHD Url..."
              }
          }
      },
      "variables": {
          "addressPrefix": "10.0.0.0/16",
          "subnet1Name": "Subnet-1",
          "subnet2Name": "Subnet-2",
          "subnet1Prefix": "10.0.0.0/24",
          "subnet2Prefix": "10.0.1.0/24",
          "publicIPAddressType": "Dynamic",
          "vnetID": "[resourceId('Microsoft.Network/virtualNetworks',parameters('virtualNetworkName'))]",
          "subnet1Ref": "[concat(variables('vnetID'),'/subnets/',variables('subnet1Name'))]",
          "hostDNSNameScriptArgument": "[concat(parameters('dnsNameForPublicIP'),'.',parameters('location'),'.cloudapp.azure.com')]",
          "osDiskVhdName": "[concat('http://',parameters('userStorageAccountName'),'.blob.core.windows.net/',parameters('userStorageContainerName'),'/',parameters('vmName'),'osDisk.vhd')]"
      },
      "resources": [
          {
              "type": "Microsoft.Network/publicIPAddresses",
              "apiVersion": "2015-06-15",
              "name": "[parameters('publicIPAddressName')]",
              "location": "[parameters('location')]",
              "properties": {
                  "publicIPAllocationMethod": "[variables('publicIPAddressType')]",
                  "dnsSettings": {
                      "domainNameLabel": "[parameters('dnsNameForPublicIP')]"
                  }
              }
          },
          {
              "type": "Microsoft.Network/virtualNetworks",
              "apiVersion": "2015-06-15",
              "name": "[parameters('virtualNetworkName')]",
              "location": "[parameters('location')]",
              "properties": {
                  "addressSpace": {
                      "addressPrefixes": [
                          "[variables('addressPrefix')]"
                      ]
                  },
                  "subnets": [
                      {
                          "name": "[variables('subnet1Name')]",
                          "properties": {
                              "addressPrefix": "[variables('subnet1Prefix')]"
                          }
                      },
                      {
                          "name": "[variables('subnet2Name')]",
                          "properties": {
                              "addressPrefix": "[variables('subnet2Prefix')]"
                          }
                      }
                  ]
              }
          },
          {
              "type": "Microsoft.Network/networkInterfaces",
              "apiVersion": "2015-06-15",
              "name": "[parameters('nicName')]",
              "location": "[parameters('location')]",
              "dependsOn": [
                  "[concat('Microsoft.Network/publicIPAddresses/', parameters('publicIPAddressName'))]",
                  "[concat('Microsoft.Network/virtualNetworks/', parameters('virtualNetworkName'))]"
              ],
              "properties": {
                  "ipConfigurations": [
                      {
                          "name": "ipconfig1",
                          "properties": {
                              "privateIPAllocationMethod": "Dynamic",
                              "publicIPAddress": {
                                  "id": "[resourceId('Microsoft.Network/publicIPAddresses',parameters('publicIPAddressName'))]"
                              },
                              "subnet": {
                                  "id": "[variables('subnet1Ref')]"
                              }
                          }
                      }
                  ]
              }
          },
          {
              "type": "Microsoft.Compute/virtualMachines",
              "apiVersion": "2015-06-15",
              "name": "[parameters('vmName')]",
              "location": "[parameters('location')]",
              "dependsOn": [
                  "[concat('Microsoft.Network/networkInterfaces/', parameters('nicName'))]"
              ],
              "properties": {
                  "hardwareProfile": {
                      "vmSize": "[parameters('vmSize')]"
                  },
                  "osProfile": {
                      "computername": "[parameters('vmName')]",
                      "adminUsername": "[parameters('adminUsername')]",
                      "adminPassword": "[parameters('adminPassword')]"
                  },
                  "storageProfile": {
                      "osDisk": {
                          "name": "[concat(parameters('vmName'),'-osDisk')]",
                          "osType": "[parameters('osType')]",
                          "caching": "ReadWrite",
                          "image": {
                              "uri": "[parameters('vhdUrl')]"
                          },
                          "vhd": {
                              "uri": "[variables('osDiskVhdName')]"
                          },
                          "createOption": "FromImage"
                      }
                  },
                  "networkProfile": {
                      "networkInterfaces": [
                          {
                              "id": "[resourceId('Microsoft.Network/networkInterfaces',parameters('nicName'))]"
                          }
                      ]
                  }
              }
          }
      ]
  }
  ```


### <a name="deploy-an-azure-vm-using-powershell"></a>Bereitstellen einer Azure-VM mithilfe von PowerShell

Kopieren Sie das folgende Skript, und bearbeiten Sie es, um Werte für die Variablen `$storageaccount` und `$vhdUrl` anzugeben. Führen Sie das Skript aus, um eine Azure-VM-Ressource von Ihrer vorhandenen generalisierten VHD zu erstellen.

```powershell
# storage account of existing generalized VHD
$storageaccount = "testwinrm11815"
# generalized VHD URL
$vhdUrl = "https://testwinrm11815.blob.core.windows.net/vhds/testvm1234562016651857.vhd"

echo "New-AzResourceGroupDeployment -Name "dplisvvm$postfix" -ResourceGroupName "$rgName" -TemplateFile "C:\certLocation\VHDtoImage.json" -userStorageAccountName "$storageaccount" -dnsNameForPublicIP "$vmName" -subscriptionId "$mysubid" -location "$location" -vmName "$vmName" -vaultName "$kvname" -vaultResourceGroup "$rgName" -certificateUrl
$objAzureKeyVaultSecret.Id -vhdUrl "$vhdUrl" -vmSize "Standard\_A2" -publicIPAddressName "myPublicIP1" -virtualNetworkName "myVNET1" -nicName "myNIC1" -adminUserName "isv" -adminPassword $pwd"

# deploying VM with existing VHD
New-AzResourceGroupDeployment -Name "dplisvvm$postfix" -ResourceGroupName "$rgName"
```

## <a name="how-do-i-test-a-hidden-preview-image"></a>Wie teste ich ein ausgeblendetes Vorschauimage?

Sie können ausgeblendete Vorschauimages mithilfe von Schnellstartvorlagen bereitstellen.
So stellen Sie ein Vorschauimage bereit: 
1. Navigieren Sie zu der jeweiligen Schnellstartvorlage für [Linux](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-linux) oder [Windows](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-windows), und wählen Sie „in Azure bereitstellen“. Dadurch werden Sie zum Azure-Portal geführt.
2. Wählen Sie im Azure-Portal die Option „Vorlage bearbeiten“ aus.
3. Suchen Sie in der JSON-Vorlage nach „imageReference“, und aktualisieren Sie die Parameter „publisherid“, „offerid“, „skuid“ und „version“ des Images. Zum Testen des Vorschauimages hängen Sie „-PREVIEW“ an „offerid“ an.
 ![image](https://user-images.githubusercontent.com/79274470/110191995-71c7d500-7de0-11eb-9f3c-6a42f55d8f03.png)
4. Klicken Sie auf Speichern.
5. Geben Sie die restlichen Details an. Überprüfen und Erstellen


## <a name="next-steps"></a>Nächste Schritte

- [Behandeln von Problemen bei der VM-Zertifizierung](azure-vm-create-certification-faq.md)
