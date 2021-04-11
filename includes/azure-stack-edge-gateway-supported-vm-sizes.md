---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 03/27/2021
ms.author: alkohli
ms.openlocfilehash: f92dc476d3bc79fdb522424e36e44a13644c725e
ms.sourcegitcommit: 3ee3045f6106175e59d1bd279130f4933456d5ff
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/31/2021
ms.locfileid: "106078796"
---
Die VM-Größe bestimmt den Umfang der Computeressourcen (CPU, GPU und Arbeitsspeicher), die für die VM zur Verfügung gestellt werden. Sie müssen VMs mit einer für die Workload geeigneten Größe erstellen. Auch wenn alle VMs auf derselben Hardware ausgeführt werden, gibt es bei den VM-Größen unterschiedliche Grenzwerte für den Datenträgerzugriff. Dies kann Ihnen helfen, den gesamten Datenträgerzugriff auf allen Ihren VMs zu verwalten. Wenn eine Workload zunimmt, können Sie auch die Größe einer vorhandenen VM ändern.

Die folgenden VMs werden für die Erstellung auf Ihrem Azure Stack Edge-Gerät unterstützt.

### <a name="dv2-series"></a>Dv2-Serie
|Size     |vCPU     |Arbeitsspeicher (GiB) | Temporärer Speicher (GiB)   | Max. Anzahl Datenträger | Maximale Anzahl NICs |
|-------------------|----|----|-----|----|------|
|**Standard_D1_v2** |1   |3,5 |50   | 4    |2 |
|**Standard_D2_v2** |2   |7   |100  | 8    |4 |
|**Standard_D3_v2** |4   |14  |200  | 16  |4 |
|**Standard_D4_v2** |8   |28  |400  | 32  |8 |
|**Standard_D5_v2** |16  |56  |800  | 64  |8 |
|**Standard_D11_v2** |2   |14  |100 | 8     |2 |
|**Standard_D12_v2** |4   |28  |200  | 16   |4 |
|**Standard_D13_v2** |8   |56  |400  | 32  |8 |

### <a name="dsv2-series"></a>DSv2-Serie
|Size     |vCPU     |Arbeitsspeicher (GiB) | Temporärer Speicher (GiB)  | Max. Anzahl Datenträger| 
|--------------------|----|----|----|-----|
|**Standard_DS1_v2** |1   |3,5 |4000  |4  | 
|**Standard_DS2_v2** |2   |7   |8.000  |8  | 
|**Standard_DS3_v2** |4   |14  |16000 |16 | 
|**Standard_DS4_v2** |8   |28  |32000 |32 | 
|**Standard_DS5_v2** |16  |56  |64000 |64 |  
|**Standard_DS11_v2**|2   |14  |8.000  |4  | 
|**Standard_DS12_v2**|4   |28  |16000 |8  | 
|**Standard_DS13_v2**|8   |56  |32000 |16 | 


Weitere Informationen finden Sie unter [Dv2- und DSv2-Serie](../articles/virtual-machines/dv2-dsv2-series.md#dv2-series).

### <a name="ncast4_v3-series-preview"></a>NCasT4_v3-Serie (Vorschau)

Diese Größen werden GPU-VMs auf Ihrem Gerät unterstützt und sind für computeintensive GPU-beschleunigte Anwendungen optimiert. Diese Serie ist speziell auf Rückschlussworkloads mit der Tesla T4-GPU von Nvidia ausgelegt. 

|Size     |vCPU     |Arbeitsspeicher (GiB) | Temporärer Speicher (GiB)  | GPU | GPU-Arbeitsspeicher (GiB) | Maximale Anzahl NICs |
|---------------------|----|----|-----|-----|-------|--------------|
|**Standard_NC4as_T4_v3** |4   |28  |180   |1 |16   |4 |
|**Standard_NC8as_T4_v3** |8   |56  |360   |1 |16  |8 |

Weitere Informationen finden Sie unter [NCasT4_v3-Serie](../articles/virtual-machines/nct4-v3-series.md).

### <a name="f-series"></a>F-Serie

Diese Serien sind für Computeworkloads optimiert und laufen auf Intel Xeon-Prozessoren. 

| Size | vCPUs | Memory: GiB | Temporärer Speicher (GiB) |  Max. Anzahl Datenträger | Maximale Anzahl NICs |
|---|---|---|---|---|---|
| Standard_F1  | 1  | 2   |16      | 4  |  2 |
| Standard_F2 | 2  | 4 |32      | 8  |  4 |
| Standard_F4  | 4  | 8 |64   | 16 |  4 |
| Standard_F8 | 8 | 16  |132    | 32 |  8 |
| Standard_F16 | 16 | 32  |262   | 64 |  8 |
| Standard_F1s | 1 | 2  | 4  | 4 | 1 |
| Standard_F2s | 2 | 4 |8   | 8 | 4 |
| Standard_F4s | 4 | 8 |16 | 16 |  4 |
| Standard_F8s | 8 | 16 |32 | 32 |  8 |
| Standard_F16s | 16 | 32 |64 | 64 |  8 |

Weitere Informationen finden Sie unter [Fsv2-Serie](../articles/virtual-machines/fsv2-series.md).

