---
title: include file
description: include file
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 04/08/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: a5e0e459800e7cb57672518597f3d04a74f53118
ms.sourcegitcommit: ae3d707f1fe68ba5d7d206be1ca82958f12751e8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/10/2020
ms.locfileid: "81008633"
---
Derzeit gibt es für Ultra-Datenträger weitere Einschränkungen, die wie folgt lauten:

Die einzigen Optionen für die Infrastrukturredundanz, die derzeit für Disk Ultra-Datenträger zur Verfügung stehen, sind Verfügbarkeitszonen. Virtuelle Computer, die andere Redundanzoptionen verwenden, können keinen Disk Ultra-Datenträger anfügen.

In der folgenden Tabelle sind die Regionen, in denen Disk Ultra-Datenträger verfügbar sind, sowie die entsprechenden Verfügbarkeitsoptionen aufgeführt:

> [!NOTE]
> Einige Verfügbarkeitszonen innerhalb dieser Regionen bieten keine Disk Ultra-Datenträger an.

|Regions  |Keine Redundanz bei der Infrastruktur  |Verfügbarkeitszonen  |
|---------|---------|---------|
|USA (Westen)     |Ja         |Nein         |
|USA, Westen 2    |Nein         |Ja         |
|East US     |Nein         |Ja         |
|USA (Ost) 2     |Nein         |Ja         |
|Asien, Südosten     |Nein         |Ja         |
|Nordeuropa     |Nein         |Ja         |
|Europa, Westen     |Nein         |Ja         |
|UK, Süden     |Nein         |Ja         |

- Werden nur auf den folgenden VM-Serien unterstützt:
    - [ESv3](https://azure.microsoft.com/blog/introducing-the-new-dv3-and-ev3-vm-sizes/)
    - [DSv3](https://azure.microsoft.com/blog/introducing-the-new-dv3-and-ev3-vm-sizes/)
    - FSv2
    - [M](../articles/virtual-machines/workloads/sap/hana-vm-operations-storage.md)
    - [Mv2](../articles/virtual-machines/workloads/sap/hana-vm-operations-storage.md)
- Nicht jede VM-Größe ist in allen unterstützten Regionen mit Ultra-Datenträgern verfügbar.
- Sind nur als Datenträger für Daten verfügbar und unterstützen nur für physische Sektorgröße von 4 KB. Aufgrund der nativen Sektorgröße von 4 KB von Ultra-Datenträgern gibt es einige Anwendungen die mit Ultra-Datenträgern nicht kompatibel sind. Ein Beispiel hierfür wäre Oracle Database, das Version 12.2 oder höher erfordert, um Ultra-Datenträger zu unterstützen.  
- Können nur als leere Datenträger erstellt werden.  
- Unterstützt derzeit keine Momentaufnahmen von Datenträgern, VM-Images, Verfügbarkeitsgruppen, Azure Dedicated Hosts oder Azure Disk Encryption.
- Unterstützt derzeit keine Integration in Azure Backup oder Azure Site Recovery.
- Die IOPS-Obergrenze liegt bei allgemein verfügbaren virtuellen Computern derzeit bei 80.000.

Azure Ultra-Datenträger bieten standardmäßig bis zu 16 TiB pro Region pro Abonnement, aber Disk Ultra-Datenträger unterstützen auf Anfrage auch höhere Kapazitäten. Wenden Sie sich an den Azure-Support, um eine Erhöhung der Kapazität anzufordern.