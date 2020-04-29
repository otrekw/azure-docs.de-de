---
title: Verwenden von MSAL mit Azure Active Directory B2C – Learn | Azure
titleSuffix: Microsoft identity platform
description: Microsoft Authentication Library für JavaScript (MSAL.js) ermöglicht es Anwendungen, mit Azure AD B2C zusammenzuarbeiten und Token zum Aufrufen gesicherter Web-APIs abzurufen. Bei diesen Web-APIs kann es sich um Microsoft Graph, andere Microsoft-APIs, Web-APIs von Dritten oder um Ihre eigene Web-API handeln.
services: active-directory
author: negoe
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 09/16/2019
ms.author: negoe
ms.reviewer: nacanuma
ms.custom: aaddev
ms.openlocfilehash: 8e076dfd6670265d458eb35d8e1b3e4500009a12
ms.sourcegitcommit: 31ef5e4d21aa889756fa72b857ca173db727f2c3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2020
ms.locfileid: "81534481"
---
# <a name="use-microsoft-authentication-library-for-javascript-to-work-with-azure-active-directory-b2c"></a>Verwenden von Microsoft Authentication Library für JavaScript für die Zusammenarbeit mit Azure Active Directory B2C

[Microsoft Authentication Library für JavaScript (MSAL.js)](https://github.com/AzureAD/microsoft-authentication-library-for-js) ermöglicht es JavaScript-Entwicklern, Benutzer mit Social-Media-Identitäten und lokalen Identitäten mithilfe von [Azure Active Directory B2C (Azure AD B2C)](https://docs.microsoft.com/azure/active-directory-b2c/) zu authentifizieren. Wenn Sie Azure AD B2C als Identitätsverwaltungsdienst verwenden, können Sie anpassen und steuern, wie Kunden sich registrieren und anmelden und wie sie ihre Profile verwalten, wenn sie Ihre Anwendungen verwenden.

Azure AD B2C ermöglicht Ihnen auch das Branding und Anpassen der Benutzeroberfläche Ihrer Anwendungen beim Authentifizierungsprozess, um Ihren Kunden eine nahtlose Umgebung zu bieten.

In diesem Artikel wird gezeigt, wie Sie MSAL.js verwenden, um mit Azure AD B2C zu arbeiten. Außerdem werden wichtige Punkte zusammengefasst, die Sie berücksichtigen sollten. Eine ausführliche Erläuterung und ein Tutorial finden Sie in der [Azure AD B2C-Dokumentation](https://docs.microsoft.com/azure/active-directory-b2c/overview).

## <a name="prerequisites"></a>Voraussetzungen

Wenn Sie nicht bereits einen eigenen [Azure AD B2C-Mandanten](https://docs.microsoft.com/azure/active-directory-b2c/tutorial-create-tenant) erstellt haben, erstellen Sie zunächst einen. (Wenn Sie bereits über einen Azure AD B2C-Mandanten verfügen, können Sie auch diesen verwenden.)

Diese Demonstration besteht aus zwei Teilen:

- Schützen einer Web-API
- Registrieren einer Single-Page-Webanwendung zum Authentifizieren und Aufrufen *dieser* Web-API

## <a name="nodejs-web-api"></a>Node.js-Web-API

> [!NOTE]
> Aktuell befindet sich MSAL.js für Node noch in der Entwicklungsphase (siehe [Roadmap](https://github.com/AzureAD/microsoft-authentication-library-for-js/wiki#roadmap)). In der Zwischenzeit wird empfohlen, [passport-azure-ad](https://github.com/AzureAD/passport-azure-ad) zu verwenden, eine Authentifizierungsbibliothek für Node.js, die von Microsoft entwickelt und unterstützt wird.

In den folgenden Schritten wird gezeigt, wie eine **Web-API** Azure AD B2C verwenden kann, um sich selbst zu schützen und ausgewählte Bereiche für eine Clientanwendung verfügbar zu machen.

### <a name="step-1-register-your-application"></a>Schritt 1: Anwendung registrieren

Sie müssen Ihre Web-API erst registrieren, um sie mit Azure AD B2C schützen zu können. Ausführliche Schritte hierzu finden Sie unter [Registrieren Ihrer Anwendung](https://docs.microsoft.com/azure/active-directory-b2c/add-web-application?tabs=applications).

### <a name="step-2-download-the-sample-application"></a>Schritt 2: Herunterladen der Beispielanwendung

Laden Sie die Beispielanwendung als ZIP-Datei herunter, oder klonen Sie sie aus GitHub:

```console
git clone https://github.com/Azure-Samples/active-directory-b2c-javascript-nodejs-webapi.git
```

### <a name="step-3-configure-authentication"></a>Schritt 3: Konfigurieren der Authentifizierung

1. Öffnen Sie die Datei `config.js` im Beispiel.

2. Konfigurieren Sie das Beispiel mit den Anwendungsanmeldeinformationen, die Sie zuvor bei der Registrierung Ihrer Anwendung erhalten haben. Ändern Sie die folgenden Codezeilen, indem Sie die entsprechenden Werte durch Ihre Client-ID, Ihren Host, Ihre Mandanten-ID und den Richtliniennamen ersetzen.

```JavaScript
const clientID = "<Application ID for your Node.js web API - found on Properties page in Azure portal e.g. 93733604-cc77-4a3c-a604-87084dd55348>";
const b2cDomainHost = "<Domain of your B2C host eg. fabrikamb2c.b2clogin.com>";
const tenantId = "<your-tenant-ID>.onmicrosoft.com"; // Alternatively, you can use your Directory (tenant) ID (GUID)
const policyName = "<Name of your sign in / sign up policy, e.g. B2C_1_signupsignin1>";
```

Weitere Informationen finden Sie in diesem [Beispiel für eine Node.js-B2C-Web-API](https://github.com/Azure-Samples/active-directory-b2c-javascript-nodejs-webapi).

---

## <a name="javascript-spa"></a>JavaScript SPA

Die folgenden Schritte veranschaulichen, wie eine **Single-Page-Webanwendung** Azure AD B2C zum Registrieren und Anmelden sowie zum Aufrufen einer geschützten Web-API verwenden kann.

### <a name="step-1-register-your-application"></a>Schritt 1: Anwendung registrieren

Sie müssen zuerst Ihre Anwendung registrieren, um die Authentifizierung zu implementieren. Ausführliche Schritte hierzu finden Sie unter [Registrieren Ihrer Anwendung](https://docs.microsoft.com/azure/active-directory-b2c/tutorial-register-applications).

### <a name="step-2-download-the-sample-application"></a>Schritt 2: Herunterladen der Beispielanwendung

Laden Sie die Beispielanwendung als ZIP-Datei herunter, oder klonen Sie sie aus GitHub:

```console
git clone https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp.git
```

### <a name="step-3-configure-authentication"></a>Schritt 3: Konfigurieren der Authentifizierung

Es gibt zwei Punkte, die beim Konfigurieren Ihrer Anwendung von Interesse sind:

- Konfigurieren des API-Endpunkts und von verfügbar gemachten Bereichen
- Konfigurieren von Authentifizierungsparametern und Tokenbereichen

1. Öffnen Sie die Datei `apiConfig.js` im Beispiel.

2. Konfigurieren Sie das Beispiel mit den Parametern, die Sie zuvor bei der Registrierung Ihrer Web-API erhalten haben. Ändern Sie die folgenden Codezeilen, indem Sie die entsprechenden Werte durch die Adresse Ihrer Web-API und die verfügbar gemachten Bereiche ersetzen.

   ```javascript
    // The current application coordinates were pre-registered in a B2C tenant.
    const apiConfig = {
        b2cScopes: ["https://fabrikamb2c.onmicrosoft.com/helloapi/demo.read"], //API scopes you exposed during api registration
        webApi: "https://fabrikamb2chello.azurewebsites.net/hello"
    };
   ```

3. Öffnen Sie die Datei `authConfig.js` im Beispiel.

4. Konfigurieren Sie das Beispiel mit den Parametern, die Sie zuvor bei der Registrierung Ihrer Single-Page-Webanwendung erhalten haben. Ändern Sie die folgenden Codezeilen, indem Sie die entsprechenden Werte durch Ihre Client-ID, Autoritätsmetadaten und Bereiche für die Tokenanforderung ersetzen.

   ```javascript
    // Config object to be passed to Msal on creation.
    const msalConfig = {
        auth: {
            clientId: "e760cab2-b9a1-4c0d-86fb-ff7084abd902",
            authority: "https://fabrikamb2c.b2clogin.com/fabrikamb2c.onmicrosoft.com/B2C_1_signupsignin1",
            validateAuthority: false
        },
        cache: {
            cacheLocation: "localStorage", // This configures where your cache will be stored
            storeAuthStateInCookie: false // Set this to "true" to save cache in cookies
        }
    };

    // Add here scopes for id token to be used at the MS Identity Platform endpoint
    const loginRequest = {
        scopes: ["openid", "profile"],
    };
   ```

Weitere Informationen finden Sie in diesem [Beispiel für eine JavaScript-B2C-Single-Page-Webanwendung](https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp).

---

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen:
- [Benutzerabläufe](https://docs.microsoft.com/azure/active-directory-b2c/tutorial-create-user-flows)
- [Benutzerdefinierte Richtlinien](https://docs.microsoft.com/azure/active-directory-b2c/custom-policy-get-started)
- [Anpassung der Benutzererfahrung](https://docs.microsoft.com/azure/active-directory-b2c/custom-policy-configure-user-input)
