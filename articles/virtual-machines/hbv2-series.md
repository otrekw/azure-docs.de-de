---
title: 'HBv2-Serie: Azure Virtual Machines'
description: Spezifikationen für die VMs der HBv2-Serie
author: vermagit
ms.service: virtual-machines
ms.subservice: sizes
ms.topic: conceptual
ms.date: 02/03/2020
ms.author: amverma
ms.openlocfilehash: f9717105c9241777d72a8943e87f33ab31c8038c
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/28/2020
ms.locfileid: "87288491"
---
# <a name="hbv2-series"></a>HBv2-Serie

VMs der HBv2-Serie sind für Anwendungen mit hohen Anforderungen an die Speicherbandbreite optimiert, z. B. Strömungssimulationen, Finite-Element-Analysen oder Lagerstättensimulationen. HBv2-VMs bieten 120 AMD-Prozessorkerne des Modells EPYC 7742, 4 GB RAM pro CPU-Kern und kein gleichzeitiges Multithreading. Jede HBv2-VM bietet eine Speicherbandbreite von bis zu 340 Gbit/s und eine FP64-Computeleistung von bis zu 4 TeraFLOP.

Storage Premium Unterstützt

Livemigration: Nicht unterstützt

Updates mit Speicherbeibehaltung: Nicht unterstützt

| Size | vCPU | Prozessor | Arbeitsspeicher (GB) | Speicherbandbreite GB/s | Basis-CPU-Frequenz (GHz) | Frequenz für alle Kerne (GHz, Spitze) | Frequenz für Einzelkern (GHz, Spitze) | RDMA-Leistung (Gbit/s) | MPI-Unterstützung | Temporärer Speicher (GB) | Max. Anzahl Datenträger | Max. Ethernet-Karten |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_HB120rs_v2 | 120 | AMD EPYC 7V12 | 480 | 350 | 2.45 | 3.1 | 3.3 | 200 | All | 480 + 960 | 8 | 1 |


[!INCLUDE [virtual-machines-common-sizes-table-defs](../../includes/virtual-machines-common-sizes-table-defs.md)]

## <a name="other-sizes"></a>Andere Größen

- [Allgemeiner Zweck](sizes-general.md)
- [Arbeitsspeicheroptimiert](sizes-memory.md)
- [Speicheroptimiert](sizes-storage.md)
- [GPU-optimiert](sizes-gpu.md)
- [High Performance Computing](sizes-hpc.md)
- [Vorherige Generationen](sizes-previous-gen.md)

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen dazu, wie Sie mit [Azure-Computeeinheiten (ACU)](acu.md) die Computeleistung von Azure-SKUs vergleichen können.