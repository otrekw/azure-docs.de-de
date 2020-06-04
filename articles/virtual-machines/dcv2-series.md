---
title: DC-Serie – Azure Virtual Machines
description: Spezifikationen für die VMs der DC-Serie.
services: virtual-machines
author: susaxen
ms.service: virtual-machines
ms.topic: article
ms.date: 02/20/2020
ms.author: lahugh
ms.openlocfilehash: 431c1b1211165f43feb7fe1f93c73c2bf141e004
ms.sourcegitcommit: f57297af0ea729ab76081c98da2243d6b1f6fa63
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2020
ms.locfileid: "82871802"
---
# <a name="dcsv2-series"></a>DCsv2-Serie


Die DCsv2-Serie kann Ihnen beim Schutz der Vertraulichkeit und Integrität Ihrer Daten und Ihres Codes während der Verarbeitung in der öffentlichen Cloud helfen. Diese Computer werden von der neuesten Generation von Intel XEON E-2288G-Prozessoren mit SGX-Technologie unterstützt. Mit der Intel Turbo Boost Technology können diese Computer bis zu 5,0 GHz erreichen. Instanzen der DCsv2-Serie ermöglichen es Kunden, Secure Enclave-basierte Anwendungen zu erstellen, um ihren Code und Daten während der Verwendung zu schützen.

Beispiele für Anwendungsfälle sind Freigabe von vertraulichen Mehrparteiendaten, Betrugserkennung, Geldwäschebekämpfung, Blockchain, vertrauliche Nutzungsanalyse, Business Intelligence-Analyse und vertrauliches maschinelles Lernen.

Storage Premium Unterstützt*

Storage Premium-Zwischenspeicherung: Unterstützt*

Livemigration: Nicht unterstützt

Updates mit Speicherbeibehaltung: Nicht unterstützt

\* Außer für Standard_DC8_v2



| Size             | vCPU | Memory: GiB | Temporärer Speicher (SSD): GiB | Max. Anzahl Datenträger | Maximaler Durchsatz (Cache und temporärer Speicher): IOPS/MBps (Cachegröße in GiB) | Maximaler Durchsatz des Datenträgers ohne Cache: IOPS/MBps | Maximale Anzahl NICs/Erwartete Netzwerkbandbreite (MBit/s) | EPC-Speicher (MiB) |
|------------------|------|-------------|------------------------|----------------|-------------------------------------------------------------------------|-------------------------------------------|----------------------------------------------|---------------------|
| Standard_DC1s_v2 | 1    | 4           | 50                     | 1              | 2000/16 (21)                                                            | 1600/24                                   | 2   | 28                                      |
| Standard_DC2s_v2 | 2    | 8           | 100                    | 2              | 4000/32 (43)                                                            | 3200/48                                   | 2  | 56                                          |
| Standard_DC4s_v2 | 4    | 16          | 200                    | 4              | 8000/64 (86)                                                            | 6400/96                                   | 2  | 112                                          |
| Standard_DC8_v2  | 8   | 32          | 400                    | 8              | 16000/128 (172)                                                         | 12800/192                                 | 2   | 168                                         |

- Virtuelle Computer der DCsv2-Serie sind [VMs der Generation 2](./linux/generation-2.md#creating-a-generation-2-vm) und unterstützen nur `Gen2`-Images.
- Derzeit nur in den Regionen „Vereinigtes Königreich, Süden“, „Kanada, Mitte“ und „USA, Osten“ verfügbar.
- Vorherige Generation vertraulicher Compute-VMs: [DC-Serie](sizes-previous-gen.md#preview-dc-series)
- Erstellen Sie DCsv2-VMs mithilfe von [Azure-Portal](./linux/quick-create-portal.md) oder [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-azure-compute.acc-virtual-machine-v2?tab=overview).



## <a name="other-sizes"></a>Andere Größen

- [Allgemeiner Zweck](sizes-general.md)
- [Arbeitsspeicheroptimiert](sizes-memory.md)
- [Speicheroptimiert](sizes-storage.md)
- [GPU-optimiert](sizes-gpu.md)
- [High Performance Computing](sizes-hpc.md)
- [Vorherige Generationen](sizes-previous-gen.md)

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen dazu, wie Sie mit [Azure-Computeeinheiten (ACU)](acu.md) die Computeleistung von Azure-SKUs vergleichen können.
