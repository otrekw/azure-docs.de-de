---
title: include file
description: include file
services: virtual-machines
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 04/28/2020
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: fc60d1266f4aad4c337611f0fa32fe8293744559
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "82792274"
---
## <a name="list-information"></a>Auflisten von Informationen

Listet alle Kataloge nach Namen auf

```azurepowershell-interactive
$galleries = Get-AzResource -ResourceType Microsoft.Compute/galleries
$galleries.Name
```

Listet alle Imagedefinitionen nach Namen auf

```azurepowershell-interactive
$imageDefinitions = Get-AzResource -ResourceType Microsoft.Compute/galleries/images
$imageDefinitions.Name
```

Listet alle Imageversionen nach Namen auf

```azurepowershell-interactive
$imageVersions = Get-AzResource -ResourceType Microsoft.Compute/galleries/images/versions
$imageVersions.Name
```

Löscht eine Imageversion Dieses Beispiel löscht die Imageversion *1.0.0*.

```azurepowershell-interactive
Remove-AzGalleryImageVersion `
   -GalleryImageDefinitionName myImageDefinition `
   -GalleryName myGallery `
   -Name 1.0.0 `
   -ResourceGroupName myGalleryRG
```





