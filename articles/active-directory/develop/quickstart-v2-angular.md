---
title: 'Schnellstart: Anmelden von Benutzern in Angular-Single-Page-Webanwendungen | Azure'
titleSuffix: Microsoft identity platform
description: In dieser Schnellstartanleitung erfahren Sie, wie Sie aus einer Angular-App eine API aufrufen, für die Zugriffstoken erforderlich sind, die über die Microsoft Identity Platform ausgegeben werden.
services: active-directory
author: jasonnutter
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.custom: aaddev, identityplatformtop40, scenarios:getting-started, languages:JavaScript, devx-track-js
ms.topic: quickstart
ms.workload: identity
ms.date: 03/18/2020
ms.author: janutter
ms.openlocfilehash: e30ed370ea5858d97f2cc0a64bc92d9bc9e64b3f
ms.sourcegitcommit: 4a54c268400b4158b78bb1d37235b79409cb5816
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2021
ms.locfileid: "108128539"
---
# <a name="quickstart-sign-in-users-and-get-an-access-token-in-an-angular-single-page-application"></a>Schnellstart: Anmelden von Benutzern und Abrufen eines Zugriffstokens in einer Angular-Single-Page-Webanwendung

In diesem Schnellstart laden Sie ein Codebeispiel herunter und führen es aus, das zeigt, wie eine Angular-Single-Page-Webanwendung (SPA) Benutzer anmelden und Microsoft Graph aufrufen kann. Das Codebeispiel veranschaulicht das Abrufen eines Zugriffstokens zum Aufrufen der Microsoft Graph-API oder einer beliebigen Web-API.

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Abonnement. [Erstellen Sie ein kostenloses Konto.](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
* [Node.js](https://nodejs.org/en/download/).
* [Visual Studio Code](https://code.visualstudio.com/download) zum Bearbeiten von Projektdateien oder [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/) zum Ausführen des Projekts

> [!div renderon="docs"]
>
> ## <a name="register-and-download-the-quickstart-app"></a>Registrieren und Herunterladen der Schnellstart-App
>
> Verwenden Sie eine der folgenden Optionen, um die Schnellstart-App zu starten:
>
> ### <a name="option-1-express-register-and-automatically-configure-the-app-and-then-download-the-code-sample"></a>Option 1 (Express): Registrieren und automatisches Konfigurieren der App und anschließendes Herunterladen des Codebeispiels
>
> 1. Navigieren Sie zur Umgebung des Schnellstarts <a href="https://portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/ApplicationsListBlade/quickStartType/JavascriptSpaQuickstartPage/sourceType/docs" target="_blank">Azure-Portal – App-Registrierungen</a>.
> 1. Geben Sie einen Namen für Ihre Anwendung ein, und klicken Sie auf **Registrieren**.
> 1. Suchen Sie im Schnellstartbereich nach dem Schnellstart für Angular. Befolgen Sie die Anweisungen zum Herunterladen und automatischen Konfigurieren der neuen Anwendung.
>
> ### <a name="option-2-manual-register-and-manually-configure-the-application-and-code-sample"></a>Option 2 (manuell): Registrieren Sie die Anwendung und das Codebeispiel, und konfigurieren Sie sie manuell.
>
> #### <a name="step-1-register-the-application"></a>Schritt 1: Registrieren der Anwendung
>
> 1. Befolgen Sie die Anleitung zum [Registrieren einer Single-Page-Webanwendung](./scenario-spa-app-registration.md) im Azure-Portal.
> 1. Fügen Sie im Bereich **Authentifizierung** Ihrer App-Registrierung eine neue Plattform hinzu, und registrieren Sie den Umleitungs-URI: `http://localhost:4200/`.
> 1. In dieser Schnellstartanleitung wird der [Flow zur impliziten Genehmigung](v2-oauth2-implicit-grant-flow.md) verwendet. Wählen Sie im Abschnitt **Implizite Genehmigung und Hybridflows** die Optionen **ID-Token** und **Zugriffstoken** aus. ID- und Zugriffstoken sind erforderlich, da diese App Benutzer anmeldet und eine API aufruft.

> [!div class="sxs-lookup" renderon="portal"]
>
> #### <a name="step-1-configure-the-application-in-the-azure-portal"></a>Schritt 1: Konfigurieren der Anwendung im Azure-Portal
>
> Damit das Codebeispiel in dieser Schnellstartanleitung funktioniert, müssen Sie einen Umleitungs-URI für `http://localhost:4200/` hinzufügen und die Option **Implizite Genehmigung** aktivieren.
> > [!div renderon="portal" id="makechanges" class="nextstepaction"]
> > [Make these changes for me]() (Diese Änderungen für mich vornehmen)
>
> > [!div id="appconfigured" class="alert alert-info"]
> > ![Bereits konfiguriert](media/quickstart-v2-javascript/green-check.png): Ihre Anwendung ist mit diesen Attributen konfiguriert.

#### <a name="step-2-download-the-code-sample"></a>Schritt 2: Herunterladen des Codebeispiels
>[!div renderon="docs"]
>Wenn Sie das Projekt mit einem Webserver unter Verwendung von Node.js ausführen möchten, klonen Sie das [Beispielrepository](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-angular), oder [laden Sie die Kernprojektdateien herunter](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-angular/archive/master.zip). Öffnen Sie die Dateien mit einem Editor (beispielsweise Visual Studio Code).

> [!div renderon="portal" id="autoupdate" class="sxs-lookup nextstepaction"]
> [Laden Sie das Codebeispiel herunter](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-angular/archive/master.zip).

> [!div renderon="docs"]
>#### <a name="step-3-configure-the-javascript-app"></a>Schritt 3: Konfigurieren der JavaScript-App
>
> Bearbeiten Sie im Ordner *src/app* die Datei *app.module.ts*, und legen Sie unter `MsalModule.forRoot` die Werte `clientId` und `authority` fest.
>
>```javascript
>MsalModule.forRoot({
>    auth: {
>        clientId: 'Enter_the_Application_Id_here', // This is your client ID
>        authority: 'https://login.microsoftonline.com/Enter_the_Tenant_Info_Here', // This is your tenant info
>        redirectUri: 'Enter_the_Redirect_Uri_Here' // This is your redirect URI
>    },
>    cache: {
>        cacheLocation: 'localStorage',
>        storeAuthStateInCookie: isIE, // set to true for IE 11
>    },
>},
> //... )
>```

> [!div class="sxs-lookup" renderon="portal"]
> > [!NOTE]
> > Enter_the_Supported_Account_Info_Here

> [!div renderon="docs"]
>
> Ersetzen Sie die folgenden Werte:
>
>|Wertname|BESCHREIBUNG|
>|---------|---------|
>|Enter_the_Application_Id_Here|Der Wert für **Anwendungs-ID (Client)** auf der Seite **Übersicht** Ihrer Anwendungsregistrierung. |
>|Enter_the_Cloud_Instance_Id_Here|Die Azure-Cloudinstanz. Geben Sie für die Azure-Hauptcloud oder für die globale Azure-Cloud `https://login.microsoftonline.com` ein. Informationen zu nationalen Clouds (z. B. für China) finden Sie unter [Nationale Clouds](./authentication-national-cloud.md).|
>|Enter_the_Tenant_Info_Here| Legen Sie eine der folgenden Optionen fest: Wenn Ihre Anwendung *Konten in diesem Organisationsverzeichnis* unterstützt, ersetzen Sie diesen Wert durch die Verzeichnis-ID (Mandant) oder den Mandantennamen (z. B. `contoso.microsoft.com`). Falls Ihre Anwendung *Konten in einem beliebigen Organisationsverzeichnis* unterstützt, ersetzen Sie diesen Wert durch `organizations`. Unterstützt Ihre Anwendung *Konten in allen Organisationsverzeichnissen und persönliche Microsoft-Konten*, ersetzen Sie diesen Wert durch `common`. Wenn Sie die Unterstützung *ausschließlich auf persönliche Microsoft-Konten* beschränken möchten, ersetzen Sie diesen Wert durch `consumers`. |
>|Enter_the_Redirect_Uri_Here|Ersetzen Sie diesen Wert durch `http://localhost:4200` .|
>|cacheLocation  | (Optional) Legen Sie den Browserspeicher für den Authentifizierungsstatus fest. Der Standardwert lautet `sessionStorage`.   |
>|storeAuthStateInCookie  | (Optional) Identifizieren Sie die Bibliothek, in der der Authentifizierungsanforderungsstatus gespeichert wird. Dieser Status ist für die Überprüfung der Authentifizierungsflows in den Browsercookies erforderlich. Dieses Cookie wird für Internet Explorer und Microsoft Edge festgelegt, um diese beiden Browser zu unterstützen. Weitere Informationen finden Sie im Artikel zu [bekannten Problemen](https://github.com/AzureAD/microsoft-authentication-library-for-js/wiki/Known-issues->on-IE-and-Edge-Browser#issues). |
>
> Die Werte für **Anwendungs-ID (Client)** , **Verzeichnis-ID (Mandant)** und **Unterstützte Kontotypen** finden Sie im Azure-Portal auf der Seite **Übersicht** der App.

> Weitere Informationen zu den verfügbaren konfigurierbaren Optionen finden Sie unter [Initialisieren von Clientanwendungen](msal-js-initializing-client-applications.md).

> Den Quellcode für die MSAL.js-Bibliothek finden Sie auf GitHub im Repository [AzureAD/microsoft-authentication-library-for-js](https://github.com/AzureAD/microsoft-authentication-library-for-js).

> [!div class="sxs-lookup" renderon="portal"]
> #### <a name="step-3-your-app-is-configured-and-ready-to-run"></a>Schritt 3: Ihre App ist konfiguriert und betriebsbereit
> Wir haben das Projekt mit Werten der Eigenschaften ihrer App konfiguriert.

> [!div renderon="docs"]
>
> Scrollen Sie in derselben Datei nach unten, und aktualisieren Sie `graphMeEndpoint`. 
> - Ersetzen Sie die Zeichenfolge `Enter_the_Graph_Endpoint_Herev1.0/me` durch `https://graph.microsoft.com/v1.0/me`.
> - `Enter_the_Graph_Endpoint_Herev1.0/me` ist der Endpunkt, für den API-Aufrufe ausgeführt werden. Geben Sie für den Microsoft Graph-API-Hauptdienst (global) `https://graph.microsoft.com/` (einschließlich des nachstehenden Schrägstrichs) ein. Weitere Informationen finden Sie in der [Dokumentation](/graph/deployments).
>
>
> ```javascript
>      protectedResourceMap: [
>        ['Enter_the_Graph_Endpoint_Herev1.0/me', ['user.read']]
>      ],
> ```
>
>

>[!div renderon="docs"]
>#### <a name="step-4-run-the-project"></a>Schritt 4: Ausführen des Projekts

Wenn Sie Node.js verwenden, gehen Sie wie folgt vor:

1. Starten Sie den Server, indem Sie im Projektverzeichnis die folgenden Befehle ausführen:

   ```console
   npm install
   npm start
   ```

1. Wechseln Sie zu `http://localhost:4200/`.
1. Wählen Sie **Anmelden**. Bei der ersten Anmeldung werden Sie aufgefordert, der Anwendung zu erlauben, auf Ihr Profil zuzugreifen und Sie automatisch anzumelden.
1. Wählen Sie **Profil** aus, um Microsoft Graph aufzurufen. Ihre Benutzerprofilinformationen werden auf der Seite angezeigt.

## <a name="how-the-sample-works"></a>Funktionsweise des Beispiels

![Diagramm, das die Funktionsweise der Beispiel-App in diesem Schnellstart demonstriert](./media/quickstart-v2-angular/diagram-auth-flow-spa-angular.svg)

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich im Angular-Tutorial darüber, wie Sie einen Benutzer anmelden und Token abrufen:

> [!div class="nextstepaction"]
> [Anmelden von Benutzern und Aufrufen der Microsoft Graph-API aus einer Angular-SPA (Single-Page-Webanwendung)](./tutorial-v2-angular.md)