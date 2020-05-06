---
title: Abrufen eines Tokens in einer Web-App, die Web-APIs aufruft – Microsoft Identity Platform | Azure
description: Erfahren Sie, wie Sie in einer Web-App, die Web-APIs aufruft, ein Token abrufen
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 10/30/2019
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 40e788099a159e1f60c0af02deccd7e3bef82744
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2020
ms.locfileid: "82181731"
---
# <a name="a-web-app-that-calls-web-apis-acquire-a-token-for-the-app"></a>Web-App, die Web-APIs aufruft: Abrufen eines Tokens für die App

Sie haben das Clientanwendungsobjekt erstellt. Jetzt rufen Sie damit ein Token ab, um eine Web-API aufzurufen. In ASP.NET oder ASP.NET Core erfolgt das Aufrufen einer Web-API im Controller:

- Mit dem Tokencache wird ein Token für die Web-API abgerufen. Zum Abrufen dieses Token rufen Sie die MSAL-Methode `AcquireTokenSilent` auf (oder eine entsprechende Methode des Pakets Microsoft.Identity.Web).
- Rufen Sie die geschützte API auf, indem Sie das Zugriffstoken als Parameter übergeben.

# <a name="aspnet-core"></a>[ASP.NET Core](#tab/aspnetcore)

Die Controllermethoden sind durch ein `[Authorize]`-Attribut geschützt, mit dem erzwungen wird, dass Benutzer für die Verwendung der Web-App authentifiziert werden müssen. Mit dem folgenden Code wird Microsoft Graph aufgerufen:

```csharp
[Authorize]
public class HomeController : Controller
{
 readonly ITokenAcquisition tokenAcquisition;

 public HomeController(ITokenAcquisition tokenAcquisition)
 {
  this.tokenAcquisition = tokenAcquisition;
 }

 // Code for the controller actions (see code below)

}
```

Der `ITokenAcquisition`-Dienst wird von ASP.NET mithilfe der Abhängigkeitsinjektion eingefügt.

Im Folgenden finden Sie den vereinfachten Code für die Aktion des `HomeController`, mit der ein Token zum Aufrufen von Microsoft Graph abgerufen wird:

```csharp
public async Task<IActionResult> Profile()
{
 // Acquire the access token.
 string[] scopes = new string[]{"user.read"};
 string accessToken = await tokenAcquisition.GetAccessTokenForUserAsync(scopes);

 // Use the access token to call a protected web API.
 HttpClient client = new HttpClient();
 client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", accessToken);
 string json = await client.GetStringAsync(url);
}
```

Um den für dieses Szenario erforderlichen Code besser zu verstehen, sollten Sie sich den Schritt der 2. Phase ([2.1 Web-App ruft Microsoft Graph auf](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/2-WebApp-graph-user/2-1-Call-MSGraph)) im Tutorial [ms-identity-aspnetcore-webapp-tutorial](https://github.com/Azure-Samples/ms-identity-aspnetcore-webapp-tutorial) ansehen.

Es gibt andere komplexe Varianten, wie beispielsweise:

- Aufrufen mehrerer APIs
- Verarbeiten von inkrementeller Zustimmung und bedingtem Zugriff

Diese erweiterten Schritte werden in Kapitel 3 des Tutorials [3-WebApp-multi-APIs](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/3-WebApp-multi-APIs) erörtert.

# <a name="aspnet"></a>[ASP.NET](#tab/aspnet)

Der Code für ASP.NET ähnelt dem für ASP.NET Core dargestellten Code:

- Eine durch das Attribut „[Authorize]“ geschützte Controlleraktion extrahiert die Mandanten- und Benutzer-ID des `ClaimsPrincipal`-Members aus dem Controller. (ASP.NET verwendet `HttpContext.User`.)
- Anschließend wird ein MSAL.NET-`IConfidentialClientApplication`-Objekt erstellt.
- Zuletzt wird die `AcquireTokenSilent`-Methode der vertraulichen Clientanwendung aufgerufen.

# <a name="java"></a>[Java](#tab/java)

Im Java-Beispiel befindet sich der Code, der eine API aufruft, in der „getUsersFromGraph“-Methode in [AuthPageController.java#L62](https://github.com/Azure-Samples/ms-identity-java-webapp/blob/d55ee4ac0ce2c43378f2c99fd6e6856d41bdf144/src/main/java/com/microsoft/azure/msalwebsample/AuthPageController.java#L62).

Die Methode versucht, `getAuthResultBySilentFlow` aufzurufen. Wenn der Benutzer mehr Bereichen zustimmen muss, verarbeitet der Code das `MsalInteractionRequiredException`-Objekt, um den Benutzer dazu aufzufordern.

```java
@RequestMapping("/msal4jsample/graph/me")
public ModelAndView getUserFromGraph(HttpServletRequest httpRequest, HttpServletResponse response)
        throws Throwable {

    IAuthenticationResult result;
    ModelAndView mav;
    try {
        result = authHelper.getAuthResultBySilentFlow(httpRequest, response);
    } catch (ExecutionException e) {
        if (e.getCause() instanceof MsalInteractionRequiredException) {

            // If the silent call returns MsalInteractionRequired, redirect to authorization endpoint
            // so user can consent to new scopes.
            String state = UUID.randomUUID().toString();
            String nonce = UUID.randomUUID().toString();

            SessionManagementHelper.storeStateAndNonceInSession(httpRequest.getSession(), state, nonce);

            String authorizationCodeUrl = authHelper.getAuthorizationCodeUrl(
                    httpRequest.getParameter("claims"),
                    "User.Read",
                    authHelper.getRedirectUriGraph(),
                    state,
                    nonce);

            return new ModelAndView("redirect:" + authorizationCodeUrl);
        } else {

            mav = new ModelAndView("error");
            mav.addObject("error", e);
            return mav;
        }
    }

    if (result == null) {
        mav = new ModelAndView("error");
        mav.addObject("error", new Exception("AuthenticationResult not found in session."));
    } else {
        mav = new ModelAndView("auth_page");
        setAccountInfo(mav, httpRequest);

        try {
            mav.addObject("userInfo", getUserInfoFromGraph(result.accessToken()));

            return mav;
        } catch (Exception e) {
            mav = new ModelAndView("error");
            mav.addObject("error", e);
        }
    }
    return mav;
}
// Code omitted here
```

# <a name="python"></a>[Python](#tab/python)

Im Python-Beispiel befindet sich der Code, der Microsoft Graph aufruft, in [app.py#L53-L62](https://github.com/Azure-Samples/ms-identity-python-webapp/blob/48637475ed7d7733795ebeac55c5d58663714c60/app.py#L53-L62).

Der Code versucht, ein Token aus dem Tokencache abzurufen. Nach dem Festlegen des Autorisierungsheaders wird dann die Web-API aufgerufen. Wenn kein Token abgerufen werden kann, wird der Benutzer erneut angemeldet.

```python
@app.route("/graphcall")
def graphcall():
    token = _get_token_from_cache(app_config.SCOPE)
    if not token:
        return redirect(url_for("login"))
    graph_data = requests.get(  # Use token to call downstream service.
        app_config.ENDPOINT,
        headers={'Authorization': 'Bearer ' + token['access_token']},
        ).json()
    return render_template('display.html', result=graph_data)
```

---

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Aufrufen einer Web-API](scenario-web-app-call-api-call-api.md)
