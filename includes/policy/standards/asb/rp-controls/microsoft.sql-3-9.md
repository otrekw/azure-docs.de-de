---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 04/14/2021
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: be27a9f79e66065d6a622ce915f5871b3ce64a2e
ms.sourcegitcommit: 3b5cb7fb84a427aee5b15fb96b89ec213a6536c2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/14/2021
ms.locfileid: "107513456"
---
|Name<br /><sub>(Azure-Portal)</sub> |Beschreibung |Auswirkungen |Version<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[Ein Azure Active Directory-Administrator sollte für SQL Server-Instanzen bereitgestellt werden](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F1f314764-cb73-4fc9-b863-8eca98ac36e9) |Hiermit wird die Bereitstellung eines Azure Active Directory-Administrators für Ihre SQL Server-Instanz überwacht, um die Azure AD-Authentifizierung zu aktivieren. Die Azure AD-Authentifizierung vereinfacht die Berechtigungsverwaltung und ermöglicht eine zentrale Identitätsverwaltung für Datenbankbenutzer und andere Microsoft-Dienste. |AuditIfNotExists, Disabled |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/SQL/SQL_DB_AuditServerADAdmins_Audit.json) |
