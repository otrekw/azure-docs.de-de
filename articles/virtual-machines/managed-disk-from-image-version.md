---
title: Erstellen eines verwalteten Datenträgers aus einer Imageversion
description: Erstellen eines verwalteten Datenträgers aus einer Imageversion in einer Shared Image Gallery.
author: cynthn
ms.service: virtual-machines
ms.subservice: shared-image-gallery
ms.topic: how-to
ms.workload: infrastructure
ms.date: 10/27/2020
ms.author: cynthn
ms.reviewer: olayemio
ms.openlocfilehash: 359816bbf3fb93a4ec2bfa3092adbb2236ae2f07
ms.sourcegitcommit: 7edadd4bf8f354abca0b253b3af98836212edd93
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2021
ms.locfileid: "102562553"
---
# <a name="create-a-managed-disk-from-an-image-version"></a>Erstellen eines verwalteten Datenträgers aus einer Imageversion

Falls erforderlich, können Sie das Betriebssystem oder einen einzelnen Datenträger aus einer Imageversion als verwalteten Datenträger aus einer Imageversion exportieren, die in einer Shared Image Gallery gespeichert ist.


## <a name="cli"></a>Befehlszeilenschnittstelle (CLI)

Listen Sie die Imageversionen in einem Katalog mithilfe von [az sig image-version list](/cli/azure/sig/image-version#az_sig_image_version_list) auf. In diesem Beispiel suchen wir nach allen Imageversionen, die Teil der Imagedefinition *myImageDefinition* im Imagekatalog *myGallery* sind.

```azurecli-interactive
az sig image-version list \
   --resource-group myResourceGroup\
   --gallery-name myGallery \
   --gallery-image-definition myImageDefinition \
   -o table
```

Legen Sie die `source`-Variable auf die ID der Imageversion fest, und verwenden Sie anschließend [az disk create](/cli/azure/disk#az_disk_create), um den verwalteten Datenträger zu erstellen. 

In diesem Beispiel exportieren wir den Betriebssystemdatenträger der Imageversion, um einen verwalteten Datenträger namens *myManagedOSDisk* in der Region *EastUS* in einer Ressourcengruppe namens *myResourceGroup* zu erstellen. 

```azurecli-interactive
source="/subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.Compute/galleries/<galleryName>/images/<galleryImageDefinition>/versions/<imageVersion>"

az disk create --resource-group myResourceGroup --location EastUS --name myManagedOSDisk --gallery-image-reference $source 
```



Wenn Sie einen Datenträger aus der Imageversion exportieren möchten, fügen Sie `--gallery-image-reference-lun` hinzu, um den LUN-Speicherort des zu exportierenden Datenträgers anzugeben. 

In diesem Beispiel exportieren wir den Datenträger an LUN 0 der Imageversion, um einen verwalteten Datenträger namens *myManagedDataDisk* in der Region *EastUS* in einer Ressourcengruppe namens *myResourceGroup* zu erstellen. 

```azurecli-interactive
source="/subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.Compute/galleries/<galleryName>/images/<galleryImageDefinition>/versions/<imageVersion>"

az disk create --resource-group myResourceGroup --location EastUS --name myManagedDataDisk --gallery-image-reference $source --gallery-image-reference-lun 0
``` 

## <a name="powershell"></a>PowerShell

Listen Sie die Imageversionen in einem Katalog mithilfe von [Get-AzResource](/powershell/module/az.resources/get-azresource) auf. 

```azurepowershell-interactive
Get-AzResource `
   -ResourceType Microsoft.Compute/galleries/images/versions | `
   Format-Table -Property Name,ResourceId,ResourceGroupName
```

Sobald Sie über alle erforderlichen Informationen verfügen, können Sie [Get-AzGalleryImageVersion](/powershell/module/az.compute/get-azgalleryimageversion) verwenden, um die gewünschte Version des Quellimages abzurufen und sie einer Variable zuzuweisen. In diesem Beispiel rufen wir die `1.0.0`-Imageversion der `myImageDefinition`-Definition im `myGallery`-Quellkatalog in der `myResourceGroup`-Ressourcengruppe ab.

```azurepowershell-interactive
$sourceImgVer = Get-AzGalleryImageVersion `
   -GalleryImageDefinitionName myImageDefinition `
   -GalleryName myGallery `
   -ResourceGroupName myResourceGroup `
   -Name 1.0.0
```

Nachdem Sie die Variable `source` auf die ID der Imageversion festgelegt haben, verwenden Sie [New-AzDiskConfig](/powershell/module/az.compute/new-azdiskconfig) zum Erstellen einer Datenträgerkonfiguration und [New-AzDisk](/powershell/module/az.compute/new-azdisk) zum Erstellen des Datenträgers. 

In diesem Beispiel exportieren wir den Betriebssystemdatenträger der Imageversion, um einen verwalteten Datenträger namens *myManagedOSDisk* in der Region *EastUS* in einer Ressourcengruppe namens *myResourceGroup* zu erstellen. 

Erstellt Sie eine Datenträgerkonfiguration.
```azurepowershell-interactive
$diskConfig = New-AzDiskConfig `
   -Location EastUS `
   -CreateOption FromImage `
   -GalleryImageReference @{Id = $sourceImgVer.Id}
```

Erstellen Sie den Datenträger.

```azurepowershell-interactive
New-AzDisk -Disk $diskConfig `
   -ResourceGroupName myResourceGroup `
   -DiskName myManagedOSDisk
```

Wenn Sie einen Datenträger mit der Imageversion exportieren möchten, fügen Sie der Datenträgerkonfiguration eine LUN-ID hinzu, um den LUN-Speicherort des zu exportierenden Datenträgers anzugeben. 

In diesem Beispiel exportieren wir den Datenträger an LUN 0 der Imageversion, um einen verwalteten Datenträger namens *myManagedDataDisk* in der Region *EastUS* in einer Ressourcengruppe namens *myResourceGroup* zu erstellen. 

Erstellt Sie eine Datenträgerkonfiguration.
```azurepowershell-interactive
$diskConfig = New-AzDiskConfig `
   -Location EastUS `
   -CreateOption FromImage `
   -GalleryImageReference @{Id = $sourceImgVer.Id; Lun=0}
```

Erstellen Sie den Datenträger.

```azurepowershell-interactive
New-AzDisk -Disk $diskConfig `
   -ResourceGroupName myResourceGroup `
   -DiskName myManagedDataDisk
```

## <a name="next-steps"></a>Nächste Schritte

Sie können auch mithilfe von [Azure CLI](image-version-managed-image-cli.md) oder [PowerShell](image-version-managed-image-powershell.md) eine Imageversion aus einem verwalteten Datenträger erstellen.


