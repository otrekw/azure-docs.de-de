---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 09/16/2020
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: 750fa9d06cfeb9755665766068ebad6d9d26c17a
ms.sourcegitcommit: bdd5c76457b0f0504f4f679a316b959dcfabf1ef
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90986933"
---
|Name<br /><sub>(Azure-Portal)</sub> |Beschreibung |Auswirkungen |Version<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[Verwendung benutzerdefinierter RBAC-Regeln überwachen](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fa451c1ef-c6ca-483d-87ed-f49761e3ffb5) |Hiermit werden integrierte Rollen wie z.B. „Benutzer“, „Mitwirkender“ und „Leser“ anstelle benutzerdefinierter RBAC-Rollen überwacht, die fehleranfällig sind. Die Verwendung benutzerdefinierter Rollen wird als Ausnahme betrachtet und erfordert eine strenge Überprüfung und Bedrohungsmodellierung. |Audit, Disabled |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/General/Subscription_AuditCustomRBACRoles_Audit.json) |
|[Benutzerdefinierte Rollen für Abonnementbesitzer dürfen nicht vorhanden sein](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F10ee2ea2-fb4d-45b8-a7e9-a2e770044cd9) |Mit dieser Richtlinie wird sichergestellt, dass keine benutzerdefinierten Abonnementbesitzerrollen vorhanden sind. |Audit, Disabled |[2.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/General/CustomSubscription_OwnerRole_Audit.json) |
