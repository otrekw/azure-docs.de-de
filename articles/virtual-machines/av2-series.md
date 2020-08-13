---
title: Av2-Serie
description: Hier finden Sie Spezifikationen für die virtuellen Computer der Av2-Serie.
author: migerdes
ms.service: virtual-machines
ms.subservice: sizes
ms.topic: conceptual
ms.date: 02/03/2020
ms.author: jushiman
ms.openlocfilehash: fcbcb89505130dddb42d97c153b172a22a9b29b2
ms.sourcegitcommit: 2ff0d073607bc746ffc638a84bb026d1705e543e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/06/2020
ms.locfileid: "87831480"
---
# <a name="av2-series"></a>Av2-Serie

Die VMs der Av2-Reihe können auf vielen verschiedenen Hardwaretypen und Prozessoren bereitgestellt werden. Die Konfigurationen für CPU-Leistung und Arbeitsspeicher bei virtuellen Computern der Av2-Serie eignen sich am besten für Workloads der Einstiegsebene wie Entwicklung und Tests. Die Größe ist gedrosselt, um eine konsistente Prozessorleistung für die ausgeführte Instanz zu ermöglichen – unabhängig von der für die Bereitstellung gewählten Hardware. Fragen Sie die virtuelle Hardware über die virtuelle Maschine ab, um die physische Hardware zu ermitteln, auf der diese Größe bereitgestellt wird. Einige mögliche Anwendungsfälle: Entwicklungs- und Testserver, Webserver mit geringem Datenverkehr, kleine bis mittelgroße Datenbanken, Proof of Concept und Coderepositorys.

ACU: 100

Storage Premium  Nicht unterstützt

Storage Premium-Zwischenspeicherung:  Nicht unterstützt

Livemigration: Unterstützt

Updates mit Speicherbeibehaltung: Unterstützt


| Size | vCPU | Memory: GiB | Temporärer Speicher (SSD): GiB | Maximaler Durchsatz (temporärer Speicher): IOPS/MBit/s Lesen/MBit/s Schreiben | Max. Datenträger/Durchsatz: IOPS | Maximale Anzahl NICs | Erwartete Netzwerkbandbreite (MBit/s)
|---|---|---|---|---|---|---|---|
| Standard_A1_v2  | 1 | 2  | 10 | 1\.000/20/10  | 2/2 x 500   | 2 | 250  |
| Standard_A2_v2  | 2 | 4  | 20 | 2\.000/40/20  | 4/4 x 500   | 2 | 500  |
| Standard_A4_v2  | 4 | 8  | 40 | 4\.000/80/40  | 8/8 x 500   | 4 | 1000 |
| Standard_A8_v2  | 8 | 16 | 80 | 8\.000/160/80 | 16/16 x 500 | 8 | 2000 |
| Standard_A2m_v2 | 2 | 16 | 20 | 2\.000/40/20  | 4/4 x 500   | 2 | 500  |
| Standard_A4m_v2 | 4 | 32 | 40 | 4\.000/80/40  | 8/8 x 500   | 4 | 1000 |
| Standard_A8m_v2 | 8 | 64 | 80 | 8\.000/160/80 | 16/16 x 500 | 8 | 2000 |

[!INCLUDE [virtual-machines-common-sizes-table-defs](../../includes/virtual-machines-common-sizes-table-defs.md)]

## <a name="other-sizes-and-information"></a>Weitere Größen und Informationen

- [Allgemeiner Zweck](sizes-general.md)
- [Arbeitsspeicheroptimiert](sizes-memory.md)
- [Speicheroptimiert](sizes-storage.md)
- [GPU-optimiert](sizes-gpu.md)
- [High Performance Computing](sizes-hpc.md)
- [Vorherige Generationen](sizes-previous-gen.md)

Preisrechner: [Preisrechner](https://azure.microsoft.com/pricing/calculator/)

Weitere Informationen zu Datenträgertypen: [Datenträgertypen](./linux/disks-types.md#ultra-disk)

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen dazu, wie Sie mit [Azure-Computeeinheiten (ACU)](acu.md) die Computeleistung von Azure-SKUs vergleichen können.