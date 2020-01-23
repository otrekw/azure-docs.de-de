---
title: Konfigurieren einer Web-API, die Web-APIs aufruft | Azure
titleSuffix: Microsoft identity platform
description: Erfahren Sie, wie Sie eine Web-API erstellen, die Web-APIs aufruft (Codekonfiguration der App)
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/16/2019
ms.author: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1e54e26bba1618a3b5835480ed1b1fe698bc8db4
ms.sourcegitcommit: 05cdbb71b621c4dcc2ae2d92ca8c20f216ec9bc4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/16/2020
ms.locfileid: "76043418"
---
# <a name="a-web-api-that-calls-web-apis-code-configuration"></a>Web-API, die Web-APIs aufruft: Codekonfiguration

Nachdem Sie Ihre Web-API registriert haben, können Sie den Code für die Anwendung konfigurieren.

Der Code, den Sie zum Konfigurieren der Web-API verwenden, sodass diese Downstream-Web-APIs aufruft, baut auf dem zum Schutz einer Web-API verwendeten Code auf. Weitere Informationen finden Sie unter [Geschützte Web-API: App-Konfiguration](scenario-protected-web-api-app-configuration.md).

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

Ein Beispiel für die Implementierung des OBO-Flusses finden Sie auch unter [Node.js und Azure Functions](https://github.com/Azure-Samples/ms-identity-nodejs-webapi-onbehalfof-azurefunctions/blob/master/MiddleTierAPI/MyHttpTrigger/index.js#L61).

## <a name="protocol"></a>Protocol

Weitere Informationen zum OBO-Protokoll finden Sie unter [Microsoft Identity Platform und der On-Behalf-Of-Fluss von OAuth 2.0](https://docs.microsoft.com/azure/active-directory/develop/v2-oauth2-on-behalf-of-flow).

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Web-API, die Web-APIs aufruft: Abrufen eines Tokens für die App](scenario-web-api-call-api-acquire-token.md)
