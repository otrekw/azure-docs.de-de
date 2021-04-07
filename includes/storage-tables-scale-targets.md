---
author: tamram
ms.service: storage
ms.topic: include
ms.date: 03/09/2020
ms.author: tamram
ms.openlocfilehash: 27617da97ced9ac775beae71e4c25202944b2aba
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "78940996"
---
In der folgenden Tabelle sind die Kapazitäts-, Skalierbarkeits- und Leistungsziele für den Tabellenspeicher beschrieben.

| Resource | Ziel |
|----------|---------------|
| Anzahl von Tabellen in einem Azure Storage-Konto | Begrenzung nur durch die Kapazität des Speicherkontos |
| Anzahl von Partitionen in einer Tabelle | Begrenzung nur durch die Kapazität des Speicherkontos |
| Anzahl von Entitäten in einer Partition | Begrenzung nur durch die Kapazität des Speicherkontos |
| Maximale Größe einer einzelnen Tabelle | 500 TiB |
| Maximale Größe einer einzelnen Entität, einschließlich aller Eigenschaftswerte | 1 MiB |
| Maximale Anzahl von Eigenschaften in einer Tabellenentität | 255 (einschließlich der drei Systemeigenschaften **PartitionKey**, **RowKey** und **Timestamp**) |
| Maximale Gesamtgröße einer individuellen Eigenschaft in einer Entität | Variiert je nach Eigenschaftstyp. Weitere Informationen finden Sie unter **Eigenschaftstypen** in [Grundlegendes zum Tabellendienst-Datenmodell](/rest/api/storageservices/understanding-the-table-service-data-model). |
| Größe von **PartitionKey** | Eine Zeichenfolge mit bis zu 1 KB |
| Größe von **RowKey** | Eine Zeichenfolge mit bis zu 1 KB |
| Größe einer Entitätsgruppentransaktion | Eine Transaktion kann höchstens 100 Entitäten umfassen, und die Nutzlast muss weniger als 4 MiB groß sein. Eine Entitätsgruppentransaktion kann ein Update für eine Entität nur einmal einschließen. |
| Maximale Anzahl gespeicherter Zugriffsrichtlinien pro Tabelle | 5 |
| Maximale Anforderungsrate pro Speicherkonto | 20.000 Transaktionen pro Sekunde, ausgehend von einer Entitätsgröße von 1KiB |
| Zieldurchsatz für eine einzelne Tabellenpartition (Entitäten von 1KiB) | Bis zu 2.000 Entitäten pro Sekunde |