---
title: DC-Serie – Azure Virtual Machines
description: Spezifikationen für die VMs der DC-Serie.
author: susaxen
ms.service: virtual-machines
ms.subservice: sizes
ms.topic: article
ms.date: 02/20/2020
ms.author: jushiman
ms.openlocfilehash: e5a4c9d1afcc4caa0f9ac0db8b59274006ab3097
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "84676555"
---
# <a name="dcsv2-series"></a>DCsv2-Serie


Die DCsv2-Serie kann Ihnen beim Schutz der Vertraulichkeit und Integrität Ihrer Daten und Ihres Codes während der Verarbeitung in der öffentlichen Cloud helfen. Diese Computer werden von der neuesten Generation von Intel XEON E-2288G-Prozessoren mit SGX-Technologie unterstützt. Mit der Intel Turbo Boost Technology können diese Computer bis zu 5,0 GHz erreichen. Instanzen der DCsv2-Serie ermöglichen es Kunden, Secure Enclave-basierte Anwendungen zu erstellen, um ihren Code und Daten während der Verwendung zu schützen.

Beispiele für Anwendungsfälle sind Freigabe von vertraulichen Mehrparteiendaten, Betrugserkennung, Geldwäschebekämpfung, Blockchain, vertrauliche Nutzungsanalyse, Business Intelligence-Analyse und vertrauliches maschinelles Lernen.

Storage Premium Unterstützt*

Storage Premium-Zwischenspeicherung: Unterstützt*

Livemigration: Nicht unterstützt

Updates mit Speicherbeibehaltung: Nicht unterstützt

\* Außer für Standard_DC8_v2



| Size             | vCPU | Memory: GiB | Temporärer Speicher (SSD): GiB | Max. Anzahl Datenträger | Maximaler Durchsatz (Cache und temporärer Speicher): IOPS/MBps (Cachegröße in GiB) | Maximale Anzahl NICs/Erwartete Netzwerkbandbreite (MBit/s) | EPC-Speicher (MiB) |
|------------------|------|-------------|------------------------|----------------|-------------------------------------------------------------------------|----------------------------------------------|---------------------|
| Standard_DC1s_v2 | 1    | 4           | 50                     | 1              | 2\.000/16                                                                                               | 2   | 28                                      |
| Standard_DC2s_v2 | 2    | 8           | 100                    | 2              | 4\.000/32                                                                                               | 2  | 56                                          |
| Standard_DC4s_v2 | 4    | 16          | 200                    | 4              | 8\.000/64                                                                                               | 2  | 112                                          |
| Standard_DC8_v2  | 8   | 32          | 400                    | 8              | 16.000/128                                                                                         | 2   | 168                                         |

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
