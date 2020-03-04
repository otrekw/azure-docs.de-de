---
title: DC-Serie – Azure Virtual Machines
description: Spezifikationen für die VMs der DC-Serie.
services: virtual-machines
author: jonbeck7
ms.service: virtual-machines
ms.topic: article
ms.date: 02/20/2020
ms.author: lahugh
ms.openlocfilehash: c4e141b7854925f5d12afce19481a6e9c2f8dd1d
ms.sourcegitcommit: 99ac4a0150898ce9d3c6905cbd8b3a5537dd097e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/25/2020
ms.locfileid: "77599611"
---
# <a name="preview-dcv2-series"></a>Vorschau: DCv2-Serie


Die DCv2-Serie kann Ihnen beim Schutz der Vertraulichkeit und Integrität Ihrer Daten und Ihres Codes während der Verarbeitung in der öffentlichen Cloud helfen. Diese Computer werden von der neuesten Generation von Intel XEON E-2288G-Prozessoren mit SGX-Technologie unterstützt. Mit der Intel Turbo Boost Technology können diese Computer bis zu 5,0 GHz erreichen. Instanzen der DCv2-Serie ermöglichen es Kunden, Secure Enclave-basierte Anwendungen zu erstellen, um ihren Code und Daten zu schützen, während sie verwendet werden.

Beispiele für Anwendungsfälle sind Freigabe von vertraulichen Mehrparteiendaten, Betrugserkennung, Geldwäschebekämpfung, Blockchain, vertrauliche Nutzungsanalyse, Business Intelligence-Analyse und vertrauliches maschinelles Lernen.

Storage Premium Unterstützt*

Storage Premium-Zwischenspeicherung: Unterstützt*

\* Außer für Standard_DC8_v2



| Size             | vCPU | Memory: GiB | Temporärer Speicher (SSD): GiB | Max. Anzahl Datenträger | Maximaler Durchsatz (Cache und temporärer Speicher): IOPS/MBps (Cachegröße in GiB) | Maximaler Durchsatz des Datenträgers ohne Cache: IOPS/MBps | Maximale Anzahl NICs/Erwartete Netzwerkbandbreite (MBit/s) |
|------------------|------|-------------|------------------------|----------------|-------------------------------------------------------------------------|-------------------------------------------|----------------------------------------------|
| Standard_DC1s_v2 | 1    | 4           | 50                     | 1              | 2000/16 (21)                                                            | 1600/24                                   | 2                                            |
| Standard_DC2s_v2 | 2    | 8           | 100                    | 2              | 4\.000/32 (43)                                                            | 3200/48                                   | 2                                            |
| Standard_DC4s_v2 | 4    | 16          | 200                    | 4              | 8\.000/64 (86)                                                            | 6400/96                                   | 2                                            |
| Standard_DC8_v2  | 8   | 32          | 400                    | 8              | 16.000/128 (172)                                                         | 12800/192                                 | 2                                            |

- Virtuelle Computer der DCv2-Serie sind [VMs der Generation 2](./linux/generation-2.md#creating-a-generation-2-vm) und unterstützen nur `Gen2`-Images.




## <a name="other-sizes"></a>Andere Größen

- [Allgemeiner Zweck](sizes-general.md)
- [Arbeitsspeicheroptimiert](sizes-memory.md)
- [Speicheroptimiert](sizes-storage.md)
- [GPU-optimiert](sizes-gpu.md)
- [High Performance Computing](sizes-hpc.md)
- [Vorherige Generationen](sizes-previous-gen.md)

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen dazu, wie Sie mit [Azure-Computeeinheiten (ACU)](acu.md) die Computeleistung von Azure-SKUs vergleichen können.