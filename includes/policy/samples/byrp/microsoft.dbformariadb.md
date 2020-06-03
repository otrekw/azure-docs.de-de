---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 05/29/2020
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: 83b8e9d2bd65d04a5260495c5add3cb98288955c
ms.sourcegitcommit: f1132db5c8ad5a0f2193d751e341e1cd31989854
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/31/2020
ms.locfileid: "84235482"
---
|Name |BESCHREIBUNG |Auswirkungen |Version |GitHub |
|---|---|---|---|---|
|[Georedundante Sicherung muss für Azure Database for MariaDB aktiviert sein](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F0ec47710-77ff-4a3d-9181-6aa50af424d0) |Diese Richtlinie überwacht alle Azure Database for MariaDB-Instanzen mit nicht aktivierter georedundanter Sicherung. |Audit, Disabled |1.0.0 |[Link](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/SQL/GeoRedundant_DBForMariaDB_Audit.json) |
|[MariaDB-Server muss einen VNET-Dienstendpunkt verwenden](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fdfbd9a64-6114-48de-a47d-90574dc2e489) |Diese Richtlinie überwacht alle MariaDB-Server, die nicht für die Verwendung eines VNET-Dienstendpunkts konfiguriert sind. Weitere Details finden Sie unter [https://aka.ms/mariadbvirtualnetwork](https://aka.ms/mariadbvirtualnetwork). |AuditIfNotExists, Disabled |1.0.1 |[Link](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/SQL/MariaDB_VirtualNetworkServiceEndpoint_Audit.json) |
|[Privater Endpunkt muss für MariaDB-Server aktiviert sein](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F0a1302fb-a631-4106-9753-f3d494733990) |Diese Richtlinie überwacht MariaDB-Server, die nicht für die Verwendung eines privaten Endpunkts konfiguriert sind. Weitere Details finden Sie unter [https://aka.ms/mariadbprivatelink](https://aka.ms/mariadbprivatelink). |AuditIfNotExists, Disabled |1.0.1 |[Link](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/SQL/MariaDB_EnablePrivateEndPoint_Audit.json) |
|[Öffentlicher Netzwerkzugriff muss für MariaDB-Server deaktiviert sein](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Ffdccbe47-f3e3-4213-ad5d-ea459b2fa077) |Diese Richtlinie überwacht MariaDB-Server in Ihrer Umgebung, für die der öffentliche Netzwerkzugriff aktiviert ist. Weitere Details finden Sie unter [https://go.microsoft.com/fwlink/?linkid=2119542](https://go.microsoft.com/fwlink/?linkid=2119542). |Audit, Disabled |1.0.0 |[Link](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/SQL/MariaDB_DisablePublicNetworkAccess_Audit.json) |
