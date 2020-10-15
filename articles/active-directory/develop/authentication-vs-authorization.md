---
title: Authentifizierung im Vergleich zu Autorisierung | Azure
titleSuffix: Microsoft identity platform
description: Lernen Sie die Grundlagen von Authentifizierung und Autorisierung in Microsoft Identity Platform (v2.0) kennen.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 05/22/2020
ms.author: ryanwi
ms.reviewer: jmprieur, saeeda, sureshja, hirsin
ms.custom: aaddev, identityplatformtop40, scenarios:getting-started
ms.openlocfilehash: 095bc5594cf9b6eaf0df7929ea83c25a401793e2
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "86252112"
---
# <a name="authentication-vs-authorization"></a>Authentifizierung im Vergleich zu Autorisierung

In diesem Artikel werden Authentifizierung und Autorisierung definiert und kurz erläutert, wie Sie Microsoft Identity Platform zum Authentifizieren und Autorisieren von Benutzern in Ihren Web-Apps, Web-APIs oder Apps, die geschützte Web-APIs aufrufen, verwenden können. Wenn Sie auf einen Begriff stoßen, mit dem Sie nicht vertraut sind, werfen Sie einen Blick in unser [Glossar](developer-glossary.md), oder sehen Sie sich unsere [Videos zu Microsoft Identity Platform](identity-videos.md) an, in denen grundlegende Konzepte behandelt werden.

## <a name="authentication"></a>Authentifizierung

Bei der **Authentifizierung** weisen Sie nach, dass Sie die Person sind, als die Sie sich ausgeben. Authentifizierung wird manchmal verkürzt als AuthN bezeichnet. Microsoft Identity Platform implementiert das [OpenID Connect](https://openid.net/connect/)-Protokoll für die Verarbeitung der Authentifizierung.

## <a name="authorization"></a>Authorization

Die **Autorisierung** ist die Gewährung einer Handlungsberechtigung für eine authentifizierte Person. Sie gibt an, auf welche Daten Sie zugreifen dürfen und welche Aktionen Sie damit ausführen können. Autorisierung wird manchmal verkürzt als AuthZ bezeichnet. In Microsoft Identity Platform ist das [OAuth 2.0](https://oauth.net/2/)-Autorisierungsprotokoll für die Verarbeitung der Autorisierung implementiert.

## <a name="authentication-and-authorization-using-microsoft-identity-platform"></a>Authentifizierung und Autorisierung mit der Microsoft Identity Platform

Statt Apps zu erstellen, die jeweils über eigene Informationen in Bezug auf Benutzername und Kennwort verfügen, was mit einem hohen Verwaltungsaufwand verbunden ist, wenn Sie für mehrere Apps Benutzer hinzufügen oder entfernen müssen, besteht die Möglichkeit, dass Apps diese Aufgabe an einen zentralen Identitätsanbieter delegieren können.

Azure Active Directory (Azure AD) ist ein zentraler Identitätsanbieter in der Cloud. Die Delegierung der Authentifizierung und Autorisierung an den Anbieter ermöglicht beispielsweise die Verwendung von Richtlinien für bedingten Zugriff, die erfordern, dass sich ein Benutzer an einem bestimmten Ort befindet, die Verwendung der [mehrstufigen Authentifizierung](../authentication/concept-mfa-howitworks.md) (manchmal auch als zweistufige Authentifizierung oder 2FA bezeichnet) sowie die Schaffung der Möglichkeit, dass sich Benutzer nur einmal anmelden müssen und dann automatisch bei allen Web-Apps angemeldet werden, die dasselbe zentralisierte Verzeichnis nutzen. Diese Funktion wird als **einmaliges Anmelden** (Single Sign-On, SSO) bezeichnet.

Microsoft Identity Platform vereinfacht die Autorisierung und Authentifizierung für Anwendungsentwickler durch die Bereitstellung von Identity-as-a-Service mit Unterstützung für branchenübliche Protokolle wie OAuth 2.0 und OpenID Connect sowie Open-Source-Bibliotheken für verschiedene Plattformen, damit Sie schnell mit der Programmierung beginnen können. Sie ermöglicht Entwicklern nicht nur das Erstellen von Anwendungen, mit denen alle Microsoft-Identitäten angemeldet werden, sondern auch das Abrufen von Token zum Aufrufen von [Microsoft Graph](https://developer.microsoft.com/graph/), anderen Microsoft-APIs oder von Entwicklern erstellten APIs.

Im Folgenden finden Sie einen kurzen Vergleich der verschiedenen Protokolle, die von der Microsoft Identity Platform verwendet werden:

* **OAuth im Vergleich zu OpenID Connect**: OAuth wird für die Autorisierung und OpenID Connect (OIDC) für die Authentifizierung verwendet. OpenID Connect baut auf OAuth 2.0 auf, sodass beide eine ähnliche Terminologie und einen ähnlichen Flow aufweisen. Sie können sogar in einer Anforderung einen Benutzer (mit OpenID Connect) authentifizieren und (mit OAuth 2.0) Autorisierung für den Zugriff auf eine geschützte Ressource erhalten, die der Benutzer besitzt. Weitere Informationen finden Sie unter [OAuth 2.0- und OpenID Connect-Protokolle](active-directory-v2-protocols.md) und [OpenID Connect-Protokoll](v2-protocols-oidc.md).
* **OAuth im Vergleich zu SAML**: OAuth wird für die Autorisierung und SAML für die Authentifizierung verwendet. Unter [Microsoft Identity Platform und OAuth 2.0-SAML-Bearerassertionsflow](v2-saml-bearer-assertion.md) finden Sie weitere Informationen dazu, wie die beiden Protokolle zusammen verwendet werden können, um einen Benutzer (mit SAML) zu authentifizieren und (mit OAuth 2.0) Autorisierung für den Zugriff auf eine geschützte Ressource zu erhalten.
* **OpenID Connect im Vergleich zu SAML**: Sowohl OpenID Connect als auch SAML werden zum Authentifizieren eines Benutzers und zum Aktivieren des einmaligen Anmeldens (Single Sign-On, SSO) verwendet. Die SAML-Authentifizierung wird häufig bei Identitätsanbietern wie Active Directory-Verbunddienste (AD FS) im Verbund mit Azure AD und daher häufig in Unternehmensanwendungen verwendet. OpenID Connect wird häufig für Apps verwendet, die sich ausschließlich in der Cloud befinden, wie z. B. mobile Apps, Websites und Web-APIs.

## <a name="next-steps"></a>Nächste Schritte

Weitere Themen zu den Grundlagen von Authentifizierung und Autorisierung:

* Unter [Sicherheitstoken](security-tokens.md) erfahren Sie, wie Zugriffstoken, Aktualisierungstoken und ID-Token bei der Autorisierung und Authentifizierung verwendet werden.
* Unter [Anwendungsmodell](application-model.md) erhalten Sie Informationen zum Registrierungsvorgang für Ihre Anwendung, damit diese in Microsoft Identity Platform integriert werden kann.
* Unter [App-Anmeldefluss](app-sign-in-flow.md) lernen Sie den Anmeldeflow von Web-, Desktop- und mobilen Apps in Microsoft Identity Platform kennen.

* Weitere Informationen zu den von der Microsoft Identity Platform implementierten Protokollen finden Sie unter [OAuth 2.0- und OpenID Connect-Protokolle auf der Microsoft Identity Platform](active-directory-v2-protocols.md).
* Unter [SAML-Protokoll für einmaliges Anmelden](single-sign-on-saml-protocol.md) erfahren Sie, wie Microsoft Identity Platform das einmalige Anmelden unterstützt.
* Unter [Einmaliges Anmelden bei Anwendungen in Azure Active Directory](../manage-apps/what-is-single-sign-on.md) finden Sie weitere Informationen zu den verschiedenen Möglichkeiten, wie Sie einmaliges Anmelden in Ihre App implementieren können.
