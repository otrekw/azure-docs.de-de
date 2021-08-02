---
title: Das Aktivieren von Webanwendungsoptionen mit Azure Active Directory B2C
description: Aktivieren Sie die Verwendung von Webanwendungsoptionen auf verschiedene Weisen.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 05/25/2021
ms.author: mimart
ms.subservice: B2C
ms.custom: b2c-support
ms.openlocfilehash: 3335e035a2d36cc7830d8bc93db82a7d318d26b3
ms.sourcegitcommit: 80d311abffb2d9a457333bcca898dfae830ea1b4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/25/2021
ms.locfileid: "110482339"
---
# <a name="configure-authentication-in-a-sample-web-application-using-azure-active-directory-b2c-options"></a>Konfigurieren Sie die Authentifizierung in einer Beispielwebanwendung mit den Azure Active Directory B2C-Optionen

In diesem Artikel werden Möglichkeiten beschrieben, wie Sie die Azure Active Directory B2C-Authentifizierungsoberfläche (Azure AD B2C) für Ihre Webanwendung anpassen und verbessern können. Machen Sie sich zunächst mit den folgenden Artikeln vertraut: [Das Konfigurieren der Authentifizierung in einer Beispielwebanwendung](configure-authentication-sample-web-app.md) oder [Das Aktivieren der Authentifizierung in Ihrer eigenen Webanwendung](enable-authentication-web-application.md).

## <a name="use-a-custom-domain"></a>Verwenden einer benutzerdefinierten Domäne

Verwenden einer [benutzerdefinierten Domäne](custom-domain.md) in der Umleitungs-URL Ihrer Anwendung bietet eine nahtlosere Benutzererfahrung. Aus der Sicht des Benutzers verbleibt der Benutzer während des Anmeldevorgangs in Ihrer Domäne und wird nicht auf die Azure AD B2C-Standarddomäne „.b2clogin.com“ umgeleitet.

Um eine benutzerdefinierte Domäne zu verwenden, befolgen Sie die Anleitung unter [Das Aktivieren benutzerdefinierter Domänen](custom-domain.md). Öffnen Sie die `appsettings.json`-Datei im Projektstammordner. Diese Datei enthält Informationen zu Ihrem Azure AD B2C-Identitätsanbieter. Aktualisieren Sie den `Instance`-Eintrag mit Ihrer benutzerdefinierten Domäne.

Die folgende JSON zeigt die App-Einstellungen vor der Änderung: 

```JSon
"AzureAdB2C": {
  "Instance": "https://contoso.b2clogin.com",
  ...
}
```  

Die folgende JSON zeigt die App-Einstellungen nach der Änderung: 

```JSon
"AzureAdB2C": {
  "Instance": "https://login.contoso.com",
  ...
}
``` 

## <a name="use-your-tenant-id"></a>Verwenden Sie Ihre Mandanten-ID

Sie können den Namen des B2C-Mandanten in der URL durch Ihre Mandanten-ID-GUID der ersetzen, um alle Verweise auf „b2c“ in der URL zu entfernen.  Sie können z. B. `https://account.contosobank.co.uk/contosobank.onmicrosoft.com/` in `https://account.contosobank.co.uk/<tenant ID GUID>/` ändern

Um die Mandanten-ID zu verwenden, befolgen Sie die Anleitung [Das Aktivieren benutzerdefinierter Domänen](custom-domain.md#optional-use-tenant-id). Öffnen Sie die `appsettings.json`-Datei im Projektstammordner. Diese Datei enthält Informationen zu Ihrem Azure AD B2C-Identitätsanbieter. Aktualisieren Sie den `Domain`-Eintrag mit Ihrer benutzerdefinierten Domäne.

Die folgende JSON zeigt die App-Einstellungen vor der Änderung: 

```JSon
"AzureAdB2C": {
  "Domain": "tenant-name.onmicrosoft.com",
  ...
}
```  

Die folgende JSON zeigt die App-Einstellungen nach der Änderung:

```JSon
"AzureAdB2C": {
  "Domain": "00000000-0000-0000-0000-000000000000",
  ...
}
``` 

## <a name="support-advanced-scenarios"></a>Die Unterstützung für erweiterte Szenarien

Die `AddMicrosoftIdentityWebAppAuthentication`-Methode in der Microsoft Identity Platform-API ermöglicht es den Entwicklern, einen Code für erweiterte Authentifizierungsszenarien hinzuzufügen oder OpenIdConnect-Ereignisse zu abonnieren. Beispielsweise können Sie „OnRedirectToIdentityProvider“ abonnieren, mit dem Sie die Authentifizierungsanforderung anpassen können, die Ihre App an Azure AD B2C sendet.

Um erweiterte Szenarien zu unterstützen, öffnen Sie `Startup.cs` und ersetzen Sie in der `ConfigureServices`-Funktion `AddMicrosoftIdentityWebAppAuthentication` durch den folgenden Codeausschnitt: 

```csharp
// Configuration to sign-in users with Azure AD B2C

//services.AddMicrosoftIdentityWebAppAuthentication(Configuration, "AzureAdB2C");

services.AddAuthentication(OpenIdConnectDefaults.AuthenticationScheme)
        .AddMicrosoftIdentityWebApp(options =>
{
    Configuration.Bind("AzureAdB2C", options);
    options.Events ??= new OpenIdConnectEvents();
    options.Events.OnRedirectToIdentityProvider += OnRedirectToIdentityProviderFunc;
});
```

Der obige Code fügt das „OnRedirectToIdentityProvider“-Ereignis mit einem Verweis auf die *OnRedirectToIdentityProviderFunc*-Methode hinzu. Fügen Sie den folgenden Codeausschnitt zu der `Startup.cs`-Klasse hinzu.

```csharp
private async Task OnRedirectToIdentityProviderFunc(RedirectContext context)
{
    // Custom code here
    
    // Don't remove this line
    await Task.CompletedTask.ConfigureAwait(false);
}
```

Sie können die Parameter zwischen Ihrem Controller und der *OnRedirectToIdentityProvider*-Funktion mithilfe von Kontextparametern übergeben. 


## <a name="prepopulate-the-sign-in-name"></a>Auffüllen des Anmeldenamens

Während einer User Journey zur Anmeldung kann Ihre App auf einen bestimmten Benutzer ausgerichtet werden. Bei der Ausrichtung auf einen Benutzer kann eine Anwendung in der Autorisierungsanforderung den Abfrageparameter `login_hint` mit dem Anmeldenamen des Benutzers angeben. Azure AD B2C füllt den Anmeldenamen automatisch auf, während der Benutzer nur das Kennwort angeben muss. 

Führen Sie die folgenden Schritte aus, um den Anmeldenamen voraufzufüllen:

1. Schließen Sie das Verfahren [Unterstützen erweiterter Szenarien](#support-advanced-scenarios) ab.
1. Wenn Sie eine benutzerdefinierte Richtlinie verwenden, fügen Sie den erforderlichen Eingabeanspruch hinzu, wie unter dem Verfahren [Einrichten der direkten Anmeldung](direct-signin.md#prepopulate-the-sign-in-name) beschrieben. 
1. Fügen Sie die folgende Codezeile der *OnRedirectToIdentityProvider*-Funktion hinzu:
    
    ```csharp
    private async Task OnRedirectToIdentityProviderFunc(RedirectContext context)
    {
      context.ProtocolMessage.LoginHint = "emily@contoso.com";
      
      // More code
      await Task.CompletedTask.ConfigureAwait(false);
    }
    ```

## <a name="redirect-sign-in-to-an-external-identity-provider"></a>Das Umleiten der Anmeldung an einen externen Identitätsanbieter

Wenn Sie die User Journey für die Anmeldung bei Ihrer Anwendung so konfiguriert haben, dass Konten für soziale Netzwerke inbegriffen sind, wie z.B. Facebook, LinkedIn oder Google, können Sie den Parameter `domain_hint` angeben. Dieser Abfrageparameter enthält einen Hinweis für Azure AD B2C zu dem sozialen Netzwerk als Identitätsanbieter, das für die Anmeldung verwendet werden sollte. Wenn in der Anwendung beispielsweise `domain_hint=facebook.com` angegeben ist, erfolgt der Anmeldefluss direkt auf der Anmeldeseite von Facebook. 

Führen Sie die folgenden Schritte aus, um die Anmeldung an einen externen Identitätsanbieter umzuleiten:

1. Schließen Sie das Verfahren [Unterstützen erweiterter Szenarien](#support-advanced-scenarios) ab.
1. Überprüfen Sie den Domänennamen Ihres externen Identitätsanbieters. Weitere Informationen finden Sie unter [Umleiten einer Anmeldung zu einem Anbieter sozialer Netzwerke](direct-signin.md#redirect-sign-in-to-a-social-provider). 
1. Fügen Sie die folgende Codezeile zur *OnRedirectToIdentityProvider*-Funktion in der *OnRedirectToIdentityProviderFunc*-Funktion hinzu:
    
    ```csharp
    private async Task OnRedirectToIdentityProviderFunc(RedirectContext context)
    {
      context.ProtocolMessage.DomainHint = "facebook.com";
      
      // More code
      await Task.CompletedTask.ConfigureAwait(false);
    }
    ```

## <a name="specify-the-ui-language"></a>Ändern Sie die Sprache für die Benutzeroberfläche

Die Sprachanpassung in Azure AD B2C ermöglicht es Ihrem Benutzerfluss, verschiedene Sprachen zu berücksichtigen, um den Anforderungen Ihrer Kunden gerecht zu werden. Weitere Informationen hierzu finden Sie unter [Sprachanpassung](language-customization.md).

Folgen Sie diesen Schritten um die bevorzugte Sprache einzustellen:

1. Schließen Sie das Verfahren [Unterstützen erweiterter Szenarien](#support-advanced-scenarios) ab.
1. Fügen Sie die folgende Codezeile der *OnRedirectToIdentityProvider*-Funktion hinzu:

    ```csharp
    private async Task OnRedirectToIdentityProviderFunc(RedirectContext context)
    {
      context.ProtocolMessage.UiLocales = "es";

      // More code
      await Task.CompletedTask.ConfigureAwait(false);
    }
    ```

## <a name="pass-a-custom-query-string-parameter"></a>Das Übergeben eines benutzerdefinierten Abfragezeichenfolgenparameters

Mit den benutzerdefinierten Richtlinien können Sie einen benutzerdefinierten Abfragezeichenfolgenparameter übergeben, z. B. wenn Sie den [Seiteninhalt dynamisch ändern möchten](customize-ui-with-html.md?pivots=b2c-custom-policy#configure-dynamic-custom-page-content-uri).


Um einen benutzerdefinierten Abfragezeichenfolgenparameter zu übergeben, führen Sie die folgenden Schritte aus:

1. Schließen Sie das Verfahren [Unterstützen erweiterter Szenarien](#support-advanced-scenarios) ab.
1. Fügen Sie die folgende Codezeile der *OnRedirectToIdentityProvider*-Funktion hinzu:
    
    ```csharp
    private async Task OnRedirectToIdentityProviderFunc(RedirectContext context)
    {
      context.ProtocolMessage.Parameters.Add("campaignId", "123");

      // More code
      await Task.CompletedTask.ConfigureAwait(false);
    }
    ```

## <a name="pass-id-token-hint"></a>Übergeben Sie einen ID-Token-Hinweis

Azure AD B2C ermöglicht es Anwendungen der vertrauenden Seite, ein eingehendes JSON Web Token als Teil der OAuth2-Autorisierungsanforderung zu senden. Das JWT-Token kann von einer Anwendung der vertrauenden Seite oder einem Identitätsanbieter ausgegeben werden, und es kann einen Hinweis zum Benutzer oder zur Autorisierungsanforderung übergeben. Azure AD B2C überprüft die Signatur, den Ausstellernamen und die Tokenzielgruppe und extrahiert den Anspruch aus dem eingehenden Token.

Führen Sie die folgenden Schritte aus, um einen ID-Token-Hinweis in die Authentifizierungsanforderung einzufügen: 

1. Schließen Sie das Verfahren [Unterstützen erweiterter Szenarien](#support-advanced-scenarios) ab.
1. Definieren Sie ein [technisches ID-Token-Hinweisprofil](id-token-hint.md) in Ihrer benutzerdefinierten Richtlinie.
1. Fügen Sie die folgende Codezeile der *OnRedirectToIdentityProvider*-Funktion hinzu:
    
    ```csharp
    private async Task OnRedirectToIdentityProviderFunc(RedirectContext context)
    {
      // The idTokenHint variable holds your ID token 
      context.ProtocolMessage.IdTokenHint = idTokenHint

      // More code
      await Task.CompletedTask.ConfigureAwait(false);
    }
    ```
    
## <a name="account-controller"></a>Der Konto-Controller

Wenn Sie die Aktionen **Anmelden**, **Registrieren** oder **Abmelden** anpassen möchten, wird empfohlen, das Sie einen eigenen Controller erstellen. Mit einem eigenen Controller können Sie die Parameter zwischen Ihrem Controller und der Authentifizierungsbibliothek übergeben. `AccountController` ist Teil des `Microsoft.Identity.Web.UI` NuGet-Pakets, das die Anmelde- und Abmeldeaktionen verarbeitet. Die Implementierung dafür finden Sie in der [Microsoft Identity Web-Bibliothek](https://github.com/AzureAD/microsoft-identity-web/blob/master/src/Microsoft.Identity.Web.UI/Areas/MicrosoftIdentity/Controllers/AccountController.cs). 

Der folgende Codeausschnitt veranschaulicht eine benutzerdefinierte `MyAccountController` mit der **SignIn**-Aktion. Die Aktion übergibt einen Parameter namens `campaign_id` an die Authentifizierungsbibliothek.

```csharp
using System;
using System.Collections.Generic;
using System.Diagnostics;
using System.Linq;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Authentication;
using Microsoft.AspNetCore.Authentication.OpenIdConnect;
using Microsoft.AspNetCore.Authorization;
using Microsoft.AspNetCore.Mvc;


namespace mywebapp.Controllers
{
    [AllowAnonymous]
    [Area("MicrosoftIdentity")]
    [Route("[area]/[controller]/[action]")]
    public class MyAccountController : Controller
    {

        [HttpGet("{scheme?}")]
        public IActionResult SignIn([FromRoute] string scheme)
        {
            scheme ??= OpenIdConnectDefaults.AuthenticationScheme;
            var redirectUrl = Url.Content("~/");
            var properties = new AuthenticationProperties { RedirectUri = redirectUrl };
            properties.Items["campaign_id"] = "1234";
            return Challenge(properties, scheme);
        }

    }
}

```

Ändern Sie in der `_LoginPartial.cshtml`-Ansicht den Anmeldelink zu Ihrem Controller

```
<form method="get" asp-area="MicrosoftIdentity" asp-controller="MyAccount" asp-action="SignIn">
```

In der `OnRedirectToIdentityProvider` der `Startup.cs`-Aufrufe können Sie den benutzerdefinierten Parameter lesen:

```csharp
private async Task OnRedirectToIdentityProviderFunc(RedirectContext context)
{
    // Read the custom parameter
    var campaign_id = (context.Properties.Items.ContainsKey("campaign_id"))
    
    // Add your custom code here
    
    await Task.CompletedTask.ConfigureAwait(false);
}
```

## <a name="role-based-access-control"></a>Rollenbasierte Zugriffssteuerung

Mit der [Autorisierung in ASP.NET Core](/aspnet/core/security/authorization/introduction) können Sie die [rollenbasierte Autorisierung](/aspnet/core/security/authorization/roles), die [anspruchsbasierte Autorisierung](/aspnet/core/security/authorization/claims) oder die [richtlinienbasierte Autorisierung](/aspnet/core/security/authorization/policies) verwenden, um zu überprüfen, ob der Benutzer für den Zugriff auf eine geschützte Ressource autorisiert ist.

Fügen Sie die *AddAuthorization*-Methode, die das Autorisierungsmodell hinzufügt, zur *ConfigureServices*-Methode hinzu. Im folgenden Beispiel wird eine Richtlinie namens `EmployeeOnly` erstellt. Die Richtlinie überprüft, ob ein Anspruch `EmployeeNumber` vorhanden ist. Der Wert des Anspruchs muss eine der folgenden IDs sein: 1, 2, 3, 4 oder 5.

```csharp
services.AddAuthorization(options =>
    {
        options.AddPolicy("EmployeeOnly", policy =>
              policy.RequireClaim("EmployeeNumber", "1", "2", "3", "4", "5"));
    });
```

Die Autorisierung in ASP.NET Core wird mit dem [AuthorizeAttribute](/aspnet/core/security/authorization/simple) und dessen verschiedenen Parametern gesteuert. In seiner grundlegendsten Form schränkt das Anwenden des `[Authorize]`-Attributs auf einen Controller, eine Aktion oder eine Razor-Seite den Zugriff auf die authentifizierten Benutzer dieser Komponente ein.

Die Richtlinien werden auf den Controller angewendet, indem das `[Authorize]`-Attribut mit dem Richtliniennamen verwendet wird. Der folgende Code schränkt den Zugriff auf die `Claims`-Aktion auf die Benutzer ein, die durch die `EmployeeOnly`-Richtlinie autorisiert  sind:

```csharp
[Authorize(Policy = "EmployeeOnly")]
public IActionResult Claims()
{
    return View();
}
```

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen finden Sie hier: [Die Einführung in die Autorisierung in ASP.NET Core](/aspnet/core/security/authorization/introduction)