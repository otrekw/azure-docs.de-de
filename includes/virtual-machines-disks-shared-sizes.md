---
title: include file
description: include file
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 02/18/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 34699ed89e79448d66343021dd624cb872d0172d
ms.sourcegitcommit: 64def2a06d4004343ec3396e7c600af6af5b12bb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/19/2020
ms.locfileid: "77471689"
---
Vorerst können nur auf SSD-Premium-Datenträgern freigegebene Datenträger aktiviert werden. Die Datenträgergrößen ab P15 unterstützen dieses Feature. Unterschiedliche Datenträgergrößen haben möglicherweise verschiedene `maxShares`-Limits, die Sie beim Festlegen des `maxShares`-Werts nicht überschreiten können.

Sie können für jeden Datenträger einen `maxShares`-Wert definieren, der die maximale Anzahl von Knoten darstellt, die den Datenträger gleichzeitig nutzen können. Wenn Sie z. B. einen Failovercluster mit zwei Knoten einrichten möchten, legen Sie `maxShares=2` fest. Der Maximalwert ist eine Obergrenze. Knoten können dem Cluster beitreten oder ihn verlassen (Einbindung des Datenträgers oder Aufheben der Einbindung), solange die Anzahl der Knoten kleiner als der angegebene `maxShares`-Wert ist.

> [!NOTE]
> Der `maxShares`-Wert kann nur festgelegt oder bearbeitet werden, wenn der Datenträger von allen Knoten getrennt wird.

In der folgenden Tabelle werden die zulässigen maximalen Werte für `maxShares` nach Datenträgergröße veranschaulicht:

|Datenträgergrößen  |maxShares-Limit  |
|---------|---------|
|P15, P20     |2         |
|P30, P40, P50     |5         |
|P60, P70, P80     |10         |

Die IOPS- und Bandbreitenlimits für einen Datenträger sind vom `maxShares`-Wert nicht betroffen. Beispielsweise beträgt der maximale IOPS eines P15-Datenträgers 1.100, unabhängig davon, ob maxShares = 1 oder maxShares > 1 ist.