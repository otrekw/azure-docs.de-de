---
title: Redundanzoptionen für verwaltete Azure-Datenträger
description: Erfahren Sie mehr über zonenredundanten Speicher und lokal redundanten Speicher für verwaltete Azure-Datenträger.
author: roygara
ms.author: rogarana
ms.date: 05/26/2021
ms.topic: how-to
ms.service: virtual-machines
ms.subservice: disks
ms.custom: references_regions, devx-track-azurepowershell
ms.openlocfilehash: 5bb6d0b66e365904e7f0fe4f523f3c19a48d5361
ms.sourcegitcommit: df574710c692ba21b0467e3efeff9415d336a7e1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/28/2021
ms.locfileid: "110673429"
---
# <a name="redundancy-options-for-managed-disks"></a>Redundanzoptionen für verwaltete Datenträger

Verwaltete Azure-Datenträger bieten zwei Optionen für die Speicherredundanz: zonenredundanten Speicher (ZRS) als Vorschau und lokal redundanten Speicher. Zonenredundanter Speicher (ZRS) bietet eine höhere Verfügbarkeit für verwaltete Datenträger als lokal redundanter Speicher (LRS). Allerdings ist die Wartezeit beim Schreiben bei LRS-Datenträgern besser als bei ZRS-Datenträgern, da LRS-Datenträger Daten synchron auf drei Kopien in einem einzelnen Rechenzentrum schreiben.

## <a name="locally-redundant-storage-for-managed-disks"></a>Lokal redundanter Speicher für verwaltete Datenträger

Bei lokal redundantem Speicher (LRS) werden die Daten innerhalb eines einzelnen Rechenzentrums in der ausgewählten Region repliziert. LRS schützt Ihre Daten vor Serverrack- und Laufwerkfehlern. 

Es gibt einige Möglichkeiten, wie Sie Ihre Anwendung mithilfe von LRS-Datenträgern vor einem möglichen Ausfall der gesamten Zone aufgrund von Naturkatastrophen oder Hardwareproblemen schützen können:
- Verwenden Sie eine Anwendung wie SQL Server AlwaysOn, die Daten synchron in zwei Zonen schreiben kann und im Notfall ein automatisches Failover in eine andere Zone durchführt.
- Führen Sie regelmäßige Sicherungen von LRS-Datenträgern mit ZRS-Momentaufnahmen durch.
- Aktivieren Sie die zonenübergreifende Notfallwiederherstellung für LRS-Datenträger über [Azure Site Recovery](../site-recovery/azure-to-azure-how-to-enable-zone-to-zone-disaster-recovery.md). Eine zonenübergreifende Notfallwiederherstellung bietet jedoch kein Null-RPO (Recovery Point Objective).

Wenn Ihr Workflow keine zonenübergreifenden synchronen Schreibvorgänge auf Anwendungsebene unterstützt oder Ihre Anwendung ein Null-RPO erfüllen muss, dann wären ZRS-Datenträger ideal.

## <a name="zone-redundant-storage-for-managed-disks-preview"></a>Zonenredundanter Speicher für verwaltete Datenträger (Vorschau)

Bei zonenredundantem Speicher (ZRS) wird Ihr verwalteter Azure-Datenträger synchron über drei Azure-Verfügbarkeitszonen hinweg in der ausgewählten Region repliziert. Jede Verfügbarkeitszone ist ein getrennter physischer Standort mit unabhängigen Stromversorgungs-, Kühlungs- und Netzwerkgeräten. 

ZRS-Datenträger ermöglichen die Wiederherstellung nach Fehlern in Verfügbarkeitszonen. Wenn eine gesamte Zone ausgefallen ist, kann ein ZRS-Datenträger an einen virtuellen Computer in einer anderen Zone angefügt werden. ZRS-Datenträger können auch als freigegebene Datenträger verwendet werden, um eine bessere Verfügbarkeit für gruppierte oder verteilte Anwendungen wie SQL FCI, SAP ASCS/SCS oder GFS2 zu bieten. Sie können einen freigegebenen ZRS-Datenträger an primäre und sekundäre VMs in verschiedenen Zonen anfügen, um sowohl ZRS als auch [Verfügbarkeitszonen](../availability-zones/az-overview.md) zu nutzen. Wenn bei der primären Zone ein Fehler auftritt, können Sie mithilfe von [permanenter SCSI-Reservierung](disks-shared-enable.md#supported-scsi-pr-commands) schnell ein Failover auf den sekundären virtuellen Computer durchführen.

### <a name="billing-implications"></a>Hinweise zur Gebührenberechnung

Details finden Sie auf der Seite [Azure-Preise](https://azure.microsoft.com/pricing/details/managed-disks/).

### <a name="comparison-with-other-disk-types"></a>Vergleich mit anderen Datenträgertypen

Abgesehen von einer höheren Wartezeit beim Schreiben sind Datenträger, die ZRS verwenden, identisch mit Datenträgern, die LRS verwenden. Sie weisen dieselben Leistungsziele auf. Es wird empfohlen, ein [Datenträger-Benchmarking](disks-benchmarks.md) durchzuführen, um die Workload Ihrer Anwendung zu simulieren, um die Latenz zwischen den LRS- und ZRS-Datenträgern zu vergleichen. 

### <a name="limitations"></a>Einschränkungen

Während der Vorschau gelten für ZRS für verwaltete Datenträger die folgenden Einschränkungen:

- Wird nur für SSD Premium- und SSD Standard-Datenträger unterstützt.
- Derzeit nur in den Regionen „USA, Westen 2“, „Europa, Westen“, „Europa, Norden“ und „Frankreich, Mitte“ verfügbar.
- ZRS-Datenträger können nur mit einer der folgenden Methoden erstellt werden:
    -  Azure Resource Manager-Vorlagen mit API-Version `2020-12-01` in der öffentlichen Vorschau
    - Der neuesten Version der Azure-Befehlszeilenschnittstelle


### <a name="create-zrs-managed-disks"></a>Erstellen von verwalteten ZRS-Datenträgern

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

#### <a name="prerequisites"></a>Voraussetzungen

Sie müssen das Features für Ihr Abonnement aktivieren. Führen Sie die folgenden Schritte aus, um das Feature für Ihr Abonnement zu aktivieren:

1.  Führen Sie den folgenden Befehl aus, um das Feature für Ihr Abonnement zu registrieren.

    ```azurecli
    az feature register --namespace Microsoft.Compute --name SsdZrsManagedDisks
    ```
 
2.  Überprüfen Sie mithilfe des folgenden Befehls, ob der Registrierungsstatus **Registriert** lautet (dies kann einige Minuten dauern), bevor Sie das Feature ausprobieren.

    ```azurecli
    az feature show --namespace Microsoft.Compute --name SsdZrsManagedDisks
    ```

#### <a name="create-a-vm-with-zrs-disks"></a>Erstellen einer VM mit ZRS-Datenträgern

```azurecli
rgName=yourRGName
vmName=yourVMName
location=westus2
vmSize=Standard_DS2_v2
image=UbuntuLTS 
osDiskSku=StandardSSD_ZRS
dataDiskSku=Premium_ZRS


az vm create -g $rgName \
-n $vmName \
-l $location \
--image $image \
--size $vmSize \
--generate-ssh-keys \
--data-disk-sizes-gb 128 \
--storage-sku os=$osDiskSku 0=$dataDiskSku
```
#### <a name="create-vms-with-a-shared-zrs-disk-attached-to-the-vms-in-different-zones"></a>Erstellen von VMs mit einem freigegebenen ZRS-Datenträger, der an die VMs in verschiedenen Zonen angefügt ist
```azurecli

location=westus2
rgName=yourRGName
vmNamePrefix=yourVMNamePrefix
vmSize=Standard_DS2_v2
image=UbuntuLTS
osDiskSku=StandardSSD_LRS
sharedDiskName=yourSharedDiskName
sharedDataDiskSku=Premium_ZRS


az disk create -g $rgName \
-n $sharedDiskName \
-l $location \
--size-gb 1024 \
--sku $sharedDataDiskSku \
--max-shares 2


sharedDiskId=$(az disk show -g $rgName -n $sharedDiskName --query 'id' -o tsv)

az vm create -g $rgName \
-n $vmNamePrefix"01" \
-l $location \
--image $image \
--size $vmSize \
--generate-ssh-keys \
--zone 1 \
--attach-data-disks $sharedDiskId \
--storage-sku os=$osDiskSku \
--vnet-name $vmNamePrefix"_vnet" \
--subnet $vmNamePrefix"_subnet"

az vm create -g $rgName \
-n $vmNamePrefix"02" \
-l $location \
--image $image \
--size $vmSize \
--generate-ssh-keys \
--zone 2 \
--attach-data-disks $sharedDiskId \
--storage-sku os=$osDiskSku \
--vnet-name $vmNamePrefix"_vnet" \
--subnet $vmNamePrefix"_subnet"

```
#### <a name="create-a-virtual-machine-scale-set-with-zrs-disks"></a>Erstellen einer VM-Skalierungsgruppe mit ZRS-Datenträgern
```azurecli
location=westus2
rgName=yourRGName
vmssName=yourVMSSName
vmSize=Standard_DS3_V2
image=UbuntuLTS 
osDiskSku=StandardSSD_ZRS
dataDiskSku=Premium_ZRS

az vmss create -g $rgName \
-n $vmssName \
--encryption-at-host \
--image UbuntuLTS \
--upgrade-policy automatic \
--generate-ssh-keys \
--data-disk-sizes-gb 128 \
--storage-sku os=$osDiskSku 0=$dataDiskSku
```

# <a name="resource-manager-template"></a>[Resource Manager-Vorlage](#tab/azure-resource-manager)

Verwenden Sie die `2020-12-01`-API mit Ihrer Azure Resource Manager-Vorlage, um einen ZRS-Datenträger zu erstellen.

#### <a name="prerequisites"></a>Voraussetzungen

Sie müssen das Features für Ihr Abonnement aktivieren. Führen Sie die folgenden Schritte aus, um das Feature für Ihr Abonnement zu aktivieren:

1.  Führen Sie den folgenden Befehl aus, um das Feature für Ihr Abonnement zu registrieren.

    ```powershell
     Register-AzProviderFeature -FeatureName "SsdZrsManagedDisks" -ProviderNamespace "Microsoft.Compute" 
    ```

2.  Überprüfen Sie mithilfe des folgenden Befehls, ob der Registrierungsstatus **Registriert** lautet (dies kann einige Minuten dauern), bevor Sie das Feature ausprobieren.

    ```powershell
     Get-AzProviderFeature -FeatureName "SsdZrsManagedDisks" -ProviderNamespace "Microsoft.Compute"  
    ```
    
#### <a name="create-a-vm-with-zrs-disks"></a>Erstellen einer VM mit ZRS-Datenträgern

```
$vmName = "yourVMName" 
$adminUsername = "yourAdminUsername"
$adminPassword = ConvertTo-SecureString "yourAdminPassword" -AsPlainText -Force
$osDiskType = "StandardSSD_ZRS"
$dataDiskType = "Premium_ZRS"
$region = "eastus2euap"
$resourceGroupName = "yourResourceGroupName"

New-AzResourceGroup -Name $resourceGroupName -Location $region
New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName `
-TemplateUri "https://raw.githubusercontent.com/Azure-Samples/managed-disks-powershell-getting-started/master/ZRSDisks/CreateVMWithZRSDataDisks.json" `
-resourceName $vmName `
-adminUsername $adminUsername `
-adminPassword $adminPassword `
-region $region `
-osDiskType $osDiskType `
-dataDiskType $dataDiskType
```

#### <a name="create-vms-with-a-shared-zrs-disk-attached-to-the-vms-in-different-zones"></a>Erstellen von VMs mit einem freigegebenen ZRS-Datenträger, der an die VMs in verschiedenen Zonen angefügt ist

```
$vmNamePrefix = "yourVMNamePrefix"
$adminUsername = "yourAdminUserName"
$adminPassword = ConvertTo-SecureString "yourAdminPassword" -AsPlainText -Force
$osDiskType = "StandardSSD_LRS"
$sharedDataDiskType = "Premium_ZRS"
$region = "eastus2euap"
$resourceGroupName = "zrstesting1"

New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName `
-TemplateUri "https://raw.githubusercontent.com/Azure-Samples/managed-disks-powershell-getting-started/master/ZRSDisks/CreateVMsWithASharedDisk.json" `
-vmNamePrefix $vmNamePrefix `
-adminUsername $adminUsername `
-adminPassword $adminPassword `
-region $region `
-osDiskType $osDiskType `
-dataDiskType $sharedDataDiskType
```

#### <a name="create-a-virtual-machine-scale-set-with-zrs-disks"></a>Erstellen einer VM-Skalierungsgruppe mit ZRS-Datenträgern

```
$vmssName="yourVMSSName"
$adminUsername="yourAdminName"
$adminPassword=ConvertTo-SecureString "yourAdminPassword" -AsPlainText -Force
$region="eastus2euap"
$osDiskType="StandardSSD_LRS"
$dataDiskType="Premium_ZRS"

New-AzResourceGroupDeployment -ResourceGroupName zrstesting `
-TemplateUri "https://raw.githubusercontent.com/Azure-Samples/managed-disks-powershell-getting-started/master/ZRSDisks/CreateVMSSWithZRSDisks.json" `
-vmssName "yourVMSSName" `
-adminUsername "yourAdminName" `
-adminPassword $password `
-region "eastus2euap" `
-osDiskType "StandardSSD_LRS" `
-dataDiskType "Premium_ZRS" `
```
---

## <a name="next-steps"></a>Nächste Schritte

- Sehen Sie sich weitere [Azure Resource Manager-Beispielvorlagen zum Erstellen einer VM mit ZRS-Datenträgern](https://github.com/Azure-Samples/managed-disks-powershell-getting-started/tree/master/ZRSDisks) an.
