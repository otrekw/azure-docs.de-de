---
title: Konfigurieren von geschützten Web-API-Apps | Azure
titleSuffix: Microsoft identity platform
description: Erfahren Sie, wie Sie eine geschützte Web-API erstellen und den Code Ihrer Anwendung konfigurieren.
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: b05eefb2a0e516772390f898c22e723b08973338
ms.sourcegitcommit: 98a5a6765da081e7f294d3cb19c1357d10ca333f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/20/2020
ms.locfileid: "77484450"
---
# <a name="protected-web-api-code-configuration"></a>Geschützte Web-API: Codekonfiguration

Um den Code für Ihre geschützte Web-API konfigurieren zu können, müssen Sie mit Folgendem vertraut sein:

- Wodurch werden APIs als geschützt definiert?
- Wie wird ein Bearertoken konfiguriert?
- Wie wird das Token überprüft?

## <a name="what-defines-aspnet-and-aspnet-core-apis-as-protected"></a>Wodurch werden ASP.NET- und ASP.NET Core-APIs als geschützt definiert?

Genau wie Webanwendungen sind auch die ASP.NET- und die ASP.NET Core-Web-APIs geschützt, da ihren Controlleraktionen das Attribut **[Authorize]** vorangestellt ist. Die Controlleraktionen können nur aufgerufen werden, wenn die API mit einer autorisierten Identität aufgerufen wird.

Stellen Sie sich die folgenden Fragen:

- Eine Web-API kann nur von einer App aufgerufen werden. Woher kennt die API die Identität der aufrufenden App?
- Falls die App die API im Namen eines Benutzers aufruft: Welche Identität hat der Benutzer?

## <a name="bearer-token"></a>Bearertoken

Das Bearertoken, das beim Aufrufen der App im Header festgelegt wird, enthält Informationen zur App-Identität. Außerdem enthält es Informationen zum Benutzer. (Es sei denn, die Web-App akzeptiert Dienst-zu-Dienst-Aufrufe von einer Daemon-App).

Hier ist ein C#-Codebeispiel, das zeigt, wie ein Client die API aufruft, nachdem er ein Token mit der Microsoft-Authentifizierungsbibliothek für .NET (MSAL.NET) erworben hat:

```csharp
var scopes = new[] {$"api://.../access_as_user"};
var result = await app.AcquireToken(scopes)
                      .ExecuteAsync();

httpClient = new HttpClient();
httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);

// Call the web API.
HttpResponseMessage response = await _httpClient.GetAsync(apiUri);
```

> [!IMPORTANT]
> Eine Clientanwendung fordert das Bearertoken für den Microsoft Identity Platform-Endpunkt *für die Web-API* an. Die Web-API ist die einzige Anwendung, die das Token verifizieren und die darin enthaltenen Ansprüche anzeigen sollte. Client-Apps dürfen niemals versuchen, die Ansprüche in Token zu überprüfen.
>
> Die Web-API kann später ggf. die Verschlüsselung des Tokens erforderlich machen. Diese Anforderung würde den Zugriff durch Client-Apps verhindern, die Zugriffstoken anzeigen können.

## <a name="jwtbearer-configuration"></a>JwtBearer-Konfiguration

In diesem Abschnitt wird beschrieben, wie Sie ein Bearertoken konfigurieren.

### <a name="config-file"></a>Konfigurationsdatei

```Json
{
  "AzureAd": {
    "Instance": "https://login.microsoftonline.com/",
    "ClientId": "[Client_id-of-web-api-eg-2ec40e65-ba09-4853-bcde-bcb60029e596]",
    /*
      You need specify the TenantId only if you want to accept access tokens from a single tenant
     (line-of-business app).
      Otherwise, you can leave them set to common.
      This can be:
      - A GUID (Tenant ID = Directory ID)
      - 'common' (any organization and personal accounts)
      - 'organizations' (any organization)
      - 'consumers' (Microsoft personal accounts)
    */
    "TenantId": "common"
  },
  "Logging": {
    "LogLevel": {
      "Default": "Warning"
    }
  },
  "AllowedHosts": "*"
}
```

### <a name="code-initialization"></a>Codeinitialisierung

Wenn eine App in einer Controlleraktion aufgerufen wird, die ein Attribut vom Typ **[Authorize]** enthält, extrahieren ASP.NET und ASP.NET Core das Zugriffstoken aus dem Bearertoken des Autorisierungsheaders. Das Zugriffstoken wird dann an die JwtBearer-Middleware weitergeleitet, die die Microsoft-Identitätsmodellerweiterungen für .NET aufruft.

In ASP.NET Core wird diese Middleware in der Datei „Startup.cs“ initialisiert.

```csharp
using Microsoft.AspNetCore.Authentication.JwtBearer;
```

Die Middleware wird der Web-API durch die folgende Anweisung hinzugefügt:

```csharp
 services.AddAuthentication(AzureADDefaults.JwtBearerAuthenticationScheme)
         .AddAzureAdBearer(options => Configuration.Bind("AzureAd", options));
```

 Aktuell erstellen die ASP.NET Core-Vorlagen Azure Active Directory-Web-APIs, die Benutzer innerhalb Ihrer Organisation oder einer anderen Organisation anmelden. Benutzer werden nicht mit persönlichen Konten angemeldet. Sie können die Vorlagen jedoch so ändern, dass sie den Microsoft Identity Platform-Endpunkt verwenden, indem Sie „Startup.cs“ den folgenden Code hinzufügen:

```csharp
services.Configure<JwtBearerOptions>(AzureADDefaults.JwtBearerAuthenticationScheme, options =>
{
    // This is a Microsoft identity platform web API.
    options.Authority += "/v2.0";

    // The web API accepts as audiences both the Client ID (options.Audience) and api://{ClientID}.
    options.TokenValidationParameters.ValidAudiences = new []
    {
     options.Audience,
     $"api://{options.Audience}"
    };

    // Instead of using the default validation (validating against a single tenant,
    // as we do in line-of-business apps),
    // we inject our own multitenant validation logic (which even accepts both v1 and v2 tokens).
    options.TokenValidationParameters.IssuerValidator = AadIssuerValidator.GetIssuerValidator(options.Authority).Validate;;
});
```

Der obige Codeausschnitt stammt aus dem inkrementellen ASP.NET Core-Web-API-Tutorial in [Microsoft.Identity.Web/WebApiServiceCollectionExtensions.cs#L50-L63](https://github.com/Azure-Samples/active-directory-dotnet-native-aspnetcore-v2/blob/154282843da2fc2958fad151e2a11e521e358d42/Microsoft.Identity.Web/WebApiServiceCollectionExtensions.cs#L50-L63). Die Methode **AddProtectedWebApi**, die noch mehr bewirkt als in dem Ausschnitt zu sehen, wird von „Startup.cs“ aus aufgerufen.

## <a name="token-validation"></a>Tokenüberprüfung

Im obigen Codeausschnitt überprüft die JwtBearer-Middleware das Token auf der Grundlage des Werts von `TokenValidationParameters` (genau wie die OpenID Connect-Middleware in Web-Apps). Der Token wird bei Bedarf entschlüsselt, die Ansprüche werden extrahiert, und die Signatur wird überprüft. Die Middleware validiert dann das Token, indem sie auf diese Daten überprüft:

- Audience: Das Ziel des Tokens ist die Web-API.
- Antragsteller: Das Token wurde für eine App ausgestellt, die zum Aufrufen der Web-API berechtigt ist.
- Aussteller: Das Token wurde von einem vertrauenswürdigen Sicherheitstokendienst (Security Token Service, STS) ausgestellt.
- Ablauf: Die Tokenlebensdauer liegt innerhalb des zulässigen Bereichs.
- Signatur: Das Token wurde nicht manipuliert.

Es können auch spezielle Validierungen vorliegen. So kann beispielsweise sichergestellt werden, dass die Signaturschlüssel (bei Einbettung in ein Token) vertrauenswürdig sind und das Token nicht wiedergegeben wird. Für einige Protokolle sind bestimmte Validierungen erforderlich.

### <a name="validators"></a>Validierungssteuerelemente

Die Schritte zur Überprüfung werden in Validierungssteuerelementen erfasst, die durch die Open-Source-Bibliothek [Microsoft-Identitätsmodellerweiterungen für .NET](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet) bereitgestellt werden. Die Validierungssteuerelemente sind in der Quelldatei [Microsoft.IdentityModel.Tokens/Validators.cs](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet/blob/master/src/Microsoft.IdentityModel.Tokens/Validators.cs) der Bibliothek definiert.

Die folgende Tabelle enthält eine Beschreibung der Validierungssteuerelemente:

| Validierungssteuerelement | Beschreibung |
|---------|---------|
| **ValidateAudience** | Stellt sicher, dass das Token für die Anwendung bestimmt ist, die das Token für Sie überprüft. |
| **ValidateIssuer** | Stellt sicher, dass das Token von einem vertrauenswürdigen Sicherheitstokendienst ausgestellt wurde und somit von jemandem stammt, dem Sie vertrauen. |
| **ValidateIssuerSigningKey** | Stellt sicher, dass die Anwendung, die das Token validiert, dem Schlüssel vertraut, mit dem das Token signiert wurde. Es gibt einen Sonderfall, bei dem der Schlüssel in das Token eingebettet ist. Dies ist jedoch in der Regel nicht der Fall. |
| **ValidateLifetime** | Stellt sicher, dass das Token noch oder bereits gültig ist. Das Validierungssteuerelement überprüft, ob die Lebensdauer des Tokens innerhalb des durch die Ansprüche **notbefore** und **expires** angegebenen Bereichs liegt. |
| **ValidateSignature** | Stellt sicher, dass das Token nicht manipuliert wurde. |
| **ValidateTokenReplay** | Stellt sicher, dass das Token nicht wiedergegeben wird. Es gibt einen Sonderfall für einige Protokolle mit einmaliger Verwendung. |

Die Validierungssteuerelemente werden Eigenschaften der Klasse **TokenValidationParameters** zugeordnet. Die Eigenschaften werden von der ASP.NET- und ASP.NET Core-Konfiguration initialisiert.

In den meisten Fällen müssen die Parameter nicht geändert werden. Ausnahmen sind Apps, bei denen es sich nicht um Einzelmandanten handelt. Diese Web-Apps akzeptieren Benutzer aus einer beliebigen Organisation oder von persönlichen Microsoft-Konten. In diesem Fall ist eine Überprüfung der Aussteller erforderlich.

## <a name="token-validation-in-azure-functions"></a>Tokenüberprüfung in Azure Functions

Eingehende Zugriffstoken können auch in Azure Functions überprüft werden. Beispiele für eine solche Überprüfung finden Sie in [Microsoft .NET](https://github.com/Azure-Samples/ms-identity-dotnet-webapi-azurefunctions), [NodeJS](https://github.com/Azure-Samples/ms-identity-nodejs-webapi-azurefunctions) und [Python](https://github.com/Azure-Samples/ms-identity-python-webapi-azurefunctions).

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Verifizieren von Bereichen und App-Rollen in Ihrem Code](scenario-protected-web-api-verification-scope-app-roles.md)
