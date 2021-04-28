---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 04/21/2021
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: 6c54ddf232d16b0555ec8e1f5fee57e1f597488f
ms.sourcegitcommit: 2aeb2c41fd22a02552ff871479124b567fa4463c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/22/2021
ms.locfileid: "107866616"
---
|Name<br /><sub>(Azure-Portal)</sub> |BESCHREIBUNG |Auswirkungen |Version<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[API Management-Dienst muss eine SKU mit Unterstützung virtueller Netzwerke verwenden](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F73ef9241-5d81-4cd4-b483-8443d1730fe5) |Für unterstützte API Management-SKUs führt die Dienstbereitstellung in einem virtuellen Netzwerk dazu, dass erweiterte Netzwerk- und Sicherheitsfeatures für API Management freigeschaltet werden, durch die Sie mehr Kontrolle über Ihre Netzwerksicherheitskonfiguration erhalten. Weitere Informationen finden Sie unter [https://aka.ms/apimvnet](https://aka.ms/apimvnet). |Audit, Deny, Disabled |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/API%20Management/ApiManagement_AllowedVNETSkus_AuditDeny.json) |
|[API Management-Dienste müssen ein virtuelles Netzwerk verwenden](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fef619a2c-cc4d-4d03-b2ba-8c94a834d85b) |Die Azure Virtual Network-Bereitstellung bietet verbesserte Sicherheit und Isolation und ermöglicht das Platzieren Ihres API Management-Diensts in einem Netzwerk ohne Internetrouting, für das Sie den Zugriff steuern. Diese Netzwerke können dann durch verschiedene VPN-Technologien mit Ihren lokalen Netzwerken verbunden werden, was den Zugriff auf Ihre Back-End-Dienste innerhalb des Netzwerks und/oder lokal ermöglicht. Das Entwicklerportal und das API-Gateway können so konfiguriert werden, dass darauf entweder über das Internet oder nur vom virtuellen Netzwerk aus zugegriffen werden kann. |Audit, Disabled |[1.0.1](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/API%20Management/ApiManagement_VNETEnabled_Audit.json) |
