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
ms.openlocfilehash: 7e0701cc5a9bb14800a48e2281dba1eb6ea0cf72
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2020
ms.locfileid: "87026457"
---
# <a name="a-web-api-that-calls-web-apis-acquire-a-token-for-the-app"></a>Eine Web-API, die Web-APIs aufruft: Abrufen eines Tokens für die App

Nach dem Erstellen eines Clientanwendungsobjekts rufen Sie damit ein Token ab. Dieses verwenden Sie anschließend, eine Web-API aufzurufen.

## <a name="code-in-the-controller"></a>Code im Controller

# <a name="aspnet-core"></a>[ASP.NET Core](#tab/aspnetcore)

Im Folgenden finden Sie ein Beispiel für Code mit Microsoft.Identity.Web, der in den Aktionen der API-Controller aufgerufen wird. Er ruft eine nachgelagerte API namens *todolist* auf. Um ein Token zum Aufrufen der nachgelagerten API (Downstream-API) abzurufen, fügen Sie den `ITokenAcquisition`-Dienst per Abhängigkeitsinjektion in den Konstruktor Ihres Controllers (oder bei Verwendung von Blazor in den Seitenkonstruktor) ein, und Sie verwenden ihn in den Controlleraktionen. Sie erhalten ein Token für den Benutzer (`GetAccessTokenForUserAsync`) oder im Falle eines Daemonszenarios für die Anwendung selbst (`GetAccessTokenForAppAsync`).

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

Eine Python-Web-API muss eine Middleware verwenden, um das vom Client empfangene Bearertoken zu überprüfen. Die Web-API kann dann das Zugriffstoken für die Downstream-API mithilfe der MSAL-Python-Bibliothek über einen Aufruf der [`acquire_token_on_behalf_of`](https://msal-python.readthedocs.io/en/latest/?badge=latest#msal.ConfidentialClientApplication.acquire_token_on_behalf_of)-Methode abrufen. Es steht leider noch kein Beispiel zur Veranschaulichung dieses Flows mit MSAL Python zur Verfügung.

---

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Eine Web-API, die Web-APIs aufruft: Aufrufen einer API](scenario-web-api-call-api-call-api.md)
