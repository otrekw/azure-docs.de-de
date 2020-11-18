---
title: 'Azure App Configuration-REST-API: Versionsverwaltung'
description: Referenzseiten für die Versionsverwaltung über die Azure App Configuration-REST-API
author: lisaguthrie
ms.author: lcozzens
ms.service: azure-app-configuration
ms.topic: reference
ms.date: 08/17/2020
ms.openlocfilehash: 90d131cdc7c496853f2520951c95b9903d69f8fb
ms.sourcegitcommit: 7cc10b9c3c12c97a2903d01293e42e442f8ac751
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2020
ms.locfileid: "93423746"
---
# <a name="versioning"></a>Versionskontrolle

api-version: 1.0

Jede Clientanforderung muss eine explizite API-Version als Abfragezeichenfolgenparameter angeben. Beispiel: `https://{myconfig}.azconfig.io/kv?api-version=1.0`

`api-version` wird im SemVer-Format (Hauptversion.Nebenversion) ausgedrückt. Eine Bereichs- oder Versionsaushandlung wird nicht unterstützt.

## <a name="error-response"></a>Fehlerantwort

Im Folgenden finden Sie eine Übersicht der möglichen Fehlerantworten, die vom Server zurückgegeben werden, wenn keine Übereinstimmung mit der angeforderten API-Version gefunden wird.

### <a name="api-version-unspecified"></a>API-Version nicht spezifiziert

Tritt auf, wenn ein Client eine Anforderung ohne Angabe einer API-Version sendet.

```http
HTTP/1.1 400 Bad Request
Content-Type: application/problem+json; charset=utf-8
{
  "type": "https://azconfig.io/errors/invalid-argument",
  "title": "API version is not specified",
  "name": "api-version",
  "detail": "An API version is required, but was not specified.",
  "status": 400
}
```

### <a name="unsupported-api-version"></a>Nicht unterstützte API-Version

Tritt auf, wenn eine vom Client angeforderte API-Version mit keiner der vom Server unterstützten API-Versionen übereinstimmt.

```http
HTTP/1.1 400 Bad Request
Content-Type: application/problem+json; charset=utf-8
{
  "type": "https://azconfig.io/errors/invalid-argument",
  "title": "Unsupported API version",
  "name": "api-version",
  "detail": "The HTTP resource that matches the request URI '{request uri}' does not support the API version '{api-version}'.",
  "status": 400
}
```

### <a name="invalid-api-version"></a>Ungültige API-Version

Tritt auf, wenn ein Client eine Anforderung mit einer API-Version sendet, aber der Wert nicht wohlgeformt ist oder vom Server nicht analysiert werden kann.

```http
HTTP/1.1 400 Bad Request
Content-Type: application/problem+json; charset=utf-8  
{
  "type": "https://azconfig.io/errors/invalid-argument",
  "title": "Invalid API version",
  "name": "api-version",
  "detail": "The HTTP resource that matches the request URI '{request uri}' does not support the API version '{api-version}'.",
  "status": 400
}
```

### <a name="ambiguous-api-version"></a>Mehrdeutige API-Version

Tritt auf, wenn ein Client eine API-Version anfordert, die auf dem Server nicht eindeutig ist. Beispiel: mehrere verschiedene Werte.

```http
HTTP/1.1 400 Bad Request
Content-Type: application/problem+json; charset=utf-8
{
  "type": "https://azconfig.io/errors/invalid-argument",
  "title": "Ambiguous API version",
  "name": "api-version",
  "detail": "The following API versions were requested: {comma separated api versions}. At most, only a single API version may be specified. Please update the intended API version and retry the request.",
  "status": 400
}
```
