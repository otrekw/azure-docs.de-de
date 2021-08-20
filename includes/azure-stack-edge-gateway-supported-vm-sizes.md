---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 07/01/2021
ms.author: alkohli
ms.openlocfilehash: c772d2722c90cefb99c9f2fdd111e2d654d11659
ms.sourcegitcommit: 6bd31ec35ac44d79debfe98a3ef32fb3522e3934
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2021
ms.locfileid: "113280198"
---
Die VM-Größe bestimmt den Umfang der Computeressourcen (CPU, GPU und Arbeitsspeicher), die für die VM zur Verfügung gestellt werden. Sie müssen VMs mit einer für die Workload geeigneten Größe erstellen. Auch wenn alle VMs auf derselben Hardware ausgeführt werden, gibt es bei den VM-Größen unterschiedliche Grenzwerte für den Datenträgerzugriff. Dies kann Ihnen helfen, den gesamten Datenträgerzugriff auf allen Ihren VMs zu verwalten. Wenn eine Workload zunimmt, können Sie auch die Größe einer vorhandenen VM ändern.

Die folgenden VMs werden für die Erstellung auf Ihrem Azure Stack Edge-Gerät unterstützt.

### <a name="dv2-series"></a>Dv2-Serie
| Size             | vCPU  | Arbeitsspeicher (GiB)  | Temporärer Speicher (GiB)  | Max. Durchsatz des Betriebssystem-Datenträgers (IOPS)  | Maximaler Durchsatz (temporärer Speicher): (IOPS)  | Max. Datenträgerdurchsatz: (IOPS)  | Max. Anzahl Datenträger  | Maximale Anzahl NICs  |
|------------------|-------|---------------|---------------------|--------------------------------|-------------------------------------|----------------------------------|-----------------|-----------|
| **Standard_D1_v2**   | 1     | 3,5           | 50                  | 1000                           | 3000                                | 500                              | 4               | 2         |
| **Standard_D2_v2**   | 2     | 7             | 100                 | 1000                           | 6000                                | 500                              | 8               | 4         |
| **Standard_D3_v2**   | 4     | 14            | 200                 | 1000                           | 12000                               | 500                              | 16              | 4         |
| **Standard_D4_v2**   | 8     | 28            | 400                 | 1000                           | 24.000                               | 500                              | 32              | 8         |
| **Standard_D5_v2**   | 16    | 56            | 800                 | 1000                           | 48000                               | 500                              | 64              | 8         |
| **Standard_D11_v2**  | 2     | 14            | 100                 | 1000                           | 6000                                | 500                              | 8               | 2         |
| **Standard_D12_v2**  | 4     | 28            | 200                 | 1000                           | 12000                               | 500                              | 16              | 4         |
| **Standard_D13_v2**  | 8     | 56            | 400                 | 1000                           | 24.000                               | 500                              | 32              | 8         |

<!--| **Standard_D14_v2**  | 16    | 114           | 800                 | 1000                           | 48000                               | 500                              | 64              | 8         |-->


### <a name="dsv2-series"></a>DSv2-Serie
| Size              | vCPU  | Arbeitsspeicher (GiB)  | Temporärer Speicher (GiB)  | Max. Durchsatz des Betriebssystem-Datenträgers (IOPS)  | Maximaler Durchsatz (temporärer Speicher): (IOPS)  | Max. Datenträgerdurchsatz: (IOPS)  | Max. Anzahl Datenträger  | Maximale Anzahl NICs  |
|-------------------|-------|---------------|---------------------|--------------------------------|-------------------------------------|----------------------------------|-----------------|-----------|
| **Standard_DS1_v2**   | 1     | 3,5           | 7                   | 2000                           | 4000                                | 2300                             | 4               | 2         |
| **Standard_DS2_v2**   | 2     | 7             | 14                  | 2000                           | 8.000                                | 2300                             | 8               | 4         |
| **Standard_DS3_v2**   | 4     | 14            | 28                  | 2000                           | 16000                               | 2300                             | 16              | 4         |
| **Standard_DS4_v2**   | 8     | 28            | 56                  | 2000                           | 32000                               | 2300                             | 32              | 8         |
| **Standard_DS5_v2**   | 16    | 56            | 112                 | 2000                           | 64000                               | 2300                             | 64              | 8         |
| **Standard_DS11_v2**  | 2     | 14            | 28                  | 2000                           | 8.000                                | 2300                             | 8               | 2         |
| **Standard_DS12_v2**  | 4     | 28            | 56                  | 2000                           | 16000                               | 2300                             | 16              | 4         |
| **Standard_DS13_v2**  | 8     | 56            | 112                 | 2000                           | 32000                               | 2300                             | 32              | 8         |

<!--| **Standard_DS14_v2**  | 16    | 114           | 224                 | 2000                           | 64000                               | 2300                             | 64              | 8         |-->

Weitere Informationen finden Sie unter [Dv2- und DSv2-Serie](../articles/virtual-machines/dv2-dsv2-series.md#dv2-series).

### <a name="ncast4_v3-series-preview"></a>NCasT4_v3-Serie (Vorschau)

Diese Größen werden GPU-VMs auf Ihrem Gerät unterstützt und sind für computeintensive GPU-beschleunigte Anwendungen optimiert. Diese Serie ist speziell auf Rückschlussworkloads mit der Tesla T4-GPU von Nvidia ausgelegt.

| Size                  | vCPU  | Arbeitsspeicher (GiB)  | Temporärer Speicher (GiB)  | Max. Durchsatz des Betriebssystem-Datenträgers (IOPS)  | Maximaler Durchsatz (temporärer Speicher): (IOPS)  | Max. Datenträgerdurchsatz: (IOPS)  | GPU  | GPU-Arbeitsspeicher (GiB)  | Maximale Anzahl NICs  |
|-----------------------|-------|---------------|---------------------|--------------------------------|-------------------------------------|----------------------------------|------|-------------------|-----------|
| **Standard_NC4as_T4_v3**  | 4     | 28            | 176                 | 2000                           | 48000                               | 2300                             | 1    | 16                | 4         |
| **Standard_NC8as_T4_v3**  | 8     | 56            | 352                 | 2000                           | 48000                               | 2300                             | 1    | 16                | 8         |

Weitere Informationen finden Sie unter [NCasT4_v3-Serie](../articles/virtual-machines/nct4-v3-series.md).

### <a name="f-series"></a>F-Serie

Diese Serien sind für Computeworkloads optimiert und laufen auf Intel Xeon-Prozessoren. 

| Size           | vCPUs  | Memory: GiB  | Temporärer Speicher (GiB)  | Max. Durchsatz des Betriebssystem-Datenträgers (IOPS)  | Maximaler Durchsatz (temporärer Speicher): (IOPS)  | Max. Datenträgerdurchsatz: (IOPS)  | Max. Anzahl Datenträger  | Maximale Anzahl NICs  |
|----------------|---------|--------------|---------------------|--------------------------------|-------------------------------------|----------------------------------|-----------------|-----------|
| **Standard_F1**    | 1       | 2            | 16                  | 1000                           | 3000                                | 500                              | 4               | 2         |
| **Standard_F2**    | 2       | 4            | 32                  | 1000                           | 6000                                | 500                              | 8               | 4         |
| **Standard_F4**    | 4       | 8            | 64                  | 1000                           | 12000                               | 500                              | 16              | 4         |
| **Standard_F8**    | 8       | 16           | 128                 | 1000                           | 24.000                               | 500                              | 32              | 8         |
| **Standard_F16**   | 16      | 32           | 256                 | 1000                           | 48000                               | 500                              | 64              | 8         |
| **Standard_F1s**   | 1       | 2            | 4                   | 2000                           | 4000                                | 2300                             | 4               | 2         |
| **Standard_F2s**   | 2       | 4            | 8                   | 2000                           | 8.000                                | 2300                             | 8               | 4         |
| **Standard_F4s**   | 4       | 8            | 16                  | 2000                           | 16000                               | 2300                             | 16              | 4         |
| **Standard_F8s**   | 8       | 16           | 32                  | 2000                           | 32000                               | 2300                             | 32              | 8         |
| **Standard_F16s**  | 16      | 32           | 64                  | 2000                           | 64000                               | 2300                             | 64              | 8         |

Weitere Informationen finden Sie unter [Fsv2-Serie](../articles/virtual-machines/fsv2-series.md).
