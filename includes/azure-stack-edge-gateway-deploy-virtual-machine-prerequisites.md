---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 12/21/2020
ms.author: alkohli
ms.openlocfilehash: f2443765ecc9116193cefbc729ced25fa5657e59
ms.sourcegitcommit: 799f0f187f96b45ae561923d002abad40e1eebd6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/24/2020
ms.locfileid: "97763426"
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

1. Wenn Sie **Compute** für Kubernetes konfiguriert haben, können Sie diesen Schritt überspringen. Stellen Sie sicher, dass Sie eine Netzwerkschnittstelle für Compute aktiviert haben. Wechseln Sie auf der lokalen Benutzeroberfläche zu den **Compute**-Einstellungen. Wählen Sie die Netzwerkschnittstelle aus, die Sie verwenden möchten, um einen virtuellen Switch zu erstellen. Die von Ihnen erstellten virtuellen Computer werden an einen virtuellen Switch angefügt, der an diesen Port und das zugehörige Netzwerk angefügt ist. Achten Sie darauf, ein Netzwerk auszuwählen, das mit der IP-Adresse übereinstimmt, die Sie für den virtuellen Computer verwenden werden.  

    ![Aktivieren von Computeeinstellungen 1](../articles/databox-online/media/azure-stack-edge-gpu-deploy-virtual-machine-templates/enable-compute-setting.png)

    Aktivieren Sie Compute für die Netzwerkschnittstelle. Azure Stack Edge erstellt und verwaltet einen virtuellen Switch, der dieser Netzwerkschnittstelle entspricht. Geben Sie zu diesem Zeitpunkt keine bestimmten IP-Adressen für Kubernetes ein. Die Aktivierung von Compute kann mehrere Minuten dauern.

    > [!NOTE]
    > Wählen Sie beim Erstellen von GPU-VMs eine Netzwerkschnittstelle aus, die mit dem Internet verbunden ist. Auf diese Weise können Sie die GPU-Erweiterung auf Ihrem Gerät installieren.


1. Aktivieren Sie die VM-Rolle im Azure-Portal. In diesem Schritt wird ein eindeutiges Abonnement für Ihr Gerät erstellt, das zum Erstellen von VMs über die lokalen APIs des Geräts verwendet wird. 

    1. Um die VM-Rolle zu aktivieren, wechseln Sie im Azure-Portal zur Azure Stack Edge-Ressource für Ihr Azure Stack Edge-Gerät. Wechseln Sie zu **Edgecomputing -> Virtuelle Computer**.

        ![VM-Image hinzufügen 1](../articles/databox-online/media/azure-stack-edge-gpu-deploy-virtual-machine-portal/add-virtual-machine-image-1.png)

    1. Wählen Sie **Virtuelle Computer** aus, um zur Seite **Übersicht** zu wechseln. **Aktivieren** Sie die Cloudverwaltung für virtuelle Computer.
        ![VM-Image hinzufügen 2](../articles/databox-online/media/azure-stack-edge-gpu-deploy-virtual-machine-portal/add-virtual-machine-image-2.png)