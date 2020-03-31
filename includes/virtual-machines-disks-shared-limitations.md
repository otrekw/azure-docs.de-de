---
title: include file
description: include file
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 02/18/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 77fc6070010791bf96c944114929eba95842c9d4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "77471688"
---
In der Vorschau unterliegen verwaltete Datenträger, für die freigegebene Datenträger aktiviert sind, den folgenden Einschränkungen:

- Zurzeit nur mit SSD-Premium-Datenträgern verfügbar.
- Zurzeit nur in der Region „USA, Westen-Mitte“ unterstützt.
- Alle virtuellen Computer, die einen Datenträger gemeinsam nutzen, müssen in denselben [Näherungsplatzierungsgruppen](../articles/virtual-machines/windows/proximity-placement-groups.md) bereitgestellt werden.
- Die Aktivierung kann nicht auf Betriebssystem-Datenträgern, sondern nur auf sonstigen Datenträgern erfolgen.
- Nur Basisdatenträger können mit einigen Versionen des Windows Server-Failoverclusters verwendet werden. Weitere Informationen finden Sie unter [Hardwareanforderungen und Speicheroptionen für Failoverclustering](https://docs.microsoft.com/windows-server/failover-clustering/clustering-requirements).
- Die schreibgeschützte Hostzwischenspeicherung ist für SSD-Premium-Datenträger mit `maxShares>1` nicht verfügbar.
- Verfügbarkeitsgruppen und VM-Skalierungsgruppen können nur verwendet werden, wenn `FaultDomainCount` auf 1 festgelegt ist.
- Die Unterstützung von Azure Backup und Azure Site Recovery ist noch nicht verfügbar.

Wenn Sie daran interessiert sind, freigegebene Datenträger auszuprobieren, [registrieren Sie sich für unsere Vorschau](https://aka.ms/AzureSharedDiskPreviewSignUp).
