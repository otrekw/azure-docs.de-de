---
title: Konfigurieren einer Web-API, die Web-APIs aufruft | Azure
titleSuffix: Microsoft identity platform
description: Erfahren Sie, wie Sie eine Web-API erstellen, die Web-APIs aufruft (Codekonfiguration der App)
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 08/05/2020
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 95cb1181f841ce5f958b8a85697d7261f442b410
ms.sourcegitcommit: fbb66a827e67440b9d05049decfb434257e56d2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/05/2020
ms.locfileid: "87799598"
---
# <a name="a-web-api-that-calls-web-apis-code-configuration"></a>Web-API, die Web-APIs aufruft: Codekonfiguration

Nachdem Sie Ihre Web-API registriert haben, können Sie den Code für die Anwendung konfigurieren.

Der Code, den Sie zum Konfigurieren der Web-API verwenden, sodass diese Downstream-Web-APIs aufruft, baut auf dem zum Schutz einer Web-API verwendeten Code auf. Weitere Informationen finden Sie unter [Geschützte Web-API: App-Konfiguration](scenario-protected-web-api-app-configuration.md).

# <a name="aspnet-core"></a>[ASP.NET Core](#tab/aspnetcore)

## <a name="client-secrets-or-client-certificates"></a>Geheime Clientschlüssel oder Clientzertifikate

Wenn Ihre Web-API jetzt eine Downstream-Web-API aufruft, müssen Sie in der Datei *appsettings.json* einen geheimen Clientschlüssel oder ein Clientzertifikat angeben.

```JSON
{
  "AzureAd": {
    "Instance": "https://login.microsoftonline.com/",
    "ClientId": "[Client_id-of-web-api-eg-2ec40e65-ba09-4853-bcde-bcb60029e596]",
    "TenantId": "common"
  
   // To call an API
   "ClientSecret": "[Copy the client secret added to the app from the Azure portal]",
   "ClientCertificates": [
  ]
 }
}
```

Statt eines geheimen Clientschlüssels können Sie ein Clientzertifikat angeben. Im folgenden Codeausschnitt ist die Verwendung eines in Azure Key Vault gespeicherten Zertifikats dargestellt.

```JSON
{
  "AzureAd": {
    "Instance": "https://login.microsoftonline.com/",
    "ClientId": "[Client_id-of-web-api-eg-2ec40e65-ba09-4853-bcde-bcb60029e596]",
    "TenantId": "common"
  
   // To call an API
   "ClientCertificates": [
      {
        "SourceType": "KeyVault",
        "KeyVaultUrl": "https://msidentitywebsamples.vault.azure.net",
        "KeyVaultCertificateName": "MicrosoftIdentitySamplesCert"
      }
  ]
 }
}
```

Microsoft.Identity.Web bietet verschiedene Möglichkeiten, Zertifikate zu beschreiben, sowohl durch Konfiguration als auch durch Code. Weitere Informationen finden Sie unter [Microsoft.Identity.Web-Wiki: Verwenden von Zertifikaten](https://github.com/AzureAD/microsoft-identity-web/wiki/Using-certificates) auf GitHub.

## <a name="startupcs"></a>Startup.cs

Wenn Sie Microsoft.Identity.Web verwenden und möchten, dass Ihre Web-API Downstream-Web-APIs aufruft, fügen Sie in der Datei *Startup.cs* nach `.AddMicrosoftWebApiAuthentication(Configuration)` die Zeile `.AddMicrosoftWebApiCallsWebApi()` hinzu, und wählen Sie dann eine Tokencacheimplementierung aus, z. B. `.AddInMemoryTokenCaches()`:

```csharp
using Microsoft.Identity.Web;

public class Startup
{
  ...
  public void ConfigureServices(IServiceCollection services)
  {
   // ...
   services.AddMicrosoftWebApiAuthentication(Configuration)
           .AddMicrosoftWebApiCallsWebApi(Configuration)
           .AddInMemoryTokenCaches();
  // ...
  }
  // ...
}
```

Wie bei Web-Apps können Sie verschiedene Tokencacheimplementierungen auswählen. Weitere Informationen finden Sie unter [Microsoft.Identity.Web-Wiki: Tokencacheserialisierung](https://aka.ms/ms-id-web/token-cache-serialization) auf GitHub.

Wenn Sie wissen, dass Ihre Web-API bestimmte Bereiche benötigt, können Sie diese optional als Argumente an `AddMicrosoftWebApiCallsWebApi` übergeben.

# <a name="java"></a>[Java](#tab/java)

Der On-Behalf-Of-Fluss (OBO) wird verwendet, um ein Token zum Aufrufen der Downstream-Web-API abzurufen. In diesem Flow empfängt Ihre Web-API ein Bearertoken mit den vom Benutzer delegierten Berechtigungen von der Clientanwendung und tauscht dieses Token dann mit einem anderen Zugriffstoken aus, um die Downstream-Web-API aufzurufen.

Der folgende Code verwendet den `SecurityContextHolder` des Spring Security-Frameworks in der Web-API, um das überprüfte Bearertoken abzurufen. Anschließend wird mithilfe der MSAL-Java-Bibliothek ein Token für die Downstream-API über einen `acquireToken`-Aufruf mit `OnBehalfOfParameters` abgerufen. MSAL speichert das Token zwischen, damit für nachfolgende Aufrufe der API `acquireTokenSilently` verwendet werden kann, um das zwischengespeicherte Token abzurufen.

```Java
@Component
class MsalAuthHelper {

    @Value("${security.oauth2.client.authority}")
    private String authority;

    @Value("${security.oauth2.client.client-id}")
    private String clientId;

    @Value("${security.oauth2.client.client-secret}")
    private String secret;

    @Autowired
    CacheManager cacheManager;

    private String getAuthToken(){
        String res = null;
        Authentication authentication = SecurityContextHolder.getContext().getAuthentication();

        if(authentication != null){
            res = ((OAuth2AuthenticationDetails) authentication.getDetails()).getTokenValue();
        }
        return res;
    }

    String getOboToken(String scope) throws MalformedURLException {
        String authToken = getAuthToken();

        ConfidentialClientApplication application =
                ConfidentialClientApplication.builder(clientId, ClientCredentialFactory.create(secret))
                        .authority(authority).build();

        String cacheKey = Hashing.sha256()
                .hashString(authToken, StandardCharsets.UTF_8).toString();

        String cachedTokens = cacheManager.getCache("tokens").get(cacheKey, String.class);
        if(cachedTokens != null){
            application.tokenCache().deserialize(cachedTokens);
        }

        IAuthenticationResult auth;
        SilentParameters silentParameters =
                SilentParameters.builder(Collections.singleton(scope))
                        .build();
        auth = application.acquireTokenSilently(silentParameters).join();

        if (auth == null){
            OnBehalfOfParameters parameters =
                    OnBehalfOfParameters.builder(Collections.singleton(scope),
                            new UserAssertion(authToken))
                            .build();

            auth = application.acquireToken(parameters).join();
        }

        cacheManager.getCache("tokens").put(cacheKey, application.tokenCache().serialize());

        return auth.accessToken();
    }
}
```

# <a name="python"></a>[Python](#tab/python)

Der On-Behalf-Of-Fluss (OBO) wird verwendet, um ein Token zum Aufrufen der Downstream-Web-API abzurufen. In diesem Flow empfängt Ihre Web-API ein Bearertoken mit den vom Benutzer delegierten Berechtigungen von der Clientanwendung und tauscht dieses Token dann mit einem anderen Zugriffstoken aus, um die Downstream-Web-API aufzurufen.

Eine Python-Web-API muss eine Middleware verwenden, um das vom Client empfangene Bearertoken zu überprüfen. Die Web-API kann dann das Zugriffstoken für die Downstream-API mithilfe der MSAL-Python-Bibliothek über einen Aufruf der [`acquire_token_on_behalf_of`](https://msal-python.readthedocs.io/en/latest/?badge=latest#msal.ConfidentialClientApplication.acquire_token_on_behalf_of)-Methode abrufen. Ein Beispiel zur Verwendung dieser API finden Sie im [Testcode für microsoft-authentication-library-for-python in GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-python/blob/1.2.0/tests/test_e2e.py#L429-L472). Siehe auch die Erörterung von [Issue 53](https://github.com/AzureAD/microsoft-authentication-library-for-python/issues/53) im selben Repository zum Ansatz, der die Notwendigkeit einer Anwendung auf mittlerer Ebene umgeht.

---

Ein Beispiel für die Implementierung des OBO-Flusses finden Sie auch unter [Node.js und Azure Functions](https://github.com/Azure-Samples/ms-identity-nodejs-webapi-onbehalfof-azurefunctions/blob/master/MiddleTierAPI/MyHttpTrigger/index.js#L61).

## <a name="protocol"></a>Protocol

Weitere Informationen zum OBO-Protokoll finden Sie unter [Microsoft Identity Platform und der On-Behalf-Of-Fluss von OAuth 2.0](https://docs.microsoft.com/azure/active-directory/develop/v2-oauth2-on-behalf-of-flow).

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Web-API, die Web-APIs aufruft: Abrufen eines Tokens für die App](scenario-web-api-call-api-acquire-token.md)
