---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 05/04/2021
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: ca9a38bb61d15e6129904d0d8e57db614cee7a43
ms.sourcegitcommit: 02d443532c4d2e9e449025908a05fb9c84eba039
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2021
ms.locfileid: "108793354"
---
|Name<br /><sub>(Azure-Portal)</sub> |BESCHREIBUNG |Auswirkungen |Version<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[Für SQL Server-Instanzen mit Überwachung im Speicherkontoziel muss die Datenaufbewahrung auf mindestens 90 Tage festgelegt werden](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F89099bee-89e0-4b26-a5f4-165451757743) |Zum Zweck der Untersuchung von Incidents wird empfohlen, die im Rahmen der SQL Server-Überwachung im Speicherkontoziel erfassten Daten für mindestens 90 Tage aufzubewahren. Stellen Sie sicher, dass Sie die erforderlichen Aufbewahrungsregeln für die Regionen einhalten, in denen Sie tätig sind. Dies ist gelegentlich zur Einhaltung gesetzlicher Standards erforderlich. |AuditIfNotExists, Disabled |[3.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/SQL/SqlServerAuditingRetentionDays_Audit.json) |
