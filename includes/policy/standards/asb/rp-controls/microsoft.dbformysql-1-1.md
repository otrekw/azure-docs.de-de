---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 05/14/2021
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: cd2a4fdfc626b6324ff593bdf868d8559a0a5d45
ms.sourcegitcommit: 17345cc21e7b14e3e31cbf920f191875bf3c5914
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2021
ms.locfileid: "110166012"
---
|Name<br /><sub>(Azure-Portal)</sub> |BESCHREIBUNG |Auswirkungen |Version<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[Privater Endpunkt muss für MySQL-Server aktiviert sein](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F7595c971-233d-4bcf-bd18-596129188c49) |Private Endpunktverbindungen erzwingen eine sichere Kommunikation durch das Aktivieren privater Konnektivität mit Azure Database for MySQL. Konfigurieren Sie eine private Endpunktverbindung, um nur Zugriff auf Datenverkehr zu ermöglichen, der aus bekannten Netzwerken stammt, und Zugriff von allen anderen IP-Adressen, auch solchen in Azure, zu verhindern. |AuditIfNotExists, Disabled |[1.0.2](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/SQL/MySQL_EnablePrivateEndPoint_Audit.json) |
