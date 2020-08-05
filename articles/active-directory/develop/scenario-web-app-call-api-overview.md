---
title: Erstellen einer Web-App, die Web-APIs aufruft – Microsoft Identity Platform | Azure
description: Erfahren Sie, wie Sie eine Web-App erstellen, die Web-APIs aufruft (Übersicht)
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 07/14/2020
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 0c4bef25a0be12c0335a82345ec998274f1faa67
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2020
ms.locfileid: "87058408"
---
# <a name="scenario-a-web-app-that-calls-web-apis"></a>Szenario: Web-App, die Web-APIs aufruft

Erfahren Sie, wie Sie eine Web-App erstellen, die Benutzer bei Microsoft Identity Platform anmeldet und dann Web-APIs im Namen des angemeldeten Benutzers aufruft.

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [Prerequisites](../../../includes/active-directory-develop-scenarios-prerequisites.md)]

In diesem Szenario wird angenommen, dass Sie mit dem folgenden Szenario vertraut sind:

> [!div class="nextstepaction"]
> [Web-App, die Benutzer anmeldet](scenario-web-app-sign-user-overview.md)

## <a name="overview"></a>Übersicht

Sie fügen eine Authentifizierung zu Ihrer Web-App hinzu, damit diese Benutzer anmelden und eine Web-API im Namen des angemeldeten Benutzers aufrufen kann.

![Web-App, die Web-APIs aufruft](./media/scenario-webapp/web-app.svg)

Web-Apps, die Web-APIs anrufen, sind vertrauliche Clientanwendungen.
Daher registrieren sie ein Geheimnis (Anwendungskennwort oder Zertifikat) bei Azure Active Directory (Azure AD). Dieses Geheimnis wird während des Aufrufs von Azure AD zum Abrufen eines Tokens übergeben.

## <a name="specifics"></a>Besonderheiten

> [!NOTE]
> Das Hinzufügen der Anmeldung zu einer Web-App ist eine Maßnahme zum Schutz der Web-App. Dieser Schutz wird durch die Verwendung von *Middleware*-Bibliotheken erreicht, nicht mit MSAL (Microsoft Authentication Library). Im vorherigen Szenario [Web-App, die Benutzer anmeldet](scenario-web-app-sign-user-overview.md) wurde dieses Thema behandelt.
>
> In diesem Szenario wird das Aufrufen von Web-APIs von einer Web-App behandelt. Sie müssen Zugriffstoken für diese Web-APIs abrufen. Sie verwenden MSAL-Bibliotheken zum Abrufen dieser Token.

Die Entwicklung für dieses Szenario umfasst die folgenden speziellen Aufgaben:

- Bei der [Anwendungsregistrierung](scenario-web-app-call-api-app-registration.md) müssen Sie einen Antwort-URI, ein Geheimnis oder ein Zertifikat bereitstellen, das für Azure AD freigegeben werden soll. Wenn Sie Ihre App an mehreren Standorten bereitstellen, geben Sie für jeden Standort einen Antwort-URI an.
- Die [Anwendungskonfiguration](scenario-web-app-call-api-app-configuration.md) muss die Clientanmeldeinformationen bereitstellen, die bei der Anwendungsregistrierung für Azure AD freigegeben wurden.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Web-App, die Web-APIs aufruft: App-Registrierung](scenario-web-app-call-api-app-registration.md)
