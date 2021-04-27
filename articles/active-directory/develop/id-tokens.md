---
title: Microsoft Identity Platform – ID-Token | Azure
titleSuffix: Microsoft identity platform
description: Hier erfahren Sie mehr über die Verwendung von ID-Token, die von den Azure AD v1.0- und Microsoft Identity Platform (v2.0)-Endpunkten ausgegeben werden.
services: active-directory
author: hpsin
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.topic: conceptual
ms.date: 04/02/2021
ms.author: hirsin
ms.reviewer: hirsin
ms.custom: aaddev, identityplatformtop40
ms:custom: fasttrack-edit
ms.openlocfilehash: cf7940af03c02d96f6ed2ca452322ad012bc6a2d
ms.sourcegitcommit: 5f482220a6d994c33c7920f4e4d67d2a450f7f08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/08/2021
ms.locfileid: "107105356"
---
# <a name="microsoft-identity-platform-id-tokens"></a>Microsoft Identity Platform – ID-Token

`id_tokens` werden als Teil eines [OpenID Connect](v2-protocols-oidc.md) (OIDC)-Flows an die Clientanwendung gesendet. Sie können zusammen mit einem Zugriffstoken oder anstelle dessen gesendet werden und werden dann vom Client zur Authentifizierung des Benutzers verwendet.

## <a name="using-the-id_token"></a>Verwenden des ID-Tokens

ID-Token sollten verwendet werden, um zu überprüfen, ob ein Benutzer der ist, der er vorgibt zu sein, und um zusätzliche nützliche Informationen über ihn zu erhalten. Sie sollten nicht für die Autorisierung anstelle eines [Zugriffstokens](access-tokens.md) verwendet werden. Die bereitgestellten Ansprüche können für Elemente der Benutzeroberfläche in Ihrer Anwendung, als [Schlüssel in einer Datenbank](#using-claims-to-reliably-identify-a-user-subject-and-object-id) und zum Bereitstellen des Zugriffs auf die Clientanwendung verwendet werden.  

## <a name="claims-in-an-id_token"></a>Ansprüche in einem ID-Token

`id_tokens` sind [JSON Web Token](https://tools.ietf.org/html/rfc7519) (JWT), d. h., sie bestehen aus Header, Nutzlast und Signatur. Sie können den Header und die Signatur verwenden, um die Authentizität des Tokens zu überprüfen, während die Nutzlast die Informationen zu dem von Ihrem Client angeforderten Benutzer enthält. Sofern nicht anders angegeben, sind alle hier aufgeführten JWT-Ansprüche sowohl in v1.0- als auch in v2.0-Token enthalten.

### <a name="v10"></a>v1.0

```
eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6IjdfWnVmMXR2a3dMeFlhSFMzcTZsVWpVWUlHdyIsImtpZCI6IjdfWnVmMXR2a3dMeFlhSFMzcTZsVWpVWUlHdyJ9.eyJhdWQiOiJiMTRhNzUwNS05NmU5LTQ5MjctOTFlOC0wNjAxZDBmYzljYWEiLCJpc3MiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC9mYTE1ZDY5Mi1lOWM3LTQ0NjAtYTc0My0yOWYyOTU2ZmQ0MjkvIiwiaWF0IjoxNTM2Mjc1MTI0LCJuYmYiOjE1MzYyNzUxMjQsImV4cCI6MTUzNjI3OTAyNCwiYWlvIjoiQVhRQWkvOElBQUFBcXhzdUIrUjREMnJGUXFPRVRPNFlkWGJMRDlrWjh4ZlhhZGVBTTBRMk5rTlQ1aXpmZzN1d2JXU1hodVNTajZVVDVoeTJENldxQXBCNWpLQTZaZ1o5ay9TVTI3dVY5Y2V0WGZMT3RwTnR0Z2s1RGNCdGsrTExzdHovSmcrZ1lSbXY5YlVVNFhscGhUYzZDODZKbWoxRkN3PT0iLCJhbXIiOlsicnNhIl0sImVtYWlsIjoiYWJlbGlAbWljcm9zb2Z0LmNvbSIsImZhbWlseV9uYW1lIjoiTGluY29sbiIsImdpdmVuX25hbWUiOiJBYmUiLCJpZHAiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC83MmY5ODhiZi04NmYxLTQxYWYtOTFhYi0yZDdjZDAxMWRiNDcvIiwiaXBhZGRyIjoiMTMxLjEwNy4yMjIuMjIiLCJuYW1lIjoiYWJlbGkiLCJub25jZSI6IjEyMzUyMyIsIm9pZCI6IjA1ODMzYjZiLWFhMWQtNDJkNC05ZWMwLTFiMmJiOTE5NDQzOCIsInJoIjoiSSIsInN1YiI6IjVfSjlyU3NzOC1qdnRfSWN1NnVlUk5MOHhYYjhMRjRGc2dfS29vQzJSSlEiLCJ0aWQiOiJmYTE1ZDY5Mi1lOWM3LTQ0NjAtYTc0My0yOWYyOTU2ZmQ0MjkiLCJ1bmlxdWVfbmFtZSI6IkFiZUxpQG1pY3Jvc29mdC5jb20iLCJ1dGkiOiJMeGVfNDZHcVRrT3BHU2ZUbG40RUFBIiwidmVyIjoiMS4wIn0=.UJQrCA6qn2bXq57qzGX_-D3HcPHqBMOKDPx4su1yKRLNErVD8xkxJLNLVRdASHqEcpyDctbdHccu6DPpkq5f0ibcaQFhejQNcABidJCTz0Bb2AbdUCTqAzdt9pdgQvMBnVH1xk3SCM6d4BbT4BkLLj10ZLasX7vRknaSjE_C5DI7Fg4WrZPwOhII1dB0HEZ_qpNaYXEiy-o94UJ94zCr07GgrqMsfYQqFR7kn-mn68AjvLcgwSfZvyR_yIK75S_K37vC3QryQ7cNoafDe9upql_6pB2ybMVlgWPs_DmbJ8g0om-sPlwyn74Cc1tW3ze-Xptw_2uVdPgWyqfuWAfq6Q
```

Zeigen Sie dieses v1. 0-Beispieltoken in [jwt.ms](https://jwt.ms/#id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6IjdfWnVmMXR2a3dMeFlhSFMzcTZsVWpVWUlHdyIsImtpZCI6IjdfWnVmMXR2a3dMeFlhSFMzcTZsVWpVWUlHdyJ9.eyJhdWQiOiJiMTRhNzUwNS05NmU5LTQ5MjctOTFlOC0wNjAxZDBmYzljYWEiLCJpc3MiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC9mYTE1ZDY5Mi1lOWM3LTQ0NjAtYTc0My0yOWYyOTU2ZmQ0MjkvIiwiaWF0IjoxNTM2Mjc1MTI0LCJuYmYiOjE1MzYyNzUxMjQsImV4cCI6MTUzNjI3OTAyNCwiYWlvIjoiQVhRQWkvOElBQUFBcXhzdUIrUjREMnJGUXFPRVRPNFlkWGJMRDlrWjh4ZlhhZGVBTTBRMk5rTlQ1aXpmZzN1d2JXU1hodVNTajZVVDVoeTJENldxQXBCNWpLQTZaZ1o5ay9TVTI3dVY5Y2V0WGZMT3RwTnR0Z2s1RGNCdGsrTExzdHovSmcrZ1lSbXY5YlVVNFhscGhUYzZDODZKbWoxRkN3PT0iLCJhbXIiOlsicnNhIl0sImVtYWlsIjoiYWJlbGlAbWljcm9zb2Z0LmNvbSIsImZhbWlseV9uYW1lIjoiTGluY29sbiIsImdpdmVuX25hbWUiOiJBYmUiLCJpZHAiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC83MmY5ODhiZi04NmYxLTQxYWYtOTFhYi0yZDdjZDAxMWRiNDcvIiwiaXBhZGRyIjoiMTMxLjEwNy4yMjIuMjIiLCJuYW1lIjoiYWJlbGkiLCJub25jZSI6IjEyMzUyMyIsIm9pZCI6IjA1ODMzYjZiLWFhMWQtNDJkNC05ZWMwLTFiMmJiOTE5NDQzOCIsInJoIjoiSSIsInN1YiI6IjVfSjlyU3NzOC1qdnRfSWN1NnVlUk5MOHhYYjhMRjRGc2dfS29vQzJSSlEiLCJ0aWQiOiJmYTE1ZDY5Mi1lOWM3LTQ0NjAtYTc0My0yOWYyOTU2ZmQ0MjkiLCJ1bmlxdWVfbmFtZSI6IkFiZUxpQG1pY3Jvc29mdC5jb20iLCJ1dGkiOiJMeGVfNDZHcVRrT3BHU2ZUbG40RUFBIiwidmVyIjoiMS4wIn0=.UJQrCA6qn2bXq57qzGX_-D3HcPHqBMOKDPx4su1yKRLNErVD8xkxJLNLVRdASHqEcpyDctbdHccu6DPpkq5f0ibcaQFhejQNcABidJCTz0Bb2AbdUCTqAzdt9pdgQvMBnVH1xk3SCM6d4BbT4BkLLj10ZLasX7vRknaSjE_C5DI7Fg4WrZPwOhII1dB0HEZ_qpNaYXEiy-o94UJ94zCr07GgrqMsfYQqFR7kn-mn68AjvLcgwSfZvyR_yIK75S_K37vC3QryQ7cNoafDe9upql_6pB2ybMVlgWPs_DmbJ8g0om-sPlwyn74Cc1tW3ze-Xptw_2uVdPgWyqfuWAfq6Q) an.

### <a name="v20"></a>v2.0

```
eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6IjFMVE16YWtpaGlSbGFfOHoyQkVKVlhlV01xbyJ9.eyJ2ZXIiOiIyLjAiLCJpc3MiOiJodHRwczovL2xvZ2luLm1pY3Jvc29mdG9ubGluZS5jb20vOTEyMjA0MGQtNmM2Ny00YzViLWIxMTItMzZhMzA0YjY2ZGFkL3YyLjAiLCJzdWIiOiJBQUFBQUFBQUFBQUFBQUFBQUFBQUFJa3pxRlZyU2FTYUZIeTc4MmJidGFRIiwiYXVkIjoiNmNiMDQwMTgtYTNmNS00NmE3LWI5OTUtOTQwYzc4ZjVhZWYzIiwiZXhwIjoxNTM2MzYxNDExLCJpYXQiOjE1MzYyNzQ3MTEsIm5iZiI6MTUzNjI3NDcxMSwibmFtZSI6IkFiZSBMaW5jb2xuIiwicHJlZmVycmVkX3VzZXJuYW1lIjoiQWJlTGlAbWljcm9zb2Z0LmNvbSIsIm9pZCI6IjAwMDAwMDAwLTAwMDAtMDAwMC02NmYzLTMzMzJlY2E3ZWE4MSIsInRpZCI6IjkxMjIwNDBkLTZjNjctNGM1Yi1iMTEyLTM2YTMwNGI2NmRhZCIsIm5vbmNlIjoiMTIzNTIzIiwiYWlvIjoiRGYyVVZYTDFpeCFsTUNXTVNPSkJjRmF0emNHZnZGR2hqS3Y4cTVnMHg3MzJkUjVNQjVCaXN2R1FPN1lXQnlqZDhpUURMcSFlR2JJRGFreXA1bW5PcmNkcUhlWVNubHRlcFFtUnA2QUlaOGpZIn0.1AFWW-Ck5nROwSlltm7GzZvDwUkqvhSQpm55TQsmVo9Y59cLhRXpvB8n-55HCr9Z6G_31_UbeUkoz612I2j_Sm9FFShSDDjoaLQr54CreGIJvjtmS3EkK9a7SJBbcpL1MpUtlfygow39tFjY7EVNW9plWUvRrTgVk7lYLprvfzw-CIqw3gHC-T7IK_m_xkr08INERBtaecwhTeN4chPC4W3jdmw_lIxzC48YoQ0dB1L9-ImX98Egypfrlbm0IBL5spFzL6JDZIRRJOu8vecJvj1mq-IUhGt0MacxX8jdxYLP-KUu2d9MbNKpCKJuZ7p8gwTL5B7NlUdh_dmSviPWrw
```

Zeigen Sie dieses v2. 0-Beispieltoken in [jwt.ms](https://jwt.ms/#id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6IjFMVE16YWtpaGlSbGFfOHoyQkVKVlhlV01xbyJ9.eyJ2ZXIiOiIyLjAiLCJpc3MiOiJodHRwczovL2xvZ2luLm1pY3Jvc29mdG9ubGluZS5jb20vOTEyMjA0MGQtNmM2Ny00YzViLWIxMTItMzZhMzA0YjY2ZGFkL3YyLjAiLCJzdWIiOiJBQUFBQUFBQUFBQUFBQUFBQUFBQUFJa3pxRlZyU2FTYUZIeTc4MmJidGFRIiwiYXVkIjoiNmNiMDQwMTgtYTNmNS00NmE3LWI5OTUtOTQwYzc4ZjVhZWYzIiwiZXhwIjoxNTM2MzYxNDExLCJpYXQiOjE1MzYyNzQ3MTEsIm5iZiI6MTUzNjI3NDcxMSwibmFtZSI6IkFiZSBMaW5jb2xuIiwicHJlZmVycmVkX3VzZXJuYW1lIjoiQWJlTGlAbWljcm9zb2Z0LmNvbSIsIm9pZCI6IjAwMDAwMDAwLTAwMDAtMDAwMC02NmYzLTMzMzJlY2E3ZWE4MSIsInRpZCI6IjkxMjIwNDBkLTZjNjctNGM1Yi1iMTEyLTM2YTMwNGI2NmRhZCIsIm5vbmNlIjoiMTIzNTIzIiwiYWlvIjoiRGYyVVZYTDFpeCFsTUNXTVNPSkJjRmF0emNHZnZGR2hqS3Y4cTVnMHg3MzJkUjVNQjVCaXN2R1FPN1lXQnlqZDhpUURMcSFlR2JJRGFreXA1bW5PcmNkcUhlWVNubHRlcFFtUnA2QUlaOGpZIn0.1AFWW-Ck5nROwSlltm7GzZvDwUkqvhSQpm55TQsmVo9Y59cLhRXpvB8n-55HCr9Z6G_31_UbeUkoz612I2j_Sm9FFShSDDjoaLQr54CreGIJvjtmS3EkK9a7SJBbcpL1MpUtlfygow39tFjY7EVNW9plWUvRrTgVk7lYLprvfzw-CIqw3gHC-T7IK_m_xkr08INERBtaecwhTeN4chPC4W3jdmw_lIxzC48YoQ0dB1L9-ImX98Egypfrlbm0IBL5spFzL6JDZIRRJOu8vecJvj1mq-IUhGt0MacxX8jdxYLP-KUu2d9MbNKpCKJuZ7p8gwTL5B7NlUdh_dmSviPWrw) an.

### <a name="header-claims"></a>Headeransprüche

|Anspruch | Format | BESCHREIBUNG |
|-----|--------|-------------|
|`typ` | Zeichenfolge – immer „JWT“ | Gibt an, dass das Token ein JWT-Token ist.|
|`alg` | String | Gibt den Algorithmus an, mit dem das Token signiert wurde. Beispiel: "RS256" |
|`kid` | String | Der Fingerabdruck des öffentlichen Schlüssels, der zum Überprüfen dieses Tokens verwendet wird. Wird in v1. 0- und v2. 0-`id_tokens` ausgegeben. |
|`x5t` | String | Identisch (in Verwendung und Wert) mit `kid`. Dies ist jedoch ein Legacyanspruch, der aus Kompatibilitätsgründen nur in v1.0-`id_tokens` ausgegeben wird. |

### <a name="payload-claims"></a>Nutzlastansprüche

Diese Liste zeigt die JWT-Ansprüche, die (sofern nicht anders angegeben) standardmäßig in den meisten ID-Token enthalten sind.  Allerdings kann Ihre App mithilfe von [optionalen Ansprüchen](active-directory-optional-claims.md) zusätzliche JWT-Ansprüche im ID-Token anfordern.  Diese können vom `groups`-Anspruch bis zu Informationen zum Namen des Benutzers reichen.

|Anspruch | Format | BESCHREIBUNG |
|-----|--------|-------------|
|`aud` |  Zeichenfolge, ein App-ID-URI | Identifiziert den vorgesehenen Empfänger des Tokens. In `id_tokens` ist die Zielgruppe die Anwendungs-ID Ihrer App, die Ihrer App im Azure-Portal zugewiesen ist. Ihre App sollte diesen Wert überprüfen und das Token ablehnen, wenn der Wert nicht übereinstimmt. |
|`iss` |  Zeichenfolge, ein STS-URI | Identifiziert den Sicherheitstokendienst (STS), der das Token und den Azure AD-Mandanten, in dem der Benutzer authentifiziert wurde, erstellt und zurückgibt. Wenn das Token vom v2.0-Endpunkt ausgegeben wurde, endet der URI mit `/v2.0`.  Die GUID, die angibt, dass der Benutzer ein Consumer-Benutzer eines Microsoft-Kontos ist, lautet `9188040d-6c67-4c5b-b112-36a304b66dad`. Ihre App sollte ggf. den GUID-Teil des Anspruchs verwenden, um die Mandanten einzuschränken, die sich bei der App anmelden können. |
|`iat` |  Ganze Zahl, ein UNIX-Zeitstempel | „Issued At“ gibt an, wann die Authentifizierung für dieses Token erfolgt ist.  |
|`idp`|Zeichenfolge, in der Regel ein STS-URI | Der Identitätsanbieter, der den Antragsteller des Tokens authentifiziert hat. Dieser Wert ist identisch mit dem Wert des Ausstelleranspruchs, es sei denn, das Benutzerkonto ist nicht im gleichen Mandanten wie der Aussteller vorhanden (etwa Gäste). Wenn der Anspruch nicht vorhanden ist, bedeutet dies, dass stattdessen der Wert `iss` verwendet werden kann.  Für in einem Organisationskontext verwendete persönliche Konten (etwa ein zu einem Azure AD-Mandanten eingeladenes persönliches Konto) kann der `idp`-Anspruch „live.com“ oder ein STS-URI sein, der den Microsoft-Kontomandanten `9188040d-6c67-4c5b-b112-36a304b66dad` enthält. |
|`nbf` |  Ganze Zahl, ein UNIX-Zeitstempel | Der Anspruch „nbf“ (nicht vor) gibt die Zeit an, vor der das JWT NICHT für die Bearbeitung akzeptiert werden darf.|
|`exp` |  Ganze Zahl, ein UNIX-Zeitstempel | Der Anspruch „exp“ (Ablaufzeit) gibt die Ablaufzeit an, ab oder nach der das JWT NICHT für die Bearbeitung akzeptiert werden darf.  Es ist wichtig zu beachten, dass eine Ressource das Token auch vor diesem Zeitpunkt ablehnen kann (wenn beispielsweise eine Änderung der Authentifizierung erforderlich ist oder ein Tokenwiderruf erkannt wurde). |
| `c_hash`| String |Der Codehash ist nur dann in ID-Token enthalten, wenn das ID-Token zusammen mit einem OAuth 2.0-Autorisierungscode ausgestellt wird. Mit seiner Hilfe kann die Authentizität eines Autorisierungscodes überprüft werden. Weitere Informationen zum Ausführen dieser Überprüfung finden Sie in der [OpenID Connect-Spezifikation](https://openid.net/specs/openid-connect-core-1_0.html). |
|`at_hash`| String |Der Zugriffstokenhash ist nur in ID-Token enthalten, wenn das ID-Token vom `/authorize`-Endpunkt zusammen mit einem OAuth 2.0-Zugriffstoken ausgestellt wird. Mit seiner Hilfe kann die Authentizität eines Zugriffstokens überprüft werden. Weitere Informationen zum Ausführen dieser Überprüfung finden Sie in der [OpenID Connect-Spezifikation](https://openid.net/specs/openid-connect-core-1_0.html#HybridIDToken). Dies wird für ID-Token vom `/token`-Endpunkt nicht zurückgegeben. |
|`aio` | Nicht transparente Zeichenfolge | Ein interner Anspruch, der von Azure AD verwendet wird, um Daten für die Wiederverwendung von Token aufzuzeichnen. Sollte ignoriert werden.|
|`preferred_username` | String | Der primäre Benutzername, der den Benutzer darstellt. Dabei kann es sich um eine E-Mail-Adresse, eine Telefonnummer oder einen generischen Benutzernamen ohne bestimmtes Format handeln. Der Wert kann geändert werden und sich im Laufe der Zeit ändern. Da er geändert werden kann, darf dieser Wert nicht verwendet werden, um Autorisierungsentscheidungen zu treffen. Der Bereich `profile` ist erforderlich, um diesen Anspruch zu empfangen.|
|`email` | String | Der Anspruch `email` ist standardmäßig für Gastkonten vorhanden, die über eine E-Mail-Adresse verfügen.  Ihre App kann den E-Mail-Anspruch für verwaltete Benutzer (unter demselben Mandanten wie die Ressource) über den `email` [optionalen Anspruch](active-directory-optional-claims.md) anfordern.  Auf dem v2.0-Endpunkt kann Ihre App auch den OpenID Connect-Bereich `email` anfordern. Sie müssen nicht sowohl den optionalen Anspruch als auch den Bereich abrufen, um den Anspruch zu erhalten.  Für den Anspruch „email“ werden nur adressierbare E-Mails aus den Profilinformationen des Benutzers unterstützt. |
|`name` | String | Der `name`-Anspruch gibt einen visuell lesbaren Wert an, der den Antragsteller des Tokens identifiziert. Der Wert ist nicht zwingend eindeutig, kann geändert werden und dient nur zu Anzeigezwecken. Der Bereich `profile` ist erforderlich, um diesen Anspruch zu empfangen. |
|`nonce`| String | Die Nonce stimmt mit dem Parameter überein, der in der ursprünglichen /authorize-Anforderung an den IDP enthalten ist. Wenn diese Angaben nicht übereinstimmen, sollte Ihre Anwendung das Token ablehnen. |
|`oid` | Zeichenfolge, eine GUID | Der unveränderliche Bezeichner für ein Objekt im Microsoft-Identitätssystem, in diesem Fall ein Benutzerkonto. Diese ID identifiziert den Benutzer anwendungsübergreifend eindeutig: Zwei verschiedene Anwendungen, die den gleichen Benutzer anmelden, erhalten den gleichen Wert im `oid`-Anspruch. Microsoft Graph gibt diese ID als `id`-Eigenschaft für ein bestimmtes Benutzerkonto zurück. Da mit `oid` mehrere Apps Benutzer korrelieren können, ist der `profile`-Bereich erforderlich, um diesen Anspruch zu erhalten. Beachten Sie Folgendes: Wenn ein einzelner Benutzer in mehreren Mandanten vorhanden ist, enthält der Benutzer in jedem Mandanten eine andere Objekt-ID. Sie werden als unterschiedliche Konten betrachtet, obwohl sich der Benutzer bei jedem Konto mit den gleichen Anmeldeinformationen anmeldet. Der `oid`-Anspruch ist eine GUID und kann nicht wiederverwendet werden. |
|`roles`| Array von Zeichenfolgen | Die Rollen, die dem sich anmeldenden Benutzer zugewiesen wurden. |
|`rh` | Nicht transparente Zeichenfolge |Ein interner Anspruch, der von Azure zum erneuten Überprüfen von Token verwendet wird. Sollte ignoriert werden. |
|`sub` | String | Der Prinzipal, für den das Token Informationen zusichert, z. B. der Benutzer einer App. Dieser Wert ist unveränderlich und kann nicht erneut zugewiesen oder wiederverwendet werden. Der Antragsteller ist ein paarweiser Bezeichner: Er gilt nur für eine bestimmte Anwendungs-ID. Wenn sich ein Benutzer bei zwei verschiedenen Apps mit zwei verschiedenen Client-IDs anmeldet, erhalten diese Apps zwei unterschiedliche Werte für den Antragstelleranspruch. Dies kann – abhängig von den Architektur- und Datenschutzanforderungen – wünschenswert sein oder nicht. |
|`tid` | Zeichenfolge, eine GUID | Eine GUID, die den Azure AD-Mandanten darstellt, aus dem der Benutzer stammt. Bei Geschäfts- und Schulkonten ist die GUID die unveränderliche Mandanten-ID der Organisation, zu der der Benutzer gehört. Für persönliche Konten lautet der Wert `9188040d-6c67-4c5b-b112-36a304b66dad`. Der Bereich `profile` ist erforderlich, um diesen Anspruch zu empfangen. |
|`unique_name` | String | Ein lesbarer Wert, der Aufschluss über den Antragsteller des Tokens gibt. Dieser Wert ist immer eindeutig. Da aber E-Mails und andere Bezeichner wiederverwendet werden können, kann dieser Wert in anderen Konten erneut auftreten und sollte daher nur zu Anzeigezwecken verwendet werden. Wird nur in v1.0-`id_tokens` ausgegeben. |
|`uti` | Nicht transparente Zeichenfolge | Ein interner Anspruch, der von Azure zum erneuten Überprüfen von Token verwendet wird. Sollte ignoriert werden. |
|`ver` | Zeichenfolge, 1.0 oder 2.0 | Gibt die Version des ID-Tokens an. |
|`hasgroups`|Boolean|Ist immer auf TRUE festgelegt (sofern vorhanden) und gibt an, dass der Benutzer mindestens einer Gruppe angehört. Wird anstelle des Gruppenanspruchs für JWTs in Flows mit impliziter Gewährung verwendet, wenn der Anspruch für vollständige Gruppen das URI-Fragment über die URL-Längenbeschränkung (derzeit 6 oder mehr Gruppen) erweitert. Gibt an, dass der Client die Gruppen des Benutzers über die Microsoft Graph-API bestimmen soll (`https://graph.microsoft.com/v1.0/users/{userID}/getMemberObjects`).|
|`groups:src1`|JSON-Objekt | Für Tokenanforderungen ohne Längenbeschränkung (siehe `hasgroups` oben), die aber dennoch zu groß für das Token sind, ist ein Link zur Liste der vollständigen Gruppen für den Benutzer enthalten. Für JWTs als verteilter Anspruch, für SAML als neuer Anspruch anstelle des Anspruchs `groups`. <br><br>**JWT-Beispielwert**: <br> `"groups":"src1"` <br> `"_claim_sources`: `"src1" : { "endpoint" : "https://graph.microsoft.com/v1.0/users/{userID}/getMemberObjects" }`<br><br> Weitere Informationen finden Sie unter [Gruppenüberschreitungsanspruch](#groups-overage-claim).|

> [!NOTE]
> Die ID-Token der Versionen 1.0 und 2.0 weisen Unterschiede in der Menge der enthaltenen Informationen auf, wie aus den obigen Beispielen hervorgeht. Die Version basiert auf dem Endpunkt, von dem aus die Anforderung erfolgt ist. Obwohl vorhandene Anwendungen wahrscheinlich den Azure AD-Endpunkt verwenden, sollten neue Anwendungen Microsoft Identity Platform verwenden.
>
> - v1.0: Azure AD-Endpunkte: `https://login.microsoftonline.com/common/oauth2/authorize`
> - v2.0: Microsoft Identity Platform-Endpunkte: `https://login.microsoftonline.com/common/oauth2/v2.0/authorize`

### <a name="using-claims-to-reliably-identify-a-user-subject-and-object-id"></a>Verwenden von Ansprüchen zur zuverlässigen Identifizierung von Benutzern (Betreff und Objekt-ID)

Wenn Sie einen Benutzer identifizieren (wenn Sie ihn z. B. in einer Datenbank suchen oder entscheiden, welche Berechtigungen er erhält), ist es wichtig, Informationen zu verwenden, die im Lauf der Zeit unverändert und eindeutig bleiben. Ältere Anwendungen verwenden manchmal Felder wie die E-Mail-Adresse, eine Telefonnummer oder den UPN.  Alle diese Informationen können sich im Lauf der Zeit ändern und auch wiederverwendet werden (z. B. wenn ein Mitarbeiter seinen Namen ändert oder wenn ein Mitarbeiter die E-Mail-Adresse eines früheren Mitarbeiters erhält, der nicht mehr im Unternehmen arbeitet). Daher ist es **enorm wichtig**, dass Ihre Anwendung keine für Menschen lesbaren Daten verwendet, um Benutzer zu identifizieren. Für Menschen lesbar bedeutet ganz allgemein, dass jemand diese Informationen lesen und ändern könnte. Verwenden Sie stattdessen die Ansprüche, die vom OIDC-Standard bereitgestellt werden, oder die von Microsoft bereitgestellten Erweiterungsansprüche `sub` und `oid`.

Verwenden Sie zum ordnungsgemäßen Speichern von Informationen pro Benutzer nur `sub` oder `oid` (die als GUIDs eindeutig sind) und bei Bedarf `tid` für das Routing oder Sharding.  Wenn Sie Daten dienstübergreifend freigeben müssen, eignet sich `oid`+`tid` am besten, da alle Apps dieselben Ansprüche `oid` und `tid` für einen bestimmten Benutzer erhalten.  Der `sub`-Anspruch in der Microsoft Identity-Plattform wird „paarweise“ festgelegt, d. h., er basiert auf einer Kombination aus dem Tokenempfänger, dem Mandanten und dem Benutzer und ist damit eindeutig.  Daher erhalten zwei Apps, die ID-Token für einen bestimmten Benutzer anfordern, unterschiedliche `sub`-Ansprüche, aber dieselben `oid`-Ansprüche für diesen Benutzer.

>[!NOTE]
> Verwenden Sie den `idp`-Anspruch nicht zum Speichern von Informationen zu einem Benutzer, um Benutzer über Mandanten hinweg zu korrelieren.  Dies funktioniert nicht, da die Ansprüche `oid` und `sub` für einen Benutzer in verschiedenen Mandanten unterschiedlich sind. Dies ist so konzipiert, um sicherzustellen, dass Anwendungen Benutzer nicht mandantenübergreifend nachverfolgen können.  
>
> Gastszenarien, in denen ein Benutzer einem Mandanten angehört und sich in einem anderen authentifiziert, sollten den Benutzer wie einen neuen Benutzer des Diensts behandeln.  Ihre Dokumente und Berechtigungen im Mandanten Contoso dürfen nicht im Mandanten Fabrikam angewandt werden. Dies ist wichtig, um versehentliche Datenlecks zwischen Mandanten zu vermeiden.

### <a name="groups-overage-claim"></a>Gruppenüberschreitungsanspruch
Um sicherzustellen, dass die Tokengröße die Grenzwerte für HTTP-Header nicht überschreitet, schränkt Azure AD die Anzahl der im `groups`-Anspruch enthaltenen Objekt-IDs ein. Wenn ein Benutzer Mitglied mehrerer Gruppen als die zulässige Überschreitungsgrenze (150 für SAML-Token, 200 für JWT-Token) ist, gibt Azure AD den Gruppenanspruch nicht im Token aus. Stattdessen ist ein Überschreitungsanspruch im Token enthalten, der der Anwendung anzeigt, dass die Microsoft Graph-API abgefragt werden soll, um die Gruppenmitgliedschaft des Benutzers abzurufen.

```json
{
  ...
  "_claim_names": {
   "groups": "src1"
    },
    {
  "_claim_sources": {
    "src1": {
        "endpoint":"[Url to get this user's group membership from]"
        }
       }
     }
  ...
}
```

## <a name="id-token-lifetime"></a>ID-Tokengültigkeitsdauer

Standardmäßig ist ein ID-Token eine Stunde lang gültig. Nach einer Stunde muss der Client ein neues ID-Token beziehen.

Sie können die Gültigkeitsdauer eines ID-Tokens anpassen, um zu steuern, wie oft die Clientanwendung den Ablauf der Anwendungssitzung veranlasst und wie oft eine erneute Authentifizierung des Benutzers erforderlich ist (entweder im Hintergrund oder interaktiv). Weitere Informationen hierzu finden Sie unter [Konfigurierbare Tokengültigkeitsdauer](active-directory-configurable-token-lifetimes.md).

## <a name="validating-an-id_token"></a>Überprüfen eines ID-Tokens

Die Überprüfung eines `id_token` ist dem ersten Schritt der [Überprüfung eines Zugriffstokens](access-tokens.md#validating-tokens) ähnlich. Ihr Client kann bestätigen, dass der richtige Aussteller das Token zurückgesendet hat und dass es nicht manipuliert wurde. Da `id_tokens` immer JWT-Token sind, sind zahlreiche Bibliotheken zum Überprüfen dieser Token verfügbar. Wir empfehlen, eine dieser Bibliotheken zu verwenden, anstatt den Vorgang selbst auszuführen.  Beachten Sie, dass nur vertrauliche Clients (mit einem Geheimnis) ID-Token überprüfen sollten.  Öffentliche Anwendungen (Code, der vollständig auf einem Gerät oder in einem Netzwerk ausgeführt wird, das Sie nicht steuern können, z. B. dem Browser eines Benutzers oder dessen Heimnetzwerk) profitieren nicht vom Validieren des ID-Tokens, da ein böswilliger Benutzer die zur Validierung des Tokens verwendeten Schlüssel abfangen und bearbeiten kann.

Um das Token manuell zu überprüfen, beachten Sie die Details der Schritte unter [Überprüfen eines Zugriffstokens](access-tokens.md#validating-tokens). Nach der Überprüfung der Signatur des Tokens sollten die folgenden JWT-Ansprüche im ID-Token überprüft werden (dies kann auch von Ihrer Tokenüberprüfungsbibliothek ausgeführt werden):

* Zeitstempel: Die Zeitstempel `iat`, `nbf` und `exp` sollten alle vor oder nach der aktuellen Zeit liegen (je nach Bedarf).
* Zielgruppe: Der `aud`-Anspruch sollte mit der App-ID Ihrer Anwendung übereinstimmen.
* Nonce: Der `nonce`-Anspruch in der Nutzlast muss mit dem nonce-Parameter übereinstimmen, der während der ersten Anforderung an den /authorize-Endpunkt übergeben wurde.

## <a name="next-steps"></a>Nächste Schritte

* Informationen zu [Zugriffstoken](access-tokens.md)
* Passen Sie die JWT-Ansprüche in Ihrem ID-Token mithilfe von [optionalen Ansprüchen](active-directory-optional-claims.md) an.
