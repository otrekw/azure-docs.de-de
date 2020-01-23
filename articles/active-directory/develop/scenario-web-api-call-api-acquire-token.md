---
title: Abrufen eines Tokens für eine Web-API, die Web-APIs aufruft | Azure
titleSuffix: Microsoft identity platform
description: Hier erfahren Sie, wie Sie eine Web-API erstellen, die Web-APIs aufruft, für die ein Token für die App abgerufen werden muss.
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
ms.date: 05/07/2019
ms.author: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9cf660cbf981079ca20111e34fcd34504d8dcbfb
ms.sourcegitcommit: 05cdbb71b621c4dcc2ae2d92ca8c20f216ec9bc4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/16/2020
ms.locfileid: "76044122"
---
# <a name="a-web-api-that-calls-web-apis-acquire-a-token-for-the-app"></a>Eine Web-API, die Web-APIs aufruft: Abrufen eines Tokens für die App

Nach dem Erstellen eines Clientanwendungsobjekts rufen Sie damit ein Token ab. Dieses verwenden Sie anschließend, eine Web-API aufzurufen.

## <a name="code-in-the-controller"></a>Code im Controller

Im Folgenden sehen Sie ein Beispiel für den Code, der in den Aktionen der API-Controller aufgerufen wird. Er ruft eine nachgelagerte API namens *todolist* auf.

```csharp
private async Task GetTodoList(bool isAppStarting)
{
 ...
 //
 // Get an access token to call the To Do service.
 //
 AuthenticationResult result = null;
 try
 {
  app = BuildConfidentialClient(HttpContext, HttpContext.User);
  result = await app.AcquireTokenSilent(Scopes, account)
                     .ExecuteAsync()
                     .ConfigureAwait(false);
 }
...
}
```

`BuildConfidentialClient()` ähnelt dem Szenario in [Eine Web-API, die Web-APIs aufruft: App-Konfiguration](scenario-web-api-call-api-app-configuration.md). `BuildConfidentialClient()` instanziiert `IConfidentialClientApplication` mit einem Cache, der Informationen zu einem einzigen Konto enthält. Das Konto wird über die `GetAccountIdentifier`-Methode bereitgestellt.

Die `GetAccountIdentifier`-Methode verwendet die Ansprüche, die zur Identität des Benutzer gehören, für den die Web-API das JSON Web Token (JWT) empfangen hat:

```csharp
public static string GetMsalAccountId(this ClaimsPrincipal claimsPrincipal)
{
 string userObjectId = GetObjectId(claimsPrincipal);
 string tenantId = GetTenantId(claimsPrincipal);

 if (    !string.IsNullOrWhiteSpace(userObjectId)
      && !string.IsNullOrWhiteSpace(tenantId))
 {
  return $"{userObjectId}.{tenantId}";
 }

 return null;
}
```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Eine Web-API, die Web-APIs aufruft: Aufrufen einer API](scenario-web-api-call-api-call-api.md)
