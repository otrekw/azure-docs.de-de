---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 05/29/2020
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: 0c6fa6bacda2d0dd4c3b3fc35adc7b53a1f7d77a
ms.sourcegitcommit: f1132db5c8ad5a0f2193d751e341e1cd31989854
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/31/2020
ms.locfileid: "84233761"
---
|Name |BESCHREIBUNG |Auswirkungen |Version |GitHub |
|---|---|---|---|---|
|[Für Azure Cosmos DB zulässige Standorte](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F0473574d-2d43-4217-aefe-941fcdf7e684) |Mit dieser Richtlinie können Sie die Standorte einschränken, die Ihre Organisation beim Bereitstellen von Azure Cosmos DB-Ressourcen angeben kann. Wird zur Erzwingung Ihrer Geokonformitätsanforderungen verwendet. |[parameters('policyEffect')] |1.0.0 |[Link](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Cosmos%20DB/Cosmos_Locations_Deny.json) |
|[Advanced Threat Protection für Cosmos DB-Konten bereitstellen](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fb5f04e03-92a3-4b09-9410-2cc5e5047656) |Mit dieser Richtlinie wird Advanced Threat Protection über Cosmos DB-Konten hinweg aktiviert. |DeployIfNotExists, Disabled |1.0.0 |[Link](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Cosmos%20DB/CosmosDbAdvancedThreatProtection_Deploy.json) |
