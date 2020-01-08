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
ms.openlocfilehash: cc92fb7bc5ddf451279e6c157f9e93aa7fe9a12a
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2019
ms.locfileid: "75423618"
---
# <a name="web-api-that-calls-web-apis---acquire-a-token-for-the-app"></a>Web-API, die Web-APIs aufruft – Abrufen eines Tokens für die App

Mit Ihrem erstellen Clientanwendungsobjekt rufen Sie im Folgenden ein Token ab. Dieses verwenden Sie anschließend, um Web-APIs aufzurufen.

## <a name="code-in-the-controller"></a>Code im Controller

Hier sehen Sie ein Beispiel für Code, der in den Aktionen des API-Controllers aufgerufen wird und eine Downstream-API (mit dem Namen „ToDoList“) aufruft.

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

`BuildConfidentialClient()` gleicht dem unter [Web-API, die Web-APIs aufruft (App-Konfiguration)](scenario-web-api-call-api-app-configuration.md) Beschriebenen. `BuildConfidentialClient()` instanziiert `IConfidentialClientApplication` mit einem Zwischenspeicher, der alle Informationen zu einem Konto enthält. Das Konto wird über die `GetAccountIdentifier`-Methode bereitgestellt.

Die Methode `GetAccountIdentifier` verwendet die Ansprüche, die zur Identität des Benutzer gehören, für den die API das JSON Web Token erhalten hat:

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
> [Aufrufen einer Web-API](scenario-web-api-call-api-call-api.md)
