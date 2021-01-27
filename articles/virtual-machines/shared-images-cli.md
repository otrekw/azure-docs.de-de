---
title: Erstellen von Katalogen mit geteilten Images mithilfe der Azure CLI
description: In diesem Artikel erfahren Sie, wie Sie die Azure CLI zum Erstellen eines geteilten Images einer VM in Azure verwenden.
author: cynthn
ms.service: virtual-machines
ms.subservice: imaging
ms.topic: how-to
ms.workload: infrastructure
ms.date: 05/04/2020
ms.author: cynthn
ms.reviewer: akjosh
ms.custom: devx-track-azurecli
ms.openlocfilehash: ee7f7b524225845dc68100ee8ec9292eef111232
ms.sourcegitcommit: aaa65bd769eb2e234e42cfb07d7d459a2cc273ab
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/27/2021
ms.locfileid: "98882347"
---
# <a name="create-a-shared-image-gallery-with-the-azure-cli"></a>Erstellen eines Katalogs mit freigegebenen Images mit der Azure-Befehlszeilenschnittstelle

Der [Katalog mit freigegebenen Images](./shared-image-galleries.md) vereinfacht das Freigeben benutzerdefinierter Images in Ihrer Organisation. Benutzerdefinierte Images sind wie Marketplace-Images, Sie erstellen sie jedoch selbst. Benutzerdefinierte Images können zum Starten von Konfigurationen verwendet werden, z.B. zum Vorabladen von Anwendungen, Anwendungskonfigurationen und anderen Betriebssystemkonfigurationen. 

Shared Image Gallery ermöglicht Ihnen die Freigabe Ihrer benutzerdefinierten VM-Images für andere Benutzer. Wählen Sie aus, welche Images Sie teilen möchten, in welchen Regionen Sie sie verfügbar machen möchten, und mit wem Sie sie teilen möchten. 

[!INCLUDE [virtual-machines-common-shared-images-cli](../../includes/virtual-machines-common-shared-images-cli.md)]


## <a name="next-steps"></a>Nächste Schritte

Erstellen Sie eine Imageversion aus einer [VM](image-version-vm-cli.md) oder einem [verwalteten Image](image-version-managed-image-cli.md) über die Azure-Befehlszeilenschnittstelle.

Weitere Informationen zu Katalogen mit geteilten Images finden Sie in der [Übersicht](./shared-image-galleries.md). Sollten Probleme auftreten, sehen Sie unter [Problembehandlung für Kataloge mit freigegebenen Images](troubleshooting-shared-images.md) nach.