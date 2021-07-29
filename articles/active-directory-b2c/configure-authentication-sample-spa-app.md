---
title: Konfigurieren der Authentifizierung in einer Single-Page-Beispielwebanwendung mit Azure Active Directory B2C
description: Hier finden Sie Informationen zum Verwenden von Azure Active Directory B2C zum Anmelden und Registrieren von Benutzern in einer Single-Page-Webanwendung.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 06/11/2021
ms.author: mimart
ms.subservice: B2C
ms.custom: b2c-support
ms.openlocfilehash: addf3870c22105a2ff42202e768d1e8cda4ffbde
ms.sourcegitcommit: 8651d19fca8c5f709cbb22bfcbe2fd4a1c8e429f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/14/2021
ms.locfileid: "112073003"
---
# <a name="configure-authentication-in-a-sample-single-page-application-using-azure-active-directory-b2c"></a>Konfigurieren der Authentifizierung in einer Single-Page-Beispielwebanwendung (SPA) mit Azure Active Directory B2C

In diesem Artikel wird anhand einer JavaScript-Single-Page-Beispielwebanwendung veranschaulicht, wie Sie Ihren Single-Page-Webanwendungen die Authentifizierung von Azure Active Directory B2C (Azure AD B2C) hinzufügen.

## <a name="overview"></a>Übersicht

OpenID Connect (OIDC) ist ein Authentifizierungsprotokoll auf Grundlage von OAuth 2.0, mit dem Benutzer sicher bei einer Anwendung angemeldet werden können. Für diese Single-Page-Beispielwebanwendung werden [MSAL.js](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-browser) und der OIDC PKCE-Flow verwendet. MSAL.js ist eine von Microsoft bereitgestellte Bibliothek, die das Hinzufügen von Authentifizierungs- und Autorisierungsunterstützung für Single-Page-Webanwendungen vereinfacht.

### <a name="sign-in-flow"></a>Anmeldeflow
Der Anmeldeflow umfasst die folgenden Schritte:

1. Der Benutzer navigiert zur Web-App und wählt **Anmelden** aus. 
1. Die App initiiert eine Authentifizierungsanforderung und leitet den Benutzer zu Azure AD B2C um.
1. Der Benutzer [registriert sich oder meldet sich an](add-sign-up-and-sign-in-policy.md), [setzt das Kennwort zurück](add-password-reset-policy.md) oder meldet sich mit einem [Social Media-Konto](add-identity-provider.md) an.
1. Nach erfolgreicher Anmeldung gibt Azure AD B2C ein ID-Token an die App zurück.
1. Die Single-Page-Webanwendung überprüft das ID-Token, liest die Ansprüche und ermöglicht es dem Benutzer wiederum, geschützte Ressourcen/APIs aufzurufen.

### <a name="app-registration-overview"></a>Übersicht über die App-Registrierung

Damit sich Ihre App mit Azure AD B2C anmelden und eine Web-API aufrufen kann, müssen Sie im Azure AD B2C-Verzeichnis zwei Anwendungen registrieren.  

- Durch die **Webanwendungsregistrierung** kann sich Ihre App mit Azure AD B2C anmelden. Bei der App-Registrierung geben Sie den *Umleitungs-URI* an. Der Umleitungs-URI ist der Endpunkt, an den der Benutzer umgeleitet wird, nachdem er sich mit Azure AD B2C authentifiziert hat. Beim App-Registrierungsprozess wird eine *Anwendungs-ID* generiert, die auch als *Client-ID* bezeichnet wird und Ihre App eindeutig identifiziert.

- Durch die **Web-API**-Registrierung kann Ihre App eine sichere Web-API aufrufen. Die Registrierung umfasst die Web-API-*Bereiche*. Bereiche bieten eine Möglichkeit, Berechtigungen für geschützte Ressourcen wie Ihre Web-API zu verwalten. Sie erteilen der Webanwendung Berechtigungen für die Bereiche der Web-API. Wenn ein Zugriffstoken angefordert wird, gibt Ihre App im Bereichsparameter der Anforderung die gewünschten Berechtigungen an.  

In den folgenden Diagrammen werden die App-Registrierungen und die Anwendungsarchitektur veranschaulicht.

![Web-App mit Web-API-Aufrufregistrierungen und Token](./media/configure-authentication-sample-spa-app/spa-app-with-api-architecture.png) 

### <a name="call-to-a-web-api"></a>Aufrufen einer Web-API

[!INCLUDE [active-directory-b2c-app-integration-call-api](../../includes/active-directory-b2c-app-integration-call-api.md)]

### <a name="sign-out-flow"></a>Abmeldeflow

[!INCLUDE [active-directory-b2c-app-integration-sign-out-flow](../../includes/active-directory-b2c-app-integration-sign-out-flow.md)]

## <a name="prerequisites"></a>Voraussetzungen

Ein Computer, auf dem Folgendes ausgeführt wird:

* [Visual Studio Code](https://code.visualstudio.com/) oder ein anderer Code-Editor
* [Node.js-Runtime](https://nodejs.org/en/download/)

## <a name="step-1-configure-your-user-flow"></a>Schritt 1: Konfigurieren Ihres Benutzerflows

[!INCLUDE [active-directory-b2c-app-integration-add-user-flow](../../includes/active-directory-b2c-app-integration-add-user-flow.md)]

## <a name="step-2-register-your-spa-and-api"></a>Schritt 2: Registrieren Ihrer Single-Page-Webanwendung (SPA) und API

In diesem Schritt erstellen Sie die SPA- und Web-API-Anwendungsregistrierungen und geben die Bereiche Ihrer Web-API an.

### <a name="21-register-the-web-api-application"></a>2.1 Registrieren der Web-API-Anwendung

[!INCLUDE [active-directory-b2c-app-integration-register-api](../../includes/active-directory-b2c-app-integration-register-api.md)]

### <a name="22-configure-scopes"></a>2.2 Konfigurieren der Bereiche

[!INCLUDE [active-directory-b2c-app-integration-api-scopes](../../includes/active-directory-b2c-app-integration-api-scopes.md)]

### <a name="23-register-the-client-app"></a>2.3 Registrieren der Client-App

Führen Sie die folgenden Schritte aus, um die App-Registrierung zu erstellen:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Wählen Sie auf der Symbolleiste des Portals das Symbol **Verzeichnis und Abonnement** aus, und wählen Sie dann das Verzeichnis aus, das Ihren Azure AD B2C-Mandanten enthält.
1. Suchen Sie im Azure-Portal nach **Azure AD B2C**, und wählen Sie diese Option dann aus.
1. Wählen Sie **App-Registrierungen** aus, und wählen Sie dann **Registrierung einer neuen Anwendung** aus.
1. Geben Sie unter **Name** einen Namen für die Anwendung ein. Beispielsweise *MyApp*.
1. Wählen Sie unter **Unterstützte Kontotypen** die Option **Konten in einem beliebigen Identitätsanbieter oder Organisationsverzeichnis (zum Authentifizieren von Benutzern mit Benutzerflows)** aus. 
1. Wählen Sie unter **Umleitungs-URI** die Option **Single-Page-Anwendung (SPA)** aus, und geben Sie `http://localhost:6420` in das URL-Textfeld ein.
1. Aktivieren Sie unter **Berechtigungen** das Kontrollkästchen **Administratoreinwilligung für OpenID- und Offlinezugriffsberechtigungen erteilen**.
1. Wählen Sie **Registrieren**.

Aktivieren Sie als Nächstes den Flow zur impliziten Genehmigung:

1. Wählen Sie unter Verwalten die Option Authentifizierung aus.
1. Wählen Sie Neue Benutzeroberfläche ausprobieren aus (sofern die Option angezeigt wird).
1. Aktivieren Sie unter „Implizite Genehmigung“ das Kontrollkästchen „ID-Token“.
1. Wählen Sie „Speichern“ aus.

Notieren Sie sich die **Anwendungs-ID (Client)** , die Sie später beim Konfigurieren der Webanwendung verwenden.
    ![Abrufen Ihrer Anwendungs-ID](./media/configure-authentication-sample-web-app/get-azure-ad-b2c-app-id.png)  

### <a name="25-grant-permissions"></a>2.5 Erteilen von Berechtigungen

[!INCLUDE [active-directory-b2c-app-integration-grant-permissions](../../includes/active-directory-b2c-app-integration-grant-permissions.md)]

## <a name="step-3-get-the-spa-sample-code"></a>Schritt 3: Abrufen des SPA-Beispielcodes

In diesem Beispiel wird veranschaulicht, wie eine Single-Page-Webanwendung Azure AD B2C für die Benutzerregistrierung und -anmeldung sowie zum Aufrufen einer geschützten Web-API verwenden kann. Laden Sie das folgende Beispiel herunter:

  [Laden Sie eine ZIP-Datei herunter](https://github.com/Azure-Samples/ms-identity-b2c-javascript-spa/archive/main.zip), oder klonen Sie das Beispiel aus GitHub:

  ```
  git clone https://github.com/Azure-Samples/ms-identity-b2c-javascript-spa.git
  ```

### <a name="31-update-the-spa-sample"></a>3.1 Aktualisieren des SPA-Beispiels

Nachdem Sie das Beispiel abgerufen haben, können Sie den Code mit dem Namen Ihres Azure AD B2C-Mandanten und der in Schritt 2.3 notierten Anwendungs-ID von *MyApp* aktualisieren.

Öffnen Sie die Datei *authConfig.js* im Ordner *App*.
1. Suchen Sie im Objekt `msalConfig` die Zuweisung für `clientId`, und ersetzen Sie diese durch die **Anwendungs-ID (Client-ID)** , die Sie sich in Schritt 2.3 notiert haben.

Öffnen Sie die Datei `policies.js`.
1. Suchen Sie die Einträge unter `names`, und ersetzen Sie ihre Zuweisung durch die Namen der Benutzerflows, die Sie in einem früheren Schritt erstellt haben, etwa `b2c_1_susi`.
1. Suchen Sie die Einträge unter `authorities`, und ersetzen Sie sie nach Bedarf durch die Namen der Benutzerflows, die Sie in einem früheren Schritt erstellt haben, etwa `https://<your-tenant-name>.b2clogin.com/<your-tenant-name>.onmicrosoft.com/<your-sign-in-sign-up-policy>`.
1. Suchen Sie die Zuweisung für `authorityDomain`, und ersetzen Sie sie durch `<your-tenant-name>.b2clogin.com`.

Öffnen Sie die Datei `apiConfig.js`.
1. Suchen Sie die Zuweisung für `b2cScopes`, und ersetzen Sie die URL durch die Bereichs-URL, die Sie für die Web-API erstellt haben, z. B. `b2cScopes: ["https://<your-tenant-name>.onmicrosoft.com/tasks-api/tasks.read"]`.
1. Suchen Sie die Zuweisung für `webApi`, und ersetzen Sie die aktuelle URL durch `http://localhost:5000/tasks`.


Der sich ergebende Code sollte in etwa wie das folgende Beispiel aussehen:

*authConfig.js:*

```javascript
const msalConfig = {
  auth: {
    clientId: "<your-MyApp-application-ID>"
    authority: b2cPolicies.authorities.signUpSignIn.authority,
    knownAuthorities: [b2cPolicies.authorityDomain],
  },
  cache: {
    cacheLocation: "localStorage",
    storeAuthStateInCookie: true
  }
};

const loginRequest = {
  scopes: ["openid", "profile"],
};

const tokenRequest = {
  scopes: apiConfig.b2cScopes
};
```

*policies.js:*

```javascript
const b2cPolicies = {
    names: {
        signUpSignIn: "b2c_1_susi",
        forgotPassword: "b2c_1_reset",
        editProfile: "b2c_1_edit_profile"
    },
    authorities: {
        signUpSignIn: {
            authority: "https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com/b2c_1_susi",
        },
        forgotPassword: {
            authority: "https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com/b2c_1_reset",
        },
        editProfile: {
            authority: "https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com/b2c_1_edit_profile"
        }
    },
    authorityDomain: "your-tenant-name.b2clogin.com"
}
```

*apiConfig.js:*

```javascript
const apiConfig = {
  b2cScopes: ["https://your-tenant-name.onmicrosoft.com/tasks-api/tasks.read"],
  webApi: "http://localhost:5000/tasks"
};
```

## <a name="step-4-get-the-web-api-sample-code"></a>Schritt 4: Abrufen des Web-API-Beispielcodes

Nachdem Sie die Web-API registriert und die zugehörigen Bereiche definiert haben, können Sie den Web-API-Code für die Verwendung mit Ihrem Azure AD B2C-Mandanten konfigurieren. Laden Sie das folgende Beispiel herunter:

[Laden Sie ein \*ZIP-Archiv](https://github.com/Azure-Samples/active-directory-b2c-javascript-nodejs-webapi/archive/master.zip) herunter, oder klonen Sie das Beispiel-Web-API-Projekt von GitHub. Sie können auch direkt zum Projekt [Azure-Samples/active-directory-b2c-javascript-nodejs-webapi](https://github.com/Azure-Samples/active-directory-b2c-javascript-nodejs-webapi) auf GitHub navigieren.

```console
git clone https://github.com/Azure-Samples/active-directory-b2c-javascript-nodejs-webapi.git
```

### <a name="41-update-the-web-api"></a>4.1 Aktualisieren der Web-API

1. Öffnen Sie die Datei *config.json* in Ihrem Code-Editor.
1. Ändern Sie die Variablenwerte gemäß der zuvor erstellten Anwendungsregistrierung. Aktualisieren Sie außerdem den Richtliniennamen (`policyName`) mit dem Benutzerablauf, den Sie im Rahmen der Voraussetzungen erstellt haben. Beispielsweise *b2c_1_susi*.
    
    ```json
    "credentials": {
        "tenantName": "<your-tenant-name>",
        "clientID": "<your-webapi-application-ID>"
    },
    "policies": {
        "policyName": "b2c_1_susi"
    },
    "resource": {
        "scope": ["tasks.read"] 
    },
    ```

### <a name="42-enable-cors"></a>4.2 Aktivieren von CORS

Damit Ihre Single-Page-Webanwendung die Node.js-Web-API aufrufen kann, müssen Sie [CORS](https://expressjs.com/en/resources/middleware/cors.html) in der Web-API aktivieren. In einer Produktionsanwendung müssen Sie sorgfältig darauf achten, von welcher Domäne die Anforderung stammt. In diesem Beispiel können Sie Anforderungen von jeder beliebigen Domäne zulassen.

Verwenden Sie die folgende Middleware, um CORS zu aktivieren. In dem Node.js-Web-API-Codebeispiel, das Sie heruntergeladen haben, wurde sie bereits der Datei *index.js* hinzugefügt.

```javascript
app.use((req, res, next) => {
    res.header("Access-Control-Allow-Origin", "*");
    res.header("Access-Control-Allow-Headers", "Authorization, Origin, X-Requested-With, Content-Type, Accept");
    next();
});
```

## <a name="step-5-run-the-spa-and-web-api"></a>Schritt 5: Ausführen der SPA und der Web-API

Nun können Sie den bereichsbezogenen API-Zugriff der Single-Page-Webanwendung testen. Führen Sie sowohl die Node.js-Web-API als auch die exemplarische JavaScript-Single-Page-Webanwendung auf Ihrem lokalen Computer aus. Melden Sie sich anschließend bei der Single-Page-Webanwendung an, und wählen Sie die Schaltfläche **API aufrufen** aus, um eine Anforderung an die geschützte API zu initiieren.

### <a name="run-the-nodejs-web-api"></a>Ausführen der Node.js-Web-API

1. Öffnen Sie ein Konsolenfenster, und wechseln Sie zum Verzeichnis mit dem Node.js-Web-API-Beispiel. Beispiel:

    ```console
    cd active-directory-b2c-javascript-nodejs-webapi
    ```

1. Führen Sie die folgenden Befehle aus:

    ```console
    npm install && npm update
    node index.js
    ```

    Im Konsolenfenster wird die Portnummer angezeigt, unter der die Anwendung gehostet wird.

    ```console
    Listening on port 5000...
    ```

### <a name="run-the-single-page-app"></a>Ausführen der Single-Page-Webanwendung

1. Öffnen Sie ein weiteres Konsolenfenster, und wechseln Sie zum Verzeichnis mit dem JavaScript-SPA-Beispiel. Beispiel:

    ```console
    cd ms-identity-b2c-javascript-spa
    ```

1. Führen Sie die folgenden Befehle aus:

    ```console
    npm install && npm update
    npm start
    ```

    Im Konsolenfenster wird die Portnummer angezeigt, unter der die Anwendung gehostet wird.

    ```console
    Listening on port 6420...
    ```

1. Navigieren Sie in Ihrem Browser zu `http://localhost:6420`, um die Anwendung anzuzeigen.

    ![Beispiel-App für Singe-Page-Anwendung, die im Browser angezeigt wird](./media/configure-authentication-sample-spa-app/sample-app-sign-in.png)

1. Melden Sie sich mit der E-Mail-Adresse und dem Kennwort an, die Sie im [vorherigen Tutorial](tutorial-single-page-app.md) verwendet haben. Nach erfolgreicher Anmeldung sollte die Meldung `User 'Your Username' logged-in` angezeigt werden.
1. Wählen Sie die Schaltfläche **API aufrufen** aus. Die Single-Page-Webanwendung sendet das Zugriffstoken in einer Anforderung an die geschützte Web-API, die den Anzeigenamen des angemeldeten Benutzers zurückgibt:

    ![Single-Page-Webanwendung im Browser mit dem von der API zurückgegebenen Benutzernamen (JSON-Ergebnis)](./media/configure-authentication-sample-spa-app/sample-app-result.png)

## <a name="deploy-your-application"></a>Bereitstellen der Anwendung 

In einer Produktionsanwendung handelt es sich bei dem Umleitungs-URI der App-Registrierung in der Regel um einen öffentlich zugänglichen Endpunkt, an dem Ihre App ausgeführt wird, wie beispielsweise `https://contoso.com/signin-oidc`. 

Sie können Umleitungs-URIs in Ihren registrierten Anwendungen jederzeit hinzufügen und ändern. Für Umleitungs-URIs gelten die folgenden Einschränkungen:

* Die Antwort-URL muss mit dem Schema `https` beginnen.
* Bei der Antwort-URL muss die Groß-/Kleinschreibung beachtet werden. Die Groß-/Kleinschreibung muss der Groß-/Kleinschreibung des URL-Pfads Ihrer ausgeführten Anwendung entsprechen. 

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie mehr über das [Codebeispiel](https://github.com/Azure-Samples/ms-identity-b2c-javascript-spa).
* Erfahren Sie mehr über Authentifizierungsoptionen und das [Aktivieren der Authentifizierung in Ihrer eigenen Single-Page-Webanwendung mit Azure AD B2C](enable-authentication-spa-app-options.md).
