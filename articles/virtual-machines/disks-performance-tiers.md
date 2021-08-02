---
title: Ändern der Leistung verwalteter Azure-Datenträger – CLI/PowerShell
description: Erfahren Sie, wie Sie Leistungsstufen für vorhandene verwaltete Datenträger mithilfe des Azure PowerShell-Moduls oder der Azure CLI ändern können.
author: roygara
ms.service: virtual-machines
ms.topic: how-to
ms.date: 05/13/2021
ms.author: rogarana
ms.subservice: disks
ms.custom: references_regions, devx-track-azurecli, devx-track-azurepowershell
ms.openlocfilehash: 32f589be39740d62ef77967867522688d29a3618
ms.sourcegitcommit: df574710c692ba21b0467e3efeff9415d336a7e1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/28/2021
ms.locfileid: "110673497"
---
# <a name="change-your-performance-tier-using-the-azure-powershell-module-or-the-azure-cli"></a>Ändern der Leistungsstufe mithilfe des Azure PowerShell-Moduls oder der Azure CLI

[!INCLUDE [virtual-machines-disks-performance-tiers-intro](../../includes/virtual-machines-disks-performance-tiers-intro.md)]

## <a name="restrictions"></a>Beschränkungen

[!INCLUDE [virtual-machines-disks-performance-tiers-restrictions](../../includes/virtual-machines-disks-performance-tiers-restrictions.md)]

## <a name="create-an-empty-data-disk-with-a-tier-higher-than-the-baseline-tier"></a>Erstellen/Aktualisieren eines Datenträgers mit einer höheren Stufe als der Baselinestufe

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

```azurecli
subscriptionId=<yourSubscriptionIDHere>
resourceGroupName=<yourResourceGroupNameHere>
diskName=<yourDiskNameHere>
diskSize=<yourDiskSizeHere>
performanceTier=<yourDesiredPerformanceTier>
region=westcentralus

az login

az account set --subscription $subscriptionId

az disk create -n $diskName -g $resourceGroupName -l $region --sku Premium_LRS --size-gb $diskSize --tier $performanceTier
```
## <a name="create-an-os-disk-with-a-tier-higher-than-the-baseline-tier-from-an-azure-marketplace-image"></a>Erstellen eines Betriebssystemdatenträgers mit einer höheren Stufe als der Baselinestufe aus einem Azure Marketplace-Image

```azurecli
resourceGroupName=<yourResourceGroupNameHere>
diskName=<yourDiskNameHere>
performanceTier=<yourDesiredPerformanceTier>
region=westcentralus
image=Canonical:UbuntuServer:18.04-LTS:18.04.202002180

az disk create -n $diskName -g $resourceGroupName -l $region --image-reference $image --sku Premium_LRS --tier $performanceTier
```

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell
$subscriptionId='yourSubscriptionID'
$resourceGroupName='yourResourceGroupName'
$diskName='yourDiskName'
$diskSizeInGiB=4
$performanceTier='P50'
$sku='Premium_LRS'
$region='westcentralus'

Connect-AzAccount

Set-AzContext -Subscription $subscriptionId

$diskConfig = New-AzDiskConfig -SkuName $sku -Location $region -CreateOption Empty -DiskSizeGB $diskSizeInGiB -Tier $performanceTier
New-AzDisk -DiskName $diskName -Disk $diskConfig -ResourceGroupName $resourceGroupName
```
---

## <a name="update-the-tier-of-a-disk"></a>Aktualisieren der Stufe eines Datenträgers

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

```azurecli
resourceGroupName=<yourResourceGroupNameHere>
diskName=<yourDiskNameHere>
performanceTier=<yourDesiredPerformanceTier>

az disk update -n $diskName -g $resourceGroupName --set tier=$performanceTier
```

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell
$resourceGroupName='yourResourceGroupName'
$diskName='yourDiskName'
$performanceTier='P1'

$diskUpdateConfig = New-AzDiskUpdateConfig -Tier $performanceTier

Update-AzDisk -ResourceGroupName $resourceGroupName -DiskName $diskName -DiskUpdate $diskUpdateConfig
```
---

## <a name="show-the-tier-of-a-disk"></a>Anzeigen der Stufe eines Datenträgers

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

```azurecli
az disk show -n $diskName -g $resourceGroupName --query [tier] -o tsv
```

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell
$disk = Get-AzDisk -ResourceGroupName $resourceGroupName -DiskName $diskName

$disk.Tier
```
---

## <a name="change-the-performance-tier-of-a-disk-without-downtime-preview"></a>Ändern der Leistungsstufe eines Datenträgers ohne Downtime (Vorschau)

Sie können Ihre Leistungsstufe auch ohne Downtime ändern, damit Sie Ihre VM nicht freigeben oder Ihren Datenträger trennen müssen, um die Stufe zu ändern.

### <a name="prerequisites"></a>Voraussetzungen

Ihr Datenträger muss die Anforderungen erfüllen, die im Abschnitt [Leistungsstufe ohne Downtime ändern (Vorschau)](#change-performance-tier-without-downtime-preview) beschrieben sind. Wenn dies nicht der Fall ist, kommt es beim Ändern der Leistungsstufe zu Ausfallzeiten.

Sie müssen das Feature für Ihr Abonnement aktivieren, bevor Sie die Leistungsstufe eines Datenträgers ohne Ausfallzeiten ändern können. Führen Sie die folgenden Schritte aus, um das Feature für Ihr Abonnement zu aktivieren:

1.  Führen Sie den folgenden Befehl aus, um das Feature für Ihr Abonnement zu registrieren.

    ```azurecli
    az feature register --namespace Microsoft.Compute --name LiveTierChange
    ```
 
1.  Überprüfen Sie mithilfe des folgenden Befehls, ob der Registrierungsstatus **Registriert** lautet (dies kann einige Minuten dauern), bevor Sie das Feature ausprobieren.

    ```azurecli
    az feature show --namespace Microsoft.Compute --name LiveTierChange
    ```

### <a name="update-the-performance-tier-of-a-disk-without-downtime-via-azure-cli"></a>Aktualisieren der Leistungsstufe eines Datenträgers ohne Downtime über die Azure CLI

Das folgende Skript aktualisiert die Stufe eines Datenträgers, der höher als die Basisstufe ist. Ersetzen Sie `<yourResourceGroupNameHere>`, `<yourDiskNameHere>` und `<yourDesiredPerformanceTier>`, und führen Sie dann das Skript aus:

```azurecli
resourceGroupName=<yourResourceGroupNameHere>
diskName=<yourDiskNameHere>
performanceTier=<yourDesiredPerformanceTier>
 
az disk update -n $diskName -g $resourceGroupName --set tier=$performanceTier
```

### <a name="update-the-performance-tier-of-a-disk-without-downtime-via-arm-template"></a>Aktualisieren der Leistungsstufe eines Datenträgers ohne Downtime mithilfe einer ARM-Vorlage

Das folgende Skript aktualisiert die Stufe eines Datenträgers, die höher als die Baselinestufe ist, mithilfe der Beispielvorlage [CreateUpdateDataDiskWithTier.json](https://github.com/Azure/azure-managed-disks-performance-tiers/blob/main/CreateUpdateDataDiskWithTier.json). Ersetzen Sie `<yourSubScriptionID>`, `<yourResourceGroupName>`, `<yourDiskName>`, `<yourDiskSize>` und `<yourDesiredPerformanceTier>`, und führen Sie dann das Skript aus:

 ```cli
subscriptionId=<yourSubscriptionID>
resourceGroupName=<yourResourceGroupName>
diskName=<yourDiskName>
diskSize=<yourDiskSize>
performanceTier=<yourDesiredPerformanceTier>
region=EastUS2EUAP

 az login

 az account set --subscription $subscriptionId

 az group deployment create -g $resourceGroupName \
--template-uri "https://raw.githubusercontent.com/Azure/azure-managed-disks-performance-tiers/main/CreateUpdateDataDiskWithTier.json" \
--parameters "region=$region" "diskName=$diskName" "performanceTier=$performanceTier" "dataDiskSizeInGb=$diskSize"
```

## <a name="confirm-your-disk-has-changed-tiers"></a>Überprüfen der Leistungsstufenänderung Ihres Datenträgers

Ein Wechsel der Leistungsstufe kann bis zu 15 Minuten dauern. Verwenden Sie eine der folgenden Methoden, um zu bestätigen, dass Ihr Datenträger die Leistungsstufe gewechselt hat:

### <a name="azure-resource-manager"></a>Azure Resource Manager

```cli
az resource show -n $diskName -g $resourceGroupName --namespace Microsoft.Compute --resource-type disks --api-version 2020-12-01 --query [properties.tier] -o tsv
```

### <a name="azure-cli"></a>Azure CLI

```azurecli
az disk show -n $diskName -g $resourceGroupName --query [tier] -o tsv
```

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie die Größe eines Datenträgers ändern müssen, um die höheren Leistungsstufen zu nutzen, lesen Sie die folgenden Artikel:

- [Erweitern von virtuellen Festplatten auf virtuellen Linux-Computern mit der Azure-CLI](linux/expand-disks.md)
- [Erweitern eines verwalteten Datenträgers, der an einen virtuellen Windows-Computer angefügt ist](windows/expand-os-disk.md)
