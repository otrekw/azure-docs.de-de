---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 04/28/2021
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: e7b8d0a28746dd07d305283c75e359dca6ed9e83
ms.sourcegitcommit: 516eb79d62b8dbb2c324dff2048d01ea50715aa1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2021
ms.locfileid: "108211282"
---
|Name<br /><sub>(Azure-Portal)</sub> |BESCHREIBUNG |Auswirkungen |Version<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[Windows-Computer überwachen, auf denen der Log Analytics-Agent nicht wie erwartet verbunden ist](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F6265018c-d7e2-432f-a75d-094d5f6f4465) |Erfordert, dass die erforderlichen Komponenten im Bereich der Richtlinienzuweisung bereitgestellt werden. Einzelheiten dazu finden Sie unter [https://aka.ms/gcpol](https://aka.ms/gcpol). Computer werden als nicht konform eingestuft, wenn der Agent nicht installiert ist, oder wenn er installiert ist, das COM-Objekt „AgentConfigManager.MgmtSvcCfg“ jedoch zurückgibt, dass er nicht bei dem Arbeitsbereich registriert ist, der der im Richtlinienparameter angegebenen ID entspricht. |auditIfNotExists |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Guest%20Configuration/GuestConfiguration_WindowsLogAnalyticsAgentConnection_AINE.json) |
|[Der Log Analytics-Agent sollte in Virtual Machine Scale Sets installiert sein](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fefbde977-ba53-4479-b8e9-10b957924fbf) |Diese Richtlinie überwacht alle Windows-/Linux-Virtual Machine Scale Sets, wenn der Log Analytics-Agent nicht installiert ist. |AuditIfNotExists, Disabled |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Monitoring/VMSS_LogAnalyticsAgent_AuditIfNotExists.json) |
|[Der Log Analytics-Agent sollte auf virtuellen Computern installiert werden](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fa70ca396-0a34-413a-88e1-b956c1e683be) |Diese Richtlinie überwacht alle virtuellen Windows-/Linux-Computer, wenn der Log Analytics-Agent nicht installiert ist. |AuditIfNotExists, Disabled |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Monitoring/VirtualMachines_LogAnalyticsAgent_AuditIfNotExists.json) |
