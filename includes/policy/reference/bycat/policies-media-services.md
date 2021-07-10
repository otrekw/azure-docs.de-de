---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 06/11/2021
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: 887d2784d4949a70763e6c08ce3ec72aaddd0bcd
ms.sourcegitcommit: 942a1c6df387438acbeb6d8ca50a831847ecc6dc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/11/2021
ms.locfileid: "112019475"
---
|Name<br /><sub>(Azure-Portal)</sub> |Beschreibung |Auswirkungen |Version<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[Azure Media Services-Konten sollten ein API verwenden, welches Private Link unterstützt](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fa77d8bb4-8d22-4bc1-a884-f582a705b480) |Media Services-Konten sollten mit einem API erstellt werden, welches Private Link unterstützt. |Audit, Deny, Disabled |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Media%20Services/MediaServices_RequirePrivateLinkSupport_Audit.json) |
|[Azure Media Services-Konten, die Zugriff auf die Legacy-API (v2) erlauben, sollten blockiert werden](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fccf93279-9c91-4143-a841-8d1f21505455) |Die Media Services Legacy-API (v2) gestattet Anforderungen, die nicht über Azure Policy verwaltet werden können. Media Services-Ressourcen, die mit der API vom 01.05.2020 oder neuer erstellt wurden, blockieren den Zugriff auf die Legacy-API (v2). |Audit, Deny, Disabled |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Media%20Services/MediaServices_BlockRestV2_Audit.json) |
|[Azure Media Services-Richtlinien für symmetrische Schlüssel sollten die Tokenauthentifizierung verwenden](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fdaccf7e4-9808-470c-a848-1c5b582a1afb) |Richtlinien für symmetrische Schlüssel definieren die Bedingungen, die für den Zugriff auf symmetrische Schlüssel erfüllt sein müssen. Eine Tokeneinschränkung stellt sicher, dass nur Benutzer mit gültigen Token von einem Authentifizierungsdienst, z. B. Azure Active Directory, auf symmetrische Schlüssel zugreifen können. |Audit, Deny, Disabled |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Media%20Services/ContentKeyPolicies_RequireTokenAuth_Audit.json) |
|[Azure Media Services-Aufträge mit HTTPS-Eingaben sollten Eingabe-URIs auf zulässige URI-Muster begrenzen](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fe9914afe-31cd-4b8a-92fa-c887f847d477) |Beschränken Sie HTTPS-Eingaben, die von Media Services-Aufträgen verwendet werden, auf bekannte Endpunkte. Eingaben von HTTPS-Endpunkten können vollständig deaktiviert werden, indem eine leere Liste mit zulässigen Auftragseingabemustern festgelegt wird. Wenn in Auftragseingaben ein „baseUri“ angegeben wird, werden die Muster mit diesem Wert abgeglichen. Wenn „baseUri“ nicht festgelegt ist, wird das Muster mit der „files“-Eigenschaft abgeglichen. |Verweigern, deaktiviert |[1.0.1](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Media%20Services/Jobs_RestrictHttpInputs.json) |
