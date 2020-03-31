---
title: 'Serien Dv2 und Dsv2: Azure Virtual Machines'
description: Spezifikationen für die VMs der Serien Dv2 und Dsv2
services: virtual-machines
author: joelpelley
ms.service: virtual-machines
ms.topic: article
ms.date: 02/03/2020
ms.author: lahugh
ms.openlocfilehash: 808b14f118e842cb9e52d110075f92ba25a343c9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "78164422"
---
# <a name="dv2-and-dsv2-series"></a>Dv2- und DSv2-Serie

Die Serien Dv2 und Dsv2, Nachfolger der ursprünglichen D-Serie, sind mit schnellen CPUs und einer optimalen CPU-zu-Arbeitsspeicher-Konfiguration ausgestattet und eignen sich daher für die meisten Produktionsworkloads. Die Dv2-Serie ist ca. 35 % schneller als die D-Serie. Die Dv2-Serie wird auf dem Intel® Xeon®-Prozessor 8171M mit 2,1 GHz (Skylake), dem Intel® Xeon®-Prozessor E5-2673 v4 mit 2,3 GHz (Broadwell) oder dem Intel® Xeon®-Prozessor E5-2673 v3 mit 2,4 GHz (Haswell) mit Intel Turbo Boost Technology 2.0 ausgeführt. Der Dv2-Serie hat die gleichen Arbeitsspeicher- und Datenträgerkonfigurationen wie die D-Serie.

## <a name="dv2-series"></a>Dv2-Serie

Die Größen der Dv2-Serie werden auf dem Intel® Xeon®-Prozessor 8171M mit 2,1 GHz (Skylake), dem Intel® Xeon®-Prozessor E5-2673 v4 mit 2,3 GHz (Broadwell) oder dem Intel® Xeon®-Prozessor E5-2673 v3 mit 2,4 GHz (Haswell) mit Intel Turbo Boost Technology 2.0 ausgeführt.

ACU: 210–250

Storage Premium  Nicht unterstützt

Storage Premium-Zwischenspeicherung:  Nicht unterstützt

Livemigration: Unterstützt

Updates mit Speicherbeibehaltung: Unterstützt

| Size | vCPU | Memory: GiB | Temporärer Speicher (SSD): GiB | Maximaler Durchsatz (temporärer Speicher): IOPS/MBit/s Lesen/MBit/s Schreiben | Max. Anzahl Datenträger | Durchsatz: IOPS | Maximale Anzahl NICs/erwartete Netzwerkbandbreite (MBit/s) |
|---|---|---|---|---|---|---|---|
| Standard_D1_v2 | 1  | 3,5 | 50  | 3000/46/23    | 4  | 4 x 500  | 2/750   |
| Standard_D2_v2 | 2  | 7   | 100 | 6000/93/46    | 8  | 8 x 500  | 2/1.500  |
| Standard_D3_v2 | 4  | 14  | 200 | 12000/187/93  | 16 | 16 x 500 | 4/3.000  |
| Standard_D4_v2 | 8  | 28  | 400 | 24000/375/187 | 32 | 32 x 500 | 8/6.000  |
| Standard_D5_v2 | 16 | 56  | 800 | 48000/750/375 | 64 | 64 x 500 | 8/12.000 |

## <a name="dsv2-series"></a>DSv2-Serie

Die Größen der DSv2-Serie werden auf dem Intel® Xeon®-Prozessor 8171M mit 2,1 GHz (Skylake), dem Intel® Xeon®-Prozessor E5-2673 v4 mit 2,3 GHz (Broadwell) oder dem Intel® Xeon®-Prozessor E5-2673 v3 mit 2,4 GHz (Haswell) mit Intel Turbo Boost Technology 2.0 ausgeführt und verwenden Storage Premium.

ACU: 210–250

Storage Premium  Unterstützt

Storage Premium-Zwischenspeicherung:  Unterstützt

Livemigration: Unterstützt

Updates mit Speicherbeibehaltung: Unterstützt

| Size | vCPU | Memory: GiB | Temporärer Speicher (SSD): GiB | Max. Anzahl Datenträger | Maximaler Durchsatz (Cache und temporärer Speicher): IOPS/MBit/s (Cachegröße in GiB) | Maximaler Durchsatz des Datenträgers ohne Cache: IOPS/MBit/s | Maximale Anzahl NICs/erwartete Netzwerkbandbreite (MBit/s) |
|---|---|---|---|---|---|---|---|
| Standard_DS1_v2 | 1  | 3,5 | 7   | 4  | 4\.000/32 (43)    | 3\.200/48   | 2/750   |
| Standard_DS2_v2 | 2  | 7   | 14  | 8  | 8\.000/64 (86)    | 6\.400/96   | 2/1.500  |
| Standard_DS3_v2 | 4  | 14  | 28  | 16 | 16.000/128 (172) | 12.800/192 | 4/3.000  |
| Standard_DS4_v2 | 8  | 28  | 56  | 32 | 32.000/256 (344) | 25.600/384 | 8/6.000  |
| Standard_DS5_v2 | 16 | 56  | 112 | 64 | 64.000/512 (688) | 51.200/768 | 8/12.000 |

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
