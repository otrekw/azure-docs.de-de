---
title: include file
description: include file
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 04/06/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 0b185d545e129c941d5df2e8ce86ee684174b666
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "81008335"
---
Vorerst können nur Disk Ultra und SSD Premium freigegebene Datenträger ermöglichen. Unterschiedliche Datenträgergrößen haben möglicherweise verschiedene `maxShares`-Limits, die Sie beim Festlegen des `maxShares`-Werts nicht überschreiten können. Bei SSD Premium sind die Datenträgergrößen, die die Freigabe ihrer Datenträger unterstützen, P15 und größer.

Sie können für jeden Datenträger einen `maxShares`-Wert definieren, der die maximale Anzahl von Knoten darstellt, die den Datenträger gleichzeitig nutzen können. Wenn Sie z. B. einen Failovercluster mit zwei Knoten einrichten möchten, legen Sie `maxShares=2` fest. Der Maximalwert ist eine Obergrenze. Knoten können dem Cluster beitreten oder ihn verlassen (Einbindung des Datenträgers oder Aufheben der Einbindung), solange die Anzahl der Knoten kleiner als der angegebene `maxShares`-Wert ist.

> [!NOTE]
> Der `maxShares`-Wert kann nur festgelegt oder bearbeitet werden, wenn der Datenträger von allen Knoten getrennt wird.

### <a name="premium-ssd-ranges"></a>SSD Premium-Bereiche

In der folgenden Tabelle werden die zulässigen maximalen Werte für `maxShares` nach Premium-Datenträgergröße veranschaulicht:

|Datenträgergrößen  |maxShares-Limit  |
|---------|---------|
|P15, P20     |2         |
|P30, P40, P50     |5         |
|P60, P70, P80     |10         |

Die IOPS- und Bandbreitenlimits für einen Datenträger sind vom `maxShares`-Wert nicht betroffen. Beispielsweise beträgt der maximale IOPS eines P15-Datenträgers 1100, unabhängig davon, ob maxShares = 1 oder maxShares > 1 ist.

### <a name="ultra-disk-ranges"></a>Disk Ultra-Bereiche

Der minimale `maxShares`-Wert ist 1, während der maximale `maxShares`-Wert 5 beträgt. Es gibt keine Größenbeschränkungen für Disk Ultra. Disk Ultra-Instanzen beliebiger Größe können jeden Wert für `maxShares` verwenden, bis einschließlich des Maximalwerts.