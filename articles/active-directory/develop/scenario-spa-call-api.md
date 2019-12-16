---
title: Erstellen einer Single-Page-Webanwendung, die eine Web-API aufruft – Microsoft Identity Platform | Azure
description: Informationen zum Erstellen einer Single-Page-Webanwendung, die eine Web-API aufruft.
services: active-directory
documentationcenter: dev-center-name
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/06/2019
ms.author: ryanwi
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1171d8c3bc28c7b325cc8daf6cc072965363339c
ms.sourcegitcommit: 5ab4f7a81d04a58f235071240718dfae3f1b370b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/10/2019
ms.locfileid: "74965330"
---
# <a name="single-page-application-call-a-web-api"></a>Single-Page-Webanwendung: Aufrufen einer Web-API

Es wird empfohlen, dass Sie die `acquireTokenSilent`-Methode aufrufen, um ein Zugriffstoken abzurufen oder zu verlängern, bevor Sie eine Web-API aufrufen. Sobald Sie über ein Token verfügen, können Sie eine geschützte Web-API aufrufen.

## <a name="call-a-web-api"></a>Aufrufen einer Web-API

### <a name="javascript"></a>JavaScript

Verwenden Sie das abgerufene Zugriffstoken als Bearer in einer HTTP-Anforderung, um eine beliebige Web-API (z. B. Microsoft Graph-API) aufzurufen. Beispiel:

```javascript
    var headers = new Headers();
    var bearer = "Bearer " + access_token;
    headers.append("Authorization", bearer);
    var options = {
         method: "GET",
         headers: headers
    };
    var graphEndpoint = "https://graph.microsoft.com/v1.0/me";

    fetch(graphEndpoint, options)
        .then(function (response) {
             //do something with response
        }
```

### <a name="angular"></a>Angular

Der MSAL Angular-Wrapper nutzt den HTTP-Interceptor, um Zugriffstoken automatisch im Hintergrund abzurufen und sie an die HTTP-Anforderungen für APIs anzufügen. Weitere Informationen finden Sie unter [Abrufen eines Tokens zum Aufrufen einer API](scenario-spa-acquire-token.md).

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Übergang in die Produktion](scenario-spa-production.md)
