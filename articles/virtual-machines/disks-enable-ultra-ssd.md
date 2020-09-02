---
title: Ultra-Datenträger für VMs – verwaltete Azure-Datenträger
description: Informationen zu Ultra-Datenträgern für virtuelle Azure-Computer
author: roygara
ms.service: virtual-machines
ms.topic: how-to
ms.date: 05/11/2020
ms.author: rogarana
ms.subservice: disks
ms.custom: references_regions
ms.openlocfilehash: 4c005bc49780edcb7f322455e37163e78d87619f
ms.sourcegitcommit: b33c9ad17598d7e4d66fe11d511daa78b4b8b330
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/25/2020
ms.locfileid: "88852688"
---
# <a name="using-azure-ultra-disks"></a>Verwenden von Azure Ultra-Datenträgern

In diesem Artikel wird erläutert, wie eine Disk Ultra bereitgestellt und verwendet wird. Konzeptionelle Informationen zu Disk Ultra finden Sie unter [Welche Datenträgertypen sind in Azure verfügbar?](disks-types.md#ultra-disk).

Azure Ultra-Datenträger bieten hohen Durchsatz, einen hohen IOPS-Wert und einen Datenträgerspeicher mit durchgängig geringer Latenz für virtuelle Azure IaaS-Computer (VMs). Dieses neue Angebot bietet Spitzenleistung auf den gleichen Verfügbarkeitsebenen wie unsere vorhandenen Datenträgerangebote. Ein Hauptvorteil von Ultra-Datenträgern ist die Möglichkeit zum dynamischen Ändern der SSD-Leistung zusammen mit Ihren Workloads, ohne dass Sie Ihre VMs neu starten müssen. Ultra-Datenträger eignen sich für datenintensive Workloads wie SAP HANA, führende Datenbanksysteme und Workloads mit vielen Transaktionen.

## <a name="ga-scope-and-limitations"></a>Umfang und Einschränkungen für allgemeine Verfügbarkeit

[!INCLUDE [managed-disks-ultra-disks-GA-scope-and-limitations](../../includes/managed-disks-ultra-disks-GA-scope-and-limitations.md)]

## <a name="determine-vm-size-and-region-availability"></a>Ermitteln der VM-Größe und Regionsverfügbarkeit

### <a name="vms-using-availability-zones"></a>Virtuelle Computer mit Verfügbarkeitszonen

Sie müssen ermitteln, in welcher Verfügbarkeitszone Sie sich befinden, um Ultra-Datenträger verwenden zu können. Nicht jede Region unterstützt jede VM-Größe bei Ultra-Datenträgern. Führen Sie einen der folgenden Befehle aus, um zu ermitteln, ob Ihre Region, Zone und VM-Größe Ultra-Datenträger unterstützt. Ersetzen Sie dazu die Werte für **region**, **vmSize** und **subscription** entsprechend:

#### <a name="cli"></a>Befehlszeilenschnittstelle (CLI)

```azurecli
subscription="<yourSubID>"
# example value is southeastasia
region="<yourLocation>"
# example value is Standard_E64s_v3
vmSize="<yourVMSize>"

az vm list-skus --resource-type virtualMachines  --location $region --query "[?name=='$vmSize'].locationInfo[0].zoneDetails[0].Name" --subscription $subscription
```

#### <a name="powershell"></a>PowerShell

```powershell
$region = "southeastasia"
$vmSize = "Standard_E64s_v3"
(Get-AzComputeResourceSku | where {$_.Locations.Contains($region) -and ($_.Name -eq $vmSize) -and $_.LocationInfo[0].ZoneDetails.Count -gt 0})[0].LocationInfo[0].ZoneDetails
```

Die Antwort liegt in der nachstehend gezeigten Form vor, wobei X für die Zone steht, die für die Bereitstellung in Ihrer ausgewählten Region zu verwenden ist. X kann entweder 1, 2 oder 3 sein.

Behalten Sie den Wert für **Zones** bei. Er steht für Ihre Verfügbarkeitszone, und Sie benötigen ihn zum Bereitstellen eines Ultra-Datenträgers.

|ResourceType  |Name  |Position  |Zones  |Einschränkung  |Funktion  |Wert  |
|---------|---------|---------|---------|---------|---------|---------|
|disks     |UltraSSD_LRS         |eastus2         |X         |         |         |         |

> [!NOTE]
> Kommt vom Befehl keine Antwort, wird die ausgewählte VM-Größe für Ultra-Datenträger in der ausgewählten Region nicht unterstützt.

Nachdem Sie nun wissen, in welcher Zone die Bereitstellung erfolgen muss, führen Sie die Bereitstellungsschritte in diesem Artikel aus, um entweder einen virtuellen Computer mit einem zugeordneten Ultra-Datenträger bereitzustellen oder einem vorhandenen virtuellen Computer einen Ultra-Datenträger zuzuordnen.

### <a name="vms-with-no-redundancy-options"></a>Virtuelle Computer ohne Redundanzoptionen

Die in der Region „USA, Westen“ bereitgestellten Disk Ultra-Datenträger müssen vorerst ohne Redundanzoptionen bereitgestellt werden. Jedoch befindet sich möglicherweise nicht jede Datenträgergröße, die Disk Ultra-Datenträger unterstützt, in dieser Region. Sie können einen der folgenden Codeausschnitte verwenden, um zu bestimmen, welche in der Region „USA, Westen“ Disk Ultra-Datenträger unterstützen. Stellen Sie sicher, dass Sie zuerst die Werte `vmSize` und `subscription` ersetzen:

```azurecli
subscription="<yourSubID>"
region="westus"
# example value is Standard_E64s_v3
vmSize="<yourVMSize>"

az vm list-skus --resource-type virtualMachines  --location $region --query "[?name=='$vmSize'].capabilities" --subscription $subscription
```

```azurepowershell
$region = "westus"
$vmSize = "Standard_E64s_v3"
(Get-AzComputeResourceSku | where {$_.Locations.Contains($region) -and ($_.Name -eq $vmSize) })[0].Capabilities
```

Die Antwort ist ähnlich der folgenden Form. `UltraSSDAvailable   True` gibt an, ob die Größe des virtuellen Computers Disk Ultra-Datenträger in dieser Region unterstützt.

```
Name                                         Value
----                                         -----
MaxResourceVolumeMB                          884736
OSVhdSizeMB                                  1047552
vCPUs                                        64
HyperVGenerations                            V1,V2
MemoryGB                                     432
MaxDataDiskCount                             32
LowPriorityCapable                           True
PremiumIO                                    True
VMDeploymentTypes                            IaaS
vCPUsAvailable                               64
ACUs                                         160
vCPUsPerCore                                 2
CombinedTempDiskAndCachedIOPS                128000
CombinedTempDiskAndCachedReadBytesPerSecond  1073741824
CombinedTempDiskAndCachedWriteBytesPerSecond 1073741824
CachedDiskBytes                              1717986918400
UncachedDiskIOPS                             80000
UncachedDiskBytesPerSecond                   1258291200
EphemeralOSDiskSupported                     True
AcceleratedNetworkingEnabled                 True
RdmaEnabled                                  False
MaxNetworkInterfaces                         8
UltraSSDAvailable                            True
```

## <a name="deploy-an-ultra-disk-using-azure-resource-manager"></a>Bereitstellen eines Ultra-Datenträgers über Azure Resource Manager

Bestimmen Sie zuerst die Größe des bereitzustellenden virtuellen Computers. Eine Liste der unterstützten VM-Größen finden Sie im Abschnitt [Umfang und Einschränkungen für allgemeine Verfügbarkeit](#ga-scope-and-limitations).

Wenn Sie einen virtuellen Computer mit mehreren Ultra-Datenträgern erstellen möchten, hilft Ihnen das Beispiel zum [Erstellen einer VM mit mehreren Ultra-Datenträgern](https://aka.ms/ultradiskArmTemplate) weiter.

Falls Sie Ihre eigene Vorlage verwenden möchten, sollten Sie sicherstellen, dass **apiVersion** für `Microsoft.Compute/virtualMachines` und `Microsoft.Compute/Disks` auf `2018-06-01` (oder höher) festgelegt ist.

Legen Sie die Datenträger-SKU auf **UltraSSD_LRS** fest, und geben Sie anschließend die Datenträgerkapazität, den IOPS-Wert, die Verfügbarkeitszone und den Durchsatz in MBit/s an, um einen Ultra-Datenträger zu erstellen.

Nachdem der virtuelle Computer bereitgestellt wurde, können Sie die Datenträger für Daten partitionieren und formatieren sowie für Ihre Workloads konfigurieren.


## <a name="deploy-an-ultra-disk-using-the-azure-portal"></a>Bereitstellen eines Ultra-Datenträgers über das Azure-Portal

In diesem Abschnitt wird die Bereitstellung eines virtuellen Computers mit einem Ultra-Datenträger als Datenträger beschrieben. Dabei wird davon ausgegangen, dass Sie mit der Bereitstellung eines virtuellen Computers vertraut sind. Andernfalls finden Sie entsprechende Informationen in [Schnellstart: Erstellen eines virtuellen Windows-Computers im Azure-Portal](./windows/quick-create-portal.md).

- Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an und navigieren Sie zur Bereitstellung eines virtuellen Computers.
- Achten Sie darauf, dass Sie eine [unterstützte VM-Größe und Region](#ga-scope-and-limitations) auswählen.
- Wählen Sie **Verfügbarkeitszone** in **Verfügbarkeitsoptionen** aus.
- Geben Sie die gewünschten Werte für die restlichen Einstellungen an.
- Wählen Sie **Datenträger** aus.

![create-ultra-disk-enabled-vm.png](media/virtual-machines-disks-getting-started-ultra-ssd/create-ultra-disk-enabled-vm.png)

- Wählen Sie auf dem Blatt „Datenträger“ **Ja** für **Ultra-Datenträgerkompatibilität aktivieren** aus.
- Wählen Sie **Neuen Datenträger erstellen und anfügen** aus, um jetzt einen Ultra-Datenträger anzufügen.

![enable-and-attach-ultra-disk.png](media/virtual-machines-disks-getting-started-ultra-ssd/enable-and-attach-ultra-disk.png)

- Geben Sie auf dem Blatt **Neuen Datenträger erstellen** einen Namen ein und wählen Sie dann **Größe ändern** aus.
- Ändern Sie den **Kontotyp** in **Ultra-Datenträger**.
- Ändern Sie die Werte für **Benutzerdefinierte Datenträgergröße (GiB)** , **Datenträger-IOPS** und **Datenträgerdurchsatz** in die gewünschten Werte.
- Wählen Sie in beiden Blättern **OK** aus.
- Setzen Sie die VM-Bereitstellung wie bei der Bereitstellung jeder anderen VM fort.

![create-ultra-disk.png](media/virtual-machines-disks-getting-started-ultra-ssd/create-ultra-disk.png)

## <a name="attach-an-ultra-disk-using-the-azure-portal"></a>Anfügen eines Ultra-Datenträgers über das Azure-Portal

Alternativ können Sie, wenn sich Ihre vorhandene VM in einer Region/Verfügbarkeitszone befindet, in der Ultra-Datenträger verwendet werden können, Ultra-Datenträger verwenden, ohne eine neue VM erstellen zu müssen. Dazu aktivieren Sie Ultra-Datenträger in der vorhandenen VM und fügen diese dann als Datenträger an.

- Navigieren Sie zur VM und wählen Sie **Datenträger** aus.
- Wählen Sie **Bearbeiten** aus.

![options-selector-ultra-disks.png](media/virtual-machines-disks-getting-started-ultra-ssd/options-selector-ultra-disks.png)

- Wählen Sie **Ja** für **Ultra-Datenträgerkompatibilität aktivieren** aus.

![ultra-options-yes-enable.png](media/virtual-machines-disks-getting-started-ultra-ssd/ultra-options-yes-enable.png)

- Wählen Sie **Speichern** aus.
- Wählen Sie **Datenträger hinzufügen** und dann im Dropdown für **Name** die Option **Datenträger erstellen** aus.

![create-and-attach-new-ultra-disk.png](media/virtual-machines-disks-getting-started-ultra-ssd/create-and-attach-new-ultra-disk.png)

- Geben Sie einen Namen für den neuen Datenträger ein und wählen Sie dann **Größe ändern** aus.
- Ändern Sie den **Kontotyp** in **Ultra-Datenträger**.
- Ändern Sie die Werte für **Benutzerdefinierte Datenträgergröße (GiB)** , **Datenträger-IOPS** und **Datenträgerdurchsatz** in die gewünschten Werte.
- Wählen Sie **OK** und anschließend **Erstellen** aus.

![making-a-new-ultra-disk.png](media/virtual-machines-disks-getting-started-ultra-ssd/making-a-new-ultra-disk.png)

- Wählen Sie nach Rückkehr zum Blatt für den Datenträger **Speichern** aus.

![saving-and-attaching-new-ultra-disk.png](media/virtual-machines-disks-getting-started-ultra-ssd/saving-and-attaching-new-ultra-disk.png)

### <a name="adjust-the-performance-of-an-ultra-disk-using-the-azure-portal"></a>Anpassen der Leistung eines Ultra-Datenträgers über das Azure-Portal

Ultra-Datenträger haben eine einzigartige Funktion, mit der Sie deren Leistung anpassen können. Sie können diese Anpassung über das Azure-Portal für die jeweiligen Datenträger durchführen.

- Navigieren Sie zur VM und wählen Sie **Datenträger** aus.
- Wählen Sie den Ultra-Datenträger aus, dessen Leistung Sie ändern möchten.

![selecting-ultra-disk-to-modify.png](media/virtual-machines-disks-getting-started-ultra-ssd/selecting-ultra-disk-to-modify.png)

- Wählen Sie **Konfiguration** aus und nehmen Sie dann die gewünschten Änderungen vor.
- Wählen Sie **Speichern** aus.

![configuring-ultra-disk-performance-and-size.png](media/virtual-machines-disks-getting-started-ultra-ssd/configuring-ultra-disk-performance-and-size.png)

## <a name="deploy-an-ultra-disk-using-cli"></a>Bereitstellen eines Ultra-Datenträgers über CLI

Bestimmen Sie zuerst die Größe des bereitzustellenden virtuellen Computers. Eine Liste der unterstützten VM-Größen finden Sie im Abschnitt [Umfang und Einschränkungen für allgemeine Verfügbarkeit](#ga-scope-and-limitations).

Sie müssen einen virtuellen Computer erstellen, der Ultra-Datenträger verwenden kann. Nur dann können Sie einen Ultra-Datenträger zuordnen.

Ersetzen Sie die Variablen **$vmname**, **$rgname**, **$diskname**, **$location**, **$password** und **$user** durch Ihre eigenen Werte, bzw. legen Sie sie fest. Legen Sie **$zone** auf den Wert Ihrer Verfügbarkeitszone fest, den Sie am [Anfang dieses Artikels](#determine-vm-size-and-region-availability) ermittelt haben. Führen Sie anschließend den folgenden CLI-Befehl aus, um eine Ultra-fähige VM zu erstellen:

```azurecli-interactive
az vm create --subscription $subscription -n $vmname -g $rgname --image Win2016Datacenter --ultra-ssd-enabled true --zone $zone --authentication-type password --admin-password $password --admin-username $user --size Standard_D4s_v3 --location $location
```

### <a name="enable-ultra-disk-compatibility-on-an-existing-vm"></a>Aktivieren der Disk Ultra-Datenträgerkompatibilität auf einem vorhandenen virtuellen Computer

Wenn Ihre VM die in [Umfang und Einschränkungen für allgemeine Verfügbarkeit](#ga-scope-and-limitations) genannten Anforderungen erfüllt und sich in der [geeigneten Zone für Ihr Konto](#determine-vm-size-and-region-availability) befindet, können Sie auf Ihrem virtuellen Computer die Disk Ultra-Datenträgerkompatibilität aktivieren.

Sie müssen den virtuellen Computer beenden, um die Disk Ultra-Datenträgerkompatibilität zu aktivieren. Nachdem Sie den virtuellen Computer beendet haben, können Sie die Kompatibilität aktivieren, einen Disk Ultra-Datenträger anschließen und dann den virtuellen Computer neu starten:

```azurecli
az vm deallocate -n $vmName -g $rgName
az vm update -n $vmName -g $rgName --ultra-ssd-enabled true
az vm start -n $vmName -g $rgName
```

### <a name="create-an-ultra-disk-using-cli"></a>Erstellen eines Ultra-Datenträgers über CLI

Nachdem Sie nun über eine VM verfügen, der Ultra-Datenträger zugeordnet werden können, können Sie einen Ultra-Datenträger erstellen und diesen der VM zuordnen.

```azurecli-interactive
location="eastus2"
subscription="xxx"
rgname="ultraRG"
diskname="ssd1"
vmname="ultravm1"
zone=123

#create an ultra disk
az disk create `
--subscription $subscription `
-n $diskname `
-g $rgname `
--size-gb 4 `
--location $location `
--zone $zone `
--sku UltraSSD_LRS `
--disk-iops-read-write 1000 `
--disk-mbps-read-write 50
```

## <a name="attach-an-ultra-disk-to-a-vm-using-cli"></a>Zuordnen eines Ultra-Datenträgers zu einer VM über CLI

Alternativ können Sie, wenn sich Ihre vorhandene VM in einer Region/Verfügbarkeitszone befindet, in der Ultra-Datenträger verwendet werden können, Ultra-Datenträger verwenden, ohne eine neue VM erstellen zu müssen.

```azurecli
rgName="<yourResourceGroupName>"
vmName="<yourVMName>"
diskName="<yourDiskName>"
subscriptionId="<yourSubscriptionID>"

az vm disk attach -g $rgName --vm-name $vmName --disk $diskName --subscription $subscriptionId
```

### <a name="adjust-the-performance-of-an-ultra-disk-using-cli"></a>Anpassen der Leistung eines Ultra-Datenträgers über CLI

Ultra-Datenträger haben eine einzigartige Funktion, mit der Sie deren Leistung anpassen können. Mit dem folgenden Befehl wird die Verwendung dieser Funktion veranschaulicht:

```azurecli-interactive
az disk update `
--subscription $subscription `
--resource-group $rgname `
--name $diskName `
--set diskIopsReadWrite=80000 `
--set diskMbpsReadWrite=800
```

## <a name="deploy-an-ultra-disk-using-powershell"></a>Bereitstellen eines Ultra-Datenträgers über PowerShell

Bestimmen Sie zuerst die Größe des bereitzustellenden virtuellen Computers. Eine Liste der unterstützten VM-Größen finden Sie im Abschnitt [Umfang und Einschränkungen für allgemeine Verfügbarkeit](#ga-scope-and-limitations).

Um Ultra-Datenträger zu verwenden, müssen Sie eine VM erstellen, für die das Verwenden von Ultra-Datenträgern unterstützt wird. Ersetzen Sie die Variablen **$resourcegroup** und **$vmName** durch Ihre eigenen Werte. Legen Sie **$zone** auf den Wert Ihrer Verfügbarkeitszone fest, den Sie am [Anfang dieses Artikels](#determine-vm-size-and-region-availability) ermittelt haben. Führen Sie anschließend den folgenden [New-AzVm](/powershell/module/az.compute/new-azvm)-Befehl aus, um eine Ultra-fähige VM zu erstellen:

```powershell
New-AzVm `
    -ResourceGroupName $resourcegroup `
    -Name $vmName `
    -Location "eastus2" `
    -Image "Win2016Datacenter" `
    -EnableUltraSSD $true `
    -size "Standard_D4s_v3" `
    -zone $zone
```

### <a name="enable-ultra-disk-compatibility-on-an-existing-vm"></a>Aktivieren der Disk Ultra-Datenträgerkompatibilität auf einem vorhandenen virtuellen Computer

Wenn Ihre VM die in [Umfang und Einschränkungen für allgemeine Verfügbarkeit](#ga-scope-and-limitations) genannten Anforderungen erfüllt und sich in der [geeigneten Zone für Ihr Konto](#determine-vm-size-and-region-availability) befindet, können Sie auf Ihrem virtuellen Computer die Disk Ultra-Datenträgerkompatibilität aktivieren.

Sie müssen den virtuellen Computer beenden, um die Disk Ultra-Datenträgerkompatibilität zu aktivieren. Nachdem Sie den virtuellen Computer beendet haben, können Sie die Kompatibilität aktivieren, einen Disk Ultra-Datenträger anschließen und dann den virtuellen Computer neu starten:

```azurepowershell
#stop the VM
$vm1 = Get-AzureRMVM -name $vmName -ResourceGroupName $rgName
Update-AzureRmVM -ResourceGroupName $rgName -VM $vm1 -UltraSSDEnabled 1
#start the VM
```

### <a name="create-an-ultra-disk-using-powershell"></a>Erstellen eines Ultra-Datenträgers über PowerShell

Nachdem Sie nun über eine VM verfügen, für die Ultra-Datenträger verwendet werden können, können Sie einen Ultra-Datenträger erstellen und diesen der VM zuordnen:

```powershell
$diskconfig = New-AzDiskConfig `
-Location 'EastUS2' `
-DiskSizeGB 8 `
-DiskIOPSReadWrite 1000 `
-DiskMBpsReadWrite 100 `
-AccountType UltraSSD_LRS `
-CreateOption Empty `
-zone $zone;

New-AzDisk `
-ResourceGroupName $resourceGroup `
-DiskName 'Disk02' `
-Disk $diskconfig;
```

## <a name="attach-an-ultra-disk-to-a-vm-using-powershell"></a>Zuordnen eines Ultra-Datenträgers zu einer VM über PowerShell

Alternativ können Sie, wenn sich Ihre vorhandene VM in einer Region/Verfügbarkeitszone befindet, in der Ultra-Datenträger verwendet werden können, Ultra-Datenträger verwenden, ohne eine neue VM erstellen zu müssen.

```powershell
# add disk to VM
$subscription = "<yourSubscriptionID>"
$resourceGroup = "<yourResourceGroup>"
$vmName = "<yourVMName>"
$diskName = "<yourDiskName>"
$lun = 1
Login-AzureRMAccount -SubscriptionId $subscription
$vm = Get-AzVM -ResourceGroupName $resourceGroup -Name $vmName
$disk = Get-AzDisk -ResourceGroupName $resourceGroup -Name $diskName
$vm = Add-AzVMDataDisk -VM $vm -Name $diskName -CreateOption Attach -ManagedDiskId $disk.Id -Lun $lun
Update-AzVM -VM $vm -ResourceGroupName $resourceGroup
```

### <a name="adjust-the-performance-of-an-ultra-disk-using-powershell"></a>Anpassen der Leistung eines Ultra-Datenträgers über PowerShell

Ultra-Datenträger haben eine einzigartige Funktion, mit der Sie die Leistung anpassen können. Der folgende Befehl ist ein Beispiel für eine Anpassung der Leistung, ohne dass der Datenträger getrennt werden muss:

```powershell
$diskupdateconfig = New-AzDiskUpdateConfig -DiskMBpsReadWrite 2000
Update-AzDisk -ResourceGroupName $resourceGroup -DiskName $diskName -DiskUpdate $diskupdateconfig
```
## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie unter [Verwenden von Azure Ultra Disks in Azure Kubernetes Service (Vorschauversion)](../aks/use-ultra-disks.md).
