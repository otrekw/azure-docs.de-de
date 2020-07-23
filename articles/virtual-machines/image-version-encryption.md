---
title: 'Vorschau: Erstellen einer mit eigenen Schlüsseln verschlüsselten Imageversion'
description: Erstellen Sie eine Imageversion in einem Katalog mit freigegebenen Images, und verwenden Sie dabei kundenseitig verwaltete Verschlüsselungsschlüssel.
author: cynthn
ms.service: virtual-machines
ms.workload: infrastructure-services
ms.topic: how-to
ms.date: 05/06/2020
ms.author: cynthn
ms.openlocfilehash: 469e225a1cc40dc2ecc45339d9355484e87c4af2
ms.sourcegitcommit: f844603f2f7900a64291c2253f79b6d65fcbbb0c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/10/2020
ms.locfileid: "86223583"
---
# <a name="preview-use-customer-managed-keys-for-encrypting-images"></a>Vorschau: Verwenden von kundenseitig verwalteten Schlüsseln zum Verschlüsseln von Images

Katalogimages werden als verwaltete Datenträger gespeichert und sind daher automatisch per serverseitiger Verschlüsselung verschlüsselt. Die serverseitige Verschlüsselung verwendet die [AES-Verschlüsselung](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard) mit 256 Bit – eine der stärksten verfügbaren Blockchiffren – und ist FIPS 140-2-konform. Weitere Informationen zu den kryptografischen Modulen, die verwalteten Azure-Datenträgern zugrunde liegen, finden Sie unter [Kryptografie-API: Die nächste Generation](/windows/desktop/seccng/cng-portal).

Sie können von der Plattform verwaltete Schlüssel für die Verschlüsselung Ihrer Images verwenden oder die Verschlüsselung mit eigenen Schlüsseln verwalten. Wenn Sie die Verschlüsselung mit eigenen Schlüsseln verwalten möchten, können Sie einen *kundenseitig verwalteten Schlüssel* angeben, der zum Verschlüsseln und Entschlüsseln aller Datenträger in Ihren Images verwendet werden soll. 

Die serverseitige Verschlüsselung mit kundenseitig verwalteten Schlüsseln verwendet Azure Key Vault. Sie können entweder [ihre RSA-Schlüssel](../key-vault/keys/hsm-protected-keys.md) in den Schlüsseltresor importieren oder neue RSA-Schlüssel in Azure Key Vault generieren.

Um kundenseitig verwaltete Schlüssel für Images zu verwenden, benötigen Sie zunächst einen Azure Key Vault. Danach erstellen Sie einen Datenträgerverschlüsselungssatz. Dieser Datenträgerverschlüsselungssatz wird dann beim Erstellen Ihrer Imageversionen verwendet.

Weitere Informationen zum Erstellen und Verwenden von Datenträgerverschlüsselungssätzen finden Sie unter [Vom Kunden verwaltete Schlüssel](./windows/disk-encryption.md#customer-managed-keys).

## <a name="limitations"></a>Einschränkungen

Bei der Verwendung von kundenseitig verwalteten Schlüsseln zum Verschlüsseln von Images aus einem Katalog mit freigegebenen Images gibt es einige Einschränkungen:  

- Verschlüsselungsschlüssel müssen sich in dem gleichen Abonnement und der gleichen Region wie das Image befinden.

- Images, die kundenseitig verwaltete Schlüssel verwenden, können nicht freigegeben werden. 

- Images, die kundenseitig verwaltete Schlüssel verwenden, können nicht in andere Regionen repliziert werden.

- Sobald Sie eigene Schlüssel zum Verschlüsseln eines Datenträgers oder Images verwenden, können Sie nicht zur Verwendung von plattformseitig verwalteten Schlüsseln zum Verschlüsseln dieser Datenträger oder Images zurückkehren.


> [!IMPORTANT]
> Die Verschlüsselung mithilfe kundenseitig verwalteter Schlüssel befindet sich derzeit in der öffentlichen Vorschau.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).


## <a name="powershell"></a>PowerShell

Für die öffentliche Vorschau müssen Sie das Feature zuerst registrieren.

```azurepowershell-interactive
Register-AzProviderFeature -FeatureName SIGEncryption -ProviderNamespace Microsoft.Compute
```

Es dauert einige Minuten, bis die Registrierung abgeschlossen ist. Verwenden Sie „Get-AzProviderFeature“, um den Status der Featureregistrierung zu überprüfen.

```azurepowershell-interactive
Get-AzProviderFeature -FeatureName SIGEncryption -ProviderNamespace Microsoft.Compute
```

Wenn „RegistrationState“ als „Registrated“ zurückgegeben wird, können Sie mit dem nächsten Schritt fortfahren.

Überprüfen Sie Ihre Anbieterregistrierung. Stellen Sie sicher, dass `Registered` zurückgegeben wird.

```azurepowershell-interactive
Get-AzResourceProvider -ProviderNamespace Microsoft.Compute | Format-table -Property ResourceTypes,RegistrationState
```

Wenn nicht `Registered` zurückgegeben wird, registrieren Sie die Anbieter wie folgt:

```azurepowershell-interactive
Register-AzResourceProvider -ProviderNamespace Microsoft.Compute
```

Um einen Datenträgerverschlüsselungssatz für eine Imageversion anzugeben, verwenden Sie [New-AzGalleryImageDefinition](/powershell/module/az.compute/new-azgalleryimageversion) mit dem Parameter `-TargetRegion`. 

```azurepowershell-interactive

$sourceId = <ID of the image version source>

$osDiskImageEncryption = @{DiskEncryptionSetId='subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myRG/providers/Microsoft.Compute/diskEncryptionSets/myDESet'}

$dataDiskImageEncryption1 = @{DiskEncryptionSetId='subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myRG/providers/Microsoft.Compute/diskEncryptionSets/myDESet1';Lun=1}

$dataDiskImageEncryption2 = @{DiskEncryptionSetId='subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myRG/providers/Microsoft.Compute/diskEncryptionSets/myDESet2';Lun=2}

$dataDiskImageEncryptions = @($dataDiskImageEncryption1,$dataDiskImageEncryption2)

$encryption1 = @{OSDiskImage=$osDiskImageEncryption;DataDiskImages=$dataDiskImageEncryptions}

$region1 = @{Name='West US';ReplicaCount=1;StorageAccountType=Standard_LRS;Encryption=$encryption1}

$targetRegion = @{$region1}


# Create the image
New-AzGalleryImageVersion `
   -ResourceGroupName $rgname `
   -GalleryName $galleryName `
   -GalleryImageDefinitionName $imageDefinitionName `
   -Name $versionName -Location $location `
   -SourceImageId $sourceId `
   -ReplicaCount 2 `
   -StorageAccountType Standard_LRS `
   -PublishingProfileEndOfLifeDate '2020-12-01' `
   -TargetRegion $targetRegion
```

### <a name="create-a-vm"></a>Erstellen einer VM

Sie können eine VM aus einem Katalog mit freigegebenen Images erstellen und kundenseitig verwaltete Schlüssel zum Verschlüsseln der Datenträger verwenden. Die Syntax ist die gleiche wie beim Erstellen einer [generalisierten](vm-generalized-image-version-powershell.md) oder [spezialisierten](vm-specialized-image-version-powershell.md) VM aus einem Image. Sie müssen den erweiterten Parametersatz verwenden und `Set-AzVMOSDisk -Name $($vmName +"_OSDisk") -DiskEncryptionSetId $diskEncryptionSet.Id -CreateOption FromImage` zur VM-Konfiguration hinzufügen.

Bei Datenträgern für Daten müssen Sie den Parameter `-DiskEncryptionSetId $setID` hinzufügen, wenn Sie [Add-AzVMDataDisk](/powershell/module/az.compute/add-azvmdatadisk) verwenden.


## <a name="cli"></a>Befehlszeilenschnittstelle (CLI) 

Für die öffentliche Vorschau müssen Sie das Feature zuerst registrieren.

```azurecli-interactive
az feature register --namespace Microsoft.Compute --name SIGEncryption
```

Überprüfen Sie den Status der Featureregistrierung.

```azurecli-interactive
az feature show --namespace Microsoft.Compute --name SIGEncryption | grep state
```

Wenn `"state": "Registered"` zurückgegeben wird, können Sie mit dem nächsten Schritt fortfahren.

Überprüfen Sie die Registrierung.

```azurecli-interactive
az provider show -n Microsoft.Compute | grep registrationState
```

Wenn nicht „registered“ zurückgegeben wird, führen Sie den folgenden Befehl aus:

```azurecli-interactive
az provider register -n Microsoft.Compute
```


Um einen Datenträgerverschlüsselungssatz für eine Imageversion anzugeben, verwenden Sie [az image gallery create-image-version](/cli/azure/sig/image-version#az-sig-image-version-create) mit dem Parameter `--target-region-encryption`. Das Format für `--target-region-encryption` ist eine durch Leerzeichen getrennte Liste mit Schlüsseln für die Verschlüsselung der Datenträger für Betriebssystem und Daten. Diese sollte wie folgt aussehen: `<encryption set for the OS disk>,<Lun number of the data disk>, <encryption set for the data disk>, <Lun number for the second data disk>, <encryption set for the second data disk>`. 

Wenn die Quelle des Betriebssystemdatenträgers ein verwalteter Datenträger oder eine VM ist, verwenden Sie `--managed-image`, um die Quelle für die Imageversion anzugeben. In diesem Beispiel ist die Quelle ein verwaltetes Image, das sowohl über einen Datenträger für das Betriebssystem als auch über einen Datenträger für Daten bei LUN 0 verfügt. Der Betriebssystemdatenträger wird mit DiskEncryptionSet1 verschlüsselt, der Datenträger für Daten mit DiskEncryptionSet2.

```azurecli-interactive
az sig image-version create \
   -g MyResourceGroup \
   --gallery-image-version 1.0.0 \
   --target-regions westus=2=standard_lrs \
   --target-region-encryption DiskEncryptionSet1,0,DiskEncryptionSet2 \
   --gallery-name MyGallery \
   --gallery-image-definition MyImage \
   --managed-image "/subscriptions/<subscription ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/images/myImage"
```

Wenn die Quelle für den Betriebssystemdatenträger eine Momentaufnahme ist, verwenden Sie `--os-snapshot`, um den Datenträger anzugeben. Wenn Sie Datenträgermomentaufnahmen in die Imageversion einschließen möchten, fügen Sie diese hinzu, indem Sie mit `--data-snapshot-luns` die LUN und mit `--data-snapshots` die Momentaufnahmen angeben.

In diesem Beispiel sind die Quellen Datenträgermomentaufnahmen. Es gibt sowohl einen Betriebssystemdatenträger als auch einen Datenträger für Daten bei LUN 0. Der Betriebssystemdatenträger wird mit DiskEncryptionSet1 verschlüsselt, der Datenträger für Daten mit DiskEncryptionSet2.

```azurecli-interactive
az sig image-version create \
   -g MyResourceGroup \
   --gallery-image-version 1.0.0 \
   --target-regions westus=2=standard_lrs \
   --target-region-encryption DiskEncryptionSet1,0,DiskEncryptionSet2 \
   --os-snapshot "/subscriptions/<subscription ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/snapshots/myOSSnapshot"
   --data-snapshot-luns 0
   --data-snapshots "/subscriptions/<subscription ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/snapshots/myDDSnapshot"
   --gallery-name MyGallery \
   --gallery-image-definition MyImage 
   
```

### <a name="create-the-vm"></a>Erstellen des virtuellen Computers

Sie können eine VM aus einem Katalog mit freigegebenen Images erstellen und kundenseitig verwaltete Schlüssel zum Verschlüsseln der Datenträger verwenden. Die Syntax ist die gleiche wie beim Erstellen einer [generalisierten](vm-generalized-image-version-cli.md) oder [spezialisierten](vm-specialized-image-version-cli.md) VM aus einem Image. Sie müssen nur den Parameter `--os-disk-encryption-set` mit der ID des Verschlüsselungsschlüssels hinzufügen. Bei Datenträgern für Daten fügen Sie `--data-disk-encryption-sets` mit einer durch Leerzeichen getrennten Liste der Datenträgerverschlüsselungssätze für die Datenträger hinzu.


## <a name="portal"></a>Portal

Wenn Sie Ihre Imageversion im Portal erstellen, können Sie die Registerkarte **Verschlüsselung** verwenden, um die Informationen zu den Speicherverschlüsselungssätzen einzugeben.

1. Wählen Sie auf der Seite **Imageversion erstellen** die Registerkarte **Verschlüsselung** aus.
2. Wählen Sie unter **Verschlüsselungstyp** die Option **Verschlüsselung ruhender Daten mit einem kundenseitig verwalteten Schlüssel** aus. 
3. Wählen Sie für jeden Datenträger im Image den zu verwendenden **Datenträgerverschlüsselungssatz** aus der Dropdownliste aus. 

### <a name="create-the-vm"></a>Erstellen des virtuellen Computers

Sie können eine VM aus einem Katalog mit freigegebenen Images erstellen und kundenseitig verwaltete Schlüssel zum Verschlüsseln der Datenträger verwenden. Wenn Sie die VM im Portal erstellen, wählen Sie auf der Registerkarte **Datenträger** die Option **Verschlüsselung ruhender Daten mit einem kundenseitig verwalteten Schlüssel** als **Verschlüsselungstyp** aus. Dann können Sie den Verschlüsselungssatz in der Dropdownliste auswählen.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über die [serverseitige Datenträgerverschlüsselung](./windows/disk-encryption.md).

Weitere Informationen zur Bereitstellung von Erwerbsplaninformationen finden Sie unter [Bereitstellen von Azure Marketplace-Erwerbsplaninformationen beim Erstellen von Images](marketplace-images.md).
