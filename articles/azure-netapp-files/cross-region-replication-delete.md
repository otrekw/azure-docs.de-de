---
title: Löschen von Replikationen für die regionsübergreifende Azure NetApp Files-Replikation | Microsoft-Dokumentation
description: In diesem Artikel wird erläutert, wie eine Replikationsverbindung gelöscht werden kann, die zwischen den Quell- und Zielvolumes nicht länger benötigt wird.
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
ms.date: 11/17/2020
ms.author: b-juche
ms.openlocfilehash: e08b69271ba9d115c26418bc5e421ee6c94b031d
ms.sourcegitcommit: e2dc549424fb2c10fcbb92b499b960677d67a8dd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/17/2020
ms.locfileid: "94695706"
---
# <a name="delete-replications"></a>Löschen von Replikationen

Sie können die Replikationsverbindung zwischen den Quell- und Zielvolumes beenden, indem Sie die Volumereplikation löschen. Sie können entweder über das Quell- oder das Zielvolume einen Löschvorgang durchführen. Mit dem Löschvorgang wird nur die Autorisierung für die Replikation entfernt. Damit wird weder das Quell- noch das Zielvolume entfernt. 

## <a name="steps"></a>Schritte

1. Stellen Sie sicher, dass das Replikationspeering unterbrochen wurde, bevor Sie die Volumereplikation löschen.    
    Weitere Informationen finden Sie unter [Anzeigen des Integritätsstatus der Replikationsbeziehung](cross-region-replication-display-health-status.md) und [Unterbrechen des Replikationspeerings](cross-region-replication-manage-disaster-recovery.md#break-replication-peering-to-activate-the-destination-volume).  

1. Wenn Sie die Volumereplikation löschen möchten, wählen Sie aus dem Quell- oder Zielvolume die Option **Replikation** aus.  

2. Klicken Sie auf **Löschen**.    

3. Bestätigen Sie den Löschvorgang, indem Sie **Ja** eingeben und auf **Löschen** klicken.   

    ![Replikation löschen](../media/azure-netapp-files/cross-region-replication-delete-replication.png)

## <a name="next-steps"></a>Nächste Schritte  

* [Regionsübergreifende Replikation](cross-region-replication-introduction.md)
* [Regionsübergreifende Replikation: Anforderungen und Überlegungen](cross-region-replication-requirements-considerations.md)
* [Anzeigen des Integritätsstatus der Replikationsbeziehung](cross-region-replication-display-health-status.md)
* [Problembehandlung für die regionsübergreifende Replikation](troubleshoot-cross-region-replication.md)

