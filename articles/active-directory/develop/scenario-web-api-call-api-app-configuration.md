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
ms.date: 07/16/2019
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 38e319efb100d326d55f6f821e7c903306a7c7d0
ms.sourcegitcommit: a53fe6e9e4a4c153e9ac1a93e9335f8cf762c604
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/09/2020
ms.locfileid: "80991006"
---
# <a name="a-web-api-that-calls-web-apis-code-configuration"></a>Web-API, die Web-APIs aufruft: Codekonfiguration

Nachdem Sie Ihre Web-API registriert haben, können Sie den Code für die Anwendung konfigurieren.

Der Code, den Sie zum Konfigurieren der Web-API verwenden, sodass diese Downstream-Web-APIs aufruft, baut auf dem zum Schutz einer Web-API verwendeten Code auf. Weitere Informationen finden Sie unter [Geschützte Web-API: App-Konfiguration](scenario-protected-web-api-app-configuration.md).

# <a name="aspnet-core"></a>[ASP.NET Core](#tab/aspnetcore)

## <a name="code-subscribed-to-ontokenvalidated"></a>Von OnTokenValidated abonnierter Code

Über die Codekonfiguration für alle geschützten Web-APIs hinaus müssen Sie die Validierung des Bearertokens abonnieren, das beim Aufruf Ihrer API empfangen wird:

```csharp
/// <summary>
/// Protects the web API with the Microsoft identity platform, or Azure Active Directory (Azure AD) developer platform
/// This supposes that the configuration files have a section named "AzureAD"
/// </summary>
/// <param name="services">The service collection to which to add authentication</param>
/// <param name="configuration">Configuration</param>
/// <returns></returns>
public static IServiceCollection AddProtectedApiCallsWebApis(this IServiceCollection services,
                                                             IConfiguration configuration,
                                                             IEnumerable<string> scopes)
{
    services.AddTokenAcquisition();
    services.Configure<JwtBearerOptions>(AzureADDefaults.JwtBearerAuthenticationScheme, options =>
    {
        // When an access token for our own web API is validated, we add it
        // to the MSAL.NET cache so that it can be used from the controllers.
        options.Events = new JwtBearerEvents();

        options.Events.OnTokenValidated = async context =>
        {
            context.Success();

            // Adds the token to the cache and handles the incremental consent
            // and claim challenges
            AddAccountToCacheFromJwt(context, scopes);
            await Task.FromResult(0);
        };
    });
    return services;
}
```

## <a name="on-behalf-of-flow"></a>On-Behalf-Of-Fluss

Für die AddAccountToCacheFromJwt()-Methode ist Folgendes erforderlich:

- Instanziieren Sie eine vertrauliche Clientanwendung der Microsoft Authentication Library (MSAL).
- Rufen Sie die `AcquireTokenOnBehalf` -Methode auf. Mit diesem Aufruf wird das Bearertoken, das vom Client für die Web-API abgerufen wurde, gegen ein Bearertoken für den gleichen Benutzer ausgetauscht, jedoch mit dem API-Aufruf einer Downstream-API.

### <a name="instantiate-a-confidential-client-application"></a>Instanziieren einer vertraulichen Clientanwendung

Dieser Fluss ist nur im vertraulichen Clientfluss verfügbar, sodass die geschützte Web-API für die [ConfidentialClientApplicationBuilder-Klasse](https://docs.microsoft.com/dotnet/api/microsoft.identity.client.confidentialclientapplicationbuilder) entweder über die `WithClientSecret`- oder `WithCertificate`-Methode Clientanmeldeinformationen (Clientgeheimnis oder Zertifikat) bereitstellt.

![Liste der IConfidentialClientApplication-Methoden](https://user-images.githubusercontent.com/13203188/55967244-3d8e1d00-5c7a-11e9-8285-a54b05597ec9.png)

```csharp
IConfidentialClientApplication app;

#if !VariationWithCertificateCredentials
app = ConfidentialClientApplicationBuilder.Create(config.ClientId)
           .WithClientSecret(config.ClientSecret)
           .Build();
#else
// Building the client credentials from a certificate
X509Certificate2 certificate = ReadCertificate(config.CertificateName);
app = ConfidentialClientApplicationBuilder.Create(config.ClientId)
    .WithCertificate(certificate)
    .Build();
#endif
```

Schließlich können vertrauliche Clientanwendungen, anstatt ihre Identität über ein Clientgeheimnis oder ein Zertifikat nachzuweisen, ihre Identität mithilfe von Clientassertionen nachweisen.
Weitere Informationen zu diesem erweiterten Szenario finden Sie unter [Assertionen für vertrauliche Clients](msal-net-client-assertions.md).

### <a name="how-to-call-on-behalf-of"></a>Aufrufen von „On-Behalf-Of“

Der On-Behalf-Of-Aufruf (OBO) erfolgt durch Aufrufen der [AcquireTokenOnBehalf-Methode](https://docs.microsoft.com/dotnet/api/microsoft.identity.client.acquiretokenonbehalfofparameterbuilder) in der `IConfidentialClientApplication`-Schnittstelle.

Die `UserAssertion`-Klasse wird basierend auf dem Bearertoken erstellt, das die Web-API von den eigenen Clients empfängt. Es gibt [zwei Konstruktoren](https://docs.microsoft.com/dotnet/api/microsoft.identity.client.clientcredential.-ctor?view=azure-dotnet):
* Ein Konstruktor verwendet ein JWT-Bearertoken (JSON Web Token).
* Ein Konstruktor verwendet eine Art von Benutzerassertion, eine andere Art von Sicherheitstoken, dessen Typ dann in dem zusätzlichen Parameter `assertionType` angegeben wird.

![UserAssertion-Eigenschaften und -Methoden](https://user-images.githubusercontent.com/13203188/37082180-afc4b708-21e3-11e8-8af8-a6dcbd2dfba8.png)

In der Praxis wird der OBO-Fluss häufig verwendet, um ein Token für eine Downstream-API abzurufen und im MSAL.NET-Benutzertokencache zu speichern. Dies erfolgt, damit andere Teile der Web-API später die [Überschreibungen](https://docs.microsoft.com/dotnet/api/microsoft.identity.client.clientapplicationbase.acquiretokensilent?view=azure-dotnet) von ``AcquireTokenOnSilent`` aufrufen können, um die Downstream-APIs aufzurufen. Dieser Aufruf hat zur Folge, dass die Token aktualisiert werden, wenn dies erforderlich ist.

```csharp
private void AddAccountToCacheFromJwt(IEnumerable<string> scopes, JwtSecurityToken jwtToken, ClaimsPrincipal principal, HttpContext httpContext)
{
    try
    {
        UserAssertion userAssertion;
        IEnumerable<string> requestedScopes;
        if (jwtToken != null)
        {
            userAssertion = new UserAssertion(jwtToken.RawData, "urn:ietf:params:oauth:grant-type:jwt-bearer");
            requestedScopes = scopes ?? jwtToken.Audiences.Select(a => $"{a}/.default");
        }
        else
        {
            throw new ArgumentOutOfRangeException("tokenValidationContext.SecurityToken should be a JWT Token");
        }

        // Create the application
        var application = BuildConfidentialClientApplication(httpContext, principal);

        // .Result to make sure that the cache is filled in before the controller tries to get access tokens
        var result = application.AcquireTokenOnBehalfOf(requestedScopes.Except(scopesRequestedByMsalNet),
                                                        userAssertion)
                                .ExecuteAsync()
                                .GetAwaiter().GetResult();
     }
     catch (MsalException ex)
     {
         Debug.WriteLine(ex.Message);
         throw;
     }
}
```
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
