---
title: include file
description: include file
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 04/15/2021
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 4435784592954e4df44d5ea8dc05b33a0023fd43
ms.sourcegitcommit: fc9fd6e72297de6e87c9cf0d58edd632a8fb2552
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/30/2021
ms.locfileid: "108313469"
---
- Unterstützt keine Ultra Disks.
- Kann nicht aktiviert werden, wenn Azure Disk Encryption (Verschlüsselung der Gast-VM mit BitLocker/VM-Decrypt) in Ihren VMs oder VM-Skalierungsgruppen aktiviert ist.
- Azure Disk Encryption kann nicht auf Datenträgern aktiviert werden, auf denen die Verschlüsselung auf dem Host aktiviert ist.
- Die Verschlüsselung kann für vorhandene VM-Skalierungsgruppen aktiviert werden. Es werden jedoch nur neue VMs automatisch verschlüsselt, die nach dem Aktivieren der Verschlüsselung erstellt wurden.
- Bei vorhandenen VMs muss deren Zuordnung aufgehoben und sie müssen erneut zugeordnet werden, damit sie verschlüsselt werden können.
- Unterstützt kurzlebige Betriebssystemdatenträger, aber nur mit plattformseitig verwalteten Schlüsseln.