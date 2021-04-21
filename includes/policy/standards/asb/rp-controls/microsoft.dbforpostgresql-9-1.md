---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 04/14/2021
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: 18cbe3f50ee107d710b21e95f6f4774ecd65263c
ms.sourcegitcommit: 3b5cb7fb84a427aee5b15fb96b89ec213a6536c2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/14/2021
ms.locfileid: "107512165"
---
|Name<br /><sub>(Azure-Portal)</sub> |Beschreibung |Auswirkungen |Version<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[Georedundante Sicherung muss für Azure Database for PostgreSQL aktiviert sein](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F48af4db5-9b8b-401c-8e74-076be876a430) |Mit Azure Database for PostgreSQL können Sie die Redundanzoption für Ihren Datenbankserver auswählen. Sie kann auf einen georedundanten Sicherungsspeicher festgelegt werden, in dem die Daten nicht nur in der Region gespeichert werden, in der Ihr Server gehostet wird, sondern auch in eine gekoppelte Region repliziert werden, um die Wiederherstellungsoption bei einem Regionsausfall bereitzustellen. Das Konfigurieren von georedundantem Speicher für die Sicherung ist nur während der Erstellung des Servers zulässig. |Audit, Disabled |[1.0.1](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/SQL/GeoRedundant_DBForPostgreSQL_Audit.json) |
