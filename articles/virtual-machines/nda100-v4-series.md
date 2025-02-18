---
title: A100_v4-Serie
description: Spezifikationen für die VMs der ND A100 v4-Serie
author: iafinder
ms.service: virtual-machines
ms.subservice: vm-sizes-gpu
ms.topic: conceptual
ms.date: 05/26/2021
ms.author: iafinder
ms.openlocfilehash: 88286e15861ac5be80692d01bfe4768311e3d6e2
ms.sourcegitcommit: 34feb2a5bdba1351d9fc375c46e62aa40bbd5a1f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/10/2021
ms.locfileid: "111886950"
---
# <a name="nd-a100-v4-series"></a>A100_v4-Serie

Die VM der ND A100 v4-Serie ist ein neues Flagship-Mitglied der Azure-GPU-Familie, die für hoch anspruchsvolles Deep Learning-Training und eng gekoppelte, vertikal und horizontal skalierende HPC-Workloads konzipiert ist. 

Die ND A100 v4-Serie beginnt mit einer einzelnen VM und acht NVIDIA Ampere A100 Tensor Core-GPUs. ND A100 v4-basierte Bereitstellungen können auf Tausende von GPUs mit einer Verbindungsbandbreite von 1,6 Tbit/s pro VM hochskaliert werden. Jede GPU innerhalb der VM kommt mit einer eigenen dedizierten, topologieunabhängigen NVIDIA Mellanox HDR InfiniBand-Verbindung, die 200 Gbit/s Bandbreite bietet. Diese Verbindungen werden automatisch zwischen VMs in der gleichen VM-Skalierungsgruppe erstellt und unterstützen GPUDirect-RDMA.

Jede GPU verfügt über NVLINK 3.0-Konnektivität für die VM-interne Kommunikation, und die Instanz wird auch durch 96 physische AMD Epyc™ CPU-Kerne der zweiten Generation unterstützt.

Diese Instanzen bieten hervorragende Leistung für zahlreiche KI-, ML- und Analysetools mit integrierter Unterstützung für GPU-Beschleunigung. Dazu zählen beispielsweise TensorFlow, Pytorch, Caffe, RAPIDS und andere Frameworks. Darüber hinaus wird die horizontal skalierende InfiniBand-Verbindung von vielen vorhandenen KI- und HPC-Tools unterstützt, die auf den NCCL2-Kommunikationsbibliotheken von NVIDIA für das nahtlose GPU-Clustering aufbauen.

> [!IMPORTANT]
> Informationen zu den ersten Schritten mit ND A100 v4-VMs, einschließlich der Treiber- und Netzwerkkonfiguration, finden Sie unter [Konfiguration und Optimierung von HPC-Workloads](./workloads/hpc/configure.md).
> Aufgrund des größeren GPU-Arbeitsspeicherbedarfs werden für die ND A100_v4 [VMs der 2. Generation](./generation-2.md) und Marketplace-Images benötigt. Es werden dringend die [Azure HPC-Images](./workloads/hpc/configure.md) empfohlen. Azure HPC Ubuntu 18.04-, 20.04- und Azure HPC CentOS 7.9-Images werden unterstützt.
> 

<br>

[Storage Premium:](premium-storage-performance.md) Unterstützt<br>
[Storage Premium-Zwischenspeicherung:](premium-storage-performance.md) Unterstützt<br>
[Ultra Disks](disks-types.md#ultra-disk): Unterstützt ([Weitere Informationen](https://techcommunity.microsoft.com/t5/azure-compute/ultra-disk-storage-for-hpc-and-gpu-vms/ba-p/2189312) zur Verfügbarkeit, Verwendung und Leistung) <br>
[Livemigration:](maintenance-and-updates.md) Nicht unterstützt<br>
[Updates mit Speicherbeibehaltung:](maintenance-and-updates.md) Nicht unterstützt<br>
[Unterstützung von VM-Generationen:](generation-2.md) Generation 2<br>
[Beschleunigter Netzwerkbetrieb:](../virtual-network/create-vm-accelerated-networking-cli.md) Nicht unterstützt<br>
[Kurzlebige Betriebssystemdatenträger](ephemeral-os-disks.md): Unterstützt ([in der Vorschauphase](ephemeral-os-disks.md#preview---ephemeral-os-disks-can-now-be-stored-on-temp-disks))<br>
InfiniBand: Unterstützt, GPUDirect-RDMA, 8 x 200 Gigabit HDR<br>
Nvidia NVLink Interconnect: Unterstützt<br>
<br>

| Size | vCPU | Memory: GiB | Temporärer Speicher (SSD): GiB | GPU | GPU-Arbeitsspeicher: GiB | Max. Anzahl Datenträger | Maximaler Durchsatz des Datenträgers ohne Cache: IOPS/MBps | Max. Netzwerkbandbreite | Maximale Anzahl NICs |
|---|---|---|---|---|---|---|---|---|---|
| Standard_ND96asr_v4 | 96 | 900 | 6000 | 8 A100 40-GB-GPUs (NVLink 3.0) | 40 | 32 | 80.000/800 | 24.000 MBit/s | 8 |

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
