---
title: Dav4- und Dasv4-Serie
description: Hier finden Sie Spezifikationen für die virtuellen Computer der Dav4- und Dasv4-Serie.
author: migerdes
ms.service: virtual-machines
ms.subservice: sizes
ms.topic: article
ms.date: 02/03/2020
ms.author: jushiman
ms.openlocfilehash: 5d8478d97ba2615836f3e121d9dd56d94fcd73fc
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "84678391"
---
# <a name="dav4-and-dasv4-series"></a>Dav4- und Dasv4-Serie

Die Dav4-Serie und die Dasv4-Serie sind neue Größen, bei denen der AMD-Prozessor EPYC<sup>TM</sup> 7452 mit 2,35 GHz in einer Multithreadkonfiguration mit bis zu 256 MB L3-Cache verwendet wird. Hierbei werden 8 MB des L3-Caches jeweils acht Kernen zur Verfügung gestellt, was Kunden bei der Ausführung ihrer universellen Workloads mehr Optionen gibt. Die Dav4-Serie und die Dasv4-Serie verfügen über die gleichen Arbeitsspeicher- und Datenträgerkonfigurationen wie die D- und die Dsv3-Serie.

## <a name="dav4-series"></a>Dav4-Serie

ACU: 230 – 260

Storage Premium Nicht unterstützt

Storage Premium-Zwischenspeicherung: Nicht unterstützt

Livemigration: Unterstützt

Updates mit Speicherbeibehaltung: Unterstützt

Die Größen der Dav4-Serie basieren auf dem AMD-Prozessor EPYC<sup>TM</sup> 7452 mit 2,35 GHz, der mittels Boosting einen maximalen Takt von 3,35 GHz erreichen kann. Die Größen der Dav4-Serie bieten eine Kombination aus vCPU, Arbeitsspeicher und temporärem Speicher, die für die meisten Produktionsworkloads geeignet ist. Datenträgerspeicher wird separat zu virtuellen Computern abgerechnet. Um SSD Premium zu verwenden, verwenden Sie die Größen der Dasv4-Serie. Für Dasv4-Größen gelten die gleichen Preise und Verbrauchseinheiten für die Abrechnung wie bei der Dav4-Serie.

| Size | vCPU | Memory: GiB | Temporärer Speicher (SSD): GiB | Max. Anzahl Datenträger | Maximaler Durchsatz (temporärer Speicher): IOPS/MBit/s Lesen/MBps Schreiben | Maximale Anzahl NICs/Erwartete Netzwerkbandbreite (MBit/s) |
|-----|-----|-----|-----|-----|-----|-----|
| Standard_D2a_v4 |  2  | 8  | 50  | 4  | 3000/46/23   | 2/1000 |
| Standard_D4a_v4 |  4  | 16 | 100 | 8  | 6000/93/46   | 2/2000 |
| Standard_D8a_v4 |  8  | 32 | 200 | 16 | 12000/187/93 | 4/4000 |
| Standard_D16a_v4|  16 | 64 | 400 |32  | 24000/375/187 |8 / 8000 |
| Standard_D32a_v4|  32 | 128| 800 | 32 | 48000/750/375 |8/16000 |
| Standard_D48a_v4| 48 | 192| 1200 | 32 | 96000/1000/500 | 8/24000 |
| Standard_D64a_v4| 64 | 256 | 1600 | 32 | 96000/1000/500 | 8 / 30000 |
| Standard_D96a_v4| 96 | 384 | 2400 | 32 | 96000/1000/500 | 8 / 30000 |

## <a name="dasv4-series"></a>Dasv4-Serie

ACU:  230 – 260

Storage Premium Unterstützt

Storage Premium-Zwischenspeicherung: Unterstützt

Livemigration: Unterstützt

Updates mit Speicherbeibehaltung: Unterstützt

Die Größen der Dasv4-Serie basieren auf dem AMD-Prozessor EPYC<sup>TM</sup> 7452 mit 2,35 GHz, der mittels Boosting einen maximalen Takt von 3,35 GHz erreichen und SSD Premium verwenden kann. Die Größen der Dasv4-Serie bieten eine Kombination aus vCPU, Arbeitsspeicher und temporärem Speicher, die für die meisten Produktionsworkloads geeignet ist.

| Size | vCPU | Memory: GiB | Temporärer Speicher (SSD): GiB | Max. Anzahl Datenträger | Maximaler Durchsatz (Cache und temporärer Speicher): IOPS/MBps (Cachegröße in GiB) | Maximaler Durchsatz des Datenträgers ohne Cache: IOPS/MBps | Maximale Anzahl NICs/Erwartete Netzwerkbandbreite (MBit/s) |
|-----|-----|-----|-----|-----|-----|-----|-----|
| Standard_D2as_v4|2|8|16|4|4000/32 (50)|3200/48|2/1000 |
| Standard_D4as_v4|4|16|32|8|8000/64 (100)|6400/96|2/2000 |
| Standard_D8as_v4|8|32|64|16|16000/128 (200)|12800/192|4/4000 |
| Standard_D16as_v4|16|64|128|32|32.000/255 (400)|25600/384|8 / 8000 |
| Standard_D32as_v4|32|128|256|32|64.000/510 (800)|51200/768|8/16000 |
| Standard_D48as_v4|48|192|384|32|96.000/1.020 (1.200)|76.800/1.148|8/24000 |
| Standard_D64as_v4|64|256|512|32|128.000/1.020 (1.600)|80000/1200|8 / 30000 | 
| Standard_D96as_v4|96|384|768|32|192.000/1.020 (2.400)|80000/1200|8 / 30000 |

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
