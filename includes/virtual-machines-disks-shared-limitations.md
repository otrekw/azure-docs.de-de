---
title: include file
description: include file
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 07/14/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: d2cf7dbcd97c8f740447607eaf443bc3ea4a6733
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/20/2020
ms.locfileid: "86500593"
---
Die Aktivierung freigegebener Datenträger ist nur für eine Teilmenge von Datenträgertypen verfügbar. Derzeit können nur Disk Ultra und SSD Premium freigegebene Datenträger ermöglichen. Jeder verwaltete Datenträger, für den freigegebene Datenträger aktiviert sind, unterliegt den folgenden Einschränkungen, die nach Datenträgertyp geordnet sind:

### <a name="ultra-disks"></a>Ultra-Datenträger

Disk Ultra-Datenträger verfügen über eine eigene separate Liste von Einschränkungen, die nicht mit freigegebenen Datenträgern zusammenhängen. Informationen zu den Einschränkungen für Disk Ultra-Datenträger finden Sie unter [Verwendung von Azure Ultra-Datenträgern](../articles/virtual-machines/linux/disks-enable-ultra-ssd.md).

Bei der Freigabe von Disk Ultra-Datenträgern gelten die folgenden zusätzlichen Einschränkungen:

- Derzeit beschränkt auf Azure Resource Manager oder die SDK-Unterstützung. 
- Nur Basisdatenträger können mit einigen Versionen des Windows Server-Failoverclusters verwendet werden. Weitere Informationen finden Sie unter [Hardwareanforderungen und Speicheroptionen für Failoverclustering](https://docs.microsoft.com/windows-server/failover-clustering/clustering-requirements).

### <a name="premium-ssds"></a>SSD Premium

- Zurzeit nur in der Region „USA, Westen-Mitte“ unterstützt.
- Derzeit beschränkt auf Azure Resource Manager oder die SDK-Unterstützung. 
- Die Aktivierung kann nicht auf Betriebssystem-Datenträgern, sondern nur auf sonstigen Datenträgern erfolgen.
- Die **schreibgeschützte** Hostzwischenspeicherung ist für SSD-Premium-Datenträger mit `maxShares>1` nicht verfügbar.
- Datenträgerbursting ist für SSD-Premium-Datenträger mit `maxShares>1` nicht verfügbar.
- Wenn Sie Verfügbarkeitsgruppen und VM-Skalierungsgruppen mit freigegebenen Azure-Datenträgern verwenden, wird die [Speicherfehlerdomänenausrichtung](https://docs.microsoft.com/azure/virtual-machines/windows/manage-availability#use-managed-disks-for-vms-in-an-availability-set) mit der Fehlerdomäne des virtuellen Computers für den freigegebenen Datenträger nicht erzwungen.
- Bei der Verwendung von [Näherungsplatzierungsgruppen (PPGs)](../articles/virtual-machines/windows/proximity-placement-groups.md) müssen alle virtuellen Computer, die einen Datenträger freigeben, zur selben Näherungsplatzierungsgruppe gehören.
- Nur Basisdatenträger können mit einigen Versionen des Windows Server-Failoverclusters verwendet werden. Weitere Informationen finden Sie unter [Hardwareanforderungen und Speicheroptionen für Failoverclustering](https://docs.microsoft.com/windows-server/failover-clustering/clustering-requirements).
- Die Unterstützung von Azure Backup und Azure Site Recovery ist noch nicht verfügbar.

Wenn Sie daran interessiert sind, freigegebene Datenträger auszuprobieren, [registrieren Sie sich für den Zugriff](https://aka.ms/AzureSharedDiskGASignUp).