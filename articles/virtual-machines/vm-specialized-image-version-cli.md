---
title: Erstellen einer VM aus einer spezialisierten Imageversion mithilfe der Azure-Befehlszeilenschnittstelle
description: Erstellen Sie eine VM mit einer spezialisierten Imageversion in einem Katalog mit freigegebenen Images mithilfe der Azure-Befehlszeilenschnittstelle.
author: cynthn
ms.service: virtual-machines
ms.subservice: imaging
ms.workload: infrastructure-services
ms.topic: how-to
ms.date: 04/23/2020
ms.author: cynthn
ms.reviewer: akjosh
ms.openlocfilehash: a22c0941a405dd2fcf37403e3a12113bc51cb774
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2020
ms.locfileid: "87005802"
---
# <a name="create-a-vm-using-a-specialized-image-version-with-the-azure-cli"></a>Erstellen einer VM mit einer spezialisierten Imageversion mithilfe der Azure-Befehlszeilenschnittstelle

Erstellen Sie eine VM aus einer [spezialisierten Imageversion](./linux/shared-image-galleries.md#generalized-and-specialized-images), die in einem Katalog mit freigegebenen Images gespeichert ist. Wenn Sie eine VM mit einer generalisierten Imageversion erstellen möchten, finden Sie weitere Informationen unter [Erstellen einer VM aus einer generalisierten Imageversion](vm-generalized-image-version-cli.md).

Ersetzen Sie bei Bedarf die Ressourcennamen in diesem Beispiel. 

Listen Sie die Imagedefinitionen in einem Katalog mithilfe von [az sig image-definition list](/cli/azure/sig/image-definition#az-sig-image-definition-list) auf, um den Namen und die ID der Definitionen anzuzeigen.

```azurecli-interactive 
resourceGroup=myGalleryRG
gallery=myGallery
az sig image-definition list \
   --resource-group $resourceGroup \
   --gallery-name $gallery \
   --query "[].[name, id]" \
   --output tsv
```

Erstellen Sie den virtuellen Computer mithilfe von [az vm create](/cli/azure/vm#az-vm-create) und unter Verwendung des Parameters „--specialized“, um anzugeben, dass es sich um ein spezialisiertes Image handelt. 

Verwenden Sie die Imagedefinitions-ID für `--image`, um die VM auf der Grundlage der neuesten verfügbaren Imageversion zu erstellen. Sie können die VM auch auf der Grundlage einer bestimmten Version erstellen, indem Sie die Imageversions-ID für `--image` angeben. 

In diesem Beispiel erstellen Sie eine VM aus der aktuellen Version des Images *myImageDefinition*.

```azurecli
az group create --name myResourceGroup --location eastus
az vm create --resource-group myResourceGroup \
    --name myVM \
    --image "/subscriptions/<Subscription ID>/resourceGroups/myGalleryRG/providers/Microsoft.Compute/galleries/myGallery/images/myImageDefinition" \
    --specialized
```


## <a name="next-steps"></a>Nächste Schritte
[Azure Image Builder (Vorschauversion)](./linux/image-builder-overview.md) hilft beim Automatisieren der Erstellung von Imageversionen. Sie können den Dienst sogar zum Aktualisieren und [Erstellen einer neuen Imageversion aus einer vorhandenen](./linux/image-builder-gallery-update-image-version.md) verwenden. 

Sie können auch mithilfe von Vorlagen eine Ressource im Katalog für freigegebene Images erstellen. Es stehen mehrere Azure-Schnellstartvorlagen zur Verfügung: 

- [Erstellen eines Katalogs mit freigegebenen Images](https://azure.microsoft.com/resources/templates/101-sig-create/)
- [Erstellen einer Imagedefinition in einem Katalog mit freigegebenen Images](https://azure.microsoft.com/resources/templates/101-sig-image-definition-create/)
- [Erstellen einer Imageversion in einem Katalog mit freigegebenen Images](https://azure.microsoft.com/resources/templates/101-sig-image-version-create/)
- [Erstellen eines virtuellen Computers aus einer Imageversion](https://azure.microsoft.com/resources/templates/101-vm-from-sig/)
