---
title: Größen von virtuellen Azure-Computern – allgemeiner Zweck | Microsoft-Dokumentation
description: Auflistung der verschiedenen verfügbaren allgemeinen Größen für virtuelle Computer in Azure. Dieser Artikel listet Informationen zur Anzahl von vCPUs, Datenträgern und Netzwerkschnittstellenkarten sowie zum Speicherdurchsatz und zur Netzwerkbandbreite für Größen dieser Serie auf.
services: virtual-machines
documentationcenter: ''
author: jonbeck7
manager: gwallace
editor: ''
tags: azure-resource-manager,azure-service-management
ms.assetid: ''
ms.service: virtual-machines
ms.devlang: na
ms.topic: article
ms.workload: infrastructure-services
ms.date: 02/03/2020
ms.author: jonbeck
ms.openlocfilehash: 674197bd4d1562d8492a2605d8929572d450af90
ms.sourcegitcommit: 98a5a6765da081e7f294d3cb19c1357d10ca333f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/20/2020
ms.locfileid: "77492559"
---
# <a name="general-purpose-virtual-machine-sizes"></a>Universelle VM-Größen

Universelle VM-Größen zeichnen sich durch ein ausgewogenes Verhältnis zwischen CPU und Arbeitsspeicher aus. Ideal für Tests und Entwicklung, kleine bis mittlere Datenbanken sowie Webserver mit geringer bis mittlerer Auslastung. Dieser Artikel enthält Informationen zu den Angeboten für allgemeines Computing.

- Die virtuellen Computer der [Av2-Reihe](av2-series.md) können auf vielen verschiedenen Hardwaretypen und Prozessoren bereitgestellt werden. Die Konfigurationen für CPU-Leistung und Arbeitsspeicher bei virtuellen Computern der A-Serie eignen sich am besten für Workloads wie Entwicklung und Tests. Die Größe ist basierend auf der Hardware gedrosselt, um eine konsistente Prozessorleistung für die ausgeführte Instanz zu ermöglichen – unabhängig von der Hardware, die für die Bereitstellung gewählt wird. Fragen Sie die virtuelle Hardware über die virtuelle Maschine ab, um die physische Hardware zu ermitteln, auf der diese Größe bereitgestellt wird. Mögliche Anwendungsfälle: Entwicklungs- und Testserver, Webserver mit geringem Datenverkehr, kleine bis mittelgroße Datenbanken, Proof of Concept und Coderepositorys.

- [Virtuelle Burst-fähige Computer der B-Serie](sizes-b-series-burstable.md) sind ideal für Workloads geeignet, die nicht kontinuierlich die volle Leistung der CPU benötigen. Hierzu zählen beispielsweise Webserver, kleine Datenbanken sowie Entwicklungs- und Testumgebungen. Diese Workloads haben in der Regel kurzfristige Leistungsanforderungen. Mit der B-Serie können diese Kunden eine VM-Größe mit einer preisgünstigen Grundleistung erwerben. Für die VM-Instanz können dann Gutschriften erlangt werden, wenn für die VM weniger als die Grundleistung genutzt wird. Wenn für den virtuellen Computer Guthaben gebildet wurde, kann Leistung genutzt werden, die über die Grundleistung der VM hinausgeht. Dabei kann die CPU zu 100 Prozent verwendet werden, wenn Ihre Anwendung eine solche Leistung benötigt.

- Die [Dav4-Serie und die Dasv4-Serie](dav4-dasv4-series.md) sind neue Größen, bei denen der AMD-Prozessor EPYC<sup>TM</sup> 7452 mit 2,35 GHz in einer Multithreadkonfiguration mit bis zu 256 MB L3-Cache verwendet wird. Hierbei werden 8 GB des L3-Caches jeweils acht Kernen zur Verfügung gestellt, was Kunden bei der Ausführung ihrer universellen Workloads mehr Optionen gibt. Die Dav4-Serie und die Dasv4-Serie verfügen über die gleichen Arbeitsspeicher- und Datenträgerkonfigurationen wie die D- und die Dsv3-Serie.

- Die [DC-Serie](dc-series.md) ist eine Familie virtueller Computer in Azure, die zum Schutz der Vertraulichkeit und der Integrität Ihrer Daten und Ihres Codes beitragen kann, wenn diese in der öffentlichen Cloud verarbeitet werden. Diese Computer werden von der neuesten Generation des Intel® Xeon®-Prozessors E-2176G mit 3,7 GHz und SGX-Technologie unterstützt. Mit der Intel Turbo Boost Technology können diese Computer bis zu 4,7 GHz erreichen. Instanzen der DC-Serie ermöglichen es Kunden, Secure Enclave-basierte Anwendungen zu erstellen, um ihren Code und Daten zu schützen, während sie verwendet werden.

- Virtuelle Computer der [Dv2-Serie und der Dsv2-Serie](dv2-dsv2-series.md), eine Nachfolgerin der ursprünglichen D-Serie, sind mit leistungsfähigeren CPUs und einer optimalen CPU-zu-Arbeitsspeicher-Konfiguration ausgestattet und eignen sich daher für die meisten Produktionsworkloads. Die Dv2-Serie ist ca. 35 % schneller als die D-Serie. Die Dv2-Serie wird auf dem Intel® Xeon®-Prozessor 8171M mit 2,1 GHz (Skylake), dem Intel® Xeon®-Prozessor E5-2673 v4 mit 2,3 GHz (Broadwell) oder dem Intel® Xeon®-Prozessor E5-2673 v3 mit 2,4 GHz (Haswell) mit Intel Turbo Boost Technology 2.0 ausgeführt. Der Dv2-Serie hat die gleichen Arbeitsspeicher- und Datenträgerkonfigurationen wie die D-Serie.

- Virtuelle Computer der [Dv3-Serie und der Dsv3-Serie](dv3-dsv3-series.md) werden auf dem Intel® Xeon®-Prozessor 8171M mit 2,1 GHz (Skylake), dem Intel® Xeon®-Prozessor E5-2673 v4 mit 2,3 GHz (Broadwell) oder dem Intel® Xeon®-Prozessor E5-2673 v3 mit 2,4 GHz (Haswell) in einer Hyperthreadkonfiguration ausgeführt und bieten somit ein besseres Preis-Leistungs-Verhältnis für die meisten universellen Workloads. Der Speicher wurde erweitert (von etwa 3.5 GiB/vCPU auf 4 GiB/vCPU), während die Datenträger- und Netzwerkgrenzwerte pro Kern angepasst wurden, um sich für den Übergang zum Hyperthreading anzupassen. Die Dv3-Serie hat nicht mehr die virtuellen Computer mit hohen Arbeitsspeichergrößen der D/Dv2-Serie. Diese sind nun in den arbeitsspeicheroptimierten [Serien Ev3 und Esv3](ev3-esv3-series.md) verfügbar.

Zu den möglichen Anwendungsfällen der D-Serie zählen Unternehmensanwendungen, relationale Datenbanken, In-Memory-Caching und Analysen.

## <a name="other-sizes"></a>Andere Größen

- [Computeoptimiert](sizes-compute.md)
- [Arbeitsspeicheroptimiert](sizes-memory.md)
- [Speicheroptimiert](sizes-storage.md)
- [GPU-optimiert](sizes-gpu.md)
- [High Performance Computing](sizes-hpc.md)
- [Vorherige Generationen](sizes-previous-gen.md)

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen dazu, wie Sie mit [Azure-Computeeinheiten (ACU)](acu.md) die Computeleistung von Azure-SKUs vergleichen können.