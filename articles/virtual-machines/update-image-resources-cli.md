---
title: Auflisten, Aktualisieren und Löschen von Imageressourcen mithilfe der Azure-Befehlszeilenschnittstelle
description: Mithilfe der Azure-Befehlszeilenschnittstelle können Sie Imageressourcen in Ihrer Shared Image Gallery-Instanz auflisten, aktualisieren und löschen.
author: cynthn
ms.service: virtual-machines
ms.subservice: imaging
ms.workload: infrastructure-services
ms.topic: how-to
ms.date: 05/04/2020
ms.author: cynthn
ms.reviewer: akjosh
ms.custom: devx-track-azurecli
ms.openlocfilehash: 6099afc82e76ed28e8557ac0eee3e64cb292a715
ms.sourcegitcommit: aaa65bd769eb2e234e42cfb07d7d459a2cc273ab
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/27/2021
ms.locfileid: "98882007"
---
# <a name="list-update-and-delete-image-resources"></a>Auflisten, Aktualisieren und Löschen von Imageressourcen 

Sie können Ihre Shared Image Gallery-Ressourcen mithilfe der Azure-Befehlszeilenschnittstelle verwalten.

[!INCLUDE [virtual-machines-common-gallery-list-cli.md](../../includes/virtual-machines-common-gallery-list-cli.md)]

[!INCLUDE [virtual-machines-common-shared-images-update-delete-cli](../../includes/virtual-machines-common-shared-images-update-delete-cli.md)]

## <a name="next-steps"></a>Nächste Schritte

[Azure Image Builder (Vorschauversion)](./image-builder-overview.md) hilft beim Automatisieren der Erstellung von Imageversionen. Sie können den Dienst sogar zum Aktualisieren und [Erstellen einer neuen Imageversion aus einer vorhandenen](./linux/image-builder-gallery-update-image-version.md) verwenden.