---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 02/09/2021
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: dde3d503a3586e93d13e51fe5bfc640cca2d0bb6
ms.sourcegitcommit: 24f30b1e8bb797e1609b1c8300871d2391a59ac2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "101740598"
---
|Name<br /><sub>(Azure-Portal)</sub> |BESCHREIBUNG |Auswirkungen |Version<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[In Kubernetes Services müssen autorisierte IP-Adressbereiche definiert werden.](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F0e246bcf-5f6f-4f87-bc6f-775d4712c7ea) |Hiermit schränken Sie den Zugriff auf die Kubernetes Service-Verwaltungs-API ein, indem Sie den API-Zugriff nur auf IP-Adressen in bestimmten Bereichen gewähren. Es wird empfohlen, den Zugriff auf autorisierte IP-Adressbereiche einzuschränken, um sicherzustellen, dass nur Anwendungen aus zugelassenen Netzwerken auf den Cluster zugreifen können. |Audit, Disabled |[2.0.1](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Security%20Center/ASC_EnableIpRanges_KubernetesService_Audit.json) |
