---
title: Anpassen der Größe des Kapazitätspools oder eines Volumes für Azure NetApp Files | Microsoft-Dokumentation
description: Erfahren Sie, wie die Größe eines Kapazitätspools oder eines Volumes geändert wird. Durch eine Größenänderung des Kapazitätspools wird die erworbene Azure NetApp Files-Kapazität geändert.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 03/10/2021
ms.author: b-juche
ms.openlocfilehash: 869f46207b940521ee0b66b5afa9c6e2718ab04f
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "104594477"
---
# <a name="resize-a-capacity-pool-or-a-volume"></a>Ändern der Größe eines Kapazitätspools oder Volumes
Sie können die Größe eines Kapazitätspools oder eines Volumes nach Bedarf anpassen. 

## <a name="resize-the-capacity-pool"></a>Anpassen der Größe des Kapazitätspools 

Sie können die Größe des Kapazitätspools in 1-TiB-Schritten erhöhen oder verringern. Allerdings kann die Größe des Kapazitätspools nicht kleiner als 4 TiB sein. Durch eine Größenänderung des Kapazitätspools wird die erworbene Azure NetApp Files-Kapazität geändert.

1. Klicken Sie auf dem Blatt „Manage NetApp Account“ (NetApp-Konto verwalten) auf den Kapazitätspool, dessen Größe Sie anpassen möchten. 
2. Klicken Sie mit der rechten Maustaste auf den Namen des Kapazitätspools, oder klicken Sie am Ende der Zeile mit dem Kapazitätspool auf das Symbol „...“, um das Kontextmenü anzuzeigen. 
3. Mithilfe der Kontextmenüoptionen können Sie die Größe des Kapazitätspools ändern oder ihn löschen.

## <a name="resize-a-volume"></a>Ändern der Größe eines Volumes

Sie können die Größe eines Volumes nach Bedarf anpassen. Der Kapazitätsverbrauch eines Volumes wird mit der bereitgestellten Kapazität des dazugehörigen Pools verrechnet.

1. Klicken Sie auf dem Blatt „Manage NetApp Account“ auf **Volumes**. 
2. Klicken Sie mit der rechten Maustaste auf den Namen des Volumes, dessen Größe Sie anpassen möchten, oder klicken Sie auf am Ende der Zeile mit dem Volume auf das Symbol „...“, um das Kontextmenü anzuzeigen.
3. Mithilfe der Kontextmenüoptionen können Sie die Größe des Volumes ändern oder es löschen.

## <a name="resize-a-cross-region-replication-destination-volume"></a>Ändern der Größe eines regionsübergreifenden Replikationszielvolumes 

In einer [regionsübergreifenden Replikationsbeziehung](cross-region-replication-introduction.md) wird die Größe eines Zielvolumes basierend auf der Größe des Quellvolumes automatisch angepasst. Daher müssen Sie die Größe des Zielvolumes nicht separat ändern. Dieses Verhalten zur automatischen Größenänderung gilt, wenn sich die Volumes in einer aktiven Replikationsbeziehung befinden oder wenn das Replikationspeering mit dem [Vorgang zur Neusynchronisierung](cross-region-replication-manage-disaster-recovery.md#resync-replication) unterbrochen wird. 

In der folgenden Tabelle wird das Verhalten der Größenänderung des Zielvolumes auf dem [Spiegelungsstatus](cross-region-replication-display-health-status.md) basierend beschrieben:

|  Spiegelungsstatus  | Verhalten der Größenänderung des Zielvolumes |
|-|-|
| *Gespiegelt* | Wenn das Zielvolume initialisiert wurde und bereit ist, Spiegelungsaktualisierungen zu empfangen, wird bei Änderung der Größe des Quellvolumes automatisch die Größe des Zielvolumes geändert. |
| *Beschädigt* | Wenn Sie die Größe des Quellvolumes ändern und der Spiegelungsstatus *beschädigt* ist, wird die Größe des Zielvolumes automatisch mit dem [Vorgang zum erneuten Synchronisieren](cross-region-replication-manage-disaster-recovery.md#resync-replication) angepasst.  |
| *Nicht initialisiert* | Wenn Sie die Größe des Quellvolumes ändern und der Spiegelungsstatus noch *nicht initialisiert* ist, muss die Größenänderung des Zielvolumes manuell erfolgen. Daher sollten Sie auf den Abschluss der Initialisierung warten (d. h., bis zum Spiegelungsstatus *gespiegelt*), um die Größe des Quellvolumes zu ändern. | 

> [!IMPORTANT]
> Vergewissern Sie sich, dass in den Kapazitätspools genügend Toleranzbereich für die Quell- und Zielvolumes der regionsübergreifenden Replikation vorhanden ist. Wenn Sie die Größe des Quellvolumes ändern, wird automatisch auch die Größe des Zielvolumes geändert. Sollte der Kapazitätspool des Zielvolumes jedoch nicht über genügend Toleranzbereich verfügen, tritt ein Fehler für die Größenänderung von Quell- und Zielvolume auf.

## <a name="next-steps"></a>Nächste Schritte

- [Einrichten eines Kapazitätspools](azure-netapp-files-set-up-capacity-pool.md)
- [Verwalten eines manuellen QoS-Kapazitätspools](manage-manual-qos-capacity-pool.md)
- [Dynamisches Ändern der Dienstebene eines Volumes](dynamic-change-volume-service-level.md) 