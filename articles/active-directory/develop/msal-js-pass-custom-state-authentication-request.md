---
title: Übergeben eines benutzerdefinierten state-Parameters in Authentifizierungsanforderungen (MSAL.js) | Azure
titleSuffix: Microsoft identity platform
description: Erfahren Sie, wie Sie einen benutzerdefinierten state-Parameterwert in einer Authentifizierungsanforderung mithilfe der Microsoft Authentication Library für JavaScript (MSAL.js) übergeben.
services: active-directory
author: mmacy
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: how-to
ms.workload: identity
ms.date: 01/16/2020
ms.author: marsma
ms.reviewer: saeeda
ms.custom: aaddev
ms.openlocfilehash: 840c371e63aacf8ef410cbf84cc9f68137dd77df
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "85477582"
---
# <a name="pass-custom-state-in-authentication-requests-using-msaljs"></a>Übergeben eines benutzerdefinierten state-Parameters in Authentifizierungsanforderungen mithilfe von MSAL.js

Der *state*-Parameter gemäß OAuth 2.0 ist in einer Authentifizierungsanforderung enthalten und wird auch in der Tokenantwort zurückgegeben, um Angriffe in Form siteübergreifender Anforderungsfälschungen zu verhindern. Die Microsoft Authentication Library für JavaScript (MSAL.js) übergibt standardmäßig einen zufällig generierten eindeutigen *state*-Parameterwert in Authentifizierungsanforderungen.

Der state-Parameter kann auch verwendet werden, um vor der Umleitung Zustandsinformationen zur App zu codieren. Sie können den Zustand des Benutzers in der App, z. B. die Seite oder Ansicht, zu der er navigiert ist, als Eingabe für diesen Parameter übergeben. Die MSAL.js-Bibliothek bietet die Möglichkeit, Ihren benutzerdefinierten Zustand als state-Parameter im `Request`-Objekt zu übergeben:

```javascript
// Request type
export type AuthenticationParameters = {
    scopes?: Array<string>;
    extraScopesToConsent?: Array<string>;
    prompt?: string;
    extraQueryParameters?: QPDict;
    claimsRequest?: string;
    authority?: string;
    state?: string;
    correlationId?: string;
    account?: Account;
    sid?: string;
    loginHint?: string;
    forceRefresh?: boolean;
};
```

> [!Note]
> Wenn Sie ein zwischengespeichertes Token überspringen und zum Server wechseln möchten, übergeben Sie den booleschen Wert `forceRefresh` in das AuthenticationParameters-Objekt, das zum Erstellen einer Anmelde-/Tokenanforderung verwendet wird.
> `forceRefresh` sollte aufgrund der Leistungsauswirkungen auf Ihre Anwendung nicht standardmäßig verwendet werden.
> Wenn Sie sich auf den Cache verlassen, erhalten Ihre Benutzer eine bessere Benutzererfahrung.
> Das Überspringen des Caches sollte nur in Szenarien verwendet werden, in denen Sie wissen, dass die aktuell zwischengespeicherten Daten keine aktuellen Informationen enthalten.
> Zum Beispiel muss ein Verwaltungs Tool, das einem Benutzer Rollen hinzufügt, ein neues Token mit aktualisierten Rollen abrufen.

Beispiel:

```javascript
let loginRequest = {
    scopes: ["user.read", "user.write"],
    state: "page_url"
}

myMSALObj.loginPopup(loginRequest);
```

Der übergebene Zustand wird an die durch MSAL.js festgelegte eindeutige GUID angefügt, wenn die Anforderung gesendet wird. Wenn die Antwort zurückgegeben wird, überprüft MSAL.js, ob Zustände übereinstimmen, und gibt dann den übergebenen benutzerdefinierten Zustand im `Response`-Objekt als `accountState` zurück.

```javascript
export type AuthResponse = {
    uniqueId: string;
    tenantId: string;
    tokenType: string;
    idToken: IdToken;
    accessToken: string;
    scopes: Array<string>;
    expiresOn: Date;
    account: Account;
    accountState: string;
};
```

Weitere Informationen finden Sie im Thema zum [Erstellen einer Single-Page-Anwendung (SPA)](scenario-spa-overview.md) mithilfe von MSAL.js.