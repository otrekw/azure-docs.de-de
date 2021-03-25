---
title: Behandeln von Fehlern und Ausnahmen in MSAL.js
titleSuffix: Microsoft identity platform
description: Erfahren Sie, wie Sie Fehler und Ausnahmen, Anspruchsaufforderungen für den bedingten Zugriff und Wiederholversuche in MSAL.js-Anwendungen verarbeiten.
services: active-directory
author: mmacy
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 11/26/2020
ms.author: marsma
ms.reviewer: saeeda, hahamil
ms.custom: aaddev
ms.openlocfilehash: 20d276aba2ee3260911748cbee0a16020270059a
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "98761340"
---
# <a name="handle-errors-and-exceptions-in-msaljs"></a>Behandeln von Fehlern und Ausnahmen in MSAL.js

[!INCLUDE [Active directory error handling introduction](../../../includes/active-directory-develop-error-handling-introduction.md)]

## <a name="error-handling-in-msaljs"></a>Fehlerbehandlung in MSAL.js

MSAL.js stellt Fehlerobjekte bereit, mit denen die unterschiedlichen Typen allgemeiner Fehler abstrahiert und klassifiziert werden. Außerdem bietet es eine Schnittstelle für den Zugriff auf bestimmte Fehlerdetails (z. B. Fehlermeldungen), damit Sie sie entsprechend behandeln können.

### <a name="error-object"></a>Error-Objekt

```javascript
export class AuthError extends Error {
    // This is a short code describing the error
    errorCode: string;
    // This is a descriptive string of the error,
    // and may also contain the mitigation strategy
    errorMessage: string;
    // Name of the error class
    this.name = "AuthError";
}
```

Durch die Erweiterung der Fehlerklasse haben Sie Zugriff auf die folgenden Eigenschaften:
- `AuthError.message`:  Identisch mit `errorMessage`.
- `AuthError.stack`: Die Stapelüberwachung für ausgelöste Fehler.

### <a name="error-types"></a>Fehlertypen

Folgende Fehlertypen sind verfügbar:

- `AuthError`: Die grundlegende Fehlerklasse für die MSAL.js-Bibliothek. Sie wird auch für unerwartete Fehler verwendet.

- `ClientAuthError`: Die Fehlerklasse, die auf ein Problem mit der Clientauthentifizierung hinweist. Bei den meisten Fehlern, die von der Bibliothek gemeldet werden, handelt es sich um ClientAuthErrors. Diese Fehler resultieren z.B. aus dem Aufrufen einer Anmeldemethode, wenn die Anmeldung bereits ausgeführt wird, oder dem Abbrechen der Anmeldung durch den Benutzer.

- `ClientConfigurationError`: Fehlerklasse zur Erweiterung von `ClientAuthError`, wenn dieser Fehler ausgelöst wird, bevor Anforderungen ausgeführt werden, wenn die angegebenen Benutzerkonfigurationsparameter falsch formatiert oder nicht vorhanden sind.

- `ServerError`: Fehlerklasse, die die vom Authentifizierungsserver gesendeten Fehlerzeichenfolgen darstellt. Bei den Fehlern kann es sich um ungültige Anforderungsformate oder -parameter sowie um andere Fehler handeln, die den Server an der Authentifizierung oder Autorisierung des Benutzers hindern.

- `InteractionRequiredAuthError`: Fehlerklasse, die `ServerError` erweitert und Serverfehler darstellt, die einen interaktiven Aufruf erfordern. Dieser Fehler wird durch `acquireTokenSilent` ausgelöst, wenn der Benutzer mit dem Server interagieren muss, um Anmeldeinformationen anzugeben oder der Authentifizierung/Autorisierung zuzustimmen. Zu den Fehlercodes zählen `"interaction_required"`, `"login_required"` und `"consent_required"`.

Zur Fehlerbehandlung in Authentifizierungsflows mit Umleitungsmethoden (`loginRedirect`, `acquireTokenRedirect`) müssen Sie den Rückruf, der nach der Umleitung erfolgreich oder mit einem Fehler aufgerufen wird, mithilfe der `handleRedirectCallback()`-Methode wie folgt registrieren:

```javascript
function authCallback(error, response) {
    //handle redirect response
}

var myMSALObj = new Msal.UserAgentApplication(msalConfig);

// Register Callbacks for redirect flow
myMSALObj.handleRedirectCallback(authCallback);
myMSALObj.acquireTokenRedirect(request);
```

Die Methoden für Popupfunktionen (`loginPopup`, `acquireTokenPopup`) geben Zusagen zurück. Auf diese Weise können Sie das Zusagemuster („.then“ und „.catch“) verwenden, um die Methoden zu behandeln, wie im Folgenden dargestellt:

```javascript
myMSALObj.acquireTokenPopup(request).then(
    function (response) {
        // success response
    }).catch(function (error) {
        console.log(error);
    });
```

### <a name="errors-that-require-interaction"></a>Fehler, für die eine Interaktion erforderlich ist

Ein Fehler wird zurückgegeben, wenn Sie versuchen, eine nicht interaktive Methode (z.B. `acquireTokenSilent`) für den Tokenabruf zu verwenden, MSAL das Token aber nicht automatisch abrufen konnte.

Mögliche Gründe:

- Sie müssen sich anmelden.
- Sie müssen Ihre Zustimmung geben.
- Sie müssen eine mehrstufige Authentifizierung ausführen.

Die Lösung besteht darin, eine interaktive Methode wie `acquireTokenPopup` oder `acquireTokenRedirect` aufzurufen:

```javascript
// Request for Access Token
myMSALObj.acquireTokenSilent(request).then(function (response) {
    // call API
}).catch( function (error) {
    // call acquireTokenPopup in case of acquireTokenSilent failure
    // due to consent or interaction required
    if (error.errorCode === "consent_required"
    || error.errorCode === "interaction_required"
    || error.errorCode === "login_required") {
        myMSALObj.acquireTokenPopup(request).then(
            function (response) {
                // call API
            }).catch(function (error) {
                console.log(error);
            });
    }
});
```

[!INCLUDE [Active directory error handling claims challenges](../../../includes/active-directory-develop-error-handling-claims-challenges.md)]

Beim automatischen Abruf von Token mit MSAL.js (und `acquireTokenSilent`) kann Ihre Anwendung Fehlermeldungen empfangen, wenn für eine API, auf die Sie zugreifen möchten, eine [Anspruchsaufforderung für bedingten Zugriff](../azuread-dev/conditional-access-dev-guide.md), wie z. B. eine MFA-Richtlinie, erforderlich ist.

Sie behandeln diesen Fehler in der Regel dadurch, dass Sie einen interaktiven Aufruf ausführen, um ein Token in MSAL.js abzurufen, z. B. mit `acquireTokenPopup` oder `acquireTokenRedirect`. Siehe folgendes Beispiel:

```javascript
myMSALObj.acquireTokenSilent(accessTokenRequest).then(function(accessTokenResponse) {
    // call API
}).catch(function(error) {
    if (error instanceof InteractionRequiredAuthError) {
    
        // extract, if exists, claims from error message
        if (error.ErrorMessage.claims) {
            accessTokenRequest.claimsRequest = JSON.stringify(error.ErrorMessage.claims);
        }
        
        // call acquireTokenPopup in case of InteractionRequiredAuthError failure
        myMSALObj.acquireTokenPopup(accessTokenRequest).then(function(accessTokenResponse) {
            // call API
        }).catch(function(error) {
            console.log(error);
        });
    }
});
```

Beim interaktiven Abruf des Tokens erhält der Benutzer in Form einer Aufforderung die Möglichkeit, die erforderliche Richtlinie für bedingten Zugriff zu erfüllen.

Beim Aufruf einer API, die bedingten Zugriff erfordert, können Sie in dem von der API ausgegebenen Fehler eine Anspruchsaufforderung erhalten. In diesem Fall können Sie die im Fehler zurückgegebenen Ansprüche an das `claimsRequest`-Feld der `AuthenticationParameters.ts`-Klasse übergeben, um die entsprechende Richtlinie zu erfüllen. 

Weitere Details finden Sie unter [Anfordern zusätzlicher Ansprüche](active-directory-optional-claims.md).


[!INCLUDE [Active directory error handling retries](../../../includes/active-directory-develop-error-handling-retries.md)]

## <a name="next-steps"></a>Nächste Schritte

Erwägen Sie die Aktivierung der [Protokollierung in MSAL.js](msal-logging-js.md), um Probleme besser diagnostizieren und debuggen zu können.
