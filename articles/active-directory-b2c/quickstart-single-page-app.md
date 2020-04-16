---
title: 'Schnellstart: Einrichten der Anmeldung für eine Single-Page-Webanwendung (SPA)'
titleSuffix: Azure AD B2C
description: In dieser Schnellstartanleitung führen Sie eine Single-Page-Webanwendung aus, bei der die Kontoanmeldung über Azure Active Directory B2C erfolgt.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: quickstart
ms.date: 04/04/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 0a754873aeafe8d4e7b48d49647469874ff40f7e
ms.sourcegitcommit: 2d7910337e66bbf4bd8ad47390c625f13551510b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/08/2020
ms.locfileid: "80875874"
---
# <a name="quickstart-set-up-sign-in-for-a-single-page-app-using-azure-active-directory-b2c"></a>Schnellstart: Einrichten der Anmeldung für eine einseitige App mit Azure Active Directory B2C

Azure Active Directory B2C (Azure AD B2C) ermöglicht die Cloudidentitätsverwaltung zum Schützen Ihrer Anwendung, Ihres Unternehmens und Ihrer Kunden. Mit Azure AD B2C können sich Ihre Anwendungen über offene Standardprotokolle bei Konten für soziale Netzwerke und bei Unternehmenskonten authentifizieren. In dieser Schnellstartanleitung verwenden Sie eine Single-Page-Anwendung, um sich unter Verwendung eines sozialen Netzwerks als Identitätsanbieter anzumelden und eine per Azure AD B2C geschützte Web-API aufzurufen.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Voraussetzungen

- [Visual Studio Code](https://code.visualstudio.com/)
- [Node.js](https://nodejs.org/en/download/)
- Ein Social Media-Konto von Facebook, Google oder Microsoft
- Codebeispiel von GitHub: [active-directory-b2c-javascript-msal-singlepageapp](https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp)

    Sie können [das ZIP-Archiv herunterladen](https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp/archive/master.zip) oder das Repository klonen:

    ```console
    git clone https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp.git
    ```

## <a name="run-the-application"></a>Ausführen der Anwendung

1. Starten Sie den Server, indem Sie die folgenden Befehle über die Node.js-Eingabeaufforderung ausführen:

    ```console
    cd active-directory-b2c-javascript-msal-singlepageapp
    npm install && npm update
    npm start
    ```

    Der von *server.js* gestartete Server zeigt den Port an, an dem gelauscht wird:

    ```console
    Listening on port 6420...
    ```

1. Navigieren Sie zur URL der Anwendung. Beispiel: `http://localhost:6420`.

    ![Beispiel-App für Singe-Page-Anwendung, die im Browser angezeigt wird](./media/quickstart-single-page-app/sample-app-spa.png)

## <a name="sign-in-using-your-account"></a>Anmelden mit Ihrem Konto

1. Wählen Sie **Anmelden** aus, um die User Journey zu starten.
1. Azure AD B2C zeigt für die Beispielwebanwendung eine Anmeldeseite für ein fiktives Unternehmen namens „Fabrikam“ an. Um sich mit einem Social Media-Konto als Identitätsanbieter zu registrieren, wählen Sie die Schaltfläche des Identitätsanbieters aus, den Sie verwenden möchten.

    ![Anmelde-oder Registrierungsseite mit Schaltflächen für die Auswahl von Identitätsanbietern](./media/quickstart-single-page-app/sign-in-or-sign-up-spa.png)

    Sie authentifizieren sich mit den Anmeldeinformationen Ihres Social Media-Kontos und autorisieren die Anwendung dazu, Informationen aus Ihrem Social Media-Konto zu lesen. Wenn Sie der Anwendung Zugriff auf diese gewähren, kann die Anwendung Profilinformationen aus dem Social Media-Konto abrufen, z.B. Ihren Namen und Ihre Stadt.

1. Schließen Sie den Anmeldevorgang für den Identitätsanbieter ab.

## <a name="access-a-protected-api-resource"></a>Zugreifen auf eine geschützte API-Ressource

Wählen Sie **API aufrufen** aus, damit die Web-API Ihren Anzeigenamen als JSON-Objekt zurückgibt.

![Beispielanwendung im Browser mit der Antwort der Web-API](./media/quickstart-single-page-app/call-api-spa.png)

Die Single-Page-Beispielanwendung enthält ein Zugriffstoken in der Anforderung, die an die geschützte Web-API-Ressource gesendet wird.

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie mithilfe eines Beispiels für eine Single-Page-Webanwendung folgende Aktionen durchgeführt:

- Anmelden mit einem sozialen Netzwerk als Identitätsanbieter
- Erstellen eines Azure AD B2C-Benutzerkontos (automatische Erstellung bei der Anmeldung)
- Aufrufen einer durch Azure AD B2C geschützten Web-API

Machen Sie sich als Nächstes mit der Erstellung Ihres eigenen Azure AD B2C-Mandanten vertraut.

> [!div class="nextstepaction"]
> [Erstellen eines Azure Active Directory B2C-Mandanten im Azure-Portal](tutorial-create-tenant.md)
