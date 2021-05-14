---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 05/04/2021
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: c2ef342d28b1f8eabbf8eb44c785586722049815
ms.sourcegitcommit: 02d443532c4d2e9e449025908a05fb9c84eba039
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2021
ms.locfileid: "108792875"
---
|Name<br /><sub>(Azure-Portal)</sub> |Beschreibung |Auswirkungen |Version<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[Datenträgerverschlüsselung sollte auf virtuelle Computer angewendet werden](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F0961003e-5a0a-4549-abde-af6a37f2724d) |Hiermit werden VMs ohne aktivierte Datenträgerverschlüsselung über Azure Security Center als Empfehlungen überwacht. |AuditIfNotExists, Disabled |[2.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Security%20Center/ASC_UnencryptedVMDisks_Audit.json) |
|[Nicht angefügte Datenträger müssen verschlüsselt werden](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F2c89a2e5-7285-40fe-afe0-ae8654b92fb2) |Diese Richtlinie überwacht nicht angefügte Datenträger ohne aktivierte Verschlüsselung. |Audit, Disabled |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Compute/UnattachedDisk_Encryption_Audit.json) |
