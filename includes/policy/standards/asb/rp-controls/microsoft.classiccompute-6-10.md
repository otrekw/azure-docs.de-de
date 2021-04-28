---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 04/21/2021
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: 22592adf9d3bb538ba087ff9ab828bd374b4d61e
ms.sourcegitcommit: 2aeb2c41fd22a02552ff871479124b567fa4463c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/22/2021
ms.locfileid: "107880755"
---
|Name<br /><sub>(Azure-Portal)</sub> |BESCHREIBUNG |Auswirkungen |Version<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[Adaptive Anwendungssteuerung zum Definieren sicherer Anwendungen muss auf Computern aktiviert sein](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F47a6b606-51aa-4496-8bb7-64b11cf66adc) |Hiermit wird die Anwendungssteuerung aktiviert, um die Liste der als sicher bekannten Anwendungen zu definieren, die auf Ihren Computern ausgeführt werden. Außerdem werden Sie bei Ausführung anderer Anwendungen benachrichtigt. Dies verstärkt den Schutz Ihrer Computer vor Schadsoftware. Um die Konfiguration und Wartung Ihrer Regeln zu vereinfachen, nutzt Security Center Machine Learning, um die auf den einzelnen Computern ausgeführten Anwendungen zu analysieren und eine Liste der als sicher bekannten Anwendungen vorzuschlagen. |AuditIfNotExists, Disabled |[3.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Security%20Center/ASC_AdaptiveApplicationControls_Audit.json) |
