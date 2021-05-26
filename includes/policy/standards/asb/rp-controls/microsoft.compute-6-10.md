---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 05/14/2021
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: a3d1734461d47c5dd25793ca8e6ce04d3c3ab09d
ms.sourcegitcommit: 17345cc21e7b14e3e31cbf920f191875bf3c5914
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2021
ms.locfileid: "110163732"
---
|Name<br /><sub>(Azure-Portal)</sub> |BESCHREIBUNG |Auswirkungen |Version<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[Adaptive Anwendungssteuerung zum Definieren sicherer Anwendungen muss auf Computern aktiviert sein](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F47a6b606-51aa-4496-8bb7-64b11cf66adc) |Hiermit wird die Anwendungssteuerung aktiviert, um die Liste der als sicher bekannten Anwendungen zu definieren, die auf Ihren Computern ausgeführt werden. Außerdem werden Sie bei Ausführung anderer Anwendungen benachrichtigt. Dies verstärkt den Schutz Ihrer Computer vor Schadsoftware. Um die Konfiguration und Wartung Ihrer Regeln zu vereinfachen, nutzt Security Center Machine Learning, um die auf den einzelnen Computern ausgeführten Anwendungen zu analysieren und eine Liste der als sicher bekannten Anwendungen vorzuschlagen. |AuditIfNotExists, Disabled |[3.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Security%20Center/ASC_AdaptiveApplicationControls_Audit.json) |
