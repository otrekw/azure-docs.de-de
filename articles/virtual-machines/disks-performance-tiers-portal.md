---
title: Ändern der Leistung von verwalteten Azure-Datenträgern über das Azure-Portal
description: Erfahren Sie, wie Sie über das Azure-Portal Leistungsstufen für neue und vorhandene verwaltete Datenträger ändern.
author: roygara
ms.service: virtual-machines
ms.topic: how-to
ms.date: 01/05/2021
ms.author: rogarana
ms.subservice: disks
ms.custom: references_regions
ms.openlocfilehash: 625fb1e3dd0b433da6b60f995aa6b380c23ec9ce
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "97901022"
---
# <a name="change-your-performance-tier-using-the-azure-portal"></a>Ändern Ihrer Leistungsstufe über das Azure-Portal

[!INCLUDE [virtual-machines-disks-performance-tiers-intro](../../includes/virtual-machines-disks-performance-tiers-intro.md)]

## <a name="restrictions"></a>Beschränkungen

[!INCLUDE [virtual-machines-disks-performance-tiers-restrictions](../../includes/virtual-machines-disks-performance-tiers-restrictions.md)]

## <a name="getting-started"></a>Erste Schritte

### <a name="new-disks"></a>Neue Datenträger

Die folgenden Schritte zeigen, wie Sie die Leistungsstufe Ihres Datenträgers ändern können, wenn Sie den Datenträger zum ersten Mal erstellen:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.
1. Navigieren Sie zu dem virtuellen Computer, für den Sie einen neuen Datenträger erstellen möchten.
1. Wenn Sie den neuen Datenträger auswählen, wählen Sie zuerst die Größe des benötigten Datenträgers aus.
1. Nachdem Sie eine Größe ausgewählt haben, wählen Sie eine andere Leistungsstufe aus, um die Leistung zu ändern.
1. Wählen Sie **OK** aus, um den Datenträger zu erstellen.

:::image type="content" source="media/disks-performance-tiers-portal/new-disk-change-performance-tier.png" alt-text="Der Screenshot für das Blatt zur Datenträgererstellung, wobei ein Datenträger und die Dropdownliste der Leistungsstufe hervorgehoben ist." lightbox="media/disks-performance-tiers-portal/performance-tier-settings.png":::


## <a name="existing-disks"></a>Vorhandene Datenträger

In den folgenden Schritten wird beschrieben, wie Sie die Leistungsstufe eines vorhandenen Datenträgers ändern:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.
1. Navigieren Sie zu dem virtuellen Computer, der den Datenträger enthält, den Sie ändern möchten.
1. Heben Sie entweder die Zuordnung des virtuellen Computers auf, oder trennen Sie den Datenträger.
1. Auswählen des Datenträgers
1. Wählen Sie die **Größe und Leistung** aus.
1. Wählen Sie in der Dropdownliste **Leistungsstufen** eine andere Stufe als die aktuelle Leistungsstufe des Datenträgers aus.
1. Wählen Sie **Größe ändern** aus.

:::image type="content" source="media/disks-performance-tiers-portal/change-tier-existing-disk.png" alt-text="Der Screenshot für das Blatt zu Größe und Leistung, wobei die Leistungsstufe hervorgehoben ist." lightbox="media/disks-performance-tiers-portal/performance-tier-settings.png":::

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie die Größe eines Datenträgers ändern müssen, um die höheren Leistungsstufen zu nutzen, lesen Sie die folgenden Artikel:

- [Erweitern von virtuellen Festplatten auf virtuellen Linux-Computern mit der Azure-CLI](linux/expand-disks.md)
- [Erweitern eines verwalteten Datenträgers, der an einen virtuellen Windows-Computer angefügt ist](windows/expand-os-disk.md)
