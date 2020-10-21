---
title: Erstellen eines verwalteten Datenträgers aus einer Imageversion
description: Erstellen eines verwalteten Datenträgers aus einer Imageversion in einer Shared Image Gallery.
author: cynthn
ms.service: virtual-machines
ms.subservice: imaging
ms.topic: how-to
ms.workload: infrastructure
ms.date: 10/06/2020
ms.author: cynthn
ms.reviewer: olayemio
ms.openlocfilehash: bf4a1feb91a1ac4b0bca0d6afdbac41a8be3aa4f
ms.sourcegitcommit: 2e72661f4853cd42bb4f0b2ded4271b22dc10a52
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/14/2020
ms.locfileid: "92049496"
---
# <a name="create-a-managed-disk-from-an-image-version"></a>Erstellen eines verwalteten Datenträgers aus einer Imageversion

Falls es erforderlich ist, können Sie einen verwalteten Datenträger aus einer in einer Shared Image Gallery gespeicherten Imageversion erstellen.


## <a name="cli"></a>Befehlszeilenschnittstelle (CLI)

Legen Sie die `source`-Variable auf die ID der Imageversion fest, und verwenden Sie anschließend [az disk create](/cli/azure/disk.md#az_disk_create), um den verwalteten Datenträger zu erstellen. 


Eine Liste der Imageversionen können Sie mithilfe von [az sig image-version list](/cli/azure/sig/image-version.md#az_sig_image_version_list) anzeigen. In diesem Beispiel suchen wir nach allen Imageversionen, die Teil der Imagedefinition *myImageDefinition* im Imagekatalog *myGallery* sind.

```azurecli-interactive
az sig image-version list \
   --resource-group myResourceGroup\
   --gallery-name myGallery \
   --gallery-image-definition myImageDefinition \
   -o table
```


In diesem Beispiel erstellen wir einen verwalteten Datenträger mit dem Namen *myManagedDisk* in der Region *EastUS* und einer Ressourcengruppe mit dem Namen *myResourceGroup*. 

```azurecli-interactive
source="/subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.Compute/galleries/<galleryName>/images/<galleryImageDefinition>/versions/<imageVersion>"

az disk create --resource-group myResourceGroup --location EastUS --name myManagedDisk --gallery-image-reference $source 
```

Wenn es sich bei dem Datenträger um einen für Daten bestimmten Datenträger handelt, fügen Sie `--gallery-image-reference-lun` hinzu, um die LUN anzugeben.

## <a name="powershell"></a>PowerShell

Mit [Get-AzResource](/powershell/module/az.resources/get-azresource) können Sie alle Imageversionen auflisten. 

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

Richten Sie einige Variablen für die Datenträgerinformationen ein.

```azurepowershell-interactive
$location = "East US"
$resourceGroup = "myResourceGroup"
$diskName = "myDisk"
```

Erstellen Sie eine Datenträgerkonfiguration und anschließend mithilfe der Versions-ID des Quellimages den Datenträger. Für `-GalleryImageReference` ist die LUN nur erforderlich, wenn es sich bei der Quelle um einen für Daten bestimmten Datenträger handelt.

```azurepowershell-interactive
$diskConfig = New-AzDiskConfig `
   -Location $location `
   -CreateOption FromImage `
   -GalleryImageReference @{Id = $sourceImgVer.Id; Lun=1}
```

Erstellen Sie den Datenträger.

```azurepowershell-interactive
New-AzDisk -Disk $diskConfig `
   -ResourceGroupName $resourceGroup `
   -DiskName $diskName
```

## <a name="next-steps"></a>Nächste Schritte

Sie können auch mithilfe von [Azure CLI](image-version-managed-image-cli.md) oder [PowerShell](image-version-managed-image-powershell.md) eine Imageversion aus einem verwalteten Datenträger erstellen.


