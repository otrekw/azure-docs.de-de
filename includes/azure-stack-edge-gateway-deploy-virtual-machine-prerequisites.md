---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 01/15/2021
ms.author: alkohli
ms.openlocfilehash: f166413507afb9aff814eaddaade099d2e34ae68
ms.sourcegitcommit: b0557848d0ad9b74bf293217862525d08fe0fc1d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/07/2021
ms.locfileid: "106554843"
---
Bevor Sie virtuelle Computer auf Ihrem Azure Stack Edge-Gerät bereitstellen können, müssen Sie den Client so konfigurieren, dass er über Azure Resource Manager über Azure PowerShell eine Verbindung mit dem Gerät herstellt. Eine ausführliche Anleitung finden Sie unter [Herstellen einer Verbindung mit Azure Resource Manager auf Ihrem Azure Stack Edge-Gerät](../articles/databox-online/azure-stack-edge-gpu-connect-resource-manager.md).

Stellen Sie sicher, dass Sie mit den folgenden Schritten von Ihrem Client aus auf das Gerät zugreifen können. Sie haben diese Konfiguration bereits ausgeführt, als Sie eine Verbindung mit Azure Resource Manager hergestellt haben, und jetzt überprüfen Sie, ob die Konfiguration erfolgreich war. 

1. Vergewissern Sie sich, dass die Kommunikation mit Azure Resource Manager funktioniert, indem Sie den folgenden Befehl ausführen:     

    ```powershell
    Add-AzureRmEnvironment -Name <Environment Name> -ARMEndpoint "https://management.<appliance name>.<DNSDomain>"
    ```

1. Geben Sie Folgendes ein, um die Authentifizierungs-APIs des lokalen Geräts aufzurufen: 

    `login-AzureRMAccount -EnvironmentName <Environment Name> -TenantId c0257de7-538f-415c-993a-1b87a031879d`

    Um eine Verbindung über Azure Resource Manager herzustellen, geben Sie den Benutzernamen *EdgeArmUser* und Ihr Kennwort ein.

1. Wenn Sie Compute für Kubernetes konfiguriert haben, können Sie diesen Schritt überspringen. Stellen Sie andernfalls sicher, dass Sie eine Netzwerkschnittstelle für Compute aktiviert haben. Gehen Sie hierzu wie folgt vor: 

   a. Wechseln Sie auf der lokalen Benutzeroberfläche zu den Einstellungen für **Compute**.  
   b. Wählen Sie die Netzwerkschnittstelle aus, die Sie verwenden möchten, um einen virtuellen Switch zu erstellen. Die von Ihnen erstellten virtuellen Computer werden an einen virtuellen Switch angefügt, der an diesen Port und das zugehörige Netzwerk angefügt ist. Achten Sie darauf, ein Netzwerk auszuwählen, das mit der IP-Adresse übereinstimmt, die Sie für den virtuellen Computer verwenden werden.  

    ![Screenshot des Bereichs mit den Einstellungen für das Compute-Konfigurationsnetzwerk.](../articles/databox-online/media/azure-stack-edge-gpu-deploy-virtual-machine-templates/enable-compute-setting.png)

   c. Wählen Sie für die Netzwerkschnittstelle unter **Für Compute aktivieren** die Option **Ja** aus. Azure Stack Edge erstellt und verwaltet einen virtuellen Switch, der dieser Netzwerkschnittstelle entspricht. Geben Sie zu diesem Zeitpunkt keine bestimmten IP-Adressen für Kubernetes ein. Die Aktivierung von Compute kann mehrere Minuten dauern.

    > [!NOTE]
    > Wählen Sie beim Erstellen von GPU-VMs eine Netzwerkschnittstelle aus, die mit dem Internet verbunden ist. So können Sie die GPU-Erweiterung auf Ihrem Gerät installieren.