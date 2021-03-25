---
title: 'Microsoft Identity Plattform: Authentifizierungsbibliotheken | Azure'
description: Liste der mit der Microsoft Identity Platform kompatiblen Clientbibliotheken und Middleware. Verwenden Sie diese Bibliotheken, um Ihren Anwendungen Unterstützung für die Benutzeranmeldung (Authentifizierung) und den geschützten Web-API-Zugriff (Autorisierung) hinzuzufügen.
services: active-directory
author: mmacy
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: reference
ms.workload: identity
ms.date: 01/29/2021
ms.author: marsma
ms.reviewer: jmprieur, saeeda
ms.custom: aaddev
ms.openlocfilehash: 590e57d587c8e6e254811892b5c5e740b511c302
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "104690652"
---
# <a name="microsoft-identity-platform-authentication-libraries"></a>Microsoft Identity Plattform: Authentifizierungsbibliotheken

In den folgenden Tabellen wird die Unterstützung der Microsoft-Authentifizierungsbibliothek (Microsoft Authentication Library, MSAL) für mehrere Anwendungstypen aufgeführt. Sie enthält Links zum Quellcode der Bibliothek, den Speicherort des Pakets für das Projekt Ihrer App und Informationen dazu, ob die Bibliothek die Benutzeranmeldung (Authentifizierung), den Zugriff auf geschützte Web-APIs (Autorisierung) oder beides unterstützt.

Die Microsoft Identity Platform wurde von der OpenID Foundation als [Certified OpenID Provider](https://openid.net/certification/) zertifiziert. Wenn Sie lieber eine andere Bibliothek als die Microsoft-Authentifizierungsbibliothek (Microsoft Authentication Library, MSAL) oder eine andere von Microsoft unterstützte Bibliothek verwenden möchten, wählen Sie eine mit [zertifizierter OpenID Connect-Implementierung](https://openid.net/developers/certified/) aus.

Wenn Sie Ihre eigene Implementierung von [OAuth 2.0 oder OpenID Connect 1.0](active-directory-v2-protocols.md) auf Protokollebene von Hand schreiben möchten, achten Sie auf die Sicherheitsüberlegungen in den Spezifikationen der einzelnen Standards, und befolgen Sie eine SDL-Methodik (Software Development Lifecycle) wie z. B. [Microsoft SDL][Microsoft-SDL].

## <a name="single-page-application-spa"></a>Single-Page-Webanwendung (SPA)

Eine Single-Page-Webanwendung wird vollständig auf der Browseroberfläche ausgeführt und ruft die Seitendaten (HTML, CSS und JavaScript) dynamisch oder zur Ladezeit der Anwendung ab. Sie kann für die Interaktion mit Back-End-Datenquellen Web-APIs aufrufen.

Da der Code einer Single-Page-Webanwendung vollständig im Browser ausgeführt wird, gilt er als *öffentlicher Client*, der Geheimnisse nicht sicher speichern kann.

[!INCLUDE [active-directory-develop-libraries-spa](../../../includes/active-directory-develop-libraries-spa.md)]

## <a name="web-application"></a>Webanwendung

Eine Webanwendung führt Code auf einem Server aus, der HTML-, CSS- und JavaScript-Code generiert und zum Rendern an den Webbrowser eines Benutzers sendet. Die Identität des Benutzers wird zwischen dem Browser des Benutzers (dem Front-End) und dem Webserver (dem Back-End) als Sitzung verwaltet.

Da der Code einer Webanwendung auf dem Webserver ausgeführt wird, gilt er als *vertraulicher Client*, der Geheimnisse sicher speichern kann.

[!INCLUDE [active-directory-develop-libraries-webapp](../../../includes/active-directory-develop-libraries-webapp.md)]

## <a name="desktop-application"></a>Desktopanwendung

Eine Desktopanwendung ist in der Regel (kompilierter) Binärcode, der eine Benutzeroberfläche bereitstellt und auf dem Desktop der Benutzer ausgeführt werden soll.

Da eine Desktopanwendung auf dem Desktop des Benutzers ausgeführt wird, gilt sie als *öffentlicher Client*, der Geheimnisse nicht sicher speichern kann.

[!INCLUDE [active-directory-develop-libraries-desktop](../../../includes/active-directory-develop-libraries-desktop.md)]

## <a name="mobile-application"></a>Mobile Anwendung

Eine mobile Anwendung ist in der Regel (kompilierter) Binärcode, der eine Benutzeroberfläche bereitstellt und auf dem mobilen Gerät der Benutzer ausgeführt werden soll.

Da eine mobile Anwendung auf dem mobilen Gerät des Benutzers ausgeführt wird, gilt sie als *öffentlicher Client*, der Geheimnisse nicht sicher speichern kann.

[!INCLUDE [active-directory-develop-libraries-mobile](../../../includes/active-directory-develop-libraries-mobile.md)]

## <a name="service--daemon"></a>Dienst/Daemon

Dienste und Daemons werden häufig für die Kommunikation zwischen Servern und andere unbeaufsichtigte Kommunikationsvorgänge (manchmal auch als *monitorlos* oder „headless“ bezeichnet) verwendet. Da dabei kein Benutzer Anmeldeinformationen eingeben oder den Zugriff auf Ressourcen genehmigen kann, authentifizieren sich diese Anwendungen nicht als Benutzer, sondern als sie selbst, wenn sie autorisierten Zugriff auf die Ressourcen einer Web-API anfordern.

Ein Dienst oder Daemon, der auf einem Server ausgeführt wird, gilt als *vertraulicher Client*, der seine Geheimnisse sicher speichern kann.

[!INCLUDE [active-directory-develop-libraries-daemon](../../../includes/active-directory-develop-libraries-daemon.md)]

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Microsoft-Authentifizierungsbibliothek finden Sie in der [Übersicht über die Microsoft-Authentifizierungsbibliothek (MSAL)](msal-overview.md).

<!--Image references-->
[y]: ./media/common/yes.png
[n]: ./media/common/no.png

<!--Reference-style links -->
[AAD-App-Model-V2-Overview]: v2-overview.md
[Microsoft-SDL]: https://www.microsoft.com/securityengineering/sdl/
[preview-tos]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/
