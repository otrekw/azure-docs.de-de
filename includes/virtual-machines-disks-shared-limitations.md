---
title: Datei einfügen
description: include file
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 06/11/2021
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 19b9cfc6ad6467b2779abb3561899fd3bd8d037e
ms.sourcegitcommit: 942a1c6df387438acbeb6d8ca50a831847ecc6dc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/11/2021
ms.locfileid: "112040266"
---
Die Aktivierung freigegebener Datenträger ist nur für eine Teilmenge von Datenträgertypen verfügbar. Derzeit können nur Disk Ultra und SSD Premium freigegebene Datenträger ermöglichen. Jeder verwaltete Datenträger, für den freigegebene Datenträger aktiviert sind, unterliegt den folgenden Einschränkungen, die nach Datenträgertyp geordnet sind:

### <a name="ultra-disks"></a>Ultra-Datenträger

Disk Ultra-Datenträger verfügen über eine eigene separate Liste von Einschränkungen, die nicht mit freigegebenen Datenträgern zusammenhängen. Informationen zu den Einschränkungen für Disk Ultra-Datenträger finden Sie unter [Verwendung von Azure Ultra-Datenträgern](../articles/virtual-machines/disks-enable-ultra-ssd.md).

Bei der Freigabe von Disk Ultra-Datenträgern gelten die folgenden zusätzlichen Einschränkungen:

- Derzeit beschränkt auf Azure Resource Manager oder die SDK-Unterstützung. 
- Nur Basisdatenträger können mit einigen Versionen des Windows Server-Failoverclusters verwendet werden. Weitere Informationen finden Sie unter [Hardwareanforderungen und Speicheroptionen für Failoverclustering](/windows-server/failover-clustering/clustering-requirements).
- Nur [serverseitige Verschlüsselung](../articles/virtual-machines/disk-encryption.md) wird unterstützt, [Azure Disk Encryption](../articles/virtual-machines/windows/disk-encryption-overview.md) wird aktuell nicht unterstützt.

Freigegebene Disk Ultra-Datenträger sind in allen Regionen verfügbar, die diese Datenträger standardmäßig unterstützen. Sie müssen sich nicht für den Zugriff registrieren, um diese zu verwenden.

### <a name="premium-ssds"></a>SSD Premium

- Derzeit beschränkt auf Azure Resource Manager oder die SDK-Unterstützung. 
- Die Aktivierung kann nicht auf Betriebssystem-Datenträgern, sondern nur auf sonstigen Datenträgern erfolgen.
- Die **schreibgeschützte** Hostzwischenspeicherung ist für SSD-Premium-Datenträger mit `maxShares>1` nicht verfügbar.
- Datenträgerbursting ist für SSD-Premium-Datenträger mit `maxShares>1` nicht verfügbar.
- Wenn Sie Verfügbarkeitsgruppen und VM-Skalierungsgruppen mit freigegebenen Azure-Datenträgern verwenden, wird die [Speicherfehlerdomänenausrichtung](../articles/virtual-machines/availability.md) mit der Fehlerdomäne des virtuellen Computers für den freigegebenen Datenträger nicht erzwungen.
- Bei der Verwendung von [Näherungsplatzierungsgruppen (PPGs)](../articles/virtual-machines/windows/proximity-placement-groups.md) müssen alle virtuellen Computer, die einen Datenträger freigeben, zur selben Näherungsplatzierungsgruppe gehören.
- Nur Basisdatenträger können mit einigen Versionen des Windows Server-Failoverclusters verwendet werden. Weitere Informationen finden Sie unter [Hardwareanforderungen und Speicheroptionen für Failoverclustering](/windows-server/failover-clustering/clustering-requirements).
- Die Unterstützung für Azure Site Recovery ist noch nicht verfügbar.
- Azure Backup ist über [Azure Disk Backup](../articles/backup/disk-backup-overview.md) verfügbar.
- Nur [serverseitige Verschlüsselung](../articles/virtual-machines/disk-encryption.md) wird unterstützt, [Azure Disk Encryption](../articles/virtual-machines/windows/disk-encryption-overview.md) wird aktuell nicht unterstützt.

#### <a name="regional-availability"></a>Regionale Verfügbarkeit

Freigegebene SSD Premium-Datenträger sind in allen Regionen verfügbar, in denen verwaltete Datenträger verfügbar sind.
