---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 05/05/2020
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: a44755805d28d4bc1edee6057af9c0c678cfccf9
ms.sourcegitcommit: 11572a869ef8dbec8e7c721bc7744e2859b79962
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/05/2020
ms.locfileid: "82836466"
---
|Name |BESCHREIBUNG |Auswirkungen |Version |GitHub |
|---|---|---|---|---|
|[Für Azure Cosmos DB zulässige Standorte](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F0473574d-2d43-4217-aefe-941fcdf7e684) |Mit dieser Richtlinie können Sie die Standorte einschränken, die Ihre Organisation beim Bereitstellen von Azure Cosmos DB-Ressourcen angeben kann. Wird zur Erzwingung Ihrer Geokonformitätsanforderungen verwendet. |[parameters('policyEffect')] |1.0.0 |[Link](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Cosmos%20DB/Cosmos_Locations_Deny.json) |
|[Cosmos DB sollte einen VNET-Dienstendpunkt verwenden](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fe0a2b1a3-f7f9-4569-807f-2a9edebdf4d9) |Diese Richtlinie überwacht alle Cosmos DB-Instanzen, die nicht für die Verwendung eines VNET-Dienstendpunkts konfiguriert sind. |Audit, Disabled |1.0.0 |[Link](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Network/VirtualNetworkServiceEndpoint_CosmosDB_Audit.json) |
|[Advanced Threat Protection für Cosmos DB-Konten bereitstellen](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fb5f04e03-92a3-4b09-9410-2cc5e5047656) |Mit dieser Richtlinie wird Advanced Threat Protection über Cosmos DB-Konten hinweg aktiviert. |DeployIfNotExists, Disabled |1.0.0 |[Link](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Cosmos%20DB/CosmosDbAdvancedThreatProtection_Deploy.json) |
