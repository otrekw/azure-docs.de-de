---
title: Eingestellte Unterstützung des Azure Site Recovery-Features zur Datenverschlüsselung | Microsoft-Dokumentation
description: Details zum Azure Site Recovery-Feature zur Datenverschlüsselung
services: site-recovery
author: Sharmistha-Rai
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 11/15/2019
ms.author: sharrai
ms.openlocfilehash: 5860928d71c0e7431190908d5df5d7496e8ffb17
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "89426315"
---
# <a name="deprecation-of-site-recovery-data-encryption-feature"></a>Eingestellte Unterstützung des Azure Site Recovery-Features zur Datenverschlüsselung

In diesem Dokument werden die Details zur eingestellten Unterstützung und die Korrekturmaßnahme beschrieben, die Sie ergreifen müssen, wenn Sie das Site Recovery-Feature zur Datenverschlüsselung bei der Konfiguration der Notfallwiederherstellung von virtuellen Hyper-V-Computern für Azure verwenden. 

## <a name="deprecation-information"></a>Informationen zur eingestellten Unterstützung


Das Site Recovery-Feature zur Datenverschlüsselung stand Kunden zum Schutz von virtuellen Hyper-V-Computern zur Verfügung, um sicherzustellen, dass die replizierten Daten vor Sicherheitsbedrohungen geschützt sind. Dieses Feature wird am **30. April 2022** eingestellt. Sie wird durch das erweiterte Feature [Verschlüsselung ruhender Daten](https://azure.microsoft.com/blog/azure-site-recovery-encryption-at-rest/) ersetzt, das die [Speicherdienstverschlüsselung](../storage/common/storage-service-encryption.md) (SSE) verwendet. Mit SSE werden die Daten vor der dauerhaften Speicherung verschlüsselt und bei der Wiederherstellung entschlüsselt. Zudem werden Ihre virtuellen Computer beim Failover zu Azure über die verschlüsselten Speicherkonten ausgeführt, was eine verbesserte RTO (Recovery Time Objective) ermöglicht.

Beachten Sie, dass Sie als bestehender Kunde, der dieses Feature nutzt, Informationen mit den Details zur Einstellung und den Schritten zur Korrektur erhalten würden. 


## <a name="what-are-the-implications"></a>Was sind die Folgen?

Nach dem **30. April 2022** dürfen virtuelle Computer, die noch das außer Betrieb genommene Verschlüsselungsfeature verwenden, kein Failover mehr durchführen. 

## <a name="required-action"></a>Erforderliche Maßnahme
Gehen Sie wie folgt vor, um erfolgreiche Failovervorgänge und Replikationen fortzusetzen:

Führen Sie die folgenden Schritte für jeden virtuellen Computer aus: 
1.  [Deaktivieren Sie die Replikation](./site-recovery-manage-registration-and-protection.md#disable-protection-for-a-hyper-v-virtual-machine-replicating-to-azure-using-the-system-center-vmm-to-azure-scenario).
2.  [Erstellen Sie eine neue Replikationsrichtlinie](./hyper-v-azure-tutorial.md#set-up-a-replication-policy).
3.  [Aktivieren Sie die Replikation](./hyper-v-vmm-azure-tutorial.md#enable-replication), und wählen Sie ein Speicherkonto mit aktivierter SSE (Speicherdienstverschlüsselung) aus.

Nach Abschluss der anfänglichen Replikation für Speicherkonten mit aktivierter SSE verwenden Ihre virtuellen Computer die Verschlüsselung ruhender Daten mit Azure Site Recovery.


## <a name="next-steps"></a>Nächste Schritte
Planen Sie die Durchführung der Korrekturschritte, und führen Sie sie möglichst früh durch. Falls Sie Fragen zur eingestellten Unterstützung haben, wenden Sie sich bitte an den Microsoft-Support. Weitere Informationen zum Szenario „Hyper-V in Azure“ finden Sie [hier](hyper-v-vmm-architecture.md).
