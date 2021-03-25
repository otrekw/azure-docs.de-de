---
title: OAuth 2.0- und OpenID Connect-Protokolle in Microsoft Identity Platform | Azure
titleSuffix: Microsoft identity platform
description: Ein Leitfaden für die OAuth 2.0- und OpenID Connect-Protokolle, die von Microsoft Identity Platform unterstützt werden.
services: active-directory
author: hpsin
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.topic: conceptual
ms.date: 07/21/2020
ms.author: hirsin
ms.reviewer: hirsin
ms.custom: aaddev
ms.openlocfilehash: 765c363542b07deac44d47b94731e1109fcba045
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "98755756"
---
# <a name="oauth-20-and-openid-connect-protocols-on-the-microsoft-identity-platform"></a>OAuth 2.0 und OpenID Connect-Protokolle in Microsoft Identity Platform

Der Microsoft Identity Platform-Endpunkt für Identity-as-a-Service implementiert die Authentifizierung und Autorisierung mit den Industriestandardprotokollen OpenID Connect (OIDC) bzw. OAuth 2.0. Auch wenn der Dienst den Standard entspricht, kann es feine Unterschiede zwischen zwei Implementierungen dieser Protokolle geben. Die hier bereitgestellten Informationen sind hilfreich, wenn Sie Code direkt durch Senden und Verarbeiten von HTTP-Anforderungen schreiben oder eine Open-Source-Bibliothek eines Drittanbieters verwenden, statt eine unserer [Open-Source-Bibliotheken](reference-v2-libraries.md) zu nutzen.

## <a name="the-basics"></a>Grundlagen

In fast allen OAuth 2.0- und OpenID Connect-Vorgängen sind vier Beteiligte am Austausch involviert:

![Diagramm mit OAuth 2.0-Rollen](./media/active-directory-v2-flows/protocols-roles.svg)

* Microsoft Identity Platform ist der **Autorisierungsserver** und verantwortlich für das Sicherstellen der Identität des Benutzers, das Erteilen und Widerrufen des Zugriffs auf Ressourcen und das Ausstellen von Token. Der Autorisierungsserver ist auch als Identitätsanbieter bekannt und verarbeitet auf sichere Weise alles im Zusammenhang mit den Informationen des Benutzers, dessen Zugriff und den Vertrauensstellungen zwischen den Beteiligten in einem Vorgang.
* Beim **Ressourcenbesitzer** handelt es sich normalerweise um den Endbenutzer. Diese Partei besitzt die Daten und hat die Möglichkeit, Clients den Zugriff auf die Daten oder die Ressource zu gewähren.
* Der **OAuth-Client** ist Ihre App, die durch ihre Anwendungs-ID identifiziert wird. Der OAuth-Client ist normalerweise der Beteiligte, mit dem der Endbenutzer interagiert, und der Token vom Autorisierungsserver anfordert. Der Client muss vom Besitzer der Ressource die Berechtigung zum Zugriff darauf erhalten.
* Der **Ressourcenserver** ist der Ort, an dem die Ressource oder die Daten abgelegt sind. Er vertraut dem Autorisierungsserver, dass der OAuth-Client sicher authentifiziert und autorisiert wird, und verwendet Bearerzugriffstoken, um sicherzustellen, dass der Zugriff auf eine Ressource gewährt werden kann.

## <a name="app-registration"></a>App-Registrierung

Jede App, die sowohl persönliche als auch Geschäfts-, Schul- oder Unikonten akzeptieren soll, muss über die Funktion **App-Registrierungen** des [Azure-Portals](https://aka.ms/appregistrations) registriert werden, bevor diese Benutzer mit OAuth 2.0 oder OpenID Connect angemeldet werden können. Der Registrierungsprozess für die App sammelt einige Werte und weist ihr einige Werte zu:

* Eine **Anwendungs-ID**, die Ihre App eindeutig identifiziert
* Ein **Umleitungs-URI** (optional), der zum Umleiten von Antworten zurück an die App verwendet werden kann.
* Einige andere szenariospezifische Werte.

Weitere Informationen finden Sie im Artikel zum [Registrieren von Apps](quickstart-register-app.md).

## <a name="endpoints"></a>Endpunkte

Nach der Registrierung kommuniziert die App mit Microsoft Identity Platform, indem sie Anforderungen an den Endpunkt sendet:

```
https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize
https://login.microsoftonline.com/{tenant}/oauth2/v2.0/token
```

Dabei ist für `{tenant}` einer von vier verschiedenen Werten möglich:

| Wert | BESCHREIBUNG |
| --- | --- |
| `common` | Ermöglicht Benutzern mit persönlichen Microsoft-Konten und Geschäfts-, Schul- oder Unikonten aus Azure AD die Anmeldung bei der Anwendung. |
| `organizations` | Ermöglicht nur Benutzern mit Geschäfts-, Schul- oder Unikonten aus Azure AD die Anmeldung bei der Anwendung. |
| `consumers` | Ermöglicht nur Benutzern mit persönlichen Microsoft-Konten (MSA) die Anmeldung bei der Anwendung. |
| `8eaef023-2b34-4da1-9baa-8bc8c9d6a490` oder `contoso.onmicrosoft.com` | Ermöglicht nur Benutzern mit Geschäfts-, Schul- oder Unikonten eines bestimmten Azure AD-Mandanten die Anmeldung bei der Anwendung. Dabei kann entweder der Anzeigename der Domäne des Azure AD-Mandanten oder der GUID-Bezeichner des Mandanten verwendet werden. |

Um zu erfahren, wie Sie mit diesen Endpunkten interagieren, wählen Sie im Abschnitt [Protokolle](#protocols) einen bestimmten App-Typ aus, und folgen Sie den Links für weitere Informationen.

> [!TIP]
> Jede in Azure AD registrierte App kann Microsoft Identity Platform verwenden, selbst wenn keine persönlichen Konten angemeldet werden.  Auf diese Weise können Sie vorhandene Anwendungen zu Microsoft Identity Platform und [MSAL](reference-v2-libraries.md) migrieren, ohne Ihre Anwendung neu erstellen zu müssen.

## <a name="tokens"></a>Token

OAuth 2.0 und OpenID Connect machen umfassende Verwendung von **Bearertoken**, die in der Regel als [JSON Web Token (JWT)](https://tools.ietf.org/html/rfc7519) bereitgestellt werden. Ein Bearertoken ist ein einfaches Sicherheitstoken, das dem „Bearer“ (Träger) den Zugriff auf eine geschützte Ressource ermöglicht. In diesem Sinn ist der „Bearer“ jeder, der eine Kopie des Tokens erhält. Um das Bearertoken zu erhalten, muss sich die Partei zwar zunächst bei Microsoft Identity Platform authentifizieren, falls jedoch keine Maßnahmen ergriffen werden, um das Token bei der Übertragung und Speicherung zu schützen, kann das Token von einer fremden Partei abgefangen und verwendet werden. Einige Sicherheitstoken verfügen über einen integrierten Mechanismus, der eine unbefugte Verwendung durch nicht autorisierte Parteien verhindert. Trägertoken besitzen dagegen keinen solchen Mechanismus und müssen über einen sicheren Kanal wie etwa Transport Layer Security (HTTPS) übertragen werden. Wird ein Bearertoken als Klartext gesendet, kann eine böswillige Partei das Token mithilfe eines Man-in-the-Middle-Angriffs abfangen und damit unautorisiert auf eine geschützte Ressource zugreifen. Die gleichen Sicherheitsprinzipien gelten für die (Zwischen-)Speicherung von Trägertoken zur späteren Verwendung. Stellen Sie daher sicher, dass Ihre App Bearertoken stets auf sichere Weise überträgt und speichert. Weitere Sicherheitsüberlegungen zu Bearertoken finden Sie unter [RFC 6750, Abschnitt 5](https://tools.ietf.org/html/rfc6750).

Bei OAuth 2.0 und OIDC werden in erster Linie drei Typen von Token verwendet:

* [Zugriffstoken](access-tokens.md) sind Token, die ein Ressourcenserver von einem Client empfängt und die die Berechtigungen enthalten, die dem Client erteilt wurden.  
* [ID-Token](id-tokens.md) sind Token, die von einem Client vom Autorisierungsserver empfangen und zum Anmelden eines Benutzers sowie zum Abrufen grundlegender Informationen zu diesem verwendet werden.
* Aktualisierungstoken werden von einem Client verwendet, um im Lauf der Zeit neue Zugriffs- und ID-Token abzurufen.  Dabei handelt es sich um nicht transparente Zeichenfolgen, die nur vom Autorisierungsserver verstanden werden können.

## <a name="protocols"></a>Protokolle

Wenn Sie einige Beispielanforderungen sehen möchten, beginnen Sie mit einem der folgenden Protokolldokumente. Jedes Lernprogramm entspricht einem bestimmten Szenario. Wenn Sie Hilfe bei der Bestimmung des richtigen Flows benötigen, informieren Sie sich über die [App-Typen, die mit Microsoft Identity Platform erstellt werden können](v2-app-types.md).

* [Erstellen mobiler, nativer und Webanwendungen mit OAuth 2.0](v2-oauth2-auth-code-flow.md)
* [Anmelden von Benutzern mit OpenID Connect](v2-protocols-oidc.md)
* [Erstellen von Daemons oder serverseitigen Prozessen mit dem OAuth 2.0-Clientanmeldeinformations-Datenfluss](v2-oauth2-client-creds-grant-flow.md)
* [Abrufen von Token in einer Web-API mit dem „Im Namen von“-Datenfluss von OAuth 2.0](v2-oauth2-on-behalf-of-flow.md)
* [Erstellen von Single-Page-Webanwendungen mit dem impliziten OAuth 2.0-Flow](v2-oauth2-implicit-grant-flow.md)
