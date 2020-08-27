---
title: Zwischenspeichern des Authentifizierungstokens
titleSuffix: Azure Cognitive Services
description: In diesem Artikel erfahren Sie, wie Sie das Authentifizierungstoken zwischenspeichern.
author: metanMSFT
manager: guillasi
ms.service: cognitive-services
ms.subservice: immersive-reader
ms.topic: conceptual
ms.date: 01/14/2020
ms.author: metan
ms.custom: devx-track-javascript, devx-track-csharp
ms.openlocfilehash: 6850012272251c910362b61363dacd9090976680
ms.sourcegitcommit: 62e1884457b64fd798da8ada59dbf623ef27fe97
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/26/2020
ms.locfileid: "88918416"
---
# <a name="how-to-cache-the-authentication-token"></a>Vorgehensweise: Zwischenspeichern des Authentifizierungstokens

In diesem Artikel wird veranschaulicht, wie das Authentifizierungstoken zwischengespeichert wird, um die Leistung Ihrer Anwendung zu verbessern.

## <a name="using-aspnet"></a>Verwenden von ASP.NET

Importieren Sie das NuGet-Paket **Microsoft.IdentityModel.Clients.ActiveDirectory**, das zum Abrufen eines Tokens verwendet wird. Verwenden Sie als nächstes den folgenden Code, um mithilfe der Authentifizierungswerte, die Sie beim [Erstellen der Plastischer Reader-Ressource](./how-to-create-immersive-reader.md) erhalten haben, ein `AuthenticationResult` abzurufen.

```csharp
private async Task<AuthenticationResult> GetTokenAsync()
{
    AuthenticationContext authContext = new AuthenticationContext($"https://login.windows.net/{TENANT_ID}");
    ClientCredential clientCredential = new ClientCredential(CLIENT_ID, CLIENT_SECRET);
    AuthenticationResult authResult = await authContext.AcquireTokenAsync("https://cognitiveservices.azure.com/", clientCredential);
    return authResult;
}
```

Das `AuthenticationResult`-Objekt verfügt über eine `AccessToken`-Eigenschaft, bei der es sich um das tatsächliche Token handelt, das Sie beim Starten des plastischen Readers mit dem SDK verwenden. Außerdem verfügt es über eine `ExpiresOn`-Eigenschaft, die angibt, wann das Token abläuft. Vor dem Starten des plastischen Readers können Sie überprüfen, ob das Token abgelaufen ist, um ein neues Token nur dann abzurufen, wenn es abgelaufen ist.

## <a name="using-nodejs"></a>Verwenden von Node.JS

Fügen Sie Ihrem Projekt das npm-Paket [**request**](https://www.npmjs.com/package/request) hinzu. Verwenden Sie den folgenden Code, um mithilfe der Authentifizierungswerte, die Sie beim [Erstellen der Plastischer Reader-Ressource](./how-to-create-immersive-reader.md) erhalten haben, ein Token abzurufen.

```javascript
router.get('/token', function(req, res) {
    request.post(
        {
            headers: { 'content-type': 'application/x-www-form-urlencoded' },
            url: `https://login.windows.net/${TENANT_ID}/oauth2/token`,
            form: {
                grant_type: 'client_credentials',
                client_id: CLIENT_ID,
                client_secret: CLIENT_SECRET,
                resource: 'https://cognitiveservices.azure.com/'
            }
        },
        function(err, resp, json) {
            const result = JSON.parse(json);
            return res.send({
                access_token: result.access_token,
                expires_on: result.expires_on
            });
        }
    );
});
```

Die `expires_on`-Eigenschaft ist der Zeitpunkt in Datum und Uhrzeit, zu dem das Token abläuft, ausgedrückt als die Anzahl der Sekunden, die seit dem 1. Januar 1970 UTC verstrichen sind. Verwenden Sie diesen Wert, um zu bestimmen, ob das Token abgelaufen ist, bevor Sie versuchen, ein neues Token abzurufen.

```javascript
async function getToken() {
    if (Date.now() / 1000 > CREDENTIALS.expires_on) {
        CREDENTIALS = await refreshCredentials();
    }
    return CREDENTIALS.access_token;
}
```

## <a name="next-steps"></a>Nächste Schritte

* Lesen Sie die [Referenz zum SDK für den plastischen Reader](./reference.md).