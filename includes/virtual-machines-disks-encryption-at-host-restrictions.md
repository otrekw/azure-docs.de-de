---
title: include file
description: include file
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 07/10/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 7700b1118c4f04643607f44a474338ffdd5c09e0
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "86230988"
---
- Unterstützt keine Ultra Disks.
- Kann nicht aktiviert werden, wenn Azure Disk Encryption (Verschlüsselung der Gast-VM mit BitLocker/VM-Decrypt) in Ihren VMs oder VM-Skalierungsgruppen aktiviert ist.
- Azure Disk Encryption kann nicht auf Datenträgern aktiviert werden, auf denen die Verschlüsselung auf dem Host aktiviert ist.
- Die Verschlüsselung kann für vorhandene VM-Skalierungsgruppen aktiviert werden. Es werden jedoch nur neue VMs automatisch verschlüsselt, die nach dem Aktivieren der Verschlüsselung erstellt wurden.
- Bei vorhandenen VMs muss deren Zuordnung aufgehoben und sie müssen erneut zugeordnet werden, damit sie verschlüsselt werden können.