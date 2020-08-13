---
title: 'Ev4- und Esv4-Serie: Azure Virtual Machines'
description: Hier finden Sie Spezifikationen für die virtuellen Computer der Ev4- und Esv4-Serie.
author: brbell
ms.author: brbell
ms.reviewer: cynthn
ms.custom: mimckitt
ms.service: virtual-machines
ms.subservice: sizes
ms.topic: conceptual
ms.date: 6/8/2020
ms.openlocfilehash: aa7e773cb45ea23d0e091ce12cc8488114ddfbdf
ms.sourcegitcommit: 2ff0d073607bc746ffc638a84bb026d1705e543e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/06/2020
ms.locfileid: "87837056"
---
# <a name="ev4-and-esv4-series"></a>Ev4- und Esv4-Serie

Die Ev4- und Esv4-Serie werden auf den Prozessoren Intel&reg; Xeon&reg; Platinum 8272CL (Cascade Lake) in einer Hyperthreadingkonfiguration ausgeführt, eignen sich ideal für verschiedene arbeitsspeicherintensive Unternehmensanwendungen und unterstützen zu bis zu 504 GiB RAM.

> [!NOTE]
> Häufig gestellte Fragen finden Sie unter [Azure-VM-Größen ohne lokale temporäre Datenträger](azure-vms-no-temp-disk.md).

## <a name="ev4-series"></a>Ev4-Serie

Die Größen der Ev4-Serie laufen auf dem Intel Xeon&reg; Platinum 8272CL (Cascade Lake). Instanzen der Ev4-Serie eignen sich ideal für arbeitsspeicherintensive Unternehmensanwendungen. Virtuelle Computer der Ev4-Serie verfügen über Hyperthreading-Technologie von Intel&reg;.

Speicher für Remotedatenträger wird separat zu virtuellen Computern abgerechnet. Verwenden Sie die Esv4-Größen, um Datenträger mit Premium-Speicher zu nutzen. Die Preis- und Abrechnungskennzahlen für die Esv4-Größen entsprechen denen der Ev4-Serie.

> [!IMPORTANT]
> Diese neuen Größen sind zurzeit nur als Public Preview verfügbar. Sie können sich [hier](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR_Y3toRKxchLjARedqtguBRURE1ZSkdDUzg1VzJDN0cwWUlKTkcyUlo5Mi4u) für diese Ev4-und Esv4-Serie registrieren. 

ACU: 195–210

Storage Premium  Nicht unterstützt

Storage Premium-Zwischenspeicherung:  Nicht unterstützt

Livemigration: Unterstützt

Updates mit Speicherbeibehaltung: Unterstützt

| Size | vCPU | Memory: GiB | Temporärer Speicher (SSD): GiB | Max. Anzahl Datenträger | Maximale Anzahl NICs|Erwartete Netzwerkbandbreite (MBit/s) |
|---|---|---|---|---|---|---|
| Standard_E2_v4  | 2 | 16   | Nur Remotespeicher | 4 | 2|1000  |
| Standard_E4_v4  | 4 | 32  | Nur Remotespeicher | 8 | 2|2000  |
| Standard_E8_v4  | 8 | 64 | Nur Remotespeicher | 16 | 4|4000 |
| Standard_E16_v4 | 16 | 128 | Nur Remotespeicher | 32 | 8|8\.000 |
| Standard_E20_v4 | 20 | 160 | Nur Remotespeicher | 32 | 8|10000 |
| Standard_E32_v4 | 32 | 256 | Nur Remotespeicher | 32 | 8|16000 |
| Standard_E48_v4 | 48 | 384 | Nur Remotespeicher | 32 | 8|24.000 |
| Standard_E64_v4 | 64 | 504 | Nur Remotespeicher | 32| 8|30.000 |


## <a name="esv4-series"></a>Esv4-Serie

Die Größen der Esv4-Serie laufen auf dem Intel&reg; Xeon&reg; Platinum 8272CL (Cascade Lake). Instanzen der Esv4-Serie eignen sich ideal für arbeitsspeicherintensive Unternehmensanwendungen. Virtuelle Computer der Evs4-Serie verfügen über Hyperthreading-Technologie von Intel&reg;. Speicher für Remotedatenträger wird separat zu virtuellen Computern abgerechnet.

> [!IMPORTANT]
> Diese neuen Größen sind zurzeit nur als Public Preview verfügbar. Sie können sich [hier](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR_Y3toRKxchLjARedqtguBRURE1ZSkdDUzg1VzJDN0cwWUlKTkcyUlo5Mi4u) für diese Ev4-und Esv4-Serie registrieren. 

ACU: 195-210

Storage Premium  Unterstützt

Storage Premium-Zwischenspeicherung:  Unterstützt

Livemigration: Unterstützt

Updates mit Speicherbeibehaltung: Unterstützt

| Size | vCPU | Memory: GiB | Temporärer Speicher (SSD): GiB | Max. Anzahl Datenträger | Maximaler zwischengespeicherter Durchsatz: IOPS/MBit/s (Cachegröße in GiB) | Maximaler Durchsatz des Datenträgers ohne Cache: IOPS/MBit/s | Maximale Anzahl NICs|Erwartete Netzwerkbandbreite (MBit/s) |
|---|---|---|---|---|---|---|---|---|
| Standard_E2s_v4  | 2 | 16  | Nur Remotespeicher | 4 | 19.000/120 (50) | 3200/48 | 2|1000  |
| Standard_E4s_v4  | 4 | 32  | Nur Remotespeicher | 8 | 38500/242 (100) | 6400/96 | 2|2000  |
| Standard_E8s_v4  | 8 | 64  | Nur Remotespeicher | 16 | 77000/485 (200) | 12800/192 | 4|4000 |
| Standard_E16s_v4 | 16 | 128 | Nur Remotespeicher | 32 | 154000/968 (400) | 25600/384 | 8|8\.000 |
| Standard_E20s_v4 | 20 | 160 | Nur Remotespeicher | 32 | 193.000/1.211 (500) | 32000/480  | 8|10000 |
| Standard_E32s_v4 | 32 | 256 | Nur Remotespeicher | 32 | 308000/1936 (800) | 51200/768  | 8|16000 |
| Standard_E48s_v4 | 48 | 384 | Nur Remotespeicher | 32 | 462000/2904 (1200) | 76800/1152 | 8|24.000 |
| Standard_E64s_v4 <sup>1</sup> | 64 | 504| Nur Remotespeicher | 32 | 615000/3872 (1600) | 80000/1200 | 8|30.000 |

<sup>1</sup> [Eingeschränkte Kerngrößen verfügbar](./windows/constrained-vcpu.md).

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