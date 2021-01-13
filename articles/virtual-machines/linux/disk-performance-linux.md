---
title: 'Leistung von virtuellen Computern und Datenträgern: Linux'
description: Erfahren Sie mehr darüber, wie VMs und die angefügten Datenträger im Hinblick auf die Leistung unter Linux zusammenarbeiten.
author: albecker1
ms.author: albecker
ms.date: 10/12/2020
ms.topic: conceptual
ms.service: virtual-machines
ms.subservice: disks
ms.openlocfilehash: 2c48672bcfd8c552b36e3ae0807135924669c1d9
ms.sourcegitcommit: d2d1c90ec5218b93abb80b8f3ed49dcf4327f7f4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/16/2020
ms.locfileid: "97591853"
---
# <a name="virtual-machine-and-disk-performance-linux"></a>Leistung von virtuellen Computern und Datenträgern (Linux)
[!INCLUDE [VM and Disk Performance](../../../includes/virtual-machine-disk-performance.md)]

## <a name="virtual-machine-uncached-vs-cached-limits"></a>Nicht im Cache zwischengespeicherte und zwischengespeicherte Grenzwerte virtueller Computer
Virtuelle Computer, für die sowohl Storage Premium als auch die Storage Premium-Zwischenspeicherung aktiviert sind, weisen bei der Speicherbandbreite zwei Einschränkungen auf. Nehmen wir nun den virtuellen Computer Standard_D8s_v3 als Beispiel. Hier ist die Dokumentation zur [Dsv3-Serie](../dv3-dsv3-series.md) und Standard_D8s_v3-VM:

[!INCLUDE [VM and Disk Performance](../../../includes/virtual-machine-disk-performance-2.md)]

Lassen Sie uns einen Benchmarktest mit dieser Kombination aus virtuellem Computer und Datenträger durchführen, die E/A-Aktivität erzeugt. Weitere Informationen zu Benchmarktests der Speicher-E/A in Azure finden Sie unter [Benchmarktests für Ihre Anwendung in Azure Disk Storage](disks-benchmarks.md). Im Benchmarktool können Sie ersehen, dass die Kombination aus VM und Datenträger 22.800 IOPS erreichen kann:

[!INCLUDE [VM and Disk Performance](../../../includes/virtual-machine-disk-performance-3.md)]
