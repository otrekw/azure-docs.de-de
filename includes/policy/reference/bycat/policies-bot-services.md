---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 01/25/2021
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: 6a5c86dc1b10ed79e5dabbf5be7bf3ee2b677dee
ms.sourcegitcommit: fc8ce6ff76e64486d5acd7be24faf819f0a7be1d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/26/2021
ms.locfileid: "98807689"
---
|Name<br /><sub>(Azure-Portal)</sub> |Beschreibung |Auswirkungen |Version<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[Der Bot Service-Endpunkt muss ein gültiger HTTPS-URI sein.](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F6164527b-e1ee-4882-8673-572f425f5e0a) |Daten können während der Übertragung manipuliert werden. Es gibt Protokolle, die eine Verschlüsselung ermöglichen, um Missbrauch und Manipulationen vorzubeugen. Um sicherzustellen, dass Ihre Bots ausschließlich über verschlüsselte Kanäle kommunizieren, legen Sie den Endpunkt auf einen gültigen HTTPS-URI fest. Dadurch wird sichergestellt, dass das HTTPS-Protokoll zum Verschlüsseln Ihrer Daten während der Übertragung verwendet wird. Darüber hinaus ist es häufig Voraussetzung zur Einhaltung gesetzlicher oder branchenüblicher Standards. Informationen finden Sie unter [https://docs.microsoft.com/azure/bot-service/bot-builder-security-guidelines](https://docs.microsoft.com/azure/bot-service/bot-builder-security-guidelines). |Audit, Deny, Disabled |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Bot%20Services/BotService_ValidEndpoint_Audit.json) |
