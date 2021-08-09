---
title: Benutzerdefinierter Teams-Endpunkt
titleSuffix: An Azure Communication Services concept document
description: Erstellen eines benutzerdefinierten Teams-Endpunkts
author: tomaschladek
manager: nmurav
services: azure-communication-services
ms.author: tchladek
ms.date: 06/30/2021
ms.topic: overview
ms.service: azure-communication-services
ms.openlocfilehash: acbd6a332d74a9b244f34230ac4b4eb591a1ab2d
ms.sourcegitcommit: 98308c4b775a049a4a035ccf60c8b163f86f04ca
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/30/2021
ms.locfileid: "113108061"
---
# <a name="custom-teams-endpoint"></a>Benutzerdefinierter Teams-Endpunkt

> [!IMPORTANT]
> Wenn Sie einen benutzerdefinierten Teams-Endpunkt aktivieren/deaktivieren möchten, füllen Sie [dieses Formular](https://forms.office.com/r/B8p5KqCH19) aus.

Sie können mit Azure Communication Services benutzerdefinierte Teams-Endpunkte für die Kommunikation mit dem Microsoft Teams-Client oder anderen benutzerdefinierten Teams-Endpunkten erstellen. Über einen benutzerdefinierten Teams-Endpunkt können Sie die Sprach-, Video-, Chat- und Bildschirmfreigabefunktionen für Teams-Benutzer anpassen.

Sie können das Azure Communication Services-Identitäts-SDK verwenden, um AAD-Benutzertoken gegen Teams-Zugriffstoken auszutauschen. In den folgenden Diagrammen wird ein Anwendungsfall mit mehreren Mandanten gezeigt, bei dem Fabrikam Kunde des Unternehmens Contoso ist.

## <a name="calling"></a>Aufrufen 

Sprach-, Video- und Bildschirmfreigabefunktionen werden über Anruf-SDKs von Azure Communication Services bereitgestellt. Das folgende Diagramm zeigt eine Übersicht über den Prozess, den Sie beim Integrieren Ihrer Anruffunktionen mit benutzerdefinierten Teams-Endpunkte ausführen.

![Prozess zum Aktivieren der Anruffunktion für einen benutzerdefinierten Teams-Endpunkt](./media/teams-identities/teams-identity-calling-overview.png)

## <a name="chat"></a>Chat

Sie können benutzerdefinierte Teams-Endpunkte auch dazu verwenden, Chatfunktionen optional mit Graph-APIs zu integrieren. Weitere Informationen zur Graph-API finden Sie in der [Dokumentation](https://docs.microsoft.com/graph/api/channel-post-messages). 


![Prozess zum Aktivieren der Chatfunktion für einen benutzerdefinierten Teams-Endpunkt](./media/teams-identities/teams-identity-chat-overview.png)

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Ausgeben von Teams-Zugriffstoken](../quickstarts/manage-teams-identity.md)

Die folgenden Dokumente könnten Sie auch interessieren:

- Informationen zur [Teams-Interoperabilität](./teams-interop.md)
