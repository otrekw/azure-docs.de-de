---
title: Aufrufen einer Web-API über eine Daemon-App | Azure
titleSuffix: Microsoft identity platform
description: Hier erfahren Sie, wie Sie eine Daemon-App erstellen, die eine Web-API aufruft.
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 10/30/2019
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: f4b223c9195168b445b7eb81c2709a61807fddac
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "104578395"
---
# <a name="daemon-app-that-calls-web-apis---call-a-web-api-from-the-app"></a>Daemon-App, die Web-APIs aufruft – Aufrufen einer Web-API aus der App

.NET-Daemon-Apps können eine Web-API aufrufen. .NET-Daemon-Apps können auch mehrere vorab genehmigte Web-APIs aufrufen.

## <a name="calling-a-web-api-from-a-daemon-application"></a>Aufrufen einer Web-API aus einer Daemonanwendung

Gehen Sie wie folgt vor, um eine API unter Verwendung des Tokens aufzurufen:

# <a name="net"></a>[.NET](#tab/dotnet)

[!INCLUDE [Call web API in .NET](../../../includes/active-directory-develop-scenarios-call-apis-dotnet.md)]

# <a name="java"></a>[Java](#tab/java)

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

# <a name="nodejs"></a>[Node.js](#tab/nodejs)

Rufen Sie mithilfe eines HTTP-Clients wie [Axios](https://www.npmjs.com/package/axios) den API-Endpunkt-URI mit einem Zugriffstoken wie dem *Autorisierungsbearertoken* auf.

```JavaScript
const axios = require('axios');

async function callApi(endpoint, accessToken) {

    const options = {
        headers: {
            Authorization: `Bearer ${accessToken}`
        }
    };

    console.log('request made to web API at: ' + new Date().toString());

    try {
        const response = await axios.default.get(endpoint, options);
        return response.data;
    } catch (error) {
        console.log(error)
        return error;
    }
};
```

# <a name="python"></a>[Python](#tab/python)

```Python
endpoint = "url to the API"
http_headers = {'Authorization': 'Bearer ' + result['access_token'],
                'Accept': 'application/json',
                'Content-Type': 'application/json'}
data = requests.get(endpoint, headers=http_headers, stream=False).json()
```

---

## <a name="calling-several-apis"></a>Aufrufen mehrerer APIs

Für Daemon-Apps müssen die von Ihnen aufgerufenen Web-APIs vorab genehmigt werden. Es gibt keine inkrementelle Einwilligung für Daemon-Apps. (Es gibt keine Benutzerinteraktion.) Der Mandantenadministrator muss vorab in die Berechtigungen für die Anwendung und alle APIs einwilligen. Wenn Sie mehrere APIs aufrufen möchten, müssen Sie für jede Ressource ein Token abrufen, indem Sie jedes Mal `AcquireTokenForClient` aufrufen. MSAL verwendet den Tokencache der Anwendung, um unnötige Dienstaufrufe zu vermeiden.

## <a name="next-steps"></a>Nächste Schritte

# <a name="net"></a>[.NET](#tab/dotnet)

Fahren Sie mit dem nächsten Artikel in diesem Szenario fort: [Überführen in die Produktion](./scenario-daemon-production.md?tabs=dotnet).

# <a name="java"></a>[Java](#tab/java)

Fahren Sie mit dem nächsten Artikel in diesem Szenario fort: [Überführen in die Produktion](./scenario-daemon-production.md?tabs=java).

# <a name="nodejs"></a>[Node.js](#tab/nodejs)

Fahren Sie mit dem nächsten Artikel in diesem Szenario fort: [Überführen in die Produktion](./scenario-daemon-production.md?tabs=nodejs).

# <a name="python"></a>[Python](#tab/python)

Fahren Sie mit dem nächsten Artikel in diesem Szenario fort: [Überführen in die Produktion](./scenario-daemon-production.md?tabs=python).

---
