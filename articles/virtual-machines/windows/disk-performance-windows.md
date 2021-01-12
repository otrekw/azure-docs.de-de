---
title: Leistung von virtuellen Computern und Datenträgern – Windows
description: Erfahren Sie mehr darüber, wie VMs und die angefügten Datenträger im Hinblick auf die Leistung unter Windows zusammenarbeiten.
author: albecker1
ms.author: albecker
ms.date: 10/12/2020
ms.topic: conceptual
ms.service: virtual-machines
ms.subservice: disks
ms.openlocfilehash: dec8b0cc33a9fffa7cac1ac9261b3c38ef5a6449
ms.sourcegitcommit: d2d1c90ec5218b93abb80b8f3ed49dcf4327f7f4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/16/2020
ms.locfileid: "97584118"
---
# <a name="virtual-machine-and-disk-performance-windows"></a>Leistung von virtuellen Computern und Datenträgern (Windows)
[!INCLUDE [VM and Disk Performance](../../../includes/virtual-machine-disk-performance.md)]

## <a name="virtual-machine-uncached-vs-cached-limits"></a>Nicht im Cache zwischengespeicherte und zwischengespeicherte Grenzwerte virtueller Computer
 Virtuelle Computer, für die sowohl Storage Premium als auch Storage Premium-Caches aktiviert sind, weisen zwei verschiedene Einschränkungen für die Speicherbandbreite auf. Betrachten wir den virtuellen Computer Standard_D8s_v3 als Beispiel. Hier ist die Dokumentation zur [Dsv3-Serie](../dv3-dsv3-series.md), zu der der Standard_D8s_v3 gehört:

[!INCLUDE [VM and Disk Performance](../../../includes/virtual-machine-disk-performance-2.md)]

Führen wir einen Benchmarktest auf dieser Kombination aus VM und Datenträger aus, die E/A-Aktivität generiert. Alles Nötige über Benchmarktests für Speicher-E/A auf Azure erfahren Sie [hier](disks-benchmarks.md). Aus dem Benchmarktool können Sie ersehen, dass die Kombination aus VM und Datenträger 22.800 IOPS erreichen kann:

[!INCLUDE [VM and Disk Performance](../../../includes/virtual-machine-disk-performance-3.md)]