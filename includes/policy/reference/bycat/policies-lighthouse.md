---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 04/28/2021
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: 1915cc31475a9a6c1b338182e684f8b3430d906e
ms.sourcegitcommit: 516eb79d62b8dbb2c324dff2048d01ea50715aa1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2021
ms.locfileid: "108183161"
---
|Name<br /><sub>(Azure-Portal)</sub> |BESCHREIBUNG |Auswirkungen |Version<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[Allow managing tenant ids to onboard through Azure Lighthouse (Verwaltungsmandanten-IDs das Onboarding über Azure Lighthouse gestatten)](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F7a8a51a3-ad87-4def-96f3-65a1839242b6) |Das Beschränken der Azure Lighthouse-Delegierungen auf bestimmte Verwaltungsmandanten erhöht die Sicherheit, da sich der Personenkreis verkleinert, der Ihre Azure-Ressourcen verwalten kann. |deny |[1.0.1](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Lighthouse/AllowCertainManagingTenantIds_Deny.json) |
|[Delegierung von Bereichen an Verwaltungsmandanten überwachen](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F76bed37b-484f-430f-a009-fd7592dff818) |Hiermit wird die Delegierung von Bereichen an einen Verwaltungsmandanten über Azure Lighthouse überwacht. |Audit, Disabled |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Lighthouse/Lighthouse_Delegations_Audit.json) |
