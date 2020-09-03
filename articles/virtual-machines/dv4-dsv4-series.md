---
title: 'Dv4- und Dsv4-Serie: Azure-VMs'
description: Hier finden Sie Spezifikationen für die virtuellen Computer der Dv4- und Dsv4-Serie.
author: brbell
ms.author: brbell
ms.reviewer: cynthn
ms.custom: mimckitt
ms.service: virtual-machines
ms.subservice: sizes
ms.topic: conceptual
ms.date: 06/08/2020
ms.openlocfilehash: df78d11ce19c1d1fbbb04d2cd1718be74401cd83
ms.sourcegitcommit: 3fb5e772f8f4068cc6d91d9cde253065a7f265d6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89182155"
---
# <a name="dv4-and-dsv4-series"></a>Dv4- und Dsv4-Serie

Die Dv4- und Dsv4-Serie läuft auf Intel-Prozessoren des Typs Intel&reg; Xeon&reg; Platinum 8272CL (Cascade Lake) mit einer Hyperthreading-Konfiguration ausgeführt, die für die meisten Allzweck-Workloads ein besseres Preis-Leistungs-Verhältnis bieten. Sie verfügt über eine dauerhafte Turbotaktrate von 3,4 GHz. 

> [!NOTE]
> Häufig gestellte Fragen finden Sie unter [Azure-VM-Größen ohne lokale temporäre Datenträger](azure-vms-no-temp-disk.md).
## <a name="dv4-series"></a>Dv4-Serie

Die Größen der Dv4-Serie laufen auf dem Intel&reg; Xeon&reg; Platinum 8272CL (Cascade Lake). Die Größen der Dv4-Serie bieten eine Kombination aus vCPU-, Arbeitsspeicher- und Remotespeicheroptionen, die für die meisten Produktionsworkloads geeignet ist. Virtuelle Computer der Dv4-Serie verfügen über Hyperthreading-Technologie von [Intel&reg;](https://www.intel.com/content/www/us/en/architecture-and-technology/hyper-threading/hyper-threading-technology.html).

Speicher für Remotedatenträger wird separat zu virtuellen Computern abgerechnet. Verwenden Sie die Dsv4-Größen, um Datenträger mit Premium-Speicher zu nutzen. Für Dsv4-Größen gelten die gleichen Preise und Verbrauchseinheiten für die Abrechnung wie bei der Dv4-Serie.

ACU: 195-210

Storage Premium  Nicht unterstützt

Storage Premium-Zwischenspeicherung:  Nicht unterstützt

Livemigration: Unterstützt

Updates mit Speicherbeibehaltung: Unterstützt

| Size | vCPU | Memory: GiB | Temporärer Speicher (SSD): GiB | Max. Anzahl Datenträger | Maximale Anzahl NICs|Erwartete Netzwerkbandbreite (MBit/s) |
|---|---|---|---|---|---|---|
| Standard_D2_v4 | 2 | 8 | Nur Remotespeicher | 4 | 2|1000 |
| Standard_D4_v4 | 4 | 16  | Nur Remotespeicher | 8 | 2|2000 |
| Standard_D8_v4 | 8 | 32 | Nur Remotespeicher | 16 | 4|4000 |
| Standard_D16_v4 | 16 | 64 | Nur Remotespeicher | 32 | 8|8.000 |
| Standard_D32_v4 | 32 | 128 | Nur Remotespeicher | 32 | 8|16000 |
| Standard_D48_v4 | 48 | 192 | Nur Remotespeicher | 32 | 8|24.000 |
| Standard_D64_v4 | 64 | 256 | Nur Remotespeicher | 32 | 8|30.000 |

## <a name="dsv4-series"></a>Dsv4-Serie

Die Größen der Dsv4-Serie laufen auf dem Intel&reg; Xeon&reg; Platinum 8272CL (Cascade Lake). Die Größen der Dv4-Serie bieten eine Kombination aus vCPU-, Arbeitsspeicher- und Remotespeicheroptionen, die für die meisten Produktionsworkloads geeignet ist. Virtuelle Computer der Dsv4-Serie verfügen über Hyperthreading-Technologie von [Intel&reg;](https://www.intel.com/content/www/us/en/architecture-and-technology/hyper-threading/hyper-threading-technology.html). Speicher für Remotedatenträger wird separat zu virtuellen Computern abgerechnet.

ACU: 195-210

Storage Premium  Unterstützt

Storage Premium-Zwischenspeicherung:  Unterstützt

Livemigration: Unterstützt

Updates mit Speicherbeibehaltung: Unterstützt

| Size | vCPU | Memory: GiB | Temporärer Speicher (SSD): GiB | Max. Anzahl Datenträger | Maximaler Durchsatz des Datenträgers ohne Cache: IOPS/MBit/s | Maximale Anzahl NICs|Erwartete Netzwerkbandbreite (MBit/s) |
|---|---|---|---|---|---|---|---|
| Standard_D2s_v4 | 2 | 8  | Nur Remotespeicher | 4 | 3200/48 | 2|1000 |
| Standard_D4s_v4 | 4 | 16 | Nur Remotespeicher | 8 | 6400/96 | 2|2000 |
| Standard_D8s_v4 | 8 | 32 | Nur Remotespeicher | 16 | 12800/192 | 4|4000 |
| Standard_D16s_v4 | 16 | 64  | Nur Remotespeicher | 32 | 25600/384 | 8|8.000 |
| Standard_D32s_v4 | 32 | 128 | Nur Remotespeicher | 32 | 51200/768 | 8|16000 |
| Standard_D48s_v4 | 48 | 192 | Nur Remotespeicher | 32 | 76800/1152 | 8|24.000 |
| Standard_D64s_v4 | 64 | 256 | Nur Remotespeicher | 32 | 80000/1200 | 8|30.000 |
