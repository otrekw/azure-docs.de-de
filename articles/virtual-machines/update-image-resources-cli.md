---
title: Auflisten, Aktualisieren und Löschen von Imageressourcen mithilfe der Azure-Befehlszeilenschnittstelle
description: Mithilfe der Azure-Befehlszeilenschnittstelle können Sie Imageressourcen in Ihrer Shared Image Gallery-Instanz auflisten, aktualisieren und löschen.
author: cynthn
ms.service: virtual-machines
ms.subservice: shared-image-gallery
ms.workload: infrastructure-services
ms.topic: how-to
ms.date: 05/04/2020
ms.author: cynthn
ms.reviewer: akjosh
ms.custom: devx-track-azurecli
ms.openlocfilehash: 3554eccf28c5abbe9741af5b9718fb83710dab38
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "102553628"
---
# <a name="list-update-and-delete-image-resources"></a>Auflisten, Aktualisieren und Löschen von Imageressourcen 

Sie können Ihre Shared Image Gallery-Ressourcen mithilfe der Azure-Befehlszeilenschnittstelle verwalten.

[!INCLUDE [virtual-machines-common-gallery-list-cli.md](../../includes/virtual-machines-common-gallery-list-cli.md)]

[!INCLUDE [virtual-machines-common-shared-images-update-delete-cli](../../includes/virtual-machines-common-shared-images-update-delete-cli.md)]

## <a name="next-steps"></a>Nächste Schritte

[Azure Image Builder (Vorschauversion)](./image-builder-overview.md) hilft beim Automatisieren der Erstellung von Imageversionen. Sie können den Dienst sogar zum Aktualisieren und [Erstellen einer neuen Imageversion aus einer vorhandenen](./linux/image-builder-gallery-update-image-version.md) verwenden.