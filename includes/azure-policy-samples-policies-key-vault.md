---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 02/10/2020
ms.author: dacoulte
ms.openlocfilehash: 53494bc9642a3df91492d2353a6aa3c6875c1fff
ms.sourcegitcommit: bdf31d87bddd04382effbc36e0c465235d7a2947
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/12/2020
ms.locfileid: "77170128"
---
|Name |BESCHREIBUNG |Auswirkungen |Version |
|---|---|---|---|
|[Diagnoseeinstellungen für Key Vault in Event Hub bereitstellen](https://github.com/Azure/azure-policy/blob/masterbuilt-in-policies/policyDefinitions/Key%20Vault/KeyVault_DiagnosticLog_Deploy.json) |Hiermit werden die Diagnoseeinstellungen für Key Vault zum Streamen in eine regionale Event Hub-Instanz bereitgestellt, wenn eine Key Vault-Instanz erstellt oder aktualisiert wird, in der diese Diagnoseeinstellungen fehlen. |deployIfNotExists |1.0.0 |
|[Diagnoseprotokolle in Key Vault sollten aktiviert sein](https://github.com/Azure/azure-policy/blob/masterbuilt-in-policies/policyDefinitions/Key%20Vault/KeyVault_AuditDiagnosticLog_Audit.json) |Hiermit wird die Aktivierung von Diagnoseprotokollen überwacht. Auf diese Weise können Sie vergangene Aktivitäten nachvollziehen, wenn es zu einem Sicherheitsincident kommt oder Ihr Netzwerk gefährdet ist. |AuditIfNotExists, Disabled |1.0.0 |
|[Key Vault-Objekte müssen wiederherstellbar sein](https://github.com/Azure/azure-policy/blob/masterbuilt-in-policies/policyDefinitions/Key%20Vault/KeyVault_Recoverable_Audit.json) |Diese Richtlinie überwacht, ob Schlüsseltresorobjekte wiederherstellbar sind. Das Feature „Vorläufiges Löschen“ unterstützt Sie dabei, Ressourcen für eine bestimmte Aufbewahrungsdauer (90 Tage) selbst nach einem DELETE-Vorgang beizubehalten, während das Objekt als gelöscht dargestellt wird. Wenn der Löschschutz aktiviert ist, kann ein Tresor oder ein Objekt im gelöschten Zustand erst nach Ablauf des Aufbewahrungszeitraums von 90 Tagen endgültig gelöscht werden. Diese Tresore und Objekte können weiterhin wiederhergestellt werden, sodass die Kunden sicher sein können, dass die Aufbewahrungsrichtlinie eingehalten wird. |Audit, Disabled |1.0.0 |
