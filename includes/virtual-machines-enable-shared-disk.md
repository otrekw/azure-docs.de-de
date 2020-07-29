---
title: include file
description: include file
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 07/14/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 29a90b94db5e6e5791361bad004efcf649e1950b
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/20/2020
ms.locfileid: "86500595"
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

#### <a name="cli"></a>Befehlszeilenschnittstelle (CLI)
```azurecli

az disk create -g myResourceGroup -n mySharedDisk --size-gb 1024 -l westcentralus --sku PremiumSSD_LRS --max-shares 2

```

#### <a name="powershell"></a>PowerShell
```azurepowershell-interactive

$datadiskconfig = New-AzDiskConfig -Location 'WestCentralUS' -DiskSizeGB 1024 -AccountType PremiumSSD_LRS -CreateOption Empty

New-AzDisk -ResourceGroupName 'myResourceGroup' -DiskName 'mySharedDisk' -Disk $datadiskconfig

```

#### <a name="azure-resource-manager"></a>Azure Resource Manager
Bevor Sie die folgende Vorlage verwenden, ersetzen Sie `[parameters('dataDiskName')]`, `[resourceGroup().location]`, `[parameters('dataDiskSizeGB')]` und `[parameters('maxShares')]` durch Ihre eigenen Werte.

```json
{ 
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "dataDiskName": {
      "type": "string",
      "defaultValue": "mySharedDisk"
    },
    "dataDiskSizeGB": {
      "type": "int",
      "defaultValue": 1024
    },
    "maxShares": {
      "type": "int",
      "defaultValue": 2
    }
  },
  "resources": [
    {
      "type": "Microsoft.Compute/disks",
      "name": "[parameters('dataDiskName')]",
      "location": "[resourceGroup().location]",
      "apiVersion": "2019-07-01",
      "sku": {
        "name": "Premium_LRS"
      },
      "properties": {
        "creationData": {
          "createOption": "Empty"
        },
        "diskSizeGB": "[parameters('dataDiskSizeGB')]",
        "maxShares": "[parameters('maxShares')]"
      }
    }
  ] 
}
```

### <a name="deploy-an-ultra-disk-as-a-shared-disk"></a>Bereitstellen einer Disk Ultra als freigegebenen Datenträger

Um einen verwalteten Datenträger mit aktiviertem Feature für freigegebene Datenträger bereitzustellen, ändern Sie den Parameter `maxShares` in einen Wert, der größer als 1 ist. Dadurch kann der Datenträger für mehrere VMs freigegeben werden.

> [!IMPORTANT]
> Der Wert von `maxShares` kann nur festgelegt oder geändert werden, wenn die Einbindung eines Datenträgers auf allen VMs aufgehoben wird. Informationen zu den zulässigen Werten für `maxShares` finden Sie unter [Datenträgergrößen](#disk-sizes).

#### <a name="cli"></a>Befehlszeilenschnittstelle (CLI)
```azurecli
#Creating an Ultra shared Disk 
az disk create -g rg1 -n clidisk --size-gb 1024 -l westus --sku UltraSSD_LRS --max-shares 5 --disk-iops-read-write 2000 --disk-mbps-read-write 200 --disk-iops-read-only 100 --disk-mbps-read-only 1

#Updating an Ultra shared Disk 
az disk update -g rg1 -n clidisk --disk-iops-read-write 3000 --disk-mbps-read-write 300 --set diskIopsReadOnly=100 --set diskMbpsReadOnly=1

#Show shared disk properties:
az disk show -g rg1 -n clidisk
```

#### <a name="powershell"></a>PowerShell
```azurepowershell-interactive

$datadiskconfig = New-AzDiskConfig -Location 'WestCentralUS' -DiskSizeGB 1024 -AccountType UltraSSD_LRS -CreateOption Empty -DiskIOPSReadWrite 2000 -DiskMBpsReadWrite 200 -DiskIOPSReadOnly 100 -DiskMBpsReadOnly 1

New-AzDisk -ResourceGroupName 'myResourceGroup' -DiskName 'mySharedDisk' -Disk $datadiskconfig

```

#### <a name="azure-resource-manager"></a>Azure Resource Manager

Um einen verwalteten Datenträger mit aktiviertem Feature für freigegebene Datenträger bereitzustellen, verwenden Sie die Eigenschaft `maxShares`, und definieren Sie einen Wert, der größer als 1 ist. Dadurch kann der Datenträger für mehrere VMs freigegeben werden.

> [!IMPORTANT]
> Der Wert von `maxShares` kann nur festgelegt oder geändert werden, wenn die Einbindung eines Datenträgers auf allen VMs aufgehoben wird. Informationen zu den zulässigen Werten für `maxShares` finden Sie unter [Datenträgergrößen](#disk-sizes).

Bevor Sie die folgende Vorlage verwenden, ersetzen Sie `[parameters('dataDiskName')]`, `[resourceGroup().location]`, `[parameters('dataDiskSizeGB')]`, `[parameters('maxShares')]`, `[parameters('diskIOPSReadWrite')]`, `[parameters('diskMBpsReadWrite')]`, `[parameters('diskIOPSReadOnly')]` und `[parameters('diskMBpsReadOnly')]` durch Ihre eigenen Werte.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "diskName": {
      "type": "string",
      "defaultValue": "uShared30"
    },
    "location": {
        "type": "string",
        "defaultValue": "westus",
        "metadata": {
                "description": "Location for all resources."
        }
    },
    "dataDiskSizeGB": {
      "type": "int",
      "defaultValue": 1024
    },
    "maxShares": {
      "type": "int",
      "defaultValue": 2
    },
    "diskIOPSReadWrite": {
      "type": "int",
      "defaultValue": 2048
    },
    "diskMBpsReadWrite": {
      "type": "int",
      "defaultValue": 20
    },    
    "diskIOPSReadOnly": {
      "type": "int",
      "defaultValue": 100
    },
    "diskMBpsReadOnly": {
      "type": "int",
      "defaultValue": 1
    }    
  }, 
  "resources": [
    {
        "type": "Microsoft.Compute/disks",
        "name": "[parameters('diskName')]",
        "location": "[parameters('location')]",
        "apiVersion": "2019-07-01",
        "sku": {
            "name": "UltraSSD_LRS"
        },
        "properties": {
            "creationData": {
                "createOption": "Empty"
            },
            "diskSizeGB": "[parameters('dataDiskSizeGB')]",
            "maxShares": "[parameters('maxShares')]",
            "diskIOPSReadWrite": "[parameters('diskIOPSReadWrite')]",
            "diskMBpsReadWrite": "[parameters('diskMBpsReadWrite')]",
            "diskIOPSReadOnly": "[parameters('diskIOPSReadOnly')]",
            "diskMBpsReadOnly": "[parameters('diskMBpsReadOnly')]"
        }
    }
  ]
}
```

### <a name="using-azure-shared-disks-with-your-vms"></a>Verwenden von freigegebenen Azure-Datenträgern mit ihren VMs

Nachdem Sie mit `maxShares>1` einen freigegebenen Datenträger bereitgestellt haben, können Sie den Datenträger auf einem oder mehreren virtuellen Computern einbinden.

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
