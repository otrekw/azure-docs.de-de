---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 04/14/2021
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: 8a6d26eb0fb795f0ca047ff4a976a5b31b9493ee
ms.sourcegitcommit: 3b5cb7fb84a427aee5b15fb96b89ec213a6536c2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/14/2021
ms.locfileid: "107511656"
---
|Name<br /><sub>(Azure-Portal)</sub> |Beschreibung |Auswirkungen |Version<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[Für Schlüsseltresore sollte der Löschschutz aktiviert sein.](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F0b60c0b2-2dc2-4e1c-b5c9-abbed971de53) |Das böswillige Löschen eines Schlüsseltresors kann zu dauerhaftem Datenverlust führen. Ein böswilliger Insider in Ihrer Organisation kann möglicherweise Schlüsseltresore löschen oder bereinigen. Der Löschschutz schützt Sie vor Insiderangriffen, indem ein verbindlicher Aufbewahrungszeitraum für vorläufig gelöschte Schlüsseltresore durchgesetzt wird. Niemand innerhalb Ihrer Organisation oder von Microsoft kann Ihre Schlüsseltresore während des Aufbewahrungszeitraums für vorläufiges Löschen löschen. |Audit, Deny, Disabled |[1.1.1](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Key%20Vault/KeyVault_Recoverable_Audit.json) |
