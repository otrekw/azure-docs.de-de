---
title: Entfernen von Konten aus dem Tokencache bei der Abmeldung – Microsoft Identity Platform | Azure
description: Erfahren Sie, wie Sie ein Konto bei der Abmeldung aus dem Tokencache entfernen
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 09/30/2019
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 934b756329065c466f21fca1480247065bdea28b
ms.sourcegitcommit: d187fe0143d7dbaf8d775150453bd3c188087411
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/08/2020
ms.locfileid: "80881611"
---
# <a name="a-web-app-that-calls-web-apis-remove-accounts-from-the-token-cache-on-global-sign-out"></a>Web-App, die Web-APIs aufruft: Entfernen von Konten aus dem Tokencache bei der globalen Abmeldung

Sie haben bereits erfahren, wie Sie Ihrer Web-App eine Anmeldefunktion hinzufügen (siehe [Web-App, die Benutzer anmeldet: An- und Abmeldung](scenario-web-app-sign-user-sign-in.md)).

Bei einer Web-App, die Web-APIs aufruft, funktioniert die Abmeldung anders. Wenn sich der Benutzer von Ihrer Anwendung (oder von einer beliebigen Anwendung) abmeldet, müssen Sie die diesem Benutzer zugeordneten Token aus dem Tokencache entfernen.

## <a name="intercept-the-callback-after-single-sign-out"></a>Abfangen des Rückrufs nach einmaliger Abmeldung

Um den Eintrag, der mit dem abgemeldeten Konto verbunden ist, aus dem Tokencache zu löschen, kann Ihre Anwendung das „after `logout`“-Ereignis abfangen. Web-Apps speichern die Zugriffstoken für die einzelnen Benutzer in einem Tokencache. Durch das Abfangen des „after `logout`“-Rückrufs kann Ihre Web-App den Benutzer aus dem Cache entfernen.

# <a name="aspnet-core"></a>[ASP.NET Core](#tab/aspnetcore)

Bei ASP.NET Core wird dieser Abfangmechanismus in der `AddMsal()`-Methode von [WebAppServiceCollectionExtensions.cs#L151-L157](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/blob/db7f74fd7e65bab9d21092ac1b98a00803e5ceb2/Microsoft.Identity.Web/WebAppServiceCollectionExtensions.cs#L151-L157) veranschaulicht.

Die Abmelde-URL, die Sie zuvor für Ihre Anwendung registriert haben, ermöglicht Ihnen das Implementieren der einmaligen Abmeldung. Der `logout`-Endpunkt von Microsoft Identity Platform ruft Ihre Abmelde-URL auf. Dieser Aufruf erfolgt, wenn die Abmeldung von Ihrer Web-App, einer anderen Web-App oder dem Browser gestartet wurde. Weitere Informationen finden Sie unter [Einmaliges Abmelden](v2-protocols-oidc.md#single-sign-out).

```csharp
public static class WebAppServiceCollectionExtensions
{
 public static IServiceCollection AddMsal(this IServiceCollection services, IConfiguration configuration, IEnumerable<string> initialScopes, string configSectionName = "AzureAd")
 {
  // Code omitted here

  services.Configure<OpenIdConnectOptions>(AzureADDefaults.OpenIdScheme, options =>
  {
   // Code omitted here

   // Handling the sign-out: Remove the account from MSAL.NET cache.
   options.Events.OnRedirectToIdentityProviderForSignOut = async context =>
   {
    // Remove the account from MSAL.NET token cache.
    var tokenAcquisition = context.HttpContext.RequestServices.GetRequiredService<ITokenAcquisition>();
    await tokenAcquisition.RemoveAccountAsync(context).ConfigureAwait(false);
   };
  });
  return services;
 }
}
```

Der Code für `RemoveAccountAsync` ist unter [Microsoft.Identity.Web/TokenAcquisition.cs#L264-L288](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/blob/db7f74fd7e65bab9d21092ac1b98a00803e5ceb2/Microsoft.Identity.Web/TokenAcquisition.cs#L264-L288) verfügbar.

# <a name="aspnet"></a>[ASP.NET](#tab/aspnet)

Im ASP.NET-Beispiel werden Konten bei der globalen Abmeldung nicht aus dem Cache entfernt.

# <a name="java"></a>[Java](#tab/java)

Im Java-Beispiel werden Konten bei der globalen Abmeldung nicht aus dem Cache entfernt.

# <a name="python"></a>[Python](#tab/python)

Im Python-Beispiel werden Konten bei der globalen Abmeldung nicht aus dem Cache entfernt.

---

## <a name="next-steps"></a>Nächste Schritte

# <a name="aspnet-core"></a>[ASP.NET Core](#tab/aspnetcore)

> [!div class="nextstepaction"]
> [Abrufen eines Tokens für die Web-App](https://docs.microsoft.com/azure/active-directory/develop/scenario-web-app-call-api-acquire-token?tabs=aspnetcore)

# <a name="aspnet"></a>[ASP.NET](#tab/aspnet)

> [!div class="nextstepaction"]
> [Abrufen eines Tokens für die Web-App](https://docs.microsoft.com/azure/active-directory/develop/scenario-web-app-call-api-acquire-token?tabs=aspnet)

# <a name="java"></a>[Java](#tab/java)

> [!div class="nextstepaction"]
> [Abrufen eines Tokens für die Web-App](https://docs.microsoft.com/azure/active-directory/develop/scenario-web-app-call-api-acquire-token?tabs=java)

# <a name="python"></a>[Python](#tab/python)

> [!div class="nextstepaction"]
> [Abrufen eines Tokens für die Web-App](https://docs.microsoft.com/azure/active-directory/develop/scenario-web-app-call-api-acquire-token?tabs=python)

---
