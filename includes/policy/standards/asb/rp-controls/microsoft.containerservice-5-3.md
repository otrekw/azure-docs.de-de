---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 03/10/2021
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: d29a29bcccb065b7325285f7cbebe7075e4b4fb2
ms.sourcegitcommit: b572ce40f979ebfb75e1039b95cea7fce1a83452
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/11/2021
ms.locfileid: "103020936"
---
|Name<br /><sub>(Azure-Portal)</sub> |Beschreibung |Auswirkungen |Version<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[Für Kubernetes Service muss ein Upgrade auf eine Kubernetes-Version ohne Sicherheitsrisiko durchgeführt werden.](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Ffb893a29-21bb-418c-a157-e99480ec364c) |Führen Sie ein Upgrade Ihres Kubernetes Service-Clusters auf eine höhere Kubernetes-Version durch, um es vor bekannten Sicherheitsrisiken in Ihrer aktuellen Kubernetes-Version zu schützen. Das Sicherheitsrisiko „CVE-2019-9946“ wurde in den Kubernetes-Versionen 1.11.9+, 1.12.7+, 1.13.5+ und 1.14.0+ gepatcht. |Audit, Disabled |[1.0.2](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Security%20Center/ASC_UpgradeVersion_KubernetesService_Audit.json) |
