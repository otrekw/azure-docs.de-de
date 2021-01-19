---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 01/08/2021
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: 0688e5e0703f74ee48e37119edcece465e51b5e5
ms.sourcegitcommit: 8dd8d2caeb38236f79fe5bfc6909cb1a8b609f4a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/08/2021
ms.locfileid: "98047219"
---
|Name<br /><sub>(Azure-Portal)</sub> |BESCHREIBUNG |Auswirkungen |Version<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[App Configuration sollte einen kundenseitig verwalteten Schlüssel verwenden.](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F967a4b4b-2da9-43c1-b7d0-f98d0d74d0b1) |Kundenseitig verwaltete Schlüssel bieten einen verbesserten Datenschutz, da Sie so Ihre Verschlüsselungsschlüssel verwalten können. Dies ist häufig zur Einhaltung von Compliancevorgaben erforderlich. |Audit, Deny, Disabled |[1.1.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/App%20Configuration/CustomerManagedKey_Audit.json) |
|[App Configuration sollte Private Link verwenden.](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fca610c1d-041c-4332-9d88-7ed3094967c7) |Mit Azure Private Link können Sie Ihr virtuelles Netzwerk mit Azure-Diensten verbinden, ohne auf der Quelle oder auf dem Ziel eine öffentliche IP-Adresse zu verwenden. Auf der Private Link-Plattform wird die Konnektivität zwischen dem Consumer und den Diensten über das Azure-Backbone-Netzwerk verarbeitet. Indem Sie private Endpunkte nicht dem gesamten Dienst, sondern nur Ihren App Configuration-Instanzen zuordnen, sind Sie auch vor Risiken aufgrund von Datenlecks geschützt. Weitere Informationen finden Sie unter [https://aka.ms/appconfig/private-endpoint](https://aka.ms/appconfig/private-endpoint). |AuditIfNotExists, Disabled |[1.0.2](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/App%20Configuration/PrivateLink_Audit.json) |
