---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 08/13/2021
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: 8256cbaab86b8855ee71431edff8d8a4ec55ee65
ms.sourcegitcommit: 2d412ea97cad0a2f66c434794429ea80da9d65aa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/14/2021
ms.locfileid: "122356366"
---
|Name<br /><sub>(Azure-Portal)</sub> |BESCHREIBUNG |Auswirkungen |Version<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[Allow managing tenant ids to onboard through Azure Lighthouse (Verwaltungsmandanten-IDs das Onboarding über Azure Lighthouse gestatten)](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F7a8a51a3-ad87-4def-96f3-65a1839242b6) |Das Beschränken der Azure Lighthouse-Delegierungen auf bestimmte Verwaltungsmandanten erhöht die Sicherheit, da sich der Personenkreis verkleinert, der Ihre Azure-Ressourcen verwalten kann. |deny |[1.0.1](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Lighthouse/AllowCertainManagingTenantIds_Deny.json) |
|[Delegierung von Bereichen an Verwaltungsmandanten überwachen](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F76bed37b-484f-430f-a009-fd7592dff818) |Hiermit wird die Delegierung von Bereichen an einen Verwaltungsmandanten über Azure Lighthouse überwacht. |Audit, Disabled |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Lighthouse/Lighthouse_Delegations_Audit.json) |
