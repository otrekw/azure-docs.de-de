---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 08/13/2021
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: 8a0eb129a215bbf3df897e800c625f0efe74ff87
ms.sourcegitcommit: 2d412ea97cad0a2f66c434794429ea80da9d65aa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/14/2021
ms.locfileid: "122264359"
---
|Name<br /><sub>(Azure-Portal)</sub> |BESCHREIBUNG |Auswirkungen |Version<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[Network Watcher muss aktiviert sein](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fb6e2945c-0b7b-40f5-9233-7a5323b5cdc6) |Network Watcher ist ein regionaler Dienst, mit dem Sie Bedingungen auf der Ebene von Netzwerkszenarien in Azure überwachen und diagnostizieren können. Die Überwachung auf Szenarioebene erlaubt die umfassende Diagnose von Problemen auf Netzwerkebene. Es muss eine Network Watcher-Ressourcengruppe in jeder Region erstellt werden, in der ein virtuelles Netzwerk vorhanden ist. Wenn eine Network Watcher-Ressourcengruppe in einer bestimmten Region nicht verfügbar ist, wird eine Warnung angezeigt. |AuditIfNotExists, Disabled |[3.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Network/NetworkWatcher_Enabled_Audit.json) |
