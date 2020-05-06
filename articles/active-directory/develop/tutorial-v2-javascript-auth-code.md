---
title: JavaScript-Tutorial mit Single-Page-App 2.0 – Microsoft Identity Platform | Azure
description: Es wird beschrieben, wie JavaScript-Single-Page-Anwendungen den Autorisierungscodefluss nutzen können, um eine API aufzurufen, für die Zugriffstoken des Azure Active Directory v2.0-Endpunkts erforderlich sind.
services: active-directory
documentationcenter: dev-center-name
author: hahamil
manager: CelesteDG
ROBOTS: NOINDEX
ms.service: active-directory
ms.subservice: develop
ms.topic: tutorial
ms.workload: identity
ms.date: 04/22/2020
ms.author: hahamil
ms.custom: aaddev, identityplatformtop40
ms.openlocfilehash: ee156c8118a67061d0a000867ee64fe1f3ebd18c
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2020
ms.locfileid: "82181882"
---
# <a name="sign-in-users-and-call-the-microsoft-graph-api-from-a-javascript-single-page-application-spa---msaljs-20"></a>Anmelden von Benutzern und Aufrufen der Microsoft Graph-API aus einer JavaScript-Single-Page-Webanwendung (SPA): MSAL.js 2.0

> [!IMPORTANT]
> Diese Funktion steht derzeit als Vorschau zur Verfügung. Vorschauversionen werden Ihnen zur Verfügung gestellt, wenn Sie die [zusätzlichen Nutzungsbedingungen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) akzeptieren. Einige Aspekte dieses Features werden bis zur allgemeinen Verfügbarkeit unter Umständen noch geändert.

In diesem Tutorial wird eine Version von MSAL.js verwendet, für die der OAuth 2.0-Autorisierungscodefluss mit PKCE genutzt wird. Weitere Informationen zu diesem Protokoll sowie zu den Unterschieden zwischen einem impliziten Fluss und dem Autorisierungscodefluss finden Sie in der [Dokumentation](https://docs.microsoft.com/azure/active-directory/develop/v2-oauth2-auth-code-flow). Falls Sie nach einem Tutorial zum impliziten Fluss suchen, können Sie das [Tutorial zu MSAL.js v1](https://docs.microsoft.com/azure/active-directory/develop/tutorial-v2-javascript-spa) durcharbeiten.

In dieser Version von MSAL.js wurde die aktuelle msal-core-Bibliothek verbessert und der Autorisierungscodefluss im Browser genutzt. Die meisten Features der alten Bibliothek sind auch in dieser Version verfügbar, aber für den Authentifizierungsfluss gelten jeweils leichte Abweichungen. In dieser Version wird der implizite Fluss **NICHT** unterstützt.

In diesem Leitfaden werden folgende Aktionen für eine JavaScript-Single-Page-Anwendung (SPA) beschrieben:
- Anmelden von persönlichen Konten sowie von Geschäfts-, Schul- oder Unikonten
- Abrufen eines Zugriffstokens
- Aufrufen der Microsoft Graph-API oder anderer APIs, für die Zugriffstoken des *Microsoft Identity Platform-Endpunkts* erforderlich sind

## <a name="how-the-sample-app-generated-by-this-guide-works"></a>Funktionsweise der über diesen Leitfaden generierten Beispiel-App

![Zeigt, wie die in diesem Tutorial generierte Beispiel-App funktioniert](media/active-directory-develop-guidedsetup-javascriptspa-introduction/javascriptspa-intro.svg)

### <a name="more-information"></a>Weitere Informationen

Die über diesen Leitfaden erstellte Beispiel-App ermöglicht einer JavaScript-SPA das Abfragen der Microsoft Graph-API oder einer Web-API, die Token vom Microsoft Identity Platform-Endpunkt akzeptiert. In diesem Szenario wird nach der Benutzeranmeldung ein Zugriffstoken angefordert und den HTTP-Anforderungen über den Autorisierungsheader hinzugefügt. Tokenabruf und -verlängerung werden von der Microsoft Authentication Library (MSAL) verarbeitet.

### <a name="libraries"></a>Bibliotheken

In diesem Leitfaden werden die folgenden Bibliotheken verwendet:

|Bibliothek|BESCHREIBUNG|
|---|---|
|[msal.js](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-browser)|Microsoft Authentication Library für JavaScript: Browserpaket|

## <a name="set-up-your-web-server-or-project"></a>Einrichten Ihres Webservers oder Projekts

Möchten Sie stattdessen das Projekt dieses Beispiels herunterladen? Klonen Sie die Projektdateien, wenn Sie das Projekt mit einem lokalen Webserver (z. B. Node.js) ausführen möchten:

`git clone https://github.com/Azure-Samples/ms-identity-javascript-v2`

Wenn Sie das Codebeispiel vor der Ausführung konfigurieren möchten, navigieren Sie direkt zum [Konfigurationsschritt](#register-your-application).

## <a name="prerequisites"></a>Voraussetzungen

* Zum Durcharbeiten dieses Tutorials benötigen Sie einen lokalen Webserver, z. B. [Node.js](https://nodejs.org/en/download/) oder [.NET Core](https://www.microsoft.com/net/core).

* Installieren Sie bei Verwendung von Node.js zum Ausführen des Projekts eine IDE (Integrated Development Environment, integrierte Entwicklungsumgebung), z. B. [Visual Studio Code](https://code.visualstudio.com/download), um die Projektdateien zu bearbeiten.

* Die Anleitungen in diesem Tutorial basieren auf Node.js.

## <a name="create-your-project"></a>Erstellen Ihres Projekts

Vergewissern Sie sicher, dass [Node.js](https://nodejs.org/en/download/) installiert ist, und erstellen Sie einen Ordner für Ihre Anwendung. Implementieren Sie als Nächstes einen kleinen [Express](https://expressjs.com/)-Webserver für die Bereitstellung Ihrer `index.html`-Datei.

1. Navigieren Sie zunächst im Terminal zu Ihrem Projektordner, und führen Sie dann die folgenden npm-Befehle aus.
    ```console
    npm init -y
    npm install @azure/msal-Browser
    npm install express
    ```
2. Erstellen Sie als Nächstes eine JS-Datei mit dem Namen *server.js*, und fügen Sie anschließend den folgenden Code hinzu:

   ```JavaScript
   const express = require('express');
   const morgan = require('morgan');
   const path = require('path');
   const argv = require('yargs')
      .usage('Usage: $0 -p [PORT]')
      .alias('p', 'port')
      .describe('port', '(Optional) Port Number - default is 3000')
      .strict()
      .argv;

   const DEFAULT_PORT = 3000;

   //initialize express.
   const app = express();

   // Initialize variables.
   let port = DEFAULT_PORT; // -p {PORT} || 3000;
   if (argv.p) {
      port = argv.p;
   }

   // Configure morgan module to log all requests.
   app.use(morgan('dev'));

   // Set the front-end folder to serve public assets.
   app.use("/lib", express.static(path.join(__dirname, "../../lib/msal-browser/lib")));

   // Setup app folders
   app.use(express.static('app'));

   // Set up a route for index.html.
   app.get('*', function (req, res) {
      res.sendFile(path.join(__dirname + '/index.html'));
   });

   // Start the server.
   app.listen(port);
   console.log(`Listening on port ${port}...`);
    ```

Sie verfügen jetzt über einen einfachen Server zum Bereitstellen Ihrer SPA. Die vorgesehene Ordnerstruktur am Ende dieses Tutorials lautet wie folgt:

![eine Textdarstellung der vorgesehenen SPA-Ordnerstruktur](./media/tutorial-v2-javascript-spa/single-page-application-folder-structure.png)

## <a name="create-the-spa-ui"></a>Erstellen der Benutzeroberfläche für die Single-Page-Webanwendung (SPA)

1. Erstellen Sie die Datei *index.html* für Ihre JavaScript-Single-Page-Webanwendung im Ordner *app*. Mit dieser Datei wird eine Benutzeroberfläche implementiert, die mit dem **Bootstrap 4-Framework** erstellt wurde, und es werden Skriptdateien für die Konfiguration, Authentifizierung und API-Aufrufe importiert.

   Fügen Sie der Datei *index.html* den folgenden Code hinzu:

   ```html
   <!DOCTYPE html>
   <html lang="en">
      <head>
         <meta charset="UTF-8">
         <meta name="viewport" content="width=device-width, initial-scale=1.0, shrink-to-fit=no">
         <title>Quickstart | MSAL.JS Vanilla JavaScript SPA</title>

         <!-- IE support: add promises polyfill before msal.js  -->
         <script type="text/javascript" src="//cdn.jsdelivr.net/npm/bluebird@3.7.2/js/browser/bluebird.min.js"></script>
         <script type="text/javascript" src="https://alcdn.msauth.net/browser/2.0.0-beta.0/js/msal-browser.js"></script>

         <!-- adding Bootstrap 4 for UI components  -->
         <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.4.1/css/bootstrap.min.css" integrity="sha384-Vkoo8x4CGsO3+Hhxv8T/Q5PaXtkKtu6ug5TOeNV6gBiFeWPGFN9MuhOf23Q9Ifjh" crossorigin="anonymous">
         <link rel="SHORTCUT ICON" href="https://c.s-microsoft.com/favicon.ico?v2" type="image/x-icon">
      </head>
      <body>
         <nav class="navbar navbar-expand-lg navbar-dark bg-primary">
            <a class="navbar-brand" href="/">MS Identity Platform</a>
            <div class="btn-group ml-auto dropleft">
               <button type="button" id="SignIn" class="btn btn-secondary" onclick="signIn()">
                  Sign In
               </button>
            </div>
         </nav>
         <br>
         <h5 class="card-header text-center">Vanilla JavaScript SPA calling MS Graph API with MSAL.JS</h5>
         <br>
         <div class="row" style="margin:auto" >
         <div id="card-div" class="col-md-3" style="display:none">
         <div class="card text-center">
            <div class="card-body">
            <h5 class="card-title" id="WelcomeMessage">Please sign-in to see your profile and read your mails</h5>
            <div id="profile-div"></div>
            <br>
            <br>
            <button class="btn btn-primary" id="seeProfile" onclick="seeProfile()">See Profile</button>
            <br>
            <br>
            <button class="btn btn-primary" id="readMail" onclick="readMail()">Read Mails</button>
            </div>
         </div>
         </div>
         <br>
         <br>
            <div class="col-md-4">
            <div class="list-group" id="list-tab" role="tablist">
            </div>
            </div>
            <div class="col-md-5">
             <div class="tab-content" id="nav-tabContent">
            </div>
            </div>
         </div>
         <br>
         <br>

         <!-- importing bootstrap.js and supporting js libraries -->
         <script src="https://code.jquery.com/jquery-3.4.1.slim.min.js" integrity="sha384-J6qa4849blE2+poT4WnyKhv5vZF5SrPo0iEjwBvKU7imGFAV0wwj1yYfoRSJoZ+n" crossorigin="anonymous"></script>
         <script src="https://cdn.jsdelivr.net/npm/popper.js@1.16.0/dist/umd/popper.min.js" integrity="sha384-Q6E9RHvbIyZFJoft+2mJbHaEWldlvI9IOYy5n3zV9zzTtmI3UksdQRVvoxMfooAo" crossorigin="anonymous"></script>
         <script src="https://stackpath.bootstrapcdn.com/bootstrap/4.4.1/js/bootstrap.min.js" integrity="sha384-wfSDF2E50Y2D1uUdj0O3uMBJnjuUD4Ih7YwaYd1iqfktj0Uod8GCExl3Og8ifwB6" crossorigin="anonymous"></script>

         <!-- importing app scripts (load order is important) -->
         <script type="text/javascript" src="./authConfig.js"></script>
         <script type="text/javascript" src="./graphConfig.js"></script>
         <script type="text/javascript" src="./ui.js"></script>

         <!-- <script type="text/javascript" src="./authRedirect.js"></script>   -->
         <!-- uncomment the above line and comment the line below if you would like to use the redirect flow -->
         <script type="text/javascript" src="./authPopup.js"></script>
         <script type="text/javascript" src="./graph.js"></script>
      </body>
     </html>
     ```

   > [!TIP]
   > Sie können die Version von „MSAL.js“ im obigen Skript durch die neueste veröffentlichte Version unter [MSAL.js-Releases](https://github.com/AzureAD/microsoft-authentication-library-for-js/releases) ersetzen.


2. Erstellen Sie nun eine JS-Datei mit dem Namen *ui.js*, die auf DOM-Elemente zugreift und diese aktualisiert, und fügen Sie den folgenden Code hinzu:

   ```JavaScript
   // Select DOM elements to work with
   const welcomeDiv = document.getElementById("welcomeMessage");
   const signInButton = document.getElementById("signIn");
   const signOutButton = document.getElementById('signOut');
   const cardDiv = document.getElementById("card-div");
   const mailButton = document.getElementById("readMail");
   const profileButton = document.getElementById("seeProfile");
   const profileDiv = document.getElementById("profile-div");

   function showWelcomeMessage(account) {
     // Reconfiguring DOM elements
     cardDiv.classList.remove('d-none');
     welcomeDiv.innerHTML = `Welcome ${account.name}`;
     signInButton.classList.add('d-none');
     signOutButton.classList.remove('d-none');
   }

   function updateUI(data, endpoint) {
     console.log('Graph API responded at: ' + new Date().toString());

     if (endpoint === graphConfig.graphMeEndpoint) {
       const title = document.createElement('p');
       title.innerHTML = "<strong>Title: </strong>" + data.jobTitle;
       const email = document.createElement('p');
       email.innerHTML = "<strong>Mail: </strong>" + data.mail;
       const phone = document.createElement('p');
       phone.innerHTML = "<strong>Phone: </strong>" + data.businessPhones[0];
       const address = document.createElement('p');
       address.innerHTML = "<strong>Location: </strong>" + data.officeLocation;
       profileDiv.appendChild(title);
       profileDiv.appendChild(email);
       profileDiv.appendChild(phone);
       profileDiv.appendChild(address);

     } else if (endpoint === graphConfig.graphMailEndpoint) {
         if (data.value.length < 1) {
           alert("Your mailbox is empty!")
         } else {
           const tabList = document.getElementById("list-tab");
           tabList.innerHTML = ''; // clear tabList at each readMail call
           const tabContent = document.getElementById("nav-tabContent");

           data.value.map((d, i) => {
             // Keeping it simple
             if (i < 10) {
               const listItem = document.createElement("a");
               listItem.setAttribute("class", "list-group-item list-group-item-action")
               listItem.setAttribute("id", "list" + i + "list")
               listItem.setAttribute("data-toggle", "list")
               listItem.setAttribute("href", "#list" + i)
               listItem.setAttribute("role", "tab")
               listItem.setAttribute("aria-controls", i)
               listItem.innerHTML = d.subject;
               tabList.appendChild(listItem)

               const contentItem = document.createElement("div");
               contentItem.setAttribute("class", "tab-pane fade")
               contentItem.setAttribute("id", "list" + i)
               contentItem.setAttribute("role", "tabpanel")
               contentItem.setAttribute("aria-labelledby", "list" + i + "list")
               contentItem.innerHTML = "<strong> from: " + d.from.emailAddress.address + "</strong><br><br>" + d.bodyPreview + "...";
               tabContent.appendChild(contentItem);
             }
           });
         }
     }
   }
   ```

## <a name="register-your-application"></a>Anwendung registrieren

Befolgen Sie die Anleitung zum [Registrieren einer neuen Single-Page-Webanwendung](https://docs.microsoft.com/zure/active-directory/develop/scenario-spa-app-registration).

#### <a name="set-a-redirect-url-for-nodejs"></a>Festlegen einer Umleitungs-URL für Node.js

Für Node.js können Sie den Webserverport in der Datei *server.js* festlegen. In diesem Tutorial wird zwar der Port 3000 verwendet, Sie können aber auch einen beliebigen anderen verfügbaren Port verwenden.

Wenn Sie eine Umleitungs-URL in den Registrierungsinformationen der Anwendung einrichten möchten, müssen Sie zurück zum Bereich **Anwendungsregistrierung** wechseln und eine neue **SPA** registrieren, indem Sie eine der folgenden Optionen auswählen:

- Legen Sie *`http://localhost:3000/`* als **Umleitungs-URL** fest.
- Wenn Sie einen benutzerdefinierten TCP-Port nutzen, verwenden Sie *`http://localhost:<port>/`* (wobei *\<port>* die benutzerdefinierte TCP-Portnummer ist).

### <a name="configure-your-javascript-spa"></a>Konfigurieren Ihrer JavaScript-SPA

Erstellen Sie eine neue JS-Datei mit dem Namen *authConfig.js*, die die Konfigurationsparameter für die Authentifizierung enthält, und fügen Sie den folgenden Code hinzu:

```javascript
  const msalConfig = {
    auth: {
      clientId: "Enter_the_Application_Id_Here",
      authority: "Enter_the_Cloud_Instance_Id_HereEnter_the_Tenant_Info_Here",
      redirectUri: "Enter_the_Redirect_Uri_Here",
    },
    cache: {
      cacheLocation: "sessionStorage", // This configures where your cache will be stored
      storeAuthStateInCookie: false, // Set this to "true" if you are having issues on IE11 or Edge
    }
  };

  // Add here scopes for id token to be used at MS Identity Platform endpoints.
  const loginRequest = {
   scopes: ["openid", "profile", "User.Read"]
  };

   // Add here scopes for access token to be used at MS Graph API endpoints.
  const tokenRequest = {
   scopes: ["User.Read", "Mail.Read"]
  };

```

 Hierbei gilt:
 - *\<Enter_the_Application_Id_Here>* ist die **Anwendungs-ID (Client)** für die von Ihnen registrierte Anwendung.
 - *\<Enter_the_Cloud_Instance_Id_Here>* ist die Instanz der Azure-Cloud. Geben Sie für die Azure-Hauptcloud oder für die globale Azure-Cloud einfach *https://login.microsoftonline.com* ein. Informationen zu **nationalen** Clouds (etwa für China) finden Sie unter [Nationale Clouds](https://docs.microsoft.com/azure/active-directory/develop/authentication-national-cloud).
 - *\<Enter_the_Tenant_info_here >* wird auf eine der folgenden Optionen festgelegt:
   - Wenn Ihre Anwendung *Nur Konten in diesem Organisationsverzeichnis* unterstützt, ersetzen Sie diesen Wert durch die **Mandanten-ID** oder den **Mandantennamen** (beispielsweise *contoso.microsoft.com*).
   - Wenn Ihre Anwendung *Konten in einem beliebigen Organisationsverzeichnis* unterstützt, ersetzen Sie diesen Wert durch **Organisationen**.
   - Wenn Ihre Anwendung *Konten in allen Organisationsverzeichnissen und persönliche Microsoft-Konten* unterstützt, ersetzen Sie diesen Wert durch **common**. Wenn Sie die Unterstützung *ausschließlich auf persönliche Microsoft-Konten* beschränken möchten, ersetzen Sie diesen Wert durch **consumers**.
- *\Enter_the_Redirect_Uri_Here>* ist der Port, den Sie im Portal registriert haben ( *`http://localhost:3000/`* ).


Erstellen Sie eine neue JS-Datei mit dem Namen `graphConfig.js`, die die Konfigurationsparameter zum Aufrufen der Microsoft Graph-API enthält, und fügen Sie den folgenden Code hinzu:
```javascript
// Add here the endpoints for MS Graph API services you would like to use.
const graphConfig = {
    graphMeEndpoint: "Enter_the_Graph_Endpoint_Herev1.0/me",
    graphMailEndpoint: "Enter_the_Graph_Endpoint_Herev1.0/me/messages"
};
```
- *\<Enter_the_Graph_Endpoint_Here>* ist die Instanz von der MS Graph-API. Ersetzen Sie diese Zeichenfolge für den globalen MS Graph-API-Endpunkt einfach durch `https://graph.microsoft.com`. Informationen zu Bereitstellungen in der nationalen Cloud finden Sie in der [Graph-API-Dokumentation](https://docs.microsoft.com/graph/deployments).

## <a name="use-the-microsoft-authentication-library-msal-to-sign-in-the-user"></a>Verwenden der MSAL (Microsoft Authentication Library) für die Benutzeranmeldung

### <a name="popup"></a>Popup
Erstellen Sie eine neue JS-Datei mit dem Namen `authPopup.js`, die Ihre Authentifizierungs- und Tokenerfassungslogik für die Popupanmeldung enthält, und fügen Sie den folgenden Code hinzu:

   ```JavaScript
  // Create the main myMSALObj instance
// configuration parameters are located at authConfig.js
const myMSALObj = new msal.PublicClientApplication(msalConfig);

function signIn() {
    myMSALObj.loginPopup(loginRequest)
        .then(loginResponse => {
            console.log('id_token acquired at: ' + new Date().toString());

            if (myMSALObj.getAccount()) {
                showWelcomeMessage(myMSALObj.getAccount());
            }
        }).catch(error => {
            console.error(error);
        });
}

function signOut() {
    myMSALObj.logout();
}

function getTokenPopup(request) {
    return myMSALObj.acquireTokenSilent(request)
        .catch(error => {
            console.warn(error);
            console.warn("silent token acquisition fails. acquiring token using popup");

            // fallback to interaction when silent call fails
            return myMSALObj.acquireTokenPopup(request)
                .then(tokenResponse => {
                    return tokenResponse;
                }).catch(error => {
                    console.error(error);
                });
        });
}

function seeProfile() {
    if (myMSALObj.getAccount()) {
        getTokenPopup(loginRequest)
            .then(response => {
                callMSGraph(graphConfig.graphMeEndpoint, response.accessToken, updateUI);
                profileButton.classList.add('d-none');
                mailButton.classList.remove('d-none');
            }).catch(error => {
                console.error(error);
            });
    }
}

function readMail() {
    if (myMSALObj.getAccount()) {
        getTokenPopup(tokenRequest)
            .then(response => {
                callMSGraph(graphConfig.graphMailEndpoint, response.accessToken, updateUI);
            }).catch(error => {
                console.error(error);
            });
    }
}
   ```
### <a name="redirect"></a>Umleiten
Erstellen Sie eine neue JS-Datei mit dem Namen `authRedirect.js`, die Ihre Authentifizierungs- und Tokenerfassungslogik für die Anmeldungsumleitung enthält, und fügen Sie den folgenden Code hinzu:

```javascript
// Create the main myMSALObj instance
// configuration parameters are located at authConfig.js
const myMSALObj = new msal.PublicClientApplication(msalConfig);

let accessToken;

// Register Callbacks for Redirect flow
myMSALObj.handleRedirectCallback(authRedirectCallBack);

function authRedirectCallBack(error, response) {
    if (error) {
        console.error(error);
    } else {
        if (myMSALObj.getAccount()) {
            console.log('id_token acquired at: ' + new Date().toString());
            showWelcomeMessage(myMSALObj.getAccount());
            getTokenRedirect(loginRequest);
        } else if (response.tokenType === "Bearer") {
            console.log('access_token acquired at: ' + new Date().toString());
        } else {
            console.log("token type is:" + response.tokenType);
        }
    }
}

// Redirect: once login is successful and redirects with tokens, call Graph API
if (myMSALObj.getAccount()) {
    showWelcomeMessage(myMSALObj.getAccount());
}

function signIn() {
    myMSALObj.loginRedirect(loginRequest);
}

function signOut() {
    myMSALObj.logout();
}

// This function can be removed if you do not need to support IE
function getTokenRedirect(request) {
    return myMSALObj.acquireTokenSilent(request)
        .then((response) => {
            console.log(response);
            if (response.accessToken) {
                console.log('access_token acquired at: ' + new Date().toString());
                accessToken = response.accessToken;

                callMSGraph(graphConfig.graphMeEndpoint, response.accessToken, updateUI);
                profileButton.style.display = 'none';
                mailButton.style.display = 'initial';
            }
        })
        .catch(error => {
            console.warn("silent token acquisition fails. acquiring token using redirect");
            // fallback to interaction when silent call fails
            return myMSALObj.acquireTokenRedirect(request);
        });
}

function seeProfile() {
    getTokenRedirect(loginRequest);
}

function readMail() {
    getTokenRedirect(tokenRequest);
}
```

### <a name="more-information"></a>Weitere Informationen

Wenn ein Benutzer das erste Mal die Schaltfläche **Anmelden** auswählt, ruft die `signIn`-Methode `loginPopup` auf, um den Benutzer anzumelden. Diese Methode öffnet ein Popupfenster mit dem *Microsoft Identity Platform-Endpunkt*, um die Anmeldeinformationen des Benutzers anzufordern und zu überprüfen. Nach einer erfolgreichen Anmeldung wird von *msal.js* der [Autorisierungcodefluss](https://docs.microsoft.com/azure/active-directory/develop/v2-oauth2-auth-code-flow) initiiert.

An diesem Punkt wird ein mit PKCE geschützter Autorisierungscode an den mit CORS geschützten Tokenendpunkt gesendet und gegen Token ausgetauscht. Ein ID-Token, Zugriffstoken und Aktualisierungstoken werden empfangen und von *msal.js* verarbeitet, und die im Token enthaltenen Informationen werden jeweils zwischengespeichert.

Das ID-Token enthält grundlegende Informationen zum Benutzer, z. B. den Anzeigenamen. Wenn Sie planen, die von diesem Token bereitgestellten Daten zu nutzen, muss es von Ihrem Back-End-Server überprüft werden. Hierbei wird sichergestellt, dass das Token für einen gültigen Benutzer Ihrer Anwendung ausgestellt wurde. Das Aktualisierungstoken verfügt über eine begrenzte Lebensdauer und läuft nach 24 Stunden ab. Über das Aktualisierungstoken können im Hintergrund neue Zugriffstoken bezogen werden.

Die in diesem Leitfaden generierte SPA ruft `acquireTokenSilent` bzw. `acquireTokenPopup` auf, um ein *Zugriffstoken* zu beziehen, das zum Abfragen von Informationen zum Benutzerprofil von der Microsoft Graph-API verwendet wird. Ein Beispiel zur Überprüfung des ID-Tokens finden Sie in der Beispielanwendung [active-directory-javascript-singlepageapp-dotnet-webapi-v2](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi-v2) auf GitHub. In diesem Beispiel wird für die Tokenüberprüfung eine ASP.NET-Web-API verwendet.

#### <a name="get-a-user-token-interactively"></a>Interaktives Abrufen eines Benutzertokens

Nach der erstmaligen Anmeldung sollten die Benutzer nicht jedes Mal erneut zur Authentifizierung aufgefordert werden, wenn sie ein Token für den Zugriff auf eine Ressource anfordern müssen. Verwenden Sie `acquireTokenSilent`, um diese Aufforderungen zur erneuten Authentifizierung zu verhindern. Es gibt aber einige Situationen, in denen Sie die Interaktion mit dem Microsoft Identity Platform-Endpunkt ggf. erzwingen müssen. Beispiel:

- Benutzer müssen ihre Anmeldeinformationen erneut eingeben, weil das Kennwort abgelaufen ist.
- Ihre Anwendung fordert Zugriff auf eine Ressource an, und die Einwilligung des Benutzers ist erforderlich.
- Die zweistufige Authentifizierung ist erforderlich.

Beim Aufrufen von `acquireTokenPopup` wird ein Popupfenster geöffnet (oder Benutzer werden über `acquireTokenRedirect` an den Microsoft Identity Platform-Endpunkt umgeleitet). In diesem Fenster müssen Benutzer ihre Anmeldeinformationen bestätigen, ihre Zustimmung für die erforderliche Ressource geben oder die zweistufige Authentifizierung durchführen.

#### <a name="get-a-user-token-silently"></a>Automatisches Abrufen eines Benutzertokens

Die Methode `acquireTokenSilent` wickelt den Bezug und die Verlängerung von Token ohne Eingreifen des Benutzers ab. Nach erstmaligem Ausführen von `loginPopup` (oder `loginRedirect`) wird normalerweise die Methode `acquireTokenSilent` zum Beziehen von Token verwendet, die für den Zugriff auf geschützte Ressourcen für nachfolgende Aufrufe genutzt werden. (Aufrufe zum Anfordern oder Verlängern von Token werden im Hintergrund ausgeführt.) `acquireTokenSilent` ist in bestimmten Fällen möglicherweise nicht erfolgreich – etwa, wenn das Kennwort des Benutzers abgelaufen ist. Ihre Anwendung kann diese Ausnahme auf zwei Arten handhaben:

1. Durch sofortiges Aufrufen von `acquireTokenPopup`, um eine Aufforderung zur Benutzeranmeldung auszulösen. Dieses Muster wird in der Regel in Onlineanwendungen verwendet, in denen kein nicht authentifizierter Inhalt in der Anwendung für den Benutzer verfügbar ist. Die in diesem Leitfaden generierte Beispielanwendung verwendet dieses Muster.

1. Durch eine visuelle Benachrichtigung mit dem Hinweis, dass eine interaktive Anmeldung erforderlich ist, damit der Benutzer den richtigen Zeitpunkt zum Anmelden auswählen oder damit die Anwendung `acquireTokenSilent` zu einem späteren Zeitpunkt wiederholen kann. Dieses Verfahren wird normalerweise verwendet, wenn der Benutzer andere Funktionen der Anwendung nutzen kann, ohne unterbrochen zu werden – beispielsweise, wenn in der Anwendung nicht authentifizierte Inhalte vorhanden sind. In diesem Fall kann der Benutzer entscheiden, wann er sich anmelden möchte, um auf die geschützte Ressource zuzugreifen oder die veralteten Informationen zu aktualisieren.

> [!NOTE]
> In diesem Schnellstart werden standardmäßig die Methoden `loginPopup` und `acquireTokenPopup` verwendet. Wenn Sie Internet Explorer als Browser verwenden, empfiehlt es sich, die Methoden `loginRedirect` und `acquireTokenRedirect` zu verwenden, da ein [bekanntes](https://github.com/AzureAD/microsoft-authentication-library-for-js/wiki/Known-issues-on-IE-and-Edge-Browser#issues) Problem damit vorliegt, wie Popupfenster in Internet Explorer behandelt werden. Wenn Sie erfahren möchten, wie Sie per Umleitung das gleiche Ergebnis erzielen, hilft Ihnen die Seite [*authRedirect.js*](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2/blob/quickstart/JavaScriptSPA/authRedirect.js) weiter.

## <a name="call-the-microsoft-graph-api-by-using-the-token-you-just-acquired"></a>Aufrufen der Microsoft Graph-API unter Verwendung des zuvor bezogenen Tokens


 Erstellen Sie eine JS-Datei mit dem Namen *graph.js*, die einen REST-Aufruf für die Microsoft Graph-API durchführt, und fügen Sie den folgenden Code hinzu:

   ```javascript

// Helper function to call MS Graph API endpoint
// using authorization bearer token scheme
function callMSGraph(endpoint, token, callback) {
    const headers = new Headers();
    const bearer = `Bearer ${token}`;

    headers.append("Authorization", bearer);

    const options = {
        method: "GET",
        headers: headers
    };

    console.log('request made to Graph API at: ' + new Date().toString());

    fetch(endpoint, options)
        .then(response => response.json())
        .then(response => callback(response, endpoint))
        .catch(error => console.log(error));
}
   ```

### <a name="more-information-about-making-a-rest-call-against-a-protected-api"></a>Weitere Informationen zum Richten eines REST-Aufrufs an eine geschützte API

In der Beispielanwendung aus diesem Leitfaden wird die Methode `callMSGraph()` verwendet, um eine HTTP-Anforderung vom Typ `GET` für eine geschützte Ressource auszuführen, die ein Token erfordert. Der Inhalt wird anschließend an den Aufrufer zurückgegeben. Diese Methode fügt das abgerufene Token in den *HTTP-Autorisierungsheader* ein. In der Beispielanwendung aus diesem Leitfaden ist die Ressource der Endpunkt *me* der Microsoft Graph-API, der die Profilinformationen des Benutzers anzeigt.

## <a name="test-your-code"></a>Testen Ihres Codes

1. Starten Sie für Node.js den Webserver, indem Sie im Anwendungsordner die folgenden Befehle ausführen:

   ```bash
   npm install
   npm start
   ```
1. Geben Sie in Ihrem Browser **http://localhost:3000** oder **http://localhost:{port}** ein, wobei *port* der Port ist, an dem der Webserver lauscht. Der Inhalt der Datei *index.html* und die Schaltfläche **Anmelden** sollten angezeigt werden.

## <a name="test-your-application"></a>Testen Ihrer Anwendung

Nachdem der Browser die Datei *index.html* geladen hat, klicken Sie auf **Anmelden**. Sie werden aufgefordert, sich mit dem Microsoft Identity Platform-Endpunkt anzumelden:

![Das Fenster zum Anmelden bei Ihrem JavaScript SPA-Konto](media/active-directory-develop-guidedsetup-javascriptspa-test/javascriptspascreenshot1.png)

### <a name="provide-consent-for-application-access"></a>Zustimmen zum Anwendungszugriff

Wenn Sie sich zum ersten Mal bei Ihrer Anwendung anmelden, werden Sie aufgefordert, ihr Zugriff auf Ihr Profil zu gewähren und sich anzumelden:

![Das Fenster „Angeforderte Berechtigungen“](media/active-directory-develop-guidedsetup-javascriptspa-test/javascriptspaconsent.png)

### <a name="view-application-results"></a>Anzeigen von Anwendungsergebnissen

Nach der Anmeldung werden Ihre Benutzerprofilinformationen in der angezeigten Antwort der Microsoft Graph-API zurückgegeben:

![Ergebnisse des Aufrufs der Microsoft Graph-API](media/active-directory-develop-guidedsetup-javascriptspa-test/javascriptsparesults.png)

### <a name="more-information-about-scopes-and-delegated-permissions"></a>Weitere Informationen zu Bereichen und delegierten Berechtigungen

Die Microsoft Graph-API benötigt den Bereich *user.read*, um das Benutzerprofil zu lesen. Dieser Bereich wird standardmäßig jeder Anwendung automatisch hinzugefügt, die im Azure-Portal registriert ist. Andere Microsoft Graph-APIs sowie benutzerdefinierte APIs für Ihren Back-End-Server erfordern unter Umständen zusätzliche Bereiche. Die Microsoft Graph-API benötigt beispielsweise den Bereich *Mail.Read*, um die E-Mails des Benutzers aufzuführen.

> [!NOTE]
> Wenn Sie Bereiche hinzufügen, erhält der Benutzer unter Umständen weitere Aufforderungen zur Erteilung seiner Einwilligung.

Wenn eine Back-End-API keinen Bereich benötigt (nicht empfohlen), können Sie in den Aufrufen zum Beziehen von Token *clientId* als Bereich verwenden.

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]

## <a name="next-steps"></a>Nächste Schritte

Das [GitHub-Repository für MSAL.js](https://github.com/AzureAD/microsoft-authentication-library-for-js) enthält zusätzliche Bibliotheksdokumentation, häufig gestellte Fragen und Unterstützung bei Problemen.
