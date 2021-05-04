---
title: Abrufen eines Tokens zum Aufrufen einer Web-API (Single-Page-Apps) | Azure
titleSuffix: Microsoft identity platform
description: Informationen zum Erstellen einer Single-Page-Webanwendung (Abrufen eines Tokens zum Aufrufen einer API)
services: active-directory
author: negoe
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 08/20/2019
ms.author: negoe
ms.custom: aaddev
ms.openlocfilehash: d3fe7369d3463b8508f65345729898481815070f
ms.sourcegitcommit: ad921e1cde8fb973f39c31d0b3f7f3c77495600f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/25/2021
ms.locfileid: "107947571"
---
# <a name="single-page-application-acquire-a-token-to-call-an-api"></a>Single-Page-Webanwendung: Abrufen eines Tokens zum Aufrufen einer API

Das Muster für das Abrufen von Token für APIs mit MSAL.js besteht darin, zuerst zu versuchen, eine automatische Tokenanforderung mithilfe der `acquireTokenSilent`-Methode zu senden. Wenn diese Methode aufgerufen wird, überprüft die Bibliothek zuerst den Cache im Browserspeicher, um festzustellen, ob ein gültiges Token vorhanden ist, und dann wird das Token zurückgegeben. Wenn kein gültiges Token im Cache vorhanden ist, sendet sie aus einem ausgeblendeten IFrame eine automatische Tokenanforderung an Azure Active Directory (Azure AD). Diese Methode ermöglicht der Bibliothek auch, Token zu erneuern. Weitere Informationen zu SSO-Sitzungen und Werten für die Tokengültigkeitsdauer in Azure AD finden Sie unter [Konfigurierbare Tokengültigkeitsdauern in Azure Active Directory (Vorschau)](active-directory-configurable-token-lifetimes.md).

Beim Senden von automatischen Tokenanforderungen an Azure AD können beispielsweise aufgrund einer abgelaufenen Azure AD-Sitzung oder einer Kennwortänderung Fehler auftreten. In diesem Fall können Sie eine der interaktiven Methoden aufrufen (die den Benutzer zur Eingabe auffordern), um Token abzurufen:

* [Popupfenster](#acquire-a-token-with-a-pop-up-window) mithilfe von `acquireTokenPopup`
* [Umleitung](#acquire-a-token-with-a-redirect) mithilfe von `acquireTokenRedirect`

## <a name="choose-between-a-pop-up-or-redirect-experience"></a>Auswählen zwischen einem Popupfenster oder einer Umleitung

Die Entscheidung für ein Popupfenster oder eine Umleitung hängt von Ihrem Anwendungsfluss ab:

* Wenn Sie nicht möchten, dass Benutzer während der Authentifizierung Ihre Hauptanwendungsseite verlassen, wird die Popupmethode empfohlen. Da die Authentifizierungsumleitung in einem Popupfenster erfolgt, wird der Status der Hauptanwendung beibehalten.

* Wenn für Benutzer Browsereinschränkungen oder Richtlinien gelten, bei denen Popupfenster deaktiviert sind, können Sie die Umleitungsmethode verwenden. Wenn Sie Internet Explorer als Browser nutzen, verwenden Sie die Umleitungsmethode, weil [in Internet Explorer bekannte Probleme mit Popupfenstern bestehen](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-browser/docs/internet-explorer.md#popups).

Sie können beim Erstellen der Zugriffstokenanforderung die API-Bereiche festlegen, die das Zugriffstoken einschließen soll. Beachten Sie, dass möglicherweise nicht alle angeforderten Bereiche im Zugriffstoken gewährt werden. Das ist von der Zustimmung des Benutzers abhängig.

## <a name="acquire-a-token-with-a-pop-up-window"></a>Abrufen eines Tokens mit einem Popupfenster

# <a name="javascript-msaljs-v2"></a>[JavaScript (MSAL.js v2)](#tab/javascript2)

Im folgenden Code wird das zuvor beschriebene Muster mit den Methoden für ein Popupelement kombiniert:

```javascript
// MSAL.js v2 exposes several account APIs, logic to determine which account to use is the responsibility of the developer
const account = publicClientApplication.getAllAccounts()[0];

const accessTokenRequest = {
    scopes: ["user.read"],
    account: account
}

publicClientApplication.acquireTokenSilent(accessTokenRequest).then(function(accessTokenResponse) {
    // Acquire token silent success
    let accessToken = accessTokenResponse.accessToken;
    // Call your API with token
    callApi(accessToken);
}).catch(function (error) {
    //Acquire token silent failure, and send an interactive request
    if (error instanceof InteractionRequiredAuthError) {
        publicClientApplication.acquireTokenPopup(accessTokenRequest).then(function(accessTokenResponse) {
            // Acquire token interactive success
            let accessToken = accessTokenResponse.accessToken;
            // Call your API with token
            callApi(accessToken);
        }).catch(function(error) {
            // Acquire token interactive failure
            console.log(error);
        });
    }
    console.log(error);
});
```

# <a name="javascript-msaljs-v1"></a>[JavaScript (MSAL.js v1)](#tab/javascript1)

Im folgenden Code wird das zuvor beschriebene Muster mit den Methoden für ein Popupelement kombiniert:

```javascript
const accessTokenRequest = {
    scopes: ["user.read"]
}

userAgentApplication.acquireTokenSilent(accessTokenRequest).then(function(accessTokenResponse) {
    // Acquire token silent success
    // Call API with token
    let accessToken = accessTokenResponse.accessToken;
}).catch(function (error) {
    //Acquire token silent failure, and send an interactive request
    if (error.errorMessage.indexOf("interaction_required") !== -1) {
        userAgentApplication.acquireTokenPopup(accessTokenRequest).then(function(accessTokenResponse) {
            // Acquire token interactive success
        }).catch(function(error) {
            // Acquire token interactive failure
            console.log(error);
        });
    }
    console.log(error);
});
```

# <a name="angular-msaljs-v2"></a>[Angular (MSAL.js v2)](#tab/angular2)

Der MSAL Angular-Wrapper nutzt den HTTP-Interceptor, der Zugriffstoken automatisch im Hintergrund abruft und an die HTTP-Anforderungen für APIs anfügt.

Sie können die Bereiche für APIs in der Konfigurationsoption `protectedResourceMap` angeben. `MsalInterceptor` fordert diese Bereiche beim automatischen Abrufen von Token an.

```javascript
// In app.module.ts
import { PublicClientApplication, InteractionType } from '@azure/msal-browser';
import { MsalInterceptor, MsalModule } from '@azure/msal-angular';

@NgModule({
    declarations: [
        // ...
    ],
    imports: [
        // ...
        MsalModule.forRoot( new PublicClientApplication({
        auth: {
            clientId: 'Enter_the_Application_Id_Here',
        },
        cache: {
            cacheLocation: 'localStorage',
            storeAuthStateInCookie: isIE,
        }
        }), {
            interactionType: InteractionType.Popup,
            authRequest: {
                scopes: ['user.read']
            }
        }, {
            interactionType: InteractionType.Popup,
            protectedResourceMap: new Map([ 
                ['https://graph.microsoft.com/v1.0/me', ['user.read']]
            ])
        })
    ],
    providers: [
        {
            provide: HTTP_INTERCEPTORS,
            useClass: MsalInterceptor,
            multi: true
        }
    ],
    bootstrap: [AppComponent]
})
export class AppModule { }
```

Für den erfolgreichen bzw. fehlerhaften automatischen Tokenabruf stellt MSAL Angular Ereignisse bereit, die Sie abonnieren können. Sie müssen auch daran denken, das Abonnement zu kündigen.

```javascript
import { MsalBroadcastService } from '@azure/msal-angular';
import { EventMessage, EventType } from '@azure/msal-browser';

// In app.component.ts
export class AppComponent implements OnInit {
    private readonly _destroying$ = new Subject<void>();

    constructor(private broadcastService: MsalBroadcastService) { }

    ngOnInit() {
        this.broadcastService.msalSubject$
        .pipe(
            filter((msg: EventMessage) => msg.eventType === EventType.ACQUIRE_TOKEN_SUCCESS),
            takeUntil(this._destroying$)
        )
        .subscribe((result: EventMessage) => {
            // Do something with event payload here
        });
    }

    ngOnDestroy(): void {
        this._destroying$.next(undefined);
        this._destroying$.complete();
    }
}
```

Alternativ können Sie Token auch explizit mithilfe der in der MSAL.js-Kernbibliothek beschriebenen „acquire-token“-Methoden abrufen.

# <a name="angular-msaljs-v1"></a>[Angular (MSAL.js v1)](#tab/angular1)
Der MSAL Angular-Wrapper nutzt den HTTP-Interceptor, der Zugriffstoken automatisch im Hintergrund abruft und an die HTTP-Anforderungen für APIs anfügt.
Sie können die Bereiche für APIs in der Konfigurationsoption `protectedResourceMap` angeben. `MsalInterceptor` fordert diese Bereiche beim automatischen Abrufen von Token an.

```javascript
// app.module.ts
@NgModule({
  declarations: [
    // ...
  ],
  imports: [
    // ...
    MsalModule.forRoot({
      auth: {
        clientId: 'Enter_the_Application_Id_Here',
      }
    },
    {
      popUp: !isIE,
      consentScopes: [
        'user.read',
        'openid',
        'profile',
      ],
      protectedResourceMap: [
        ['https://graph.microsoft.com/v1.0/me', ['user.read']]
      ]
    })
  ],
  providers: [
    {
      provide: HTTP_INTERCEPTORS,
      useClass: MsalInterceptor,
      multi: true
    }
  ],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

Für den erfolgreichen und den fehlerhaften automatischen Tokenabruf stellt MSAL Angular Rückrufe bereit, die Sie abonnieren können. Sie müssen auch daran denken, das Abonnement zu kündigen.

```javascript
// In app.component.ts
 ngOnInit() {
    this.subscription=  this.broadcastService.subscribe("msal:acquireTokenFailure", (payload) => {
    });
}
ngOnDestroy() {
   this.broadcastService.getMSALSubject().next(1);
   if (this.subscription) {
     this.subscription.unsubscribe();
   }
 }
```

Alternativ können Sie Token auch explizit mithilfe der in der MSAL.js-Kernbibliothek beschriebenen „acquire-token“-Methoden abrufen.

# <a name="react"></a>[React](#tab/react)

Im folgenden Code wird das zuvor beschriebene Muster mit den Methoden für ein Popupelement kombiniert:

```javascript
import { InteractionRequiredAuthError, InteractionStatus } from "@azure/msal-browser";
import { AuthenticatedTemplate, useMsal } from "@azure/msal-react";

function ProtectedComponent() {
    const { instance, inProgress, accounts } = useMsal();
    const [apiData, setApiData] = useState(null);


    useEffect(() => {
        if (!apiData && inProgress === InteractionStatus.None) {
            const accessTokenRequest = {
                scopes: ["user.read"],
                account: accounts[0]
            }
            instance.acquireTokenSilent(accessTokenRequest).then((accessTokenResponse) => {
                // Acquire token silent success
                let accessToken = accessTokenResponse.accessToken;
                // Call your API with token
                callApi(accessToken).then((response) => { setApiData(response) });
            }).catch((error) => {
                if (error instanceof InteractionRequiredAuthError) {
                    instance.acquireTokenPopup(accessTokenRequest).then(function(accessTokenResponse) {
                        // Acquire token interactive success
                        let accessToken = accessTokenResponse.accessToken;
                        // Call your API with token
                        callApi(accessToken).then((response) => { setApiData(response) });
                    }).catch(function(error) {
                        // Acquire token interactive failure
                        console.log(error);
                    });
                }
                console.log(error);
            })
        }
    }, [instance, accounts, inProgress, apiData]);

    return <p>Return your protected content here: {apiData}</p>
}

function App() {
    return (
        <AuthenticatedTemplate>
            <ProtectedComponent />
        </ AuthenticatedTemplate>
    )
}
```

Wenn Sie ein Token außerhalb einer React-Komponente abrufen müssen, können Sie alternativ `acquireTokenSilent` aufrufen, sollten aber bei einem Fehler kein Fallback auf die Interaktion verwenden. Alle Interaktionen sollten unterhalb der Komponente `MsalProvider` in der Komponentenstruktur stattfinden.

```javascript
// MSAL.js v2 exposes several account APIs, logic to determine which account to use is the responsibility of the developer
const account = publicClientApplication.getAllAccounts()[0];

const accessTokenRequest = {
    scopes: ["user.read"],
    account: account
}

// Use the same publicClientApplication instance provided to MsalProvider
publicClientApplication.acquireTokenSilent(accessTokenRequest).then(function(accessTokenResponse) {
    // Acquire token silent success
    let accessToken = accessTokenResponse.accessToken;
    // Call your API with token
    callApi(accessToken);
}).catch(function (error) {
    //Acquire token silent failure
    console.log(error);
});
```

---

## <a name="acquire-a-token-with-a-redirect"></a>Abrufen eines Tokens mit einer Umleitung

# <a name="javascript-msaljs-v2"></a>[JavaScript (MSAL.js v2)](#tab/javascript2)

Das folgende Muster entspricht dem zuvor beschriebenen Muster, wird jedoch mit einer Umleitungsmethode zum interaktiven Abrufen von Token dargestellt. Sie müssen aufrufen und beim Laden der Seite auf `handleRedirectPromise` warten.

```javascript
const redirectResponse = await publicClientApplication.handleRedirectPromise();
if (redirectResponse !== null) {
    // Acquire token silent success
    let accessToken = redirectResponse.accessToken;
    // Call your API with token
    callApi(accessToken);
} else {
    // MSAL.js v2 exposes several account APIs, logic to determine which account to use is the responsibility of the developer
    const account = publicClientApplication.getAllAccounts()[0];
    
    const accessTokenRequest = {
        scopes: ["user.read"],
        account: account
    }
    
    publicClientApplication.acquireTokenSilent(accessTokenRequest).then(function(accessTokenResponse) {
        // Acquire token silent success
        // Call API with token
        let accessToken = accessTokenResponse.accessToken;
        // Call your API with token
        callApi(accessToken);
    }).catch(function (error) {
        //Acquire token silent failure, and send an interactive request
        console.log(error);
        if (error instanceof InteractionRequiredAuthError) {
            publicClientApplication.acquireTokenRedirect(accessTokenRequest);
        }
    });
}
```

# <a name="javascript-msaljs-v1"></a>[JavaScript (MSAL.js v1)](#tab/javascript1)

Das folgende Muster entspricht dem zuvor beschriebenen Muster, wird jedoch mit einer Umleitungsmethode zum interaktiven Abrufen von Token dargestellt. Wie bereits erwähnt, müssen Sie den Umleitungsrückruf registrieren.

```javascript
function authCallback(error, response) {
    // Handle redirect response
}

userAgentApplication.handleRedirectCallback(authCallback);

const accessTokenRequest: AuthenticationParameters = {
    scopes: ["user.read"]
}

userAgentApplication.acquireTokenSilent(accessTokenRequest).then(function(accessTokenResponse) {
    // Acquire token silent success
    // Call API with token
    let accessToken = accessTokenResponse.accessToken;
}).catch(function (error) {
    //Acquire token silent failure, and send an interactive request
    console.log(error);
    if (error.errorMessage.indexOf("interaction_required") !== -1) {
        userAgentApplication.acquireTokenRedirect(accessTokenRequest);
    }
});
```

## <a name="request-optional-claims"></a>Anfordern optionaler Ansprüche

Sie können optionale Ansprüche zu folgenden Zwecken verwenden:

- Einbinden zusätzlicher Ansprüche in Token für Ihre Anwendung
- Ändern des Verhaltens bestimmter Ansprüche, die von Azure AD in Token zurückgegeben werden
- Hinzufügen und Zugreifen auf benutzerdefinierte Ansprüche für Ihre Anwendung

Zum Anfordern optionaler Ansprüche in `IdToken` können Sie ein als Zeichenfolge dargestelltes Anspruchsobjekt an das Feld `claimsRequest` der `AuthenticationParameters.ts`-Klasse senden.

```javascript
"optionalClaims":
   {
      "idToken": [
            {
                  "name": "auth_time",
                  "essential": true
             }
      ],

var request = {
    scopes: ["user.read"],
    claimsRequest: JSON.stringify(claims)
};

myMSALObj.acquireTokenPopup(request);
```

Weitere Informationen finden Sie unter [Optionale Ansprüche](active-directory-optional-claims.md).

# <a name="angular-msaljs-v2"></a>[Angular (MSAL.js v2)](#tab/angular2)

Dieser Code ist mit dem zuvor beschriebenen identisch, mit der Ausnahme, dass Bootstrapping für `MsalRedirectComponent` Umleitungen empfohlen wird. `MsalInterceptor` Konfigurationen können auch geändert werden, um Umleitungen zu verwenden.

```javascript
// In app.module.ts
import { PublicClientApplication, InteractionType } from '@azure/msal-browser';
import { MsalInterceptor, MsalModule, MsalRedirectComponent } from '@azure/msal-angular';

@NgModule({
    declarations: [
      // ...
    ],
    imports: [
        // ...
        MsalModule.forRoot( new PublicClientApplication({
            auth: {
                clientId: 'Enter_the_Application_Id_Here',
            },
            cache: {
                cacheLocation: 'localStorage',
                storeAuthStateInCookie: isIE,
            }
        }), {
            interactionType: InteractionType.Redirect,
            authRequest: {
                scopes: ['user.read']
            }
        }, {
            interactionType: InteractionType.Redirect,
            protectedResourceMap: new Map([ 
                ['https://graph.microsoft.com/v1.0/me', ['user.read']]
            ])
        })
    ],
    providers: [
        {
            provide: HTTP_INTERCEPTORS,
            useClass: MsalInterceptor,
            multi: true
        }
    ],
    bootstrap: [AppComponent, MsalRedirectComponent]
})
export class AppModule { }
```

# <a name="angular-msaljs-v1"></a>[Angular (MSAL.js v1)](#tab/angular1)
Dieser Code entspricht dem zuvor beschriebenen Code.

# <a name="react"></a>[React](#tab/react)

Wenn bei `acquireTokenSilent` ein Fehler auftritt, Fallback auf `acquireTokenRedirect`. Diese Methode initiiert eine Fullframe-Umleitung und die Antwort wird bei der Rückgabe an die Anwendung verarbeitet. Wenn diese Komponente nach der Rückgabe von der Umleitung gerendert wird, sollte `acquireTokenSilent` erfolgreich sein, da die Token aus dem Cache gezogen werden.

```javascript
import { InteractionRequiredAuthError, InteractionStatus } from "@azure/msal-browser";
import { AuthenticatedTemplate, useMsal } from "@azure/msal-react";

function ProtectedComponent() {
    const { instance, inProgress, accounts } = useMsal();
    const [apiData, setApiData] = useState(null);


    useEffect(() => {
        const accessTokenRequest = {
            scopes: ["user.read"],
            account: accounts[0]
        }
        if (!apiData && inProgress === InteractionStatus.None) {
            instance.acquireTokenSilent(accessTokenRequest).then((accessTokenResponse) => {
                // Acquire token silent success
                let accessToken = accessTokenResponse.accessToken;
                // Call your API with token
                callApi(accessToken).then((response) => { setApiData(response) });
            }).catch((error) => {
                if (error instanceof InteractionRequiredAuthError) {
                    instance.acquireTokenRedirect(accessTokenRequest);
                }
                console.log(error);
            })
        }
    }, [instance, accounts, inProgress, apiData]);

    return <p>Return your protected content here: {apiData}</p>
}

function App() {
    return (
        <AuthenticatedTemplate>
            <ProtectedComponent />
        </ AuthenticatedTemplate>
    )
}
```

Wenn Sie ein Token außerhalb einer React-Komponente abrufen müssen, können Sie alternativ `acquireTokenSilent` aufrufen, sollten aber bei einem Fehler kein Fallback auf die Interaktion verwenden. Alle Interaktionen sollten unterhalb der Komponente `MsalProvider` in der Komponentenstruktur stattfinden.

```javascript
// MSAL.js v2 exposes several account APIs, logic to determine which account to use is the responsibility of the developer
const account = publicClientApplication.getAllAccounts()[0];

const accessTokenRequest = {
    scopes: ["user.read"],
    account: account
}

// Use the same publicClientApplication instance provided to MsalProvider
publicClientApplication.acquireTokenSilent(accessTokenRequest).then(function(accessTokenResponse) {
    // Acquire token silent success
    let accessToken = accessTokenResponse.accessToken;
    // Call your API with token
    callApi(accessToken);
}).catch(function (error) {
    //Acquire token silent failure
    console.log(error);
});
```

---

## <a name="next-steps"></a>Nächste Schritte

Fahren Sie mit dem nächsten Artikel in diesem Szenario fort: [Aufrufen einer Web-API](scenario-spa-call-api.md).