---
title: Leistung von virtuellen Computern und Datenträgern
description: Erfahren Sie mehr darüber, wie VMs und die angefügten Datenträger im Hinblick auf die Leistung zusammenarbeiten.
author: albecker1
ms.author: albecker
ms.date: 10/12/2020
ms.topic: conceptual
ms.service: virtual-machines
ms.subservice: disks
ms.openlocfilehash: d3a89e7733cc033792056b8de5232232b8327025
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "100580372"
---
# <a name="virtual-machine-and-disk-performance"></a>Leistung von virtuellen Computern und Datenträgern
[!INCLUDE [VM and Disk Performance](../../includes/virtual-machine-disk-performance.md)]

## <a name="virtual-machine-uncached-vs-cached-limits"></a>Nicht im Cache zwischengespeicherte und zwischengespeicherte Grenzwerte virtueller Computer
Virtuelle Computer, für die sowohl Storage Premium als auch die Storage Premium-Zwischenspeicherung aktiviert sind, weisen bei der Speicherbandbreite zwei Einschränkungen auf. Nehmen wir nun den virtuellen Computer Standard_D8s_v3 als Beispiel. Hier ist die Dokumentation zur [Dsv3-Serie](dv3-dsv3-series.md) und Standard_D8s_v3-VM:

[!INCLUDE [VM and Disk Performance](../../includes/virtual-machine-disk-performance-2.md)]

