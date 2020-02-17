---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 02/10/2020
ms.author: dacoulte
ms.openlocfilehash: 8319ec2bf3965ee30db3e7d4ba1346bd7a3dd7d4
ms.sourcegitcommit: bdf31d87bddd04382effbc36e0c465235d7a2947
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/12/2020
ms.locfileid: "77169900"
---
|Name |BESCHREIBUNG |Auswirkungen |Version |
|---|---|---|---|
|[Zulässige Speicherorte](https://github.com/Azure/azure-policy/blob/masterbuilt-in-policies/policyDefinitions/General/AllowedLocations_Deny.json) |Mit dieser Richtlinie können Sie die Speicherorte einschränken, die Ihre Organisation beim Bereitstellen von Ressourcen angeben kann. Wird zur Erzwingung Ihrer Geokonformitätsanforderungen verwendet. Schließt Ressourcengruppen, Microsoft.AzureActiveDirectory/b2c-Verzeichnisse und Ressourcen aus, die die Region „global“ verwenden. |deny |1.0.0 |
|[Zulässige Standorte für Ressourcengruppen](https://github.com/Azure/azure-policy/blob/masterbuilt-in-policies/policyDefinitions/General/ResourceGroupAllowedLocations_Deny.json) |Mit dieser Richtlinie können Sie die Standorte einschränken, an denen Ihr Unternehmen Ressourcengruppen erstellen kann. Wird zur Erzwingung Ihrer Geokonformitätsanforderungen verwendet. |deny |1.0.0 |
|[Zulässige Ressourcentypen](https://github.com/Azure/azure-policy/blob/masterbuilt-in-policies/policyDefinitions/General/AllowedResourceTypes_Deny.json) |Mit dieser Richtlinie können Sie die Ressourcentypen angeben, die Ihre Organisation bereitstellen kann. Diese Richtlinie gilt nur für Ressourcentypen, die „tags“ und „location“ unterstützen. Um alle Ressourcen einzuschränken, duplizieren Sie diese Richtlinie und ändern den Wert für „mode“ in „All“. |deny |1.0.0 |
|[Übereinstimmung des Standorts von Ressource und Ressourcengruppe überwachen](https://github.com/Azure/azure-policy/blob/masterbuilt-in-policies/policyDefinitions/General/ResourcesInResourceGroupLocation_Audit.json) |Hiermit wird überwacht, ob der Standort der Ressource mit dem Standort der Ressourcengruppe übereinstimmt. |Überwachung |1.0.0 |
|[Verwendung benutzerdefinierter RBAC-Regeln überwachen](https://github.com/Azure/azure-policy/blob/masterbuilt-in-policies/policyDefinitions/General/Subscription_AuditCustomRBACRoles_Audit.json) |Hiermit werden integrierte Rollen wie z.B. „Benutzer“, „Mitwirkender“ und „Leser“ anstelle benutzerdefinierter RBAC-Rollen überwacht, die fehleranfällig sind. Die Verwendung benutzerdefinierter Rollen wird als Ausnahme betrachtet und erfordert eine strenge Überprüfung und Bedrohungsmodellierung. |Audit, Disabled |1.0.0 |
|[Benutzerdefinierte Rollen für Abonnementbesitzer dürfen nicht vorhanden sein](https://github.com/Azure/azure-policy/blob/masterbuilt-in-policies/policyDefinitions/General/CustomSubscription_OwnerRole_Audit.json) |Mit dieser Richtlinie wird sichergestellt, dass keine benutzerdefinierten Abonnementbesitzerrollen vorhanden sind. |Audit, Disabled |1.0.0 |
|[Nicht zulässige Ressourcentypen](https://github.com/Azure/azure-policy/blob/masterbuilt-in-policies/policyDefinitions/General/InvalidResourceTypes_Deny.json) |Über diese Richtlinie können Sie die Ressourcentypen angeben, die Ihre Organisation nicht bereitstellen kann. |Verweigern |1.0.0 |
