---
author: roygara
ms.service: storage
ms.topic: include
ms.date: 08/10/2020
ms.author: rogarana
ms.openlocfilehash: 86bf4911026e46c997469b956f9e7c75c4f17164
ms.sourcegitcommit: 77afc94755db65a3ec107640069067172f55da67
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/22/2021
ms.locfileid: "98698126"
---
#### <a name="additional-premium-file-share-level-limits"></a>Zusätzliche Freigabegrenzwerte für Dateien auf Premiumebene

|Bereich  |Ziel  |
|---------|---------|
|Mindestgröße vergrößern/verkleinern    |1 GiB      |
|IOPS-Grundwert    |400 + 1 IOPS pro GiB, bis zu 100.000|
|IOPS-Bursting    |Max. (4.000, 3 × IOPS pro GiB), bis zu 100.000|
|Ausgangsrate         |60 MiB/s + 0,06 * bereitgestelltes GiB        |
|Eingangsrate| 40 MiB/s + 0,04 * bereitgestelltes GiB |

#### <a name="file-level-limits"></a>Grenzwerte auf Dateiebene

|Bereich  |Standarddateien  |Premiumdateien  |
|---------|---------|---------|
|Size     |1 TiB         |4 TiB         |
|Max. IOPS pro Datei      |1\.000         |Bis zu 8.000*         |
|Gleichzeitige Handles     |2\.000         |2\.000         |
|Ausgehende Daten     |Siehe: Durchsatzwerte für Standarddateien         |300 MiB/Sek. (bis zu 1 GiB/s bei SMB Multichannel Vorschau)**         |
|Eingehende Daten     |Siehe: Durchsatzwerte für Standarddateien         |200 MiB/Sek. (bis zu 1 GiB/s bei SMB Multichannel Vorschau)**        |
|Throughput     |Bis zu 60 MiB/s         |Siehe: Ein-/Ausgangswerte für Premiumdateien         |

\* <sup> Gilt für Ein-/Ausgaben beim Lesen und Schreiben (normalerweise kleinere E/A-Größen <=64 K). Metadatenvorgänge (außer Lese-und Schreibvorgängen) können niedriger sein. </sup>

\*\* <sup> Unterliegen Netzwerkgrenzwerten für Computer, verfügbarer Bandbreite, E/A-Größen, Warteschlangenlänge und anderen Faktoren. Weitere Informationen finden Sie unter [SMB Multichannel-Leistung](../articles/storage/files/storage-files-smb-multichannel-performance.md). </sup>
