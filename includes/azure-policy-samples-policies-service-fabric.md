---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 02/10/2020
ms.author: dacoulte
ms.openlocfilehash: 0177f1ffbc21cf5508b86a9c0f3c625fead34faf
ms.sourcegitcommit: bdf31d87bddd04382effbc36e0c465235d7a2947
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/12/2020
ms.locfileid: "77170368"
---
|Name |BESCHREIBUNG |Auswirkungen |Version |
|---|---|---|---|
|[Service Fabric Cluster sollten die Eigenschaft ClusterProtectionLevel auf EncryptAndSign setzen](https://github.com/Azure/azure-policy/blob/masterbuilt-in-policies/policyDefinitions/Service%20Fabric/ServiceFabric_AuditClusterProtectionLevel_Audit.json) |Service Fabric bietet drei Schutzebenen („None“, „Sign“ und „EncryptAndSign“) für die Kommunikation zwischen zwei Knoten unter Verwendung eines primären Clusterzertifikats. Legen Sie die Schutzebene fest, um sicherzustellen, dass alle zwischen Knoten übertragenen Nachrichten verschlüsselt und digital signiert werden. |Audit, Disabled |1.0.0 |
|[Service Fabric-Cluster sollten nur Azure Active Directory für die Clientauthentifizierung verwenden](https://github.com/Azure/azure-policy/blob/masterbuilt-in-policies/policyDefinitions/Service%20Fabric/ServiceFabric_AuditADAuth_Audit.json) |Hiermit wird überwacht, ob die Clientauthentifizierung in Service Fabric ausschließlich über Azure Active Directory erfolgt. |Audit, Disabled |1.0.0 |
