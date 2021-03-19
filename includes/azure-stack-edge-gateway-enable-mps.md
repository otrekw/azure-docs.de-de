---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 02/11/2021
ms.author: alkohli
ms.openlocfilehash: b2c1ebe390b1a2dec7be678b5d6f3a991056a23b
ms.sourcegitcommit: 6386854467e74d0745c281cc53621af3bb201920
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/08/2021
ms.locfileid: "102517559"
---
1. Stellen Sie Folgendes sicher, bevor Sie beginnen:

    1. Sie haben Ihr [Azure Stack Edge Pro-Gerät](../articles/databox-online/azure-stack-edge-gpu-deploy-activate.md) mit einer Azure Stack Edge-Ressource in Azure konfiguriert und aktiviert.
    1. Sie haben [auf diesem Gerät Compute im Azure-Portal konfiguriert](../articles/databox-online/azure-stack-edge-deploy-configure-compute.md#configure-compute).
    
1. [Herstellen einer Verbindung mit der PowerShell-Schnittstelle](../articles/databox-online/azure-stack-edge-gpu-connect-powershell-interface.md#connect-to-the-powershell-interface).
1. Verwenden Sie den folgenden Befehl, um MPS auf Ihrem Gerät zu aktivieren.

    ```powershell
    Start-HcsGpuMPS
    ```