---
title: include file
description: include file
services: virtual-machines
author: vermagit
ms.service: virtual-machines
ms.topic: include
ms.date: 04/26/2019
ms.author: azcspmt;jonbeck;cynthn;amverma
ms.custom: include file
ms.openlocfilehash: 489ac7fa37c10a27de971151f0be35c647d2186f
ms.sourcegitcommit: 598c5a280a002036b1a76aa6712f79d30110b98d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2019
ms.locfileid: "74116637"
---
Für HPC optimierte virtuelle Azure-Computer (Azure-VMs) bieten eine herausragende Leistung, MPI-Skalierbarkeit und Kosteneffizienz für verschiedene Anwendungen in der Praxis.
 
## <a name="infiniband-networking-for-large-scale-hpc"></a>InfiniBand-Netzwerk für HPC in großem Maßstab
HBv2-VMs unterstützen Mellanox HDR InfiniBand mit 200 Gbit/s, und HB- und HC-VMs unterstützen Mellanox EDR InfiniBand mit 100 Gbit/s. Jeder dieser VM-Typen ist für eine optimierte und konsistente RDMA-Leistung in einer FAT-Struktur ohne Blocks verbunden.

HBv2-VMs unterstützen adaptives Routing und DCT (Dynamic Connected Transport, zusätzlich zum standardmäßigen RC- und UD-Transport). Diese Features verbessern die Anwendungsleistung, Skalierbarkeit und Konsistenz, und ihre Verwendung wird dringend empfohlen.  

Die HBv2-, HB- und HC-VMs unterstützen Mellanox-/OFED-Standardtreiber. Alle RDMA-Verben und MPI-Typen werden unterstützt. Jeder dieser VM-Typen unterstützt zudem die hardwarebasierte Auslagerung von MPI Collective-Funktionen, um die Anwendungsleistung zu steigern.
 
Ursprüngliche VMs der H-Serie unterstützen Mellanox FDR InfiniBand mit 56 Gbit/s. Um die InfiniBand-Schnittstelle für die H-Serie nutzen zu können, müssen Kunden die Bereitstellung mit offiziell unterstützten Images für diesen VM-Typ aus dem Azure Marketplace durchzuführen. 


## <a name="hbv2-series"></a>HBv2-Serie
VMs der HBv2-Serie sind für Anwendungen mit hohen Anforderungen an die Speicherbandbreite optimiert, z. B. Strömungssimulationen, Finite-Element-Analysen oder Lagerstättensimulationen. HBv2-VMs bieten 120 AMD-Prozessorkerne des Modells EPYC 7742, 4 GB RAM pro CPU-Kern und kein gleichzeitiges Multithreading. Jede HBv2-VM bietet eine Speicherbandbreite von bis zu 340 Gbit/s und eine FP64-Computeleistung von bis zu 4 TeraFLOP. 

Storage Premium Unterstützt

| Size | vCPU | Prozessor | Arbeitsspeicher (GB) | Speicherbandbreite GB/s | Basis-CPU-Frequenz (GHz) | Frequenz für alle Kerne (GHz, Spitze) | Frequenz für Einzelkern (GHz, Spitze) | RDMA-Leistung (Gbit/s) | MPI-Unterstützung | Temporärer Speicher (GB) | Max. Anzahl Datenträger | Max. Ethernet-Karten |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_HB120rs | 120 | AMD EPYC 7742 | 480 | 350 | 2.45 | 2.45 | 3.4 | 200 | Alle | 480 + 960 | 8 | 1 |

<br>

## <a name="hb-series"></a>HB-Serie
VMs der HB-Serie sind für Anwendungen mit hohen Anforderungen an die Speicherbandbreite optimiert, z.B. Strömungssimulationen, explizite Finite-Element-Analysen oder Wettermodelle. HB-VMs bieten 60 AMD-Prozessorkerne des Modells EPYC 7551, 4 GB RAM pro CPU-Kern und kein gleichzeitiges Multithreading. Ein HB-VM unterstützt eine Speicherbandbreite von bis zu 260 Gbit/s.  

ACU: 199-216

Storage Premium Unterstützt

Storage Premium-Zwischenspeicherung: Unterstützt

| Size | vCPU | Prozessor | Arbeitsspeicher (GB) | Speicherbandbreite GB/s | Basis-CPU-Frequenz (GHz) | Frequenz für alle Kerne (GHz, Spitze) | Frequenz für Einzelkern (GHz, Spitze) | RDMA-Leistung (Gbit/s) | MPI-Unterstützung | Temporärer Speicher (GB) | Max. Anzahl Datenträger | Max. Ethernet-Karten |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_HB60rs | 60 | AMD EPYC 7551 | 240 | 263 | 2.0 | 2.55 | 2.55 | 100 | Alle | 700 | 4 | 1 |

<br>

## <a name="hc-series"></a>HC-Serie
Die VMs der HC-Serie sind für Anwendungen mit hohen Anforderungen an die Rechenleistung optimiert, z. B. implizite Finite-Element-Analysen, Molekulardynamik oder Chemoinformatik. HC-VMs bieten 44 Intel-Prozessorkerne des Modells Xeon Platinum 8168, 8 GB RAM pro CPU-Kern und kein Hyperthreading. Die Intel Xeon Platinum-Plattform unterstützt Intels umfangreiches Ökosystem von Softwaretools, etwa die Intel Math Kernel Library. 

ACU: 297-315

Storage Premium Unterstützt

Storage Premium-Zwischenspeicherung: Unterstützt


| Size | vCPU | Prozessor | Arbeitsspeicher (GB) | Speicherbandbreite GB/s | Basis-CPU-Frequenz (GHz) | Frequenz für alle Kerne (GHz, Spitze) | Frequenz für Einzelkern (GHz, Spitze) | RDMA-Leistung (Gbit/s) | MPI-Unterstützung | Temporärer Speicher (GB) | Max. Anzahl Datenträger | Max. Ethernet-Karten |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_HC44rs | 44 | Intel Xeon Platinum 8168 | 352 | 191 | 2.7 | 3.4 | 3,7 | 100 | Alle | 700 | 4 | 1 |


<br>

## <a name="h-series"></a>H-Reihe
Virtuelle Computer der H-Serie sind für Anwendungen optimiert, für die Anforderungen hinsichtlich hoher CPU-Frequenzen oder sehr viel Arbeitsspeicher pro Kern gelten. Virtuelle Computer der H-Serie bieten 8 oder 16 Intel-Prozessorkerne des Modells Xeon E5 2667 v3, bis zu 14 GB RAM pro CPU-Kern und kein Hyperthreading. Virtuelle Computer der H-Serie bieten eine Speicherbandbreite von bis zu 80 Gbit/s.

ACU: 290-300

Storage Premium  Nicht unterstützt

Storage Premium-Zwischenspeicherung:  Nicht unterstützt

| Size | vCPU | Prozessor | Arbeitsspeicher (GB) | Speicherbandbreite GB/s | Basis-CPU-Frequenz (GHz) | Frequenz für alle Kerne (GHz, Spitze) | Frequenz für Einzelkern (GHz, Spitze) | RDMA-Leistung (Gbit/s) | MPI-Unterstützung | Temporärer Speicher (GB) | Max. Anzahl Datenträger | Max. Ethernet-Karten |
| --- | --- |--- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_H8 | 8 | Intel Xeon E5 2667 v3 | 56 | 40 | 3.2 | 3.3 | 3.6 | - | Intel 5.x, MS-MPI | 1000 | 32 | 2 |
| Standard_H16 | 16 | Intel Xeon E5 2667 v3 | 112 | 80 | 3.2 | 3.3 | 3.6 |  - | Intel 5.x, MS-MPI | 2000 | 64 | 4 |
| Standard_H8m | 8 | Intel Xeon E5 2667 v3 | 112 | 40 | 3.2 | 3.3 | 3.6 | - | Intel 5.x, MS-MPI | 1000 | 32 | 2 |
| Standard_H16m | 16 | Intel Xeon E5 2667 v3 | 224 | 80 | 3.2 | 3.3 | 3.6 | - | Intel 5.x, MS-MPI | 2000 | 64 | 4 |
| Standard_H16r <sup>1</sup> | 16 | Intel Xeon E5 2667 v3 | 112 | 80 | 3.2 | 3.3 | 3.6 | 56 | Intel 5.x, MS-MPI | 2000 | 64 | 4 |
| Standard_H16mr <sup>1</sup> | 16 | Intel Xeon E5 2667 v3 | 224 | 80 | 3.2 | 3.3 | 3.6 | 56 | Intel 5.x, MS-MPI | 2000 | 64 | 4 |

<sup>1</sup> Für MPI-Anwendungen ist ein dediziertes RDMA-Back-End-Netzwerk durch ein FDR InfiniBand-Netzwerk aktiviert.

<br>
