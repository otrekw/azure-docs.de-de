---
title: Ändern der Größe einer Windows-VM in Azure
description: Ändern der VM-Größe für einen virtuellen Azure-Computer
author: cynthn
ms.service: virtual-machines-windows
ms.subservice: sizes
ms.workload: infrastructure
ms.topic: how-to
ms.date: 01/13/2020
ms.author: cynthn
ms.openlocfilehash: 4860422fefb5a95fe41912b4898f02867f7b3aeb
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "87832245"
---
# <a name="resize-a-windows-vm"></a>Ändern der Größe eines virtuellen Windows-Computers

In diesem Artikel erfahren Sie, wie Sie einen virtuellen Computer auf eine andere [VM-Größe](../sizes.md) verschieben.

Nachdem Sie einen virtuellen Computer (virtual machine; VM) erstellen, können Sie den virtuellen Computer zentral hoch- oder herunterskalieren, indem Sie die VM-Größe ändern. In einigen Fällen müssen Sie zuerst die Zuordnung des virtuellen Computers aufheben. Dies ist möglicherweise der Fall, falls die neue Größe auf dem Hardwarecluster nicht verfügbar ist, auf dem die VM aktuell gehostet wird.

Wenn Ihr virtueller Computer Storage Premium verwendet, stellen Sie sicher, dass Sie für die Größe eine **s**-Version auswählen, damit Storage Premium unterstützt wird. Wählen Sie z. B. Standard_E4**s**_v3 statt Standard_E4_v3 aus.

## <a name="use-the-portal"></a>Verwenden des Portals

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com).
1. Öffnen Sie die Seite für den virtuellen Computer.
1. Wählen Sie im linken Menü die Option **Größe** aus.
1. Wählen Sie in der Liste der verfügbaren Größen eine neue Größe und dann **Größe ändern** aus.


Wenn der virtuelle Computer zurzeit ausgeführt wird, führt eine Änderung seiner Größe zu einem Neustart. Wenn Sie den virtuellen Computer beenden, werden möglicherweise weitere Größen angezeigt.

## <a name="use-powershell-to-resize-a-vm-not-in-an-availability-set"></a>Verwenden von PowerShell zum Ändern der Größe einer VM, die sich nicht in einer Verfügbarkeitsgruppe befindet

Legen Sie einige Variablen fest. Ersetzen Sie die Werte durch Ihre eigenen Informationen.

```powershell
$resourceGroup = "myResourceGroup"
$vmName = "myVM"
```

Listet die VM-Größen auf, die auf dem Hardwarecluster verfügbar sind, auf dem die VM gehostet wird. 
   
```powershell
Get-AzVMSize -ResourceGroupName $resourceGroup -VMName $vmName 
```

Führen Sie die folgenden Befehle aus, um die Größe des virtuellen Computers zu ändern, wenn die gewünschte Größe aufgelistet ist. Wenn die gewünschte Größe nicht aufgeführt ist, fahren Sie mit Schritt 3 fort.
   
```powershell
$vm = Get-AzVM -ResourceGroupName $resourceGroup -VMName $vmName
$vm.HardwareProfile.VmSize = "<newVMsize>"
Update-AzVM -VM $vm -ResourceGroupName $resourceGroup
```

Führen Sie die folgenden Befehle aus, wenn die gewünschte Größe nicht aufgelistet ist. Damit heben Sie die Zuordnung des virtuellen Computers auf, ändern seine Größe, und starten ihn neu. Ersetzen Sie **\<newVMsize>** durch die gewünschte Größe.
   
```powershell
Stop-AzVM -ResourceGroupName $resourceGroup -Name $vmName -Force
$vm = Get-AzVM -ResourceGroupName $resourceGroup -VMName $vmName
$vm.HardwareProfile.VmSize = "<newVMSize>"
Update-AzVM -VM $vm -ResourceGroupName $resourceGroup
Start-AzVM -ResourceGroupName $resourceGroup -Name $vmName
```

> [!WARNING]
> Die Aufhebung der Zuordnung der VM gibt alle dynamischen IP-Adressen frei, die der VM zugewiesen sind. Das Betriebssystem und die Datenträger sind nicht betroffen. 
> 
> 

## <a name="use-powershell-to-resize-a-vm-in-an-availability-set"></a>Verwenden von PowerShell zum Ändern der Größe einer VM in einer Verfügbarkeitsgruppe

Wenn die neue Größe für einen virtuellen Computer in einer Verfügbarkeitsgruppe nicht im Hardwarecluster verfügbar ist, auf dem der virtuelle Computer derzeit gehostet wird, muss bei allen virtuellen Computern in der Verfügbarkeitsgruppe die Zuordnung aufgehoben werden, um die Größe des virtuellen Computers ändern zu können. Sie müssen möglicherweise auch die Größe der anderen virtuellen Computer in der Verfügbarkeitsgruppe aktualisieren, nachdem die Größe eines virtuellen Computers geändert wurde. Führen Sie die folgenden Schritte zum Ändern der Größe eines virtuellen Computers in einer Verfügbarkeitsgruppe aus.

```powershell
$resourceGroup = "myResourceGroup"
$vmName = "myVM"
```

Listet die VM-Größen auf, die auf dem Hardwarecluster verfügbar sind, auf dem die VM gehostet wird. 
   
```powershell
Get-AzVMSize -ResourceGroupName $resourceGroup -VMName $vmName 
```

Führen Sie die folgenden Befehle aus, um die Größe der VM zu ändern, sofern die gewünschte Größe aufgelistet ist. Wenn sie nicht aufgelistet ist, fahren Sie mit dem nächsten Abschnitt fort.
   
```powershell
$vm = Get-AzVM -ResourceGroupName $resourceGroup -VMName $vmName 
$vm.HardwareProfile.VmSize = "<newVmSize>"
Update-AzVM -VM $vm -ResourceGroupName $resourceGroup
```
    
Wenn die gewünschte Größe nicht aufgelistet ist, fahren Sie mit den folgenden Schritten fort. Damit heben Sie die Zuordnung aller virtuellen Computer in der Verfügbarkeitsgruppe auf, skalieren die VMs, und starten sie neu.

Beenden Sie alle virtuellen Computer in der Verfügbarkeitsgruppe.
   
```powershell
$availabilitySetName = "<availabilitySetName>"
$as = Get-AzAvailabilitySet -ResourceGroupName $resourceGroup -Name $availabilitySetName
$virtualMachines = $as.VirtualMachinesReferences |  Get-AzResource | Get-AzVM
$virtualMachines |  Stop-AzVM -Force -NoWait  
```

Skalieren Sie alle virtuellen Computer in der Verfügbarkeitsgruppe und starten sie neu.
   
```powershell
$availabilitySetName = "<availabilitySetName>"
$newSize = "<newVmSize>"
$as = Get-AzAvailabilitySet -ResourceGroupName $resourceGroup -Name $availabilitySetName
$virtualMachines = $as.VirtualMachinesReferences |  Get-AzResource | Get-AzVM
$virtualMachines | Foreach-Object { $_.HardwareProfile.VmSize = $newSize }
$virtualMachines | Update-AzVM
$virtualMachines | Start-AzVM
```

## <a name="next-steps"></a>Nächste Schritte

Führen Sie mehrere VM-Instanzen aus, und skalieren Sie diese auf, um zusätzliche Skalierbarkeit zu erhalten. Weitere Informationen finden Sie unter [Automatisches Skalieren von Windows-Computern in einer VM-Skalierungsgruppe](../../virtual-machine-scale-sets/tutorial-autoscale-powershell.md).