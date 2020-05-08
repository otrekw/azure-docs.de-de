---
title: Anmelden von Benutzern bei JavaScript-Single-Page-Web-Apps mit Autorisierungscode | Azure
titleSuffix: Microsoft identity platform
description: Hier erfahren Sie, wie eine JavaScript-App unter Verwendung von Microsoft Identity Platform eine API aufrufen kann, für die Zugriffstoken erforderlich sind.
services: active-directory
author: hahamil
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: quickstart
ms.workload: identity
ms.date: 04/22/2020
ms.author: hahamil
ms.custom: aaddev, identityplatformtop40, scenarios:getting-started, languages:JavaScript
ROBOTS: NOINDEX
ms.openlocfilehash: 9663c11508b0478a67f528cb301d705a3125e4f6
ms.sourcegitcommit: f57297af0ea729ab76081c98da2243d6b1f6fa63
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2020
ms.locfileid: "82871523"
---
# <a name="quickstart-sign-in-users-and-get-an-access-token-in-a-javascript-spa-using-the-auth-code-flow"></a>Schnellstart: Anmelden von Benutzern und Abrufen eines Zugriffstokens in einer JavaScript-SPA mithilfe des Autorisierungscodeflows 

> [!IMPORTANT]
> Diese Funktion steht derzeit als Vorschau zur Verfügung. Vorschauversionen werden Ihnen zur Verfügung gestellt, wenn Sie die [zusätzlichen Nutzungsbedingungen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) akzeptieren. Einige Aspekte dieses Features werden bis zur allgemeinen Verfügbarkeit unter Umständen noch geändert.


In dieser Schnellstartanleitung wird MSAL.js 2.0 mit dem Autorisierungscodeflow verwendet. Wenn Sie MSAL.js 1.0 mit dem impliziten Flow verwenden möchten, sehen Sie sich [diese Schnellstartanleitung](https://docs.microsoft.com/azure/active-directory/develop/quickstart-v2-javascript) an.

In diesem Schnellstart erfahren Sie anhand eines Codebeispiels, wie in einer JavaScript-Single-Page-Anwendung (SPA) Benutzer von persönlichen Konten und Geschäfts-, Schul- und Unikonten angemeldet werden können. Eine JavaScript-Single-Page-Anwendung kann zudem ein Zugriffstoken abrufen, um die Microsoft Graph-API oder eine Web-API aufzurufen. Eine Abbildung finden Sie unter [Funktionsweise des Beispiels](#how-the-sample-works).

## <a name="prerequisites"></a>Voraussetzungen

* Azure-Abonnement ([kostenloses Azure-Abonnement erstellen](https://azure.microsoft.com/free/?WT.mc_id=A261C142F))
* [Node.js](https://nodejs.org/en/download/)
* [Visual Studio Code](https://code.visualstudio.com/download) (zum Bearbeiten der Projektdateien)


> [!div renderon="docs"]
> ## <a name="register-and-download-your-quickstart-application"></a>Registrieren und Herunterladen Ihrer Schnellstartanwendung
> Verwenden Sie eine der folgenden Optionen, um die Schnellstartanwendung zu starten.
>
> ### <a name="option-1-express-register-and-auto-configure-your-app-and-then-download-your-code-sample"></a>Option 1 (Express): Registrieren und automatisches Konfigurieren Ihrer App und anschließendes Herunterladen des Codebeispiels
>
> 1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
> 1. Wenn Sie in Ihrem Konto auf mehrere Mandanten zugreifen können, wählen Sie rechts oben das Konto aus, und legen Sie Ihre Portalsitzung auf den gewünschten Azure Active Directory-Mandanten (Azure AD) fest.
> 1. Wählen Sie [App-Registrierungen](https://portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/ApplicationsListBlade/quickStartType/JavascriptSpaQuickstartPage/sourceType/docs) aus.
> 1. Geben Sie einen Namen für Ihre Anwendung ein.
> 1. Wählen Sie unter **Unterstützte Kontotypen** **Konten in allen Organisationsverzeichnissen und persönliche Microsoft-Konten** aus.
> 1. Wählen Sie **Registrieren**.
> 1. Navigieren Sie zum Schnellstartbereich, und befolgen Sie die Anweisungen, um Ihre neue Anwendung herunterzuladen und automatisch zu konfigurieren.
>
> ### <a name="option-2-manual-register-and-manually-configure-your-application-and-code-sample"></a>Option 2 (manuell): Registrieren und manuelles Konfigurieren Ihrer Anwendung und des Codebeispiels
>
> #### <a name="step-1-register-your-application"></a>Schritt 1: Anwendung registrieren
>
> 1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
>
> 1. Wenn Sie in Ihrem Konto auf mehrere Mandanten zugreifen können, wählen Sie rechts oben das Konto aus, und legen Sie Ihre Portalsitzung auf den gewünschten Azure AD-Mandanten fest.
> 1. Wählen Sie [App-Registrierungen](https://go.microsoft.com/fwlink/?linkid=2083908) aus.
> 1. Wählen Sie **Neue Registrierung** aus.
> 1. Geben Sie auf der daraufhin angezeigten Seite **Anwendung registrieren** einen Namen für Ihre Anwendung ein.
> 1. Wählen Sie unter **Unterstützte Kontotypen** **Konten in allen Organisationsverzeichnissen und persönliche Microsoft-Konten** aus.
> 1. Wählen Sie **Registrieren**. Notieren Sie sich für die spätere Verwendung auf der Seite **Übersicht** den Wert von **Anwendungs-ID (Client)** .
> 1. Wählen Sie im linken Bereich der registrierten Anwendung die Option **Authentifizierung** aus.
> 1. Wählen Sie unter **Plattformkonfigurationen** die Option **Plattform hinzufügen** aus. Daraufhin wird auf der linken Seite ein Bereich geöffnet. Wählen Sie dort die Region **Single-Page-Webanwendungen** aus.
> 1. Legen Sie den Wert für den **Umleitungs-URI** auf `http://localhost:3000/` fest (ebenfalls im linken Bereich). 
> 1. Wählen Sie **Konfigurieren**aus.

> [!div class="sxs-lookup" renderon="portal"]
> #### <a name="step-1-configure-your-application-in-the-azure-portal"></a>Schritt 1: Konfigurieren Ihrer Anwendung im Azure-Portal
> Damit das Codebeispiel in dieser Schnellstartanleitung funktioniert, muss `http://localhost:3000/` als Umleitungs-URI (`redirectUri`) hinzugefügt werden.
> > [!div renderon="portal" id="makechanges" class="nextstepaction"]
> > [Make these changes for me]() (Diese Änderungen für mich vornehmen)
>
> > [!div id="appconfigured" class="alert alert-info"]
> > ![Bereits konfiguriert](media/quickstart-v2-javascript/green-check.png): Ihre Anwendung ist mit diesen Attributen konfiguriert.

#### <a name="step-2-download-the-project"></a>Schritt 2: Herunterladen des Projekts

> [!div renderon="docs"]
> Um das Projekt mit einem Webserver unter Verwendung von Node.js auszuführen, [laden Sie die Kernprojektdateien herunter](https://github.com/Azure-Samples/ms-identity-javascript-v2/archive/quickstart.zip).

> [!div renderon="portal" class="sxs-lookup"]
> Ausführen des Projekts mit einem Webserver unter Verwendung von Node.js

> [!div renderon="portal" id="autoupdate" class="nextstepaction" class="sxs-lookup"]
> [Laden Sie das Codebeispiel herunter](https://github.com/Azure-Samples/ms-identity-javascript-v2/archive/quickstart.zip).

> [!div renderon="docs"]
> #### <a name="step-3-configure-your-javascript-app"></a>Schritt 3: Konfigurieren Ihrer JavaScript-App
>
> Bearbeiten Sie *authConfig.js* im Ordner *app*, und legen Sie unter `msalConfig` die Werte `clientID`, `authority` und `redirectUri` fest.
>
> ```javascript
>
>  // Config object to be passed to Msal on creation
>  const msalConfig = {
>    auth: {
>      clientId: "Enter_the_Application_Id_Here",
>      authority: "Enter_the_Cloud_Instance_Id_HereEnter_the_Tenant_Info_Here",
>      redirectUri: "Enter_the_Redirect_Uri_Here",
>    },
>    cache: {
>      cacheLocation: "sessionStorage", // This configures where your cache will be stored
>      storeAuthStateInCookie: false, // Set this to "true" if you are having issues on IE11 or Edge
>    }
>  };
>
>```

> [!div renderon="portal" class="sxs-lookup"]
> > [!NOTE]
> > :::no-loc text="Enter_the_Supported_Account_Info_Here":::

> [!div renderon="docs"]
>
> Hierbei gilt:
> - *\<Enter_the_Application_Id_Here>* ist die **Anwendungs-ID (Client)** für die von Ihnen registrierte Anwendung.
> - *\<Enter_the_Cloud_Instance_Id_Here>* ist die Instanz der Azure-Cloud. Geben Sie für die Azure-Hauptcloud oder für die globale Azure-Cloud einfach *https://login.microsoftonline.com/* ein. Informationen zu **nationalen** Clouds (etwa für China) finden Sie unter [Nationale Clouds](https://docs.microsoft.com/azure/active-directory/develop/authentication-national-cloud).
> - *\<Enter_the_Tenant_info_here >* wird auf eine der folgenden Optionen festgelegt:
>    - Wenn Ihre Anwendung *Nur Konten in diesem Organisationsverzeichnis* unterstützt, ersetzen Sie diesen Wert durch die **Mandanten-ID** oder den **Mandantennamen** (beispielsweise *contoso.microsoft.com*).
>    - Wenn Ihre Anwendung *Konten in einem beliebigen Organisationsverzeichnis* unterstützt, ersetzen Sie diesen Wert durch **Organisationen**.
>    - Wenn Ihre Anwendung *Konten in allen Organisationsverzeichnissen und persönliche Microsoft-Konten* unterstützt, ersetzen Sie diesen Wert durch **common**. Wenn Sie die Unterstützung *ausschließlich auf persönliche Microsoft-Konten* beschränken möchten, ersetzen Sie diesen Wert durch **consumers**.
> - Für *\<Enter_the_Redirect_Uri_Here>* wird `http://localhost:3000` verwendet.
> > [!TIP]
> > Die Werte für **Anwendungs-ID (Client)** , **Verzeichnis-ID (Mandant)** und **Unterstützte Kontotypen** finden Sie im Azure-Portal auf der Seite **Übersicht** der App-Registrierung.
>
> [!div class="sxs-lookup" renderon="portal"]
> #### <a name="step-3-your-app-is-configured-and-ready-to-run"></a>Schritt 3: Ihre App ist konfiguriert und betriebsbereit
> Wir haben das Projekt mit Werten der Eigenschaften ihrer App konfiguriert.

> [!div renderon="docs"]
>
> Bearbeiten Sie dann im selben Ordner die Datei *graphConfig.js*, um `graphMeEndpoint` und `graphMailEndpoint` für das Objekt `apiConfig` festzulegen.
> ```javascript
>   // Add here the endpoints for MS Graph API services you would like to use.
>   const graphConfig = {
>     graphMeEndpoint: "Enter_the_Graph_Endpoint_Herev1.0/me",
>     graphMailEndpoint: "Enter_the_Graph_Endpoint_Herev1.0/me/messages"
>   };
>
>   // Add here scopes for access token to be used at MS Graph API endpoints.
>   const tokenRequest = {
>       scopes: ["Mail.Read"]
>   };
> ```
>

> [!div renderon="docs"]
>
> *\<Enter_the_Graph_Endpoint_Here>* ist der Endpunkt, für den API-Aufrufe ausgeführt werden. Geben Sie für den Microsoft Graph-API-Hauptdienst oder den globalen Microsoft Graph-API-Dienst `https://graph.microsoft.com` ein. Weitere Informationen finden Sie unter [Bereitstellungen nationaler Clouds](https://docs.microsoft.com/graph/deployments).
>
> #### <a name="step-4-run-the-project"></a>Schritt 4: Ausführen des Projekts

Führen Sie das Projekt mit einem Webserver unter Verwendung von [Node.js](https://nodejs.org/en/download/) aus:

1. Führen Sie im Projektverzeichnis die folgenden Befehle aus, um den Server zu starten:
    ```bash
    npm install
    npm start
    ```
1. Navigieren Sie zu `http://localhost:3000/`.

1. Wählen Sie **Anmelden** aus, um den Anmeldevorgang zu starten, und rufen Sie anschließend die Microsoft Graph-API auf.

    Bei der ersten Anmeldung werden Sie aufgefordert, einzuwilligen, dass die Anwendung auf Ihr Profil zugreifen und Sie anmelden darf. Bei erfolgreicher Anmeldung sollten Ihre Benutzerprofilinformationen auf der Seite angezeigt werden.

## <a name="more-information"></a>Weitere Informationen

### <a name="how-the-sample-works"></a>Funktionsweise des Beispiels

![Funktionsweise der JavaScript-Beispiel-SPA: 1. Die SPA initiiert die Anmeldung. 2. Die SPA ruft ein ID-Token von Microsoft Identity Platform ab. 3. Die SPA ruft Abruftoken auf. 4. Microsoft Identity Platform gibt ein Zugriffstoken an die SPA zurück. 5. Die SPA sendet eine HTTP GET-Anforderung mit dem Zugriffstoken an die Microsoft Graph-API. 6. Die Graph-API gibt eine HTTP-Antwort an die SPA zurück.](media/quickstart-v2-javascript/javascriptspa-intro.svg)

### <a name="msaljs"></a>msal.js

Über die MSAL.js-Bibliothek werden Benutzer angemeldet und die Token angefordert, die für den Zugriff auf eine durch Microsoft Identity Platform geschützte API verwendet werden. Die Datei *index.html* des Beispiels enthält einen Verweis auf die Bibliothek:

```html
<script type="text/javascript" src="https://alcdn.msauth.net/browser/2.0.0-beta.0/js/msal-browser.js" integrity=
"sha384-r7Qxfs6PYHyfoBR6zG62DGzptfLBxnREThAlcJyEfzJ4dq5rqExc1Xj3TPFE/9TH" crossorigin="anonymous"></script>
```
> [!TIP]
> Sie können die vorstehende Version durch die letzte veröffentlichte Version unter [MSAL.js-Releases](https://github.com/AzureAD/microsoft-authentication-library-for-js/releases) ersetzen.

Wenn Sie Node.js installiert haben, können Sie die aktuelle Version auch mithilfe von Node.js-Paket-Manager (npm) herunterladen:

```batch
npm install @azure/msal-browser
```

## <a name="next-steps"></a>Nächste Schritte

Das [GitHub-Repository für MSAL.js](https://github.com/AzureAD/microsoft-authentication-library-for-js) enthält eine zusätzliche Bibliotheksdokumentation sowie häufig gestellte Fragen und bietet Unterstützung bei Problemen.

Eine ausführliche Anleitung zum Erstellen der Anwendung für diesen Schnellstart finden Sie in folgendem Artikel:

> [!div class="nextstepaction"]
> [Aufrufen der Microsoft Graph-API aus einer JavaScript-SPA](https://docs.microsoft.com/azure/active-directory/develop/tutorial-v2-javascript-auth-code)
