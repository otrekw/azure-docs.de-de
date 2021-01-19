---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 01/08/2021
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: f03688012100c31865fb25087f7b4049c087b43e
ms.sourcegitcommit: 8dd8d2caeb38236f79fe5bfc6909cb1a8b609f4a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/08/2021
ms.locfileid: "98047288"
---
|Name<br /><sub>(Azure-Portal)</sub> |BESCHREIBUNG |Auswirkungen |Version<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[Azure Event Grid-Domänen sollten Private Link verwenden.](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F9830b652-8523-49cc-b1b3-e17dce1127ca) |Mit Azure Private Link können Sie Ihr virtuelles Netzwerk mit Azure-Diensten verbinden, ohne auf der Quelle oder auf dem Ziel eine öffentliche IP-Adresse zu verwenden. Auf der Private Link-Plattform wird die Konnektivität zwischen dem Consumer und den Diensten über das Azure-Backbone-Netzwerk verarbeitet. Indem Sie private Endpunkte nicht dem gesamten Dienst, sondern nur Ihren Event Grid-Domänen zuordnen, sind Sie auch vor Risiken aufgrund von Datenlecks geschützt. Weitere Informationen finden Sie unter [https://aka.ms/privateendpoints](https://aka.ms/privateendpoints). |Audit, Disabled |[1.0.1](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Event%20Grid/EventGridDomains_EnablePrivateEndpoint_Audit.json) |
|[Azure Event Grid-Themen sollten Private Link verwenden.](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F4b90e17e-8448-49db-875e-bd83fb6f804f) |Mit Azure Private Link können Sie Ihr virtuelles Netzwerk mit Azure-Diensten verbinden, ohne auf der Quelle oder auf dem Ziel eine öffentliche IP-Adresse zu verwenden. Auf der Private Link-Plattform wird die Konnektivität zwischen dem Consumer und den Diensten über das Azure-Backbone-Netzwerk verarbeitet. Indem Sie private Endpunkte nicht dem gesamten Dienst, sondern nur Ihren Themen zuordnen, sind Sie auch vor Risiken aufgrund von Datenlecks geschützt. Weitere Informationen finden Sie unter [https://aka.ms/privateendpoints](https://aka.ms/privateendpoints). |Audit, Disabled |[1.0.1](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Event%20Grid/EventGridTopics_EnablePrivateEndpoint_Audit.json) |
