---
title: Sicherheitstoken | Azure
titleSuffix: Microsoft identity platform
description: Erfahren Sie mehr über die Grundlagen der Sicherheitstoken in Microsoft Identity Platform (v2.0).
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 04/28/2020
ms.author: ryanwi
ms.reviewer: jmprieur, saeeda, sureshja, hirsin
ms.custom: aaddev, identityplatformtop40, scenarios:getting-started
ms.openlocfilehash: faaf4a9c4fe37bc184b9860390f1eb99eede035c
ms.sourcegitcommit: b9d4b8ace55818fcb8e3aa58d193c03c7f6aa4f1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/29/2020
ms.locfileid: "82584158"
---
# <a name="security-tokens"></a>Sicherheitstoken

Ein zentraler Identitätsanbieter ist besonders nützlich für Apps mit weltweit verteilten Benutzern, die sich nicht immer über das Netzwerk des Unternehmens anmelden. Microsoft Identity Platform authentifiziert Benutzer und stellt Sicherheitstoken bereit (z. B. [Zugriffstoken](developer-glossary.md#access-token), [Aktualisierungstoken](developer-glossary.md#refresh-token) und [ID-Token](developer-glossary.md#id-token)), die es einer [Clientanwendung](developer-glossary.md#client-application) ermöglichen, auf geschützte Ressourcen auf einem [Ressourcenserver](developer-glossary.md#resource-server) zuzugreifen.

Ein **Zugriffstoken** ist ein Sicherheitstoken, das von einem [Autorisierungsserver](developer-glossary.md#authorization-server) im Rahmen eines [OAuth 2.0](active-directory-v2-protocols.md)-Flows ausgestellt wird. Es enthält Informationen zum Benutzer und zu der App, für die das Token bestimmt ist. Diese Informationen können zum Zugreifen auf Web-APIs und andere geschützte Ressourcen verwendet werden. Weitere Informationen darüber, wie Microsoft Identity Platform Zugriffstoken ausstellt, finden Sie unter [Zugriffstoken](access-tokens.md).

Zugriffstoken sind nur für kurze Zeit gültig, sodass Autorisierungsserver manchmal gleichzeitig mit dem Zugriffstoken auch ein **Aktualisierungstoken** ausstellen. Die Clientanwendung kann dann dieses Aktualisierungstoken bei Bedarf gegen ein neues Zugriffstoken austauschen. Weitere Informationen dazu, wie Microsoft Identity Platform Aktualisierungstoken zum Widerrufen von Berechtigungen verwendet, finden Sie unter [Widerrufen von Token](access-tokens.md#token-revocation).

**ID-Token** werden als Teil eines [OpenID Connect](v2-protocols-oidc.md)-Flows an die Clientanwendung gesendet. Sie können zusammen mit einem Zugriffstoken oder anstelle eines Zugriffstokens gesendet werden und werden vom Client zur Authentifizierung des Benutzers verwendet. Weitere Informationen darüber, wie Microsoft Identity Platform ID-Token ausstellt, finden Sie unter [ID-Token](id-tokens.md).

## <a name="validating-security-tokens"></a>Überprüfen von Sicherheitstoken

Die Überprüfung des Tokens wird auf eine der folgenden Arten durchgeführt: von der App, für die das Token generiert wurde, der Web-App, mit der der Benutzer angemeldet wurde, oder der Web-API, die für die Überprüfung des Tokens aufgerufen wird. Das Token wird vom **Sicherheitstokenserver (STS)** mit einem privaten Schlüssel signiert. Der STS veröffentlicht den entsprechenden öffentlichen Schlüssel. Zur Überprüfung eines Tokens verifiziert die App die Signatur, indem mit dem öffentlichen STS-Schlüssel überprüft wird, ob die Signatur mit dem privaten Schlüssel erstellt wurde.

Token sind nur für einen begrenzten Zeitraum gültig. Normalerweise stellt der STS ein Tokenpaar bereit:

* Ein Zugriffstoken für den Zugriff auf die Anwendung oder die geschützte Ressource und
* Ein Aktualisierungstoken zum Aktualisieren des Zugriffstokens, wenn dieses demnächst abläuft.

Zugriffstoken werden als Bearertoken im `Authorization`-Header an eine Web-API übergeben. Eine App kann ein Aktualisierungstoken an den STS übergeben, und wenn der Benutzerzugriff auf die App nicht widerrufen wurde, erhält sie ein neues Zugriffstoken und ein neues Aktualisierungstoken zurück. So wird ein Fall behandelt, bei dem eine Person aus dem Unternehmen ausscheidet. Wenn der STS ein Aktualisierungstoken empfängt, stellt er kein weiteres gültiges Zugriffstoken aus, falls der Benutzer nicht mehr autorisiert ist.

## <a name="json-web-tokens-jwts-and-claims"></a>JSON Web Token (JWTs) und Ansprüche

Microsoft Identity Platform implementiert Sicherheitstoken als **JSON Web Token (JWTs)** , die **Ansprüche** enthalten.

Ein [Anspruch](developer-glossary.md#claim) stellt Assertionen zu einer Entität (z. B. Clientanwendung oder [Ressourcenbesitzer](developer-glossary.md#resource-owner)) für eine andere Entität (z. B. Ressourcenserver) bereit.

Ansprüche sind Name-Wert-Paare zur Weitergabe von Informationen zum Tokenantragsteller. Ein Anspruch kann beispielsweise Informationen zum Sicherheitsprinzipal enthalten, der vom Autorisierungsserver authentifiziert wurde. Welche Ansprüche in einem Token enthalten sind, hängt von verschiedenen Dingen ab. Hierzu zählen unter anderem die Art des Tokens, die Art der Anmeldeinformationen für die Authentifizierung des Antragstellers und die Anwendungskonfiguration.

Anwendungen können Ansprüche für verschiedene Aufgaben verwenden, z. B.:

* Überprüfen des Tokens
* Identifizieren des [Mandanten](developer-glossary.md#tenant) des Tokenantragstellers
* Anzeigen von Benutzerinformationen
* Ermitteln der Autorisierung des Antragstellers

Ein Anspruch besteht aus Schlüssel-Wert-Paaren, mit denen beispielsweise die folgenden Informationen bereitgestellt werden:

* Sicherheitstokenserver, der das Token generiert hat
* Datum, an dem das Token generiert wurde
* Antragsteller (z. B. der Benutzer – mit Ausnahme von Daemons)
* Zielgruppe, d. h. App, für die das Token generiert wurde
* App (der Client), die das Token angefordert hat. Bei Web-Apps kann dies mit der Zielgruppe identisch sein

Weitere Informationen darüber, wie Microsoft Identity Platform Token und Anspruchsinformationen implementiert, finden Sie unter [Zugriffstoken](access-tokens.md) und [ID-Token](id-tokens.md).

## <a name="how-each-flow-emits-tokens-and-codes"></a>Ausgabe von Token und Codes für die einzelnen Abläufe

Je nach Art Ihres Clients kann einer (oder auch mehrere) der Authentifizierungsflows verwendet werden, die von Microsoft Identity Platform unterstützt werden. Mit diesen Abläufen können verschiedene Token (ID-Token, Aktualisierungstoken, Zugriffstoken) und Autorisierungscodes erstellt werden, und es sind unterschiedliche Token erforderlich. Dieses Diagramm enthält eine Übersicht:

|Flow | Erforderlich | ID-Token | Zugriffstoken | Aktualisierungstoken | Autorisierungscode |
|-----|----------|----------|--------------|---------------|--------------------|
|[Autorisierungscodeflow](v2-oauth2-auth-code-flow.md) | | x | x | x | x|
|[Impliziter Flow](v2-oauth2-implicit-grant-flow.md) | | x        | x    |      |                    |
|[Hybrid-OIDC-Ablauf](v2-protocols-oidc.md#get-access-tokens)| | x  | |          |            x   |
|[Einlösung des Aktualisierungstokens](v2-oauth2-auth-code-flow.md#refresh-the-access-token) | Aktualisierungstoken | x | x | x| |
|[„Im Auftrag von“-Ablauf](v2-oauth2-on-behalf-of-flow.md) | Zugriffstoken| x| x| x| |
|[Clientanmeldeinformationen](v2-oauth2-client-creds-grant-flow.md) | | | x (nur App)| | |

Für Token, die im impliziten Modus ausgestellt werden, gilt eine Längenbeschränkung, weil sie per URL zurück an den Browser übergeben werden (`response_mode` ist hierbei `query` oder `fragment`).  Für einige Browser gilt eine Größenbeschränkung für die URL, die in die Browserleiste eingefügt werden kann. Es tritt ein Fehler auf, wenn die URL zu lang ist.  Diese Token verfügen daher nicht über Ansprüche der Art `groups` oder `wids`.

## <a name="next-steps"></a>Nächste Schritte

Weitere Themen zu den Grundlagen von Authentifizierung und Autorisierung:

* Unter [Authentifizierung im Vergleich zu Autorisierung](authentication-vs-authorization.md) lernen Sie die grundlegenden Konzepte für Authentifizierung und Autorisierung in Microsoft Identity Platform kennen.
* Unter [Anwendungsmodell](application-model.md) erhalten Sie Informationen zum Registrierungsvorgang für Ihre Anwendung, damit diese in Microsoft Identity Platform integriert werden kann.
* Unter [App-Anmeldeflow](app-sign-in-flow.md) lernen Sie den Anmeldeflow von Web-, Desktop- und mobilen Apps in Microsoft Identity Platform kennen.
