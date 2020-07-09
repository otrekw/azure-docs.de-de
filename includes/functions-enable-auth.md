---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 04/21/2020
ms.author: glenga
ms.openlocfilehash: 380878fedaa2f7ea6160c3c4801c8dece6e1a9ff
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2020
ms.locfileid: "83648961"
---
#### <a name="enable-app-service-authenticationauthorization"></a>Aktivieren der App Service-Authentifizierung/-Autorisierung

Die App Service-Plattform ermöglicht die Verwendung von Azure Active Directory (AAD) sowie von verschiedenen anderen Identitätsanbietern zum Authentifizieren von Clients. Sie können diese Strategie zum Implementieren von benutzerdefinierten Autorisierungsregeln für Ihre Funktionen sowie Benutzerinformationen aus Ihrem Funktionscode verwenden. Weitere Informationen finden Sie unter [Authentifizierung und Autorisierung in Azure App Service](../articles/app-service/overview-authentication-authorization.md) und [Arbeiten mit Clientidentitäten](../articles/azure-functions/functions-bindings-http-webhook-trigger.md#working-with-client-identities).

#### <a name="use-azure-api-management-apim-to-authenticate-requests"></a>Verwenden von Azure API Management (APIM) zum Authentifizieren von Anforderungen

APIM stellt verschiedene API-Sicherheitsoptionen für eingehende Anforderungen bereit. Weitere Informationen hierzu finden Sie unter [API Management-Authentifizierungsrichtlinien](../articles/api-management/api-management-authentication-policies.md). Wenn APIM vorhanden ist, können Sie Ihre Funktions-App so konfigurieren, dass Anforderungen nur von den IP-Adressen Ihrer APIM-Instanz angenommen werden. Weitere Informationen hierzu finden Sie unter [Einschränkungen für IP-Adressen](../articles/azure-functions/ip-addresses.md#ip-address-restrictions).
