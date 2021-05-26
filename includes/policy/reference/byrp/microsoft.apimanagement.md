---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 05/14/2021
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: 5cab9c01cd8f4d381c175cd7e5f861008c888623
ms.sourcegitcommit: 17345cc21e7b14e3e31cbf920f191875bf3c5914
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2021
ms.locfileid: "110094226"
---
|Name<br /><sub>(Azure-Portal)</sub> |BESCHREIBUNG |Auswirkungen |Version<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[API Management-Dienst muss eine SKU mit Unterstützung virtueller Netzwerke verwenden](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F73ef9241-5d81-4cd4-b483-8443d1730fe5) |Für unterstützte API Management-SKUs führt die Dienstbereitstellung in einem virtuellen Netzwerk dazu, dass erweiterte Netzwerk- und Sicherheitsfeatures für API Management freigeschaltet werden, durch die Sie mehr Kontrolle über Ihre Netzwerksicherheitskonfiguration erhalten. Weitere Informationen finden Sie unter [https://aka.ms/apimvnet](https://aka.ms/apimvnet). |Audit, Deny, Disabled |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/API%20Management/ApiManagement_AllowedVNETSkus_AuditDeny.json) |
|[API Management-Dienste müssen ein virtuelles Netzwerk verwenden](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fef619a2c-cc4d-4d03-b2ba-8c94a834d85b) |Die Azure Virtual Network-Bereitstellung bietet verbesserte Sicherheit und Isolation und ermöglicht das Platzieren Ihres API Management-Diensts in einem Netzwerk ohne Internetrouting, für das Sie den Zugriff steuern. Diese Netzwerke können dann durch verschiedene VPN-Technologien mit Ihren lokalen Netzwerken verbunden werden, was den Zugriff auf Ihre Back-End-Dienste innerhalb des Netzwerks und/oder lokal ermöglicht. Das Entwicklerportal und das API-Gateway können so konfiguriert werden, dass darauf entweder über das Internet oder nur vom virtuellen Netzwerk aus zugegriffen werden kann. |Audit, Disabled |[1.0.1](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/API%20Management/ApiManagement_VNETEnabled_Audit.json) |
