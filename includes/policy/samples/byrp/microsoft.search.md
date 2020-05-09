---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 05/05/2020
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: 5aa0bf995a6aa6c7076cc60a824514b16a69fc4d
ms.sourcegitcommit: 11572a869ef8dbec8e7c721bc7744e2859b79962
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/05/2020
ms.locfileid: "82838608"
---
|Name |BESCHREIBUNG |Auswirkungen |Version |GitHub |
|---|---|---|---|---|
|[Diagnoseeinstellungen für Suchdienste in Event Hub bereitstellen](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F3d5da587-71bd-41f5-ac95-dd3330c2d58d) |Hiermit werden die Diagnoseeinstellungen für Suchdienste zum Streamen in eine regionale Event Hub-Instanz bereitgestellt, wenn ein Suchdienst erstellt oder aktualisiert wird, in dem diese Diagnoseeinstellungen fehlen. |DeployIfNotExists, Disabled |2.0.0 |[Link](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Monitoring/Search_DeployDiagnosticLog_Deploy_EventHub.json) |
|[Diagnoseeinstellungen für Suchdienste in Log Analytics-Arbeitsbereich bereitstellen](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F08ba64b8-738f-4918-9686-730d2ed79c7d) |Hiermit werden die Diagnoseeinstellungen für Suchdienste zum Streamen in einen regionalen Log Analytics-Arbeitsbereich bereitgestellt, wenn ein Suchdienst erstellt oder aktualisiert wird, in dem diese Diagnoseeinstellungen fehlen. |DeployIfNotExists, Disabled |1.0.0 |[Link](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Monitoring/Search_DeployDiagnosticLog_Deploy_LogAnalytics.json) |
|[In den Search-Diensten sollten Diagnoseprotokolle aktiviert sein](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fb4330a05-a843-4bc8-bf9a-cacce50c67f4) |Hiermit wird die Aktivierung von Diagnoseprotokollen überwacht. Auf diese Weise können Sie vergangene Aktivitäten nachvollziehen, wenn es zu einem Sicherheitsincident kommt oder Ihr Netzwerk gefährdet ist. |AuditIfNotExists, Disabled |2.0.0 |[Link](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Search/Search_AuditDiagnosticLog_Audit.json) |
