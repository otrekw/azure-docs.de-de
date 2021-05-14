---
title: Erstellen eines neuen virtuellen Switches in Azure Stack Edge über PowerShell
description: Hier erfahren Sie, wie Sie einen virtuellen Switch auf einem Azure Stack Edge-Gerät mithilfe von Azure PowerShell erstellen.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: how-to
ms.date: 04/06/2021
ms.author: alkohli
ms.openlocfilehash: 1ad86695510a8fe93bbeeab27db53f5afbef92fd
ms.sourcegitcommit: b0557848d0ad9b74bf293217862525d08fe0fc1d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/07/2021
ms.locfileid: "106555924"
---
# <a name="create-a-new-virtual-switch-in-azure-stack-edge-pro-gpu-via-powershell"></a>Erstellen eines neuen virtuellen Switches auf der Azure Stack Edge Pro-GPU über PowerShell

[!INCLUDE [applies-to-GPU-and-pro-r-and-mini-r-skus](../../includes/azure-stack-edge-applies-to-gpu-pro-r-mini-r-sku.md)]

In diesem Artikel wird beschrieben, wie Sie einen neuen virtuellen Switch auf Ihrem Azure Stack Edge Pro-GPU-Gerät erstellen können. Beispielsweise würden Sie einen neuen virtuellen Switch erstellen, wenn Sie möchten, dass Ihre virtuellen Computer über einen anderen physischen Netzwerkport verbunden werden.

## <a name="vm-deployment-workflow"></a>VM-Bereitstellungsworkflow

1. Stellen Sie auf Ihrem Gerät eine Verbindung mit der PowerShell-Schnittstelle her.
2. Fragen Sie verfügbare physische Netzwerkschnittstellen ab.
3. Erstellen eines virtuellen Switch.
4. Überprüfen Sie das virtuelle Netzwerk und das Subnetz, die automatisch erstellt werden.

## <a name="prerequisites"></a>Voraussetzungen

Stellen Sie Folgendes sicher, bevor Sie beginnen:

- Sie haben Zugriff auf einen Clientcomputer, der auf die PowerShell-Schnittstelle Ihres Geräts zugreifen kann. Siehe [Herstellen einer Verbindung mit der PowerShell-Schnittstelle](azure-stack-edge-gpu-connect-powershell-interface.md#connect-to-the-powershell-interface). 

    Auf dem Clientcomputer muss ein [unterstütztes Betriebssystem](azure-stack-edge-gpu-system-requirements.md#supported-os-for-clients-connected-to-device) ausgeführt werden.

- Verwenden Sie die lokale Benutzeroberfläche, um Compute auf einer der physischen Netzwerkschnittstellen auf Ihrem Gerät gemäß den Anweisungen in [Computenetzwerk aktivieren](azure-stack-edge-gpu-deploy-configure-network-compute-web-proxy.md#enable-compute-network) zu aktivieren. 


## <a name="connect-to-the-powershell-interface"></a>Herstellen einer Verbindung mit der PowerShell-Schnittstelle

[Stellen Sie auf Ihrem Gerät eine Verbindung mit der PowerShell-Schnittstelle her](azure-stack-edge-gpu-connect-powershell-interface.md#connect-to-the-powershell-interface).

## <a name="query-available-network-interfaces"></a>Abfragen der verfügbaren Netzwerkschnittstellen

1. Verwenden Sie den folgenden Befehl, um eine Liste physischer Netzwerkschnittstellen anzuzeigen, auf denen Sie einen neuen virtuellen Switch erstellen können. Wählen Sie eine dieser Netzwerkschnittstellen aus.

    ```powershell
    Get-NetAdapter -Physical
    ```
    Beispielausgabe:
    
    ```powershell
        [10.100.10.10]: PS>Get-NetAdapter -Physical
        
        Name                      InterfaceDescription                    ifIndex Status       MacAddress       LinkSpeed
        ----                      --------------------                    ------- ------       ----------        -----
        Port2                     QLogic 2x1GE+2x25GE QL41234HMCU NIC ...      12 Up           34-80-0D-05-26-EA ...ps
        Ethernet                  Remote NDIS Compatible Device                11 Up           F4-02-70-CD-41-39 ...ps
        Port1                     QLogic 2x1GE+2x25GE QL41234HMCU NI...#3       9 Up           34-80-0D-05-26-EB ...ps
        Port5                     Mellanox ConnectX-4 Lx Ethernet Ad...#2       8 Up           0C-42-A1-C0-E3-99 ...ps
        Port3                     QLogic 2x1GE+2x25GE QL41234HMCU NI...#4       7 Up           34-80-0D-05-26-E9 ...ps
        Port6                     Mellanox ConnectX-4 Lx Ethernet Adapter       6 Up           0C-42-A1-C0-E3-98 ...ps
        Port4                     QLogic 2x1GE+2x25GE QL41234HMCU NI...#2       4 Up           34-80-0D-05-26-E8 ...ps
        
        [10.100.10.10]: PS>
    ```
2. Wählen Sie eine Netzwerkschnittstelle mit folgenden Eigenschaften aus:

    - Im Status **Up**. 
    - Wird nicht von vorhandenen virtuellen Switches verwendet. Zurzeit kann nur ein virtueller Switch pro Netzwerkschnittstelle konfiguriert werden. 
    
    Um den vorhandenen virtuellen Switch und die Zuordnung der Netzwerkschnittstelle zu überprüfen, führen Sie den Befehl `Get-HcsExternalVirtualSwitch` aus.
 
    Hier sehen Sie eine Beispielausgabe.

    ```powershell
    [10.100.10.10]: PS>Get-HcsExternalVirtualSwitch

    Name                          : vSwitch1
    InterfaceAlias                : {Port2}
    EnableIov                     : True
    MacAddressPools               :
    IPAddressPools                : {}
    ConfigurationSource           : Dsc
    EnabledForCompute             : True
    SupportsAcceleratedNetworking : False
    DbeDhcpHostVnicName           : f4a92de8-26ed-4597-a141-cb233c2ba0aa
    Type                          : External
    
    [10.100.10.10]: PS>
    ```
    In diesem Fall ist Port 2 einem vorhandenen virtuellen Switch zugeordnet und sollte nicht verwendet werden.

## <a name="create-a-virtual-switch"></a>Erstellen eines virtuellen Switches

Verwenden Sie das folgende Cmdlet, um einen neuen virtuellen Switch auf der angegebenen Netzwerkschnittstelle zu erstellen. Nach Abschluss dieses Vorgangs können Ihre Computeinstanzen das neue virtuelle Netzwerk verwenden.

```powershell
Add-HcsExternalVirtualSwitch -InterfaceAlias <Network interface name> -WaitForSwitchCreation $true
```

Identifizieren Sie den neu erstellten Switch mithilfe des Befehls `Get-HcsExternalVirtualSwitch`. Der neu erstellte Switch wird als `vswitch-<InterfaceAlias>` benannt. 

Beispielausgabe:

```powershell
[10.100.10.10]: P> Add-HcsExternalVirtualSwitch -InterfaceAlias Port5 -WaitForSwitchCreation $true
[10.100.10.10]: PS>Get-HcsExternalVirtualSwitch

Name                          : vSwitch1
InterfaceAlias                : {Port2}
EnableIov                     : True
MacAddressPools               :
IPAddressPools                : {}
ConfigurationSource           : Dsc
EnabledForCompute             : True
SupportsAcceleratedNetworking : False
DbeDhcpHostVnicName           : f4a92de8-26ed-4597-a141-cb233c2ba0aa
Type                          : External

Name                          : vswitch-Port5
InterfaceAlias                : {Port5}
EnableIov                     : True
MacAddressPools               :
IPAddressPools                :
ConfigurationSource           : Dsc
EnabledForCompute             : False
SupportsAcceleratedNetworking : False
DbeDhcpHostVnicName           : 9b301c40-3daa-49bf-a20b-9f7889820129
Type                          : External

[10.100.10.10]: PS>
```

## <a name="verify-network-subnet"></a>Überprüfen von Netzwerk und Subnetz 

Nachdem Sie den neuen virtuellen Switch erstellt haben, erstellt die Azure Stack Edge Pro-GPU automatisch ein virtuelles Netzwerk und Subnetz, das ihm entspricht. Sie können dieses virtuelle Netzwerk verwenden, wenn Sie virtuelle Computer erstellen.

<!--To identify the virtual network and subnet associated with the new switch that you created, use the `Get-HcsVirtualNetwork` command. This cmdlet will be released in April some time. -->

## <a name="next-steps"></a>Nächste Schritte

- [Bereitstellen von VMs auf Ihrem Azure Stack Edge Pro-GPU-Gerät über Azure PowerShell](azure-stack-edge-gpu-deploy-virtual-machine-powershell.md)

- [Bereitstellen von VMs auf einem Azure Stack Edge Pro-GPU-Gerät über das Azure-Portal](azure-stack-edge-gpu-deploy-virtual-machine-portal.md)
