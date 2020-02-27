---
title: Übersicht über die Azure-Computeeinheit | Microsoft-Dokumentation
description: Übersicht über das Konzept der Azure-Computeeinheiten. Mithilfe der ACU kann die CPU-Leistung von Azure-SKUs verglichen werden.
services: virtual-machines
documentationcenter: ''
author: jonbeck7
manager: gwallace
editor: ''
tags: azure-resource-manager,azure-service-management
ms.service: virtual-machines
ms.devlang: na
ms.topic: article
ms.workload: infrastructure-services
ms.date: 02/03/2020
ms.author: jonbeck
ms.openlocfilehash: 8e406aea10c15a97a7e66bbdc3c1d889c5bd2c52
ms.sourcegitcommit: 98a5a6765da081e7f294d3cb19c1357d10ca333f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/20/2020
ms.locfileid: "77492647"
---
# <a name="azure-compute-unit-acu"></a>Azure-Computeeinheit (ACU)

Das Konzept der Azure-Compute-Einheit (Azure Compute Unit, ACU) bietet eine Möglichkeit zum Vergleichen der Rechenleistung (CPU) zwischen den Azure-SKUs. Auf diese Weise können Sie leicht feststellen, welche SKU Ihren Leistungsanforderungen am ehesten entspricht. Zurzeit ist der ACU-Wert auf einem kleinen virtuellen Computer (Standard_A1) auf den Standardwert 100 festgelegt, und an den übrigen SKUs kann ungefähr abgelesen werden, wie viel schneller die jeweilige SKU einen Standard-Benchmarktest ausführen kann.

> [!IMPORTANT]
> Die ACU ist nur ein Richtwert. Die Ergebnisse für Ihre Workload können abweichen.
<br>

| SKU-Familie | ACU\vCPU | vCPU: Core |
| --- | --- |---|
| [A0](sizes-previous-gen.md) |50 | 1:1 |
| [A1–A4](sizes-previous-gen.md) |100 | 1:1 |
| [A5–A7](sizes-previous-gen.md) |100 | 1:1 |
| [A1_v2–A8_v2](sizes-general.md) |100 | 1:1 |
| [A2m_v2–A8m_v2](sizes-general.md) |100 | 1:1 |
| [A8–A11](sizes-previous-gen.md) |225* | 1:1 |
| [D1–D14](sizes-previous-gen.md) |160–250 | 1:1 |
| [D1_v2–D15_v2](dv2-dsv2-series.md) |210 - 250* | 1:1 |
| [DS1–DS14](sizes-previous-gen.md) |160–250 | 1:1 |
| [DS1_v2–DS15_v2](dv2-dsv2-series.md) |210 - 250* | 1:1 |
| [D_v3](dv3-dsv3-series.md) |160 - 190* | 2:1\*\*\* |
| [Ds_v3](dv3-dsv3-series.md) |160 - 190* | 2:1\*\*\* |
| [E_v3](ev3-esv3-series.md) |160 - 190* | 2:1\*\*\*|
| [Es_v3](ev3-esv3-series.md) |160 - 190* | 2:1\*\*\* |
| [F2s_v2–F72s_v2](fsv2-series.md) |195–210* | 2:1\*\*\* |
| [F1–F16](sizes-previous-gen.md) |210 - 250* | 1:1 |
| [F1s–F16s](sizes-previous-gen.md) |210 - 250* | 1:1 |
| [G1–G5](sizes-previous-gen.md) |180 - 240* | 1:1 |
| [GS1–GS5](sizes-previous-gen.md) |180 - 240* | 1:1 |
| [H](h-series.md) |290 - 300* | 1:1 |
| [HB](hb-series.md) |199 - 216** | 1:1 |
| [HC](hc-series.md) |297 - 315* | 1:1 |
| [L4s–L32s](sizes-previous-gen.md) |180 - 240* | 1:1 |
| [L8s_v2–L80s_v2](lsv2-series.md) |150–175** | 2:1 |
| [M](m-series.md) | 160–180 | 2:1\*\*\* |

*ACUs verwenden die Intel® Turbo-Technologie, um die CPU-Frequenz zu erhöhen und eine Leistungssteigerung zu erzielen.  Das Maß der Leistungssteigerung variiert basierend auf der Größe und Workload des virtuellen Computers sowie auf anderen Workloads, die auf dem gleichen Host ausgeführt werden.
**ACUs verwenden die AMD® Boost-Technologie, um die CPU-Frequenz zu erhöhen und eine Leistungssteigerung zu erzielen.  Das Maß der Leistungssteigerung variiert basierend auf der Größe und Workload des virtuellen Computers sowie auf anderen Workloads, die auf dem gleichen Host ausgeführt werden.
***Mit Hyperthreading und der Möglichkeit zum Ausführen geschachtelter Virtualisierungen

Es folgen Links für weitere Informationen zu den verschiedenen Größen:

- [Allgemeiner Zweck](sizes-general.md)
- [Arbeitsspeicheroptimiert](sizes-memory.md)
- [Computeoptimiert](sizes-compute.md)
- [GPU-optimiert](sizes-gpu.md)
- [High Performance Computing](sizes-hpc.md)
- [Speicheroptimiert](sizes-storage.md)