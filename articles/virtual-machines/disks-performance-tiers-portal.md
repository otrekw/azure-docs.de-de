---
title: Ändern der Leistung von verwalteten Azure-Datenträgern über das Azure-Portal
description: Erfahren Sie, wie Sie über das Azure-Portal Leistungsstufen für neue und vorhandene verwaltete Datenträger ändern.
author: roygara
ms.service: storage
ms.topic: how-to
ms.date: 06/29/2021
ms.author: rogarana
ms.subservice: disks
ms.custom: references_regions
ms.openlocfilehash: 37069bb17e0ce6a104ae3c1b79714da160737fe8
ms.sourcegitcommit: 82d82642daa5c452a39c3b3d57cd849c06df21b0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/07/2021
ms.locfileid: "113356273"
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

### <a name="change-the-performance-tier-of-a-disk-without-downtime-preview"></a>Ändern der Leistungsstufe eines Datenträgers ohne Downtime (Vorschau)

Sie können Ihre Leistungsstufe auch ohne Downtime ändern, damit Sie Ihre VM nicht freigeben oder Ihren Datenträger trennen müssen, um die Stufe zu ändern.

### <a name="prerequisites"></a>Voraussetzungen

Ihr Datenträger muss die Anforderungen erfüllen, die im Abschnitt [Leistungsstufe ohne Downtime ändern (Vorschau)](#change-performance-tier-without-downtime-preview) beschrieben sind. Wenn dies nicht der Fall ist, kommt es beim Ändern der Leistungsstufe zur Downtime.

Sie müssen das Feature für Ihr Abonnement aktivieren, bevor Sie die Leistungsstufe eines Datenträgers ohne Downtime ändern können. Führen Sie die folgenden Schritte aus, um das Feature für Ihr Abonnement zu aktivieren:

1.  Führen Sie den folgenden Befehl aus, um das Feature für Ihr Abonnement zu registrieren.

    ```azurecli
    az feature register --namespace Microsoft.Compute --name LiveTierChange
    ```
 
1.  Überprüfen Sie mithilfe des folgenden Befehls, ob der Registrierungsstatus **Registriert** lautet (dies kann einige Minuten dauern), bevor Sie das Feature ausprobieren.

    ```azurecli
    az feature show --namespace Microsoft.Compute --name LiveTierChange
    ```

### <a name="change-performance-tier"></a>Ändern der Leistungsstufe

Nachdem das Feature registriert wurde, können Sie die Leistungsstufen des jeweiligen Datenträgers ohne Downtime ändern.

1. Melden Sie sich über den folgenden Link beim Azure-Portal an: [https://aka.ms/diskPerfTiersPreview](https://aka.ms/diskPerfTiersPreview).
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
