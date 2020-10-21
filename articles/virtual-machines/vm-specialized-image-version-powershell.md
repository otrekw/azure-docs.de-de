---
title: Erstellen einer VM aus einem spezialisierten Image
description: Erstellen Sie eine VM mithilfe eines spezialisierten Images in einem Katalog mit freigegebenen Images.
author: cynthn
ms.service: virtual-machines
ms.subservice: imaging
ms.workload: infrastructure-services
ms.topic: how-to
ms.date: 05/04/2020
ms.author: cynthn
ms.reviewer: akjosh
ms.openlocfilehash: 289bca140392ec77fa453e594aface6be9befeca
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91446614"
---
# <a name="create-a-vm-using-a-specialized-image"></a>Erstellen einer VM mithilfe eines spezialisierten Images 

Erstellen Sie eine VM aus einer spezialisierten Imageversion, die in einem Katalog mit freigegebenen Images gespeichert ist. Wenn Sie eine VM mit einer generalisierten Imageversion erstellen möchten, finden Sie weitere Informationen unter [Erstellen einer VM mit einem generalisierten Image](vm-generalized-image-version-powershell.md).

Nachdem Sie über eine spezialisierte Imageversion verfügen, können Sie mithilfe des Cmdlets [New-AzVM](/powershell/module/az.compute/new-azvm) neue VMs erstellen. 

In diesem Beispiel verwenden wir die Imagedefinitions-ID, um sicherzustellen, dass Ihre neue VM die neueste Version eines Images verwendet. Sie können auch eine bestimmte Version verwenden, indem Sie die Imageversions-ID für `Set-AzVMSourceImage -Id` verwenden. Um beispielsweise die Imageversion *1.0.0* zu verwenden, geben Sie ein: `Set-AzVMSourceImage -Id "/subscriptions/<subscription ID where the gallery is located>/resourceGroups/myGalleryRG/providers/Microsoft.Compute/galleries/myGallery/images/myImageDefinition/versions/1.0.0"`. 

Beachten Sie, dass die Verwendung einer bestimmten Imageversion bedeutet, dass die Automatisierung fehlschlagen kann, wenn diese bestimmte Imageversion nicht verfügbar ist, weil sie gelöscht oder aus der Region entfernt wurde. Es wird empfohlen, die Imagedefinitions-ID zum Erstellen der neuen VM zu verwenden, es sei denn, eine bestimmte Imageversion ist erforderlich.

Ersetzen Sie bei Bedarf die Ressourcennamen in diesem Beispiel. 


```azurepowershell-interactive

# Create some variables for the new VM.

$resourceGroup = "mySIGSpecializedRG"
$location = "South Central US"
$vmName = "mySpecializedVM"

# Get the image. Replace the name of your resource group, gallery, and image definition. This will create the VM from the latest image version available.

$imageDefinition = Get-AzGalleryImageDefinition `
   -GalleryName myGallery `
   -ResourceGroupName myResourceGroup `
   -Name myImageDefinition


# Create a resource group
New-AzResourceGroup -Name $resourceGroup -Location $location

# Create the network resources.

$subnetConfig = New-AzVirtualNetworkSubnetConfig `
   -Name mySubnet `
   -AddressPrefix 192.168.1.0/24
$vnet = New-AzVirtualNetwork `
   -ResourceGroupName $resourceGroup `
   -Location $location `
   -Name MYvNET `
   -AddressPrefix 192.168.0.0/16 `
   -Subnet $subnetConfig
$pip = New-AzPublicIpAddress `
   -ResourceGroupName $resourceGroup `
   -Location $location `
  -Name "mypublicdns$(Get-Random)" `
  -AllocationMethod Static `
  -IdleTimeoutInMinutes 4
$nsgRuleRDP = New-AzNetworkSecurityRuleConfig `
   -Name myNetworkSecurityGroupRuleRDP  `
   -Protocol Tcp `
   -Direction Inbound `
   -Priority 1000 `
   -SourceAddressPrefix * `
   -SourcePortRange * `
   -DestinationAddressPrefix * `
   -DestinationPortRange 3389 -Access Allow
$nsg = New-AzNetworkSecurityGroup `
   -ResourceGroupName $resourceGroup `
   -Location $location `
   -Name myNetworkSecurityGroup `
   -SecurityRules $nsgRuleRDP
$nic = New-AzNetworkInterface `
   -Name $vmName `
   -ResourceGroupName $resourceGroup `
   -Location $location `
  -SubnetId $vnet.Subnets[0].Id `
  -PublicIpAddressId $pip.Id `
  -NetworkSecurityGroupId $nsg.Id

# Create a virtual machine configuration using Set-AzVMSourceImage -Id $imageDefinition.Id to use the latest available image version.

$vmConfig = New-AzVMConfig `
   -VMName $vmName `
   -VMSize Standard_D1_v2 | `
   Set-AzVMSourceImage -Id $imageDefinition.Id | `
   Add-AzVMNetworkInterface -Id $nic.Id

# Create a virtual machine
New-AzVM `
   -ResourceGroupName $resourceGroup `
   -Location $location `
   -VM $vmConfig

```

## <a name="attach-the-data-disk"></a>Anfügen des Datenträgers
Wenn Ihr Image einen Datenträger für Daten enthält, müssen Sie diesen an die VM anfügen.

```azurepowershell-interactive

$vm = Get-AzVM -Name $vmName -ResourceGroupName $resourceGroup 

$lun = $imageVersion.StorageProfile.DataDiskImages.Lun

Add-AzVMDataDisk `
   -CreateOption FromImage `
   -SourceImageUri $imageversion.Id `
   -Lun $lun `
   -Caching $imageVersion.StorageProfile.DataDiskImages.HostCaching `
   -DiskSizeInGB $imageVersion.StorageProfile.DataDiskImages.SizeInGB `
   -VM $vm

```


## <a name="next-steps"></a>Nächste Schritte
[Azure Image Builder (Vorschauversion)](./linux/image-builder-overview.md) hilft beim Automatisieren der Erstellung von Imageversionen. Sie können den Dienst sogar zum Aktualisieren und [Erstellen einer neuen Imageversion aus einer vorhandenen](./linux/image-builder-gallery-update-image-version.md) verwenden. 

Sie können auch mithilfe von Vorlagen eine Ressource im Katalog für freigegebene Images erstellen. Es stehen mehrere Azure-Schnellstartvorlagen zur Verfügung: 

- [Erstellen eines Katalogs mit freigegebenen Images](https://azure.microsoft.com/resources/templates/101-sig-create/)
- [Erstellen einer Imagedefinition in einem Katalog mit freigegebenen Images](https://azure.microsoft.com/resources/templates/101-sig-image-definition-create/)
- [Erstellen einer Imageversion in einem Katalog mit freigegebenen Images](https://azure.microsoft.com/resources/templates/101-sig-image-version-create/)
- [Erstellen eines virtuellen Computers aus einer Imageversion](https://azure.microsoft.com/resources/templates/101-vm-from-sig/)

Weitere Informationen zu Katalogen mit geteilten Images finden Sie in der [Übersicht](./windows/shared-image-galleries.md). Sollten Probleme auftreten, sehen Sie unter [Problembehandlung für Kataloge mit freigegebenen Images](troubleshooting-shared-images.md) nach.
