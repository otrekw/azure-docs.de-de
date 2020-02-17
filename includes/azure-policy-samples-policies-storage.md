---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 02/10/2020
ms.author: dacoulte
ms.openlocfilehash: 7aa6abb1a5fa9c969ca5e6d0730bed3b461fe81b
ms.sourcegitcommit: bdf31d87bddd04382effbc36e0c465235d7a2947
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/12/2020
ms.locfileid: "77170288"
---
|Name |BESCHREIBUNG |Auswirkungen |Version |
|---|---|---|---|
|[Zulässige Speicherkonten-SKUs](https://github.com/Azure/azure-policy/blob/masterbuilt-in-policies/policyDefinitions/Storage/AllowedStorageSkus_Deny.json) |Über diese Richtlinie können Sie einen Satz mit Speicherkonto-SKUs angeben, die Ihre Organisation bereitstellen kann. |Deny |1.0.0 |
|[Nicht eingeschränkten Netzwerkzugriff auf Speicherkonten überwachen](https://github.com/Azure/azure-policy/blob/masterbuilt-in-policies/policyDefinitions/Storage/Storage_NetworkAcls_Audit.json) |Überwachen Sie uneingeschränkten Netzwerkzugriff in den Firewalleinstellungen Ihres Speicherkontos. Konfigurieren Sie stattdessen Netzwerkregeln, sodass nur Anwendungen aus zulässigen Netzwerken auf ein Speicherkonto zugreifen können. Um Verbindungen von bestimmten Internetclients oder lokalen Clients zuzulassen, kann Zugriff für Datenverkehr aus bestimmten virtuellen Azure-Netzwerken oder an IP-Adressbereiche im öffentlichen Internet gewährt werden. |Audit, Disabled |1.0.0 |
|[Advanced Threat Protection für Speicherkonten bereitstellen](https://github.com/Azure/azure-policy/blob/masterbuilt-in-policies/policyDefinitions/Storage/StorageAdvancedThreatProtection_Deploy.json) |Diese Richtlinie aktiviert Advanced Threat Protection für Speicherkonten. |DeployIfNotExists, Disabled |1.0.0 |
|[Georedundanter Speicher muss für Speicherkonten aktiviert sein](https://github.com/Azure/azure-policy/blob/masterbuilt-in-policies/policyDefinitions/Storage/GeoRedundant_StorageAccounts_Audit.json) |Diese Richtlinie überwacht alle Speicherkonten mit nicht aktiviertem georedundantem Speicher. |Audit, Disabled |1.0.0 |
|[Sichere Übertragung in Speicherkonten sollte aktiviert werden](https://github.com/Azure/azure-policy/blob/masterbuilt-in-policies/policyDefinitions/Storage/Storage_AuditForHTTPSEnabled_Audit.json) |Hiermit wird die Anforderung sicherer Übertragungen in Ihrem Speicherkonto überwacht. Sichere Übertragung ist eine Option, die erzwingt, dass Ihr Storage-Konto nur Anforderungen von sicheren Verbindungen (HTTPS) akzeptiert. Durch die Verwendung von HTTPS wird eine Authentifizierung zwischen dem Server und dem Dienst sichergestellt, und die übertragenen Daten werden vor Angriffen auf Netzwerkebene geschützt, z. B. Man-in-the-Middle-Angriffe, Abhörangriffe und Session Hijacking. |Audit, Deny, Disabled |1.0.0 |
|[Speicherkonten sollten Zugriff von vertrauenswürdigen Microsoft-Diensten zulassen](https://github.com/Azure/azure-policy/blob/masterbuilt-in-policies/policyDefinitions/Storage/StorageAccess_TrustedMicrosoftServices_Audit.json) |Einige Microsoft-Dienste, die mit Speicherkonten interagieren, agieren von Netzwerken aus, denen nicht mithilfe von Netzwerkregeln Zugriff gewährt werden kann. Lassen Sie für vertrauenswürdige Microsoft-Dienste die Umgehung der Netzwerkregeln zu, damit solche Dienste ordnungsgemäß funktionieren. Diese Dienste verwenden dann eine strenge Authentifizierung, um auf das Speicherkonto zuzugreifen. |Audit, Deny, Disabled |1.0.0 |
|[Speicherkonten sollten zu neuen Azure Resource Manager-Ressourcen migriert werden](https://github.com/Azure/azure-policy/blob/masterbuilt-in-policies/policyDefinitions/Storage/Classic_AuditForClassicStorages_Audit.json) |Verwenden Sie den neuen Azure Resource Manager für Ihre Speicherkonten, um von den folgenden Sicherheitsverbesserungen zu profitieren: strengere Zugriffssteuerung (RBAC), bessere Überwachung, ARM-basierte Bereitstellung und Governance, Zugriff auf verwaltete Identitäten, Zugriff auf Schlüsseltresore für Geheimnisse, Azure AD-basierte Authentifizierung und Unterstützung für Tags und Ressourcengruppen für eine einfachere Sicherheitsverwaltung. |Audit, Deny, Disabled |1.0.0 |
