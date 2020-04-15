---
title: 'M-Serie: Azure Virtual Machines'
description: Hier finden Sie Spezifikationen für die virtuellen Computer der M-Serie.
services: virtual-machines
author: ayshakeen
ms.service: virtual-machines
ms.topic: article
ms.date: 03/31/2020
ms.author: jushiman
ms.openlocfilehash: 3e0552570d5bdb7f812852cd058710b833b7fdd4
ms.sourcegitcommit: b0ff9c9d760a0426fd1226b909ab943e13ade330
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/01/2020
ms.locfileid: "80521340"
---
# <a name="m-series"></a>M-Serie

Die M-Serie verfügt über eine hohe vCPU-Anzahl (bis zu 128 vCPUs) und eine große Menge von Arbeitsspeicher (bis zu 3,8 TiB). Dies ist auch ideal für extrem große Datenbanken oder andere Anwendungen, für die eine hohe vCPU-Anzahl und große Mengen an Arbeitsspeicher benötigt werden. Die Größen der M-Serie werden sowohl unter der Intel&reg; Xeon&reg; CPU E7-8890 v3 mit 2,50 GHz und Intel&reg; Xeon&reg; Platinum 8280M mit 2,7 GHz (Cascade Lake) unterstützt.

Virtuelle Computer der M-Serie verfügen über Hyperthreading-Technologie von Intel&reg;.

ACU: 160-180

Storage Premium  Unterstützt

Storage Premium-Zwischenspeicherung:  Unterstützt

Livemigration: Nicht unterstützt

Updates mit Speicherbeibehaltung: Nicht unterstützt

Schreibbeschleunigung:  [Unterstützt](https://docs.microsoft.com/azure/virtual-machines/windows/how-to-enable-write-accelerator)

| Size | vCPU | Memory: GiB | Temporärer Speicher (SSD): GiB | Max. Anzahl Datenträger | Maximaler Durchsatz (Cache und temporärer Speicher): IOPS/MBit/s (Cachegröße in GiB) | Maximaler Durchsatz des Datenträgers ohne Cache: IOPS/MBit/s | Maximale Anzahl NICs/erwartete Netzwerkbandbreite (MBit/s) |
|---|---|---|---|---|---|---|---|
| Standard_M8ms <sup>2</sup>       | 8   | 218,75 | 256   | 8  | 10.000/100 (793)     | 5000/125   | 4/2.000  |
| Standard_M16ms <sup>2</sup>      | 16  | 437,5  | 512   | 16 | 20.000/200 (1587)    | 10.000/250  | 8/4.000  |
| Standard_M32ts                   | 32  | 192    | 1024  | 32 | 40.000/400 (3174)    | 20.000/500  | 8/8.000  |
| Standard_M32ls                   | 32  | 256    | 1024  | 32 | 40.000/400 (3174)    | 20.000/500  | 8/8.000  |
| Standard_M32ms <sup>2</sup>      | 32  | 875    | 1024  | 32 | 40.000/400 (3174)    | 20.000/500  | 8/8.000  |
| Standard_M64s                    | 64  | 1024   | 2048  | 64 | 80.000/800 (6348)    | 40.000/1.000 | 8/16.000 |
| Standard_M64ls                   | 64  | 512    | 2048  | 64 | 80.000/800 (6348)    | 40.000/1.000 | 8/16.000 |
| Standard_M64ms <sup>3</sup>      | 64  | 1792   | 2048  | 64 | 80.000/800 (6348)    | 40.000/1.000 | 8/16.000 |
| Standard_M128s <sup>1</sup>      | 128 | 2048   | 4096  | 64 | 160.000/1.600 (12696) | 80.000/2.000 | 8/30.000 |
| Standard_M128ms <sup>1,2,3</sup> | 128 | 3\.892   | 4096  | 64 | 160.000/1.600 (12696) | 80.000/2.000 | 8/30.000 |
| Standard_M64                     | 64  | 1024   | 7168  | 64 | 80.000/800 (1228)    | 40.000/1.000 | 8/16.000 |
| Standard_M64m                    | 64  | 1792   | 7168  | 64 | 80.000/800 (1228)    | 40.000/1.000 | 8/16.000 |
| Standard_M128 <sup>1</sup>       | 128 | 2048   | 14336 | 64 | 250.000/1.600 (2456)  | 80.000/2.000 | 8/32000 |
| Standard_M128m <sup>1</sup>      | 128 | 3\.892   | 14336 | 64 | 250.000/1.600 (2456)  | 80.000/2.000 | 8/32.000 |

<sup>1</sup> Bei mehr als 64 vCPUs ist eines der folgenden Gastbetriebssysteme erforderlich: Windows Server 2016, Ubuntu 16.04 LTS, SLES 12 SP2 und Red Hat Enterprise Linux, CentOS 7.3 oder Oracle Linux 7.3 mit LIS 4.2.1.

<sup>2</sup> Eingeschränkte Kerngrößen verfügbar.

<sup>3</sup> Instanz wird isoliert auf dedizierter Hardware ausgeführt, die für einen einzigen Kunden bereitgestellt wird.

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
