---
title: include file
description: include file
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 11/18/2019
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: b819264895e35c6ef4fe9dc5263444dcac17eaa2
ms.sourcegitcommit: a5ebf5026d9967c4c4f92432698cb1f8651c03bb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/08/2019
ms.locfileid: "74935890"
---
Derzeit gibt es für Ultra-Datenträger weitere Einschränkungen, die wie folgt lauten:

- Werden in den folgenden Regionen mit jeweils einer unterschiedlichen Anzahl von Verfügbarkeitszonen unterstützt:
    - USA (Ost) 2
    - East US
    - USA, Westen 2
    - Asien, Südosten
    - Nordeuropa
    - Europa, Westen
    - UK, Süden 
- Können nur mit Verfügbarkeitszonen verwendet werden. (Verfügbarkeitsgruppen und einzelne VM-Bereitstellungen außerhalb der Zonen können keine Ultra-Datenträger anfügen.)
- Werden nur auf den folgenden VM-Serien unterstützt:
    - [ESv3](https://azure.microsoft.com/blog/introducing-the-new-dv3-and-ev3-vm-sizes/)
    - [DSv3](https://azure.microsoft.com/blog/introducing-the-new-dv3-and-ev3-vm-sizes/)
    - FSv2
    - [M](../articles/virtual-machines/workloads/sap/hana-vm-operations-storage.md)
    - [Mv2](../articles/virtual-machines/workloads/sap/hana-vm-operations-storage.md)
- Nicht jede VM-Größe ist in allen unterstützten Regionen mit Ultra-Datenträgern verfügbar.
- Sind nur als Datenträger für Daten verfügbar und unterstützen nur für physische Sektorgröße von 4 KB. Aufgrund der nativen Sektorgröße von 4 KB von Ultra-Datenträgern gibt es einige Anwendungen die mit Ultra-Datenträgern nicht kompatibel sind. Ein Beispiel hierfür wäre Oracle Database, das Version 12.2 oder höher erfordert, um Ultra-Datenträger zu unterstützen.  
- Können nur als leere Datenträger erstellt werden.  
- Unterstützen noch keine Datenträger-Momentaufnahmen, VM-Images, Verfügbarkeitsgruppen oder Azure Disk Encryption
- Unterstützen noch keine Integration in Azure Backup oder Azure Site Recovery.
- Die IOPS-Obergrenze liegt bei allgemein verfügbaren virtuellen Computern derzeit bei 80.000.
- Wenn Sie an einer eingeschränkten Vorschau einer VM teilnehmen möchten, die 160.000 IOPS mit Ultradisks erreichen kann, senden Sie eine E-Mail an UltraDiskFeedback@microsoft.com.