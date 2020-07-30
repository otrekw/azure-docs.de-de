---
title: Aufrufen einer Web-API aus einer Web-App – Microsoft Identity Platform | Azure
description: Hier erfahren Sie, wie Sie eine Web-App erstellen, die Web-APIs aufruft (Aufrufen einer geschützten Web-API).
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 07/14/2019
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 1e448f52f4e8c24dd8552cae873edac841e57fc6
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2020
ms.locfileid: "87058439"
---
# <a name="a-web-app-that-calls-web-apis-call-a-web-api"></a>Web-App, die Web-APIs aufruft: Aufrufen einer Web-API

Da Sie nun über ein Token verfügen, können Sie eine geschützte Web-API aufrufen.

## <a name="call-a-protected-web-api"></a>Aufrufen einer geschützten Web-API

Das Aufrufen einer geschützten Web-API hängt von der verwendeten Programmiersprache und dem Framework ab:

# <a name="aspnet-core"></a>[ASP.NET Core](#tab/aspnetcore)

Im Anschluss finden Sie einen vereinfachten Code für die Aktion von `HomeController`. Mit diesem wird ein Token für den Aufruf von Microsoft Graph abgerufen. Es wurde Code hinzugefügt, um das Aufrufen von Microsoft Graph als REST-API zu zeigen. Die URL für die Microsoft Graph-API wird in der Datei „appsettings.json“ bereitgestellt und in einer Variablen namens `webOptions` gelesen:

```json
{
  "AzureAd": {
    "Instance": "https://login.microsoftonline.com/",
    ...
  },
  ...
  "GraphApiUrl": "https://graph.microsoft.com"
}
```

```csharp
public async Task<IActionResult> Profile()
{
 // Acquire the access token.
 string[] scopes = new string[]{"user.read"};
 string accessToken = await tokenAcquisition.GetAccessTokenForUserAsync(scopes);

 // Use the access token to call a protected web API.
 HttpClient client = new HttpClient();
 client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", accessToken);

  var response = await httpClient.GetAsync($"{webOptions.GraphApiUrl}/beta/me");

  if (response.StatusCode == HttpStatusCode.OK)
  {
   var content = await response.Content.ReadAsStringAsync();

   dynamic me = JsonConvert.DeserializeObject(content);
   ViewData["Me"] = me;
  }

  return View();
}
```

> [!NOTE]
> Sie können dasselbe Prinzip für den Aufruf jeder anderen Web-API verwenden.
>
> Die meisten Azure-Web-APIs bieten ein SDK zur Vereinfachung des API-Aufrufs. Das ist auch bei Microsoft Graph der Fall. Im nächsten Artikel erfahren Sie, wo Sie ein Tutorial zur Veranschaulichung der API-Verwendung finden.

# <a name="java"></a>[Java](#tab/java)

```Java
private String getUserInfoFromGraph(String accessToken) throws Exception {
    // Microsoft Graph user endpoint
    URL url = new URL("https://graph.microsoft.com/v1.0/me");

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
    return responseObject.toString();
}

```

# <a name="python"></a>[Python](#tab/python)

```Python
@app.route("/graphcall")
def graphcall():
    token = _get_token_from_cache(app_config.SCOPE)
    if not token:
        return redirect(url_for("login"))
    graph_data = requests.get(  # Use token to call downstream service.
        app_config.ENDPOINT,
        headers={'Authorization': 'Bearer ' + token['access_token']},
        ).json()
    return render_template('display.html', result=graph_data)
```

---

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Übergang in die Produktion](scenario-web-app-call-api-production.md)
