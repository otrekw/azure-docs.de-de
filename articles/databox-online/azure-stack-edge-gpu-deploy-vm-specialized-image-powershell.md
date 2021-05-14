---
title: Erstellen von VM-Images auf der Grundlage eines spezialisierten Images der Windows-VHD für Ihr Azure Stack Edge Pro-GPU-Gerät
description: Hier erfahren Sie, wie Sie VM-Images auf der Grundlage spezialisierter Images erstellen – ausgehend von einer Windows-VHD oder -VHDX. Mithilfe dieses spezialisierten Images können Sie VM-Images für die Verwendung mit virtuellen Computern erstellen, die auf Ihrem Azure Stack Edge Pro-GPU-Gerät bereitgestellt werden.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: how-to
ms.date: 04/15/2021
ms.author: alkohli
ms.openlocfilehash: 6bfa42e99f295b429eba40a27eb59becb8aa80a1
ms.sourcegitcommit: d3bcd46f71f578ca2fd8ed94c3cdabe1c1e0302d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2021
ms.locfileid: "107575944"
---
# <a name="deploy-a-vm-from-a-specialized-image-on-your-azure-stack-edge-pro-gpu-device-via-azure-powershell"></a>Bereitstellen einer VM auf der Grundlage eines spezialisierten Images auf Ihrem Azure Stack Edge Pro-GPU-Gerät über Azure PowerShell 

[!INCLUDE [applies-to-GPU-and-pro-r-and-mini-r-skus](../../includes/azure-stack-edge-applies-to-gpu-pro-r-mini-r-sku.md)]

In diesem Artikel werden die Schritte beschrieben, die ausgeführt werden müssen, um eine VM (virtual machine, virtueller Computer) auf der Grundlage eines spezialisierten Images auf Ihrem Azure Stack Edge Pro-GPU-Gerät bereitzustellen. 

Informationen zum Vorbereiten eines generalisierten Images für die Bereitstellung von VMs in Azure Stack Edge Pro-GPU finden Sie unter [Verwenden eines generalisierten Images der Windows-VHD zum Erstellen eines VM-Images für Ihr Azure Stack Edge Pro-Gerät](azure-stack-edge-gpu-prepare-windows-vhd-generalized-image.md) und [Prepare generalized image from ISO](azure-stack-edge-gpu-prepare-windows-generalized-image-iso.md) (Vorbereiten eines generalisierten Images aus einer ISO-Datei).

## <a name="about-vm-images"></a>Informationen zu VM-Images

Sie können eine Windows-VHD oder -VHDX verwenden, um ein *spezialisiertes* Image oder ein *generalisiertes* Image zu erstellen. In der folgenden Tabelle sind die wichtigsten Unterschiede zwischen *spezialisierten* und *generalisierten* Images zusammengefasst:

[!INCLUDE [about-vm-images-for-azure-stack-edge](../../includes/azure-stack-edge-about-vm-images.md)]

## <a name="workflow"></a>Workflow

Der allgemeine Workflow zum Bereitstellen einer VM auf der Grundlage eines spezialisierten Images sieht wie folgt aus:

1. Kopieren Sie die VHD-Datei in ein lokales Speicherkonto auf Ihrem Azure Stack Edge Pro-GPU-Gerät.
1. Erstellen Sie auf der Grundlage der VHD einen neuen verwalteten Datenträger.
1. Erstellen Sie auf der Grundlage des verwalteten Datenträgers einen neuen virtuellen Computer, und fügen Sie den verwalteten Datenträger an.

## <a name="prerequisites"></a>Voraussetzungen

Um einen virtuellen Computer mithilfe von PowerShell auf Ihrem Gerät bereitstellen zu können, muss Folgendes erfüllt sein:

- Sie haben Zugriff auf einen Client für die Verbindungsherstellung mit Ihrem Gerät.
    - Ihr Client verfügt über ein [unterstütztes Betriebssystem](azure-stack-edge-gpu-system-requirements.md#supported-os-for-clients-connected-to-device).
    - Ihr Client ist so konfiguriert, dass er eine Verbindung mit der lokalen Azure Resource Manager-Instanz Ihres Geräts herstellt (siehe [Herstellen einer Verbindung mit Azure Resource Manager auf Ihrem Azure Stack Edge Pro-Gerät](azure-stack-edge-gpu-connect-resource-manager.md)).

## <a name="verify-the-local-azure-resource-manager-connection"></a>Überprüfen der Verbindung mit der lokalen Azure Resource Manager-Instanz

Vergewissern Sie sich, dass Ihr Client eine Verbindung mit der lokalen Azure Resource Manager-Instanz herstellen kann. 

1. Rufen Sie zur Authentifizierung lokale Geräte-APIs auf:

    ```powershell
    Login-AzureRMAccount -EnvironmentName <Environment Name>
    ```

2. Geben Sie den Benutzernamen `EdgeArmUser` und das Kennwort für die Verbindung über Azure Resource Manager an. Sollten Sie Ihr Kennwort vergessen haben, [setzen Sie das Kennwort für Azure Resource Manager zurück](azure-stack-edge-gpu-set-azure-resource-manager-password.md), und melden Sie sich mit diesem Kennwort an.

## <a name="deploy-vm-from-specialized-image"></a>Bereitstellen eines virtuellen Computers auf der Grundlage eines spezialisierten Images

In den folgenden Abschnitten erfahren Sie Schritt für Schritt, wie Sie einen virtuellen Computer auf der Grundlage eines spezialisierten Images bereitstellen.

## <a name="copy-vhd-to-local-storage-account-on-device"></a>Kopieren der VHD in ein lokales Speicherkonto auf dem Gerät

Gehen Sie wie folgt vor, um die VHD in ein lokales Speicherkonto zu kopieren:

1. Kopieren Sie die Quell-VHD in ein lokales Blobspeicherkonto auf Ihrem Azure Stack Edge-Gerät.

1. Notieren Sie sich den resultierenden URI. Er wird in einem späteren Schritt benötigt.

    Informationen zum Erstellen eines lokalen Speicherkontos sowie zum Zugreifen auf ein solches Konto finden Sie in den Abschnitten [Speicherkonto erstellen](azure-stack-edge-gpu-deploy-virtual-machine-powershell.md#create-a-storage-account) bis [Hochladen einer VHD-Datei](azure-stack-edge-gpu-deploy-virtual-machine-powershell.md#upload-a-vhd) des Artikels [Bereitstellen von VMs auf Ihrem Azure Stack Edge-Gerät über Azure PowerShell](azure-stack-edge-gpu-deploy-virtual-machine-powershell.md). 

## <a name="create-a-managed-disk-from-vhd"></a>Erstellen eines verwalteten Datenträgers auf der Grundlage einer VHD

Gehen Sie wie folgt vor, um einen verwalteten Datenträger auf der Grundlage einer VHD zu erstellen, die Sie zuvor in das Speicherkonto hochgeladen haben:

1. Legen Sie einige Parameter fest.

    ```powershell
    $VhdURI = <URI of VHD in local storage account>
    $DiskRG = <managed disk resource group>
    $DiskName = <managed disk name>    
    ```
    Hier sehen Sie eine Beispielausgabe.

    ```powershell
    PS C:\WINDOWS\system32> $VHDURI = "https://myasevmsa.blob.myasegpudev.wdshcsso.com/vhds/WindowsServer2016Datacenter.vhd"
    PS C:\WINDOWS\system32> $DiskRG = "myasevm1rg"
    PS C:\WINDOWS\system32> $DiskName = "myasemd1"
    ```
1. Erstellen Sie einen neuen verwalteten Datenträger.

    ```powershell
    $DiskConfig = New-AzureRmDiskConfig -Location DBELocal -CreateOption Import -SourceUri $VhdURI
    $disk = New-AzureRMDisk -ResourceGroupName $DiskRG -DiskName $DiskName -Disk $DiskConfig
    ```

    Hier sehen Sie eine Beispielausgabe. Der Speicherort ist hier auf den Speicherort des lokalen Speicherkontos festgelegt und lautet für alle lokalen Speicherkonten auf Ihrem Azure Stack Edge Pro-GPU-Gerät immer `DBELocal`. 

    ```powershell
    PS C:\WINDOWS\system32> $DiskConfig = New-AzureRmDiskConfig -Location DBELocal -CreateOption Import -SourceUri $VHDURI
    PS C:\WINDOWS\system32> $disk = New-AzureRMDisk -ResourceGroupName $DiskRG -DiskName $DiskName -Disk $DiskConfig
    PS C:\WINDOWS\system32>    
    ```
## <a name="create-a-vm-from-managed-disk"></a>Erstellen eines virtuellen Computers auf der Grundlage eines verwalteten Datenträgers

Gehen Sie wie folgt vor, um einen virtuellen Computer auf der Grundlage eines verwalteten Datenträgers zu erstellen:

1. Legen Sie einige Parameter fest.

    ```powershell
    $NicRG = <NIC resource group>
    $NicName = <NIC name>
    $IPConfigName = <IP config name>
    $PrivateIP = <IP address> #Optional
    
    $VMRG = <VM resource group>
    $VMName = <VM name>
    $VMSize = <VM size> 
    ```

    >[!NOTE]
    > Das `PrivateIP` ist optional. Verwenden Sie diesen Parameter, um eine statische IP zuzuweisen. Andernfalls wird standardmäßig eine dynamische IP-Adresse mit DHCP verwendet.

    Hier sehen Sie eine Beispielausgabe. In diesem Beispiel wird für alle VM-Ressourcen die gleiche Ressourcengruppe angegeben. Sie können bei Bedarf aber auch separate Ressourcengruppen für die Ressourcen erstellen und angeben.

    ```powershell
    PS C:\WINDOWS\system32> $NicRG = "myasevm1rg"
    PS C:\WINDOWS\system32> $NicName = "myasevmnic1"
    PS C:\WINDOWS\system32> $IPConfigName = "myaseipconfig1" 

    PS C:\WINDOWS\system32> $VMRG = "myasevm1rg"
    PS C:\WINDOWS\system32> $VMName = "myasetestvm1"
    PS C:\WINDOWS\system32> $VMSize = "Standard_D1_v2"   
    ```

1. Rufen Sie die Informationen zum virtuellen Netzwerk ab, und erstellen Sie eine neue Netzwerkschnittstelle.

    In diesem Beispiel wird davon ausgegangen, dass Sie eine einzelne Netzwerkschnittstelle im standardmäßigen virtuellen Netzwerk `ASEVNET` erstellen, das der Standardressourcengruppe `ASERG` zugeordnet ist. Bei Bedarf können Sie ein alternatives virtuelles Netzwerk angeben oder mehrere Netzwerkschnittstellen erstellen. Weitere Informationen finden Sie unter [Verwalten von Netzwerkschnittstellen für die virtuellen Computer auf Ihrem Azure Stack Edge Pro-GPU-Gerät über das Azure-Portal](azure-stack-edge-gpu-manage-virtual-machine-network-interfaces-portal.md).

    ```powershell
    $armVN = Get-AzureRMVirtualNetwork -Name ASEVNET -ResourceGroupName ASERG
    $ipConfig = New-AzureRmNetworkInterfaceIpConfig -Name $IPConfigName -SubnetId $armVN.Subnets[0].Id [-PrivateIpAddress $PrivateIP]
    $nic = New-AzureRmNetworkInterface -Name $NicName -ResourceGroupName $NicRG -Location DBELocal -IpConfiguration $ipConfig
    ```

    Hier sehen Sie eine Beispielausgabe.

    ```powershell
    PS C:\WINDOWS\system32> $armVN = Get-AzureRMVirtualNetwork -Name ASEVNET -ResourceGroupName ASERG
    PS C:\WINDOWS\system32> $ipConfig = New-AzureRmNetworkInterfaceIpConfig -Name $IPConfigName -SubnetId $armVN.Subnets[0].Id
    PS C:\WINDOWS\system32> $nic = New-AzureRmNetworkInterface -Name $NicName -ResourceGroupName $NicRG -Location DBELocal -IpConfiguration $ipConfig
    WARNING: The output object type of this cmdlet will be modified in a future release.
    PS C:\WINDOWS\system32>    
    ```
1. Erstellen Sie ein neues VM-Konfigurationsobjekt.

    ```powershell
    $vmConfig = New-AzureRmVMConfig -VMName $VMName -VMSize $VMSize
    ```

    
1. Fügen Sie die Netzwerkschnittstelle dem virtuellen Computer hinzu.

    ```powershell
    $vm = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $nic.Id
    ```

1. Legen Sie die Eigenschaften des Betriebssystemdatenträgers für den virtuellen Computer fest.

    ```powershell
    $vm = Set-AzureRmVMOSDisk -VM $vm -ManagedDiskId $disk.Id -StorageAccountType StandardLRS -CreateOption Attach –[Windows/Linux]
    ```
    Das letzte Flag in diesem Befehl ist entweder `-Windows` oder `-Linux`, je nachdem, welches Betriebssystem Sie für Ihren virtuellen Computer verwenden.

1. Erstellen Sie den virtuellen Computer.

    ```powershell
    New-AzureRmVM -ResourceGroupName $VMRG -Location DBELocal -VM $vm 
    ```

    Hier sehen Sie eine Beispielausgabe. 

    ```powershell
    PS C:\WINDOWS\system32> $vmConfig = New-AzureRmVMConfig -VMName $VMName -VMSize $VMSize
    PS C:\WINDOWS\system32> $vm = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $nic.Id
    PS C:\WINDOWS\system32> $vm = Set-AzureRmVMOSDisk -VM $vm -ManagedDiskId $disk.Id -StorageAccountType StandardLRS -CreateOption Attach -Windows
    PS C:\WINDOWS\system32> New-AzureRmVM -ResourceGroupName $VMRG -Location DBELocal -VM $vm
    WARNING: Since the VM is created using premium storage or managed disk, existing standard storage account, myasevmsa, is used for
    boot diagnostics.    
    RequestId IsSuccessStatusCode StatusCode ReasonPhrase
    --------- ------------------- ---------- ------------
                             True         OK OK        
    PS C:\WINDOWS\system32>
    ```

## <a name="delete-vm-and-resources"></a>Löschen des virtuellen Computers und der Ressourcen

In diesem Artikel wurde zum Erstellen der gesamten VM-Ressourcen nur eine einzelne Ressourcengruppe verwendet. Wenn Sie diese Ressourcengruppe löschen, werden dadurch die VM und alle zugehörigen Ressourcen gelöscht. 

1. Zeigen Sie zuerst alle Ressourcen an, die unter der Ressourcengruppe erstellt wurden.

    ```powershell
    Get-AzureRmResource -ResourceGroupName <Resource group name>
    ```
    Hier sehen Sie eine Beispielausgabe.
    
    ```powershell
    PS C:\WINDOWS\system32> Get-AzureRmResource -ResourceGroupName myasevm1rg
    
    
    Name              : myasemd1
    ResourceGroupName : myasevm1rg
    ResourceType      : Microsoft.Compute/disks
    Location          : dbelocal
    ResourceId        : /subscriptions/992601bc-b03d-4d72-598e-d24eac232122/resourceGroups/myasevm1rg/providers/Microsoft.Compute/disk
                        s/myasemd1
    
    Name              : myasetestvm1
    ResourceGroupName : myasevm1rg
    ResourceType      : Microsoft.Compute/virtualMachines
    Location          : dbelocal
    ResourceId        : /subscriptions/992601bc-b03d-4d72-598e-d24eac232122/resourceGroups/myasevm1rg/providers/Microsoft.Compute/virt
                        ualMachines/myasetestvm1
    
    Name              : myasevmnic1
    ResourceGroupName : myasevm1rg
    ResourceType      : Microsoft.Network/networkInterfaces
    Location          : dbelocal
    ResourceId        : /subscriptions/992601bc-b03d-4d72-598e-d24eac232122/resourceGroups/myasevm1rg/providers/Microsoft.Network/netw
                        orkInterfaces/myasevmnic1
    
    Name              : myasevmsa
    ResourceGroupName : myasevm1rg
    ResourceType      : Microsoft.Storage/storageaccounts
    Location          : dbelocal
    ResourceId        : /subscriptions/992601bc-b03d-4d72-598e-d24eac232122/resourceGroups/myasevm1rg/providers/Microsoft.Storage/stor
                        ageaccounts/myasevmsa
    
    PS C:\WINDOWS\system32>
    ```

1. Löschen Sie die Ressourcengruppe und alle zugeordneten Ressourcen.

    ```powershell
    Remove-AzureRmResourceGroup -ResourceGroupName <Resource group name>
    ```
    Hier sehen Sie eine Beispielausgabe.
    
    ```powershell
    PS C:\WINDOWS\system32> Remove-AzureRmResourceGroup -ResourceGroupName myasevm1rg
    
    Confirm
    Are you sure you want to remove resource group 'myasevm1rg'
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): Y
    True
    PS C:\WINDOWS\system32>
    ```

1. Vergewissern Sie sich, dass die Ressourcengruppe gelöscht wurde. Rufen Sie alle Ressourcengruppen ab, die auf dem Gerät vorhanden sind. 

    ```powershell
    Get-AzureRmResourceGroup
    ```
    Hier sehen Sie eine Beispielausgabe.

    ```powershell
    PS C:\WINDOWS\system32> Get-AzureRmResourceGroup

    ResourceGroupName : ase-image-resourcegroup
    Location          : dbelocal
    ProvisioningState : Succeeded
    Tags              :
    ResourceId        : /subscriptions/992601bc-b03d-4d72-598e-d24eac232122/resourceGroups/ase-image-resourcegroup
    
    ResourceGroupName : ASERG
    Location          : dbelocal
    ProvisioningState : Succeeded
    Tags              :
    ResourceId        : /subscriptions/992601bc-b03d-4d72-598e-d24eac232122/resourceGroups/ASERG
    
    ResourceGroupName : myaserg
    Location          : dbelocal
    ProvisioningState : Succeeded
    Tags              :
    ResourceId        : /subscriptions/992601bc-b03d-4d72-598e-d24eac232122/resourceGroups/myaserg
        
    PS C:\WINDOWS\system32>
    ```

## <a name="next-steps"></a>Nächste Schritte

- [Vorbereiten eines generalisierten Images aus einer Windows-VHD zum Bereitstellen von VMs auf Azure Stack Edge Pro-GPU-Geräten](azure-stack-edge-gpu-prepare-windows-vhd-generalized-image.md)
- [Vorbereiten eines generalisierten Images aus einer ISO-Datei zum Bereitstellen von VMs auf Azure Stack Edge Pro-GPU-Geräten](azure-stack-edge-gpu-prepare-windows-generalized-image-iso.md)