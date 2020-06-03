---
title: 'Tutorial: Angular-Single-Page-Webanwendung – Azure'
titleSuffix: Microsoft identity platform
description: Erfahren Sie, wie Angular-SPAs eine API aufrufen können, für die Zugriffstoken vom Microsoft Identity Platform-Endpunkt erforderlich sind.
services: active-directory
author: hahamil
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: tutorial
ms.workload: identity
ms.date: 03/05/2020
ms.author: hahamil
ms.custom: aaddev, identityplatformtop40
ms.openlocfilehash: 1ede6592b3da979136d70b873142af6d2bb8b593
ms.sourcegitcommit: a8ee9717531050115916dfe427f84bd531a92341
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/12/2020
ms.locfileid: "83201324"
---
# <a name="tutorial-sign-in-users-and-call-the-microsoft-graph-api-from-an-angular-single-page-application"></a>Tutorial: Anmelden von Benutzern und Aufrufen der Microsoft Graph-API aus einer Angular-SPA (Single-Page-Webanwendung)

> [!IMPORTANT]
> Diese Funktion steht derzeit als Vorschau zur Verfügung. Vorschauversionen werden Ihnen zur Verfügung gestellt, wenn Sie die [zusätzlichen Nutzungsbedingungen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) akzeptieren. Einige Aspekte dieses Features werden bis zur allgemeinen Verfügbarkeit unter Umständen noch geändert.

In diesem Tutorial werden folgende Aktionen für eine Angular-Single-Page-Webanwendung (SPA) beschrieben:
- Anmelden von persönlichen Konten sowie von Geschäfts-, Schul- oder Unikonten
- Abrufen eines Zugriffstokens
- Aufrufen der Microsoft Graph-API oder anderer APIs, für die Zugriffstoken des *Microsoft Identity Platform-Endpunkts* erforderlich sind

>[!NOTE]
>In diesem Tutorial werden die erforderlichen Schritte zum Erstellen einer neuen Angular-SPA mithilfe der Microsoft-Authentifizierungsbibliothek beschrieben. Eine Beispiel-App zum Herunterladen finden Sie in der [Schnellstartanleitung](quickstart-v2-angular.md).

## <a name="how-the-sample-app-works"></a>Funktionsweise der Beispiel-App

![Abbildung zur Veranschaulichung, wie die in diesem Tutorial generierte Beispiel-App funktioniert](media/active-directory-develop-guidedsetup-javascriptspa-introduction/javascriptspa-intro.svg)

### <a name="more-information"></a>Weitere Informationen

Die in diesem Tutorial erstellte Beispiel-App ermöglicht einer Angular-SPA das Abfragen der Microsoft Graph-API oder einer Web-API, die Token vom Microsoft Identity Platform-Endpunkt akzeptiert. Die MSAL für Angular ist ein Wrapper der MSAL.js-Kernbibliothek. Hiermit können Angular-Anwendungen (6+) die Authentifizierung für Unternehmensbenutzer per Microsoft Azure Active Directory, Benutzer von Microsoft-Konten und Benutzer mit Social Media-Identität (z. B. Facebook, Google und LinkedIn) durchführen. Die Bibliothek ermöglicht es den Anwendungen zudem, Zugriff auf Microsoft Cloud Services oder Microsoft Graph zu erhalten.

In diesem Szenario wird nach der Benutzeranmeldung ein Zugriffstoken angefordert und den HTTP-Anforderungen über den Autorisierungsheader hinzugefügt. Die MSAL übernimmt die Tokenbeschaffung und -erneuerung.

### <a name="libraries"></a>Bibliotheken

In diesem Tutorial wird die folgende Bibliothek verwendet:

|Bibliothek|BESCHREIBUNG|
|---|---|
|[msal.js](https://github.com/AzureAD/microsoft-authentication-library-for-js)|Microsoft Authentication Library für JavaScript: Angular-Wrapper|

Den Quellcode für die MSAL.js-Bibliothek finden Sie auf GitHub im Repository [AzureAD/microsoft-authentication-library-for-js](https://github.com/AzureAD/microsoft-authentication-library-for-js).

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie Folgendes:

* Ein lokaler Webserver, z. B. [Node.js](https://nodejs.org/en/download/). Die Anleitungen in diesem Tutorial basieren auf Node.js.
* Eine IDE (Integrated Development Environment, integrierte Entwicklungsumgebung), z. B. [Visual Studio Code](https://code.visualstudio.com/download), um die Projektdateien zu bearbeiten.

## <a name="create-your-project"></a>Erstellen Ihres Projekts

Verwenden Sie die folgenden npm-Befehle, um eine neue Angular-Anwendung zu generieren:

```Bash
npm install -g @angular/cli@8                    # Install the Angular CLI
npm install @angular/material@8 @angular/cdk@8   # Install the Angular Material component library (optional, for UI)
ng new my-application --routing=true --style=css # Generate a new Angular app
npm install msal @azure/msal-angular             # Install MSAL and MSAL Angular in your application
ng generate component page-name                  # To add a new page (such as a home or profile page)
```

## <a name="register-your-application"></a>Anwendung registrieren

Befolgen Sie die Anleitung zum [Registrieren einer Single-Page-Webanwendung](https://docs.microsoft.com/azure/active-directory/develop/scenario-spa-app-registration) im Azure-Portal.

Notieren Sie sich auf der Seite **Übersicht** Ihres Registrierungsvorgangs den Wert von **Anwendungs-ID (Client)** zur späteren Verwendung.

Registrieren Sie Ihren **Umleitungs-URI** als **http://localhost:4200/** , und aktivieren Sie die Einstellungen für die implizite Genehmigung.

## <a name="configure-the-application"></a>Konfigurieren der Anwendung

1. Bearbeiten Sie im Ordner *src/app* die Datei *app.module.ts*, und fügen Sie `MSALModule` zu `imports` sowie die Konstante `isIE` hinzu:

    ```javascript
    const isIE = window.navigator.userAgent.indexOf('MSIE ') > -1 || window.navigator.userAgent.indexOf('Trident/') > -1;
    @NgModule({
      declarations: [
        AppComponent
      ],
      imports: [
        BrowserModule,
        AppRoutingModule,
        MsalModule.forRoot({
          auth: {
            clientId: 'Enter_the_Application_Id_here', // This is your client ID
            authority: 'Enter_the_Cloud_Instance_Id_Here'/'Enter_the_Tenant_Info_Here', // This is your tenant ID
            redirectUri: 'Enter_the_Redirect_Uri_Here'// This is your redirect URI
          },
          cache: {
            cacheLocation: 'localStorage',
            storeAuthStateInCookie: isIE, // Set to true for Internet Explorer 11
          },
        }, {
          popUp: !isIE,
          consentScopes: [
            'user.read',
            'openid',
            'profile',
          ],
          unprotectedResources: [],
          protectedResourceMap: [
            ['https://graph.microsoft.com/v1.0/me', ['user.read']]
          ],
          extraQueryParameters: {}
        })
      ],
      providers: [],
      bootstrap: [AppComponent]
    })
    ```

    Ersetzen Sie die folgenden Werte:

    |Wertname|Info|
    |---------|---------|
    |Enter_the_Application_Id_Here|Der Wert für **Anwendungs-ID (Client)** auf der Seite **Übersicht** Ihrer Anwendungsregistrierung. |
    |Enter_the_Cloud_Instance_Id_Here|Die Instanz der Azure-Cloud. Geben Sie für die Azure-Hauptcloud oder für die globale Azure-Cloud **https://login.microsoftonline.com** ein. Informationen zu nationalen Clouds (z. B. für China) finden Sie unter [Nationale Clouds](https://docs.microsoft.com/azure/active-directory/develop/authentication-national-cloud).|
    |Enter_the_Tenant_Info_Here| Legen Sie eine der folgenden Optionen fest: Wenn Ihre Anwendung *Konten in diesem Organisationsverzeichnis* unterstützt, ersetzen Sie diesen Wert durch die Verzeichnis-ID (Mandant) oder den Mandantennamen (z. B. **contoso.microsoft.com**). Wenn Ihre Anwendung *Konten in einem beliebigen Organisationsverzeichnis* unterstützt, ersetzen Sie diesen Wert durch **Organisationen**. Wenn Ihre Anwendung *Konten in allen Organisationsverzeichnissen und persönliche Microsoft-Konten* unterstützt, ersetzen Sie diesen Wert durch **common**. Wenn Sie die Unterstützung *ausschließlich auf persönliche Microsoft-Konten* beschränken möchten, ersetzen Sie diesen Wert durch **consumers**. |
    |Enter_the_Redirect_Uri_Here|Ersetzen Sie diesen Wert durch **http://localhost:4200** .|

    Weitere Informationen zu den verfügbaren konfigurierbaren Optionen finden Sie unter [Initialisieren von Clientanwendungen](msal-js-initializing-client-applications.md).

2. Fügen Sie in derselben Datei oben die folgende Importanweisung hinzu:

    ```javascript
    import { MsalModule, MsalInterceptor } from '@azure/msal-angular';
    ```

3. Fügen Sie in der Datei `src/app/app.component.ts` oben die folgenden Importanweisungen hinzu:

    ```javascript
    import { MsalService } from '@azure/msal-angular';
    import { Component, OnInit } from '@angular/core';
    ```
## <a name="sign-in-a-user"></a>Anmelden eines Benutzers

Fügen Sie `AppComponent` den folgenden Code zum Anmelden eines Benutzers hinzu:

```javascript
export class AppComponent implements OnInit {
    constructor(private broadcastService: BroadcastService, private authService: MsalService) { }

    login() {
        const isIE = window.navigator.userAgent.indexOf('MSIE ') > -1 || window.navigator.userAgent.indexOf('Trident/') > -1;

        if (isIE) {
          this.authService.loginRedirect({
            extraScopesToConsent: ["user.read", "openid", "profile"]
          });
        } else {
          this.authService.loginPopup({
            extraScopesToConsent: ["user.read", "openid", "profile"]
          });
        }
    }
}
```

> [!TIP]
> Wir empfehlen die Verwendung von `loginRedirect` für Internet Explorer-Benutzer.

## <a name="acquire-a-token"></a>Abrufen eines Token

### <a name="angular-interceptor"></a>Interceptor von Angular

MSAL Angular verfügt über eine `Interceptor`-Klasse, mit der automatisch Token für ausgehende Anforderungen bezogen werden, bei denen der `http`-Client von Angular für bekannte geschützte Ressourcen verwendet wird.

Fügen Sie zunächst die `Interceptor`-Klasse als Anbieter in Ihre Anwendung ein:

```javascript
import { MsalInterceptor, MsalModule } from "@azure/msal-angular";
import { HTTP_INTERCEPTORS, HttpClientModule } from "@angular/common/http";

@NgModule({
    // ...
    providers: [
        {
            provide: HTTP_INTERCEPTORS,
            useClass: MsalInterceptor,
            multi: true
        }
    ]
}
```

Geben Sie als Nächstes eine Zuordnung mit den geschützten Ressourcen für `MsalModule.forRoot()` als `protectedResourceMap` an, und binden Sie diese Bereiche in `consentScopes` ein:

```javascript
@NgModule({
  // ...
  imports: [
    // ...
    MsalModule.forRoot({
      auth: {
        clientId: 'Enter_the_Application_Id_here', // This is your client ID
        authority: 'https://login.microsoftonline.com/Enter_the_Tenant_Info_Here', // This is your tenant info
        redirectUri: 'Enter_the_Redirect_Uri_Here' // This is your redirect URI
      },
      cache: {
        cacheLocation: 'localStorage',
        storeAuthStateInCookie: isIE, // Set to true for Internet Explorer 11
      },
    },
    {
      popUp: !isIE,
      consentScopes: [
        'user.read',
        'openid',
        'profile',
      ],
      unprotectedResources: [],
      protectedResourceMap: [
        ['https://graph.microsoft.com/v1.0/me', ['user.read']]
      ],
      extraQueryParameters: {}
    })
  ],
});
```

Rufen Sie abschließend mit einer HTTP-Anforderung das Profil eines Benutzers ab:

```JavaScript
const graphMeEndpoint = "https://graph.microsoft.com/v1.0/me";

getProfile() {
  this.http.get(graphMeEndpoint).toPromise()
    .then(profile => {
      this.profile = profile;
    });
}
```

### <a name="acquiretokensilent-acquiretokenpopup-acquiretokenredirect"></a>acquireTokenSilent, acquireTokenPopup, acquireTokenRedirect
MSAL verwendet drei Methoden zum Abrufen von Token: `acquireTokenRedirect`, `acquireTokenPopup` und `acquireTokenSilent`. Wir empfehlen Ihnen jedoch, für Angular-Apps stattdessen die Klasse `MsalInterceptor` zu verwenden (wie im vorherigen Abschnitt gezeigt).

#### <a name="get-a-user-token-silently"></a>Automatisches Abrufen eines Benutzertokens

Mit der `acquireTokenSilent`-Methode werden Token ohne Eingreifen des Benutzers abgerufen und verlängert. Nach dem erstmaligen Ausführen der Methode `loginRedirect` bzw. `loginPopup` wird normalerweise `acquireTokenSilent` zum Beziehen von Token verwendet, die für den Zugriff auf geschützte Ressourcen in späteren Aufrufen genutzt werden. Aufrufe zum Anfordern oder Verlängern von Token werden im Hintergrund ausgeführt.

```javascript
const requestObj = {
    scopes: ["user.read"]
};

this.authService.acquireTokenSilent(requestObj).then(function (tokenResponse) {
    // Callback code here
    console.log(tokenResponse.accessToken);
}).catch(function (error) {
    console.log(error);
});
```

In diesem Code enthält `scopes` Bereiche, die für die Rückgabe im Zugriffstoken für die API angefordert werden.

Beispiel:

* `["user.read"]` für Microsoft Graph
* `["<Application ID URL>/scope"]` für benutzerdefinierte Web-APIs (`api://<Application ID>/access_as_user`)

#### <a name="get-a-user-token-interactively"></a>Interaktives Abrufen eines Benutzertokens

Es kann erforderlich sein, dass der Benutzer mit dem Microsoft Identity Platform-Endpunkt interagiert. Beispiel:

* Benutzer müssen ihre Anmeldeinformationen gegebenenfalls erneut eingeben, weil ihr Kennwort abgelaufen ist.
* Ihre Anwendung fordert Zugriff auf zusätzliche Ressourcenbereiche an, für die der Benutzer seine Zustimmung erteilen muss.
* Die zweistufige Authentifizierung ist erforderlich.

Das empfohlene Muster für die meisten Anwendungen ist zuerst das Aufrufen von `acquireTokenSilent`, das anschließende Abfangen der Ausnahme und dann das Aufrufen von `acquireTokenPopup` (oder `acquireTokenRedirect`), um eine interaktive Anforderung zu starten.

Durch den Aufruf von `acquireTokenPopup` wird ein Popupfenster für die Anmeldung geöffnet. Mit `acquireTokenRedirect` werden Benutzer an den Microsoft Identity Platform-Endpunkt weitergeleitet. In diesem Fenster müssen Benutzer ihre Anmeldeinformationen bestätigen, ihre Zustimmung zur erforderlichen Ressource erteilen oder die zweistufige Authentifizierung durchführen.

```javascript
  const requestObj = {
      scopes: ["user.read"]
  };

  this.authService.acquireTokenPopup(requestObj).then(function (tokenResponse) {
      // Callback code here
      console.log(tokenResponse.accessToken);
  }).catch(function (error) {
      console.log(error);
  });
```

> [!NOTE]
> In dieser Schnellstartanleitung werden für Microsoft Internet Explorer die Methoden `loginRedirect` und `acquireTokenRedirect` verwendet. Dies liegt an einem [bekannten Problem](https://github.com/AzureAD/microsoft-authentication-library-for-js/wiki/Known-issues-on-IE-and-Edge-Browser#issues) bei der Verarbeitung von Popupfenstern durch Internet Explorer.

## <a name="log-out"></a>Abmelden

Fügen Sie den folgenden Code zum Abmelden eines Benutzers hinzu:

```javascript
logout() {
  this.authService.logout();
}
```

## <a name="add-ui"></a>Hinzufügen der Benutzeroberfläche
In der [Beispielanwendung](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-angular) finden Sie ein Beispiel für das Hinzufügen einer Benutzeroberfläche über die Angular-Bibliothek mit Materialkomponenten.

## <a name="test-your-code"></a>Testen Ihres Codes

1.  Starten Sie den Webserver, um an dem Port zu lauschen, indem Sie an einer Eingabeaufforderung im Anwendungsordner die folgenden Befehle ausführen:

    ```bash
    npm install
    npm start
    ```
1. Geben Sie in Ihrem Browser **http://localhost:4200** oder **http://localhost:{port}** ein, wobei *port* der Port ist, an dem der Webserver lauscht.


### <a name="provide-consent-for-application-access"></a>Zustimmen zum Anwendungszugriff

Wenn Sie sich erstmalig bei Ihrer Anwendung anmelden, werden Sie aufgefordert, ihr Zugriff auf Ihr Profil zu gewähren und ihr zu gestatten, Sie anzumelden:

![Das Fenster „Angeforderte Berechtigungen“](media/active-directory-develop-guidedsetup-javascriptspa-test/javascriptspaconsent.png)

## <a name="add-scopes-and-delegated-permissions"></a>Hinzufügen von Bereichen und delegierten Berechtigungen

Die Microsoft Graph-API benötigt den Bereich *user.read*, um das Benutzerprofil zu lesen. Dieser Bereich wird standardmäßig jeder Anwendung automatisch hinzugefügt, die beim Registrierungsportal registriert wird. Andere Microsoft Graph-APIs sowie benutzerdefinierte APIs für Ihren Back-End-Server erfordern unter Umständen zusätzliche Bereiche. Die Microsoft Graph-API benötigt beispielsweise den Bereich *Calendars.Read*, um die Kalender des Benutzers aufzuführen.

Um auf die Kalender des Benutzers im Kontext einer Anwendung zugreifen zu können, müssen Sie den Informationen für die Anwendungsregistrierung die delegierte Berechtigung *Calendars.Read* hinzufügen. Fügen Sie dann dem Aufruf von `acquireTokenSilent` den Bereich *Calendars.Read* hinzu.

>[!NOTE]
>Wenn Sie die Anzahl der Bereiche erhöhen, werden Benutzer ggf. zu weiteren Genehmigungen aufgefordert.

Wenn eine Back-End-API keinen Bereich benötigt (nicht empfohlen), können Sie in den Aufrufen zum Beziehen von Token *clientId* als Bereich verwenden.

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]

## <a name="next-steps"></a>Nächste Schritte

Falls Sie mit der Identitäts- und Zugriffsverwaltung noch nicht eingehend vertraut sind, können Sie verschiedene Artikel lesen, um sich über die Konzepte der modernen Authentifizierung zu informieren. Beginnen Sie mit [Authentifizierung im Vergleich zu Autorisierung](authentication-vs-authorization.md).

Wenn Sie sich eingehender mit der Entwicklung von Single-Page-Webanwendungen auf der Microsoft Identity Platform beschäftigen möchten, kann Ihnen die mehrteilige Artikelreihe [Szenario: Single-Page-Webanwendung](scenario-spa-overview.md) den Einstieg erleichtern.
