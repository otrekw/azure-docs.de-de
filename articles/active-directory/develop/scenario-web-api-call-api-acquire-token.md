---
title: Abrufen eines Tokens für eine Web-API, die Web-APIs aufruft | Azure
titleSuffix: Microsoft identity platform
description: Hier erfahren Sie, wie Sie eine Web-API erstellen, die Web-APIs aufruft, für die ein Token für die App abgerufen werden muss.
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 07/15/2020
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: ab0b74ffbcd8167613c6a8470e2f9102566edc60
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2020
ms.locfileid: "91257230"
---
# <a name="a-web-api-that-calls-web-apis-acquire-a-token-for-the-app"></a>Eine Web-API, die Web-APIs aufruft: Abrufen eines Tokens für die App

Nach dem Erstellen eines Clientanwendungsobjekts rufen Sie damit ein Token ab. Dieses verwenden Sie anschließend, eine Web-API aufzurufen.

## <a name="code-in-the-controller"></a>Code im Controller

# <a name="aspnet-core"></a>[ASP.NET Core](#tab/aspnetcore)

Von *Microsoft.Identity.Web* werden Erweiterungsmethoden hinzugefügt, die praktische Dienste zum Aufrufen von Microsoft Graph oder einer Downstream-Web-API bereitstellen. Eine ausführliche Beschreibung dieser Methoden finden Sie unter [Web-App, die Web-APIs aufruft: Aufrufen einer Web-API](scenario-web-api-call-api-call-api.md). Mit diesen Hilfsmethoden ist kein manueller Tokenabruf erforderlich.

Wenn Sie allerdings manuell ein Token abrufen möchten, zeigt der folgende Code anhand eines Beispiels, wie Sie dies mithilfe von *Microsoft.Identity.Web* in einem API-Controller tun können. Er ruft eine nachgelagerte API namens *todolist* auf.
Um ein Token zum Aufrufen der nachgelagerten API (Downstream-API) abzurufen, fügen Sie den `ITokenAcquisition`-Dienst per Abhängigkeitsinjektion in den Konstruktor Ihres Controllers (oder bei Verwendung von Blazor in den Seitenkonstruktor) ein, und Sie verwenden ihn in den Controlleraktionen. Sie erhalten ein Token für den Benutzer (`GetAccessTokenForUserAsync`) oder im Falle eines Daemonszenarios für die Anwendung selbst (`GetAccessTokenForAppAsync`).

```csharp
[Authorize]
public class MyApiController : Controller
{
    /// <summary>
    /// The web API will accept only tokens 1) for users, 2) that have the `access_as_user` scope for
    /// this API.
    /// </summary>
    static readonly string[] scopeRequiredByApi = new string[] { "access_as_user" };

     static readonly string[] scopesToAccessDownstreamApi = new string[] { "api://MyTodolistService/access_as_user" };

    private readonly ITokenAcquisition _tokenAcquisition;

    public MyApiController(ITokenAcquisition tokenAcquisition)
    {
        _tokenAcquisition = tokenAcquisition;
    }

    public IActionResult Index()
    {
        HttpContext.VerifyUserHasAnyAcceptedScope(scopeRequiredByApi);

        string accessToken = _tokenAcquisition.GetAccessTokenForUserAsync(scopesToAccessDownstreamApi);
        return await callTodoListService(accessToken);
    }
}
```

Ausführliche Informationen zur `callTodoListService`-Methode finden Sie unter [Web-API, die Web-APIs aufruft: Aufrufen einer Web-API](scenario-web-api-call-api-call-api.md).

# <a name="java"></a>[Java](#tab/java)
Im Folgenden sehen Sie ein Beispiel für den Code, der in den Aktionen der API-Controller aufgerufen wird. Er ruft die Downstream-API (Microsoft Graph) auf.

```java
@RestController
public class ApiController {

    @Autowired
    MsalAuthHelper msalAuthHelper;

    @RequestMapping("/graphMeApi")
    public String graphMeApi() throws MalformedURLException {

        String oboAccessToken = msalAuthHelper.getOboToken("https://graph.microsoft.com/.default");

        return callMicrosoftGraphMeEndpoint(oboAccessToken);
    }

}
```

# <a name="python"></a>[Python](#tab/python)

Bei einer Python-Web-API muss das vom Client empfangene Bearertoken mithilfe von Middleware überprüft werden. Die Web-API kann dann das Zugriffstoken für eine Downstream-API unter Verwendung der MSAL Python-Bibliothek durch Aufrufen der Methode [`acquire_token_on_behalf_of`](https://msal-python.readthedocs.io/en/latest/?badge=latest#msal.ConfidentialClientApplication.acquire_token_on_behalf_of) abrufen. Es ist noch kein Beispiel verfügbar, das diesen Flow mit MSAL Python veranschaulicht.

---

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Eine Web-API, die Web-APIs aufruft: Aufrufen einer API](scenario-web-api-call-api-call-api.md)
