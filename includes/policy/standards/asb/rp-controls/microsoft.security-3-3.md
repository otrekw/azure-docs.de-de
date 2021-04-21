---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 04/14/2021
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: 1ceb27a785ab186aad4a72ff2d27e08ee67a4575
ms.sourcegitcommit: 3b5cb7fb84a427aee5b15fb96b89ec213a6536c2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/14/2021
ms.locfileid: "107512867"
---
|Name<br /><sub>(Azure-Portal)</sub> |Beschreibung |Auswirkungen |Version<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[Maximal 3 Besitzer sollten für Ihr Abonnement festgelegt sein](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F4f11b553-d42e-4e3a-89be-32ca364cad4c) |Es wird empfohlen, bis zu drei Abonnementbesitzer festzulegen, um die Möglichkeit einer Sicherheitsverletzung durch einen kompromittierten Besitzer zu verringern. |AuditIfNotExists, Disabled |[3.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Security%20Center/ASC_DesignateLessThanXOwners_Audit.json) |
|[Ihrem Abonnement sollte mehr als ein Besitzer zugewiesen sein](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F09024ccc-0c5f-475e-9457-b7c0d9ed487b) |Es wird empfohlen, mehrere Abonnementbesitzer festzulegen, um Redundanz beim Administratorzugriff zu gewährleisten. |AuditIfNotExists, Disabled |[3.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Security%20Center/ASC_DesignateMoreThanOneOwner_Audit.json) |
