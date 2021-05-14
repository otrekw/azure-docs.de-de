---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 05/04/2021
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: acde4db609dedc9f2ccce409102a152e5d0ca425
ms.sourcegitcommit: 02d443532c4d2e9e449025908a05fb9c84eba039
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2021
ms.locfileid: "108791260"
---
|Name<br /><sub>(Azure-Portal)</sub> |Beschreibung |Auswirkungen |Version<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[Für Schlüsseltresore sollte der Löschschutz aktiviert sein.](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F0b60c0b2-2dc2-4e1c-b5c9-abbed971de53) |Das böswillige Löschen eines Schlüsseltresors kann zu dauerhaftem Datenverlust führen. Ein böswilliger Insider in Ihrer Organisation kann möglicherweise Schlüsseltresore löschen oder bereinigen. Der Löschschutz schützt Sie vor Insiderangriffen, indem ein verbindlicher Aufbewahrungszeitraum für vorläufig gelöschte Schlüsseltresore durchgesetzt wird. Niemand innerhalb Ihrer Organisation oder von Microsoft kann Ihre Schlüsseltresore während des Aufbewahrungszeitraums für vorläufiges Löschen löschen. |Audit, Deny, Disabled |[1.1.1](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Key%20Vault/KeyVault_Recoverable_Audit.json) |
