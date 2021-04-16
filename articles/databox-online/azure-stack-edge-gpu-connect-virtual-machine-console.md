---
title: Herstellen einer Verbindung mit der VM-Konsole eines Azure Stack Edge Pro GPU-Geräts
description: In diesem Artikel wird beschrieben, wie Sie eine Verbindung mit der Konsole einer VM herstellen, die auf einem Azure Stack Edge Pro GPU-Gerät ausgeführt wird.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: how-to
ms.date: 03/22/2021
ms.author: alkohli
ms.openlocfilehash: 68cf157a512e9b1f6caee4734869c5bb4b836e2f
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "104962363"
---
# <a name="connect-to-a-virtual-machine-console-on-an-azure-stack-edge-pro-gpu-device"></a>Herstellen einer Verbindung mit einer VM-Konsole eines Azure Stack Edge Pro GPU-Geräts

[!INCLUDE [applies-to-GPU-and-pro-r-and-mini-r-skus](../../includes/azure-stack-edge-applies-to-gpu-pro-r-mini-r-sku.md)]

Die Lösung „Azure Stack Edge Pro GPU“ führt nicht containerisierte Workloads über VMs aus. In diesem Artikel wird beschrieben, wie Sie eine Verbindung mit der Konsole einer auf dem Gerät bereitgestellten VM herstellen. 

Mithilfe der VM-Konsole können Sie über die Tastatur-, Maus- und Bildschirmfeatures und mithilfe der üblicherweise verfügbaren Remotedesktoptools auf Ihre VMs zugreifen. Über die Konsole haben Sie die Möglichkeit, alle Probleme zu beheben, die bei der VM-Bereitstellung auf Ihrem Gerät auftreten. Sie können selbst dann eine Verbindung mit der VM-Konsole herstellen, wenn eine VM-Bereitstellung fehlgeschlagen ist.


## <a name="workflow"></a>Workflow

Der allgemeine Workflow umfasst die folgenden Schritte:

1. Stellen Sie auf Ihrem Gerät eine Verbindung mit der PowerShell-Schnittstelle her.
1. Aktivieren Sie den Konsolenzugriff auf die VM.
1. Stellen Sie mithilfe des Remotedesktopprotokolls (RDP) eine Verbindung mit der VM her.
1. Widerrufen Sie den Konsolenzugriff auf die VM.

## <a name="prerequisites"></a>Voraussetzungen

Stellen Sie zunächst sicher, dass die folgenden Anforderungen erfüllt sind:

#### <a name="for-your-device"></a>Für Ihr Gerät

Sie sollten Zugriff auf ein aktiviertes Azure Stack Edge Pro GPU-Gerät haben. Auf dem Gerät muss mindestens eine VM bereitgestellt sein. Sie können VMs über Azure PowerShell, über Vorlagen oder über das Azure-Portal bereitstellen.

#### <a name="for-client-accessing-the-device"></a>Für den Client, der auf das Gerät zugreift

Stellen Sie sicher, dass Sie Zugriff auf ein Clientsystem haben, das folgende Bedingungen erfüllt:

- Kann auf die PowerShell-Schnittstelle des Geräts zugreifen Auf dem Client wird ein [unterstütztes Betriebssystem](azure-stack-edge-gpu-system-requirements.md#supported-os-for-clients-connected-to-device) ausgeführt.
- Auf dem Client wird PowerShell 7.0 oder höher ausgeführt. Diese PowerShell-Version funktioniert auf Windows-, Mac- und Linux-Clients. Unter folgendem Link finden Sie eine Anleitung für die [Installation von PowerShell 7.0](/powershell/scripting/whats-new/what-s-new-in-powershell-70?view=powershell-7.1&preserve-view=true).
- Verfügt über Remotedesktopfunktionen. Je nachdem, ob Sie Windows, macOS oder Linux verwenden, sollten Sie einen dieser [Remotedesktopclients](/windows-server/remote/remote-desktop-services/clients/remote-desktop-clients) installieren. Dieser Artikel enthält Anleitungen zum [Windows-Remotedesktop](/windows-server/remote/remote-desktop-services/clients/windowsdesktop#install-the-client) und zu [FreeRDP](https://www.freerdp.com/). <!--Which version of FreeRDP to use?-->


## <a name="connect-to-vm-console"></a>Verbinden mit der VM-Konsole

Führen Sie die folgenden Schritte aus, um eine Verbindung mit der VM-Konsole auf Ihrem Gerät herzustellen.

### <a name="connect-to-the-powershell-interface-on-your-device"></a>Herstellen einer Verbindung mit der PowerShell-Schnittstelle auf Ihrem Gerät

Stellen Sie zunächst eine [Verbindung mit der PowerShell-Schnittstelle](azure-stack-edge-gpu-connect-powershell-interface.md#connect-to-the-powershell-interface) Ihres Geräts her. 

### <a name="enable-console-access-to-the-vm"></a>Aktivieren Sie den Konsolenzugriff auf die VM.

1.  Führen Sie in der PowerShell-Schnittstelle den folgenden Befehl aus, um den Zugriff auf die VM-Konsole zu aktivieren.

    ```powershell
    Grant-HcsVMConnectAccess -ResourceGroupName <VM resource group> -VirtualMachineName <VM name>
    ```
2. Notieren Sie sich die ID der VM aus der Beispielausgabe. Sie werden sie in einem späteren Schritt benötigen.

    ```powershell
    [10.100.10.10]: PS>Grant-HcsVMConnectAccess -ResourceGroupName mywindowsvm1rg -VirtualMachineName mywindowsvm1
        
    VirtualMachineId       : 81462e0a-decb-4cd4-96e9-057094040063
    VirtualMachineHostName : 3V78B03
    ResourceGroupName      : mywindowsvm1rg
    VirtualMachineName     : mywindowsvm1
    Id                     : 81462e0a-decb-4cd4-96e9-057094040063
    [10.100.10.10]: PS>
    ```

### <a name="connect-to-the-vm"></a>Herstellen der Verbindung zur VM

Sie können jetzt mithilfe eines Remotedesktop-Clients eine Verbindung mit der VM-Konsole herstellen.

#### <a name="use-windows-remote-desktop"></a>Verwenden des Windows-Remotedesktop

1. Erstellen Sie eine neue Textdatei, und geben Sie den folgenden Text ein.

    ```
    pcb:s:<VM ID from PowerShell>;EnhancedMode=0
    full address:s:<IP address of the device>   
    server port:i:2179
    username:s:EdgeARMUser
    negotiate security layer:i:0
    ```
1. Speichern Sie die Datei als * *.rdp* in Ihrem Clientsystem. Sie werden mithilfe dieses Profils eine Verbindung mit der VM herstellen.
1. Doppelklicken Sie auf das Profil, um eine Verbindung mit der VM herzustellen. Geben Sie die folgenden Anmeldeinformationen an:

    - **Benutzername:** Melden Sie sich als „EdgeARMUser“ an.
    - **Kennwort:** Geben Sie das lokale Azure Resource Manager-Kennwort für Ihr Gerät an. Wenn Sie das Kennwort vergessen haben, [setzen Sie das Azure Resource Manager-Kennwort über das Azure-Portal zurück](azure-stack-edge-gpu-set-azure-resource-manager-password.md#reset-password-via-the-azure-portal). 

#### <a name="use-freerdp"></a>Verwenden von FreeRDP

Wenn Sie FreeRDP auf Ihrem Linux-Client verwenden, führen Sie den folgenden Befehl aus: 

```powershell
./wfreerdp /u:EdgeARMUser /vmconnect:<VM ID from PowerShell> /v:<IP address of the device>
```

## <a name="revoke-vm-console-access"></a>Widerrufen des VM-Konsolenzugriffs

Wenn Sie den Zugriff auf die VM-Konsole aufheben möchten, kehren Sie zur PowerShell-Schnittstelle Ihres Geräts zurück. Führen Sie den folgenden Befehl aus:

```
Revoke-HcsVMConnectAccess -ResourceGroupName <VM resource group> -VirtualMachineName <VM name>
```
Beispielausgabe:

```powershell
[10.100.10.10]: PS>Revoke-HcsVMConnectAccess -ResourceGroupName mywindowsvm1rg -VirtualMachineName mywindowsvm1

VirtualMachineId       : 81462e0a-decb-4cd4-96e9-057094040063
VirtualMachineHostName : 3V78B03
ResourceGroupName      : mywindowsvm1rg
VirtualMachineName     : mywindowsvm1
Id                     : 81462e0a-decb-4cd4-96e9-057094040063

[10.100.10.10]: PS>
```
> [!NOTE] 
> Es ist empfehlenswert, nach Verwendung der VM-Konsole den Zugriff zu widerrufen oder das PowerShell-Fenster zu schließen, um die Sitzung zu beenden. 

## <a name="next-steps"></a>Nächste Schritte

- Führen Sie für [Azure Stack Edge Pro](azure-stack-edge-gpu-troubleshoot.md) im Azure-Portal eine Problembehandlung durch.