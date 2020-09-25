---
title: Steigern der Leistung von verwalteten Azure-Datenträgern
description: Hier erhalten Sie Informationen zu Leistungsstufen für verwaltete Datenträger sowie zum Upgrade der Leistungsstufen für Ihre verwalteten Datenträger.
author: roygara
ms.service: virtual-machines
ms.topic: how-to
ms.date: 09/22/2020
ms.author: rogarana
ms.subservice: disks
ms.custom: references_regions
ms.openlocfilehash: aa188babf56d4a825059fe6103e2e07745eb134f
ms.sourcegitcommit: bdd5c76457b0f0504f4f679a316b959dcfabf1ef
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90974137"
---
# <a name="performance-tiers-for-managed-disks-preview"></a>Leistungsstufen für verwaltete Datenträger (Vorschau)

Azure Disk Storage bietet derzeit integrierte Burstfunktionen, um eine höhere Leistung bei der Handhabung von kurzfristigem, unerwarteten Datenverkehr zu erreichen. SSD Premium-Datenträger verfügen über die Flexibilität, die Leistung der Datenträger zu erhöhen, ohne die tatsächliche Größe der Datenträger zu erhöhen, sodass Sie Ihre Workloadleistungsanforderungen erfüllen und die Kosten senken können. Dieses Feature befindet sich derzeit in der Vorschauversion. Dies ist ideal für Ereignisse, die vorübergehend ein konstant höheres Leistungsniveau erfordern, z. B. Feiertagseinkäufe, Leistungstests oder die Ausführung einer Trainingsumgebung. Um diese Ereignisse zu behandeln, können Sie so lange wie nötig eine höhere Leistungsstufe wählen und zur ursprünglichen Stufe zurückkehren, wenn die zusätzliche Leistung nicht mehr erforderlich ist.

## <a name="how-it-works"></a>Funktionsweise

Wenn Sie einen Datenträger zum ersten Mal bereitstellen, wird die Baselineleistungsstufe für diesen Datenträger auf der Grundlage der bereitgestellten Datenträgergröße festgelegt. Es kann eine höhere Leistungsstufe gewählt werden, um eine höhere Nachfrage zu befriedigen, und wenn diese Leistung nicht mehr erforderlich ist, können Sie zur ursprünglichen Baselineleistungsstufe zurückkehren. Wenn Sie z. B. einen P10-Datenträger (128 GiB) zur Verfügung stellen, wird Ihre Baselineleistungsstufe auf P10 (500 IOPS und 100 MB/s) festgelegt. Sie können die Stufe so aktualisieren, dass sie der Leistung von P50 (7500 IOPS und 250 MB/s) entspricht, ohne die Datenträgergröße zu erhöhen und zu P10 zurückkehren, wenn die höhere Leistung nicht mehr benötigt wird.

| Datenträgergröße | Baselineleistungsstufe | Kann aktualisiert werden auf |
|----------------|-----|-------------------------------------|
| 4 GiB | P1 | P2, P3, P4, P6, P10, P15, P20, P30, P40, P50 |
| 8 GiB | P2 | P3, P4, P6, P10, P15, P20, P30, P40, P50 |
| 16 GiB | P3 | P4, P6, P10, P15, P20, P30, P40, P50 | 
| 32GiB | P4 | P6, P10, P15, P20, P30, P40, P50 |
| 64GiB | P6 | P10, P15, P20, P30, P40, P50 |
| 128 GB | P10 | P15, P20, P30, P40, P50 |
| 256 GiB | P15 | P20, P30, P40, P50 |
| 512 GB | P20 | P30, P40, P50 |
| 1 TiB | P30 | P40, P50 |
| 2 TiB | P40 | P50 |
| 4 TiB | P50 | Keine |
| 8 TiB | P60 |  P70, P80 |
| 16 TiB | P70 | P80 |
| 32 TiB | P80 | Keine |

## <a name="restrictions"></a>Beschränkungen

- Wird derzeit nur für SSD Premium unterstützt.
- Datenträger müssen von einer aktiven VM getrennt werden, bevor ein Stufenwechsel vorgenommen wird.
- Die Verwendung der Leistungsstufen P60, P70 und P80 ist auf Datenträger mit 4096 GiB oder mehr beschränkt.

## <a name="regional-availability"></a>Regionale Verfügbarkeit

Die Anpassung der Leistungsstufe eines verwalteten Datenträgers ist derzeit nur für Premium-SSDs in den folgenden Regionen verfügbar:

- USA, Westen-Mitte 
- USA, Osten 2 
- Europa, Westen
- Australien, Osten 
- Australien, Südosten 
- Indien (Süden)

## <a name="createupdate-a-data-disk-with-a-tier-higher-than-the-baseline-tier"></a>Erstellen/Aktualisieren Sie einen Datenträger mit einer höheren Stufe als der Baselinestufe.

1. Erstellen Sie einen leeren Datenträger mit einer höheren Stufe als der Baselinestufe, oder aktualisieren Sie die Stufe eines Datenträgers, der höher als die Baselinestufe ist, mithilfe der Beispielvorlage [CreateUpdateDataDiskWithTier.json](https://github.com/Azure/azure-managed-disks-performance-tiers/blob/main/CreateUpdateDataDiskWithTier.json).

     ```cli
     subscriptionId=<yourSubscriptionIDHere>
     resourceGroupName=<yourResourceGroupNameHere>
     diskName=<yourDiskNameHere>
     diskSize=<yourDiskSizeHere>
     performanceTier=<yourDesiredPerformanceTier>
     region=<yourRegionHere>
    
     az login
    
     az account set --subscription $subscriptionId
    
     az group deployment create -g $resourceGroupName \
     --template-uri "https://raw.githubusercontent.com/Azure/azure-managed-disks-performance-tiers/main/CreateUpdateDataDiskWithTier.json" \
     --parameters "region=$region" "diskName=$diskName" "performanceTier=$performanceTier" "dataDiskSizeInGb=$diskSize"
     ```

1. Bestätigen Sie die Stufe des Datenträgers.

    ```cli
    az resource show -n $diskName -g $resourceGroupName --namespace Microsoft.Compute --resource-type disks --api-version 2020-06-30 --query [properties.tier] -o tsv
     ```

## <a name="createupdate-an-os-disk-with-a-tier-higher-than-the-baseline-tier"></a>Erstellen/Aktualisieren Sie einen Betriebssystemdatenträger mit einer höheren Stufe als der Baselinestufe.

1. Erstellen Sie einen Betriebssystemdatenträger aus einem Marketplace-Image, oder aktualisieren Sie den Datenträger eines Betriebssystemdatenträgers, der höher als die Baselinestufe ist, mithilfe der Beispielvorlage [CreateUpdateOSDiskWithTier.json](https://github.com/Azure/azure-managed-disks-performance-tiers/blob/main/CreateUpdateOSDiskWithTier.json).

     ```cli
     resourceGroupName=<yourResourceGroupNameHere>
     diskName=<yourDiskNameHere>
     performanceTier=<yourDesiredPerformanceTier>
     region=<yourRegionHere>
    
     az group deployment create -g $resourceGroupName \
     --template-uri "https://raw.githubusercontent.com/Azure/azure-managed-disks-performance-tiers/main/CreateUpdateOSDiskWithTier.json" \
     --parameters "region=$region" "diskName=$diskName" "performanceTier=$performanceTier"
     ```
 
 1. Bestätigen Sie die Stufe des Datenträgers.
 
     ```cli
     az resource show -n $diskName -g $resourceGroupName --namespace Microsoft.Compute --resource-type disks --api-version 2020-06-30 --query [properties.tier] -o tsv
     ```

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie die Größe eines Datenträgers ändern müssen, um die Vorteile der größeren Leistungsstufen zu nutzen, lesen Sie unsere Artikel zu diesem Thema:

- [Erweitern von virtuellen Festplatten auf virtuellen Linux-Computern mit der Azure-CLI](linux/expand-disks.md)
- [Erweitern eines verwalteten Datenträgers, der an einen virtuellen Windows-Computer angefügt ist](windows/expand-os-disk.md)