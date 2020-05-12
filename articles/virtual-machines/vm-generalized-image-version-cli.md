---
title: Erstellen einer VM aus einem generalisierten Image mithilfe der Azure-Befehlszeilenschnittstelle
description: Erstellen Sie eine VM aus einem generalisierten Image mithilfe der Azure-Befehlszeilenschnittstelle.
author: cynthn
ms.service: virtual-machines
ms.subservice: imaging
ms.topic: how-to
ms.workload: infrastructure
ms.date: 05/04/2020
ms.author: cynthn
ms.openlocfilehash: 5e59872a4da0136232652008a2980601428eeab6
ms.sourcegitcommit: e0330ef620103256d39ca1426f09dd5bb39cd075
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/05/2020
ms.locfileid: "82793898"
---
# <a name="create-a-vm-from-a-generalized-image-version-using-the-cli"></a>Erstellen einer VM aus einer generalisierten Imageversion mithilfe der Befehlszeilenschnittstelle

Erstellen Sie eine VM aus einer [generalisierten Imageversion](https://docs.microsoft.com/azure/virtual-machines/linux/shared-image-galleries#generalized-and-specialized-images), die in einem Katalog mit freigegebenen Images gespeichert ist. Informationen zum Erstellen einer VM aus einem spezialisierten Image finden Sie unter [Erstellen einer VM aus einem spezialisierten Image](vm-specialized-image-version-powershell.md). 


## <a name="get-the-image-id"></a>Abrufen der Image-ID

Listen Sie die Imagedefinitionen in einem Katalog mithilfe von [az sig image-definition list](/cli/azure/sig/image-definition#az-sig-image-definition-list) auf, um den Namen und die ID der Definitionen anzuzeigen.

```azurecli-interactive 
resourceGroup=myGalleryRG
gallery=myGallery
az sig image-definition list --resource-group $resourceGroup --gallery-name $gallery --query "[].[name, id]" --output tsv
```

## <a name="create-the-vm"></a>Erstellen des virtuellen Computers

Erstellen Sie einen virtuellen Computer mit [az vm create](/cli/azure/vm#az-vm-create). Wenn Sie die neueste Version des Images verwenden möchten, legen Sie `--image` auf die ID der Imagedefinition fest. 

Ersetzen Sie bei Bedarf die Ressourcennamen in diesem Beispiel. 

```azurecli-interactive 
imgDef="/subscriptions/<subscription ID where the gallery is located>/resourceGroups/myGalleryRG/providers/Microsoft.Compute/galleries/myGallery/images/myImageDefinition"
vmResourceGroup=myResourceGroup
location=eastus
vmName=myVM
adminUsername=azureuser


az group create --name $vmResourceGroup --location $location

az vm create\
   --resource-group $vmResourceGroup \
   --name $vmName \
   --image $imgDef \
   --admin-username $adminUsername \
   --generate-ssh-keys
```

Sie können auch eine bestimmte Version verwenden, indem Sie die Imageversion mit der ID für den `--image`-Parameter verwenden. Um beispielsweise die Imageversion *1.0.0* zu verwenden, geben Sie ein: `--image "/subscriptions/<subscription ID where the gallery is located>/resourceGroups/myGalleryRG/providers/Microsoft.Compute/galleries/myGallery/images/myImageDefinition/versions/1.0.0"`.

## <a name="next-steps"></a>Nächste Schritte

[Azure Image Builder (Vorschauversion)](./linux/image-builder-overview.md) hilft beim Automatisieren der Erstellung von Imageversionen. Sie können den Dienst sogar zum Aktualisieren und [Erstellen einer neuen Imageversion aus einer vorhandenen](./linux/image-builder-gallery-update-image-version.md) verwenden. 
