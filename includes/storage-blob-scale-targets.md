---
author: tamram
ms.service: storage
ms.topic: include
ms.date: 06/23/2020
ms.author: tamram
ms.openlocfilehash: 43a72d915fa5a00c54bef7a06fe3815a7d63f2fc
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85805513"
---
| Resource | Ziel | Ziel (Vorschau) |
|-|-|-|
| Maximale Größe eines einzelnen Blobcontainers | Identisch mit maximaler Speicherkontokapazität |  |
| Maximale Anzahl von Blöcken in einem Blockblob oder einem Anfügeblob | 50.000 Blöcke |  |
| Maximale Größe eines Blocks in einem Blockblob | 100 MiB | 4000 MiB (Vorschau) |
| Maximale Größe eines Blockblobs | 50.000 × 100MiB (ca. 4,75TiB) | 50.000 × 4.000 MiB (ca. 190,7 TiB) (Vorschau) |
| Maximale Größe eines Blocks in einem Anfügeblob | 4 MiB |  |
| Maximale Größe eines Anfügeblobs | 50.000 × 4MiB (ca. 195GiB) |  |
| Max. Größe eines Seitenblobs | 8 TiB |  |
| Maximale Anzahl gespeicherter Zugriffsrichtlinien pro Blobcontainer | 5 |  |
| Zielanforderungsrate für ein einzelnes Blob | Bis zu 500 Anforderungen pro Sekunde |  |
| Zieldurchsatz für ein einzelnes Seitenblob | Bis zu 60 MiB pro Sekunde |  |
| Zieldurchsatz für ein einzelnes Blockblob | Bis zur Eingangs-/Ausgangsbegrenzung des Speicherkontos<sup>1</sup> |  |

<sup>1</sup> Der Durchsatz für ein einzelnes Blob ist von mehreren Faktoren abhängig, einschließlich, aber nicht beschränkt auf: Parallelität, Anforderungsgröße, Leistungsstufe, Geschwindigkeit der Quelle bei Uploads und Downloadziel. Laden Sie größere Blobs oder Blöcke hoch, um von den Leistungsverbesserungen für [Blockblobs mit hohem Durchsatz](https://azure.microsoft.com/blog/high-throughput-with-azure-blob-storage/) zu profitieren. Rufen Sie den [Put Blob](/rest/api/storageservices/put-blob)- oder [Put Block](/rest/api/storageservices/put-block)-Vorgang insbesondere bei einer Blob- oder Blockgröße von mehr als 4 MiB für Standardspeicherkonten auf. Verwenden Sie für Premium-Blockblobkonten oder für Data Lake Storage Gen2-Speicherkonten eine Block- oder Blobgröße von mehr als 256 KiB.

In der folgenden Tabelle werden die maximal zulässigen Block- und Blob-Größen für die Dienstversion beschrieben.

| Dienstversion | Maximale Blockgröße (über Put Block) | Maximale Blob-Größe (über Put Block-Liste) | Maximale Blob-Größe über einen einzelnen Schreibvorgang (über Put Blob) |
|-|-|-|-|
| Ab Version 2019-12-12 | 4000 MiB (Vorschau) | Ungefähr 190,7 TiB (4.000 MiB X 50.000 Blöcke) (Vorschau) | 5000 MiB (Vorschau) |
| Version 2016-05-31 bis Version 2019-07-07 | 100 MiB | Ungefähr 4,75 TiB (100 MiB X 50.000 Blöcke) | 256 MiB |
| Versionen vor 2016-05-31 | 4 MiB | Ungefähr 195 GiB (4 MiB X 50.000 Blöcke) | 64 MiB |
