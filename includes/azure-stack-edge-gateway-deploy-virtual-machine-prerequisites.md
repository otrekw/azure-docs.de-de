---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 08/31/2020
ms.author: alkohli
ms.openlocfilehash: 14d1e466a7e84ff3454284c596a09ebcabf9d603
ms.sourcegitcommit: 58d3b3314df4ba3cabd4d4a6016b22fa5264f05a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/02/2020
ms.locfileid: "89304030"
---
Bevor Sie virtuelle Computer auf Ihrem Azure Stack Edge-Gerät bereitstellen können, müssen Sie den Client so konfigurieren, dass er über Azure Resource Manager über Azure PowerShell eine Verbindung mit dem Gerät herstellt. Ausführliche Schritte finden Sie unter [Herstellen einer Verbindung mit Azure Resource Manager auf Ihrem Azure Stack Edge-Gerät](../articles/databox-online/azure-stack-edge-j-series-connect-resource-manager.md).


Stellen Sie sicher, dass die folgenden Schritte für den Zugriff auf das Gerät über Ihren Client verwendet werden können (Sie haben diese Konfiguration beim Herstellen einer Verbindung mit Azure Resource Manager ausgeführt. Sie werden lediglich überprüfen, ob die Konfiguration erfolgreich war.): 

1. Überprüfen Sie, ob die Azure Resource Manager-Kommunikation funktioniert. Typ:     

    ```powershell
    Add-AzureRmEnvironment -Name <Environment Name> -ARMEndpoint "https://management.<appliance name>.<DNSDomain>"
    ```

1. Lokale Geräte-APIs zum Authentifizieren aufrufen. Typ: 

    `login-AzureRMAccount -EnvironmentName <Environment Name>`

    Benutzername bereitstellen – *EdgeARMuser* und das Kennwort für die Verbindung über Azure Resource Manager.

1. Wenn Sie **Compute** für Kubernetes konfiguriert haben, können Sie diesen Schritt überspringen. Stellen Sie sicher, dass Sie eine Netzwerkschnittstelle für Compute aktiviert haben. Wechseln Sie auf der lokalen Benutzeroberfläche zu den **Compute**-Einstellungen. Wählen Sie die Netzwerkschnittstelle aus, die Sie verwenden möchten, um einen virtuellen Switch zu erstellen. Die von Ihnen erstellten virtuellen Computer werden an einen virtuellen Switch angefügt, der an diesen Port und das zugehörige Netzwerk angefügt ist. Achten Sie darauf, ein Netzwerk auszuwählen, das mit der statischen IP-Adresse übereinstimmt, die Sie für den virtuellen Computer verwenden werden.  

    ![Aktivieren von Computeeinstellungen 1](../articles/databox-online/media/azure-stack-edge-gpu-deploy-virtual-machine-templates/enable-compute-setting.png)

    Aktivieren Sie Compute für die Netzwerkschnittstelle. Azure Stack Edge erstellt und verwaltet einen virtuellen Switch, der dieser Netzwerkschnittstelle entspricht. Geben Sie zu diesem Zeitpunkt keine bestimmten IP-Adressen für Kubernetes ein. Die Aktivierung von Compute kann mehrere Minuten dauern.

    <!--If you decide to use another network interface for compute, make sure that you:
    
    - Delete all the VMs that you have deployed using Azure Resource Manager.
    
    - Delete all virtual network interfaces and the virtual network associated with this network interface. 
    
    - You can now enable another network interface for compute.-->

<!--1. You may also need to configure TLS 1.2 on your client machine if running older versions of AzCopy.--> 

