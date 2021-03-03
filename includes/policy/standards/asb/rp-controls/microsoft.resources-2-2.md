---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 02/09/2021
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: 90f4131a616d0cac93ac2a08999178ac7ea8a0eb
ms.sourcegitcommit: 24f30b1e8bb797e1609b1c8300871d2391a59ac2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "101728483"
---
|Name<br /><sub>(Azure-Portal)</sub> |BESCHREIBUNG |Auswirkungen |Version<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[Für Ihr Abonnement sollte die automatische Bereitstellung des Log Analytics-Agents aktiviert sein.](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F475aae12-b88a-4572-8b36-9b712b2b3a17) |Azure Security Center sammelt Daten von Ihren virtuellen Azure-Computern, um sie in Bezug auf Sicherheitslücken und Bedrohungen zu überwachen. Die Daten werden mit dem Log Analytics-Agent (ehemals Microsoft Monitoring Agent, MMA) gesammelt. Der Agent liest verschiedene sicherheitsrelevante Konfigurationen und Ereignisprotokolle auf dem Computer und kopiert die Daten zur Analyse in den Log Analytics-Arbeitsbereich. Wir empfehlen Ihnen, die automatische Bereitstellung zu aktivieren, damit der Agent auf allen unterstützten und neu erstellten Azure-VMs automatisch bereitgestellt wird. |AuditIfNotExists, Disabled |[1.0.1](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Security%20Center/ASC_Automatic_provisioning_log_analytics_monitoring_agent.json) |
|[Das Azure Monitor-Protokollprofil muss Protokolle für die Kategorien „write“, „delete“ und „action“ erfassen](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F1a4e592a-6a6e-44a5-9814-e36264ca96e7) |Diese Richtlinie stellt sicher, dass ein Protokollprofil Protokolle für die Kategorien „write“, „delete“ und „action“ sammelt. |AuditIfNotExists, Disabled |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Monitoring/ActivityLog_CaptureAllCategories.json) |
|[Azure Monitor muss Aktivitätsprotokolle aus allen Regionen erfassen](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F41388f1c-2db0-4c25-95b2-35d7f5ccbfa9) |Diese Richtlinie überwacht das Azure Monitor-Protokollprofil darauf, dass keine Aktivitäten aus allen unterstützten Azure-Regionen, einschließlich „Global“, exportiert werden. |AuditIfNotExists, Disabled |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Monitoring/ActivityLog_CaptureAllRegions.json) |
