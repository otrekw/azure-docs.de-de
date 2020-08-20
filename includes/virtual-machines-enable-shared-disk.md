---
title: include file
description: include file
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 08/17/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 37d9cf0b0ac7145e389f9fc768ff0b593b27ae1a
ms.sourcegitcommit: 54d8052c09e847a6565ec978f352769e8955aead
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/18/2020
ms.locfileid: "88515230"
---
## <a name="limitations"></a>Einschränkungen

[!INCLUDE [virtual-machines-disks-shared-limitations](virtual-machines-disks-shared-limitations.md)]

## <a name="supported-operating-systems"></a>Unterstützte Betriebssysteme

Freigegebene Datenträger unterstützen verschiedene Betriebssysteme. Informationen zu den unterstützten Betriebssystemen finden Sie im Konzeptartikel in den Abschnitten [Windows](../articles/virtual-machines/windows/disks-shared.md#windows) und [Linux](../articles/virtual-machines/linux/disks-shared.md#linux).

## <a name="disk-sizes"></a>Datenträgergrößen

[!INCLUDE [virtual-machines-disks-shared-sizes](virtual-machines-disks-shared-sizes.md)]

## <a name="deploy-shared-disks"></a>Bereitstellen von freigegebenen Datenträgern

### <a name="deploy-a-premium-ssd-as-a-shared-disk"></a>Bereitstellen einer SSD Premium als freigegebenen Datenträger

Um einen verwalteten Datenträger mit aktivierter Funktion freigegebener Datenträger bereitzustellen, verwenden Sie die neue Eigenschaft `maxShares`, und definieren Sie einen Wert, der größer als 1 ist. Dadurch kann der Datenträger für mehrere VMs freigegeben werden.

> [!IMPORTANT]
> Der Wert von `maxShares` kann nur festgelegt oder geändert werden, wenn die Einbindung eines Datenträgers auf allen VMs aufgehoben wird. Informationen zu den zulässigen Werten für `maxShares` finden Sie unter [Datenträgergrößen](#disk-sizes).

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

```azurecli
az disk create -g myResourceGroup -n mySharedDisk --size-gb 1024 -l westcentralus --sku Premium_LRS --max-shares 2
```

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell-interactive
$dataDiskConfig = New-AzDiskConfig -Location 'WestCentralUS' -DiskSizeGB 1024 -AccountType Premium_LRS -CreateOption Empty -MaxSharesCount 2

New-AzDisk -ResourceGroupName 'myResourceGroup' -DiskName 'mySharedDisk' -Disk $dataDiskConfig
```

# <a name="resource-manager-template"></a>[Resource Manager-Vorlage](#tab/azure-resource-manager)

Bevor Sie die folgende Vorlage verwenden, ersetzen Sie `[parameters('dataDiskName')]`, `[resourceGroup().location]`, `[parameters('dataDiskSizeGB')]` und `[parameters('maxShares')]` durch Ihre eigenen Werte.

[Vorlage für freigegebene SSD Premium-Datenträger](https://aka.ms/SharedPremiumDiskARMtemplate)

---

### <a name="deploy-an-ultra-disk-as-a-shared-disk"></a>Bereitstellen einer Disk Ultra als freigegebenen Datenträger

Um einen verwalteten Datenträger mit aktiviertem Feature für freigegebene Datenträger bereitzustellen, ändern Sie den Parameter `maxShares` in einen Wert, der größer als 1 ist. Dadurch kann der Datenträger für mehrere VMs freigegeben werden.

> [!IMPORTANT]
> Der Wert von `maxShares` kann nur festgelegt oder geändert werden, wenn die Einbindung eines Datenträgers auf allen VMs aufgehoben wird. Informationen zu den zulässigen Werten für `maxShares` finden Sie unter [Datenträgergrößen](#disk-sizes).


# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

##### <a name="regional-disk-example"></a>Beispiel für einen regionalen Datenträger

```azurecli
#Creating an Ultra shared Disk 
az disk create -g rg1 -n clidisk --size-gb 1024 -l westus --sku UltraSSD_LRS --max-shares 5 --disk-iops-read-write 2000 --disk-mbps-read-write 200 --disk-iops-read-only 100 --disk-mbps-read-only 1

#Updating an Ultra shared Disk 
az disk update -g rg1 -n clidisk --disk-iops-read-write 3000 --disk-mbps-read-write 300 --set diskIopsReadOnly=100 --set diskMbpsReadOnly=1

#Show shared disk properties:
az disk show -g rg1 -n clidisk
```

##### <a name="zonal-disk-example"></a>Beispiel für einen zonalen Datenträger

Dieses Beispiel ist fast identisch mit dem vorherigen, es wird jedoch ein Datenträger in der Verfügbarkeitszone 1 erstellt.

```azurecli
#Creating an Ultra shared Disk 
az disk create -g rg1 -n clidisk --size-gb 1024 -l westus --sku UltraSSD_LRS --max-shares 5 --disk-iops-read-write 2000 --disk-mbps-read-write 200 --disk-iops-read-only 100 --disk-mbps-read-only 1 --zone 1

#Updating an Ultra shared Disk 
az disk update -g rg1 -n clidisk --disk-iops-read-write 3000 --disk-mbps-read-write 300 --set diskIopsReadOnly=100 --set diskMbpsReadOnly=1

#Show shared disk properties:
az disk show -g rg1 -n clidisk
```

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

##### <a name="regional-disk-example"></a>Beispiel für einen regionalen Datenträger

```azurepowershell-interactive
$datadiskconfig = New-AzDiskConfig -Location 'WestCentralUS' -DiskSizeGB 1024 -AccountType UltraSSD_LRS -CreateOption Empty -DiskIOPSReadWrite 2000 -DiskMBpsReadWrite 200 -DiskIOPSReadOnly 100 -DiskMBpsReadOnly 1 -MaxSharesCount 5

New-AzDisk -ResourceGroupName 'myResourceGroup' -DiskName 'mySharedDisk' -Disk $datadiskconfig
```

##### <a name="zonal-disk-example"></a>Beispiel für einen zonalen Datenträger

Dieses Beispiel ist fast identisch mit dem vorherigen, es wird jedoch ein Datenträger in der Verfügbarkeitszone 1 erstellt.

```azurepowershell-interactive
$datadiskconfig = New-AzDiskConfig -Location 'WestCentralUS' -DiskSizeGB 1024 -AccountType UltraSSD_LRS -CreateOption Empty -DiskIOPSReadWrite 2000 -DiskMBpsReadWrite 200 -DiskIOPSReadOnly 100 -DiskMBpsReadOnly 1 -MaxSharesCount 5 -Zone 1

New-AzDisk -ResourceGroupName 'myResourceGroup' -DiskName 'mySharedDisk' -Disk $datadiskconfig
```

# <a name="resource-manager-template"></a>[Resource Manager-Vorlage](#tab/azure-resource-manager)

##### <a name="regional-disk-example"></a>Beispiel für einen regionalen Datenträger

Bevor Sie die folgende Vorlage verwenden, ersetzen Sie `[parameters('dataDiskName')]`, `[resourceGroup().location]`, `[parameters('dataDiskSizeGB')]`, `[parameters('maxShares')]`, `[parameters('diskIOPSReadWrite')]`, `[parameters('diskMBpsReadWrite')]`, `[parameters('diskIOPSReadOnly')]` und `[parameters('diskMBpsReadOnly')]` durch Ihre eigenen Werte.

[Vorlage für regionale freigegebene Disk Ultra-Datenträger](https://aka.ms/SharedUltraDiskARMtemplateRegional)

##### <a name="zonal-disk-example"></a>Beispiel für einen zonalen Datenträger

Bevor Sie die folgende Vorlage verwenden, ersetzen Sie `[parameters('dataDiskName')]`, `[resourceGroup().location]`, `[parameters('dataDiskSizeGB')]`, `[parameters('maxShares')]`, `[parameters('diskIOPSReadWrite')]`, `[parameters('diskMBpsReadWrite')]`, `[parameters('diskIOPSReadOnly')]` und `[parameters('diskMBpsReadOnly')]` durch Ihre eigenen Werte.

[Vorlage für zonale freigegebene Disk Ultra-Datenträger](https://aka.ms/SharedUltraDiskARMtemplateZonal)

---

## <a name="using-azure-shared-disks-with-your-vms"></a>Verwenden von freigegebenen Azure-Datenträgern mit ihren VMs

Nachdem Sie mit `maxShares>1` einen freigegebenen Datenträger bereitgestellt haben, können Sie den Datenträger auf einem oder mehreren virtuellen Computern einbinden.

> [!NOTE]
> Wenn Sie einen Ultra-Datenträger bereitstellen, stellen Sie sicher, dass er die erforderlichen Anforderungen erfüllt. Ausführliche Informationen finden Sie im Abschnitt [PowerShell](../articles/virtual-machines/windows/disks-enable-ultra-ssd.md#enable-ultra-disk-compatibility-on-an-existing-vm-1) oder [CLI](../articles/virtual-machines/linux/disks-enable-ultra-ssd.md#enable-ultra-disk-compatibility-on-an-existing-vm) des Artikels zu Ultra-Datenträgern.

```azurepowershell-interactive

$resourceGroup = "myResourceGroup"
$location = "WestCentralUS"

$vm = New-AzVm -ResourceGroupName $resourceGroup -Name "myVM" -Location $location -VirtualNetworkName "myVnet" -SubnetName "mySubnet" -SecurityGroupName "myNetworkSecurityGroup" -PublicIpAddressName "myPublicIpAddress"

$dataDisk = Get-AzDisk -ResourceGroupName $resourceGroup -DiskName "mySharedDisk"

$vm = Add-AzVMDataDisk -VM $vm -Name "mySharedDisk" -CreateOption Attach -ManagedDiskId $dataDisk.Id -Lun 0

update-AzVm -VM $vm -ResourceGroupName $resourceGroup
```

## <a name="supported-scsi-pr-commands"></a>Unterstützte SCSI-PR-Befehle

Nachdem Sie den freigegebenen Datenträger auf Ihren VMs in Ihrem Cluster eingebunden haben, können Sie mithilfe von SCSI-PR ein Quorum und Lese-/Schreibzugriff auf den Datenträger einrichten. Die folgenden PR-Befehle sind für freigegebene Azure-Datenträger verfügbar:

Wenn Sie mit dem Datenträger interagieren möchten, beginnen Sie mit der Liste „persistent-reservation-action“:

```
PR_REGISTER_KEY 

PR_REGISTER_AND_IGNORE 

PR_GET_CONFIGURATION 

PR_RESERVE 

PR_PREEMPT_RESERVATION 

PR_CLEAR_RESERVATION 

PR_RELEASE_RESERVATION 
```

Wenn Sie PR_RESERVE, PR_PREEMPT_RESERVATION oder PR_RELEASE_RESERVATION verwenden, geben Sie einen der folgenden permanenten Reservierungstypen (persistent-reservation-type) an:

```
PR_NONE 

PR_WRITE_EXCLUSIVE 

PR_EXCLUSIVE_ACCESS 

PR_WRITE_EXCLUSIVE_REGISTRANTS_ONLY 

PR_EXCLUSIVE_ACCESS_REGISTRANTS_ONLY 

PR_WRITE_EXCLUSIVE_ALL_REGISTRANTS 

PR_EXCLUSIVE_ACCESS_ALL_REGISTRANTS 
```

Ferner müssen Sie beim Verwenden von PR_RESERVE, PR_REGISTER_AND_IGNORE, PR_REGISTER_KEY, PR_PREEMPT_RESERVATION, PR_CLEAR_RESERVATION, oder PR_RELEASE-RESERVATION einen persistent-reservation-key angeben.


## <a name="next-steps"></a>Nächste Schritte

Wenn Sie die Bereitstellung des Datenträgers mithilfe von Azure Resource Manager-Vorlagen bevorzugen, sehen Sie sich die folgenden Beispielvorlagen an:
- [SSD Premium](https://aka.ms/SharedPremiumDiskARMtemplate)
- [Regionale Ultra-Datenträger](https://aka.ms/SharedUltraDiskARMtemplateRegional)
- [Zonale Ultra-Datenträger](https://aka.ms/SharedUltraDiskARMtemplateZonal)