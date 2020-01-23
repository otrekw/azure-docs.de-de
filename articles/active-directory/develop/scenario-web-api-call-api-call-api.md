---
title: 'Web-API, die Web-APIs aufruft: Microsoft Identity Platform | Azure'
description: Hier erfahren Sie, wie Sie eine Web-API erstellen, die Web-APIs aufruft.
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
ms.openlocfilehash: 701f1f7c26731f3e9653955907f5f16d2688cdb2
ms.sourcegitcommit: 05cdbb71b621c4dcc2ae2d92ca8c20f216ec9bc4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/16/2020
ms.locfileid: "76043440"
---
# <a name="a-web-api-that-calls-web-apis-call-an-api"></a>Eine Web-API, die Web-APIs aufruft: Aufrufen einer API

Sobald Sie über ein Token verfügen, können Sie eine geschützte Web-API aufrufen. Dafür verwenden Sie den Controller Ihrer ASP.NET- oder ASP.NET Core-Web-API.

## <a name="controller-code"></a>Controllercode

Der folgende Code setzt den Beispielcode fort, den Sie hier finden: [Eine Web-API, die Web-APIs aufruft: Abrufen eines Tokens für die App](scenario-web-api-call-api-acquire-token.md). Der Code wird in den Aktionen der API-Controller aufgerufen. Er ruft eine nachgelagerte API namens *todolist* auf.

Nachdem Sie das Token abgerufen haben, können Sie es als Bearertoken zum Aufrufen der nachgelagerten API verwenden.

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

// After the token has been returned by Microsoft Authentication Library (MSAL), add it to the HTTP authorization header before making the call to access the To Do list service.
_httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);

// Call the To Do list service.
HttpResponseMessage response = await _httpClient.GetAsync(TodoListBaseAddress + "/api/todolist");
...
}
```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Eine Web-API, die Web-APIs aufruft: Übergang in die Produktion](scenario-web-api-call-api-production.md)
