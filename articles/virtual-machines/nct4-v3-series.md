---
title: NCas T4 v3-Serie
description: Hier finden Sie Spezifikationen für die VMs der NCas T4 v3-Serie.
services: virtual-machines
ms.subservice: sizes
author: vikancha-MSFT
ms.service: virtual-machines
ms.topic: conceptual
ms.date: 08/10/2020
ms.author: vikancha
ms.openlocfilehash: af9f7eb21d533bc5fb365e7cbf1fb8fc18184fa7
ms.sourcegitcommit: 3246e278d094f0ae435c2393ebf278914ec7b97b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/02/2020
ms.locfileid: "89375226"
---
# <a name="ncast4_v3-series-in-preview"></a>NCasT4_v3-Serie (Vorschau) 

Die virtuellen Computer der NCasT4_v3-Serie basieren auf GPUs vom Typ [Nvidia Tesla T4](https://www.nvidia.com/en-us/data-center/tesla-t4/) sowie auf CPUs vom Typ AMD EPYC 7V12(Rome). Die VMs verfügen über bis zu 4 NVIDIA T4-GPUs mit jeweils 16 GB Arbeitsspeicher, bis zu 64 AMD EPYC 7V12(Rome)-Prozessorkerne ohne Multithreading und 440 GiB Systemspeicher. Diese virtuellen Computer eignen sich ideal für die Ausführung von ML- und KI-Workloads unter Verwendung von CUDA, TensorFlow, Pytorch, Caffe und anderen Frameworks oder von Grafikworkloads mit NVIDIA GRID-Technologie. Die NCasT4_v3-Serie eignet sich ideal für die Ausführung von Rückschlussworkloads.

Sie können eine [Anfrage senden](https://aka.ms/NCT4v3Preview), um am Vorschauprogramm teilzunehmen.

<br>

ACU: 230 – 260

Storage Premium  Unterstützt

Storage Premium-Zwischenspeicherung:  Unterstützt

Livemigration: Nicht unterstützt

Updates mit Speicherbeibehaltung: Nicht unterstützt

| Size | vCPU | Memory: GiB | Temporärer Speicher (SSD): GiB | GPU | GPU-Arbeitsspeicher: GiB | Max. Anzahl Datenträger | Maximale Anzahl NICs |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_NC4as_T4_v3 |4 |28 |180 | 1 | 16 | 8 | 2 |
| Standard_NC8as_T4_v3 |8 |56 |360 | 1 | 16 | 16 | 4  |
| Standard_NC16as_T4_v3 |16 |110 |360 | 1 | 16 | 32 | 8  |
| Standard_NC64as_T4_v3 |64 |440 |2880 | 4 | 64 | 32 | 8  |


[!INCLUDE [virtual-machines-common-sizes-table-defs](../../includes/virtual-machines-common-sizes-table-defs.md)]

## <a name="supported-operating-systems-and-drivers"></a>Unterstützte Betriebssysteme und Treiber

Um die GPU-Funktionen von virtuellen Azure-Computern der NCasT4_v3-Serie unter Windows oder Linux nutzen zu können, müssen Nvidia-GPU-Treiber installiert werden.

Wenn Sie Nvidia-GPU-Treiber manuell installieren möchten, finden Sie Informationen zu unterstützten Betriebssystemen, Treibern und Installation sowie Schritte zur Überprüfung unter [Einrichten von GPU-Treibern der N-Serie für Windows](./windows/n-series-driver-setup.md).

## <a name="other-sizes"></a>Andere Größen

- [Allgemeiner Zweck](sizes-general.md)
- [Arbeitsspeicheroptimiert](sizes-memory.md)
- [Speicheroptimiert](sizes-storage.md)
- [GPU-optimiert](sizes-gpu.md)
- [High Performance Computing](sizes-hpc.md)
- [Vorherige Generationen](sizes-previous-gen.md)

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen dazu, wie Sie mit [Azure-Computeeinheiten (ACU)](acu.md) die Computeleistung von Azure-SKUs vergleichen können.
