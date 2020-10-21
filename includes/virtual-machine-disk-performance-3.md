---
title: Includedatei
description: Includedatei
services: virtual-machines
author: albecker1
ms.service: virtual-machines
ms.topic: include
ms.date: 10/12/2020
ms.author: albecker1
ms.custom: include file
ms.openlocfilehash: 14e74bfbcd087ccc1d8c5f2f10a8e44ed37cce84
ms.sourcegitcommit: 2c586a0fbec6968205f3dc2af20e89e01f1b74b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/14/2020
ms.locfileid: "92016545"
---
![Metrikmenü](media/vm-disk-performance/utilization-metrics-example/fio-output.jpg)

Standard_D8s_v3 kann in der Summe jedoch 28.600 IOPS erreichen, und dank der Metriken können wir herausfinden, was vor sich geht, und den Engpass bei der Speicher-E/A ermitteln. Suchen Sie zuerst die Metrikschaltfläche im Menü auf der linken Seite, und wählen Sie sie aus:

![Metrikmenü](media/vm-disk-performance/utilization-metrics-example/metrics-menu.jpg)

Sehen wir uns zunächst die Metrik **VM Cached IOPS Consumed Percentage** (Verbrauchte von der VM zwischengespeicherte IOPS in Prozent) an:

![Verbrauchte von der VM zwischengespeicherte IOPS in Prozent](media/vm-disk-performance/utilization-metrics-example/vm-cached.jpg)

Diese Metrik teilt uns mit, dass von den 16.000 IOPS, die in der VM für zwischengespeicherte IOPS zugeteilt sind, 61 % verwendet werden. Dies bedeutet, dass der Engpass bei der Speicher-E/A nicht bei den zwischengespeicherten Datenträgern liegt, da er die 100 % nicht erreicht. Sehen wir uns nun die Metrik **VM Uncached IOPS Consumed Percentage** (Verbrauchte von der VM nicht zwischengespeicherte IOPS in Prozent) an:

![Verbrauchte von der VM nicht zwischengespeicherte IOPS in Prozent](media/vm-disk-performance/utilization-metrics-example/vm-uncached.jpg)

Diese Metrik liegt bei 100 % und gibt an, dass alle 12.800 IOPS, die der VM für nicht zwischengespeicherte IOPS zugeteilt sind, verwendet werden. Eine Möglichkeit, dieses Problem zu beheben, besteht darin, zu einer größeren VM zu wechseln, die die zusätzliche E/A verarbeiten kann. Bevor wir uns dafür entscheiden, wollen wir aber noch einen Blick auf die angefügten Datenträger werfen, um herauszufinden, wie viele IOPS sie erfahren. Wir sehen uns zuerst den Betriebssystem-Datenträger in Form der Metrik **Beanspruchte Betriebssystem-Datenträger-IOPS in Prozent** an:

![Beanspruchte Betriebssystem-Datenträger-IOPS in Prozent](media/vm-disk-performance/utilization-metrics-example/os-disk.jpg)

Diese Metrik gibt an, dass von den für diesen P30-Betriebssystem-Datenträger bereitgestellten 5.000 IOPS annähernd 90 % verwendet werden. Dies bedeutet, dass für den Betriebssystem-Datenträger kein Engpass besteht. Sehen wir uns nun die an die VM angefügten Datenträger in Form der Metrik **Beanspruchte Datenträger-IOPS in Prozent** an:

![Beanspruchte Datenträger-IOPS in Prozent](media/vm-disk-performance/utilization-metrics-example/data-disks-no-splitting.jpg)

Diese Metrik teilt uns mit, dass der durchschnittlich verbrauchte Prozentsatz an IOPS über alle Datenträger bei ungefähr 42 % liegt. Dieser Prozentsatz wird auf der Grundlage der von den Datenträgern verwendeten IOPS berechnet, die nicht aus dem Hostcache bedient werden. Sehen wir uns diese Metrik genauer an, indem wir **Aufteilung** auf diese Metriken anwenden und nach dem LUN-Wert aufteilen:

![Beanspruchte Datenträger-IOPS in Prozent mit Aufteilung](media/vm-disk-performance/utilization-metrics-example/data-disks-splitting.jpg)

Diese Metrik informiert uns, dass die an LUN 3 und 2 angefügten Datenträger ungefähr 85 % ihrer bereitgestellten IOPS verbrauchen. Dieses Diagramm zeigt, wie die E/A aus dem Blickwinkel der VM- und Datenträgerarchitektur aussieht:

![Beispieldiagramm für Speicher-E/A-Metriken](media/vm-disk-performance/utilization-metrics-example/metrics-diagram.jpg)
