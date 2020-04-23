---
title: 'Zertifizierung virtueller Azure-Computer: Azure Marketplace'
description: Es wird beschrieben, wie Sie ein VM-Angebot im kommerziellen Marketplace testen und übermitteln.
author: emuench
ms.author: mingshen
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/09/2020
ms.openlocfilehash: 9bd7e40855f30612b90cf28365c0b1410cd3e3d8
ms.sourcegitcommit: ffc6e4f37233a82fcb14deca0c47f67a7d79ce5c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/21/2020
ms.locfileid: "81731123"
---
# <a name="azure-virtual-machine-vm-image-certification"></a>Azure-VM-Imagezertifizierung

> [!NOTE]
> Wir verlagern die Verwaltung Ihrer Azure-VM-Angebote vom Cloud-Partnerportal ins Partner Center. Befolgen Sie bis zum Abschluss der Migration Ihrer Angebote die Anweisungen zur Angebotsverwaltung im Cloud-Partnerportal unter [Erstellen von Zertifikaten für Azure Key Vault](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/virtual-machine/cpp-create-key-vault-cert).

In diesem Artikel wird beschrieben, wie Sie ein VM-Image im kommerziellen Marketplace testen und übermitteln, um sicherzustellen, dass es die aktuellen Anforderungen für die Veröffentlichung im Azure Marketplace erfüllt.

Führen Sie die folgenden Schritte aus, bevor Sie Ihr VM-Angebot übermitteln:

1. Erstellen und Bereitstellen von Zertifikaten
2. Bereitstellen einer Azure-VM mit Ihrem generalisierten Image
3. Durchführen von Überprüfungen

## <a name="create-and-deploy-certificates-for-azure-key-vault"></a>Erstellen und Bereitstellen von Zertifikaten für Azure Key Vault

In diesem Abschnitt wird beschrieben, wie Sie die selbstsignierten Zertifikate erstellen und bereitstellen, die zum Einrichten der Konnektivität für die Windows-Remoteverwaltung (WinRM) auf einem in Azure gehosteten virtuellen Computer benötigt werden.

### <a name="create-certificates-for-azure-key-vault"></a>Erstellen von Zertifikaten für Azure Key Vault

Dieser Vorgang besteht aus drei Schritten:

1. Erstellen Sie das Sicherheitszertifikat.
2. Erstellen Sie die Azure Key Vault-Instanz, auf der das Zertifikat gespeichert werden soll.
3. Speichern Sie die Zertifikate im Schlüsseltresor.

Für diesen Vorgang können Sie entweder eine neue oder eine vorhandene Azure-Ressourcengruppe verwenden.

#### <a name="create-the-security-certificate"></a>Erstellen des Sicherheitszertifikats

Bearbeiten Sie das folgende Azure PowerShell-Skript, und führen Sie es aus, um die Zertifikatsdatei (PFX) in einem lokalen Ordner zu erstellen. Ersetzen Sie die Werte für die Parameter, die in der folgenden Tabelle aufgeführt sind.

| **Parameter** | **Beschreibung** |
| --- | --- |
| $certroopath | Der lokale Ordner, in dem die PFX-Datei gespeichert werden soll. |
| $location | Einer der geografischen Azure-Standardstandorte. |
| $vmName | Der Name des geplanten virtuellen Azure-Computers. |
| $certname | Der Name des Zertifikats. Er muss mit dem vollqualifizierten Domänennamen des geplanten virtuellen Computers übereinstimmen. |
| $certpassword | Das Kennwort für die Zertifikate. Es muss mit dem Kennwort für den geplanten virtuellen Computer übereinstimmen. |
| | |

```PowerShell
   # Certification creation script

    # pfx certification stored path
    $certroopath = "C:\certLocation"

    # location of the resource group
    $location = "westus"

    # Azure virtual machine name that we are going to create
    $vmName = "testvm000000906"

    # Certification name - should match with FQDN of Windows Azure creating VM
    $certname = "$vmName.$location.cloudapp.azure.com"

    # Certification password - should be match with password of Windows Azure creating VM
    $certpassword = "SecretPassword@123"

    $cert=New-SelfSignedCertificate -DnsName "$certname" -CertStoreLocation cert:\LocalMachine\My
    $pwd = ConvertTo-SecureString -String $certpassword -Force -AsPlainText
    $certwithThumb="cert:\localMachine\my\"+$cert.Thumbprint
    $filepath="$certroopath\$certname.pfx"
    Export-PfxCertificate -cert $certwithThumb -FilePath $filepath -Password $pwd
    Remove-Item -Path $certwithThumb

```

> [!TIP]
> Lassen Sie bei diesen Schritten dieselbe Azure PowerShell-Konsolensitzung geöffnet und aktiv, um die Werte der verschiedenen Parameter beizubehalten.

> [!WARNING]
> Wenn Sie dieses Skript speichern, sollten Sie es ausschließlich an einem sicheren Ort ablegen, da es Sicherheitsinformationen (ein Kennwort) enthält.

#### <a name="create-the-azure-key-vault-to-store-the-certificate"></a>Erstellen des Azure-Schlüsseltresors, in dem das Zertifikat gespeichert werden soll

Kopieren Sie den Inhalt der unten angegebenen Vorlage in eine Datei auf dem lokalen Computer. Im folgenden Beispielskript ist dies die Ressource `C:\certLocation\keyvault.json`.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "keyVaultName": {
      "type": "string",
      "defaultValue":"isvkv0001",
      "metadata": {
        "description": "Name of the Vault"
      }
    },
    "tenantId": {
      "type": "string",
      "defaultValue":"72f988bf-86f1-41af-91ab-2d7cd011db47",
      "metadata": {
        "description": "Tenant Id of the subscription. Get using Get-AzureSubscription cmdlet or Get Subscription API"
      }
    },
    "objectId": {
      "type": "string",
      "defaultValue":"d55739bf-d5d6-4ce0-be1c-49ade53c4315",
      "metadata": {
        "description": "Object Id of the AD user. Get using Get-AzureADUser or Get-AzureADServicePrincipal cmdlets"
      }
    },
    "keysPermissions": {
      "type": "array",
      "defaultValue": ["all"],
      "metadata": {
        "description": "Permissions to keys in the vault. Valid values are: all, create, import, update, get, list, delete, backup, restore, encrypt, decrypt, wrapkey, unwrapkey, sign, and verify."
      }
    },
    "secretsPermissions": {
      "type": "array",
      "defaultValue": ["all"],
      "metadata": {
        "description": "Permissions to secrets in the vault. Valid values are: all, get, set, list, and delete."
      }
    },
    "skuName": {
      "type": "string",
      "defaultValue": "Standard",
      "allowedValues": [
        "Standard",
        "Premium"
      ],
      "metadata": {
        "description": "SKU for the vault"
      }
    },
    "enableVaultForDeployment": {
      "type": "bool",
      "defaultValue": true,
      "allowedValues": [
        true,
        false
      ],
      "metadata": {
        "description": "Specifies if the vault is enabled for a VM deployment"
      }
    }
  },
  "resources": [
    {
      "type": "Microsoft.KeyVault/vaults",
      "name": "[parameters('keyVaultName')]",
      "apiVersion": "2015-06-01",
      "location": "[resourceGroup().location]",
      "properties": {
        "enabledForDeployment": "[parameters('enableVaultForDeployment')]",
        "tenantId": "[parameters('tenantId')]",
        "accessPolicies": [
          {
            "tenantId": "[parameters('tenantId')]",
            "objectId": "[parameters('objectId')]",
            "permissions": {
              "keys": "[parameters('keysPermissions')]",
              "secrets": "[parameters('secretsPermissions')]"
            }
          }
        ],
        "sku": {
          "name": "[parameters('skuName')]",
          "family": "A"
        }
      }
    }
  ]
}

```

Bearbeiten Sie das folgende Azure PowerShell-Skript, und führen Sie es aus, um einen Azure-Schlüsseltresor und die zugeordnete Ressourcengruppe zu erstellen. Ersetzen Sie die Werte für die Parameter, die in der folgenden Tabelle aufgeführt sind.

| **Parameter** | **Beschreibung** |
| --- | --- |
| $postfix | Zufällige numerische Zeichenfolge, die an Bereitstellungsbezeichner angefügt ist. |
| $rgName | Der Name der zu erstellenden Azure-Ressourcengruppe (RG). |
| $location | Einer der geografischen Azure-Standardstandorte. |
| $kvTemplateJson | Der Pfad der Datei (keyvault.json), die die Resource Manager-Vorlage für den Schlüsseltresor enthält. |
| $kvname | Der Name des neuen Schlüsseltresors.|
|   |   |

```PowerShell
    # Creating Key vault in resource group

    # "Random" number for deployment identifiers
    $postfix = "0101048"

    # Resource group name
    $rgName = "TestRG$postfix"

    # Location of Resource Group
    $location = "westus"

    # Key vault template location
    $kvTemplateJson = "C:\certLocation\keyvault.json"

    # Key vault name
    $kvname = "isvkv$postfix"

    # code snippet to get the Azure user object ID
    try
       {
        $accounts = Get-AzureAccount
        $accountNum = 0
        $accounts.Id | %{ ++$accountNum; Write-Host $accountNum $_}
        Write-Host "`nPlease select User, e.g. 1:" -ForegroundColor DarkYellow
        [Int] $accountChoice = Read-Host

        While($accountChoice -lt 1 -or $accountChoice -gt $accounts.Length)
        {
            Write-Host "incorrect input" -ForegroundColor Red
            Write-Host "`nPlease select User, e.g. 1:" -ForegroundColor DarkYellow
            [Int] $accountChoice = Read-Host
        }

        $accountSelected = $accounts[$accountChoice-1]
        echo $accountSelected
        $id = $accountSelected.Id

        Write-Host "User $id Selected"
        $myobjectId=(Get-AzADUser -Mail $id)[0].Id
      }
      catch
      {
      Write-Host $_.Exception.Message
      Break
      }

    # code snippet to get Azure User Tenant Id
      # SELECT Subscriptions
        #**************************************
        try
        {
        $subslist=Get-AzureSubscription
        for($i=1; $i -le $subslist.Length;$i++)
        {
           Write-Host ($i.ToString() +":"+ $subslist[$i-1].SubscriptionName)
        }
        Write-Host "`nPlease pick subscription from above, e.g. 1:" -ForegroundColor DarkYellow
        [int] $selectedsub=Read-Host

        While($selectedsub -lt 1 -or $selectedsub -gt $subslist.Length)
        {
            Write-Host "incorrect input" -ForegroundColor Red
            for($i=1; $i -le $subslist.Length;$i++)
             {
              Write-Host ($i.ToString() +":"+ $subslist[$i-1].SubscriptionName)
             }
            Write-Host "`nPlease pick subscription from above, e.g. 1:" -ForegroundColor DarkYellow
           [int] $selectedsub=Read-Host
        }
        if($selectedsub -ge 1 -and $selectedsub -le $subslist.Length)
        {
        $mysubid=$subslist[$selectedsub-1].SubscriptionId
        $mysubName=$subslist[$selectedsub-1].SubscriptionName
        $mytenantId=$subslist[$selectedsub-1].TenantId
        Write-Host "$mysubName selected"
        }
        }
        catch
        {
        Write-Host $_.Exception.Message
        Break
        }

    # Create a resource group
     Write-Host "Creating Resource Group $rgName"
     Create-ResourceGroup -rgName $rgName -location $location
     Write-Host "-----------------------------------"

    # Create key vault and configure access
    New-AzResourceGroupDeployment -Name "kvdeploy$postfix" -ResourceGroupName $rgName -TemplateFile $kvTemplateJson -keyVaultName $kvname -tenantId $mytenantId -objectId $myobjectId

    Set-AzKeyVaultAccessPolicy -VaultName $kvname -ObjectId $myobjectId -PermissionsToKeys all -PermissionsToSecrets all

```

#### <a name="store-the-certificates-to-the-key-vault"></a>Speichern der Zertifikate im Schlüsseltresor

Speichern Sie die Zertifikate, die in der PFX-Datei enthalten sind, mit diesem Skript im neuen Schlüsseltresor:

```PowerShell
     $fileName =$certroopath+"\$certname"+".pfx"

     $fileContentBytes = get-content $fileName -Encoding Byte
     $fileContentEncoded = [System.Convert]::ToBase64String($fileContentBytes)

            $jsonObject = @"
    {
    "data": "$filecontentencoded",
    "dataType" :"pfx",
    "password": "$certpassword"
    }
"@
            echo $certpassword
            $jsonObjectBytes = [System.Text.Encoding]::UTF8.GetBytes($jsonObject)
            $jsonEncoded = [System.Convert]::ToBase64String($jsonObjectBytes)
            $secret = ConvertTo-SecureString -String $jsonEncoded -AsPlainText -Force
            $objAzureKeyVaultSecret=Set-AzureKeyVaultSecret -VaultName $kvname -Name "ISVSecret$postfix" -SecretValue $secret
            echo $objAzureKeyVaultSecret.Id

```

## <a name="deploy-an-azure-vm-using-your-generalized-image"></a>Bereitstellen einer Azure-VM mit Ihrem generalisierten Image

In diesem Abschnitt wird beschrieben, wie Sie ein generalisiertes VHD-Image für die Erstellung einer neuen Azure-VM-Ressource bereitstellen. Für diesen Prozess verwenden wir die bereitgestellte Azure Resource Manager-Vorlage und das Azure PowerShell-Skript.

### <a name="prepare-an-azure-resource-manager-template"></a>Vorbereiten einer Azure Resource Manager-Vorlage

Kopieren Sie die folgende Azure Resource Manager-Vorlage für die VHD-Bereitstellung in eine lokale Datei mit dem Namen „VHDtoImage.json“. Im nächsten Skript wird der Speicherort auf dem lokalen Computer für diesen JSON-Code angefordert.

```JSON
{
    "$schema": "https://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "userStorageAccountName": {
            "type": "string"
        },
        "userStorageContainerName": {
            "type": "string",
            "defaultValue": "vhds"
        },
        "dnsNameForPublicIP": {
            "type": "string"
        },
        "adminUserName": {
            "defaultValue": "isv",
            "type": "string"
        },
        "adminPassword": {
            "type": "securestring",
            "defaultValue": "Password@123"
        },
        "osType": {
            "type": "string",
            "defaultValue": "windows",
            "allowedValues": [
                "windows",
                "linux"
            ]
        },
        "subscriptionId": {
            "type": "string"
        },
        "location": {
            "type": "string"
        },
        "vmSize": {
            "type": "string"
        },
        "publicIPAddressName": {
            "type": "string"
        },
        "vmName": {
            "type": "string"
        },
        "virtualNetworkName": {
            "type": "string"
        },
        "nicName": {
            "type": "string"
        },
        "vaultName": {
            "type": "string",
            "metadata": {
                "description": "Name of the KeyVault"
            }
        },
        "vaultResourceGroup": {
            "type": "string",
            "metadata": {
                "description": "Resource Group of the KeyVault"
            }
        },
        "certificateUrl": {
            "type": "string",
            "metadata": {
                "description": "Url of the certificate with version in KeyVault e.g. https://testault.vault.azure.net/secrets/testcert/b621es1db241e56a72d037479xab1r7"
            }
        },
        "vhdUrl": {
            "type": "string",
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
            "osDiskVhdName": "[concat('http://',parameters('userStorageAccountName'),'.blob.core.windows.net/',parameters('userStorageContainerName'),'/',parameters('vmName'),'osDisk.vhd')]"
        },
        "resources": [
            {
                "apiVersion": "2015-05-01-preview",
                "type": "Microsoft.Network/publicIPAddresses",
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
                "apiVersion": "2015-05-01-preview",
                "type": "Microsoft.Network/virtualNetworks",
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
                "apiVersion": "2015-05-01-preview",
                "type": "Microsoft.Network/networkInterfaces",
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
                "apiVersion": "2015-06-15",
                "type": "Microsoft.Compute/virtualMachines",
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
                        "adminPassword": "[parameters('adminPassword')]",
                        "secrets": [
                            {
                                "sourceVault": {
                                    "id": "[resourceId(parameters('vaultResourceGroup'), 'Microsoft.KeyVault/vaults', parameters('vaultName'))]"
                                },
                                "vaultCertificates": [
                                    {
                                        "certificateUrl": "[parameters('certificateUrl')]",
                                        "certificateStore": "My"
                                    }
                                ]
                            }
                        ],
                        "windowsConfiguration": {
                            "provisionVMAgent": "true",
                            "winRM": {
                                "listeners": [
                                    {
                                        "protocol": "http"
                                    },
                                    {
                                        "protocol": "https",
                                        "certificateUrl": "[parameters('certificateUrl')]"
                                    }
                                ]
                            },
                            "enableAutomaticUpdates": "true"
                        }
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
                    },
                "diagnosticsProfile": {
                    "bootDiagnostics": {
                        "enabled": true,
                        "storageUri": "[concat('http://', parameters('userStorageAccountName'), '.blob.core.windows.net')]"
                    }
                }
                }
            }
        ]
    }

```

Bearbeiten Sie diese Datei, um Werte für die folgenden Parameter anzugeben:

| **Parameter** | **Beschreibung** |
| --- | --- |
| ResourceGroupName | Name einer vorhandenen Azure-Ressourcengruppe. Normalerweise verwenden Sie dieselbe Ressourcengruppe wie für Ihren Schlüsseltresor. |
| TemplateFile | Vollständiger Pfadname der Datei „VHDtoImage.json“. |
| userStorageAccountName | Name des Speicherkontos. |
| sNameForPublicIP | DNS-Name für die öffentliche IP-Adresse. Muss in Kleinbuchstaben angegeben werden. |
| subscriptionId | Die Azure-Abonnement-ID. |
| Standort | Standardmäßiger geografischer Azure-Standort der Ressourcengruppe. |
| vmName | Der Name des virtuellen Computers. |
| vaultName | Der Name des Schlüsseltresors. |
| vaultResourceGroup | Die Ressourcengruppe des Schlüsseltresors. |
| certificateUrl | Webadresse (URL) des Zertifikats, einschließlich der im Schlüsseltresor gespeicherten Version, z. B. `https://testault.vault.azure.net/secrets/testcert/b621es1db241e56a72d037479xab1r7`. |
| vhdUrl | Die Webadresse der virtuellen Festplatte. |
| vmSize | Die Größe der VM-Instanz. |
| publicIPAddressName | Der Name der öffentlichen IP-Adresse. |
| virtualNetworkName | Der Name des virtuellen Netzwerks. |
| nicName | Der Name der Netzwerkschnittstellenkarte für das virtuelle Netzwerk. |
| adminUserName | Der Benutzername des Administratorkontos. |
| adminPassword | Das Administratorkennwort. |
|   |   |

### <a name="deploy-an-azure-vm"></a>Bereitstellen einer Azure-VM

Kopieren Sie das folgende Skript, und bearbeiten Sie es, um Werte für die Variablen `$storageaccount` und `$vhdUrl` anzugeben. Führen Sie das Skript aus, um eine Azure-VM-Ressource von Ihrer vorhandenen generalisierten VHD zu erstellen.

```PowerShell

# storage account of existing generalized VHD

$storageaccount = "testwinrm11815"

# generalized VHD URL

$vhdUrl = "https://testwinrm11815.blob.core.windows.net/vhds/testvm1234562016651857.vhd"

echo "New-AzResourceGroupDeployment -Name "dplisvvm$postfix" -ResourceGroupName "$rgName" -TemplateFile "C:\certLocation\VHDtoImage.json" -userStorageAccountName "$storageaccount" -dnsNameForPublicIP "$vmName" -subscriptionId "$mysubid" -location "$location" -vmName "$vmName" -vaultName "$kvname" -vaultResourceGroup "$rgName" -certificateUrl $objAzureKeyVaultSecret.Id  -vhdUrl "$vhdUrl" -vmSize "Standard\_A2" -publicIPAddressName "myPublicIP1" -virtualNetworkName "myVNET1" -nicName "myNIC1" -adminUserName "isv" -adminPassword $pwd"

# deploying VM with existing VHD

New-AzResourceGroupDeployment -Name"dplisvvm$postfix" -ResourceGroupName"$rgName" -TemplateFile"C:\certLocation\VHDtoImage.json" -userStorageAccountName"$storageaccount" -dnsNameForPublicIP"$vmName" -subscriptionId"$mysubid" -location"$location" -vmName"$vmName" -vaultName"$kvname" -vaultResourceGroup"$rgName" -certificateUrl$objAzureKeyVaultSecret.Id  -vhdUrl"$vhdUrl" -vmSize"Standard\_A2" -publicIPAddressName"myPublicIP1" -virtualNetworkName"myVNET1" -nicName"myNIC1" -adminUserName"isv" -adminPassword$pwd

```

## <a name="run-validations"></a>Durchführen von Überprüfungen

Es gibt zwei Möglichkeiten, wie für das bereitgestellte Image Überprüfungen durchgeführt werden können:

- Verwenden des „Certification Test Tool for Azure Certified“
- Verwenden der Selbsttest-API

### <a name="download-and-run-the-certification-test-tool"></a>Herunterladen und Ausführen des Tools zum Testen der Zertifizierung

Das Certification Test Tool for Azure Certified wird auf einem lokalen Windows-Computer ausgeführt, aber es wird eine Azure-basierte Windows- oder Linux-VM getestet. Das Tool zertifiziert, ob Ihr VM-Benutzerimage mit Microsoft Azure genutzt werden kann und ob während der Vorbereitung Ihrer virtuellen Festplatte die Richtlinien und Anforderungen erfüllt wurden. Die Ausgabe des Tools ist ein Kompatibilitätsbericht, den Sie in das Partner Center-Portal hochladen, um die Zertifizierung Ihrer VM anzufordern.

1. Laden Sie das neueste [Certification Test Tool for Azure Certified](https://www.microsoft.com/download/details.aspx?id=44299) herunter, und installieren Sie es.
2. Öffnen Sie das Zertifizierungstool, und wählen Sie dann **Neuen Test starten** aus.
3. Geben Sie auf dem Bildschirm **Testinformationen** einen **Testnamen** für den Testlauf ein.
4. Wählen Sie die **Plattform** für Ihre VM aus (entweder „Windows Server“ oder „Linux“). Die Auswahl der Plattform wirkt sich auf die restlichen Optionen aus.
5. Aktivieren Sie das Kontrollkästchen **Für Azure SQL-Datenbank testen**, wenn Ihre VM diesen Datenbankdienst verwendet.

### <a name="connect-the-certification-tool-to-a-vm-image"></a>Verbinden des Zertifizierungstools mit einem VM-Image

Das Tool verbindet Windows-basierte VMs mit [Azure PowerShell](https://docs.microsoft.com/powershell/) und Linux-VMs per [SSH.NET](https://www.ssh.com/ssh/protocol/).

### <a name="connect-the-certification-tool-to-a-linux-vm-image"></a>Verbinden des Zertifizierungstools mit einem Linux-VM-Image

1. Wählen Sie den Modus für die **SSH-Authentifizierung** aus: „Kennwortauthentifizierung“ oder „Authentifizierung per Schlüsseldatei“.
2. Geben Sie bei Verwendung der Authentifizierung per Kennwort Werte für den **VM-DNS-Namen**, den **Benutzernamen** und das **Kennwort** ein. Sie können auch die Standardnummer für den **SSH-Port** ändern.

    ![Azure Certified Test Tool: Kennwortauthentifizierung eines Linux-VM-Images](media/avm-cert2.png)

3. Wenn Sie die Authentifizierung per Schlüsseldatei verwenden, geben Sie Werte für den **VM-DNS-Namen**, den **Benutzernamen** und den Speicherort des **privaten Schlüssels** ein. Sie können auch eine **Passphrase** angeben oder die Standardnummer für den **SSH-Port** ändern.

### <a name="connect-the-certification-tool-to-a-windows-based-vm-image"></a>**Verbinden des Zertifizierungstools mit einem Windows-basierten VM-Image**

1. Geben Sie den vollqualifizierten **VM-DNS-Namen** ein (z. B. „MyVMName.Cloudapp.net“).
2. Geben Sie Werte für den **Benutzernamen** und das **Kennwort** ein.

    ![Azure Certified Test Tool: Kennwortauthentifizierung eines Windows-VM-Images](media/avm-cert4.png)

### <a name="run-a-certification-test"></a>Ausführen eines Zertifizierungstests

Nachdem Sie die Parameterwerte für Ihr VM-Image im Zertifizierungstool angegeben haben, klicken Sie auf **Verbindung testen**, um eine gültige Verbindung mit Ihrer VM zu erstellen. Klicken Sie nach dem Überprüfen der Verbindung auf **Weiter**, um den Test zu starten. Nach Abschluss des Tests werden die Testergebnisse in einer Tabelle angezeigt. In der Spalte „Status“ wird für jeden Test „Bestanden“, „Fehler“ oder „Warnung“ angezeigt. Wenn einer der Tests nicht erfolgreich durchgeführt wird, wird das Image _nicht_ zertifiziert. Überprüfen Sie in diesem Fall die Anforderungen und Fehlermeldungen, nehmen Sie die vorgeschlagenen Änderungen vor, und führen Sie den Test erneut durch.

Geben Sie nach Abschluss des automatisierten Tests auf dem Bildschirm **Fragebogen** auf den Registerkarten **Allgemeine Bewertung** und **Kernelanpassung** weitere Informationen zu Ihrem VM-Image an, und wählen Sie anschließend **Weiter** aus.

Auf dem letzten Bildschirm können Sie zusätzliche Informationen angeben, z. B. SSH-Zugriffsinformationen für ein Linux-VM-Image und Erläuterungen für nicht erfolgreiche Bewertungen bei Ausnahmen.

Wählen Sie abschließend **Bericht generieren** aus, um zusätzlich zu Ihren Antworten im Fragebogen auch die Testergebnisse und Protokolldateien für die ausgeführten Testfälle herunterzuladen. Speichern Sie die Ergebnisse in demselben Container wie die VHDs.

## <a name="next-step"></a>Nächster Schritt

- [Abrufen des Shared Access Signature-URI für Ihr VM-Image](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/virtual-machine/cpp-get-sas-uri)
