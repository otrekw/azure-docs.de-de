---
title: Größen von Azure-VMs – Arbeitsspeicher | Microsoft-Dokumentation
description: Liste der verschiedenen verfügbaren arbeitsspeicheroptimierten Größen für virtuelle Computer in Azure Dieser Artikel listet Informationen zur Anzahl von vCPUs, Datenträgern und Netzwerkschnittstellenkarten sowie zum Speicherdurchsatz und zur Netzwerkbandbreite für Größen dieser Serie auf.
services: virtual-machines
documentationcenter: ''
author: jonbeck7
manager: gwallace
editor: ''
tags: azure-resource-manager,azure-service-management
keywords: VM-Isolation,isolierte VM,Isolation,isoliert
ms.assetid: ''
ms.service: virtual-machines
ms.devlang: na
ms.topic: article
ms.workload: infrastructure-services
ms.date: 02/03/2020
ms.author: jonbeck
ms.openlocfilehash: 26a44cdf3e57508cebb070669a8c1f473a59df8a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "77492543"
---
# <a name="memory-optimized-virtual-machine-sizes"></a>Arbeitsspeicheroptimierte Größen virtueller Computer

Arbeitsspeicheroptimierte VM-Größen bieten ein hohes Arbeitsspeicher-zu-CPU-Verhältnis und eignen sich hervorragend für relationale Datenbankserver, mittelgroße bis große Caches und In-Memory-Analysen. Dieser Artikel enthält Informationen zur Anzahl von vCPUs, Datenträgern und NICs sowie zum Speicherdurchsatz und zur Netzwerkbandbreite der einzelnen Größen in dieser Gruppe.

- Die [Serien Dv2 und DSv2](dv2-dsv2-series-memory.md), Nachfolger der ursprünglichen D-Serie, weisen eine leistungsfähigere CPU auf. Die Dv2-Serie ist ca. 35 % schneller als die D-Serie. Sie wird auf dem Intel® Xeon®-Prozessor 8171M mit 2,1 GHz (Skylake), dem Intel® Xeon®-Prozessor E5-2673 v4 mit 2,3 GHz (Broadwell) oder dem Intel® Xeon®-Prozessor E5-2673 v3 mit 2,4 GHz (Haswell) und mit Intel Turbo Boost Technology 2.0 ausgeführt. Der Dv2-Serie hat die gleichen Arbeitsspeicher- und Datenträgerkonfigurationen wie die D-Serie.

    Die Serien Dv2 und DSv2 eignen sich ideal für Clientanwendungen, die schnellere vCPUs und eine höhere Leistung des temporären Speichers erfordern oder einen höheren Arbeitsspeicherbedarf haben. Sie bieten eine leistungsfähige Kombination für viele Anwendungen für den Unternehmenseinsatz.

- Die [Serien Eav4 und Easv4](eav4-easv4-series.md) verwenden den AMD-Prozessor EPYC<sup>TM</sup> 7452 mit 2,35 GHz in einer Multithreadkonfiguration mit bis zu 256 MB L3-Cache, sodass bei der Ausführung der meisten arbeitsspeicheroptimierten Workloads mehr Optionen zur Verfügung stehen. Die Eav4-Serie und die Easv4-Serie verfügen über die gleichen Arbeitsspeicher- und Datenträgerkonfigurationen wie die Ev3- und die Esv3-Serie.

- Die [Serien Ev3 und Esv3](ev3-esv3-series.md) bieten den Intel® Xeon®-Prozessor 8171M mit 2,1 GHz (Skylake) oder den Intel® Xeon®-Prozessor E5-2673 v4 mit 2,3 GHz (Broadwell) in einer Hyperthreadkonfiguration und somit ein besseres Preis-Leistungs-Verhältnis für die meisten universellen Workloads und besseren Einklang von Ev3 mit den universellen VMs der meisten anderen Clouds. Der Speicher wurde erweitert (von 7 GiB/vCPU auf 8 GiB/vCPU), während die Datenträger- und Netzwerkgrenzwerte pro Kern angepasst wurden, um auf den Übergang zum Hyperthreading vorbereitet zu sein. Die Ev3-Serie ist der Nachfolger für die VMs mit großen Arbeitsspeichergrößen der D/Dv2-Familien.

- Die [M-Serie](m-series.md) verfügt über eine hohe vCPU-Anzahl (bis zu 128 vCPUs) und eine große Menge an Arbeitsspeicher (bis zu 3,8 TiB). Dies ist auch ideal für extrem große Datenbanken oder andere Anwendungen, für die eine hohe vCPU-Anzahl und große Mengen an Arbeitsspeicher benötigt werden.

- Die [Mv2-Serie](mv2-series.md) bietet die höchste vCPU-Anzahl (bis zu 416 vCPUs) und den größten Arbeitsspeicher (bis zu 8,19 TiB) für die virtuellen Computer in der Cloud. Dies ist ideal für extrem große Datenbanken oder andere Anwendungen, für die eine hohe vCPU-Anzahl und große Mengen an Arbeitsspeicher benötigt werden.

Azure Compute bietet VM-Größen, die für einen bestimmten Hardwaretyp isoliert und für einen einzelnen Kunden bestimmt sind. Diese VM-Größen eignen sich am besten für Workloads, die ein hohes Maß an Isolation von anderen Kunden erfordern, wenn es um Workloads mit Elementen wie Konformität und gesetzlichen Anforderungen geht. Kunden können auch die Ressourcen dieser isolierten virtuellen Computer weiter unterteilen, indem sie die [Azure-Unterstützung für geschachtelte virtuelle Computer](https://azure.microsoft.com/blog/nested-virtualization-in-azure/) verwenden. Auf den Seiten für die VM-Familien finden Sie Optionen für Ihre isolierten VMs.

## <a name="other-sizes"></a>Andere Größen

- [Allgemeiner Zweck](sizes-general.md)
- [Computeoptimiert](sizes-compute.md)
- [Speicheroptimiert](sizes-storage.md)
- [GPU-optimiert](sizes-gpu.md)
- [High Performance Computing](sizes-hpc.md)
- [Vorherige Generationen](sizes-previous-gen.md)

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen dazu, wie Sie mit [Azure-Computeeinheiten (ACU)](acu.md) die Computeleistung von Azure-SKUs vergleichen können.