---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 04/14/2021
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: 56b2ae4aff99bed46fa5ad7f0947193b2f465431
ms.sourcegitcommit: 3b5cb7fb84a427aee5b15fb96b89ec213a6536c2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/14/2021
ms.locfileid: "107511601"
---
|Name<br /><sub>(Azure-Portal)</sub> |Beschreibung |Auswirkungen |Version<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[Für Ihr Abonnement sollte die automatische Bereitstellung des Log Analytics-Agents aktiviert sein.](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F475aae12-b88a-4572-8b36-9b712b2b3a17) |Azure Security Center sammelt Daten von Ihren Azure-VMs, um diese auf Sicherheitslücken und Bedrohungen zu überwachen. Die Daten werden mit dem Log Analytics-Agent (ehemals Microsoft Monitoring Agent, MMA) gesammelt. Der Agent liest verschiedene sicherheitsrelevante Konfigurationen und Ereignisprotokolle auf dem Computer und kopiert die Daten zur Analyse in den Log Analytics-Arbeitsbereich. Wir empfehlen Ihnen, die automatische Bereitstellung zu aktivieren, damit der Agent auf allen unterstützten und allen neu erstellten Azure-VMs automatisch bereitgestellt wird. |AuditIfNotExists, Disabled |[1.0.1](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Security%20Center/ASC_Automatic_provisioning_log_analytics_monitoring_agent.json) |
