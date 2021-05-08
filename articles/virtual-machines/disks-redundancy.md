---
title: Redundanzoptionen für verwaltete Azure-Datenträger
description: Erfahren Sie mehr über zonenredundanten Speicher und lokal redundanten Speicher für verwaltete Azure-Datenträger.
author: roygara
ms.author: rogarana
ms.date: 03/02/2021
ms.topic: how-to
ms.service: virtual-machines
ms.subservice: disks
ms.custom: references_regions
ms.openlocfilehash: 0882efeccfc8dc83686d75ab39b8364219c3b5f1
ms.sourcegitcommit: 272351402a140422205ff50b59f80d3c6758f6f6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/17/2021
ms.locfileid: "107588087"
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

### <a name="limitations"></a>Einschränkungen

Während der Vorschau gelten für ZRS für verwaltete Datenträger die folgenden Einschränkungen:

- Wird nur für SSD Premium- und SSD Standard-Datenträger unterstützt.
- Derzeit nur in der Region EastUS2EUAP verfügbar.
- ZRS-Datenträger können nur mit Azure Resource Manager-Vorlagen erstellt werden, die die `2020-12-01`-API verwenden.

Registrieren Sie sich [hier](https://aka.ms/ZRSDisksPreviewSignUp) für die Vorschauversion.

### <a name="billing-implications"></a>Hinweise zur Gebührenberechnung

Details finden Sie auf der Seite [Azure-Preise](https://azure.microsoft.com/pricing/details/managed-disks/).

### <a name="comparison-with-other-disk-types"></a>Vergleich mit anderen Datenträgertypen

Abgesehen von einer höheren Wartezeit beim Schreiben sind Datenträger, die ZRS verwenden, identisch mit Datenträgern, die LRS verwenden. Sie weisen dieselben Leistungsziele auf. Es wird empfohlen, ein [Datenträger-Benchmarking](disks-benchmarks.md) durchzuführen, um die Workload Ihrer Anwendung zu simulieren, um die Latenz zwischen den LRS- und ZRS-Datenträgern zu vergleichen. 

### <a name="create-zrs-managed-disks"></a>Erstellen von verwalteten ZRS-Datenträgern

Verwenden Sie die `2020-12-01`-API mit Ihrer Azure Resource Manager-Vorlage, um einen ZRS-Datenträger zu erstellen.

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

## <a name="next-steps"></a>Nächste Schritte

- Erstellen Sie mit den verlinkten [Azure Resource Manager-Beispielvorlagen eine VM mit ZRS-Datenträgern](https://github.com/Azure-Samples/managed-disks-powershell-getting-started/tree/master/ZRSDisks).
