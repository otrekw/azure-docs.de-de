---
author: tamram
ms.service: storage
ms.topic: include
ms.date: 11/08/2019
ms.author: tamram
ms.openlocfilehash: 2ed88d8abb7cbe96093b68d89030e6e464a35541
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2019
ms.locfileid: "75392445"
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

<sup>1</sup> Der Durchsatz für ein einzelnes Blob ist von mehreren Faktoren abhängig, einschließlich, aber nicht beschränkt auf: Parallelität, Anforderungsgröße, Leistungsstufe, Geschwindigkeit der Quelle bei Uploads und Downloadziel. Laden Sie größere Blobs oder Blöcke hoch, um von den Leistungsverbesserungen für [Blockblobs mit hohem Durchsatz](https://azure.microsoft.com/blog/high-throughput-with-azure-blob-storage/) zu profitieren. Rufen Sie den [Put Blob](/rest/api/storageservices/put-blob)- oder [Put Block](/rest/api/storageservices/put-block)-Vorgang insbesondere bei einer Blob- oder Blockgröße von mehr als 4 MiB für Standardspeicherkonten auf. Verwenden Sie für Premium-Blockblobkonten oder für Data Lake Storage Gen2-Speicherkonten eine Block- oder Blobgröße von mehr als 256 KiB.
