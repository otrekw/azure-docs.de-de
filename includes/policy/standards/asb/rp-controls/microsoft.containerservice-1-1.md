---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 03/31/2021
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: 515c5f6695eef27c3c16187f08229b9573524222
ms.sourcegitcommit: 99fc6ced979d780f773d73ec01bf651d18e89b93
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/31/2021
ms.locfileid: "106097258"
---
|Name<br /><sub>(Azure-Portal)</sub> |BESCHREIBUNG |Auswirkungen |Version<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[In Kubernetes Services müssen autorisierte IP-Adressbereiche definiert werden.](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F0e246bcf-5f6f-4f87-bc6f-775d4712c7ea) |Hiermit schränken Sie den Zugriff auf die Kubernetes Service-Verwaltungs-API ein, indem Sie den API-Zugriff nur auf IP-Adressen in bestimmten Bereichen gewähren. Es wird empfohlen, den Zugriff auf autorisierte IP-Adressbereiche einzuschränken, um sicherzustellen, dass nur Anwendungen aus zugelassenen Netzwerken auf den Cluster zugreifen können. |Audit, Disabled |[2.0.1](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Security%20Center/ASC_EnableIpRanges_KubernetesService_Audit.json) |
