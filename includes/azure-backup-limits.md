---
title: include file
description: include file
services: backup
author: dcurwin
ms.service: backup
ms.topic: include
ms.date: 12/07/2018
ms.author: dacurwin
ms.custom: include file
ms.openlocfilehash: ca6c57b139b195b94dc7171336cfcc34d18b1872
ms.sourcegitcommit: 5aefc96fd34c141275af31874700edbb829436bb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/04/2019
ms.locfileid: "74828972"
---
Für Azure Backup gelten die folgenden Beschränkungen.

| **Begrenzung** | **Standard** |
| --- | --- |
| Server oder Computer, die in einem Tresor registriert werden können. | Windows Server/Windows Client/System Center Data Protection Manager: 50 verwendet. <br/><br/> IaaS-VMs: 1.000.  |
| Die Größe einer Datenquelle im Tresorspeicher. |54.400GB Maximum. Die Beschränkung gilt nicht für IaaS-VM-Sicherungen. |
| Sicherungstresore in einem Azure-Abonnement. |500 Tresore pro Region. |
| Planen täglicher Sicherungen. |Windows Server/Client: Drei pro Tag.<br/> System Center DPM Zwei pro Tag. <br/> IaaS-VMs: Eine pro Tag.  |
| Datenträger, die für die Sicherung an einen virtuellen Azure-Computer angeschlossen sind. | 16 |
| Einzelner Datenträger, der für die Sicherung an einen virtuellen Azure-Computer angeschlossen ist.| 32 TB|
