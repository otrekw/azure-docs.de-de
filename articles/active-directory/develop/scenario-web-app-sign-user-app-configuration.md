---
title: 'Konfigurieren einer Web-App, die Benutzer anmeldet: Microsoft Identity Platform | Azure'
description: Erfahren Sie, wie Sie eine Web-App erstellen, die Benutzer anmeldet (Codekonfiguration)
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
ms.date: 10/30/2019
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: d54103cad8a3550bdc300cba2308397dd1ce3d6c
ms.sourcegitcommit: b8f2fee3b93436c44f021dff7abe28921da72a6d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/18/2020
ms.locfileid: "77425577"
---
# <a name="web-app-that-signs-in-users-code-configuration"></a>Web-App für Benutzeranmeldungen: Codekonfiguration

Erfahren Sie, wie Sie den Code für Ihre Web-App zur Benutzeranmeldung konfigurieren.

## <a name="libraries-for-protecting-web-apps"></a>Bibliotheken zum Schützen von Web-Apps

<!-- This section can be in an include for Web App and Web APIs -->
Die folgenden Bibliotheken werden zum Schützen einer Web-App (und einer Web-API) verwendet:

| Plattform | Bibliothek | BESCHREIBUNG |
|----------|---------|-------------|
| ![.NET](media/sample-v2-code/logo_net.png) | [Identitätsmodellerweiterungen für .NET](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet/wiki) | Die Microsoft Identity-Modellerweiterung für .NET schlägt bei der direkten Verwendung durch ASP.NET und ASP.NET Core eine Reihe von DLLs vor, die unter .NET Framework und .NET Core ausgeführt werden. Über eine ASP.NET-/ASP.NET Core-Web-App können Sie mithilfe der **TokenValidationParameters**-Klasse die Tokenvalidierung steuern (insbesondere in einigen Partnerszenarien). |
| ![Java](media/sample-v2-code/small_logo_java.png) | [MSAL Java](https://github.com/AzureAD/microsoft-authentication-library-for-java/wiki) | Unterstützung für Java-Webanwendungen |
| ![Python](media/sample-v2-code/small_logo_python.png) | [MSAL Python](https://github.com/AzureAD/microsoft-authentication-library-for-python/wiki) | Unterstützung für Python-Webanwendungen |

Wählen Sie die Registerkarte aus, die der gewünschten Plattform entspricht:

# <a name="aspnet-core"></a>[ASP.NET Core](#tab/aspnetcore)

Die Codeausschnitte in diesem und den folgenden Artikeln stammen aus [Kapitel 1 des inkrementellen Tutorials zu ASP.NET Core-Web-Apps](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/1-WebApp-OIDC/1-1-MyOrg).

In diesem Tutorial finden Sie auch ausführliche Informationen zur Implementierung.

# <a name="aspnet"></a>[ASP.NET](#tab/aspnet)

Die Codeausschnitte in diesem und den folgenden Artikeln stammen aus dem [ASP.NET-Web-App-Beispiel](https://github.com/Azure-Samples/ms-identity-aspnet-webapp-openidconnect).

Dieses Beispiel enthält auch ausführliche Informationen zur Implementierung.

# <a name="java"></a>[Java](#tab/java)

Die Codeausschnitte in diesem und den folgenden Artikeln stammen aus dem MSAL-Java-Beispiel [Java-Webanwendung für den Aufruf von Microsoft Graph](https://github.com/Azure-Samples/ms-identity-java-webapp).

Dieses Beispiel enthält auch ausführliche Informationen zur Implementierung.

# <a name="python"></a>[Python](#tab/python)

Die Codeausschnitte in diesem und den folgenden Artikeln stammen aus dem MSAL-Python-Beispiel [Python-Webanwendung für den Aufruf von Microsoft Graph](https://github.com/Azure-Samples/ms-identity-python-webapp).

Dieses Beispiel enthält auch ausführliche Informationen zur Implementierung.

---

## <a name="configuration-files"></a>Konfigurationsdateien

Webanwendungen, die Benutzer mithilfe der Microsoft Identity Platform anmelden, werden in der Regel mithilfe von Konfigurationsdateien konfiguriert. Folgende Einstellungen sind einzugeben:

- Die Cloudinstanz (`Instance`), wenn Ihre App beispielsweise in nationalen Clouds ausgeführt werden soll
- Die Zielgruppe in der Mandanten-ID (`TenantId`)
- Die Client-ID (`ClientId`) für Ihre Anwendung, die aus dem Azure-Portal kopiert wird

Mitunter können Anwendungen durch `Authority`, eine Verkettung von `Instance` und `TenantId`, parametrisiert werden.

# <a name="aspnet-core"></a>[ASP.NET Core](#tab/aspnetcore)

In ASP.NET Core befinden sich diese Einstellungen in der Datei [appsettings.json](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/blob/bc564d68179c36546770bf4d6264ce72009bc65a/1-WebApp-OIDC/1-1-MyOrg/appsettings.json#L2-L8) im Abschnitt „AzureAd“.

```Json
{
  "AzureAd": {
    // Azure cloud instance among:
    // - "https://login.microsoftonline.com/" for Azure public cloud
    // - "https://login.microsoftonline.us/" for Azure US government
    // - "https://login.microsoftonline.de/" for Azure AD Germany
    // - "https://login.chinacloudapi.cn/" for Azure AD China operated by 21Vianet
    "Instance": "https://login.microsoftonline.com/",

    // Azure AD audience among:
    // - "TenantId" as a GUID obtained from the Azure portal to sign in users in your organization
    // - "organizations" to sign in users in any work or school account
    // - "common" to sign in users with any work or school account or Microsoft personal account
    // - "consumers" to sign in users with a Microsoft personal account only
    "TenantId": "[Enter the tenantId here]",

    // Client ID (application ID) obtained from the Azure portal
    "ClientId": "[Enter the Client Id]",
    "CallbackPath": "/signin-oidc",
    "SignedOutCallbackPath ": "/signout-callback-oidc"
  }
}
```

In ASP.NET Core gibt es eine weitere Datei ([properties\launchSettings.json](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/blob/bc564d68179c36546770bf4d6264ce72009bc65a/1-WebApp-OIDC/1-1-MyOrg/Properties/launchSettings.json#L6-L7)), die die URL (`applicationUrl`) und den SSL-Port (`sslPort`) für Ihre Anwendung und verschiedene Profile enthält.

```Json
{
  "iisSettings": {
    "windowsAuthentication": false,
    "anonymousAuthentication": true,
    "iisExpress": {
      "applicationUrl": "http://localhost:3110/",
      "sslPort": 44321
    }
  },
  "profiles": {
    "IIS Express": {
      "commandName": "IISExpress",
      "launchBrowser": true,
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development"
      }
    },
    "webApp": {
      "commandName": "Project",
      "launchBrowser": true,
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development"
      },
      "applicationUrl": "http://localhost:3110/"
    }
  }
}
```

Im Azure-Portal müssen die Antwort-URIs, die Sie auf der Seite **Authentifizierung** für Ihre Anwendung registrieren müssen, mit diesen URLs übereinstimmen. Für die beiden obigen Konfigurationsdateien wäre dies `https://localhost:44321/signin-oidc`. Der Grund hierfür ist, dass `applicationUrl` zwar `http://localhost:3110` ist, aber `sslPort` angegeben wird (44321). `CallbackPath` ist `/signin-oidc`, wie in `appsettings.json`definiert.

Auf die gleiche Weise wird der Abmelde-URI auf `https://localhost:44321/signout-callback-oidc` festgelegt.

# <a name="aspnet"></a>[ASP.NET](#tab/aspnet)

In ASP.NET wird die Anwendung über die Zeilen 12 bis 15 der Datei [Web.config](https://github.com/Azure-Samples/ms-identity-aspnet-webapp-openidconnect/blob/a2da310539aa613b77da1f9e1c17585311ab22b7/WebApp/Web.config#L12-L15) konfiguriert.

```XML
<?xml version="1.0" encoding="utf-8"?>
<!--
  For more information on how to configure your ASP.NET application, visit
  https://go.microsoft.com/fwlink/?LinkId=301880
  -->
<configuration>
  <appSettings>
    <add key="webpages:Version" value="3.0.0.0" />
    <add key="webpages:Enabled" value="false" />
    <add key="ClientValidationEnabled" value="true" />
    <add key="UnobtrusiveJavaScriptEnabled" value="true" />
    <add key="ida:ClientId" value="[Enter your client ID, as obtained from the app registration portal]" />
    <add key="ida:ClientSecret" value="[Enter your client secret, as obtained from the app registration portal]" />
    <add key="ida:AADInstance" value="https://login.microsoftonline.com/{0}{1}" />
    <add key="ida:RedirectUri" value="https://localhost:44326/" />
    <add key="vs:EnableBrowserLink" value="false" />
  </appSettings>
```

Im Azure-Portal müssen die Antwort-URIs, die Sie auf der Seite **Authentifizierung** für Ihre Anwendung registrieren müssen, mit diesen URLs übereinstimmen. Das heißt, sie sollten `https://localhost:44326/` sein.

# <a name="java"></a>[Java](#tab/java)

In Java befindet sich die Konfiguration in der Datei [application.properties](https://github.com/Azure-Samples/ms-identity-java-webapp/blob/d55ee4ac0ce2c43378f2c99fd6e6856d41bdf144/src/main/resources/application.properties) unter `src/main/resources`.

```Java
aad.clientId=Enter_the_Application_Id_here
aad.authority=https://login.microsoftonline.com/Enter_the_Tenant_Info_Here/
aad.secretKey=Enter_the_Client_Secret_Here
aad.redirectUriSignin=http://localhost:8080/msal4jsample/secure/aad
aad.redirectUriGraph=http://localhost:8080/msal4jsample/graph/me
```

Im Azure-Portal müssen die Antwort-URIs, die Sie auf der Seite **Authentifizierung** für Ihre Anwendung registrieren müssen, mit den `redirectUri`-Instanzen übereinstimmen, die von der Anwendung definiert werden. Das heißt, sie sollten `http://localhost:8080/msal4jsample/secure/aad` und `http://localhost:8080/msal4jsample/graph/me` sein.

# <a name="python"></a>[Python](#tab/python)

Hier finden Sie die Python-Konfigurationsdatei in [app_config.py](https://github.com/Azure-Samples/ms-identity-python-webapp/blob/0.1.0/app_config.py):

```Python
CLIENT_SECRET = "Enter_the_Client_Secret_Here"
AUTHORITY = "https://login.microsoftonline.com/common""
CLIENT_ID = "Enter_the_Application_Id_here"
ENDPOINT = 'https://graph.microsoft.com/v1.0/users'
SCOPE = ["User.ReadBasic.All"]
SESSION_TYPE = "filesystem"  # So the token cache will be stored in a server-side session
```

> [!NOTE]
> In dieser Schnellstartanleitung wird vorgeschlagen, den geheimen Clientschlüssel der Einfachheit halber in der Konfigurationsdatei zu speichern. In Ihrer Produktions-App sollten Sie andere Möglichkeiten zum Speichern Ihres Geheimnisses, z. B. einen Schlüsseltresor oder eine Umgebungsvariable, verwenden, wie in der [Flask-Dokumentation](https://flask.palletsprojects.com/en/1.1.x/config/#configuring-from-environment-variables) beschrieben.
>
> ```python
> CLIENT_SECRET = os.getenv("CLIENT_SECRET")
> if not CLIENT_SECRET:
>     raise ValueError("Need to define CLIENT_SECRET environment variable")
> ```

---

## <a name="initialization-code"></a>Initialisierungscode

Der Initialisierungscode ist je nach Plattform unterschiedlich. Für ASP.NET Core und ASP.NET wird die Anmeldung von Benutzern an die OpenID Connect-Middleware delegiert. Die ASP.NET-/ASP.NET Core-Vorlage generiert Webanwendungen für den Azure AD v1.0-Endpunkt (Azure Active Directory). Es müssen Konfigurationsänderungen vorgenommen werden, um diese an den Microsoft Identity Platform-Endpunkt (v2.0) anzupassen. Bei Java erfolgt dies von Spring in Zusammenarbeit mit der Anwendung.

# <a name="aspnet-core"></a>[ASP.NET Core](#tab/aspnetcore)

In ASP.NET Core-Web-Apps (und Web-APIs) ist die Anwendung geschützt, da es das Attribut `[Authorize]` für die Controller bzw. Controlleraktionen gibt. Mit diesem Attribut wird geprüft, ob der Benutzer authentifiziert ist. Der Code für die Anwendungsinitialisierung befindet sich in der Datei „Startup.cs“.

Zum Hinzufügen der Authentifizierung mit der Microsoft Identity Platform (ehemals Azure AD v2.0) müssen Sie den folgenden Code hinzufügen. Die Kommentare im Code sollten selbsterklärend sein.

> [!NOTE]
> Falls Sie Ihr Projekt mit dem ASP.NET Core-Standardwebprojekt in Visual Studio oder mit `dotnet new mvc` starten, ist die `AddAzureAD`-Methode standardmäßig verfügbar. Der Grund ist, dass die zugehörigen Pakete automatisch geladen werden.
>
> Wenn Sie ein Projekt von Grund auf neu erstellen und den unten angegebenen Code verwenden, empfiehlt es sich, Ihrem Projekt das NuGet-Paket **Microsoft.AspNetCore.Authentication.AzureAD.UI** hinzuzufügen, um die `AddAzureAD`-Methode verfügbar zu machen.

Der folgende Code ist in [Startup.cs#L33-L34](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/blob/faa94fd49c2da46b22d6694c4f5c5895795af26d/1-WebApp-OIDC/1-1-MyOrg/Startup.cs#L33-L34) verfügbar.

```csharp
public class Startup
{
 ...

  // This method is called by the runtime. Use this method to add services to the container.
  public void ConfigureServices(IServiceCollection services)
  {
    ...
      // Sign in users with the Microsoft identity platform
      services.AddMicrosoftIdentityPlatformAuthentication(Configuration);

      services.AddMvc(options =>
      {
          var policy = new AuthorizationPolicyBuilder()
              .RequireAuthenticatedUser()
              .Build();
            options.Filters.Add(new AuthorizeFilter(policy));
            })
        .SetCompatibilityVersion(CompatibilityVersion.Version_2_2);
    }
```

Die Erweiterungsmethode `AddMicrosoftIdentityPlatformAuthentication` ist in [Microsoft.Identity.Web/WebAppServiceCollectionExtensions.cs#L23](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/blob/faa94fd49c2da46b22d6694c4f5c5895795af26d/Microsoft.Identity.Web/WebAppServiceCollectionExtensions.cs#L23) definiert. Sie hat folgende Aufgaben:

- Hinzufügen des Authentifizierungsdiensts
- Konfigurieren von Optionen zum Lesen der Konfigurationsdatei
- Konfigurieren der OpenID Connect-Optionen, damit die verwendete Autorität der Microsoft Identity Platform-Endpunkt (ehemals Azure AD v2.0) ist
- Validieren des Ausstellers des Tokens
- Sicherstellen der Zuordnung der dem Namen entsprechenden Ansprüche aus dem Anspruch `preferred_username` im ID-Token

Zusätzlich zur Konfiguration können Sie den Namen des Konfigurationsabschnitts angeben, indem Sie `AddMicrosoftIdentityPlatformAuthentication` aufrufen. Dieser lautet standardmäßig `AzureAd`.

Das Verfolgen von Ereignissen der OpenID Connect-Middleware kann beim Troubleshooting Ihrer Webanwendung helfen, wenn die Authentifizierung nicht funktioniert. Wenn Sie `subscribeToOpenIdConnectMiddlewareDiagnosticsEvents` auf `true` festlegen, werden Informationen durch die ASP.NET Core-Middleware ergänzt, während sie den Flow von der HTTP-Antwort zur Identität des Benutzers in `HttpContext.User` durchlaufen.

```csharp
/// <summary>
/// Add authentication with the Microsoft identity platform.
/// This method expects the configuration file to have a section named "AzureAd" with the necessary settings to initialize authentication options.
/// </summary>
/// <param name="services">Service collection to which to add this authentication scheme</param>
/// <param name="configuration">The Configuration object</param>
/// <param name="subscribeToOpenIdConnectMiddlewareDiagnosticsEvents">
/// Set to true if you want to debug, or just understand the OpenID Connect events.
/// </param>
/// <returns></returns>
public static IServiceCollection AddMicrosoftIdentityPlatformAuthentication(
  this IServiceCollection services,
  IConfiguration configuration,
  string configSectionName = "AzureAd",
  bool subscribeToOpenIdConnectMiddlewareDiagnosticsEvents = false)
{
  services.AddAuthentication(AzureADDefaults.AuthenticationScheme)
      .AddAzureAD(options => configuration.Bind(configSectionName, options));
  services.Configure<AzureADOptions>(options => configuration.Bind(configSectionName, options));

  services.Configure<OpenIdConnectOptions>(AzureADDefaults.OpenIdScheme, options =>
  {
      // Per the following code, this application signs in users in any work or school
      // accounts and any Microsoft personal accounts.
      // If you want to direct Azure AD to restrict the users who can sign in, change
      // the tenant value of the appsettings.json file in the following way:
      // - Only work or school accounts => 'organizations'
      // - Only Microsoft personal accounts => 'consumers'
      // - Work or school and personal accounts => 'common'
      // If you want to restrict the users who can sign in to only one tenant,
      // set the tenant value in the appsettings.json file to the tenant ID
      // or domain of this organization.
      options.Authority = options.Authority + "/v2.0/";

      // If you want to restrict the users who can sign in to several organizations,
      // set the tenant value in the appsettings.json file to 'organizations', and add the
      // issuers you want to accept to the options.TokenValidationParameters.ValidIssuers collection.
      options.TokenValidationParameters.IssuerValidator = AadIssuerValidator.GetIssuerValidator(options.Authority).Validate;

      // Set nameClaimType to be preferred_username.
      // This change is needed because certain token claims from the Azure AD v1 endpoint
      // (on which the original .NET Core template is based) are different from the Microsoft identity platform endpoint.
      // For more details, see [ID tokens](https://docs.microsoft.com/azure/active-directory/develop/id-tokens)
      // and [Access tokens](https://docs.microsoft.com/azure/active-directory/develop/access-tokens).
      options.TokenValidationParameters.NameClaimType = "preferred_username";

      // ...

      if (subscribeToOpenIdConnectMiddlewareDiagnosticsEvents)
      {
          OpenIdConnectMiddlewareDiagnostics.Subscribe(options.Events);
      }
  });
  return services;
}
  ...
```

Mithilfe der `AadIssuerValidator`-Klasse kann der Aussteller des Tokens in vielen Fällen überprüft werden. Diese Klasse funktioniert mit v1.0- oder v2.0-Token, Anwendungen mit einem oder mehreren Mandanten oder Anwendungen, die Benutzer mit ihren persönlichen Microsoft-Konten bei der öffentlichen Azure Cloud oder nationalen Clouds anmelden. Sie ist unter [Microsoft.Identity.Web/Resource/AadIssuerValidator.cs](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/blob/master/Microsoft.Identity.Web/Resource/AadIssuerValidator.cs) verfügbar.

# <a name="aspnet"></a>[ASP.NET](#tab/aspnet)

Der Code für die Authentifizierung in ASP.NET-Web-Apps und -Web-APIs befindet sich in der Datei [App_Start/Startup.Auth.cs](https://github.com/Azure-Samples/ms-identity-aspnet-webapp-openidconnect/blob/a2da310539aa613b77da1f9e1c17585311ab22b7/WebApp/App_Start/Startup.Auth.cs#L17-L61).

```csharp
 public void ConfigureAuth(IAppBuilder app)
 {
  app.SetDefaultSignInAsAuthenticationType(CookieAuthenticationDefaults.AuthenticationType);

  app.UseCookieAuthentication(new CookieAuthenticationOptions());

  app.UseOpenIdConnectAuthentication(
    new OpenIdConnectAuthenticationOptions
    {
     // `Authority` represents the identity platform endpoint - https://login.microsoftonline.com/common/v2.0.
     // `Scope` describes the initial permissions that your app will need.
     //  See https://azure.microsoft.com/documentation/articles/active-directory-v2-scopes/.
     ClientId = clientId,
     Authority = String.Format(CultureInfo.InvariantCulture, aadInstance, "common", "/v2.0"),
     RedirectUri = redirectUri,
     Scope = "openid profile",
     PostLogoutRedirectUri = redirectUri,
    });
 }
```

# <a name="java"></a>[Java](#tab/java)

Das Java-Beispiel verwendet das Spring-Framework. Die Anwendung ist geschützt, da Sie einen Filter implementieren, der jede HTTP-Antwort abfängt. Im Schnellstart für Java-Web-Apps handelt es sich dabei um den Filter `AuthFilter` in `src/main/java/com/microsoft/azure/msalwebsample/AuthFilter.java`.

Der Filter verarbeitet den OAuth 2.0-Autorisierungscodeflow und überprüft, ob der Benutzer authentifiziert ist (`isAuthenticated()`-Methode). Wenn der Benutzer nicht authentifiziert ist, wird die URL der Azure AD-Autorisierungsendpunkte berechnet und der Browser zu diesem URI umgeleitet.

Wenn die Antwort mit dem Autorisierungscode eingeht, wird mit MSAL für Java das Token abgerufen. Wenn das Token schließlich vom Tokenendpunkt (im Umleitungs-URI) empfangen wird, erfolgt die Benutzeranmeldung.

Weitere Informationen finden Sie unter der `doFilter()`-Methode in [AuthFilter.java](https://github.com/Azure-Samples/ms-identity-java-webapp/blob/master/src/main/java/com/microsoft/azure/msalwebsample/AuthFilter.java).

> [!NOTE]
> Der Code von `doFilter()` wird in einer etwas anderen Reihenfolge geschrieben, aber der Flow entspricht dem beschriebenen.

Unter [Microsoft Identity Platform und der OAuth 2.0-Autorisierungscodeflow](v2-oauth2-auth-code-flow.md) finden Sie Einzelheiten zu dem durch diese Methode ausgelösten Autorisierungscodeflow.

# <a name="python"></a>[Python](#tab/python)

Das Python-Beispiel verwendet Flask. Die Initialisierung von Flask und MSAL für Python erfolgt in [app.py#L1-L28](https://github.com/Azure-Samples/ms-identity-python-webapp/blob/e03be352914bfbd58be0d4170eba1fb7a4951d84/app.py#L1-L28).

```Python
import uuid
import requests
from flask import Flask, render_template, session, request, redirect, url_for
from flask_session import Session  # https://pythonhosted.org/Flask-Session
import msal
import app_config


app = Flask(__name__)
app.config.from_object(app_config)
Session(app)
```

---

## <a name="next-steps"></a>Nächste Schritte

Im nächsten Artikel erfahren Sie, wie An- und Abmeldung ausgelöst werden.

# <a name="aspnet-core"></a>[ASP.NET Core](#tab/aspnetcore)

> [!div class="nextstepaction"]
> [An- und Abmelden](https://docs.microsoft.com/azure/active-directory/develop/scenario-web-app-sign-user-sign-in?tabs=aspnetcore)

# <a name="aspnet"></a>[ASP.NET](#tab/aspnet)

> [!div class="nextstepaction"]
> [An- und Abmelden](https://docs.microsoft.com/azure/active-directory/develop/scenario-web-app-sign-user-sign-in?tabs=aspnet)

# <a name="java"></a>[Java](#tab/java)

> [!div class="nextstepaction"]
> [An- und Abmelden](https://docs.microsoft.com/azure/active-directory/develop/scenario-web-app-sign-user-sign-in?tabs=java)

# <a name="python"></a>[Python](#tab/python)

> [!div class="nextstepaction"]
> [An- und Abmelden](https://docs.microsoft.com/azure/active-directory/develop/scenario-web-app-sign-user-sign-in?tabs=python)

---
