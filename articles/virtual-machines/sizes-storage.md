---
title: Größen von virtuellen Azure-Computern – Speicher | Microsoft-Dokumentation
description: Auflistung der verschiedenen verfügbaren speicheroptimierten Größen für virtuelle Computer in Azure. Dieser Artikel listet Informationen zur Anzahl von vCPUs, Datenträgern und Netzwerkschnittstellenkarten sowie zum Speicherdurchsatz und zur Netzwerkbandbreite für Größen dieser Serie auf.
services: virtual-machines
documentationcenter: ''
author: sasha-melamed
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
ms.openlocfilehash: 5a611dc288fc18a14f6000689f9f9b49d4b3feb8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "77913335"
---
# <a name="storage-optimized-virtual-machine-sizes"></a>Speicheroptimierte VM-Größen

Datenspeicheroptimierte VM-Größen bieten hohen Datenträgerdurchsatz und E/A und eignen sich ideal für Big Data, SQL, NoSQL-Datenbanken, Datawarehousing und große transaktionale Datenbanken.  Beispiele bilden Cassandra, MongoDB, Cloudera und Redis. Dieser Artikel enthält Informationen zur Anzahl von vCPUs, Datenträgern und NICs sowie zum lokalen Speicherdurchsatz und zur Netzwerkbandbreite für die einzelnen optimierten Größen.

Die [Lsv2-Reihe](lsv2-series.md) bietet direkt zugeordneten lokalen NVMe-Speicher mit hohem Durchsatz und geringer Latenz auf dem [AMD EPYC<sup>TM</sup> 7551-Prozessor](https://www.amd.com/en/products/epyc-7000-series) mit einer Turbo-Taktfrequenz von 2,55 GHz für alle Kerne und einer maximalen Turbo-Taktfrequenz von 3,0 GHz. Die VMs der Lsv2-Reihe sind in Größen von 8 bis 80 vCPUs in einer Konfiguration mit simultanem Multithreading verfügbar.  Es stehen 8 GiB Arbeitsspeicher pro vCPU und ein NVMe-SSD-M.2-Gerät mit 1,92 TB pro 8 vCPUs zur Verfügung, mit bis zu 19,2 TB (10x1,92 TB) auf dem L80s v2.

## <a name="other-sizes"></a>Andere Größen

- [Allgemeiner Zweck](sizes-general.md)
- [Computeoptimiert](sizes-compute.md)
- [Arbeitsspeicheroptimiert](sizes-memory.md)
- [GPU-optimiert](sizes-gpu.md)
- [High Performance Computing](sizes-hpc.md)
- [Vorherige Generationen](sizes-previous-gen.md)

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen dazu, wie Sie mit [Azure-Computeeinheiten (ACU)](acu.md) die Computeleistung von Azure-SKUs vergleichen können.

Erfahren Sie, wie Sie die Leistung von virtuellen Computern der Lsv2-Reihe für [Windows](windows/storage-performance.md) oder [Linux](linux/storage-performance.md) optimieren.
