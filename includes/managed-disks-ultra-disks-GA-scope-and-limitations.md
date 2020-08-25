---
title: include file
description: include file
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 06/25/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 546c21802d275fe99ed2acbf00e32d37db3603f2
ms.sourcegitcommit: 152c522bb5ad64e5c020b466b239cdac040b9377
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/14/2020
ms.locfileid: "88225194"
---
Derzeit gibt es für Ultra-Datenträger weitere Einschränkungen, die wie folgt lauten:

Die einzigen Optionen für die Infrastrukturredundanz, die derzeit für Disk Ultra-Datenträger zur Verfügung stehen, sind Verfügbarkeitszonen. Virtuelle Computer, die andere Redundanzoptionen verwenden, können keinen Disk Ultra-Datenträger anfügen.

In der folgenden Tabelle sind die Regionen, in denen Disk Ultra-Datenträger verfügbar sind, sowie die entsprechenden Verfügbarkeitsoptionen aufgeführt:

> [!NOTE]
> Wenn eine Region in der folgenden Liste keine Verfügbarkeitszonen für Ultradatenträger enthält, müssen die virtuellen Computer in dieser Region ohne Optionen für die Infrastrukturredundanz bereitgestellt werden, um einen Ultradatenträger anzufügen.

|Regions  |Anzahl von Verfügbarkeitszonen mit Unterstützung von Ultradatenträgern  |
|---------|---------|
|US Government, Virginia     |Keine         |
|US Gov Arizona     |Keine         |
|USA Süd Mitte     |Keine         |
|USA (Mitte)     |Drei Zonen         |
|USA (Westen)     |Keine         |
|USA, Westen 2    |Drei Zonen         |
|East US     |Drei Zonen         |
|USA (Ost) 2     |Drei Zonen         |
|Asien, Südosten     |Drei Zonen         |
|Asien, Osten     |Keine         |
|Nordeuropa     |Drei Zonen          |
|Europa, Westen     |Drei Zonen          |
|UK, Süden     |Drei Zonen          |
|Japan, Osten     |Drei Zonen         |
|Frankreich, Mitte    |Zwei Zonen        |
|Brasilien Süd    |Keine        |
|Australien (Osten)    |Drei Zonen        |
|Kanada, Mitte*    |Drei Zonen        |

\* Wenden Sie sich an den Azure-Support, um Zugriff auf Verfügbarkeitszonen für diese Region zu erhalten.

- Werden nur auf den folgenden VM-Serien unterstützt:
    - [ESv3](../articles/virtual-machines/ev3-esv3-series.md#esv3-series)
    - [Easv4](../articles/virtual-machines/eav4-easv4-series.md#easv4-series)
    - [Edsv4](../articles/virtual-machines/edv4-edsv4-series.md#edsv4-series)
    - [Esv4](../articles/virtual-machines/ev4-esv4-series.md#esv4-series)
    - [DSv3](../articles/virtual-machines/dv3-dsv3-series.md#dsv3-series)
    - [FSv2](../articles/virtual-machines/fsv2-series.md)
    - [LSv2](../articles/virtual-machines/lsv2-series.md)
    - [M](../articles/virtual-machines/workloads/sap/hana-vm-operations-storage.md)
    - [Mv2](../articles/virtual-machines/workloads/sap/hana-vm-operations-storage.md)
- Nicht jede VM-Größe ist in allen unterstützten Regionen mit Ultra-Datenträgern verfügbar.
- Sind nur als Datenträger für Daten verfügbar und unterstützen nur für physische Sektorgröße von 4 KB. Aufgrund der nativen Sektorgröße von 4 KB von Ultra-Datenträgern gibt es einige Anwendungen die mit Ultra-Datenträgern nicht kompatibel sind. Ein Beispiel hierfür wäre Oracle Database, das Version 12.2 oder höher erfordert, um Ultra-Datenträger zu unterstützen.  
- Können nur als leere Datenträger erstellt werden.  
- Unterstützt derzeit keine Momentaufnahmen von Datenträgern, VM-Images, Verfügbarkeitsgruppen, Azure Dedicated Hosts oder Azure Disk Encryption.
- Unterstützt derzeit keine Integration in Azure Backup oder Azure Site Recovery.
- Unterstützt nur Lese- und Schreibvorgänge, die nicht zwischengespeichert sind.
- Die IOPS-Obergrenze liegt bei allgemein verfügbaren virtuellen Computern derzeit bei 80.000.

Azure Ultra-Datenträger bieten standardmäßig bis zu 16 TiB pro Region pro Abonnement, aber Disk Ultra-Datenträger unterstützen auf Anfrage auch höhere Kapazitäten. Wenden Sie sich an den Azure-Support, um eine Erhöhung der Kapazität anzufordern.