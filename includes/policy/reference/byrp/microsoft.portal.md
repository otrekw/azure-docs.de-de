---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 04/14/2021
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: e28dc20fe5e789bbaa5aad9402d37f039eabe6e5
ms.sourcegitcommit: 3b5cb7fb84a427aee5b15fb96b89ec213a6536c2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/14/2021
ms.locfileid: "107504210"
---
|Name<br /><sub>(Azure-Portal)</sub> |Beschreibung |Auswirkungen |Version<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[Freigegebene Dashboards dürfen keine Markdownkacheln mit Inline-Inhalten aufweisen](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F04c655fe-0ac7-48ae-9a32-3a2e208c7624) |Verhindert das Erstellen eines freigegebenen Dashboards mit Inlineinhalten auf Markdownkacheln und erzwingt, dass der Inhalt als online gehostete Markdowndatei gespeichert werden soll. Wenn Sie Inlineinhalte auf der Markdownkachel verwenden, können Sie die Verschlüsselung des Inhalts nicht verwalten. Das Konfigurieren eines eigenen Speichers ermöglicht das Verschlüsseln, das doppelte Verschlüsseln und sogar das Verwenden eigener Schlüssel. Wenn Sie diese Richtlinie aktivieren, können Benutzer die Version 2020-09-01-preview oder eine höhere Version der REST-API für freigegebene Dashboards verwenden. |Audit, Deny, Disabled |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Portal/SharedDashboardInlineContent_Deny.json) |
