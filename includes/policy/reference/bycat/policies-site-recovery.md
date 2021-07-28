---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 06/11/2021
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: df4af57a55acb0e89bf6155de9c6a5aded9f9a46
ms.sourcegitcommit: 942a1c6df387438acbeb6d8ca50a831847ecc6dc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/11/2021
ms.locfileid: "112019852"
---
|Name<br /><sub>(Azure-Portal)</sub> |Beschreibung |Auswirkungen |Version<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[\[Vorschau\]: Azure Recovery Services-Tresore für die Verwendung privater DNS-Zonen konfigurieren](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F942bd215-1a66-44be-af65-6a1c0318dbe2) |Verwenden Sie private DNS-Zonen, um die DNS-Auflösung für einen privaten Endpunkt außer Kraft zu setzen. Eine private DNS-Zone wird mit Ihrem virtuellen Netzwerk verknüpft, um Recovery Services-Tresore aufzulösen. Weitere Informationen finden Sie unter [https://aka.ms/privatednszone](https://aka.ms/privatednszone). |DeployIfNotExists, Disabled |[1.0.0-preview](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Site%20Recovery/SiteRecovery_PrivateDnsZones_DeployIfNotExist.json) |
|[\[Vorschau\]: Private Endpunkte für Azure Recovery Services-Tresore konfigurieren](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fe95a8a5c-0987-421f-84ab-df4d88ebf7d1) |Private Endpunkte verbinden Ihr virtuelles Netzwerk ohne eine öffentliche IP-Adresse an Quelle oder Ziel mit Azure-Diensten. Durch die Zuordnung privater Endpunkte zu Ihren Sitewiederherstellungsressourcen von Recovery Services-Tresoren können Sie das Risiko von Datenlecks reduzieren. Um private Links zu verwenden, muss die Identität des verwalteten Diensts dem Recovery Services-Tresor zugewiesen werden. Weitere Informationen zu privaten Verbindungen finden Sie unter [https://docs.microsoft.com/azure/site-recovery/azure-to-azure-how-to-enable-replication-private-endpoints](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-how-to-enable-replication-private-endpoints). |DeployIfNotExists, Disabled |[1.0.0-preview](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Site%20Recovery/SiteRecovery_PrivateEndpoints_DeployIfNotExist.json) |
|[\[Vorschau\]: Azure Recovery Services-Tresore müssen Private Link verwenden](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F11e3da8c-1d68-4392-badd-0ff3c43ab5b0) |Mit Azure Private Link können Sie Ihr virtuelles Netzwerk mit Azure-Diensten verbinden, ohne auf der Quelle oder auf dem Ziel eine öffentliche IP-Adresse zu verwenden. Die Private Link-Plattform verarbeitet die Konnektivität zwischen dem Consumer und den Diensten über das Azure-Backbone-Netzwerk. Durch das Zuordnen privater Endpunkte zu Azure Recovery Services-Tresoren wird das Risiko von Datenlecks verringert. Weitere Informationen zu privaten Links für Azure Site Recovery finden Sie unter: [https://aka.ms/HybridScenarios-PrivateLink](https://aka.ms/HybridScenarios-PrivateLink) und [https://aka.ms/AzureToAzure-PrivateLink](https://aka.ms/AzureToAzure-PrivateLink). |Audit, Disabled |[1.0.0-preview](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Site%20Recovery/RecoveryServices_SiteRecovery_PrivateEndpoint_Audit.json) |
