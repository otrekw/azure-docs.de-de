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
ms.openlocfilehash: aa9327bd0ba6763aa4e89630611aabb3c5195655
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85448696"
---
Derzeit gibt es für Ultra-Datenträger weitere Einschränkungen, die wie folgt lauten:

Die einzigen Optionen für die Infrastrukturredundanz, die derzeit für Disk Ultra-Datenträger zur Verfügung stehen, sind Verfügbarkeitszonen. Virtuelle Computer, die andere Redundanzoptionen verwenden, können keinen Disk Ultra-Datenträger anfügen.

In der folgenden Tabelle sind die Regionen, in denen Disk Ultra-Datenträger verfügbar sind, sowie die entsprechenden Verfügbarkeitsoptionen aufgeführt:

> [!NOTE]
> Wenn eine Region in der folgenden Liste keine Verfügbarkeitszonen für Ultradatenträger enthält, müssen die virtuellen Computer in dieser Region ohne Optionen für die Infrastrukturredundanz bereitgestellt werden, um einen Ultradatenträger anzufügen.

|Regions  |Anzahl von Verfügbarkeitszonen mit Unterstützung von Ultradatenträgern  |
|---------|---------|
|US Government, Virginia     |Keine         |
|USA Süd Mitte     |Keine         |
|USA (Mitte)     |Drei Zonen         |
|USA (Westen)     |Keine         |
|USA, Westen 2    |Drei Zonen         |
|East US     |Drei Zonen         |
|USA (Ost) 2     |Zwei Zonen         |
|Asien, Südosten     |Drei Zonen         |
|Nordeuropa     |Drei Zonen          |
|Europa, Westen     |Drei Zonen          |
|UK, Süden     |Drei Zonen          |
|Japan, Osten     |Zwei Zonen         |
|Frankreich, Mitte    |Zwei Zonen        |


- Werden nur auf den folgenden VM-Serien unterstützt:
    - [ESv3](../articles/virtual-machines/ev3-esv3-series.md#esv3-series)
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