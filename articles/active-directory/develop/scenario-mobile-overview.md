---
title: Erstellen einer mobilen App, die Web-APIs aufruft | Azure
titleSuffix: Microsoft identity platform
description: Erfahren Sie, wie Sie eine mobile App erstellen, die Web-APIs aufruft (Übersicht)
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: jmprieur
ms.reviwer: brandwe
ms.custom: aaddev, identityplatformtop40
ms.openlocfilehash: 6675d67299091325fcc3e12572a906716bf5b88d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "77132415"
---
# <a name="scenario-mobile-application-that-calls-web-apis"></a>Szenario: Mobile App, die Web-APIs aufruft

Hier erfahren Sie, wie Sie eine mobile App erstellen, die Web-APIs aufruft.

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [Prerequisites](../../../includes/active-directory-develop-scenarios-prerequisites.md)]

## <a name="getting-started"></a>Erste Schritte

Erstellen Sie Ihre erste mobile Anwendung, und probieren Sie einen Schnellstart aus.

> [!div class="nextstepaction"]
> [Schnellstart: Abrufen eines Tokens und Aufrufen der Microsoft Graph-API von einer Android-App aus](./quickstart-v2-android.md)
>
> [Schnellstart: Abrufen eines Tokens und Aufrufen der Microsoft Graph-API von einer iOS-App aus](./quickstart-v2-ios.md)
>
> [Schnellstart: Abrufen eines Tokens und Aufrufen der Microsoft Graph-API von einer Xamarin.iOS- und Xamarin.Android-App aus](https://github.com/Azure-Samples/active-directory-xamarin-native-v2)

## <a name="overview"></a>Übersicht

Eine personalisierte und übersichtliche Benutzeroberfläche ist für mobile Apps äußerst wichtig.  Microsoft Identity Platform ermöglicht Entwicklern von mobilen Apps, diese Oberfläche für iOS- und Android-Benutzer zu schaffen. Ihre Anwendung kann Benutzer von Azure Active Directory (Azure AD), Benutzer eines persönlichen Microsoft-Kontos und Azure AD B2C-Benutzer anmelden. Sie kann außerdem Token erwerben, um in ihrem Auftrag eine Web-API aufzurufen. Zum Implementieren dieser Abläufe verwenden wir die Microsoft-Authentifizierungsbibliothek (Microsoft Authentication Library, MSAL). MSAL implementiert den [OAuth2.0-Autorisierungscodeablauf](v2-oauth2-auth-code-flow.md), den Branchenstandard.

![Daemon-Apps](./media/scenarios/mobile-app.svg)

Aspekte für mobile Apps:

- **Benutzerfreundlichkeit ist der Schlüssel:** Ermöglichen Sie es Benutzern, den Wert Ihrer App zu erkennen, bevor Sie sie auffordern, sich anzumelden. Fordern Sie nur die erforderlichen Berechtigungen an.
- **Unterstützen aller Benutzerkonfigurationen:** Viele geschäftliche Benutzer mobiler Geräte müssen Richtlinien für den bedingten Zugriff und Gerätekonformitätsrichtlinien befolgen. Stellen Sie sicher, dass diese wichtigen Szenarios unterstützt werden.
- **Implementieren von einmaligem Anmelden (SSO):** Mit MSAL und Microsoft Identity Platform können Sie einmaliges Anmelden (SSO) über den Gerätebrowser oder Microsoft Authenticator (und das Intune-Unternehmensportal unter Android) aktivieren.

## <a name="specifics"></a>Besonderheiten

Berücksichtigen Sie die folgenden Aspekte, wenn Sie eine mobile App auf Microsoft Identity Platform erstellen:

- Abhängig von der Plattform sind möglicherweise bei der ersten Anmeldung Benutzerinteraktionen erforderlich. Beispielsweise müssen Apps unter iOS eine Benutzeraktion anzeigen, wenn sie zum ersten Mal einmaliges Anmelden (Single Sign-On, SSO) über Microsoft Authenticator (und das Intune-Unternehmensportal unter Android) verwenden.
- Unter iOS und Android verwendet MSAL möglicherweise einen externen Browser zum Anmelden von Benutzern. Der externe Browser wird möglicherweise über Ihrer App angezeigt. Sie können die Konfiguration anpassen, sodass stattdessen In-App-Webansichten verwendet werden.
- Verwenden Sie niemals einen geheimen Schlüssel in einer mobilen Anwendung. In diesen Anwendungen sind Geheimnisse für alle Benutzer zugänglich.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [App-Registrierung](scenario-mobile-app-registration.md)
