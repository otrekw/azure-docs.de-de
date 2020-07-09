---
title: Größen von virtuellen Azure-Computern – allgemeiner Zweck | Microsoft-Dokumentation
description: Auflistung der verschiedenen verfügbaren allgemeinen Größen für virtuelle Computer in Azure. Dieser Artikel listet Informationen zur Anzahl von vCPUs, Datenträgern und Netzwerkschnittstellenkarten sowie zum Speicherdurchsatz und zur Netzwerkbandbreite für Größen dieser Serie auf.
services: virtual-machines
documentationcenter: ''
author: mimckitt
manager: gwallace
editor: ''
tags: azure-resource-manager,azure-service-management
ms.assetid: ''
ms.service: virtual-machines
ms.devlang: na
ms.topic: article
ms.workload: infrastructure-services
ms.date: 02/20/2020
ms.author: mimckitt
ms.openlocfilehash: b5e597eebda6a730d301eefdbd2d7e6859f5a7cc
ms.sourcegitcommit: 8017209cc9d8a825cc404df852c8dc02f74d584b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/01/2020
ms.locfileid: "84248310"
---
# <a name="general-purpose-virtual-machine-sizes"></a>Universelle VM-Größen

Universelle VM-Größen zeichnen sich durch ein ausgewogenes Verhältnis zwischen CPU und Arbeitsspeicher aus. Ideal für Tests und Entwicklung, kleine bis mittlere Datenbanken sowie Webserver mit geringer bis mittlerer Auslastung. Dieser Artikel enthält Informationen zu den Angeboten für allgemeines Computing.

- Die virtuellen Computer der [Av2-Reihe](av2-series.md) können auf vielen verschiedenen Hardwaretypen und Prozessoren bereitgestellt werden. Die Konfigurationen für CPU-Leistung und Arbeitsspeicher bei virtuellen Computern der A-Serie eignen sich am besten für Workloads wie Entwicklung und Tests. Die Größe ist basierend auf der Hardware gedrosselt, um eine konsistente Prozessorleistung für die ausgeführte Instanz zu ermöglichen – unabhängig von der Hardware, die für die Bereitstellung gewählt wird. Fragen Sie die virtuelle Hardware über die virtuelle Maschine ab, um die physische Hardware zu ermitteln, auf der diese Größe bereitgestellt wird. Mögliche Anwendungsfälle: Entwicklungs- und Testserver, Webserver mit geringem Datenverkehr, kleine bis mittelgroße Datenbanken, Proof of Concept und Coderepositorys.

  > [!NOTE]
  > Die Außerbetriebnahme von A8- bis A11-VMs ist für März 2021 geplant. Weitere Informationen finden Sie im [HPC-Migrationsleitfaden](https://azure.microsoft.com/resources/hpc-migration-guide/).

- [Virtuelle Burst-fähige Computer der B-Serie](sizes-b-series-burstable.md) sind ideal für Workloads geeignet, die nicht kontinuierlich die volle Leistung der CPU benötigen. Hierzu zählen beispielsweise Webserver, kleine Datenbanken sowie Entwicklungs- und Testumgebungen. Diese Workloads haben in der Regel kurzfristige Leistungsanforderungen. Mit der B-Serie können diese Kunden eine VM-Größe mit einer preisgünstigen Grundleistung erwerben. Für die VM-Instanz können dann Gutschriften erlangt werden, wenn für die VM weniger als die Grundleistung genutzt wird. Wenn für den virtuellen Computer Guthaben gebildet wurde, kann Leistung genutzt werden, die über die Grundleistung der VM hinausgeht. Dabei kann die CPU zu 100 Prozent verwendet werden, wenn Ihre Anwendung eine solche Leistung benötigt.

- Die [Dav4-Serie und die Dasv4-Serie](dav4-dasv4-series.md) sind neue Größen, bei denen der AMD-Prozessor EPYC<sup>TM</sup> 7452 mit 2,35 GHz in einer Multithreadkonfiguration mit bis zu 256 MB L3-Cache verwendet wird. Hierbei werden 8 MB des L3-Caches jeweils acht Kernen zur Verfügung gestellt, um Kunden mehr Möglichkeiten bei der Ausführung ihrer universellen Workloads zu bieten. Die Dav4-Serie und die Dasv4-Serie verfügen über die gleichen Arbeitsspeicher- und Datenträgerkonfigurationen wie die D- und die Dsv3-Serie.

- Die [DCv2-Serie](dcv2-series.md) kann Ihnen beim Schutz der Vertraulichkeit und Integrität Ihrer Daten und Ihres Codes während der Verarbeitung in der öffentlichen Cloud helfen. Diese Computer werden von der neuesten Generation von Intel XEON E-2288G-Prozessoren mit SGX-Technologie unterstützt. Mit der Intel Turbo Boost Technology können diese Computer bis zu 5,0 GHz erreichen. Instanzen der DCv2-Serie ermöglichen es Kunden, Secure Enclave-basierte Anwendungen zu erstellen, um ihren Code und Daten zu schützen, während sie verwendet werden.

- Virtuelle Computer der [Dv2-Serie und der Dsv2-Serie](dv2-dsv2-series.md), eine Nachfolgerin der ursprünglichen D-Serie, sind mit leistungsfähigeren CPUs und einer optimalen CPU-zu-Arbeitsspeicher-Konfiguration ausgestattet und eignen sich daher für die meisten Produktionsworkloads. Die Dv2-Serie ist ca. 35 % schneller als die D-Serie. Die Dv2-Serie wird auf dem Intel® Xeon®-Prozessor 8171M mit 2,1 GHz (Skylake), dem Intel® Xeon®-Prozessor E5-2673 v4 mit 2,3 GHz (Broadwell) oder dem Intel® Xeon®-Prozessor E5-2673 v3 mit 2,4 GHz (Haswell) mit Intel Turbo Boost Technology 2.0 ausgeführt. Der Dv2-Serie hat die gleichen Arbeitsspeicher- und Datenträgerkonfigurationen wie die D-Serie.

- Virtuelle Computer der [Dv3-Serie und der Dsv3-Serie](dv3-dsv3-series.md) werden auf dem Intel® Xeon®-Prozessor 8171M mit 2,1 GHz (Skylake), dem Intel® Xeon®-Prozessor E5-2673 v4 mit 2,3 GHz (Broadwell) oder dem Intel® Xeon®-Prozessor E5-2673 v3 mit 2,4 GHz (Haswell) in einer Hyperthreadkonfiguration ausgeführt und bieten somit ein besseres Preis-Leistungs-Verhältnis für die meisten universellen Workloads. Der Speicher wurde erweitert (von etwa 3.5 GiB/vCPU auf 4 GiB/vCPU), während die Datenträger- und Netzwerkgrenzwerte pro Kern angepasst wurden, um sich für den Übergang zum Hyperthreading anzupassen. Die Dv3-Serie hat nicht mehr die virtuellen Computer mit hohen Arbeitsspeichergrößen der D/Dv2-Serie. Diese sind nun in den arbeitsspeicheroptimierten [Serien Ev3 und Esv3](ev3-esv3-series.md) verfügbar.

- VMs der [Ddv4- und Ddsv4-Serie](ddv4-ddsv4-series.md) werden auf Intel-Prozessoren der zweiten Generation des Typs Intel&reg; Xeon&reg; Platinum 8272CL (Cascade Lake) ausgeführt, die für die meisten universellen Workloads ein besseres Preis-Leistungs-Verhältnis bieten. Sie verfügen über eine kontinuierliche Turbo-Taktfrequenz von 3,4 GHz für alle Kerne sowie [Intel&reg; Turbo Boost Technology 2.0](https://www.intel.com/content/www/us/en/architecture-and-technology/turbo-boost/turbo-boost-technology.html), [Intel&reg; Hyper-Threading Technology](https://www.intel.com/content/www/us/en/architecture-and-technology/hyper-threading/hyper-threading-technology.html) und [Intel&reg; Advanced Vector Extensions 512 (Intel AVX-512)](https://www.intel.com/content/www/us/en/architecture-and-technology/avx-512-overview.html). 


## <a name="other-sizes"></a>Andere Größen

- [Computeoptimiert](sizes-compute.md)
- [Arbeitsspeicheroptimiert](sizes-memory.md)
- [Speicheroptimiert](sizes-storage.md)
- [GPU-optimiert](sizes-gpu.md)
- [High Performance Computing](sizes-hpc.md)
- [Vorherige Generationen](sizes-previous-gen.md)

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen dazu, wie Sie mit [Azure-Computeeinheiten (ACU)](acu.md) die Computeleistung von Azure-SKUs vergleichen können.
