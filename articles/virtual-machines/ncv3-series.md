---
title: 'NCv3-Serie: Azure Virtual Machines'
description: Spezifikationen für die VMs der NCv3-Serie
services: virtual-machines
author: jonbeck7
ms.service: virtual-machines
ms.topic: article
ms.date: 02/03/2020
ms.author: lahugh
ms.openlocfilehash: 18a2198063c1dfc5149c5ca017da3807da821dd1
ms.sourcegitcommit: 98a5a6765da081e7f294d3cb19c1357d10ca333f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/20/2020
ms.locfileid: "77492387"
---
# <a name="ncv3-series"></a>NCv3-Serie

NCv3-Serien-VMs werden mit [NVIDIA Tesla V100](https://www.nvidia.com/data-center/tesla-v100/)-GPUs betrieben. Diese GPUs können eine 1,5-mal so hohe Rechenleistung erzielen wie die NCv2-Serie. Kunden können diese neuen GPUs für herkömmliche HPC-Workloads wie Modellierung von Lagerstätten, DNA-Sequenzierung, Proteinanalysen, Monte Carlo-Simulationen und Ähnliches nutzen. Die NC24rs v3-Konfiguration bietet eine Netzwerkschnittstelle mit geringer Wartezeit und hohem Durchsatz, die sich ideal für die Verarbeitung eng gekoppelter paralleler Computingworkloads eignet. Zusätzlich zu den GPUs werden virtuelle Computer der NCv3-Serie mit Intel Xeon E5-2690 v4-CPUs (Broadwell) betrieben.

Storage Premium  Unterstützt

Storage Premium-Zwischenspeicherung:  Unterstützt

> [!IMPORTANT]
> Für diese VM-Serie ist das vCPU-Kontingent (Kernkontingent) in Ihrem Abonnement anfänglich in jeder Region auf 0 festgelegt. Sie können für diese Serie in einer [verfügbaren Region](https://azure.microsoft.com/regions/services/) eine [Anhebung des vCPU-Kontingents anfordern](../azure-supportability/resource-manager-core-quotas-request.md).
>
| Size | vCPU | Memory: GiB | Temporärer Speicher (SSD): GiB | GPU | GPU-Arbeitsspeicher: GiB | Max. Anzahl Datenträger | Maximaler Durchsatz des Datenträgers ohne Cache: IOPS/MBit/s | Maximale Anzahl NICs |
|---|---|---|---|---|---|---|---|---|
| Standard_NC6s_v3    | 6  | 112 | 736  | 1 | 16 | 12 | 20.000/200 | 4 |
| Standard_NC12s_v3   | 12 | 224 | 1474 | 2 | 32 | 24 | 40.000/400 | 8 |
| Standard_NC24s_v3   | 24 | 448 | 2948 | 4 | 64 | 32 | 80.000/800 | 8 |
| Standard_NC24rs_v3* | 24 | 448 | 2948 | 4 | 64 | 32 | 80.000/800 | 8 |

Eine GPU entspricht einer V100-Karte.

\* RDMA-fähig [!INCLUDE [virtual-machines-common-sizes-table-defs](../../includes/virtual-machines-common-sizes-table-defs.md)]

## <a name="supported-operating-systems-and-drivers"></a>Unterstützte Betriebssysteme und Treiber

Um die GPU-Funktionen von virtuellen Azure-Computern der N-Serie nutzen zu können, müssen NVIDIA-GPU-Treiber installiert werden.

Mit der [NVIDIA-GPU-Treibererweiterung](/extensions/hpccompute-gpu-windows.md) werden entsprechende NVIDIA-CUDA- oder GRID-Treiber auf einem virtuellen Computer der N-Serie installiert. Installieren oder verwalten Sie die Erweiterung mithilfe des Azure-Portals oder mit Tools wie Azure PowerShell oder Azure Resource Manager-Vorlagen. Informationen zu unterstützten Betriebssystemen und Bereitstellungsschritten finden Sie in der [Dokumentation zur NVIDIA-GPU-Treibererweiterung](/extensions/hpccompute-gpu-windows.md). Allgemeine Informationen zu VM-Erweiterungen finden Sie unter [Erweiterungen und Features für virtuelle Azure-Computer](/extensions/overview.md).

Wenn Sie NVIDIA-GPU-Treiber manuell installieren möchten, finden Sie Informationen zu unterstützten Betriebssystemen und Treibern sowie Schritte zur Installation und zur Überprüfung unter [Einrichten von GPU-Treibern der N-Serie für Windows](/windows/n-series-driver-setup.md) bzw. [Einrichten von GPU-Treibern der N-Serie für Linux](/linux/n-series-driver-setup).

## <a name="other-sizes"></a>Andere Größen

- [Allgemeiner Zweck](sizes-general.md)
- [Arbeitsspeicheroptimiert](sizes-memory.md)
- [Speicheroptimiert](sizes-storage.md)
- [GPU-optimiert](sizes-gpu.md)
- [High Performance Computing](sizes-hpc.md)
- [Vorherige Generationen](sizes-previous-gen.md)

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen dazu, wie Sie mit [Azure-Computeeinheiten (ACU)](acu.md) die Computeleistung von Azure-SKUs vergleichen können.