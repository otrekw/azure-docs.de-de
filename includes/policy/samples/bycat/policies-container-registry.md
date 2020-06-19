---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 06/10/2020
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: 0ba2284775d97c51db52b474c4c42ddaab29c514
ms.sourcegitcommit: 537c539344ee44b07862f317d453267f2b7b2ca6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/11/2020
ms.locfileid: "84709419"
---
|Name<br /><sub>(Azure-Portal)</sub> |BESCHREIBUNG |Auswirkungen |Version<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[Containerregistrierungen müssen mit einem kundenseitig verwalteten Schlüssel (CMK) verschlüsselt werden](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F5b9159ae-1701-4a6f-9a7a-aa9c8ddd0580) |Hiermit überwachen Sie Containerregistrierungen, für die die Verschlüsselung mit kundenseitig verwalteten Schlüsseln (Customer-Managed Key, CMK) nicht aktiviert ist. Weitere Informationen zur CMK-Verschlüsselung finden Sie unter [https://aka.ms/acr/CMK](https://aka.ms/acr/CMK). |Audit, Disabled |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Container%20Registry/ACR_CMKEncryptionEnabled_Audit.json) |
|[Containerregistrierungen dürfen keinen uneingeschränkten Netzwerkzugriff zulassen](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fd0793b48-0edc-4296-a390-4c75d1bdfd71) |Hiermit überwachen Sie Containerregistrierungen, für die keine Netzwerkregeln (IP oder VNET) konfiguriert sind und standardmäßig den gesamten Netzwerkzugriff zulassen. Containerregistrierungen mit mindestens einer IP-/Firewallregel oder einem konfigurierten virtuellen Netzwerk werden als konform eingestuft. Weitere Informationen zu Container Registry-Netzwerkregeln finden Sie unter [https://aka.ms/acr/vnet](https://aka.ms/acr/vnet). |Audit, Disabled |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Container%20Registry/ACR_NetworkRulesExist_Audit.json) |
|[Containerregistrierungen müssen Private Link-Instanzen verwenden](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fe8eef0a8-67cf-4eb4-9386-14b0e78733d4) |Überwachen Sie Containerregistrierungen, die nicht über mindestens eine Verbindung mit privatem Endpunkt verfügen. Clients in einem virtuellen Netzwerk können über Private Link-Instanzen sicher auf Ressourcen mit Verbindungen mit privaten Endpunkten zugreifen. Weitere Informationen finden Sie unter [https://aka.ms/acr/private-link](https://aka.ms/acr/private-link). |Audit, Disabled |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Container%20Registry/ACR_PrivateEndpointEnabled_Audit.json) |
