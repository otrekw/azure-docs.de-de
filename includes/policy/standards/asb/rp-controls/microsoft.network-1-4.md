---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 05/14/2021
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: 25c320a2cd7e1ca2ad15aa26cba943a1f028ee7a
ms.sourcegitcommit: 17345cc21e7b14e3e31cbf920f191875bf3c5914
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2021
ms.locfileid: "110166145"
---
|Name<br /><sub>(Azure-Portal)</sub> |BESCHREIBUNG |Auswirkungen |Version<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[Gesamten Internetdatenverkehr über Ihre bereitgestellte Azure Firewall-Instanz leiten](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Ffc5e4038-4584-4632-8c85-c0448d374b2c) |Azure Security Center hat festgestellt, dass einige Ihrer Subnetze nicht durch eine Firewall der nächsten Generation geschützt werden. Schützen Ihrer Subnetze vor möglichen Bedrohungen durch Einschränken des Zugriffs auf die Subnetze mit Azure Firewall oder einer unterstützten Firewall der nächsten Generation |AuditIfNotExists, Disabled |[3.0.0-preview](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Network/ASC_All_Internet_traffic_should_be_routed_via_Azure_Firewall.json) |
|[Azure DDoS Protection Standard muss aktiviert sein.](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fa7aca53f-2ed4-4466-a25e-0b45ade68efd) |DDoS Protection Standard sollte für alle virtuellen Netzwerke mit einem Subnetz aktiviert werden, das Teil eines Application Gateways mit einer öffentlichen IP-Adresse ist. |AuditIfNotExists, Disabled |[3.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Security%20Center/ASC_EnableDDoSProtection_Audit.json) |
