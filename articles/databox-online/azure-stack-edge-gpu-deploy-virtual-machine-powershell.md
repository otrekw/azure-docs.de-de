---
title: Bereitstellen von VMs auf Ihrem Azure Stack Edge-Gerät über Azure PowerShell
description: Hier erfahren Sie, wie virtuelle Computer (Virtual Machines, VMs) auf einem Azure Stack Edge-Gerät mithilfe von Azure PowerShell erstellt und verwaltet werden.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: how-to
ms.date: 02/22/2021
ms.author: alkohli
ms.openlocfilehash: 90925f30e84ec3c06e715669ebd982c823dfaf5a
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "105568323"
---
# <a name="deploy-vms-on-your-azure-stack-edge-device-via-azure-powershell"></a>Bereitstellen von VMs auf Ihrem Azure Stack Edge-Gerät über Azure PowerShell

[!INCLUDE [applies-to-GPU-and-pro-r-and-mini-r-skus](../../includes/azure-stack-edge-applies-to-gpu-pro-r-mini-r-sku.md)]

In diesem Artikel wird beschrieben, wie Sie eine einen virtuellen Computer (Virtual Machine, VM) mithilfe von Azure PowerShell auf Ihrem Azure Stack Edge-Gerät erstellen und verwalten können. Die Informationen gelten für Azure Stack Edge Pro mit GPU (Graphical Processing Unit)-, Azure Stack Edge Pro R- und Azure Stack Edge Mini R-Geräte.

## <a name="vm-deployment-workflow"></a>VM-Bereitstellungsworkflow

Der Bereitstellungsworkflow wird im folgenden Diagramm gezeigt:

![Diagramm des VM-Bereitstellungsworkflows.](media/azure-stack-edge-gpu-deploy-virtual-machine-powershell/vm-workflow-r.svg)

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [azure-stack-edge-gateway-deploy-vm-prerequisites](../../includes/azure-stack-edge-gateway-deploy-virtual-machine-prerequisites.md)]


## <a name="query-for-a-built-in-subscription-on-the-device"></a>Abfrage für ein integriertes Abonnement auf dem Gerät

Bei Azure Resource Manager wird nur ein einziges für Benutzer sichtbares festes Abonnement unterstützt. Dieses Abonnement ist pro Gerät eindeutig. Der Abonnementname und die Abonnement-ID können nicht geändert werden.

Das Abonnement enthält alle Ressourcen, die für eine VM-Erstellung erforderlich sind. 

> [!IMPORTANT]
> Das Abonnement wird erstellt, wenn Sie VMs im Azure-Portal aktivieren, und es befindet sich lokal auf Ihrem Gerät.

Das Abonnement wird zur Bereitstellung der VMs verwendet.

1.  Führen Sie den folgenden Befehl zum Auflisten des Abonnements aus:

    ```powershell
    Get-AzureRmSubscription
    ```
    
    Hier ist eine Beispielausgabe angegeben:

    ```powershell
    PS C:\windows\system32> Get-AzureRmSubscription
    
    Name                 Id                 TenantId          State
    ----                 --                --------           -----
    Default Provider Subscription A4257FDE-B946-4E01-ADE7-674760B8D1A3 c0257de7-538f-415c-993a-1b87a031879d Enabled
    
    PS C:\windows\system32>
    ```
        
1. Rufen Sie eine Liste der registrierten Ressourcenanbieter ab, die auf dem Gerät ausgeführt werden. Die Liste enthält normalerweise Compute, Netzwerk und Speicher.

    ```powershell
    Get-AzureRMResourceProvider
    ```

    > [!NOTE]
    > Die Ressourcenanbieter wurden vorab registriert, und sie können nicht bearbeitet oder geändert werden.
    
    Hier ist eine Beispielausgabe angegeben:

    ```powershell
    Get-AzureRmResourceProvider
    ProviderNamespace : Microsoft.Compute
    RegistrationState : Registered
    ResourceTypes     : {virtualMachines, virtualMachines/extensions, locations, operations...}
    Locations         : {DBELocal}
    ZoneMappings      :
    
    ProviderNamespace : Microsoft.Network
    RegistrationState : Registered
    ResourceTypes     : {operations, locations, locations/operations, locations/usages...}
    Locations         : {DBELocal}
    ZoneMappings      :
    
    ProviderNamespace : Microsoft.Resources
    RegistrationState : Registered
    ResourceTypes     : {tenants, locations, providers, checkresourcename...}
    Locations         : {DBELocal}
    ZoneMappings      :
    
    ProviderNamespace : Microsoft.Storage
    RegistrationState : Registered
    ResourceTypes     : {storageaccounts, storageAccounts/blobServices, storageAccounts/tableServices,
                        storageAccounts/queueServices...}
    Locations         : {DBELocal}
    ZoneMappings      :
    ```
    
## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Erstellen Sie mit [New-AzureRmResourceGroup](/powershell/module/az.resources/new-azresourcegroup) eine Azure-Ressourcengruppe. Eine Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen wie ein Speicherkonto, ein Datenträger und ein verwalteter Datenträger bereitgestellt und verwaltet werden.

> [!IMPORTANT]
> Alle Ressourcen werden im Speicherort des Geräts erstellt, der auf **DBELocal** festgelegt wird.

```powershell
New-AzureRmResourceGroup -Name <Resource group name> -Location DBELocal
```

Hier ist eine Beispielausgabe angegeben:

```powershell
New-AzureRmResourceGroup -Name rg191113014333 -Location DBELocal 
Successfully created Resource Group:rg191113014333
```

## <a name="create-a-storage-account"></a>Speicherkonto erstellen

Erstellen Sie mithilfe der im vorhergehenden Schritt erstellten Ressourcengruppe ein neues Speicherkonto. Dabei handelt es sich um ein lokales Speicherkonto, das Sie zum Hochladen des virtuellen Datenträgerimages für die VM verwenden.

```powershell
New-AzureRmStorageAccount -Name <Storage account name> -ResourceGroupName <Resource group name> -Location DBELocal -SkuName Standard_LRS
```

> [!NOTE]
> Mithilfe von Azure Resource Manager können Sie nur lokale Speicherkonten erstellen, beispielsweise lokal redundanten Speicher (Standard oder Premium). Informationen zum Erstellen von mehrstufigen Speicherkonten finden Sie im [Tutorial: Übertragen von Daten über Speicherkonten mit Azure Stack Edge Pro mit GPU](./azure-stack-edge-gpu-deploy-add-storage-accounts.md).

Hier ist eine Beispielausgabe angegeben:

```powershell
New-AzureRmStorageAccount -Name sa191113014333  -ResourceGroupName rg191113014333 -SkuName Standard_LRS -Location DBELocal

ResourceGroupName      : rg191113014333
StorageAccountName     : sa191113014333
Id                     : /subscriptions/a4257fde-b946-4e01-ade7-674760b8d1a3/resourceGroups/rg191113014333/providers/Microsoft.Storage/storageaccounts/sa191113014333
Location               : DBELocal
Sku                    : Microsoft.Azure.Management.Storage.Models.Sku
Kind                   : Storage
Encryption             : Microsoft.Azure.Management.Storage.Models.Encryption
AccessTier             :
CreationTime           : 11/13/2019 9:43:49 PM
CustomDomain           :
Identity               :
LastGeoFailoverTime    :
PrimaryEndpoints       : Microsoft.Azure.Management.Storage.Models.Endpoints
PrimaryLocation        : DBELocal
ProvisioningState      : Succeeded
SecondaryEndpoints     :
SecondaryLocation      :
StatusOfPrimary        : Available
StatusOfSecondary      :
Tags                   :
EnableHttpsTrafficOnly : False
NetworkRuleSet         :
Context                : Microsoft.WindowsAzure.Commands.Common.Storage.LazyAzureStorageContext
ExtendedProperties     : {}
```

Führen Sie den Befehl `Get-AzureRmStorageAccountKey` aus, um den Speicherkontoschlüssel abzurufen. Hier ist eine Beispielausgabe angegeben:

```powershell
PS C:\Users\Administrator> Get-AzureRmStorageAccountKey

cmdlet Get-AzureRmStorageAccountKey at command pipeline position 1
Supply values for the following parameters:
(Type !? for Help.)
ResourceGroupName: my-resource-ase
Name:myasestoracct

KeyName Value
------- -----
key1 /IjVJN+sSf7FMKiiPLlDm8mc9P4wtcmhhbnCa7...
key2 gd34TcaDzDgsY9JtDNMUgLDOItUU0Qur3CBo6Q...
```

## <a name="add-the-blob-uri-to-the-host-file"></a>Hinzufügen des Blob-URI zur Hostdatei

Sie haben bereits den Blob-URI in der Hostdatei für den Client hinzugefügt, über den Sie eine Verbindung mit Azure Blob Storage in „Schritt 5: Ändern der Hostdatei für die Endpunktnamensauflösung“ unter [Bereitstellen von VMS auf Ihrem Azure Stack Edge-Gerät über Azure PowerShell](./azure-stack-edge-gpu-connect-resource-manager.md#step-5-modify-host-file-for-endpoint-name-resolution) hergestellt haben. Dieser Eintrag wurde verwendet, um den BLOB-URI hinzuzufügen:

\<Azure consistent network services VIP \> \<storage name\>.blob.\<appliance name\>.\<dnsdomain\>

## <a name="install-certificates"></a>Installieren von Zertifikaten

Wenn Sie HTTPS verwenden, müssen Sie die erforderlichen Zertifikate auf Ihrem Gerät installieren. Hier installieren Sie das Blobendpunktzertifikat. Weitere Informationen finden Sie unter [Verwenden von Zertifikaten bei Ihrem Azure Stack Edge Pro mit GPU-Gerät](azure-stack-edge-gpu-manage-certificates.md).

## <a name="upload-a-vhd"></a>Hochladen einer VHD-Datei

Kopieren Sie die zu verwendenden Datenträgerimages in Seitenblobs in dem lokalen Speicherkonto, das Sie vorher erstellt haben. Sie können ein Tool wie [AzCopy](../storage/common/storage-use-azcopy-v10.md) zum Hochladen der virtuellen Festplatte (Virtual Hard Disk, VHD) in das Speicherkonto verwenden. 

<!--Before you use AzCopy, make sure that the [AzCopy is configured correctly](#configure-azcopy) for use with the blob storage REST API version that you're using with your Azure Stack Edge Pro device.

```powershell
AzCopy /Source:<sourceDirectoryForVHD> /Dest:<blobContainerUri> /DestKey:<storageAccountKey> /Y /S /V /NC:32  /BlobType:page /destType:blob 
```

> [!NOTE]
> Set `BlobType` to `page` for creating a managed disk out of VHD. Set `BlobType` to `block` when you're writing to tiered storage accounts by using AzCopy.

You can download the disk images from Azure Marketplace. For more information, see [Get the virtual disk image from Azure Marketplace](azure-stack-edge-j-series-create-virtual-machine-image.md).

Here's some example output that uses AzCopy 7.3. For more information about this command, see [Upload VHD file to storage account by using AzCopy](../devtest-labs/devtest-lab-upload-vhd-using-azcopy.md).


```powershell
AzCopy /Source:\\hcsfs\scratch\vm_vhds\linux\ /Dest:http://sa191113014333.blob.dbe-1dcmhq2.microsoftdatabox.com/vmimages /DestKey:gJKoyX2Amg0Zytd1ogA1kQ2xqudMHn7ljcDtkJRHwMZbMK== /Y /S /V /NC:32 /BlobType:page /destType:blob /z:2e7d7d27-c983-410c-b4aa-b0aa668af0c6
```-->
Verwenden Sie mit AzCopy 10 die folgenden Befehle:  

```powershell
$StorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName <ResourceGroupName> -Name <StorageAccountName>)[0].Value

$endPoint = (Get-AzureRmStorageAccount -name <StorageAccountName> -ResourceGroupName <ResourceGroupName>).PrimaryEndpoints.Blob

$StorageAccountContext = New-AzureStorageContext -StorageAccountName <StorageAccountName> -StorageAccountKey <StorageAccountKey> -Endpoint <Endpoint>

$StorageAccountSAS = New-AzureStorageAccountSASToken -Service Blob,File,Queue,Table -ResourceType Container,Service,Object -Permission "acdlrw" -Context <StorageAccountContext> -Protocol HttpsOnly

<AzCopy exe path> cp "Full VHD path" "<BlobEndPoint>/<ContainerName><StorageAccountSAS>"
```

Hier ist eine Beispielausgabe angegeben: 

```powershell
$ContainerName = <ContainerName>
$ResourceGroupName = <ResourceGroupName>
$StorageAccountName = <StorageAccountName>
$VHDPath = "Full VHD Path"
$VHDFile = <VHDFileName>

$StorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $ResourceGroupName -Name $StorageAccountName)[0].Value

$endPoint = (Get-AzureRmStorageAccount -name $StorageAccountName -resourcegroupname $ResourceGroupName).PrimaryEndpoints.Blob

$StorageAccountContext = New-AzureStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey -Endpoint $endPoint

$StorageAccountSAS = New-AzureStorageAccountSASToken -Service Blob,File,Queue,Table -ResourceType Container,Service,Object -Permission "acdlrw" -Context $StorageAccountContext -Protocol HttpsOnly

C:\AzCopy.exe  cp "$VHDPath\$VHDFile" "$endPoint$ContainerName$StorageAccountSAS"
```

## <a name="create-a-managed-disk-from-the-vhd"></a>Erstellen verwalteter Datenträger aus der VHD

Wenn Sie einen verwalteten Datenträger aus der hochgeladenen VHD erstellen möchten, führen Sie den folgenden Befehl aus:

```powershell
$DiskConfig = New-AzureRmDiskConfig -Location DBELocal -CreateOption Import -SourceUri "Source URL for your VHD"
```
Hier ist eine Beispielausgabe angegeben: 

<code>
$DiskConfig = New-AzureRmDiskConfig -Location DBELocal -CreateOption Import –SourceUri http://</code><code>sa191113014333.blob.dbe-1dcmhq2.microsoftdatabox.com/vmimages/ubuntu13.vhd</code> 

```powershell
New-AzureRMDisk -ResourceGroupName <Resource group name> -DiskName <Disk name> -Disk $DiskConfig
```

Hier ist eine Beispielausgabe angegeben. Weitere Informationen zu diesem Cmdlet finden Sie unter [New-AzureRmDisk](/powershell/module/azurerm.compute/new-azurermdisk?view=azurermps-6.13.0&preserve-view=true).

```powershell
Tags               :
New-AzureRmDisk -ResourceGroupName rg191113014333 -DiskName ld191113014333 -Disk $DiskConfig

ResourceGroupName  : rg191113014333
ManagedBy          :
Sku                : Microsoft.Azure.Management.Compute.Models.DiskSku
Zones              :
TimeCreated        : 11/13/2019 1:49:07 PM
OsType             :
CreationData       : Microsoft.Azure.Management.Compute.Models.CreationData
DiskSizeGB         : 30
EncryptionSettings :
ProvisioningState  : Succeeded
Id                 : /subscriptions/a4257fde-b946-4e01-ade7-674760b8d1a3/resourceGroups/rg191113014333/providers/Micros
                     oft.Compute/disks/ld191113014333
Name               : ld191113014333
Type               : Microsoft.Compute/disks
Location           : DBELocal
Tags               : {}
```

## <a name="create-a-vm-image-from-the-image-managed-disk"></a>Erstellen eines VM-Images aus dem verwalteten Imagedatenträger

Wenn Sie ein VM-Image aus dem verwalteten Datenträger erstellen möchten, führen Sie den folgenden Befehl aus. Ersetzen Sie *\<Disk name>* , *\<OS type>* und *\<Disk size>* durch echte Werte.

```powershell
$imageConfig = New-AzureRmImageConfig -Location DBELocal
$ManagedDiskId = (Get-AzureRmDisk -Name <Disk name> -ResourceGroupName <Resource group name>).Id
Set-AzureRmImageOsDisk -Image $imageConfig -OsType '<OS type>' -OsState 'Generalized' -DiskSizeGB <Disk size> -ManagedDiskId $ManagedDiskId 

The supported OS types are Linux and Windows.

For OS Type=Linux, for example:
Set-AzureRmImageOsDisk -Image $imageConfig -OsType 'Linux' -OsState 'Generalized' -DiskSizeGB <Disk size> -ManagedDiskId $ManagedDiskId
New-AzureRmImage -Image $imageConfig -ImageName <Image name>  -ResourceGroupName <Resource group name>
```

Hier ist eine Beispielausgabe. Weitere Informationen zu diesem Cmdlet finden Sie unter [New-AzureRmImage](/powershell/module/azurerm.compute/new-azurermimage?view=azurermps-6.13.0&preserve-view=true).

```powershell
New-AzureRmImage -Image Microsoft.Azure.Commands.Compute.Automation.Models.PSImage -ImageName ig191113014333  -ResourceGroupName rg191113014333
ResourceGroupName    : rg191113014333
SourceVirtualMachine :
StorageProfile       : Microsoft.Azure.Management.Compute.Models.ImageStorageProfile
ProvisioningState    : Succeeded
Id                   : /subscriptions/a4257fde-b946-4e01-ade7-674760b8d1a3/resourceGroups/rg191113014333/providers/Micr
                       osoft.Compute/images/ig191113014333
Name                 : ig191113014333
Type                 : Microsoft.Compute/images
Location             : dbelocal
Tags                 : {}
```

## <a name="create-your-vm-with-previously-created-resources"></a>Erstellen Ihres virtuellen Computers mit zuvor erstellten Ressourcen

Bevor Sie den virtuellen Computer erstellen und bereitstellen, müssen Sie ein einziges virtuelles Netzwerk erstellen und ihm eine virtuelle Netzwerkschnittstelle zuordnen.

> [!IMPORTANT]
> Es gelten die folgenden Regeln:
> - Sie können nur ein einziges virtuelles Netzwerk erstellen, sogar über Ressourcengruppen hinweg. Das virtuelle Netzwerk muss genau denselben Adressraum wie das logische Netzwerk haben.
> - Das virtuelle Netzwerk kann nur ein einzelnes Subnetz enthalten. Das Subnetz muss genau denselben Adressraum wie das virtuelle Netzwerk haben.
> - Wenn Sie die virtuelle Netzwerkschnittstellenkarte erstellen, können Sie nur die statische Zuordnungsmethode verwenden. Der Benutzer muss eine private IP-Adresse angeben.

### <a name="query-the-automatically-created-virtual-network"></a>Abfragen des automatisch erstellten virtuellen Netzwerks

Wenn Sie Compute über die lokale Benutzeroberfläche Ihres Geräts aktivieren, wird in der Ressourcengruppe `ASERG` das virtuelle Netzwerk `ASEVNET` automatisch erstellt. 

Fragen Sie mit dem folgenden Befehl das vorhandene virtuelle Netzwerk ab:

```powershell
$aRmVN = Get-AzureRMVirtualNetwork -Name ASEVNET -ResourceGroupName ASERG 
```

<!--```powershell
$subNetId=New-AzureRmVirtualNetworkSubnetConfig -Name <Subnet name> -AddressPrefix <Address Prefix>
$aRmVN = New-AzureRmVirtualNetwork -ResourceGroupName <Resource group name> -Name <Vnet name> -Location DBELocal -AddressPrefix <Address prefix> -Subnet $subNetId
```-->

### <a name="create-a-virtual-network-interface-card"></a>Erstellen einer virtuellen Netzwerkadapterkarte

Wenn Sie eine virtuelle Netzwerkschnittstellenkarte unter Angabe der ID für das Subnetz des virtuellen Netzwerks erstellen möchten, führen Sie den folgenden Befehl aus:

```powershell
$ipConfig = New-AzureRmNetworkInterfaceIpConfig -Name <IP config Name> -SubnetId $aRmVN.Subnets[0].Id -PrivateIpAddress <Private IP>
$Nic = New-AzureRmNetworkInterface -Name <Nic name> -ResourceGroupName <Resource group name> -Location DBELocal -IpConfiguration $ipConfig
```

Hier ist eine Beispielausgabe angegeben:

```powershell
PS C:\Users\Administrator> $subNetId=New-AzureRmVirtualNetworkSubnetConfig -Name my-ase-subnet -AddressPrefix "5.5.0.0/16"

PS C:\Users\Administrator> $aRmVN = New-AzureRmVirtualNetwork -ResourceGroupName Resource-my-ase -Name my-ase-virtualnetwork -Location DBELocal -AddressPrefix "5.5.0.0/16" -Subnet $subNetId
WARNING: The output object type of this cmdlet will be modified in a future release.
PS C:\Users\Administrator> $ipConfig = New-AzureRmNetworkInterfaceIpConfig -Name my-ase-ip -SubnetId $aRmVN.Subnets[0].Id
PS C:\Users\Administrator> $Nic = New-AzureRmNetworkInterface -Name my-ase-nic -ResourceGroupName Resource-my-ase -Location DBELocal -IpConfiguration $ipConfig
WARNING: The output object type of this cmdlet will be modified in a future release.

PS C:\Users\Administrator> $Nic

PS C:\Users\Administrator> (Get-AzureRmNetworkInterface)[0]

Name                        : nic200108020444
ResourceGroupName           : rg200108020444
Location                    : dbelocal
Id                          : /subscriptions/a4257fde-b946-4e01-ade7-674760b8d1a3/resourceGroups/rg200108020444/providers/Microsoft.Network/networ
                              kInterfaces/nic200108020444
Etag                        : W/"f9d1759d-4d49-42fa-8826-e218e0b1d355"
ResourceGuid                : 3851ae62-c13e-4416-9386-e21d9a2fef0f
ProvisioningState           : Succeeded
Tags                        :
VirtualMachine              : {
                                "Id": "/subscriptions/a4257fde-b946-4e01-ade7-674760b8d1a3/resourceGroups/rg200108020444/providers/Microsoft.Compu
                              te/virtualMachines/VM200108020444"
                              }
IpConfigurations            : [
                                {
                                  "Name": "ip200108020444",
                                  "Etag": "W/\"f9d1759d-4d49-42fa-8826-e218e0b1d355\"",
                                  "Id": "/subscriptions/a4257fde-b946-4e01-ade7-674760b8d1a3/resourceGroups/rg200108020444/providers/Microsoft.Net
                              work/networkInterfaces/nic200108020444/ipConfigurations/ip200108020444",
                                  "PrivateIpAddress": "5.5.166.65",
                                  "PrivateIpAllocationMethod": "Static",
                                  "Subnet": {
                                    "Id": "/subscriptions/a4257fde-b946-4e01-ade7-674760b8d1a3/resourceGroups/DbeSystemRG/providers/Microsoft.Netw
                              ork/virtualNetworks/vSwitch1/subnets/subnet123",
                                    "ResourceNavigationLinks": [],
                                    "ServiceEndpoints": []
                                  },
                                  "ProvisioningState": "Succeeded",
                                  "PrivateIpAddressVersion": "IPv4",
                                  "LoadBalancerBackendAddressPools": [],
                                  "LoadBalancerInboundNatRules": [],
                                  "Primary": true,
                                  "ApplicationGatewayBackendAddressPools": [],
                                  "ApplicationSecurityGroups": []
                                }
                              ]
DnsSettings                 : {
                                "DnsServers": [],
                                "AppliedDnsServers": []
                              }
EnableIPForwarding          : False
EnableAcceleratedNetworking : False
NetworkSecurityGroup        : null
Primary                     : True
MacAddress                  : 00155D18E432                :
```

Optional können Sie beim Erstellen einer virtuellen Netzwerkschnittstellenkarte für eine VM die öffentliche IP-Adresse übergeben. In dieser Instanz gibt die öffentliche IP-Adresse die private IP-Adresse zurück. 

```powershell
New-AzureRmPublicIPAddress -Name <Public IP> -ResourceGroupName <ResourceGroupName> -AllocationMethod Static -Location DBELocal
$publicIP = (Get-AzureRmPublicIPAddress -Name <Public IP> -ResourceGroupName <Resource group name>).Id
$ipConfig = New-AzureRmNetworkInterfaceIpConfig -Name <ConfigName> -PublicIpAddressId $publicIP -SubnetId $subNetId
```

### <a name="create-a-vm"></a>Erstellen einer VM

Sie können jetzt das VM-Image verwenden, um einen virtuellen Computer zu erstellen und dem zuvor erstellten virtuellen Netzwerk anzufügen.

```powershell
$pass = ConvertTo-SecureString "<Password>" -AsPlainText -Force;
$cred = New-Object System.Management.Automation.PSCredential("<Enter username>", $pass)
```

Nachdem Sie die VM erstellt und eingeschaltet haben, verwenden Sie den folgenden Benutzernamen und das zugehörige Kennwort, um sich bei ihr anzumelden.

```powershell
$VirtualMachine = New-AzureRmVMConfig -VMName <VM name> -VMSize "Standard_D1_v2"

$VirtualMachine = Set-AzureRmVMOperatingSystem -VM $VirtualMachine -<OS type> -ComputerName <Your computer Name> -Credential $cred

$VirtualMachine = Set-AzureRmVMOSDisk -VM $VirtualMachine -Name <OS Disk Name> -Caching "ReadWrite" -CreateOption "FromImage" -Linux -StorageAccountType StandardLRS

$nicID = (Get-AzureRmNetworkInterface -Name <nic name> -ResourceGroupName <Resource Group Name>).Id

$VirtualMachine = Add-AzureRmVMNetworkInterface -VM $VirtualMachine -Id $nicID

$image = (Get-AzureRmImage -ResourceGroupName <Resource Group Name> -ImageName $ImageName).Id

$VirtualMachine = Set-AzureRmVMSourceImage -VM $VirtualMachine -Id $image

New-AzureRmVM -ResourceGroupName <Resource Group Name> -Location DBELocal -VM $VirtualMachine -Verbose
```

## <a name="connect-to-the-vm"></a>Herstellen der Verbindung zur VM

Je nachdem, ob Sie einen virtuellen Windows-Computer (Windows-VM) oder einen virtuellen Linux-Computer (Linux-VM) erstellt haben, können die Verbindungsanweisungen unterschiedlich sein.

### <a name="connect-to-a-linux-vm"></a>Herstellen einer Verbindung mit einem virtuellen Linux-Computer

Wenn Sie eine Verbindung mit einem virtuellen Linux-Computer herstellen möchten, führen Sie die folgenden Schritte aus:

[!INCLUDE [azure-stack-edge-gateway-connect-vm](../../includes/azure-stack-edge-gateway-connect-virtual-machine-linux.md)]

### <a name="connect-to-a-windows-vm"></a>Herstellen einer Verbindung mit einem virtuellen Windows-Computer

Wenn Sie eine Verbindung mit einem virtuellen Windows-Computer herstellen möchten, führen Sie die folgenden Schritte aus:

[!INCLUDE [azure-stack-edge-gateway-connect-vm](../../includes/azure-stack-edge-gateway-connect-virtual-machine-windows.md)]


<!--Connect to the VM by using the private IP that you passed during the VM creation.

Open an SSH session to connect with the IP address.

`ssh -l <username> <ip address>`

When you're prompted, provide the password that you used when creating the VM.

If you need to provide the SSH key, use this command:

ssh -i c:/users/Administrator/.ssh/id_rsa Administrator@5.5.41.236

If you used a public IP address during VM creation, you can use that IP to connect to the VM. To get the public IP: 

```powershell
$publicIp = Get-AzureRmPublicIpAddress -Name <Public IP> -ResourceGroupName <Resource group name>
```
The public IP in this instance is the same as the private IP that you passed during the virtual network interface creation.-->


## <a name="manage-the-vm"></a>Verwalten der VM

In den folgenden Abschnitten werden einige der häufigen Vorgänge beschrieben, die Sie auf Ihrem Azure Stack Edge Pro-Gerät erstellen können.

### <a name="list-vms-that-are-running-on-the-device"></a>Auflisten der auf dem Gerät ausgeführten VMs

Wenn Sie eine Liste aller auf Ihrem Azure Stack Edge-Gerät ausgeführten VMs zurückgeben möchten, führen Sie diesen Befehl aus:


`Get-AzureRmVM -ResourceGroupName <String> -Name <String>`
 

### <a name="turn-on-the-vm"></a>Einschalten der VM

Wenn Sie einen auf Ihrem Gerät ausgeführten virtuellen Computer einschalten möchten, führen Sie das folgende Cmdlet aus:

`Start-AzureRmVM [-Name] <String> [-ResourceGroupName] <String>`

Weitere Informationen zu diesem Cmdlet finden Sie unter [Start-AzureRmVM](/powershell/module/azurerm.compute/start-azurermvm?view=azurermps-6.13.0&preserve-view=true).

### <a name="suspend-or-shut-down-the-vm"></a>Anhalten oder Herunterfahren des virtuellen Computers

Wenn Sie einen auf Ihrem Gerät ausgeführten virtuellen Computer anhalten oder herunterfahren möchten, führen Sie das folgende Cmdlet aus:


```powershell
Stop-AzureRmVM [-Name] <String> [-StayProvisioned] [-ResourceGroupName] <String>
```

Weitere Informationen zu diesem Cmdlet finden Sie unter [Stop-AzureRmVM cmdlet](/powershell/module/azurerm.compute/stop-azurermvm?view=azurermps-6.13.0&preserve-view=true).

### <a name="add-a-data-disk"></a>Hinzufügen eines Datenträgers

Wenn die Anforderungen an die Workload auf Ihrem virtuellen Computer größer werden, müssen Sie möglicherweise einen Datenträger hinzufügen. Führen Sie hierzu den folgenden Befehl aus:

```powershell
Add-AzureRmVMDataDisk -VM $VirtualMachine -Name "disk1" -VhdUri "https://contoso.blob.core.windows.net/vhds/diskstandard03.vhd" -LUN 0 -Caching ReadOnly -DiskSizeinGB 1 -CreateOption Empty 
 
Update-AzureRmVM -ResourceGroupName "<Resource Group Name string>" -VM $VirtualMachine
```

### <a name="delete-the-vm"></a>Löschen der virtuellen Computer

Wenn Sie einen virtuellen Computer von Ihrem Gerät entfernen möchten, führen Sie das folgende Cmdlet aus:

```powershell
Remove-AzureRmVM [-Name] <String> [-ResourceGroupName] <String>
```

Weitere Informationen zu diesem Cmdlet finden Sie unter [Remove-AzureRmVm cmdlet](/powershell/module/azurerm.compute/remove-azurermvm?view=azurermps-6.13.0&preserve-view=true).

## <a name="next-steps"></a>Nächste Schritte

[Azure Resource Manager-Cmdlets](/powershell/module/azurerm.resources/?view=azurermps-6.13.0&preserve-view=true)