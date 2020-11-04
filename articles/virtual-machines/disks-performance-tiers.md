---
title: Ändern der Leistung verwalteter Azure-Datenträger
description: Erfahren Sie mehr über Leistungsstufen für verwaltete Datenträger sowie über das Ändern der Leistungsstufen für vorhandene verwaltete Datenträger.
author: roygara
ms.service: virtual-machines
ms.topic: how-to
ms.date: 09/24/2020
ms.author: rogarana
ms.subservice: disks
ms.custom: references_regions
ms.openlocfilehash: 404f435e321e53694807a627121d84f6cbf6724d
ms.sourcegitcommit: 28c5fdc3828316f45f7c20fc4de4b2c05a1c5548
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/22/2020
ms.locfileid: "92359678"
---
# <a name="performance-tiers-for-managed-disks-preview"></a>Leistungsstufen für verwaltete Datenträger (Vorschau)

Azure Disk Storage bietet derzeit integrierte Burstfunktionen, um eine höhere Leistung bei der Verarbeitung von kurzfristigem, unerwarteten Datenverkehr bereitzustellen. SSD Premium bietet die Flexibilität, die Datenträgerleistung zu erhöhen, ohne die tatsächliche Datenträgergröße heraufzusetzen. Diese Funktion ermöglicht es Ihnen, ihre Workloadleistungsanforderungen abzugleichen und Kosten zu senken. 

> [!NOTE]
> Diese Funktion steht derzeit als Vorschau zur Verfügung. 

Diese Funktion eignet sich ideal für Ereignisse, die vorübergehend ein konstant höheres Leistungsniveau erfordern, z. B. Feiertagseinkäufe, Leistungstests oder die Ausführung einer Trainingsumgebung. Um diese Ereignisse zu verarbeiten, können Sie eine höhere Leistungsstufe verwenden, solange Sie diese benötigen. Anschließend können Sie zur ursprünglichen Stufe zurückkehren, wenn Sie die zusätzliche Leistung nicht mehr benötigen.

## <a name="how-it-works"></a>Funktionsweise

Wenn Sie einen Datenträger zum ersten Mal bereitstellen, wird die Baselineleistungsstufe für diesen Datenträger auf der Grundlage der bereitgestellten Datenträgergröße festgelegt. Sie können eine höhere Leistungsstufe verwenden, um höheren Bedarf zu erfüllen. Wenn Sie diese Leistungsstufe nicht mehr benötigen, können Sie zur anfänglichen Baselineleistungsstufe zurückkehren.

Ihre Abrechnung ändert sich, wenn sich die Stufe ändert. Wenn Sie z. B. einen P10-Datenträger (128 GiB) zur Verfügung stellen, wird Ihre Baselineleistungsstufe auf P10 (500 IOPS und 100 MBit/s) festgelegt. Ihnen wird die P10-Rate in Rechnung gestellt. Sie können die Stufe auf die Leistung von P50 (7.500 IOPS und 250 MBit/s) aktualisieren, ohne die Größe des Datenträgers zu erhöhen. Während des Upgrades wird Ihnen die P50-Rate in Rechnung gestellt. Wenn Sie die höhere Leistung nicht mehr benötigen, können Sie zur P10-Stufe zurückkehren. Der Datenträger wird erneut mit der P10-Rate abgerechnet.

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

Informationen zur Abrechnung finden Sie unter [Verwaltete Datenträger – Preise](https://azure.microsoft.com/pricing/details/managed-disks/).

## <a name="restrictions"></a>Beschränkungen

- Dieses Feature wird derzeit nur für SSD Premium unterstützt.
- Sie müssen entweder die Zuordnung des virtuellen Computers aufheben oder den Datenträger von einem ausgeführten virtuellen Computer trennen, damit Sie die Stufe des Datenträgers ändern können.
- Die Verwendung der Leistungsstufen P60, P70 und P80 ist auf Datenträger mit 4.096 GiB oder mehr beschränkt.
- Die Leistungsstufe eines Datenträgers kann nur ein Mal alle 24 Stunden herabgestuft werden.

## <a name="regional-availability"></a>Regionale Verfügbarkeit

Die Möglichkeit zum Anpassen der Leistungsstufe eines verwalteten Datenträgers besteht zurzeit nur für SSDs Premium in folgenden Regionen: USA, Osten 2; USA, Süden-Mitte; USA, Westen-Mitte und Australien, Südosten

## <a name="create-an-empty-data-disk-with-a-tier-higher-than-the-baseline-tier"></a>Erstellen/Aktualisieren eines Datenträgers mit einer höheren Stufe als der Baselinestufe

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
     
## <a name="update-the-tier-of-a-disk"></a>Aktualisieren der Stufe eines Datenträgers

```azurecli
resourceGroupName=<yourResourceGroupNameHere>
diskName=<yourDiskNameHere>
performanceTier=<yourDesiredPerformanceTier>

az disk update -n $diskName -g $resourceGroupName --set tier=$performanceTier
```
## <a name="show-the-tier-of-a-disk"></a>Anzeigen der Stufe eines Datenträgers

```azurecli
az disk show -n $diskName -g $resourceGroupName --query [tier] -o tsv
```

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie die Größe eines Datenträgers ändern müssen, um die höheren Leistungsstufen zu nutzen, lesen Sie die folgenden Artikel:

- [Erweitern von virtuellen Festplatten auf virtuellen Linux-Computern mit der Azure-CLI](linux/expand-disks.md)
- [Erweitern eines verwalteten Datenträgers, der an einen virtuellen Windows-Computer angefügt ist](windows/expand-os-disk.md)
