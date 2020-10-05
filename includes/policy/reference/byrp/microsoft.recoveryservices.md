---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 09/16/2020
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: ddb6402e4250ca9b361bcb40a393c125d271fdf6
ms.sourcegitcommit: bdd5c76457b0f0504f4f679a316b959dcfabf1ef
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90985106"
---
|Name<br /><sub>(Azure-Portal)</sub> |Beschreibung |Auswirkungen |Version<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[Azure Backup muss für Virtual Machines aktiviert sein.](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F013e242c-8828-4970-87b3-ab247555486d) |Schützen Sie Ihre Azure Virtual Machines, indem Sie Azure Backup aktivieren. Azure Backup ist eine sichere und kostengünstige Lösung zum Schutz von Daten für Azure. |AuditIfNotExists, Disabled |[1.0.1](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Backup/VirtualMachines_EnableAzureBackup_Audit.json) |
|[Sicherung von VMs eines Standorts in einem vorhandenen zentralen Tresor am selben Standort konfigurieren](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F09ce66bc-1220-4153-8104-e3f51c936913) |Diese Richtlinie konfiguriert den Azure Backup-Schutz für virtuelle Computer an einem bestimmten Standort für einen vorhandenen zentralen Tresor am gleichen Standort. Sie gilt nur für virtuelle Computer, die noch nicht für die Sicherung konfiguriert sind. Es wird empfohlen, diese Richtlinie nicht mehr als 200 virtuellen Computern gleichzeitig zuzuweisen. Wenn die Richtlinie mehr als 200 virtuellen Computern zugewiesen wird, kann dies dazu führen, dass die Sicherung einige Stunden später als geplant ausgelöst wird. Diese Richtlinie wird verbessert, um weitere VM-Images zu unterstützen. |deployIfNotExists, auditIfNotExists, disabled |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Backup/VirtualMachineBackup_Backup_DeployIfNotExists.json) |
|[Stellen Sie dem Log Analytics-Arbeitsbereich für ressourcenspezifische Kategorien Diagnoseeinstellungen für den Recovery Services-Tresor bereit.](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fc717fb0c-d118-4c43-ab3d-ece30ac81fb3) |Stellen Sie Diagnoseeinstellungen für den Recovery Services-Tresor bereit, um Daten für ressourcenspezifische Kategorien in den Log Analytics-Arbeitsbereich zu streamen. Wenn eine der ressourcenspezifischen Kategorien nicht aktiviert ist, wird eine neue Diagnoseeinstellung erstellt. |deployIfNotExists |[1.0.1-preview](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Backup/EnableRecoveryServiceVaultDiagnosticSetting_Backup_DeployIfNotExist.json) |
