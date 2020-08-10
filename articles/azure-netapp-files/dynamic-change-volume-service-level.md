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
ms.openlocfilehash: e19db61efbf93e3191d5780d07952f3d195c7a59
ms.sourcegitcommit: 3d56d25d9cf9d3d42600db3e9364a5730e80fa4a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/03/2020
ms.locfileid: "87533046"
---
# <a name="dynamically-change-the-service-level-of-a-volume"></a>Dynamisches Ändern der Dienstebene eines Volumes

Sie können den Servicelevel eines vorhandenen Volumes ändern, indem Sie das Volume in einen anderen Kapazitätspool verschieben, der den von Ihnen für das Volume gewünschten [Servicelevel](azure-netapp-files-service-levels.md) verwendet. Diese direkte Änderung des Servicelevels für das Volume erfordert keine Migration von Daten. Auch der Zugriff auf das Volume wird hiervon nicht beeinträchtigt.  

Diese Funktion ermöglicht es Ihnen, die Anforderungen Ihrer Workload nach Bedarf zu erfüllen.  Sie können ein vorhandenes Volume so ändern, dass es einen höheren Servicelevel verwendet, um die Leistung zu verbessern, oder so dass es einen niedrigeren Servicelevel verwendet, um die Kosten zu optimieren. Wenn sich das Volume beispielsweise derzeit in einem Kapazitätspool befindet, der den Servicelevel *Standard* verwendet, und Sie möchten, dass das Volume den Servicelevel *Premium* verwendet, können Sie das Volume dynamisch in einen Kapazitätspool verschieben, der den Servicelevel *Premium* verwendet.  

Der Kapazitätspool, in den Sie das Volume verschieben möchten, muss bereits vorhanden sein. Der Kapazitätspool kann andere Volumes enthalten.  Wenn Sie das Volume in einen ganz neuen Kapazitätspool verschieben möchten, müssen Sie [den Kapazitätspool erstellen](azure-netapp-files-set-up-capacity-pool.md), bevor Sie das Volume verschieben.  

## <a name="considerations"></a>Überlegungen

* Nachdem das Volume in einen anderen Kapazitätspool verschoben wurde, haben Sie keinen Zugriff mehr auf die Aktivitätsprotokolle und Volumenmetriken des vorherigen Volumes. Das Volume beginnt in dem neuen Kapazitätspool mit neuen Aktivitätsprotokollen und Metriken.

* Wenn Sie ein Volume in einen Kapazitätspool mit einem höheren Servicelevel verschieben (z. B. eine Verschiebung vom Servicelevel *Standard* in *Premium* oder  *Ultra*), müssen Sie mindestens sieben Tage warten, bevor Sie das Volume in einen Kapazitätspool mit einem niedrigeren Servicelevel erneut verschieben können (z. B. eine Verschiebung von *Ultra* in *Premium* oder *Standard*).  
Diese Wartezeit ist nicht erforderlich, wenn Sie das Volume in einen Kapazitätspool verschieben, der denselben oder einen niedrigeren Servicelevel besitzt.

## <a name="register-the-feature"></a>Registrieren der Funktion

Die Funktion zum Verschieben eines Volumes in einen anderen Kapazitätspool befindet sich derzeit in der Vorschauphase. Wenn Sie diese Funktion zum ersten Mal verwenden, müssen Sie sie zuerst registrieren.

1. Registrieren Sie die Funktion: 

    ```azurepowershell-interactive
    Register-AzProviderFeature -ProviderNamespace Microsoft.NetApp -FeatureName ANFTierChange
    ```

2. Überprüfen Sie den Status der Funktionsregistrierung: 

    > [!NOTE]
    > Der **RegistrationState** kann einige Minuten lang den Status `Registering` aufweisen, bevor der Wechsel in `Registered` erfolgt. Warten Sie, bis der Status **Registriert** lautet, bevor Sie fortfahren.

    ```azurepowershell-interactive
    Get-AzProviderFeature -ProviderNamespace Microsoft.NetApp -FeatureName ANFTierChange
    ```

## <a name="move-a-volume-to-another-capacity-pool"></a>Verschieben eines Volumes in einen anderen Kapazitätspool

1.  Klicken Sie auf der Seite „Volumes“ mit der rechten Maustaste auf das Volume, dessen Servicelevel Sie ändern möchten. Wählen Sie **Pool ändern** aus.

    ![Klicken Sie mit der rechten Maustaste auf das Volume.](../media/azure-netapp-files/right-click-volume.png)

2. Wählen Sie im Fenster „Pool ändern“ den Kapazitätspool aus, in den Sie das Volume verschieben möchten. 

    ![Pool ändern](../media/azure-netapp-files/change-pool.png)

3.  Klicken Sie auf **OK**.


## <a name="next-steps"></a>Nächste Schritte  

* [Dienstebenen für Azure NetApp Files](azure-netapp-files-service-levels.md)
* [Einrichten eines Kapazitätspools](azure-netapp-files-set-up-capacity-pool.md)
