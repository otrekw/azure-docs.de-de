---
title: Msv2-Serie (Vorschau) – Azure Virtual Machines
description: Hier finden Sie Spezifikationen für VMs der Msv2-Serie.
author: ayshakeen
ms.service: virtual-machines
ms.subservice: vm-sizes-memory
ms.topic: conceptual
ms.date: 04/07/2020
ms.author: jushiman
ms.openlocfilehash: a7f4757467523837423d52998eb6b8204090e627
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "102562570"
---
# <a name="msv2-and-mdsv2-series-medium-memory-preview"></a>Msv2- und Mdsv2-Serie mit mittlerem Arbeitsspeicher (Vorschau)


> [!IMPORTANT]
> Wenn Sie an der Vorschau teilnehmen möchten, füllen Sie das Formular unter **https://aka.ms/Mv2MedMemoryPreview** aus.  

Die VM-Serien „Msv2“ und „Mdsv2“ mit mittlerem Arbeitsspeicher verfügen über einen Intel® Xeon® Platinum 8280-Prozessor (Cascade Lake) mit einer Kernbasisfrequenz von 2,7 GHz und einer maximalen Einzelkern-Turbofrequenz von 4,0 GHz. Mit diesen VMs erhalten Kunden größere Flexibilität durch Optionen mit lokalen Datenträgern und ohne Datenträger. Außerdem erhalten Kunden Zugriff auf eine Reihe neuer isolierter VM-Größen mit mehr CPU-Leistung und Arbeitsspeicher, die bis zu 192 vCPUs mit 4 TiB Arbeitsspeicher reichen. 


VMs der Msv2- und Mdsv2-Serie sind nur als Generation 2 erhältlich und unterstützen eine Teilmenge der unterstützten Images der Generation 2. Unten finden Sie eine vollständige Liste der unterstützten Images für die Msv2- und Mdsv2-Serie.  

- Windows Server 2019 oder höher
- SUSE Linux Enterprise Server 12 SP4 oder höher, oder SUSE Linux Enterprise Server 15 SP1 oder höher
- Red Hat Enterprise Linux 7.6, 7.7, 8.1 oder höher 
- Oracle Enterprise Linux 7.7 oder höher

Weitere Informationen zu VMs der Generation 2 finden Sie unter [Unterstützung für VMs der Generation 2 in Azure](./generation-2.md).



[Storage Premium:](premium-storage-performance.md) Unterstützt<br>
[Storage Premium-Zwischenspeicherung:](premium-storage-performance.md) Unterstützt<br>
[Livemigration:](maintenance-and-updates.md) Nicht unterstützt<br>
[Updates mit Speicherbeibehaltung:](maintenance-and-updates.md) Nicht unterstützt<br>
[Unterstützung von VM-Generationen:](generation-2.md) Generation 2<br>
[Schreibbeschleunigung:](./how-to-enable-write-accelerator.md) Unterstützt<br>
[Beschleunigter Netzwerkbetrieb](../virtual-network/create-vm-accelerated-networking-cli.md): Unterstützt<br>
[Kurzlebige Betriebssystemdatenträger:](ephemeral-os-disks.md) Nicht unterstützt <br>
<br>
 
## <a name="msv2-medium-memory-diskless"></a>Msv2 mit mittlerem Arbeitsspeicher – ohne Datenträger 

| Size | vCPU | Memory: GiB | Temporärer Speicher (SSD): GiB | Max. Anzahl Datenträger | Maximaler Durchsatz des Datenträgers ohne Cache: IOPS/MBit/s | Maximale Anzahl NICs | Erwartete Netzwerkbandbreite (MBit/s) | 
|---|---|---|---|---|---|---|---|
| Standard_M32ms_v2 | 32 | 875 | 0 | 32 |  20.000/500 | 8 | 8.000 | 
| Standard_M64s_v2 | 64 | 1024 | 0 | 64 | 40000/1000 | 8 | 16000 | 
| Standard_M64ms_v2 | 64 | 1792 | 0 | 64 | 40000/1000 | 8 | 16000 | 
| Standard_M128s_v2 | 128 | 2048 | 0 | 64 | 80.000/2.000 | 8 | 30.000 | 
| Standard_M128ms_v2 | 128 | 3.892 | 0 | 64 | 80.000/2.000 | 8 | 30.000 | 
| Standard_M192is_v2 | 192 | 2048 | 0 | 64 | 80.000/2.000 | 8 | 30.000 | 
| Standard_M192ims_v2 | 192 | 4096 | 0 | 64 | 80.000/2.000 | 8 | 30.000 | 

## <a name="mdsv2-medium-memory-with-disk"></a>Mdsv2 mit mittlerem Arbeitsspeicher – mit Datenträger  

| Size | vCPU | Memory: GiB | Temporärer Speicher (SSD): GiB | Max. Anzahl Datenträger | Maximaler Durchsatz (Cache und temporärer Speicher): IOPS/MBit/s | Maximaler Durchsatz des Datenträgers ohne Cache: IOPS/MBit/s | Maximale Anzahl NICs | Erwartete Netzwerkbandbreite (MBit/s) | 
|---|---|---|---|---|---|---|---|---|
| Standard_M32dms_v2 | 32 | 875 | 1024 | 32 | 40000/400 | 20.000/500 | 8 | 8.000 | 
| Standard_M64ds_v2 | 64 | 1024 | 2048 | 64 | 80000/800 | 40000/1000 | 8 | 16000 | 
| Standard_M64dms_v2 | 64 | 1792 | 2048 | 64 | 80000/800 | 40000/1000 | 8 | 16000 | 
| Standard_M128ds_v2 | 128 | 2048 | 4096 | 64 |160000/1600 | 80.000/2.000 | 8 | 30.000 | 
| Standard_M128dms_v2 | 128 | 3.892 | 4096 | 64 | 160000/1600 | 80.000/2.000 | 8 | 30.000 | 
| Standard_M192ids_v2 | 192 | 2048 | 4096 | 64 | 160000/1600 | 80.000/2.000 | 8 | 30.000 | 
| Standard_M192idms_v2 | 192 | 4096 | 4096 | 64 | 160000/1600 | 80.000/2.000 | 8 | 30.000 | 


[!INCLUDE [virtual-machines-common-sizes-table-defs](../../includes/virtual-machines-common-sizes-table-defs.md)]

## <a name="other-sizes-and-information"></a>Weitere Größen und Informationen

- [Allgemeiner Zweck](sizes-general.md)
- [Arbeitsspeicheroptimiert](sizes-memory.md)
- [Speicheroptimiert](sizes-storage.md)
- [GPU-optimiert](sizes-gpu.md)
- [High Performance Computing](sizes-hpc.md)
- [Vorherige Generationen](sizes-previous-gen.md)

Preisrechner: [Preisrechner](https://azure.microsoft.com/pricing/calculator/)

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen dazu, wie Sie mit [Azure-Computeeinheiten (ACU)](acu.md) die Computeleistung von Azure-SKUs vergleichen können.
