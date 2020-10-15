---
author: roygara
ms.service: storage
ms.topic: include
ms.date: 08/10/2020
ms.author: rogarana
ms.openlocfilehash: d704c6026e9d007a7365a3b72649ca509585da4d
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "88057787"
---
#### <a name="additional-premium-file-share-level-limits"></a>Zusätzliche Freigabegrenzwerte für Dateien auf Premiumebene

|Bereich  |Ziel  |
|---------|---------|
|Mindestgröße vergrößern/verkleinern    |1 GiB      |
|IOPS-Grundwert    |1 IOPS pro GiB, bis zu 100.000|
|IOPS-Bursting    |3 x IOPS pro GiB, bis zu 100.000|
|Ausgangsrate         |60 MiB/s + 0,06 * bereitgestelltes GiB        |
|Eingangsrate| 40 MiB/s + 0,04 * bereitgestelltes GiB |

#### <a name="file-level-limits"></a>Grenzwerte auf Dateiebene

|Bereich  |Standarddateien  |Premiumdateien  |
|---------|---------|---------|
|Size     |1 TiB         |4 TiB         |
|Max. IOPS pro Datei      |1.000         |5.000         |
|Gleichzeitige Handles     |2\.000         |2\.000         |
|Ausgehende Daten     |Siehe: Durchsatzwerte für Standarddateien         |300 MiB/s         |
|Eingehende Daten     |Siehe: Durchsatzwerte für Standarddateien         |200 MiB/s         |
|Throughput     |Bis zu 60 MiB/s         |Siehe: Ein-/Ausgangswerte für Premiumdateien         |