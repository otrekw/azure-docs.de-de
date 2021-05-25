---
title: Anspruchsaufforderungen und -anforderungen
titleSuffix: Microsoft identity platform
description: Erläuterung von Anspruchsherausforderungen und -anforderungen auf der Microsoft Identity Platform.
services: active-directory
author: knicholasa
manager: martinco
ms.service: active-directory
ms.subservice: develop
ms.topic: reference
ms.workload: identity
ms.date: 05/11/2021
ms.author: nichola
ms.reviewer: kkrishna, kylemar
ms.openlocfilehash: bf3b6db0bcb5478412837a2fa43f610dbf8690d6
ms.sourcegitcommit: 32ee8da1440a2d81c49ff25c5922f786e85109b4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/12/2021
ms.locfileid: "109795432"
---
# <a name="claims-challenges-and-claims-requests"></a>Anspruchsaufforderungen und -anforderungen

Eine **Anspruchsaufforderung** ist eine Antwort, die von einer API gesendet wird und angibt, dass ein von einer Clientanwendung gesendetes Zugriffstoken über unzureichende Ansprüche verfügt. Dies kann daran liegen, dass das Token die für diese API festgelegten Richtlinien für bedingten Zugriff nicht erfüllt oder das Zugriffstoken widerrufen wurde.

Von der Clientanwendung wird eine **Anspruchsanforderung** gestellt, um den Benutzer zurück an den Identitätsanbieter umzuleiten, um ein neues Token mit Ansprüchen abzurufen, die die zusätzlichen Anforderungen erfüllen, die nicht erfüllt wurden.

Anwendungen, die erweiterte Sicherheitsfeatures wie [fortlaufende Zugriffsevaluierung (CAE)](../conditional-access/concept-continuous-access-evaluation.md) und den [Authentifizierungskontext für bedingten Zugriff](https://techcommunity.microsoft.com/t5/azure-active-directory-identity/granular-conditional-access-for-sensitive-data-and-actions/ba-p/1751775) verwenden, müssen darauf vorbereitet sein, Anspruchsherausforderungen zu bewältigen.

Ihre Anwendung erhält nur dann Anspruchsherausforderungen, wenn sie deklariert, dass sie sie mithilfe von **Clientfunktionen** verarbeiten kann.

Um Informationen darüber zu erhalten, ob Clientanwendungen Anspruchsherausforderungen bewältigen können, muss ein API-Implementator **xms_cc** als optionalen Anspruch im Anwendungsmanifest anfordern.

## <a name="claims-challenge-header-format"></a>Anspruchsausforderung: Headerformat

Die Anspruchsausforderung ist eine Direktive im www-authenticate-Header, die von einer API zurückgegeben wird, wenn ein Zugriffstoken nicht autorisiert ist und ein neues Zugriffstoken erforderlich ist. Die Anspruchsforderung besteht aus mehreren Teilen: dem HTTP-Statuscode der Antwort und dem www-authenticate-Header, der selbst aus mehreren Teilen besteht und eine Anspruchs-Direktive enthalten muss.

``` https
HTTP 401; Unauthorized

www-authenticate =Bearer realm="", authorization_uri="https://login.microsoftonline.com/common/oauth2/authorize", error="insufficient_claims", claims="eyJhY2Nlc3NfdG9rZW4iOnsiYWNycyI6eyJlc3NlbnRpYWwiOnRydWUsInZhbHVlIjoiYzEifX19"
```

 **HTTP-Statuscode**: Muss **401 Nicht autorisiert** lauten.

**www-authenticate-Antwortheader** mit:

| Parameter    | Erforderlich/optional | Beschreibung |
|--------------|-------------|--------------|
| Authentifizierungsart | Erforderlich | Muss **Bearer** sein.|
| Realm | Optional | Die Mandanten-ID oder der Domänenname des Mandanten (z. B. microsoft.com), auf die zugegriffen wird. MUSS eine leere Zeichenfolge sein, wenn die Authentifizierung den gemeinsamen [Endpunkt](howto-convert-app-to-be-multi-tenant.md#update-your-code-to-send-requests-to-common) durchläuft. |
| `authorization_uri` | Erforderlich | Der URI des Autorisierungsendpunkts, an dem bei Bedarf eine interaktive Authentifizierung durchgeführt werden kann. Wenn im Bereich angegeben, MÜSSEN die Mandanteninformationen im authorization_uri enthalten sein. Wenn realm eine leere Zeichenfolge ist, MUSS die authorization_uri für den [allgemeinen Endpunkt](howto-convert-app-to-be-multi-tenant.md#update-your-code-to-send-requests-to-common) gelten. |
| `error` | Erforderlich | Muss „insufficient_claims“ sein, wenn eine Anspruchsaufforderung generiert werden soll. | 
| `claims` | Erforderlich, wenn der Fehler „insufficient_claims“ lautet. | Eine Zeichenfolge in Anführungszeichen, die eine Base64-codierte [Anspruchsanforderung](https://openid.net/specs/openid-connect-core-1_0.html#ClaimsParameter) enthält. Die Anspruchsanforderung sollte Ansprüche für den „access_token“ auf der obersten Ebene des JSON-Objekts anfordern. Der Wert (angeforderte Ansprüche) ist kontextabhängig und wird später in diesem Dokument angegeben. Aus Größengründen SOLLTEN Anwendungen der vertrauenden Seite den JSON-Code vor der Base64-Codierung minimieren. Der unformatierte JSON-Code des obigen Beispiels lautet {"access_token":{"acrs":{"essential":true,"value":"cp1"}}}. |

Die Antwort 401 kann mehrere www-authenticate-Header enthalten. Alle oben genannten Felder müssen im gleichen www-authenticate-Header enthalten sein. Der Header www-authenticate mit der Anspruchsaufforderung KÖNNTE andere Felder enthalten. Felder im Header sind ungeordnet. Gemäß RFC 7235 darf jeder Parametername nur einmal pro Abfrage des Authentifizierungsschemas auftreten.

## <a name="claims-request"></a>Anspruchsanforderung

Wenn eine Anwendung eine Anspruchsaufforderung empfängt, die angibt, dass das vorherige Zugriffstoken nicht mehr als gültig betrachtet wird, sollte die Anwendung das Token aus einem lokalen Cache oder einer Benutzersitzung löschen. Anschließend sollte der angemeldete Benutzer zurück an Azure AD umgeleitet werden, um ein neues Token mithilfe des [OAuth 2.0-Autorisierungscodeflows](v2-oauth2-auth-code-flow.md) mit einem **Anspruchsparameter** abzurufen, der die zusätzlichen Anforderungen erfüllt, die nicht erfüllt wurden.

Unten sehen Sie ein Beispiel:

``` https
GET https://login.microsoftonline.com/14c2f153-90a7-4689-9db7-9543bf084dad/oauth2/v2.0/authorize
?client_id=2810aca2-a927-4d26-8bca-5b32c1ef5ea9
&redirect_uri=https%3A%2F%contoso.com%3A44321%2Fsignin-oidc
&response_type=code
&scope=openid%20profile%20offline_access%20user.read%20Sites.Read.All
&response_mode=form_post
&login_hint=kalyan%ccontoso.onmicrosoft.com
&domain_hint=organizations
claims=%7B%22access_token%22%3A%7B%22acrs%22%3A%7B%22essential%22%3Atrue%2C%22value%22%3A%22urn%3Amicrosoft%3Areq1%22%7D%7D%7D
```

Die Anspruchsaufforderung sollte als Teil aller Aufrufe des [/authorize](v2-oauth2-auth-code-flow.md#request-an-authorization-code)-Endpunkts von Azure AD übergeben werden, bis ein Token erfolgreich abgerufen wurde und danach nicht mehr benötigt wird.

Zum Auffüllen des Anspruchsparameters muss der Entwickler Folgendes unternehmen:

1. Decodieren Sie die zuvor empfangene Base64-Zeichenfolge.
2. URL Codieren Sie die Zeichenfolge, und fügen Sie sie dem **Anspruchsparameter** erneut hinzu.

Nach Abschluss dieses Ablaufs erhält die Anwendung ein Zugriffstoken mit den zusätzlichen Ansprüchen, die belegen, dass der Benutzer die erforderlichen Bedingungen erfüllt hat.

## <a name="client-capabilities"></a>Clientfunktionen

Ihre Anwendung erhält nur dann Anspruchsherausforderungen, wenn sie deklariert, dass sie sie mithilfe von **Clientfunktionen** verarbeiten kann.

Um zusätzlichen Datenverkehr oder Auswirkungen auf die Benutzerfreundlichkeit zu vermeiden, geht Azure AD davon aus, dass Ihre App Ansprüche verarbeiten kann, es sei denn, Sie entscheiden sich explizit dafür. Eine Anwendung erhält keine Anspruchsherausforderungen (und kann die zugehörigen Features wie CAE-Token nicht verwenden), es sei denn, sie deklariert, dass sie bereit ist, sie mit der „cp1“-Funktion zu behandeln.

### <a name="how-to-communicate-client-capabilities-to-azure-ad"></a>Wie Clientfunktionen an Azure AD kommuniziert werden

Der folgende Beispiel-Anspruchsparameter zeigt, wie eine Clientanwendung ihre Funktion zum Azure AD in einem [OAuth 2.0-Autorisierungscodefluss](v2-oauth2-auth-code-flow.md) kommuniziert.

```json
Claims: {"access_token":{"xms_cc":{"values":["cp1"]}}}
```

Diejenigen, die die MSAL-Bibliothek verwenden, verwenden den folgenden Code:

```c#
_clientApp = PublicClientApplicationBuilder.Create(App.ClientId)
 .WithDefaultRedirectUri()
 .WithAuthority(authority)
 .WithClientCapabilities(new [] {"cp1"})
 .Build();*
```

Benutzer, die Microsoft.Identity.Web verwenden, können der Konfigurationsdatei den folgenden Code hinzufügen:

```c#
{
  "AzureAd": {
    "Instance": "https://login.microsoftonline.com/",
    // the remaining settings
    // ... 
    "ClientCapabilities": [ "cp1" ]
},
```

Ein Beispiel dafür, wie die Anforderung an Azure AD aussieht:

```https
GET https://login.microsoftonline.com/14c2f153-90a7-4689-9db7-9543bf084dad/oauth2/v2.0/authorize
?client_id=2810aca2-a927-4d26-8bca-5b32c1ef5ea9
&redirect_uri=https%3A%2F%contoso.com%3A44321%2Fsignin-oidc
&response_type=code
&scope=openid%20profile%20offline_access%20user.read%20Sites.Read.All
&response_mode=form_post
&login_hint=kalyan%ccontoso.onmicrosoft.com
&domain_hint=organizations
&claims=%7B%22access_token%22%3A%7B%22xms_cc%22%3A%7B%22values%22%3A%5B%22cp1%22%5D%7D%7D%7D
```

Wenn Sie bereits über eine Nutzlast für den Anspruchsparameter verfügen, fügen Sie diese dem vorhandenen Satz hinzu.

Beispiel: Sie verfügen bereits über die folgende Antwort von einem Authentifizierungskontextvorgang für den Bedingungszugriff

```json
{"access_token":{"acrs":{"essential":true,"value":"c25"}}}
```

Sie würden die Clientfunktion in der vorhandenen **Anspruchsnutzlast** vorausbeschreiten.

```json
{"access_token":{"xms_cc":{"values":["cp1"]},"acrs":{"essential":true,"value":"c25"}}}
```

## <a name="receiving-xms_cc-claim-in-an-access-token"></a>Empfangen xms_cc Anspruchs in einem Zugriffstoken

Um Informationen darüber zu erhalten, ob Clientanwendungen Anspruchsherausforderungen bewältigen können, muss ein API-Implementator **xms_cc** als optionalen Anspruch im Anwendungsmanifest anfordern.

Der **xms_cc** Anspruch mit dem Wert „cp1“ im Zugriffstoken ist die autoritative Methode, um zu bestimmen, dass eine Clientanwendung eine Anspruchsforderung behandeln kann. **xms_cc** ist ein optionaler Anspruch, der nicht immer im Zugriffstoken ausgestellt wird, selbst wenn der Client eine Anspruchsanforderung mit „xms_cc“ sendet. Damit ein Zugriffstoken den Anspruch **xms_cc** enthalten kann, muss die Ressourcenanwendung (d. h. die API-Implementierung) xms_cc als [optionalen Anspruch](active-directory-optional-claims.md) im Anwendungsmanifest anfordern. Bei einer Anforderung als optionaler Anspruch wird **xms_cc** nur dann dem Zugriffstoken hinzugefügt, wenn die Clientanwendung **xms_cc** in der Anspruchsanforderung sendet. Der Wert der **xms_cc** Anspruchsanforderung wird als Wert des **xms_cc** Anspruchs in das Zugriffstoken eingeschlossen, wenn es sich um einen bekannten Wert handelt. Der einzige derzeit bekannte Wert ist **cp1**.

Bei den ungeordneten Werten wird nicht zwischen Groß- und Kleinschreibung unterschieden. Wenn in der **xms_cc** Anspruchsanforderung mehr als ein Wert angegeben ist, handelt es sich bei diesen Werten um eine mehrwertige Sammlung als Wert des **xms_cc**-Anspruchs.

Eine Anforderung von :

```json
{ "access_token": { "xms_cc":{"values":["cp1","foo", "bar"] } }}
```

führt zu einem Anspruch von

```json
"xms_cc": ["cp1", "foo", "bar"]
```

im Zugriffstoken, wenn **cp1**, **foo** und **bar** bekannte Funktionen sind.

So sieht das Manifest der App aus, nachdem der **xms_cc** [optionale Anspruch](active-directory-optional-claims.md) angefordert wurde

```c#
"optionalClaims":
{
    "accessToken": [
    {
        "additionalProperties": [],
        "essential": false,
        "name": "xms_cc",
        "source": null
    }],
    "idToken": [],
    "saml2Token": []
}
```

Die API kann dann ihre Antworten basierend darauf anpassen, ob der Client Anspruchsaufforderungen verarbeiten kann oder nicht.

Beispiel in C#

```c#
Claim ccClaim = context.User.FindAll(clientCapabilitiesClaim).FirstOrDefault(x => x.Type == "xms_cc");
if (ccClaim != null && ccClaim.Value == "cp1")
{
    // Return formatted claims challenge as this client understands this
}
else
{
    // Throw generic exception
    throw new UnauthorizedAccessException("The caller does not meet the authentication bar to carry our this operation. The service cannot allow this operation");
}
```

## <a name="next-steps"></a>Nächste Schritte

- [Microsoft Identity Platform und der OAuth 2.0-Autorisierungscodeflow](v2-oauth2-auth-code-flow.md#request-an-authorization-code)
- [Verwenden von CAE-fähigen APIs in Ihren Anwendungen](app-resilience-continuous-access-evaluation.md)
- [Granularer bedingter Zugriff für sensible Daten und Aktionen](https://techcommunity.microsoft.com/t5/azure-active-directory-identity/granular-conditional-access-for-sensitive-data-and-actions/ba-p/1751775)
