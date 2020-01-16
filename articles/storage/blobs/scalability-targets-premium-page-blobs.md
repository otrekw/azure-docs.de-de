---
title: Skalierbarkeitsziele für Seitenblob-Speicherkonten mit Premium-Leistung
titleSuffix: Azure Storage
description: Ein Seitenblob-Speicherkonto mit Premium-Leistung ist für Lese-/Schreibvorgänge optimiert. Dieser Speicherkontotyp sichert einen nicht verwalteten Datenträger für einen virtuellen Azure-Computer.
services: storage
author: tamram
ms.service: storage
ms.topic: conceptual
ms.date: 12/18/2019
ms.author: tamram
ms.subservice: common
ms.openlocfilehash: 1542204a2ac779a711407ab1a36c6f8ab9669510
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2019
ms.locfileid: "75476076"
---
# <a name="scalability-and-performance-targets-for-premium-page-blob-storage-accounts"></a>Skalierungs- und Leistungsziele für Seitenblob-Speicherkonten mit Premium-Leistung

[!INCLUDE [storage-scalability-intro-include](../../../includes/storage-scalability-intro-include.md)]

## <a name="scale-targets-for-premium-page-blob-accounts"></a>Skalierungsziele für Seitenblobkonten mit Premium-Leistung

Ein Seitenblob-Speicherkonto mit Premium-Leistung ist für Lese-/Schreibvorgänge optimiert. Dieser Speicherkontotyp sichert einen nicht verwalteten Datenträger für einen virtuellen Azure-Computer.

> [!NOTE]
> Microsoft empfiehlt, nach Möglichkeit verwaltete Datenträger für virtuelle Azure-Computer (VMs) zu verwenden. Weitere Informationen zu verwalteten Datenträgern finden Sie in der [Übersicht über Azure Disk Storage für Windows-VMs](../../virtual-machines/windows/managed-disks-overview.md).

Seitenblob-Speicherkonten mit Premium-Leistung weisen folgende Skalierbarkeitsziele auf:

| Gesamtkapazität des Kontos                            | Gesamtbandbreite für ein lokal redundantes Speicherkonto                     |
| ------------------------------------------------- | --------------------------------------------------------------------------- |
| Datenträgerkapazität: 35 TB <br>Kapazität für Momentaufnahmen: 10 TB | Bis zu 50 Gigabit pro Sekunde für eingehenden<sup>1</sup> und ausgehenden<sup>2</sup> Datenverkehr |

<sup>1</sup> Alle Daten (Anforderungen), die an ein Speicherkonto gesendet werden

<sup>2</sup> Alle Daten (Antworten), die von einem Speicherkonto empfangen werden

Ein Seitenblobkonto mit Premium-Leistung ist ein universelles Konto, das für Premium-Leistung konfiguriert ist. Es werden Speicherkonten vom Typ „Universell v2“ empfohlen.

Wenn Sie Seitenblob-Speicherkonten mit Premium-Leistung für nicht verwaltete Datenträger verwenden und Ihre Anwendung die Skalierbarkeitsziele eines einzelnen Speicherkontos überschreitet, empfiehlt Microsoft die Migration zu verwalteten Datenträgern. Weitere Informationen zu verwalteten Datenträgern finden Sie in der [Übersicht über Azure Disk Storage für Windows-VMs](../../virtual-machines/windows/managed-disks-overview.md) und der [Übersicht über Azure Disk Storage für Linux-VMs](../../virtual-machines/linux/managed-disks-overview.md).

Falls Sie keine Migration zu verwalteten Datenträgern durchführen möchten, sollten Sie Ihre Anwendung für die Verwendung von mehreren Speicherkonten erstellen und die Daten über diese Speicherkonten hinweg partitionieren. Wenn Sie beispielsweise 51-TB-Datenträger an mehrere VMs anfügen möchten, können Sie sie auf zwei Speicherkonten verteilen. 35 TB ist der Grenzwert pro Storage Premium-Konto. Stellen Sie sicher, dass ein Speicherkonto mit Premium-Leistung stets Datenträger mit einer Größe von maximal 35 TB enthält.

## <a name="see-also"></a>Weitere Informationen

- [Skalierbarkeits- und Leistungsziele für Storage Standard-Konten](../common/scalability-targets-standard-account.md)
- [Skalierbarkeitsziele für Blockblob-Speicherkonten mit Premium-Leistung](../blobs/scalability-targets-premium-block-blobs.md)
