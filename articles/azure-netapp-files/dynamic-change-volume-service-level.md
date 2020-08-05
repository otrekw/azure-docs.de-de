---
title: Dynamisches Ändern des Servicelevels eines Volumes für Azure NetApp Files | Microsoft-Dokumentation
description: Beschreibt, wie Sie den Servicelevel eines Volumes dynamisch ändern.
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
ms.date: 07/24/2020
ms.author: b-juche
ms.openlocfilehash: bd28f949d35d38c9e64af7ff4196aa1754fbc37a
ms.sourcegitcommit: d7bd8f23ff51244636e31240dc7e689f138c31f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/24/2020
ms.locfileid: "87172535"
---
# <a name="dynamically-change-the-service-level-of-a-volume"></a>Dynamisches Ändern der Dienstebene eines Volumes

Sie können den Servicelevel eines vorhandenen Volumes ändern, indem Sie das Volume in einen anderen Kapazitätspool verschieben, der den von Ihnen für das Volume gewünschten [Servicelevel](azure-netapp-files-service-levels.md) verwendet. Diese direkte Änderung des Servicelevels für das Volume erfordert keine Migration von Daten. Auch der Zugriff auf das Volume wird hiervon nicht beeinträchtigt.  

> [!IMPORTANT] 
> Die Verwendung dieser Funktion erfordert ein Whitelisting. Senden Sie eine E-Mail mit Ihrer Abonnement-ID an anffeedback@microsoft.com, um dieses Feature anzufordern.

Diese Funktion ermöglicht es Ihnen, die Anforderungen Ihrer Workload nach Bedarf zu erfüllen.  Sie können ein vorhandenes Volume so ändern, dass es einen höheren Servicelevel verwendet, um die Leistung zu verbessern, oder so dass es einen niedrigeren Servicelevel verwendet, um die Kosten zu optimieren. Wenn sich das Volume beispielsweise derzeit in einem Kapazitätspool befindet, der den Servicelevel *Standard* verwendet, und Sie möchten, dass das Volume den Servicelevel *Premium* verwendet, können Sie das Volume dynamisch in einen Kapazitätspool verschieben, der den Servicelevel *Premium* verwendet.  

Der Kapazitätspool, in den Sie das Volume verschieben möchten, muss bereits vorhanden sein. Der Kapazitätspool kann andere Volumes enthalten.  Wenn Sie das Volume in einen ganz neuen Kapazitätspool verschieben möchten, müssen Sie [den Kapazitätspool erstellen](azure-netapp-files-set-up-capacity-pool.md), bevor Sie das Volume verschieben.  

## <a name="considerations"></a>Überlegungen

* Nachdem das Volume in einen anderen Kapazitätspool verschoben wurde, haben Sie keinen Zugriff mehr auf die Aktivitätsprotokolle und Volumenmetriken des vorherigen Volumes. Das Volume beginnt in dem neuen Kapazitätspool mit neuen Aktivitätsprotokollen und Metriken.

* Wenn Sie ein Volume in einen Kapazitätspool mit einem höheren Servicelevel verschieben (z. B. eine Verschiebung vom Servicelevel *Standard* in *Premium* oder  *Ultra*), müssen Sie mindestens sieben Tage warten, bevor Sie das Volume in einen Kapazitätspool mit einem niedrigeren Servicelevel erneut verschieben können (z. B. eine Verschiebung von *Ultra* in *Premium* oder *Standard*).  
Diese Wartezeit ist nicht erforderlich, wenn Sie das Volume in einen Kapazitätspool verschieben, der denselben oder einen niedrigeren Servicelevel besitzt.

## <a name="steps"></a>Schritte

1.  Klicken Sie auf der Seite „Volumes“ mit der rechten Maustaste auf das Volume, dessen Servicelevel Sie ändern möchten. Wählen Sie **Pool ändern** aus.

    ![Klicken Sie mit der rechten Maustaste auf das Volume.](../media/azure-netapp-files/right-click-volume.png)

2. Wählen Sie im Fenster „Pool ändern“ den Kapazitätspool aus, in den Sie das Volume verschieben möchten. 

    ![Pool ändern](../media/azure-netapp-files/change-pool.png)

3.  Klicken Sie auf **OK**.


## <a name="next-steps"></a>Nächste Schritte  

* [Dienstebenen für Azure NetApp Files](azure-netapp-files-service-levels.md)
* [Einrichten eines Kapazitätspools](azure-netapp-files-set-up-capacity-pool.md)
