---
title: include file
description: include file
services: virtual-machines-windows, virtual-machines-linux
author: cynthn
ms.service: multiple
ms.topic: include
ms.date: 06/11/2019
ms.author: cynthn;azcspmt;jonbeck
ms.custom: include file
ms.openlocfilehash: 8f256749c363e2900fe62bda557f7cb4caa72e3e
ms.sourcegitcommit: 76bc196464334a99510e33d836669d95d7f57643
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/12/2020
ms.locfileid: "77179036"
---
GPU-optimierte VM-Größen sind für spezialisierte virtuelle Computer mit einzelnen oder mehreren NVIDIA-GPUs verfügbar. Diese Größen sind für rechenintensive, grafikintensive und visualisierungsorientierte Workloads vorgesehen. Dieser Artikel enthält Informationen über die Anzahlen und Typen von GPUs, vCPUs, Datenträgern und NICs. Der Speicherdurchsatz und die Netzwerkbandbreite sind für die jeweiligen Größen in dieser Gruppe ebenfalls enthalten.

* Die Größen **NC, NCv2 und NCv3** sind für rechen- und netzwerkintensive Anwendungen und Algorithmen optimiert. Einige Beispiele sind CUDA- und OpenCL-basierte Anwendungen und Simulationen, KI und Deep Learning. Die NCv3-Serie ist für High Performance Computing-Workloads, die mit der Tesla V100-GPU von NVIDIA ausgestattet sind, konzipiert. Virtuelle Computer der NC-Serie verwenden den Prozessor Intel Xeon E5-2690 v3 mit 2,60 GHz (Haswell) und virtuelle Computer der NCv2- und der NCv3-Serie den Prozessor Intel Xeon E5-2690 v4 (Broadwell).

* **ND und NDv2:** Die ND-Serie ist auf Trainings- und Rückschlussszenarien für Deep Learning spezialisiert. Für die Serie werden die NVIDIA Tesla P40-GPU und der Intel Xeon E5-2690 v4-Prozessor (Broadwell) verwendet. Für die NDv2-Serie wird der Intel Xeon Platinum 8168-Prozessor (Skylake) verwendet.

* Die Größen **NV und NVv3** sind für Remotevisualisierung, Streaming, Spiele, Codierung und VDI-Szenarien mit Frameworks wie OpenGL und DirectX optimiert und konzipiert.  Diese VMs werden von der NVIDIA Tesla M60-GPU unterstützt.

* Größen vom Typ **NVv4** sind für VDI und Remotevisualisierung optimiert und konzipiert. Dank partitionierter GPUs bietet NVv4 die passende Größe für Workloads, die kleinere GPU-Ressourcen erfordern.  Diese virtuellen Computer basieren auf der AMD Radeon Instinct MI25-GPU.


## <a name="nc-series"></a>NC-Serie

Storage Premium  Nicht unterstützt

Storage Premium-Zwischenspeicherung:  Nicht unterstützt

Virtuelle Computer der NC-Serie werden mit der [NVIDIA-Grafikkarte Tesla K80](https://www.nvidia.com/content/dam/en-zz/Solutions/Data-Center/tesla-product-literature/Tesla-K80-BoardSpec-07317-001-v05.pdf) und dem Prozessor Intel Xeon E5-2690 v3 (Haswell) betrieben. Benutzer können Daten schneller analysieren, indem sie CUDA für Anwendungen zur Energieuntersuchung, Absturzsimulationen, Rendering mit Raytracing, Deep Learning und mehr verwenden. Die NC24r-Konfiguration bietet eine Netzwerkschnittstelle mit geringer Wartezeit und hohem Durchsatz, die sich ideal für die Verarbeitung eng gekoppelter paralleler Computingworkloads eignet.

| Size | vCPU | Memory: GiB | Temporärer Speicher (SSD): GiB | GPU | GPU-Arbeitsspeicher: GiB | Max. Anzahl Datenträger | Maximale Anzahl NICs |
| --- | --- | --- | --- | --- | --- | --- | ---- |
| Standard_NC6 |6 |56 | 340 | 1 | 12 | 24 | 1 |
| Standard_NC12 |12 |112 | 680 | 2 | 24 | 48 | 2 |
| Standard_NC24 |24 |224 | 1440 | 4 | 48 | 64 | 4 |
| Standard_NC24r* |24 |224 | 1440 | 4 | 48 | 64 | 4 |

1 GPU = halbe K80-Karte

*RDMA-fähig

## <a name="ncv2-series"></a>NCv2-Serie

Storage Premium  Unterstützt

Storage Premium-Zwischenspeicherung:  Unterstützt

NCv2-Serien-VMs werden mit [NVIDIA Tesla P100](https://www.nvidia.com/en-us/data-center/tesla-p100/)-GPUs betrieben. Im Vergleich zur NC-Serie können diese GPUs eine mehr als doppelte Rechenleistung erzielen. Kunden können diese neuen GPUs für herkömmliche HPC-Workloads wie Modellierung von Lagerstätten, DNA-Sequenzierung, Proteinanalysen, Monte Carlo-Simulationen und Ähnliches nutzen. Zusätzlich zu den GPUs werden virtuelle Computer der NCv2-Serie mit Intel Xeon E5-2690 v4-CPUs (Broadwell) betrieben.

Die NC24rs v2-Konfiguration bietet eine Netzwerkschnittstelle mit geringer Wartezeit und hohem Durchsatz, die sich ideal für die Verarbeitung eng gekoppelter paralleler Computingworkloads eignet.

> [!IMPORTANT]
> Für diese Größenfamilie ist das vCPU-Kontingent (Kernkontingent) in Ihrem Abonnement anfänglich in jeder Region auf „0“ festgelegt. Für diese Familie können Sie in einer [verfügbaren Region](https://azure.microsoft.com/regions/services/) eine [Anhebung des vCPU-Kontingents anfordern](../articles/azure-portal/supportability/resource-manager-core-quotas-request.md).
>

| Size | vCPU | Memory: GiB | Temporärer Speicher (SSD): GiB | GPU | GPU-Arbeitsspeicher: GiB | Max. Anzahl Datenträger | Maximaler Durchsatz des Datenträgers ohne Cache: IOPS/MBps | Maximale Anzahl NICs |
| --- | --- | --- | --- | --- | --- | ---  | ---| --- |
| Standard_NC6s_v2 | 6 |112 | 736 | 1 | 16 | 12 | 20000/200 | 4 |
| Standard_NC12s_v2 | 12 |224 | 1474 | 2 | 32 | 24 | 40000/400 | 8 |
| Standard_NC24s_v2 | 24 |448 | 2948 | 4 | 64 | 32 | 80000/800 | 8 |
| Standard_NC24rs_v2* | 24 |448 | 2948 | 4 | 64 | 32 | 80000/800 | 8 |

Eine GPU entspricht einer P100-Karte.

*RDMA-fähig

## <a name="ncv3-series"></a>NCv3-Serie

Storage Premium  Unterstützt

Storage Premium-Zwischenspeicherung:  Unterstützt

NCv3-Serien-VMs werden mit [NVIDIA Tesla V100](https://www.nvidia.com/en-us/data-center/tesla-v100/)-GPUs betrieben. Diese GPUs können eine 1,5-mal so hohe Rechenleistung erzielen wie die NCv2-Serie. Kunden können diese neuen GPUs für herkömmliche HPC-Workloads wie Modellierung von Lagerstätten, DNA-Sequenzierung, Proteinanalysen, Monte Carlo-Simulationen und Ähnliches nutzen. Die NC24rs v3-Konfiguration bietet eine Netzwerkschnittstelle mit geringer Wartezeit und hohem Durchsatz, die sich ideal für die Verarbeitung eng gekoppelter paralleler Computingworkloads eignet. Zusätzlich zu den GPUs werden virtuelle Computer der NCv3-Serie mit Intel Xeon E5-2690 v4-CPUs (Broadwell) betrieben.

> [!IMPORTANT]
> Für diese Größenfamilie ist das vCPU-Kontingent (Kernkontingent) in Ihrem Abonnement anfänglich in jeder Region auf „0“ festgelegt. Für diese Familie können Sie in einer [verfügbaren Region](https://azure.microsoft.com/regions/services/) eine [Anhebung des vCPU-Kontingents anfordern](../articles/azure-portal/supportability/resource-manager-core-quotas-request.md).
>

| Size | vCPU | Memory: GiB | Temporärer Speicher (SSD): GiB | GPU | GPU-Arbeitsspeicher: GiB | Max. Anzahl Datenträger | Maximaler Durchsatz des Datenträgers ohne Cache: IOPS/MBps | Maximale Anzahl NICs |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_NC6s_v3 | 6 |112 | 736 | 1 | 16 | 12 | 20000/200 | 4 |
| Standard_NC12s_v3 | 12 |224 | 1474 | 2 | 32 | 24 | 40000/400 | 8 |
| Standard_NC24s_v3 | 24 |448 | 2948 | 4 | 64 | 32 | 80000/800 | 8 | 
| Standard_NC24rs_v3* |24 |448 | 2948 | 4 | 64 | 32 | 80000/800 | 8 |

Eine GPU entspricht einer V100-Karte.

*RDMA-fähig

## <a name="updated-ndv2-series-preview"></a>Aktualisierte NDv2-Serie (Vorschau)

Storage Premium  Unterstützt

Storage Premium-Zwischenspeicherung:  Unterstützt

InfiniBand: Unterstützt

Die VMs der NDv2-Serie sind ein neues Mitglied der GPU-Familie und darauf ausgelegt, die Anforderungen von besonders ressourcenintensiven Workloads zu erfüllen – beispielsweise KI-Workloads mit GPU-Beschleunigung, Machine Learning-, Simulations- und HPC-Workloads. 

Die VMs der NDv2-Serie sind mit 8 NVIDIA Tesla V100-GPUs mit NVLINK-Bus ausgestattet und umfassen jeweils 32 GB an GPU-Arbeitsspeicher. Jede NDv2-VM verfügt außerdem über 40 Intel Xeon Platinum 8168-Kerne (Skylake) ohne Hyperthreading und 672 GiB an Systemarbeitsspeicher. 

NDv2-Instanzen bieten dank CUDA GPU-optimierter Computekernel hervorragende Leistung für HPC- und KI-Workloads sowie für zahlreiche KI-, ML- und Analysetools mit integrierter Unterstützung der GPU-Beschleunigung. Dazu zählen beispielsweise TensorFlow, Pytorch, Caffe, RAPIDS und andere Frameworks. 

Entscheidend ist, dass die NDv2-Serie sowohl auf rechenintensive Workloads zum zentralen Hochskalieren (mit 8 GPUs pro VM) als auch auf das horizontalen Skalieren (mit mehreren kombinierten VMs) ausgelegt ist. Die NDv2-Serie unterstützt ab sofort Back-End-Netzwerke mit 100 Gigabit InfiniBand EDR- – ähnlich der Datenrate auf HPC-VMs der HB-Serie –, um Hochleistungsclustering für parallele Szenarien zu ermöglichen, z. B. für ein verteiltes Training für KI und ML. Dieses Back-End-Netzwerk unterstützt alle wichtigen InfiniBand-Protokolle – einschließlich derer, die in den NCCL2-Bibliotheken von NDVIA verwendet werden. Dadurch ist ein nahtloses Clustering von GPUs möglich.

> Bei [Aktivierung von InfiniBand](https://docs.microsoft.com/azure/virtual-machines/workloads/hpc/enable-infiniband) auf der ND40rs_v2-VM verwenden Sie bitte den Mellanox OFED-Treiber 4.7-1.0.0.1.

> Aufgrund des größeren GPU-Arbeitsspeichers werden für die neue ND40rs_v2-VM [VMs der 2. Generation](https://docs.microsoft.com/azure/virtual-machines/windows/generation-2) und Marketplace-Images benötigt. 

> [Registrieren Sie sich, um frühen Zugriff auf die Vorschauversion der NDv2-VM anzufordern.](https://aka.ms/AzureNDrv2Preview)

> Hinweis: Die ND40s_v2-VM mit 16 GB Arbeitsspeicher pro GPU ist nicht mehr als Vorschauversion verfügbar und wurde durch die aktualisierte ND40rs_v2-VM ersetzt.
<br>

| Size | vCPU | Memory: GiB | Temporärer Speicher (SSD): GiB | GPU | GPU-Arbeitsspeicher: GiB | Max. Anzahl Datenträger | Maximaler Durchsatz des Datenträgers ohne Cache: IOPS/MBps | Max. Netzwerkbandbreite | Maximale Anzahl NICs |
|---|---|---|---|---|---|---|---|---|---|
| Standard_ND40rs_v2 | 40 | 672 | 2948 | 8 V100 32 GB (NVLink) | 16 | 32 | 80000/800 | 24.000 MBit/s | 8 |

## <a name="nd-series"></a>ND-Serie

Storage Premium  Unterstützt

Storage Premium-Zwischenspeicherung:  Unterstützt

Die virtuellen Computer der ND-Serie sind eine neue Ergänzung der GPU-Familie und für Workloads in den Bereichen KI und Deep Learning konzipiert. Sie bieten eine ausgezeichnete Leistung für Training und Rückschluss. ND-Instanzen werden mit [NVIDIA Tesla P40](https://images.nvidia.com/content/pdf/tesla/184427-Tesla-P40-Datasheet-NV-Final-Letter-Web.pdf)-GPUs und Intel Xeon E5-2690 v4 (Broadwell)-CPUs betrieben. Diese Instanzen bieten eine ausgezeichnete Leistung für Gleitkommavorgänge mit einfacher Genauigkeit, für KI-Workloads mit Microsoft Cognitive Toolkit sowie für TensorFlow, Caffe und andere Frameworks. Die ND-Serie bietet auch einen wesentlich größeren GPU-Arbeitsspeicher (24 GB) und eignet sich somit für deutlich umfangreichere neurale Netzmodelle. Genau wie die NC-Serie bietet auch die ND-Serie eine Konfiguration mit einem sekundären, RDMA-basierten Netzwerk mit geringer Wartezeit und hohem Durchsatz sowie InfiniBand-Konnektivität, sodass Sie umfangreiche Trainingsaufträge über mehrere GPUs hinweg ausführen können.

> [!IMPORTANT]
> Für diese Größenfamilie ist das regionsspezifische vCPU-Kontingent (Kernkontingent) in Ihrem Abonnement anfänglich auf „0“ festgelegt. Für diese Familie können Sie in einer [verfügbaren Region](https://azure.microsoft.com/regions/services/) eine [Anhebung des vCPU-Kontingents anfordern](../articles/azure-portal/supportability/resource-manager-core-quotas-request.md).
>

| Size | vCPU | Memory: GiB | Temporärer Speicher (SSD): GiB | GPU | GPU-Arbeitsspeicher: GiB | Max. Anzahl Datenträger | Maximaler Durchsatz des Datenträgers ohne Cache: IOPS/MBps | Maximale Anzahl NICs |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_ND6s | 6 |112 | 736 | 1 | 24 | 12 | 20000/200 | 4 |
| Standard_ND12s | 12 |224 | 1474 | 2 | 48 | 24 | 40000/400 | 8 | 
| Standard_ND24s | 24 |448 | 2948 | 4 | 96 | 32 | 80000/800 | 8 |
| Standard_ND24rs* | 24 |448 | 2948 | 4 | 96 | 32 | 80000/800 | 8 |

Eine GPU entspricht einer P40-Karte.

*RDMA-fähig

## <a name="nv-series"></a>NV-Serie

Storage Premium  Nicht unterstützt

Storage Premium-Zwischenspeicherung:  Nicht unterstützt

Die virtuellen Computer der NV-Serie nutzen NVIDIA-GPUs vom Typ [Tesla M60](https://images.nvidia.com/content/tesla/pdf/188417-Tesla-M60-DS-A4-fnl-Web.pdf) sowie NVIDIA GRID-Technologie und ermöglichen die Verwendung beschleunigter Desktopanwendungen und virtueller Desktops, mit denen Kunden ihre Daten oder Simulationen visualisieren können. Benutzer können ihre grafikintensiven Workflows mit den NV-Instanzen visualisieren, um überragende Grafikfunktionen zu erhalten, und darüber hinaus Workloads mit einfacher Genauigkeit ausführen (beispielsweise Codierung und Rendering). Virtuelle Computer der NV-Serie werden außerdem mit Intel Xeon E5-2690 v3 (Haswell)-CPUs betrieben.

Alle GPUs in NV-Instanzen beinhalten eine GRID-Lizenz. Diese Lizenz bietet Ihnen die erforderliche Flexibilität für die Verwendung einer NV-Instanz als virtuelle Arbeitsstation für einen einzelnen Benutzer. Außerdem besteht für ein Szenario mit einer virtuellen Anwendung die Möglichkeit, dass 25 Benutzer gleichzeitig eine Verbindung mit dem virtuellen Computer herstellen.

| Size | vCPU | Memory: GiB | Temporärer Speicher (SSD): GiB | GPU | GPU-Arbeitsspeicher: GiB | Max. Anzahl Datenträger | Maximale Anzahl NICs | Virtuelle Arbeitsstationen | Virtuelle Anwendungen |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_NV6 |6 |56 |380 | 1 | 8 | 24 | 1 | 1 | 25 |
| Standard_NV12 |12 |112 |680 | 2 | 16 | 48 | 2 | 2 | 50 |
| Standard_NV24 |24 |224 |1440 | 4 | 32 | 64 | 4 | 4 | 100 |

1 GPU = halbe M60-Karte

## <a name="nvv3-series--sup1sup"></a>NVv3-Serie <sup>1</sup>

Storage Premium  Unterstützt

Storage Premium-Zwischenspeicherung:  Unterstützt

Die virtuellen Computer der NVv3-Serie verfügen über GPUs vom Typ [NVIDIA Tesla M60](https://images.nvidia.com/content/tesla/pdf/188417-Tesla-M60-DS-A4-fnl-Web.pdf) sowie NVIDIA GRID-Technologie mit Intel E5-2690 v4 (Broadwell)-CPUs. Diese virtuellen Computer wurden für GPU-beschleunigte Grafikanwendungen und virtuelle Desktops entwickelt, um Kunden die Datenvisualisierung, Ergebnissimulation, CAD oder das Rendering und Streaming von Inhalten zu erleichtern. Außerdem können diese virtuellen Computer Workloads mit einfacher Genauigkeit wie Codierung und Rendering ausführen. Virtuelle Computer der NVv3-Serie unterstützen Storage Premium und verfügen im Vergleich zur NV-Vorgängerserie über doppelt so viel Systemarbeitsspeicher (RAM).  

Alle GPUs in NVv3-Instanzen beinhalten eine GRID-Lizenz. Diese Lizenz bietet Ihnen die erforderliche Flexibilität für die Verwendung einer NV-Instanz als virtuelle Arbeitsstation für einen einzelnen Benutzer. Außerdem besteht für ein Szenario mit einer virtuellen Anwendung die Möglichkeit, dass 25 Benutzer gleichzeitig eine Verbindung mit dem virtuellen Computer herstellen.

| Size | vCPU | Memory: GiB | Temporärer Speicher (SSD): GiB | GPU | GPU-Arbeitsspeicher: GiB | Max. Anzahl Datenträger | Maximaler Durchsatz des Datenträgers ohne Cache: IOPS/MBps | Maximale Anzahl NICs | Virtuelle Arbeitsstationen | Virtuelle Anwendungen | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_NV12s_v3 |12 |112 |736 | 1 | 8 | 12 | 20000/200 | 4 | 1 | 25 |
| Standard_NV24s_v3 |24 |224 |1474 | 2 | 16 | 24 | 40000/400 | 8 | 2 | 50 |
| Standard_NV48s_v3 |48 |448 |2948 | 4 | 32 | 32 | 80000/800 | 8 | 4 | 100 |

1 GPU = halbe M60-Karte

<sup>1</sup> Virtuelle Computer der NVv3-Serie verfügen über Hyperthreading-Technologie von Intel.

## <a name="nvv4-series-preview--sup1sup"></a>NVv4-Serie (Vorschauversion) <sup>1</sup>

Storage Premium  Unterstützt

Storage Premium-Zwischenspeicherung:  Unterstützt

Die virtuellen Computer der NVv4-Serie basieren auf GPUs vom Typ [AMD Radeon Instinct MI25](https://www.amd.com/en/products/professional-graphics/instinct-mi25) sowie auf CPUs vom Typ AMD EPYC 7V12 (Rome). Mit der NVv4-Serie führt Azure virtuelle Computer mit partiellen GPUs ein. Wählen Sie den virtuellen Computer mit der passenden Größe für GPU-beschleunigte Grafikanwendungen und virtuelle Desktops aus – angefangen bei einer Achtel-GPU mit 2 GiB Framepuffer bis hin zu einer vollständigen GPU mit 16 GiB Framepuffer. Von virtuellen Computern der NVv4-Serie wird derzeit nur das Windows-Gastbetriebssystem unterstützt.

[Registrieren Sie sich, um diese virtuellen Computer in der Vorschauversion zu nutzen](https://aka.ms/nvv4signup).
<br>

| Size | vCPU | Memory: GiB | Temporärer Speicher (SSD): GiB | GPU | GPU-Arbeitsspeicher: GiB | Max. Anzahl Datenträger | Maximale Anzahl NICs |
| --- | --- | --- | --- | --- | --- | --- | --- | 
| Standard_NV4as_v4 |4 |14 |88 | 1/8 | 2 | 4 | 2 |
| Standard_NV8as_v4 |8 |28 |176 | 1/4 | 4 | 8 | 4 |
| Standard_NV16as_v4 |16 |56 |352 | 1/2 | 8 | 16 | 8 | 
| Standard_NV32as_v4 |32 |112 |704 | 1 | 16 | 32 | 8 | 



<sup>1</sup> Virtuelle Computer der NVv4-Serie verfügen über AMD-Technologie für gleichzeitiges Multithreading.

