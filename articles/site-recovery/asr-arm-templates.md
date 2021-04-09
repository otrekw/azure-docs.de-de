---
title: Azure Resource Manager-Vorlagen
description: Azure Resource Manager-Vorlagen für die Verwendung von Azure Site Recovery-Funktionen
services: site-recovery
author: rishjai
manager: gaggupta
ms.service: site-recovery
ms.topic: article
ms.date: 02/04/2021
ms.author: rishjai
ms.openlocfilehash: efd5c1ab3e23348373be76c79d1b9689bc5f4941
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "101720527"
---
# <a name="azure-resource-manager-templates-for-azure-site-recovery"></a>Azure Resource Manager-Vorlagen für Azure Site Recovery

Die folgende Tabelle enthält Links zu Azure Resource Manager-Vorlagen für die Verwendung von Azure Site Recovery-Funktionen.

| Vorlage | BESCHREIBUNG |
|---|---|
|**Azure zu Azure** | |
| [Erstellen eines Recovery Services-Tresors](./quickstart-create-vault-template.md)| Erstellen Sie einen Recovery Services-Tresor. Der Tresor kann für Azure Backup und für Azure Site Recovery verwendet werden. |
| [Aktivieren der Replikation für Azure-VMs](https://aka.ms/asr-arm-enable-replication) | Aktivieren Sie die Replikation für Azure-VMs mithilfe des vorhandenen Tresors und benutzerdefinierter Einstellungen für das Ziel.|
| [Auslösen eines Failovers und erneutes Schützen](https://aka.ms/asr-arm-failover-reprotect) | Lösen Sie ein Failover und den Vorgang für erneuten Schutz für eine Gruppe von Azure-VMs aus. |
| [Ausführen eines umfassenden Notfallwiederherstellungsflows für Azure-VMs](https://aka.ms/asr-arm-e2e-flow) | Starten Sie einen vollständigen Notfallwiederherstellungsflow (Aktivieren der Replikation + Failover und erneutes Schützen + Failback und erneutes Schützen) für Azure-VMs, der auch als 540°-Flow bezeichnet wird.|
|   |   |