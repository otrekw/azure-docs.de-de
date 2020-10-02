---
title: 'H-Serie: Azure Virtual Machines'
description: Spezifikationen für die VMs der H-Serie
author: ju-shim
ms.service: virtual-machines
ms.subservice: sizes
ms.topic: conceptual
ms.date: 09/08/2020
ms.author: amverma
ms.reviewer: jushiman
ms.openlocfilehash: b1f30e91b9ce96daf8b2eb8ac6c8cb38b86b347f
ms.sourcegitcommit: 1b320bc7863707a07e98644fbaed9faa0108da97
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/09/2020
ms.locfileid: "89594406"
---
# <a name="h-series"></a>H-Reihe

Virtuelle Computer der H-Serie sind für Anwendungen optimiert, für die Anforderungen hinsichtlich hoher CPU-Frequenzen oder sehr viel Arbeitsspeicher pro Kern gelten. Virtuelle Computer der H-Serie bieten 8 oder 16 Intel-Prozessorkerne des Modells Xeon E5 2667 v3, bis zu 14 GB RAM pro CPU-Kern und kein Hyperthreading. Die H-Serie unterstützt Mellanox EDR InfiniBand mit 56 GBit/s in einer FAT-Strukturkonfiguration ohne Blocks für einheitliche RDMA-Leistung. Virtuelle Computer der H-Serie sind derzeit nicht SR-IOV-fähig und unterstützen Intel MPI 5.x und MS-MPI.

ACU: 290-300

Storage Premium  Nicht unterstützt

Storage Premium-Zwischenspeicherung:  Nicht unterstützt

Livemigration: Nicht unterstützt

Updates mit Speicherbeibehaltung: Nicht unterstützt

| Size | vCPU | Prozessor | Arbeitsspeicher (GB) | Speicherbandbreite GB/s | Basis-CPU-Frequenz (GHz) | Frequenz für alle Kerne (GHz, Spitze) | Frequenz für Einzelkern (GHz, Spitze) | RDMA-Leistung (Gbit/s) | MPI-Unterstützung | Temporärer Speicher (GB) | Max. Anzahl Datenträger | Max. Datenträger-Datendurchsatz: IOPS | Max. Ethernet-Karten |
| --- | --- |--- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_H8   | 8  | Intel Xeon E5 2667 v3 | 56 | 40 | 3.2 | 3.3 | 3.6 | - | Intel 5.x, MS-MPI | 1000 | 32 | 32 x 500 | 2 |
| Standard_H16  | 16 | Intel Xeon E5 2667 v3 | 112 | 80 | 3.2 | 3.3 | 3.6 | - | Intel 5.x, MS-MPI | 2000 | 64 | 64 x 500 | 4 |
| Standard_H8m  | 8  | Intel Xeon E5 2667 v3 | 112 | 40 | 3.2 | 3.3 | 3.6 | - | Intel 5.x, MS-MPI | 1000 | 32 | 32 x 500 | 2 |
| Standard_H16m | 16 | Intel Xeon E5 2667 v3 | 224 | 80 | 3.2 | 3.3 | 3.6 | - | Intel 5.x, MS-MPI | 2000 | 64 | 64 x 500 | 4 |
| Standard_H16r <sup>1</sup>  | 16 | Intel Xeon E5 2667 v3 | 112 | 80 | 3.2 | 3.3 | 3.6 | 56 | Intel 5.x, MS-MPI | 2000 | 64 | 64 x 500 | 4 |
| Standard_H16mr <sup>1</sup> | 16 | Intel Xeon E5 2667 v3 | 224 | 80 | 3.2 | 3.3 | 3.6 | 56 | Intel 5.x, MS-MPI | 2000 | 64 | 64 x 500 | 4 |

<sup>1</sup> Für MPI-Anwendungen ist ein dediziertes RDMA-Back-End-Netzwerk durch ein FDR InfiniBand-Netzwerk aktiviert.

[!INCLUDE [virtual-machines-common-sizes-table-defs](../../includes/virtual-machines-common-sizes-table-defs.md)]

> [!NOTE]
> Im Rahmen der [RDMA-fähigen VMs](sizes-hpc.md#rdma-capable-instances) ist die H-Serie nicht SR-IOV-fähig. Daher unterscheiden sich die unterstützten [VM-Images](./workloads/hpc/configure.md#vm-images), Anforderungen an [InfiniBand-Treiber](./workloads/hpc/enable-infiniband.md) und unterstützten [MPI-Bibliotheken](./workloads/hpc/setup-mpi.md) von den SR-IOV-fähigen VMs.

## <a name="other-sizes"></a>Andere Größen

- [Allgemeiner Zweck](sizes-general.md)
- [Arbeitsspeicheroptimiert](sizes-memory.md)
- [Speicheroptimiert](sizes-storage.md)
- [GPU-optimiert](sizes-gpu.md)
- [High Performance Computing](sizes-hpc.md)
- [Vorherige Generationen](sizes-previous-gen.md)

## <a name="next-steps"></a>Nächste Schritte

- Informieren Sie sich über das [Konfigurieren Ihrer VMs](./workloads/hpc/configure.md), [Aktivieren von InfiniBand](./workloads/hpc/enable-infiniband.md), [Einrichten von MPI](./workloads/hpc/setup-mpi.md) und Optimieren von HPC-Anwendungen für Azure unter [High Performance Computing auf InfiniBand-fähigen virtuellen Computern der H- und N-Serie](./workloads/hpc/overview.md).
- Informieren Sie sich in den [Tech Community-Blogs zu Azure Compute](https://techcommunity.microsoft.com/t5/azure-compute/bg-p/AzureCompute) über die neuesten Ankündigungen, und machen Sie sich mit einigen Beispielen und Ergebnissen zu HPC vertraut.
- Eine allgemeinere Übersicht über die Architektur für die Ausführung von HPC-Workloads finden Sie unter [High Performance Computing (HPC) in Azure](/azure/architecture/topics/high-performance-computing/).
- Weitere Informationen dazu, wie Sie mit [Azure-Computeeinheiten (ACU)](acu.md) die Computeleistung von Azure-SKUs vergleichen können.
