---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 08/13/2021
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: cd6c96d23d924df3a1d4f0f0e285b14ea1a11344
ms.sourcegitcommit: 2d412ea97cad0a2f66c434794429ea80da9d65aa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/14/2021
ms.locfileid: "122343046"
---
|Name<br /><sub>(Azure-Portal)</sub> |BESCHREIBUNG |Auswirkungen |Version<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[Azure Container Instances-Containergruppe muss in einem virtuellen Netzwerk bereitgestellt werden](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F8af8f826-edcb-4178-b35f-851ea6fea615) |Schützen Sie die Kommunikation zwischen Ihren Containern mit virtuellen Azure-Netzwerken. Wenn Sie ein virtuelles Netzwerk angeben, können Ressourcen innerhalb des virtuellen Netzwerks sicher und privat miteinander kommunizieren. |Audit, Disabled, Deny |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Container%20Instance/ContainerInstance_VNET_Audit.json) |
|[Azure Container Instances-Containergruppe muss einen kundenseitig verwalteten Schlüssel für die Verschlüsselung verwenden](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F0aa61e00-0a01-4a3c-9945-e93cffedf0e6) |Schützen Sie Ihre Container mithilfe von kundenseitig verwalteten Schlüsseln, um die Flexibilität zu erhöhen. Wenn Sie einen vom Kunden verwalteten Schlüssel angeben, wird dieser zum Schützen und Steuern des Zugriffs auf den Schlüssel verwendet, mit dem Ihre Daten verschlüsselt werden. Die Verwendung von kundenseitig verwalteten Schlüsseln bietet zusätzliche Funktionen zum Steuern der Rotation des Schlüsselverschlüsselungsschlüssels oder zum kryptografischen Löschen von Daten. |Audit, Disabled, Deny |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Container%20Instance/ContainerInstance_CMK_Audit.json) |
