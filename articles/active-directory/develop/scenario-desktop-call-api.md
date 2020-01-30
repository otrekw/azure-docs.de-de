---
title: 'Aufrufen von Web-APIs über eine Desktop-App: Microsoft Identity Platform | Azure'
description: Hier erfahren Sie, wie Sie eine Desktop-App erstellen, die Web-APIs aufruft.
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
ms.openlocfilehash: 2b3d9fdc163d0661670f3d0cf6e6a276c8b691bd
ms.sourcegitcommit: af6847f555841e838f245ff92c38ae512261426a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/23/2020
ms.locfileid: "76702163"
---
# <a name="desktop-app-that-calls-web-apis-call-a-web-api"></a>Desktop-App, die Web-APIs aufruft: Aufrufen einer Web-API

Da Sie nun über ein Token verfügen, können Sie eine geschützte Web-API aufrufen.

## <a name="call-a-web-api"></a>Aufrufen einer Web-API

# <a name="nettabdotnet"></a>[.NET](#tab/dotnet)

[!INCLUDE [Call web API in .NET](../../../includes/active-directory-develop-scenarios-call-apis-dotnet.md)]

<!--
More includes will come later for Python and Java
-->
# <a name="pythontabpython"></a>[Python](#tab/python)

```Python
endpoint = "url to the API"
http_headers = {'Authorization': 'Bearer ' + result['access_token'],
                'Accept': 'application/json',
                'Content-Type': 'application/json'}
data = requests.get(endpoint, headers=http_headers, stream=False).json()
```

# <a name="javatabjava"></a>[Java](#tab/java)

```Java
HttpURLConnection conn = (HttpURLConnection) url.openConnection();

// Set the appropriate header fields in the request header.
conn.setRequestProperty("Authorization", "Bearer " + accessToken);
conn.setRequestProperty("Accept", "application/json");

String response = HttpClientHelper.getResponseStringFromConn(conn);

int responseCode = conn.getResponseCode();
if(responseCode != HttpURLConnection.HTTP_OK) {
    throw new IOException(response);
}

JSONObject responseObject = HttpClientHelper.processResponse(responseCode, response);
```

# <a name="macostabmacos"></a>[MacOS](#tab/macOS)

## <a name="call-a-web-api-in-msal-for-ios-and-macos"></a>Aufrufen einer Web-API in MSAL für iOS und macOS

Diese Methoden zum Abrufen von Token geben ein `MSALResult`-Objekt zurück. `MSALResult` stellt die Eigenschaft `accessToken` bereit, die verwendet werden kann, um eine Web-API aufzurufen. Fügen Sie dem HTTP-Autorisierungsheader ein Zugriffstoken hinzu, bevor der Aufruf zum Zugriff auf die geschützte Web-API erfolgt.

Objective-C:

```objc
NSMutableURLRequest *urlRequest = [NSMutableURLRequest new];
urlRequest.URL = [NSURL URLWithString:"https://contoso.api.com"];
urlRequest.HTTPMethod = @"GET";
urlRequest.allHTTPHeaderFields = @{ @"Authorization" : [NSString stringWithFormat:@"Bearer %@", accessToken] };

NSURLSessionDataTask *task =
[[NSURLSession sharedSession] dataTaskWithRequest:urlRequest
     completionHandler:^(NSData * _Nullable data, NSURLResponse * _Nullable response, NSError * _Nullable error) {}];
[task resume];
```

Swift:

```swift
let urlRequest = NSMutableURLRequest()
urlRequest.url = URL(string: "https://contoso.api.com")!
urlRequest.httpMethod = "GET"
urlRequest.allHTTPHeaderFields = [ "Authorization" : "Bearer \(accessToken)" ]

let task = URLSession.shared.dataTask(with: urlRequest as URLRequest) { (data: Data?, response: URLResponse?, error: Error?) in }
task.resume()
```

## <a name="call-several-apis-incremental-consent-and-conditional-access"></a>Aufrufen mehrerer APIs: Inkrementelle Zustimmung und bedingter Zugriff

Um nach dem Abrufen eines Tokens für die erste API mehrere APIs für denselben Benutzer aufzurufen, rufen Sie `AcquireTokenSilent` auf. In den meisten Fällen erhalten Sie ein Token für die anderen APIs automatisch.

```csharp
var result = await app.AcquireTokenXX("scopeApi1")
                      .ExecuteAsync();

result = await app.AcquireTokenSilent("scopeApi2")
                  .ExecuteAsync();
```

Ein Interaktion ist in folgenden Fällen erforderlich:

- Der Benutzer hat der ersten API zugestimmt, muss nun aber weiteren Bereichen zustimmen. Diese Art von Zustimmung wird als inkrementelle Zustimmung bezeichnet.
- Für die erste API war keine mehrstufige Authentifizierung erforderlich, für die nächste wird diese jedoch benötigt.

```csharp
var result = await app.AcquireTokenXX("scopeApi1")
                      .ExecuteAsync();

try
{
 result = await app.AcquireTokenSilent("scopeApi2")
                  .ExecuteAsync();
}
catch(MsalUiRequiredException ex)
{
 result = await app.AcquireTokenInteractive("scopeApi2")
                  .WithClaims(ex.Claims)
                  .ExecuteAsync();
}
```
---

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Übergang in die Produktion](scenario-desktop-production.md)
