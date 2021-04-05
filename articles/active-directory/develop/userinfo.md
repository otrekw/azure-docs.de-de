---
title: 'Microsoft Identity Platform: UserInfo-Endpunkt | Azure'
titleSuffix: Microsoft identity platform
description: Erfahren Sie etwas über den UserInfo-Endpunkt der Microsoft Identity Platform.
services: active-directory
author: hpsin
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.topic: conceptual
ms.date: 09/21/2020
ms.author: hirsin
ms.reviewer: hirsin
ms.custom: aaddev
ms.openlocfilehash: 3bd0621d607b121f19bc47c717343b8e2e39b04f
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "98752732"
---
# <a name="microsoft-identity-platform-userinfo-endpoint"></a>Microsoft Identity Platform: UserInfo-Endpunkt

Der UserInfo-Endpunkt ist Teil des [OpenID Connect-Standards](https://openid.net/specs/openid-connect-core-1_0.html#UserInfo) (OIDC), der für die Rückgabe von Ansprüchen über den authentifizierenden Benutzer konzipiert ist.  Für die Microsoft Identity Platform wird der UserInfo-Endpunkt bei Microsoft Graph (https://graph.microsoft.com/oidc/userinfo) gehostet. 

## <a name="find-the-well-known-configuration-endpoint"></a>Suchen des bekannten Konfigurationsendpunkts (.well-known)

Sie können den UserInfo-Endpunkt mithilfe des OpenID Connect-Discovery-Dokuments unter `https://login.microsoftonline.com/common/v2.0/.well-known/openid-configuration` programmgesteuert ermitteln. Er ist im Feld `userinfo_endpoint` aufgeführt. Sie können dieses Muster cloudübergreifend verwenden, um auf den richtigen Endpunkt zu verweisen.  Es wird nicht empfohlen, den UserInfo-Endpunkt in Ihrer App hart zu codieren. Verwenden Sie stattdessen das OIDC-Discovery-Dokument, um diesen Endpunkt zur Laufzeit zu suchen.

Als Teil der OpenID Connect-Spezifikation wird der UserInfo-Endpunkt häufig von [OIDC-konformen Bibliotheken](https://openid.net/developers/certified/) automatisch aufgerufen, um Informationen zum Benutzer zu erhalten.  Ohne das Hosting eines solchen Endpunkts wäre Microsoft Identity Platform nicht standardkonform, und bei einigen Bibliotheken würden Fehler auftreten.  Aus der [Liste der im OIDC-Standard identifizierten Ansprüche](https://openid.net/specs/openid-connect-core-1_0.html#StandardClaims) erstellen wir die Namensansprüche, den Antragstelleranspruch und die E-Mail-Adresse, sofern diese Angaben verfügbar sind und die Einwilligung für sie erteilt wurde.  

## <a name="consider-use-an-id-token-instead"></a>Berücksichtigen Sie dabei Folgendes: Alternative Verwendung eines ID-Tokens

Die im ID-Token verfügbaren Informationen, die Ihre App empfangen kann, sind eine übergeordnete Menge der Informationen, die sie vom UserInfo-Endpunkt abrufen kann.  Da Sie ein ID-Token zur gleichen Zeit wie ein Token zum Aufrufen des UserInfo-Endpunkts abrufen können, empfiehlt es sich, Informationen zum Benutzer mithilfe dieses ID-Tokens abzurufen, anstatt den UserInfo-Endpunkt aufzurufen.  Durch die Verwendung des ID-Tokens entfallen ein bis zwei Netzwerkanforderungen vom Anwendungsstart, sodass die Latenz der Anwendung verringert wird.

Wenn Sie weitere Informationen zum Benutzer benötigen, sollten Sie die [Microsoft Graph-API `/user`](/graph/api/user-get) aufrufen, um Informationen wie Büronummer oder Position abzurufen.   Sie können auch [optionale Ansprüche](active-directory-optional-claims.md) verwenden, um zusätzliche Benutzerinformationen in Ihre ID und Zugriffstoken einzubeziehen.

## <a name="calling-the-userinfo-endpoint"></a>Aufrufen des UserInfo-Endpunkts

UserInfo ist eine Standard-OAuth-Bearertoken-API, die wie jede andere Microsoft Graph-API mithilfe des beim Abrufen eines Tokens für Microsoft Graph erhaltenen Zugriffstokens aufgerufen wird. Sie gibt eine JSON-Antwort zurück, die Ansprüche über den Benutzer enthält.

### <a name="permissions"></a>Berechtigungen

Verwenden Sie die folgenden [OIDC-Berechtigungen](v2-permissions-and-consent.md#openid-connect-scopes), um die UserInfo-API aufzurufen. `openid` ist erforderlich, und die Bereiche `profile` und `email` stellen sicher, dass zusätzliche Informationen in der Antwort bereitgestellt werden.

|Berechtigungstyp      | Berechtigungen    |
|:--------------------|:---------------------------------------------------------|
|Delegiert (Geschäfts-, Schul- oder Unikonto) | OpenID (erforderlich), Profil, E-Mail-Adresse |
|Delegiert (persönliches Microsoft-Konto) | OpenID (erforderlich), Profil, E-Mail-Adresse |
|Application | Nicht verfügbar |

> [!TIP]
> Kopieren Sie diese URL in Ihren Browser, um ein Token für den UserInfo-Endpunkt und ein [ID-Token](id-tokens.md) abzurufen, und ersetzen Sie dabei die Client-ID und den Umleitungs-URI durch Ihre eigenen Werte. Beachten Sie, dass nur Bereiche für OpenID oder Graph angefordert werden und nichts anderes.  Dies ist erforderlich, da Sie Berechtigungen nicht für zwei verschiedene Ressourcen in derselben Tokenanforderung anfordern können.
>
> `https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id=<yourClientID>&response_type=token+id_token&redirect_uri=<YourRedirectUri>&scope=user.read+openid+profile+email&response_mode=fragment&state=12345&nonce=678910`
>
> Sie können dieses Zugriffstoken im nächsten Abschnitt verwenden.

Wie bei anderen Microsoft Graph-Token ist das hier erhaltene Token möglicherweise kein JWT. Wenn Sie einen Microsoft-Kontobenutzer angemeldet haben, wird ein verschlüsseltes Tokenformat verwendet. Dies liegt daran, dass Microsoft Graph ein spezielles Tokenausstellungsmuster aufweist. Die Möglichkeit, das Zugriffstoken zum Aufrufen des UserInfo-Endpunkts zu verwenden, wird dadurch nicht beeinträchtigt.

### <a name="calling-the-api"></a>Aufrufen der API

Die UserInfo-API unterstützt sowohl GET als auch POST gemäß der OIDC-Spezifikation.

```http
GET or POST /oidc/userinfo HTTP/1.1
Host: graph.microsoft.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJub25jZSI6Il…
```

### <a name="userinfo-response"></a>UserInfo-Antwort

```jsonc
{
    "sub": "OLu859SGc2Sr9ZsqbkG-QbeLgJlb41KcdiPoLYNpSFA",
    "name": "Mikah Ollenburg", // names all require the “profile” scope.
    "family_name": " Ollenburg",
    "given_name": "Mikah",
    "email": "mikoll@contoso.com" //requires the “email” scope.
}
```

Bei den hier aufgeführten Ansprüchen handelt es sich um alle Ansprüche, die vom UserInfo-Endpunkt zurückgegeben werden können.  Dies sind dieselben Werte, die in den [ID-Token](id-tokens.md) an die App ausgegeben würden.  

## <a name="notes-and-caveats-on-the-userinfo-endpoint"></a>Hinweise und Einschränkungen zum UserInfo-Endpunkt

* Wenn Sie diesen UserInfo-Endpunkt aufrufen möchten, müssen Sie den v2.0-Endpunkt verwenden.  Wenn Sie den v1.0-Endpunkt verwenden, erhalten Sie ein Token für den v1.0-UserInfo-Endpunkt, der unter login.microsoftonline.com gehostet wird.  Es wird für alle OIDC-konformen Apps und Bibliotheken empfohlen, den v2.0-Endpunkt zu verwenden, um Kompatibilität zu gewährleisten.
* Die Antwort vom UserInfo-Endpunkt kann nicht angepasst werden.  Wenn Sie Ansprüche anpassen möchten, bearbeiten Sie die in den Token zurückgegebenen Informationen mithilfe der [Anspruchszuordnung]( active-directory-claims-mapping.md).
* Der Antwort vom UserInfo-Endpunkt kann nichts hinzugefügt werden.  Wenn Sie zusätzliche Ansprüche über den Benutzer abrufen möchten, fügen Sie den Token neue [optionale Ansprüche]( active-directory-optional-claims.md) hinzu.

## <a name="next-steps"></a>Nächste Schritte

* [Überprüfen des Inhalts von ID-Token](id-tokens.md)
* [Anpassen des Inhalts eines ID-Tokens mithilfe von optionalen Ansprüchen](active-directory-optional-claims.md)
* [Anfordern eines Zugriffstokens und eines ID-Tokens mit dem OAuth2-Protokoll](v2-protocols-oidc.md)
