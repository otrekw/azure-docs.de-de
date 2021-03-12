---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 03/05/2021
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: 6feeb7a4c97cdb5fe5eb409abd2b92c4d96ed5cb
ms.sourcegitcommit: 5bbc00673bd5b86b1ab2b7a31a4b4b066087e8ed
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/07/2021
ms.locfileid: "102444423"
---
|Name<br /><sub>(Azure-Portal)</sub> |Beschreibung |Auswirkungen |Version<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[In Kubernetes Services müssen autorisierte IP-Adressbereiche definiert werden.](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F0e246bcf-5f6f-4f87-bc6f-775d4712c7ea) |Hiermit schränken Sie den Zugriff auf die Kubernetes Service-Verwaltungs-API ein, indem Sie den API-Zugriff nur auf IP-Adressen in bestimmten Bereichen gewähren. Es wird empfohlen, den Zugriff auf autorisierte IP-Adressbereiche einzuschränken, um sicherzustellen, dass nur Anwendungen aus zugelassenen Netzwerken auf den Cluster zugreifen können. |Audit, Disabled |[2.0.1](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Security%20Center/ASC_EnableIpRanges_KubernetesService_Audit.json) |
