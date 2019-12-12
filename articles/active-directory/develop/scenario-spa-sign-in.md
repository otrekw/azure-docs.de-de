---
title: Single-Page-Webanwendung (Anmeldung) – Microsoft Identity Platform
description: Hier erfahren Sie, wie Sie eine Single-Page-Webanwendung (Anmeldung) erstellen.
services: active-directory
documentationcenter: dev-center-name
author: navyasric
manager: CelesteDG
editor: ''
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/06/2019
ms.author: nacanuma
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: e0fd546724b8d684746a9f4d63a03bc6b58ded52
ms.sourcegitcommit: 6bb98654e97d213c549b23ebb161bda4468a1997
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/03/2019
ms.locfileid: "74764643"
---
# <a name="single-page-application-sign-in"></a>Single-Page-Webanwendung: Anmeldung

Hier erfahren Sie, wie Sie die Anmeldung zum Code Ihrer Single-Page-Webanwendung hinzufügen.

Bevor Sie die Token für den Zugriff auf APIs in Ihrer Anwendung abrufen können, benötigen Sie einen authentifizierten Benutzerkontext. Sie können Benutzer in MSAL.js auf zwei Arten bei Ihrer Anwendung anmelden:

* [Popupfenster](#sign-in-with-a-pop-up-window) mithilfe der `loginPopup`-Methode
* [Umleiten](#sign-in-with-redirect) mithilfe der `loginRedirect`-Methode

Sie können auch optional diejenigen Bereiche der APIs übergeben, für die der Benutzer zum Zeitpunkt der Anmeldung seine Zustimmung erteilen muss.

> [!NOTE]
> Wenn Ihre Anwendung bereits Zugriff auf einen authentifizierten Benutzerkontext oder das ID-Token hat, können Sie den Anmeldeschritt überspringen und Token direkt abrufen. Weitere Informationen hierzu finden Sie unter [SSO-Anmeldung ohne MSAL.js](msal-js-sso.md#sso-without-msaljs-login).

## <a name="choosing-between-a-pop-up-or-redirect-experience"></a>Auswählen zwischen einem Popupfenster oder einer Umleitung

Sie können in Ihrer Anwendung nicht die Popupfenster- und die Umleitungsmethode verwenden. Die Entscheidung für ein Popupfenster oder eine Umleitung hängt von Ihrem Anwendungsfluss ab:

* Wenn Sie nicht möchten, dass Benutzer während der Authentifizierung Ihre Hauptanwendungsseite verlassen, wird die Popupmethode empfohlen. Da die Authentifizierungsumleitung in einem Popupfenster erfolgt, wird der Status der Hauptanwendung beibehalten.

* Wenn für Benutzer Browsereinschränkungen oder Richtlinien gelten, bei denen Popupfenster deaktiviert sind, können Sie die Umleitungsmethode verwenden. Verwenden Sie für Internet Explorer die Umleitungsmethode, da es hier [bekannte Probleme mit Popupfenstern gibt](https://github.com/AzureAD/microsoft-authentication-library-for-js/wiki/Known-issues-on-IE-and-Edge-Browser).

## <a name="sign-in-with-a-pop-up-window"></a>Anmelden mit einem Popupfenster

### <a name="javascript"></a>JavaScript

```javascript
const loginRequest = {
    scopes: ["https://graph.microsoft.com/User.ReadWrite"]
}

userAgentApplication.loginPopup(loginRequest).then(function (loginResponse) {
    //login success
    let idToken = loginResponse.idToken;
}).catch(function (error) {
    //login failure
    console.log(error);
});
```

### <a name="angular"></a>Angular

Mit dem MSAL Angular-Wrapper können Sie bestimmte Routen in Ihrer Anwendung sichern, indem Sie `MsalGuard` zu der Routendefinition hinzufügen. Dieser Wächter ruft die Methode für die Anmeldung auf, wenn auf diese Route zugegriffen wird.

```javascript
// In app.routes.ts
{ path: 'product', component: ProductComponent, canActivate : [MsalGuard],
    children: [
      { path: 'detail/:id', component: ProductDetailComponent  }
    ]
   },
  { path: 'myProfile' ,component: MsGraphComponent, canActivate : [MsalGuard] },
```

Aktivieren Sie für ein Popupfenster die Konfigurationsoption `popUp`. Sie können wie folgt auch die Bereiche übergeben, die eine Zustimmung erfordern:

```javascript
//In app.module.ts
@NgModule({
  imports: [ MsalModule.forRoot({
                clientID: 'your_app_id',
                popUp: true,
                consentScopes: ["https://graph.microsoft.com/User.ReadWrite"]
            })]
         })
```

## <a name="sign-in-with-redirect"></a>Anmelden mit einer Umleitung

### <a name="javascript"></a>JavaScript

Die Umleitungsmethoden geben aufgrund der Navigation weg von der Hauptanwendung keine Zusage zurück. Sie müssen vor dem Abrufen der Umleitungsmethoden Erfolgs- und Fehlerrückrufe registrieren, um die zurückgegebenen Token zu verarbeiten und auf sie zuzugreifen.

```javascript
function authCallback(error, response) {
    //handle redirect response
}

userAgentApplication.handleRedirectCallback(authCallback);

const loginRequest = {
    scopes: ["https://graph.microsoft.com/User.ReadWrite"]
}

userAgentApplication.loginRedirect(loginRequest);
```

### <a name="angular"></a>Angular

Hier entspricht der Code dem zuvor im Abschnitt über das Anmelden mit einem Popupfenster beschriebenen Code. Der Standardfluss wird umgeleitet.

> [!NOTE]
> Das ID-Token enthält nicht die Bereiche, zu denen Zustimmung erteilt wurde, und repräsentiert nur den authentifizierten Benutzer. Die Bereiche mit Zustimmung werden im Zugriffstoken zurückgegeben, das Sie im nächsten Schritt abrufen.

## <a name="sign-out"></a>Abmeldung

Die MSAL-Bibliothek bietet eine `logout`-Methode, die den Cache im Speicher des Browsers löscht und eine Abmeldungsanforderung an Azure Active Directory (Azure AD) sendet. Nach der Abmeldung wird die Bibliothek standardmäßig zurück zur Startseite der Anwendung umgeleitet.

Durch Festlegen von `postLogoutRedirectUri` können Sie den URI konfigurieren, zu dem nach der Anmeldung umgeleitet werden soll. Dieser URI sollte auch als Abmeldungs-URI in Ihrer Anwendungsregistrierung registriert sein.

### <a name="javascript"></a>JavaScript

```javascript
const config = {

    auth: {
        clientID: 'your_app_id',
        redirectUri: "your_app_redirect_uri", //defaults to application start page
        postLogoutRedirectUri: "your_app_logout_redirect_uri"
    }
}

const userAgentApplication = new UserAgentApplication(config);
userAgentApplication.logout();

```

### <a name="angular"></a>Angular

```javascript
//In app.module.ts
@NgModule({
  imports: [ MsalModule.forRoot({
                clientID: 'your_app_id',
                postLogoutRedirectUri: "your_app_logout_redirect_uri"
            })]
         })

// In app.component.ts
this.authService.logout();
```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Abrufen eines Tokens für die App](scenario-spa-acquire-token.md)
