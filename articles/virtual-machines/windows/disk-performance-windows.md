---
title: Leistung von virtuellen Computern und Datenträgern
description: Erfahren Sie mehr darüber, wie VMs und die angefügten Datenträger im Hinblick auf die Leistung zusammenarbeiten.
author: albecker1
ms.author: albecker
ms.date: 09/25/2020
ms.topic: conceptual
ms.service: virtual-machines
ms.subservice: disks
ms.openlocfilehash: 18c07fceb65623c286b31398314e2b6f124955a6
ms.sourcegitcommit: f5580dd1d1799de15646e195f0120b9f9255617b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/29/2020
ms.locfileid: "91540006"
---
# <a name="virtual-machine-and-disk-performance"></a>Leistung von virtuellen Computern und Datenträgern
[!INCLUDE [VM and Disk Performance](../../../includes/virtual-machine-disk-performance.md)]

## <a name="virtual-machine-uncached-vs-cached-limits"></a>Nicht im Cache zwischengespeicherte und zwischengespeicherte Grenzwerte virtueller Computer
 Virtuelle Computer, für die sowohl Storage Premium als auch Storage Premium-Caches aktiviert sind, weisen zwei verschiedene Einschränkungen für die Speicherbandbreite auf. Betrachten wir den virtuellen Computer Standard_D8s_v3 als Beispiel. Hier ist die Dokumentation zur [Dsv3-Serie](../dv3-dsv3-series.md), zu der der Standard_D8s_v3 gehört:

[!INCLUDE [VM and Disk Performance](../../../includes/virtual-machine-disk-performance-2.md)]
