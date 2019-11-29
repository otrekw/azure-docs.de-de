---
title: Eingestellte Unterstützung des Azure Site Recovery-Features zur Datenverschlüsselung | Microsoft-Dokumentation
description: Details zum Azure Site Recovery-Feature zur Datenverschlüsselung
services: site-recovery
author: rajani-janaki-ram
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 11/15/2019
ms.author: rajanaki
ms.openlocfilehash: 5e74466891a5926d8ae8feb3c1c48348ecf3cfe6
ms.sourcegitcommit: 2d3740e2670ff193f3e031c1e22dcd9e072d3ad9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/16/2019
ms.locfileid: "74134398"
---
# <a name="deprecation-of-site-recovery-data-encryption-feature"></a>Eingestellte Unterstützung des Azure Site Recovery-Features zur Datenverschlüsselung

In diesem Dokument werden die Details zur eingestellten Unterstützung und die Korrekturmaßnahme beschrieben, die Sie ergreifen müssen, wenn Sie das Site Recovery-Feature zur Datenverschlüsselung bei der Konfiguration der Notfallwiederherstellung von virtuellen Hyper-V-Computern für Azure verwenden. 

## <a name="deprecation-information"></a>Informationen zur eingestellten Unterstützung


Das Site Recovery-Feature zur Datenverschlüsselung stand Kunden zum Schutz von virtuellen Hyper-V-Computern zur Verfügung, um sicherzustellen, dass die replizierten Daten vor Sicherheitsbedrohungen geschützt sind. Dieses Feature wird am **30. Dezember 2019** eingestellt. Sie wird durch das erweiterte Feature [Verschlüsselung ruhender Daten](https://azure.microsoft.com/blog/azure-site-recovery-encryption-at-rest/) ersetzt, das die [Speicherdienstverschlüsselung](https://docs.microsoft.com/azure/storage/common/storage-service-encryption) (SSE) verwendet. Mit SSE werden die Daten vor der dauerhaften Speicherung verschlüsselt und bei der Wiederherstellung entschlüsselt. Zudem werden Ihre virtuellen Computer beim Failover zu Azure über die verschlüsselten Speicherkonten ausgeführt, was eine verbesserte RTO (Recovery Time Objective) ermöglicht.

Beachten Sie, dass Sie als bestehender Kunde, der dieses Feature nutzt, Informationen mit den Details zur Einstellung und den Schritten zur Korrektur erhalten würden. 


## <a name="what-are-the-implications"></a>Was sind die Folgen?

Nach dem **30. Dezember 2019** dürfen virtuelle Computer, die noch das abgelaufene Verschlüsselungsfeature verwenden, keinen Failover durchführen. 

## <a name="required-action"></a>Erforderliche Maßnahme
Gehen Sie wie folgt vor, um erfolgreiche Failovervorgänge und Replikationen fortzusetzen:

Führen Sie die folgenden Schritte für jeden virtuellen Computer aus: 
1.  [Deaktivieren Sie die Replikation](https://docs.microsoft.com/azure/site-recovery/site-recovery-manage-registration-and-protection#disable-protection-for-a-hyper-v-virtual-machine-replicating-to-azure-using-the-system-center-vmm-to-azure-scenario).
2.  [Erstellen Sie eine neue Replikationsrichtlinie](https://docs.microsoft.com/azure/site-recovery/hyper-v-azure-tutorial#set-up-a-replication-policy).
3.  [Aktivieren Sie die Replikation](https://docs.microsoft.com/azure/site-recovery/hyper-v-vmm-azure-tutorial#enable-replication), und wählen Sie ein Speicherkonto mit aktivierter SSE (Speicherdienstverschlüsselung) aus.

Nach Abschluss der anfänglichen Replikation für Speicherkonten mit aktivierter SSE verwenden Ihre virtuellen Computer die Verschlüsselung ruhender Daten mit Azure Site Recovery.


## <a name="next-steps"></a>Nächste Schritte
Planen Sie die Durchführung der Korrekturschritte, und führen Sie sie möglichst früh durch. Falls Sie Fragen zur eingestellten Unterstützung haben, wenden Sie sich bitte an den Microsoft-Support. Weitere Informationen zum Szenario „Hyper-V in Azure“ finden Sie [hier](hyper-v-vmm-architecture.md).

