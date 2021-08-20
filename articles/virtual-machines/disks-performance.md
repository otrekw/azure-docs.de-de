---
title: Leistung von virtuellen Computern und Datenträgern
description: Erfahren Sie mehr darüber, wie VMs und die angefügten Datenträger im Hinblick auf die Leistung zusammenarbeiten.
author: roygara
ms.author: rogarana
ms.date: 06/29/2021
ms.topic: conceptual
ms.service: storage
ms.subservice: disks
ms.openlocfilehash: 64b535c717d446957e06464a7b3393565a4e250e
ms.sourcegitcommit: 82d82642daa5c452a39c3b3d57cd849c06df21b0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/07/2021
ms.locfileid: "113362303"
---
# <a name="virtual-machine-and-disk-performance"></a>Leistung von virtuellen Computern und Datenträgern
[!INCLUDE [VM and Disk Performance](../../includes/virtual-machine-disk-performance.md)]

## <a name="virtual-machine-uncached-vs-cached-limits"></a>Nicht im Cache zwischengespeicherte und zwischengespeicherte Grenzwerte virtueller Computer
Virtuelle Computer, für die sowohl Storage Premium als auch die Storage Premium-Zwischenspeicherung aktiviert sind, weisen bei der Speicherbandbreite zwei Einschränkungen auf. Nehmen wir nun den virtuellen Computer Standard_D8s_v3 als Beispiel. Hier ist die Dokumentation zur [Dsv3-Serie](dv3-dsv3-series.md) und Standard_D8s_v3-VM:

[!INCLUDE [VM and Disk Performance](../../includes/virtual-machine-disk-performance-2.md)]

