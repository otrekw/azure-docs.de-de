---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 03/05/2020
ms.author: dacoulte
ms.openlocfilehash: 45cb4e589a00a2ab17fee280bb6066eaee4f24c9
ms.sourcegitcommit: 05b36f7e0e4ba1a821bacce53a1e3df7e510c53a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78669393"
---
|Name |BESCHREIBUNG |Auswirkungen |Version |GitHub |
|---|---|---|---|---|
|[Containerregistrierungen müssen mit einem kundenseitig verwalteten Schlüssel (CMK) verschlüsselt werden](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F5b9159ae-1701-4a6f-9a7a-aa9c8ddd0580) |Hiermit überwachen Sie Containerregistrierungen, für die die Verschlüsselung mit kundenseitig verwalteten Schlüsseln (Customer-Managed Key, CMK) nicht aktiviert ist. Weitere Informationen zur CMK-Verschlüsselung finden Sie unter: https://aka.ms/acr/CMK. |Audit, Disabled |1.0.0-preview |[Link](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Container%20Registry/ACR_CMKEncryptionEnabled_Audit.json)
|[Containerregistrierungen dürfen keinen uneingeschränkten Netzwerkzugriff zulassen](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fd0793b48-0edc-4296-a390-4c75d1bdfd71) |Hiermit überwachen Sie Containerregistrierungen, für die keine Netzwerkregeln (IP oder VNET) konfiguriert sind und standardmäßig den gesamten Netzwerkzugriff zulassen. Containerregistrierungen mit mindestens einer IP-/Firewallregel oder einem konfigurierten virtuellen Netzwerk werden als konform eingestuft. Weitere Informationen zu Container Registry-Netzwerkregeln finden Sie unter: https://aka.ms/acr/vnet. |Audit, Disabled |1.0.0-preview |[Link](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Container%20Registry/ACR_NetworkRulesExist_Audit.json)
