---
title: Aktivieren der Datenträgerverschlüsselung für verwaltete Service Fabric-Clusterknoten (Vorschau)
description: Hier erfahren Sie, wie Sie die Datenträgerverschlüsselung für verwaltete Azure Service Fabric-Clusterknoten in Windows mithilfe einer ARM-Vorlage aktivieren.
ms.topic: how-to
ms.date: 02/15/2021
ms.openlocfilehash: aa6388d9d165cd149faae3c19b81320198fd0b02
ms.sourcegitcommit: 4a54c268400b4158b78bb1d37235b79409cb5816
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2021
ms.locfileid: "108127279"
---
# <a name="enable-disk-encryption-for-service-fabric-managed-cluster-preview-nodes"></a>Aktivieren der Datenträgerverschlüsselung für verwaltete Service Fabric-Clusterknoten (Vorschau)

In dieser Anleitung erfahren Sie, wie Sie die Datenträgerverschlüsselung für verwaltete Service Fabric-Clusterknoten in Windows mithilfe der Funktion [Azure Disk Encryption](../virtual-machines/windows/disk-encryption-overview.md) für [VM-Skalierungsgruppen](../virtual-machine-scale-sets/disk-encryption-azure-resource-manager.md) über ARM-Vorlagen (Azure Resource Manager) aktivieren.

## <a name="register-for-azure-disk-encryption"></a>Registrieren für Azure Disk Encryption

Die Vorschauversion der Datenträgerverschlüsselung für die VM-Skalierungsgruppe erfordert eine Selbstregistrierung. Führen Sie den folgenden Befehl aus:

```powershell
Register-AzProviderFeature -FeatureName "UnifiedDiskEncryption" -ProviderNamespace "Microsoft.Compute"
```

Überprüfen Sie den Registrierungsstatus, indem Sie Folgendes ausführen:

```powershell
Get-AzProviderFeature -ProviderNamespace "Microsoft.Compute" -FeatureName "UnifiedDiskEncryption"
```

Sobald sich der Status in *Registriert* geändert hat, können Sie den Vorgang fortsetzen.

## <a name="provision-a-key-vault-for-disk-encryption"></a>Bereitstellen eines Key Vault für die Datenträgerverschlüsselung

Azure Disk Encryption erfordert Azure Key Vault zum Steuern und Verwalten von Verschlüsselungsschlüsseln und Geheimnissen für die Datenträgerverschlüsselung. Ihr Key Vault und Ihr verwalteter Service Fabric-Cluster müssen sich in derselben Azure-Region und demselben Azure-Abonnement befinden. Solange diese Anforderungen erfüllt sind, können Sie einen neuen oder vorhandenen Key Vault verwenden, indem Sie die Datenträgerverschlüsselung für diesen aktivieren.

### <a name="create-key-vault-with-disk-encryption-enabled"></a>Erstellen eines Key Vault mit aktivierter Datenträger Verschlüsselung

Führen Sie die folgenden Befehle aus, um einen neuen Key Vault für die Datenträgerverschlüsselung zu erstellen. Stellen Sie sicher, dass die Region für den Key Vault [für verwaltete Service Fabric-Cluster unterstützt](faq-managed-cluster.md#what-regions-are-supported-in-the-preview) wird und mit der Region des Clusters übereinstimmt.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```powershell
$resourceGroupName = "<rg-name>" 
$keyvaultName = "<kv-name>" 

New-AzResourceGroup -Name $resourceGroupName -Location eastus2 
New-AzKeyVault -ResourceGroupName $resourceGroupName -Name $keyvaultName -Location eastus2 -EnabledForDiskEncryption
```

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

```azurecli
$resourceGroupName = "<rg-name>" 
$keyvaultName = "<kv-name>" 

az keyvault create --resource-group $resourceGroupName --name $keyvaultName --enabled-for-disk-encryption
```

---

### <a name="update-existing-key-vault-to-enable-disk-encryption"></a>Aktualisieren eines vorhandenen Key Vault zum Aktivieren der Datenträgerverschlüsselung

Führen Sie die folgenden Befehle aus, um die Datenträgerverschlüsselung für einen vorhandenen Key Vault zu aktivieren.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```powershell
# ps 

Set-AzKeyVaultAccessPolicy -ResourceGroupName $resourceGroupName -VaultName $keyvaultName -EnabledForDiskEncryption
```

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

```azurecli
az keyvault update --name keyvaultName --enabled-for-disk-encryption 
```

---

## <a name="update-the-template-and-parameters-files-for-disk-encryption"></a>Aktualisieren der Vorlagen- und Parameterdateien für die Datenträgerverschlüsselung

Der folgende Schritt führt Sie durch die erforderlichen Änderungen an der Vorlage, um die Datenträgerverschlüsselung für einen [vorhandenen verwalteten Cluster](tutorial-managed-cluster-deploy.md) zu aktivieren. Alternativ können Sie mit der folgenden Vorlage einen neuen verwalteten Service Fabric-Cluster mit aktivierter Datenträgerverschlüsselung bereitstellen: https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master/SF-Managed-Standard-SKU-1-NT-DiskEncryption

1. Fügen Sie der Vorlage die folgenden Parameter hinzu, und ersetzen Sie dabei das Abonnement, den Ressourcengruppennamen und den Key Vault-Namen unter `keyVaultResourceId` durch Ihre eigenen Werte:

```json
"parameters": { 
…
    "keyVaultResourceId": { 
        "type": "string", 
        "defaultValue": "/subscriptions/########-####-####-####-############/resourceGroups/<rg-name>/providers/Microsoft.KeyVault/vaults/<kv-name>", 
        "metadata": { 
            "description": "Full resource id of the Key Vault used for disk encryption." 
        } 
    },
    "volumeType": { 
        "type": "string", 
        "defaultValue": "All", 
        "metadata": { 
            "description": "Type of the volume OS or Data to perform encryption operation" 
        }
    }
}, 
```

2. Fügen Sie als Nächstes die VM-Erweiterung `AzureDiskEncryption` den verwalteten Clusterknotentypen in der Vorlage hinzu:

```json
"properties": { 
    "vmExtensions": [ 
        { 
            "name": "AzureDiskEncryption", 
            "properties": { 
                "publisher": "Microsoft.Azure.Security", 
                "type": "AzureDiskEncryption", 
                "typeHandlerVersion": "2.2", 
                "autoUpgradeMinorVersion": true, 
                "settings": {                     
                    "EncryptionOperation": "EnableEncryption", 
                    "KeyVaultURL": "[reference(parameters('keyVaultResourceId'),'2016-10-01').vaultUri]", 
                    "KeyVaultResourceId": "[parameters('keyVaultResourceID')]",
                    "VolumeType": "[parameters('volumeType')]" 
                } 
            } 
        } 
    ] 
} 
```

3. Aktualisieren Sie schließlich die Parameterdatei, und ersetzen Sie dabei das Abonnement, die Ressourcengruppe und den Key Vault-Namen in *keyVaultResourceId* durch Ihre eigenen Werte:

```json
"parameters": { 
    … 
    "keyVaultResourceId": { 
        "value": "/subscriptions/########-####-####-####-############/resourceGroups/<rg-name>/providers/Microsoft.KeyVault/vaults/<kv-name>" 
    },   
    "volumeType": { 
        "value": "All" 
    }    
} 
```

## <a name="deploy-and-verify-the-changes"></a>Bereitstellen und Überprüfen der Änderungen

Sobald Sie fertig sind, stellen Sie die Änderungen bereit, um die Datenträgerverschlüsselung für Ihren verwalteten Cluster zu aktivieren.

```powershell
$clusterName = "<clustername>" 

New-AzResourceGroupDeployment -Name $resourceGroupName -ResourceGroupName $resourceGroupName -TemplateFile .\template_diskEncryption.json -TemplateParameterFile \.parameters_diskEncryption.json -Debug -Verbose 
```

Mithilfe des Befehls `Get-AzVmssDiskEncryption` können Sie den Status der Datenträgerverschlüsselung für die zugrunde liegende Skalierungsgruppe eines Knotentyps überprüfen. Zuerst müssen Sie den Namen der unterstützenden Ressourcengruppe Ihres verwalteten Clusters (mit dem zugrunde liegenden virtuellen Netzwerk, dem Load Balancer, der öffentlichen IP-Adresse, der NSG, den Skalierungsgruppen und Speicherkonten) ermitteln. Achten Sie darauf, dass Sie `VmssName` in den Namen des zu überprüfenden Clusterknotens (wie in der Bereitstellungsvorlage angegeben) ändern.

```powershell
$VmssName = "NT1"
$supportResourceGroupName = "SFC_" + (Get-AzServiceFabricManagedCluster -ResourceGroupName $resourceGroupName).ClusterId
Get-AzVmssDiskEncryption -ResourceGroupName $supportResourceGroupName -VMScaleSetName $VmssName
```

Die Ausgabe sollte in etwa folgendermaßen aussehen:

```console
ResourceGroupName            : SFC_########-####-####-####-############
VmScaleSetName               : NT1
EncryptionEnabled            : True
EncryptionExtensionInstalled : True
```

## <a name="next-steps"></a>Nächste Schritte

[Sample: verwalteter Standard-SKU-Service Fabric-Cluster, 1 Knotentyp mit aktivierter Datenträgerverschlüsselung](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master/SF-Managed-Standard-SKU-1-NT-DiskEncryption)

[Azure Disk Encryption für Windows-VMs](../virtual-machines/windows/disk-encryption-overview.md)

[Verschlüsseln von VM-Skalierungsgruppen mit Azure Resource Manager](../virtual-machine-scale-sets/disk-encryption-azure-resource-manager.md)
