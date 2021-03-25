---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 08/04/2020
ms.author: alkohli
ms.openlocfilehash: 6e42fef2aa4415373ed0bac39284f36ba330a4d9
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "101730646"
---
Stellen Sie über das Remotedesktopprotokoll (RDP) mithilfe der IP-Adresse, die Sie bei der Erstellung der VM übergeben haben, eine Verbindung mit Ihrer Windows-VM her.

1. Öffnen Sie RDP auf dem Client. 
1. Navigieren Sie zu **Start**, und geben Sie dann **mstsc** ein.
1. Geben Sie im Bereich **Remotedesktopverbindung** die IP-Adresse der VM und die Zugriffsanmeldeinformationen ein, die Sie in der VM-Vorlagenparameterdatei verwendet haben, und klicken Sie dann auf **Verbinden**.

   ![Screenshot des Bereichs „Remotedesktopverbindung“ zum Herstellen einer Verbindung über RDP mit Ihrer Windows-VM.](media/azure-stack-edge-gateway-connect-vm-windows/connect-vm-rdp-1.png)

   > [!NOTE]
   > Möglicherweise müssen Sie die Verbindung mit einem nicht vertrauenswürdigen Computer genehmigen. 

Sie sind jetzt bei Ihrer VM angemeldet, die auf der Appliance ausgeführt wird. 
