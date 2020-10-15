---
title: include file
description: include file
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 04/02/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: de0cf260207747f4acb02a377819a13de8b8ba22
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "80628420"
---
[!INCLUDE [virtual-machines-disks-incremental-snapshots-description](virtual-machines-disks-incremental-snapshots-description.md)]

## <a name="regional-availability"></a>Regionale Verfügbarkeit
[!INCLUDE [virtual-machines-disks-incremental-snapshots-regions](virtual-machines-disks-incremental-snapshots-regions.md)]

### <a name="restrictions"></a>Beschränkungen

[!INCLUDE [virtual-machines-disks-incremental-snapshots-restrictions](virtual-machines-disks-incremental-snapshots-restrictions.md)]

## <a name="portal"></a>Portal


1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an, und navigieren Sie zu dem Datenträger, von dem Sie eine Momentaufnahme erstellen möchten.
1. Wählen Sie auf dem Datenträger **Momentaufnahme erstellen** aus.

    :::image type="content" source="media/virtual-machines-disks-incremental-snapshots-portal/create-snapshot-button-incremental.png" alt-text="Screenshot. Das Blatt Ihres Datenträgers mit hervorgehobener Option **+ Momentaufnahme erstellen**, die Sie auswählen müssen.":::

1. Wählen Sie die Ressourcengruppe aus, die Sie verwenden möchten, und geben Sie einen Namen ein.
1. Wählen Sie **Inkrementell** und dann **Bewerten + erstellen** aus.

    :::image type="content" source="media/virtual-machines-disks-incremental-snapshots-portal/incremental-snapshot-create-snapshot-blade.png" alt-text="Screenshot. Das Blatt Ihres Datenträgers mit hervorgehobener Option **+ Momentaufnahme erstellen**, die Sie auswählen müssen.":::

1. Klicken Sie auf **Erstellen**

    :::image type="content" source="media/virtual-machines-disks-incremental-snapshots-portal/create-incremental-snapshot-validation.png" alt-text="Screenshot. Das Blatt Ihres Datenträgers mit hervorgehobener Option **+ Momentaufnahme erstellen**, die Sie auswählen müssen.":::

## <a name="next-steps"></a>Nächste Schritte

Beispielcode mit .NET, der die differenzielle Funktionalität von inkrementellen Momentaufnahmen veranschaulicht, finden Sie unter [Kopieren von Azure Managed Disks-Sicherungen in eine andere Region mit der differenziellen Funktionalität von inkrementellen Momentaufnahmen](https://github.com/Azure-Samples/managed-disks-dotnet-backup-with-incremental-snapshots).
