---
title: 'Edv4- und Edsv4-Serie: Azure Virtual Machines'
description: Hier finden Sie Spezifikationen für VMs der Serie Ev4, Edv4, Esv4 und Edsv4.
author: brbell
ms.author: brbell
ms.reviewer: cynthn
ms.custom: mimckitt
ms.service: virtual-machines
ms.topic: conceptual
ms.date: 02/04/2020
ms.openlocfilehash: 795891f56985504a3584089d7377f753605ba4dd
ms.sourcegitcommit: 309cf6876d906425a0d6f72deceb9ecd231d387c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/01/2020
ms.locfileid: "84263039"
---
# <a name="edv4-and-edsv4-series"></a>Edv4- und Edsv4-Serie

Die Edv4- und Edsv4-Serie läuft auf Intel-Prozessoren des Typs &reg; Xeon&reg; Platinum 8272CL (Cascade Lake) in einer Hyperthreadkonfiguration. Sie eignet sich ideal für verschiedene arbeitsspeicherintensive Unternehmensanwendungen und bietet bis zu 504 GiB RAM, [Intel&reg; Turbo Boost Technology 2.0](https://www.intel.com/content/www/us/en/architecture-and-technology/turbo-boost/turbo-boost-technology.html), [Intel&reg; Hyper-Threading Technology](https://www.intel.com/content/www/us/en/architecture-and-technology/hyper-threading/hyper-threading-technology.html) and [Intel&reg; Advanced Vector Extensions 512 (Intel&reg; AVX-512)](https://www.intel.com/content/www/us/en/architecture-and-technology/avx-512-overview.html). Diese neuen VM-Größen bieten 50 % mehr lokalen Speicher sowie bessere IOPS auf lokalen Datenträgern für Lese- und Schreibvorgänge im Vergleich zu den Größen [Ev3/Esv3](https://docs.microsoft.com/azure/virtual-machines/ev3-esv3-series) mit [Gen2-VMs](https://docs.microsoft.com/azure/virtual-machines/linux/generation-2).


> [!IMPORTANT]
> Wenn Sie einen neuen virtuellen Computer unter Verwendung der Edv4- oder Edsv4-Serie bereitstellen und ein Linux-Image nutzen möchten, müssen Sie entweder RHEL 8.x, CentOS 8.x oder Oracle 7.x oder höher verwenden. Bei Wahl von RHEL 7.x, CentOS 7.x oder Orcale 6.x kommt es zu einem Kernel Panic-Fehler. Microsoft arbeitet aktiv an der Bereitstellung einer Korrektur. Nur RHEL, CentOS und Oracle sind betroffen.

## <a name="edv4-series"></a>Edv4-Serie

Die Edv4-Serie läuft auf Intel-Prozessoren des Typs Intel&reg; Xeon&reg; Platinum 8272CL (Cascade Lake). Edv4-VMs bieten bis zu 504 GiB RAM sowie schnellen und großen lokalen SSD-Speicher (bis zu 2.400 GiB). Diese VMs eignen sich ideal für arbeitsspeicherintensive Unternehmensanwendungen und Anwendungen, die von lokalem Hochgeschwindigkeitsspeicher mit geringer Latenz profitieren. An Edv4-VMs können Sie SSD Standard- und HDD Standard-Datenträger anfügen. 

ACU: 195–210

Storage Premium  Nicht unterstützt

Storage Premium-Zwischenspeicherung:  Nicht unterstützt

Livemigration: Unterstützt

Updates mit Speicherbeibehaltung: Unterstützt

| Size | vCPU | Memory: GiB | Temporärer Speicher (SSD): GiB | Max. Anzahl Datenträger | Maximaler Durchsatz (Cache und temporärer Speicher): IOPS/MBit/s | Maximale Anzahl NICs/Erwartete Netzwerkbandbreite (MBit/s) |
|---|---|---|---|---|---|---|
| Standard_E2d_v4  | 2 | 16 | 75 | 4 | 19.000/120 | 2/1000 |
| Standard_E4d_v4  | 4 | 32 | 150 | 8 | 38.500/242 | 2/2000 |
| Standard_E8d_v4 | 8 | 64 | 300 | 16 | 77.000/485 | 4/4000 |
| Standard_E16d_v4 | 16 | 128 | 600 | 32 | 154.000/968 | 8/8000 |
| Standard_E20d_v4 | 20 | 160 | 750 | 32 | 193.000/1.211  | 8/10000 |
| Standard_E32d_v4 | 32 | 256 | 1200 | 32 | 308.000/1.936 | 8/16000 |
| Standard_E48d_v4 | 48 | 384 | 1800 | 32 | 462.000/2.904 | 8/24000 |
| Standard_E64d_v4 | 64 | 504 | 2400 | 32 | 615.000/3.872 | 8/30000 |


## <a name="edsv4-series"></a>Edsv4-Serie

Die Edsv4-Serie läuft auf Intel-Prozessoren des Typs Intel&reg; Xeon&reg; Platinum 8272CL (Cascade Lake). Edsv4-VMs bieten bis zu 504 GiB RAM sowie schnellen und großen lokalen SSD-Speicher (bis zu 2.400 GiB). Diese VMs eignen sich ideal für arbeitsspeicherintensive Unternehmensanwendungen und Anwendungen, die von lokalem Hochgeschwindigkeitsspeicher mit geringer Latenz profitieren.

ACU: 195-210

Storage Premium  Unterstützt

Storage Premium-Zwischenspeicherung:  Unterstützt

Livemigration: Unterstützt

Updates mit Speicherbeibehaltung: Unterstützt

| Size | vCPU | Memory: GiB | Temporärer Speicher (SSD): GiB | Max. Anzahl Datenträger | Maximaler Durchsatz (Cache und temporärer Speicher): IOPS/MBit/s | Maximaler Durchsatz des Datenträgers ohne Cache: IOPS/MBit/s | Maximale Anzahl NICs/Erwartete Netzwerkbandbreite (MBit/s) |
|---|---|---|---|---|---|---|---|
| Standard_E2ds_v4  | 2 | 16 | 75 | 4 | 19.000/120(50) | 3200/48 | 2/1000 |
| Standard_E4ds_v4  | 4 | 32 | 150 | 8 | 38.500/242(100) | 6400/96 | 2/2000 |
| Standard_E8ds_v4 | 8 | 64 | 300 | 16 | 77.000/485(200) | 12800/192 | 4/4000 |
| Standard_E16ds_v4 | 16 | 128 | 600 | 32 | 154.000/968(400) | 25600/384 | 8/8000 |
| Standard_E20ds_v4 | 20 | 160 | 750 | 32 | 193.000/1.211(500)  | 32000/480  | 8/10000 |
| Standard_E32ds_v4 | 32 | 256 | 1200 | 32 | 308.000/1.936(800) | 51200/768  | 8/16000 |
| Standard_E48ds_v4 | 48 | 384 | 1800 | 32 | 462.000/2.904(1.200) | 76800/1152 | 8/24000 |
| Standard_E64ds_v4 <sup>1</sup> | 64 | 504 | 2400 | 32 | 615.000/3.872(1.600) | 80000/1200 | 8/30000 |

<sup>1</sup> [Eingeschränkte Kerngrößen verfügbar](https://docs.microsoft.com/azure/virtual-machines/windows/constrained-vcpu).


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
