---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 04/14/2021
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: db08f551331eaa9ed37368a36ce266efc38947ab
ms.sourcegitcommit: 3b5cb7fb84a427aee5b15fb96b89ec213a6536c2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/14/2021
ms.locfileid: "107498624"
---
|Name<br /><sub>(Azure-Portal)</sub> |Beschreibung |Auswirkungen |Version<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[Azure Data Explorer-Verschlüsselung ruhender Daten muss einen kundenseitig verwalteten Schlüssel verwenden](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F81e74cea-30fd-40d5-802f-d72103c2aaaa) |Das Aktivieren der Verschlüsselung ruhender Daten mit einem kundenseitig verwalteten Schlüssel in Ihrem Azure Data Explorer-Cluster bietet zusätzliche Kontrolle über den Schlüssel, der von der Verschlüsselung ruhender Schlüssel verwendet wird. Dieses Feature ist häufig für Kunden mit speziellen Complianceanforderungen relevant und erfordert eine Key Vault-Instanz zur Verwaltung der Schlüssel. |Audit, Deny, Disabled |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Azure%20Data%20Explorer/ADX_CMK.json) |
|[Datenträgerverschlüsselung muss in Azure Data Explorer aktiviert sein](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Ff4b53539-8df9-40e4-86c6-6b607703bd4e) |Durch das Aktivieren der Datenträgerverschlüsselung können Sie Ihre Daten schützen, um die Sicherheits- und Konformitätsverpflichtungen Ihrer Organisation zu erfüllen. |Audit, Deny, Disabled |[2.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Azure%20Data%20Explorer/ADX_disk_encrypted.json) |
|[Doppelte Verschlüsselung muss für Azure Data Explorer aktiviert sein](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fec068d99-e9c7-401f-8cef-5bdde4e6ccf1) |Durch das Aktivieren der doppelten Verschlüsselung können Sie Ihre Daten schützen, um die Sicherheits- und Konformitätsverpflichtungen Ihrer Organisation zu erfüllen. Wenn die doppelte Verschlüsselung aktiviert ist, werden Daten im Speicherkonto zweimal – einmal auf Dienstebene und einmal auf Infrastrukturebene – mit zwei unterschiedlichen Verschlüsselungsalgorithmen und zwei verschiedenen Schlüsseln verschlüsselt. |Audit, Deny, Disabled |[2.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Azure%20Data%20Explorer/ADX_doubleEncryption.json) |
|[VNET-Einschleusung muss für Azure Data Explorer aktiviert sein](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F9ad2fd1f-b25f-47a2-aa01-1a5a779e6413) |Schützen Sie Ihren Netzwerkperimeter durch VNET-Einschleusung. So können Sie Regeln von Netzwerksicherheitsgruppen durchsetzen, lokale Verbindungen herstellen und Ihre Datenverbindungsquellen mit Dienstendpunkten absichern. |Audit, Deny, Disabled |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Azure%20Data%20Explorer/ADX_VNET_configured.json) |
