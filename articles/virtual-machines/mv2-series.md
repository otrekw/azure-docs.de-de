---
title: 'Mv2-Serie: Azure Virtual Machines'
description: Hier finden Sie Spezifikationen für die virtuellen Computer der Mv2-Serie.
services: virtual-machines
author: ayshakeen
ms.service: virtual-machines
ms.topic: article
ms.date: 04/07/2020
ms.author: lahugh
ms.openlocfilehash: 764dc93608ae3b8882b7048a722c6d3415cbc644
ms.sourcegitcommit: d187fe0143d7dbaf8d775150453bd3c188087411
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/08/2020
ms.locfileid: "80885785"
---
# <a name="mv2-series"></a>Mv2-Serie

Die Mv2-Serie verfügt über eine Plattform mit hohem Durchsatz und geringer Latenz, die auf einem Prozessor vom Typ Intel® Xeon® Platinum 8180M 2,5 GHz (Skylake) mit Hyperthreading ausgeführt wird, der eine Basistaktfrequenz von 2,5 GHz für alle Kerne und eine maximale Turbotaktfrequenz von 3,8 GHz aufweist. Für alle VM-Größen der Mv2-Serie können sowohl persistente Standard- als auch Premium-Datenträger verwendet werden. Bei Instanzen der Mv2-Serie handelt es sich um arbeitsspeicheroptimierte VM-Größen zur Bereitstellung einer unvergleichlichen Computeleistung, um große In-Memory-Datenbanken und Workloads zu unterstützen. Sie verfügen über ein hohes Arbeitsspeicher/CPU-Verhältnis, das ideal für relationale Datenbankserver, große Caches und In-Memory-Analysen geeignet ist.

Virtuelle Computer der Mv2-Serie verfügen über Hyperthreading-Technologie von Intel®

Storage Premium Unterstützt

Storage Premium-Zwischenspeicherung: Unterstützt

Livemigration: Nicht unterstützt

Updates mit Speicherbeibehaltung: Nicht unterstützt

Schreibbeschleunigung: [Unterstützt](https://docs.microsoft.com/azure/virtual-machines/windows/how-to-enable-write-accelerator)

|Size | vCPU | Memory: GiB | Temporärer Speicher (SSD): GiB | Max. Anzahl Datenträger | Maximaler Durchsatz (Cache und temporärer Speicher): IOPS/MBps (Cachegröße in GiB) | Maximaler Durchsatz des Datenträgers ohne Cache: IOPS/MBps | Maximale Anzahl NICs/Erwartete Netzwerkbandbreite (MBps) |
|---|---|---|---|---|---|---|---|
| Standard_M208ms_v2<sup>1</sup> | 208 | 5\.700 | 4096 | 64 | 80000/800 (7040) | 40000/1000 | 8/16000 |
| Standard_M208s_v2<sup>1</sup> | 208 | 2850 | 4096 | 64 | 80000/800 (7040) | 40000/1000 | 8/16000 |
| Standard_M416ms_v2<sup>1</sup> | 416 | 11.400 | 8192 | 64 | 250.000/1.600 (14.080) | 80000/2000 | 8 / 32.000 |
| Standard_M416s_v2<sup>1</sup> | 416 | 5\.700 | 8192 | 64 | 250.000/1.600 (14.080) | 80000/2000 | 8 / 32.000 |

<sup>1</sup> VMs der Mv2-Serie sind nur als Generation 2 erhältlich. Wenn Sie Linux verwenden, finden Sie unter [Unterstützung für VMs der Generation 2 in Azure](./linux/generation-2.md) Anweisungen zum Suchen und Auswählen eines Images. Wenn Sie Windows verwenden, finden Sie unter [Unterstützung für VMs der Generation 2 in Azure](./windows/generation-2.md) Anweisungen zum Suchen und Auswählen eines Images. Die mindestens erforderlichen Betriebssystemversionen für alle vier verschiedenen VM-Typen der Mv2-Serie lauten wie folgt:

- Windows Server 2019 oder höher
- SUSE Linux Enterprise Server 12 SP4 oder höher, oder SUSE Linux Enterprise Server 15 SP1 oder höher
- Red Hat Enterprise Linux 7.6, 7.7, 8.1 oder höher 
- Oracle Enterprise Linux 7.7 oder höher



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
