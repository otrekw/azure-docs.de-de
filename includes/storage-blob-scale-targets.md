---
author: tamram
ms.service: storage
ms.topic: include
ms.date: 11/08/2019
ms.author: tamram
ms.openlocfilehash: 0fda881b805eb3a967cf3b05f6c6df8c65d20730
ms.sourcegitcommit: bc193bc4df4b85d3f05538b5e7274df2138a4574
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2019
ms.locfileid: "73905782"
---
| Resource | Ziel        |
|----------|---------------|
| Maximale Größe eines einzelnen Blobcontainers | Identisch mit maximaler Speicherkontokapazität |
| Maximale Anzahl von Blöcken in einem Blockblob oder einem Anfügeblob | 50.000 Blöcke |
| Maximale Größe eines Blocks in einem Blockblob | 100 MiB |
| Maximale Größe eines Blockblobs | 50.000 × 100MiB (ca. 4,75TiB) |
| Maximale Größe eines Blocks in einem Anfügeblob | 4 MiB |
| Maximale Größe eines Anfügeblobs | 50.000 × 4MiB (ca. 195GiB) |
| Max. Größe eines Seitenblobs | 8 TiB |
| Maximale Anzahl gespeicherter Zugriffsrichtlinien pro Blobcontainer | 5 |
|Zielanforderungsrate für ein einzelnes Blob | Bis zu 500 Anforderungen pro Sekunde |
|Zieldurchsatz für ein einzelnes Seitenblob | Bis zu 60 MiB pro Sekunde |
|Zieldurchsatz für ein einzelnes Blockblob |Bis zur Eingangs-/Ausgangsbegrenzung des Speicherkontos<sup>1</sup> |

<sup>1</sup> Einzelobjektdurchsatz ist abhängig von mehreren Faktoren einschließlich, aber nicht beschränkt auf: Parallelität, Anforderungsgröße, Leistungsstufe, Geschwindigkeit der Quelle bei Uploads und Downloadziel. Um den Vorteil von Leistungsverbesserungen durch [Blockblobs mit hohem Durchsatz](https://azure.microsoft.com/blog/high-throughput-with-azure-blob-storage/) zu nutzen, verwenden Sie eine Put Blob- oder Put Block-Anforderungsgröße von > 4MiB (> 256KiB für Premium-Leistungs-Blockblobspeicher oder Data Lake Storage Gen2).
