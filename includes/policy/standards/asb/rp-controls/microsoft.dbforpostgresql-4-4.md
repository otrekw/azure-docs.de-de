---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 03/31/2021
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: 494cf5e7e16ae7aac516eac6c8f411173256361d
ms.sourcegitcommit: 99fc6ced979d780f773d73ec01bf651d18e89b93
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/31/2021
ms.locfileid: "107284306"
---
|Name<br /><sub>(Azure-Portal)</sub> |BESCHREIBUNG |Auswirkungen |Version<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[Erzwingen einer SSL-Verbindung muss für PostgreSQL-Datenbankserver aktiviert sein](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fd158790f-bfb0-486c-8631-2dc6b4e8e6af) |Azure Database for PostgreSQL unterstützt das Herstellen einer Verbindung zwischen Ihrem Azure Database for PostgreSQL-Server und Clientanwendungen unter Verwendung von Secure Sockets Layer (SSL). Durch das Erzwingen von SSL-Verbindungen zwischen Ihrem Datenbankserver und Ihren Clientanwendungen können Sie sich vor Man-in-the-Middle-Angriffen schützen, indem Sie den Datenstrom zwischen dem Server und Ihrer Anwendung verschlüsseln. Diese Konfiguration erzwingt, dass SSL für den Zugriff auf Ihren Datenbankserver immer aktiviert ist. |Audit, Disabled |[1.0.1](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/SQL/PostgreSQL_EnableSSL_Audit.json) |
