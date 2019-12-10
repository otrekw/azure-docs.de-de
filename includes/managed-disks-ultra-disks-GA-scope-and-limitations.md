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
ms.openlocfilehash: df488c2a3033015175943f7d092beaaa41e34046
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2019
ms.locfileid: "74260742"
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
- Sind nur als Datenträger für Daten verfügbar und unterstützen nur für physische Sektorgröße von 4K.  
- Können nur als leere Datenträger erstellt werden.  
- Unterstützen noch keine Datenträger-Momentaufnahmen, VM-Images, Verfügbarkeitsgruppen oder Azure Disk Encryption
- Unterstützen noch keine Integration in Azure Backup oder Azure Site Recovery.
- Die IOPS-Obergrenze liegt bei allgemein verfügbaren virtuellen Computern derzeit bei 80.000.
- Wenn Sie an einer eingeschränkten Vorschau einer VM teilnehmen möchten, die 160.000 IOPS mit Ultradisks erreichen kann, senden Sie eine E-Mail an UltraDiskFeedback@microsoft.com.