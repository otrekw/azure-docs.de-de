---
title: include file
description: include file
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 08/26/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: edb5836ace6f7a1a0cff703b9921f6e6999de578
ms.sourcegitcommit: 62e1884457b64fd798da8ada59dbf623ef27fe97
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/26/2020
ms.locfileid: "88944450"
---
Die Aktivierung freigegebener Datenträger ist nur für eine Teilmenge von Datenträgertypen verfügbar. Derzeit können nur Disk Ultra und SSD Premium freigegebene Datenträger ermöglichen. Jeder verwaltete Datenträger, für den freigegebene Datenträger aktiviert sind, unterliegt den folgenden Einschränkungen, die nach Datenträgertyp geordnet sind:

### <a name="ultra-disks"></a>Ultra-Datenträger

Disk Ultra-Datenträger verfügen über eine eigene separate Liste von Einschränkungen, die nicht mit freigegebenen Datenträgern zusammenhängen. Informationen zu den Einschränkungen für Disk Ultra-Datenträger finden Sie unter [Verwendung von Azure Ultra-Datenträgern](../articles/virtual-machines/disks-enable-ultra-ssd.md).

Bei der Freigabe von Disk Ultra-Datenträgern gelten die folgenden zusätzlichen Einschränkungen:

- Derzeit beschränkt auf Azure Resource Manager oder die SDK-Unterstützung. 
- Nur Basisdatenträger können mit einigen Versionen des Windows Server-Failoverclusters verwendet werden. Weitere Informationen finden Sie unter [Hardwareanforderungen und Speicheroptionen für Failoverclustering](https://docs.microsoft.com/windows-server/failover-clustering/clustering-requirements).

Freigegebene Disk Ultra-Datenträger sind in allen Regionen verfügbar, die diese Datenträger standardmäßig unterstützen. Sie müssen sich nicht für den Zugriff registrieren, um diese zu verwenden.

### <a name="premium-ssds"></a>SSD Premium

- Derzeit nur in [einer Teilmenge der Regionen](#regional-availability) unterstützt.
- Derzeit beschränkt auf Azure Resource Manager oder die SDK-Unterstützung. 
- Die Aktivierung kann nicht auf Betriebssystem-Datenträgern, sondern nur auf sonstigen Datenträgern erfolgen.
- Die **schreibgeschützte** Hostzwischenspeicherung ist für SSD-Premium-Datenträger mit `maxShares>1` nicht verfügbar.
- Datenträgerbursting ist für SSD-Premium-Datenträger mit `maxShares>1` nicht verfügbar.
- Wenn Sie Verfügbarkeitsgruppen und VM-Skalierungsgruppen mit freigegebenen Azure-Datenträgern verwenden, wird die [Speicherfehlerdomänenausrichtung](https://docs.microsoft.com/azure/virtual-machines/windows/manage-availability#use-managed-disks-for-vms-in-an-availability-set) mit der Fehlerdomäne des virtuellen Computers für den freigegebenen Datenträger nicht erzwungen.
- Bei der Verwendung von [Näherungsplatzierungsgruppen (PPGs)](../articles/virtual-machines/windows/proximity-placement-groups.md) müssen alle virtuellen Computer, die einen Datenträger freigeben, zur selben Näherungsplatzierungsgruppe gehören.
- Nur Basisdatenträger können mit einigen Versionen des Windows Server-Failoverclusters verwendet werden. Weitere Informationen finden Sie unter [Hardwareanforderungen und Speicheroptionen für Failoverclustering](https://docs.microsoft.com/windows-server/failover-clustering/clustering-requirements).
- Die Unterstützung von Azure Backup und Azure Site Recovery ist noch nicht verfügbar.

#### <a name="regional-availability"></a>Regionale Verfügbarkeit

Freigegebene Premium-SSDs werden nur in den folgenden Regionen unterstützt:

- East US
- USA (Ost) 2
- USA (Westen)
- USA, Westen 2
- USA, Westen-Mitte
- USA Süd Mitte
- USA Nord Mitte
- USA (Mitte)
- Europa, Westen
- Nordeuropa
- Korea, Mitte
- Kanada, Mitte
- Kanada, Osten
- Japan, Osten
- Japan, Westen
- US Government, Virginia
- US Gov Arizona

Wenn Sie daran interessiert sind, freigegebene SSD Premium-Datenträger auszuprobieren, [registrieren Sie sich für den Zugriff](https://aka.ms/AzureSharedDiskGASignUp).