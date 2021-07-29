---
title: Erfassen von VM-Gastprotokollen auf Azure Stack Edge Pro GPU
description: So erstellen Sie ein Supportpaket mit Gastprotokollen für VMs auf einem Azure Stack Edge Pro GPU-Gerät.
services: databox
author: v-dalc
ms.service: databox
ms.subservice: edge
ms.topic: how-to
ms.date: 06/03/2021
ms.author: alkohli
ms.openlocfilehash: 7ffb96cdc3fbb561009b7f545a3b9a1da9eef729
ms.sourcegitcommit: ef950cf37f65ea7a0f583e246cfbf13f1913eb12
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/04/2021
ms.locfileid: "111421002"
---
# <a name="collect-vm-guest-logs-on-an-azure-stack-edge-pro-gpu-device"></a>Erfassen von VM-Gastprotokollen auf Azure Stack Edge Pro GPU-Geräten

[!INCLUDE [applies-to-GPU-and-pro-r-and-mini-r-skus](../../includes/azure-stack-edge-applies-to-gpu-pro-r-mini-r-sku.md)]

Um VM-Bereitstellungsfehler auf Ihrem Azure Stack Edge Pro GPU-Gerät zu diagnostizieren, müssen Sie die Gastprotokolle für die fehlerhafte VM auswerten. In diesem Artikel wird beschrieben, wie Gastprotokolle für die VMs in einem Supportpaket gesammelt werden.

## <a name="collect-vm-guest-logs-in-support-package"></a>Erfassen von VM-Gastprotokollen im Supportpaket

Führen Sie folgende Schritte aus, um Gastprotokolle für ausgefallene VMs auf einem Azure Stack Edge Pro GPU-Gerät zu erfassen:

1. [Stellen Sie auf Ihrem Gerät eine Verbindung mit der PowerShell-Schnittstelle her](azure-stack-edge-gpu-connect-powershell-interface.md#connect-to-the-powershell-interface).

2. Erfassen Sie Gastprotokolle für ausgefallene VMs, und schließen Sie diese Protokolle in ein Supportpaket ein, indem Sie folgende Befehle ausführen:

   ```powershell
   Get-VMInGuestLogs -FailedVM
   Get-HcsNodeSupportPackage -Path “\\<network path>” -Include InGuestVMLogFiles -Credential “domain_name\user”
   ```

   Sie finden die Protokolle im Ordner `hcslogs\VmGuestLogs`.

3. Informationen zum VM-Bereitstellungsverlauf finden Sie in folgenden Protokollen:

   **Virtuelle Linux-Computer:**
   - /var/log/cloud-init-output.log
   - /var/log/cloud-init.log
   - /var/log/waagent.log

   **Virtuelle Windows-Computer:**
   - C:\Windows\Azure\Panther\WaSetup.xml

## <a name="next-steps"></a>Nächste Schritte

- [Problembehandlung bei der VM-Bereitstellung auf Azure Stack Edge Pro GPU](azure-stack-edge-gpu-troubleshoot-virtual-machine-provisioning.md)