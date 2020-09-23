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
ms.date: 09/16/2020
ms.author: b-juche
ms.openlocfilehash: 0904ac36a9453e51dbb1efc50eee2b9bf3c669c7
ms.sourcegitcommit: 7374b41bb1469f2e3ef119ffaf735f03f5fad484
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/16/2020
ms.locfileid: "90708350"
---
# <a name="delete-replications"></a>Löschen von Replikationen

Sie können die Replikationsverbindung zwischen den Quell- und Zielvolumes beenden, indem Sie die Volumereplikation löschen. Sie können entweder über das Quell- oder das Zielvolume einen Löschvorgang durchführen. Mit dem Löschvorgang wird nur die Autorisierung für die Replikation entfernt. Damit wird weder das Quell- noch das Zielvolume entfernt. 

## <a name="steps"></a>Schritte

1. Wenn Sie die Volumereplikation löschen möchten, wählen Sie aus dem Quell- oder Zielvolume die Option **Replikation** aus.  

2. Klicken Sie auf **Löschen**.    

3. Bestätigen Sie den Löschvorgang, indem Sie **Ja** eingeben und auf **Löschen** klicken.   

    ![Replikation löschen](../media/azure-netapp-files/cross-region-replication-delete-replication.png)

## <a name="next-steps"></a>Nächste Schritte  

* [Regionsübergreifende Replikation](cross-region-replication-introduction.md)
* [Regionsübergreifende Replikation: Anforderungen und Überlegungen](cross-region-replication-requirements-considerations.md)
* [Anzeigen des Integritätsstatus der Replikationsbeziehung](cross-region-replication-display-health-status.md)
* [Problembehandlung für die regionsübergreifende Replikation](troubleshoot-cross-region-replication.md)

