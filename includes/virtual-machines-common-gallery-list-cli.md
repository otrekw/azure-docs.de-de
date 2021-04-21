---
title: include file
description: include file
services: virtual-machines
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 01/28/2020
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: 83d70a8d4c5806120ddb4ea776a8f4a6f7e63857
ms.sourcegitcommit: 4b0e424f5aa8a11daf0eec32456854542a2f5df0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/20/2021
ms.locfileid: "107800002"
---
## <a name="list-information"></a>Auflisten von Informationen

Den Speicherort, Status und weitere Informationen zu den verfügbaren Abbildkatalogen können Sit mit [az sig list](/cli/azure/sig#az_sig_list) abrufen.

```azurecli-interactive 
az sig list -o table
```

Die Abbilddefinitionen in einem Katalog, einschließlich der Informationen zum Betriebssystemtyp und -status können Sie mit [az sig image-definition list](/cli/azure/sig/image-definition#az_sig_image_definition_list) auflisten.

```azurecli-interactive 
az sig image-definition list --resource-group myGalleryRG --gallery-name myGallery -o table
```

Die Versionen geteilter Abbilder in einem Katalog können Sie mit [az sig image-version list](/cli/azure/sig/image-version#az_sig_image_version_list) auflisten.

```azurecli-interactive
az sig image-version list --resource-group myGalleryRG --gallery-name myGallery --gallery-image-definition myImageDefinition -o table
```

Die ID einer Abbildversion können Sie mit [az sig image-version show](/cli/azure/sig/image-version#az_sig_image_version_show) abrufen.

```azurecli-interactive
az sig image-version show \
   --resource-group myGalleryRG \
   --gallery-name myGallery \
   --gallery-image-definition myImageDefinition \
   --gallery-image-version 1.0.0 \
   --query "id"
```
