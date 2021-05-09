---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 04/28/2021
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: b9b7eef56c122ec9bc3a59f1442b8ad935a8c71b
ms.sourcegitcommit: 516eb79d62b8dbb2c324dff2048d01ea50715aa1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2021
ms.locfileid: "108211339"
---
|Name<br /><sub>(Azure-Portal)</sub> |BESCHREIBUNG |Auswirkungen |Version<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[Für Schlüsseltresore sollte der Löschschutz aktiviert sein.](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F0b60c0b2-2dc2-4e1c-b5c9-abbed971de53) |Das böswillige Löschen eines Schlüsseltresors kann zu dauerhaftem Datenverlust führen. Ein böswilliger Insider in Ihrer Organisation kann möglicherweise Schlüsseltresore löschen oder bereinigen. Der Löschschutz schützt Sie vor Insiderangriffen, indem ein verbindlicher Aufbewahrungszeitraum für vorläufig gelöschte Schlüsseltresore durchgesetzt wird. Niemand innerhalb Ihrer Organisation oder von Microsoft kann Ihre Schlüsseltresore während des Aufbewahrungszeitraums für vorläufiges Löschen löschen. |Audit, Deny, Disabled |[1.1.1](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Key%20Vault/KeyVault_Recoverable_Audit.json) |
